---
title: Configuración manual de dispositivos unidos a Azure Active Directory híbrido | Microsoft Docs
description: Aprenda a configurar manualmente dispositivos unidos a Azure Active Directory híbrido.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3944031101b954c380b5db6503002dae57ea16d
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518480"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Tutorial: Configuración manual de dispositivos unidos a Azure Active Directory híbrido 

Con la administración de dispositivos en Azure Active Directory (Azure AD), puede asegurarse de que los usuarios tienen acceso a los recursos desde dispositivos que cumplen los estándares de seguridad y cumplimiento. Para obtener más información, vea [Introducción a la administración de dispositivos en Azure Active Directory](overview.md).


> [!TIP]
> Si utiliza Azure AD Connect es una opción, consulte los tutoriales relacionados de los dominios [administrados](hybrid-azuread-join-managed-domains.md) o [federados](hybrid-azuread-join-federated-domains.md). Mediante Azure AD Connect, puede simplificar considerablemente la configuración de la combinación de Azure AD híbrido.

Si tiene un entorno local de Active Directory y quiere unir sus dispositivos unidos a un dominio a Azure AD, puede hacerlo configurando dispositivos híbridos unidos a Azure AD. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar manualmente Unión a Azure AD híbrido
> * Configurar un punto de conexión de servicio
> * Configurar la emisión de notificaciones
> * Habilitación de dispositivos de Windows de nivel inferior
> * Comprobación dispositivos unidos
> * Solución de problemas de la implementación
 




## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que está familiarizado con:
    
-  [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)    
-  [Planeación de la implementación de Unión a Azure Active Directory híbrido](hybrid-azuread-join-plan.md)
-  [Control de la unión a Azure AD híbrido de los dispositivos](hybrid-azuread-join-control.md)


Antes de empezar a habilitar dispositivos unidos a Azure AD híbrido en su organización, asegúrese de lo siguiente:

- Ejecuta una versión actualizada de Azure AD Connect.
- Azure AD Connect ha sincronizado con Azure AD los objetos de equipo de los dispositivos que desea que sean dispositivos unidos a Azure AD híbrido. Si los objetos de equipo pertenecen a unidades organizativas (OU) específicas, estas deben configurarse también para la sincronización en Azure AD Connect.

  

Azure AD Connect:

- Mantiene la asociación entre la cuenta del equipo en la instancia local de Active Directory y el objeto de dispositivo en Azure AD. 
- Habilita otras características relacionadas del dispositivo como Windows Hello para empresas.

Asegúrese de que las direcciones URL siguientes son accesibles desde equipos dentro de la red de su organización para el registro de equipos en Azure AD:

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- El STS de su organización (para dominios federados), el cual se debe incluir en la configuración de la intranet local del usuario.

Si su organización tiene previsto utilizar el inicio de sesión único de conexión directa, la siguiente dirección URL debe ser accesible desde los equipos de la organización. También se debe agregar a la zona de la intranet local del usuario.

- https://autologon.microsoftazuread-sso.com

Además, se debe habilitar la siguiente configuración en la zona de la intranet del usuario: "Permitir actualizaciones de barra de estado a través de scripts".

Si su organización usa una configuración administrada (no federada) con Active Directory local y no utiliza Active Directory Domain Services (AD FS) para federar con Azure AD, Unión a Azure AD híbrido de Windows 10 usará los objetos de equipo de Active Directory para sincronizarse con Azure AD. Asegúrese de que todas las unidades organizativas que contengan los objetos de equipo que deban unirse a Azure AD híbrido estén habilitadas para sincronizarse en la configuración de sincronización de Azure AD Connect.

En el caso de la versión 1703 o anterior de los dispositivos Windows 10, si su organización requiere acceso a Internet a través de un proxy de salida, debe implementar la Detección automática de proxy web (WPAD) para permitir que los equipos Windows 10 se registren en Azure AD. 

A partir de la versión 1803 de Windows 10, aunque se produzca un error en el intento de unión por parte de un dispositivo a Azure AD híbrido en un dominio federado mediante AD FS y aunque Azure AD Connect esté configurado para sincronizar los objetos de equipo o dispositivo con Azure AD, el dispositivo intentará completar la unión a Azure AD híbrido mediante el equipo o dispositivo sincronizado.

## <a name="verify-configuration-steps"></a>Comprobación de los pasos de configuración

Puede configurar dispositivos híbridos unidos a un dominio de Active AD para varios tipos de plataformas de dispositivos Windows. En este tema se incluye los pasos requeridos para todos los escenarios de configuración típicos.  

Utilice la tabla siguiente para obtener una visión general de los pasos necesarios para su escenario:  



| Pasos                                      | Dispositivos actuales de Windows y sincronización de hash de contraseña | Dispositivos actuales de Windows y federación | Dispositivos de Windows de nivel inferior |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Configuración del punto de conexión de servicio | ![Comprobar][1]                            | ![Comprobar][1]                    | ![Comprobar][1]        |
| Configurar la emisión de notificaciones           |                                        | ![Comprobar][1]                    | ![Comprobar][1]        |
| Habilitación de dispositivos que no tengan Windows 10      |                                        |                                | ![Comprobar][1]        |
| Comprobación dispositivos unidos          | ![Comprobar][1]                            | ![Comprobar][1]                    | ![Comprobar][1]        |



## <a name="configure-a-service-connection-point"></a>Configurar un punto de conexión de servicio

El objeto de punto de conexión de servicio (SCP) lo usan los dispositivos durante el registro para detectar la información del inquilino de Azure AD. En la instancia de Active Directory local, el objeto SCP para los dispositivos unidos a Azure AD híbrido debe existir en la partición del contexto de nomenclatura de la configuración del bosque del equipo. Hay solo un contexto de nomenclatura de configuración por bosque. En una configuración de Active Directory de varios bosques, el punto de conexión debe existir en todos los bosques que contienen equipos unidos al dominio.

Se puede usar el comando [**Get ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) para recuperar el contexto de nomenclatura de configuración del bosque.  

En el caso de un bosque con el nombre de dominio de Active Directory *fabrikam.com*, el contexto de nomenclatura de configuración es:

`CN=Configuration,DC=fabrikam,DC=com`

En el bosque, el objeto de SCP para el registro automático de los dispositivos unidos a un dominio se encuentra en:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

En función de cómo se haya implementado Azure AD Connect, el objeto SCP puede que ya se haya configurado.
Con el siguiente script de Windows PowerShell se puede comprobar la existencia del objeto y recuperar los valores de detección: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

La salida de **$scp.Keywords** muestra la información del inquilino de Azure AD. Este es un ejemplo:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Si el punto de conexión de servicio no existe, se puede crear mediante la ejecución del cmdlet `Initialize-ADSyncDomainJoinedComputerSync` en un servidor de Azure AD Connect. Se necesitan las credenciales de administrador de organización para ejecutar este cmdlet.  

El cmdlet:

- Crea el punto de conexión de servicio en el bosque de Active Directory al que está conectado Azure AD Connect. 
- Requiere que se especifique el parámetro `AdConnectorAccount`. Esta cuenta está configurada como la cuenta del conector de Active Directory en Azure AD Connect. 


El siguiente script muestra un ejemplo de uso del cmdlet. En este script, `$aadAdminCred = Get-Credential` requiere que escriba un nombre de usuario. Es proceso que use el formato de nombre principal de usuario (UPN) (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

El cmdlet `Initialize-ADSyncDomainJoinedComputerSync`:

- Usa el módulo de PowerShell de Active Directory y las herramientas de Azure Active Directory Domain Services (Azure AD DS). Estas herramientas se basan en los servicios web de Active Directory que se ejecutan en un controlador de dominio. Active Directory Web Services es compatible con los controladores de dominio en los que se ejecuta Windows Server 2008 R2, y las versiones posteriores.
- Solo es compatible con la versión 1.1.166.0 del módulo de MSOnline PowerShell. Para descargar este módulo, use [este vínculo](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- Si no se instalan las herramientas de Azure AD DS, `Initialize-ADSyncDomainJoinedComputerSync` producirá un error. Las herramientas de AD DS se pueden instalar mediante el Administrador del servidor en **Características** > **Herramientas de administración de servidor remoto** > **Herramientas de administración de roles**.

En los casos de los controladores de dominio en los que se ejecuta Windows Server 2008, o alguna versión anterior, use el siguiente script para crear el punto de conexión de servicio. En una configuración con varios bosques, debe usar el script siguiente para crear el punto de conexión de servicio en cada bosque en el que existan equipos.
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your Active Directory configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC
    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()

En el script anterior, `$verifiedDomain = "contoso.com"` es un marcador de posición. Reemplace el marcador de posición por uno de los nombres de dominio comprobados en Azure AD. Para poder utilizar el dominio, deberá ser su propietario.

Para más información sobre nombres de dominios comprobados, consulte [Incorporación de su nombre de dominio personalizado a Azure Active Directory](../active-directory-domains-add-azure-portal.md). 

Para obtener una lista de los dominios comprobados de la compañía, puede usar el cmdlet [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0). 

![Lista de dominios de la compañía](./media/hybrid-azuread-join-manual/01.png)

## <a name="set-up-issuance-of-claims"></a>Configurar la emisión de notificaciones

En una configuración de Azure AD federada, los dispositivos usan AD FS, o un servicio de federación local de un asociado de Microsoft para autenticarse en Azure AD. Los dispositivos se autentican para obtener un token de acceso para registrarse en Azure Active Directory Device Registration Service (Azure DRS).

Los dispositivos de Windows actuales se autentican mediante la autenticación integrada de Windows en un punto de conexión de WS-Trust activo (versiones 1.3 o 2005) hospedado por el servicio de federación local.

> [!NOTE]
> Si se usa AD FS, es preciso habilitar **adfs/services/trust/13/windowstransport** o **adfs/services/trust/2005/windowstransport**. Si usa el proxy de autenticación web, asegúrese también de que este punto de conexión se publique a través del proxy. Para ver qué puntos de conexión están habilitados, vaya a **Servicio** > **Puntos de conexión** en la consola de administración de AD FS.
>
>Si AD FS no es el servicio de federación local, siga las instrucciones de su proveedor para asegurarse de que admite puntos de conexión de WS-Trust 1.3 o 2005 y que estos se publican a través del archivo de intercambio de metadatos (MEX).

Para que se complete el registro del dispositivo, las siguientes notificaciones deben existir en el token que recibe Azure DRS. Azure DRS creará un objeto de dispositivo en Azure AD con parte de esta información. Después, Azure AD Connect utiliza esta información para asociar el objeto de dispositivo recién creado con la cuenta de equipo local.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Si hay más de un nombre de dominio comprobado, es preciso proporcionar la siguiente notificación para los equipos:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Si ya se emite una notificación de ImmutableID (por ejemplo, identificador de inicio de sesión alternativo), será preciso proporcionar una notificación correspondiente para los equipos:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

En las siguientes secciones encontrará información acerca de:
 
- Los valores que deben tener todas las notificaciones.
- El aspecto de una definición en AD FS.

La definición le ayuda a comprobar si los valores están presentes o si debe crearlos.

> [!NOTE]
> Si no usa AD FS como servidor de federación local, siga las instrucciones de su proveedor para crear la configuración apropiada para emitir estas notificaciones.

### <a name="issue-account-type-claim"></a>Emisión de notificación de tipo de cuenta

La notificación `http://schemas.microsoft.com/ws/2012/01/accounttype` debe contener un valor de **DJ**, que identifica el dispositivo como equipo unido a un dominio. En AD FS, puede agregar una regla de transformación de emisión como esta:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Emisión de objectGUID de la cuenta local del equipo

La notificación `http://schemas.microsoft.com/identity/claims/onpremobjectguid` debe contener el valor **objectGUID** de la cuenta local del equipo. En AD FS, puede agregar una regla de transformación de emisión como esta:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Emisión de objectSID de la cuenta local del equipo

La notificación `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid` debe contener el valor **objectSid** de la cuenta de equipo local. En AD FS, puede agregar una regla de transformación de emisión como esta:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-the-computer-when-multiple-verified-domain-names-are-in-azure-ad"></a>Emisión de issuerID para el equipo cuando hay varios nombres de dominio comprobados en Azure AD

La notificación `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid` debe contener el identificador uniforme de recursos (URI) de cualquiera de los nombres de dominio comprobados que se conectan al servicio de federación local (AD FS o uno de asociados) que emite el token. En AD FS, puede agregar reglas de transformación de emisión que se parecen a las que verá a continuación en ese orden específico después de las anteriores. Tenga en cuenta que se necesita una regla que emita explícitamente la regla para los usuarios. En las siguientes reglas se agrega una primera regla que identifica al usuario, en lugar de la autenticación del equipo.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


En la notificación anterior, `<verified-domain-name>` es un marcador de posición. Reemplace el marcador de posición por uno de los nombres de dominio comprobados en Azure AD. Por ejemplo, use `Value = "http://contoso.com/adfs/services/trust/"`.



Para más información sobre nombres de dominios comprobados, consulte [Incorporación de su nombre de dominio personalizado a Azure Active Directory](../active-directory-domains-add-azure-portal.md).  

Para obtener una lista de los dominios comprobados de la compañía, puede usar el cmdlet [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0). 

![Lista de dominios de la compañía](./media/hybrid-azuread-join-manual/01.png)

### <a name="issue-immutableid-for-the-computer-when-one-for-users-exists-for-example-an-alternate-login-id-is-set"></a>Emisión de ImmutableID para el equipo cuando existe uno para los usuarios (por ejemplo, se ha establecido un identificador de inicio de sesión alternativo)

La notificación `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID` debe contener un valor válido para los equipos. En AD FS, se puede crear una regla de transformación de emisión como se indica a continuación:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Script de aplicación auxiliar para crear reglas de transformación de emisión de AD FS

El siguiente script le ayuda con la creación de las reglas de transformación de emisión que se han descrito anteriormente.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

#### <a name="remarks"></a>Comentarios 

- Este script anexa las reglas a las reglas existentes. No ejecute el script dos veces porque el conjunto de reglas se agregaría dos veces. Asegúrese de que no existe ninguna regla correspondiente para estas notificaciones (en las condiciones correspondientes) antes de volver a ejecutar el script.

- Si tiene varios nombres de dominio comprobados (como se muestra en el portal de Azure AD o mediante el cmdlet **Get-MsolDomain**), establezca el valor de **$multipleVerifiedDomainNames** en el script en **$true**. Además, asegúrese de que quita cualquier notificación **issuerid** existente que pueda haber creado Azure AD Connect o a través de otros medios. Este es un ejemplo de esta regla:

      c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Si ya ha emitido una notificación **ImmutableID** para las cuentas de usuario, establezca el valor de **$immutableIDAlreadyIssuedforUsers** en el script en **$true**.

## <a name="enable-windows-down-level-devices"></a>Habilitación de dispositivos de Windows de nivel inferior

Si algunos de los dispositivos unidos a un dominio son dispositivos de Windows de nivel inferior, tendrá que:

- Establecer una directiva en Azure AD que permita a los usuarios registrar dispositivos. 
- Configurar el servicio de federación local para emitir notificaciones para admitir la autenticación integrada de Windows (IWA) para el registro de dispositivos. 
- Agregar el punto de conexión de autenticación de dispositivos de Azure AD a las zonas de la intranet locales para evitar las solicitudes de certificado al autenticar el dispositivo.
- Controlar los dispositivos de Windows de nivel inferior. 


### <a name="set-a-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Establecer una directiva de Azure AD para permitir a los usuarios registrar dispositivos.

Para registrar dispositivos de Windows de nivel inferior, asegúrese de que está habilitada la configuración que permite a los usuarios registrar dispositivos en Azure AD. En Azure Portal, puede encontrar esta opción en **Azure Active Directory** > **Usuarios y grupos** > **Configuración de dispositivo**.
    
La siguiente directiva debe establecerse en **Todo**: **Los usuarios pueden registrar sus dispositivos con Azure AD**.

![El botón Todo que permite a los usuarios registrar dispositivos](./media/hybrid-azuread-join-manual/23.png)


### <a name="configure-the-on-premises-federation-service"></a>Configuración del servicio de federación local 

El servicio de federación local debe admitir la emisión de las notificaciones **authenticationmehod** y **wiaormultiauthn** al recibir una solicitud de autenticación para el usuario de confianza de Azure AD que contiene un parámetro resource_params con el siguiente valor codificado:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Cuando llega una solicitud de este tipo, el servicio de federación local debe autenticar al usuario mediante la autenticación integrada de Windows. Una vez que la autenticación se haya realizado correctamente, el servicio debe emitir las dos notificaciones siguientes:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

En AD FS, debe agregar una regla de transformación de emisión que atraviesa el método de autenticación. Para agregar esta regla:

1. En la consola de administración de AD FS, vaya a **AD FS** > **Relaciones de confianza** > **Relaciones de confianza para usuario autenticado**.
2. Haga clic con el botón derecho en el objeto de confianza para usuario de confianza de la Plataforma de identidad de Microsoft Office 365 y seleccione **Editar reglas de notificación**.
3. En la pestaña **Reglas de transformación de emisión**, seleccione **Agregar regla**.
4. Seleccione **Enviar notificaciones con una regla personalizada** en la lista de plantillas **Regla de notificación**.
5. Seleccione **Next** (Siguiente).
6. En el cuadro de texto **Nombre de regla de notificación**, escriba **Regla de notificación del método de autenticación**.
7. En el cuadro **Regla de notificación**, escriba la siguiente regla:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. En el servidor de federación, escriba el siguiente comando de PowerShell. Reemplace **\<RPObjectName\>** por el nombre del objeto de usuario de confianza para su objeto de confianza de usuario de confianza de Azure AD. Normalmente, este objeto se llama **Plataforma de identidad de Microsoft Office 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-endpoint-to-the-local-intranet-zones"></a>Adición del punto de conexión de autenticación de dispositivos de Azure AD a las zonas de intranet locales

Para evitar las peticiones de certificados cuando los usuarios de dispositivos registrados se autentican en Azure AD se puede insertar una directiva en los dispositivos unidos a un dominio para agregar la siguiente dirección URL a la zona de intranet local en Internet Explorer:

`https://device.login.microsoftonline.com`


### <a name="control-windows-down-level-devices"></a>Control de dispositivos de Windows de nivel inferior 

Para registrar dispositivos de nivel inferior de Windows, debe descargar e instalar un paquete de Windows Installer (.msi) desde el Centro de descarga. Para más información, consulte [Control de la unión de los dispositivos híbridos a Azure AD](hybrid-azuread-join-control.md#control-windows-down-level-devices). 



## <a name="verify-joined-devices"></a>Comprobación dispositivos unidos

Para comprobar los dispositivos unidos correctamente en la organización, use el cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) del [módulo Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

La salida de este cmdlet muestra los dispositivos que se han registrado y unido en Azure AD. Para obtener todos los dispositivos, use el parámetro **-All** y, después, fíltrelos mediante la propiedad **deviceTrustType**. Los dispositivos unidos a un dominio tienen el valor de **Unido a dominio**.



## <a name="troubleshoot-your-implementation"></a>Solución de problemas de la implementación

Si tiene problemas para completar la unión a Azure AD híbrido para los dispositivos de Windows unidos a un dominio, consulte:

- [Solución de problemas de la unión a Azure AD híbrido para dispositivos actuales de Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solución de problemas de la unión a Azure AD híbrido para dispositivos de nivel inferior de Windows](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la administración de dispositivos en Azure Active Directory](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual/12.png

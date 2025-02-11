---
title: Configuración de LDAP seguro (LDAPS) en Azure AD Domain Services | Microsoft Docs
description: Configuración de LDAP seguro (LDAPS) para un dominio administrado con Servicios de dominio de Azure AD
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: ergreenl
ms.openlocfilehash: e961b6c4b97103668c591e319a81d20f533b4acb
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246380"
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services
Este artículo muestra cómo puede habilitar el protocolo ligero de acceso a directorios seguro (LDAPS) para el dominio administrado con Servicios de dominio de Azure AD. LDAP seguro también se denomina "protocolo ligero de acceso a directorios (LDAP) en la Capa de sockets seguros (SSL)/Seguridad de la capa de transporte (TLS)".

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Antes de empezar
Para realizar las tareas enumeradas en este artículo, necesita lo siguiente:

1. Una **suscripción de Azure**válida.
2. Un **directorio de Azure AD** : sincronizado con un directorio local o solo en la nube.
3. **Servicios de dominio de Azure AD** deben estar habilitado en el directorio de Azure AD. Si no lo ha hecho, siga todas las tareas descritas en [Servicios de dominio de Azure AD (vista previa): introducción](create-instance.md).
4. Un **certificado que se usará para habilitar LDAP seguro**.

   * **Recomendado**: obtenga un certificado de una entidad de certificación pública de confianza. Esta opción de configuración es más segura.
   * Si lo prefiere, puede elegir [crear un certificado autofirmado](#task-1---obtain-a-certificate-for-secure-ldap) , tal y como se muestra más adelante en este artículo.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Requisitos para el certificado LDAP seguro
Antes de habilitar LDAP seguro, adquiera un certificado válido de acuerdo con las instrucciones siguientes. Encontrará errores si intenta habilitar LDAP seguro para el dominio administrado con un certificado no válido o incorrecto.

1. **Emisor de confianza**: el certificado debe ser emitido por una autoridad de confianza para los equipos que se conectan al dominio administrado mediante LDAP seguro. Esta entidad puede ser una entidad de certificación (CA) pública o empresarial de confianza para estos equipos.
2. **Duración** : el certificado debe ser válido al menos para los próximos 3-6 meses. El acceso LDAP seguro a su dominio administrado se interrumpe cuando expira el certificado.
3. **Nombre del firmante**: el nombre del firmante del certificado debe ser el dominio administrado. Por ejemplo, si el dominio se denomina "contoso100.com", el nombre del firmante del certificado debe ser "contoso100.com". Establezca el nombre DNS (nombre alternativo del firmante) en un nombre con caracteres comodín para el dominio administrado.
4. **Uso de la clave** : el certificado debe configurarse para los usos siguientes: firmas digitales y cifrado de claves.
5. **Propósito del certificado** : el certificado debe ser válido para la autenticación del servidor SSL.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Tarea 1: Obtener un certificado para LDAP seguro
La primera tarea implica la obtención de un certificado que se utilizará para el acceso LDAP seguro al dominio administrado. Tiene dos opciones:

* Obtener un certificado de una entidad de certificación pública o empresarial.
* Creación de un certificado autofirmado.

> [!NOTE]
> Los equipos cliente que necesitan conectarse al dominio administrado mediante LDAP seguro deben confiar en el emisor del certificado de LDAP seguro.
>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Opción A (recomendada): obtención de un certificado LDAP seguro desde una entidad de certificación
Si su organización obtiene los certificados de una entidad de certificación pública, obtenga de esta el certificado LDAP seguro. Si implementa una CA empresarial, obtenga el certificado LDAP seguro desde la CA empresarial.

> [!TIP]
> **Use certificados autofirmados para dominios administrados con los sufijos de dominio ". onmicrosoft.com".**
> Si el nombre de dominio DNS del dominio administrado termina en ". onmicrosoft.com", no se puede obtener un certificado LDAP seguro desde una entidad de certificación pública. Dado el dominio "onmicrosoft.com" pertenece a Microsoft, las entidades de certificación públicas rechazan la emisión de un certificado LDAP seguro para el usuario para un dominio con este sufijo. En este escenario, cree un certificado autofirmado y úselo para configurar LDAP seguro.
>

Asegúrese de que el certificado que se obtendrá de la entidad de certificación pública cumple todos los requisitos que se describen en [Requirements for the secure LDAP certificate](#requirements-for-the-secure-ldap-certificate) (Requisitos para el certificado LDAP seguro).


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Opción B: creación de un certificado autofirmado para LDAP seguro
Si prevé que no usará un certificado de una entidad de certificación pública, puede crear un certificado autofirmado para LDAP seguro. Elija esta opción si el nombre de dominio DNS del dominio administrado finaliza por ". onmicrosoft.com".

**Creación de un certificado autofirmado con PowerShell**

En el equipo de Windows, abra una nueva ventana de PowerShell como **Administrador** y escriba los comandos siguientes para crear un nuevo certificado autofirmado.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.com, contoso100.com
```

En el ejemplo anterior, reemplace ".contoso100.com" por el nombre de dominio DNS de su dominio administrado. Por ejemplo, si ha creado un dominio administrado denominado "contoso100.onmicrosoft.com", reemplace "contoso100.com" en el atributo Subject por "contoso100.onmicrosoft.com" y " *.contoso100.com" en el atributo DnsName por "* .contoso100.onmicrosoft.com".

![Selección de un directorio de Azure AD](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

El certificado autofirmado recién creado se colocará en el almacén de certificados del equipo local.


## <a name="next-step"></a>Paso siguiente
[Tarea 2: Exportar el certificado LDAP seguro a un archivo .PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)

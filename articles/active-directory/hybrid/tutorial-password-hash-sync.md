---
title: 'Tutorial:  Integración de un bosque de AD único en Azure mediante la sincronización de hash de contraseñas (PHS) | Microsoft Docs'
description: Se muestra cómo configurar un entorno de identidad híbrida mediante la sincronización de hash de contraseñas.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45379f8f955c50e2598ebcebd34e971c29b2c81c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58103237"
---
# <a name="tutorial--integrate-a-single-ad-forest-using-password-hash-sync-phs"></a>Tutorial:  Integración de un único bosque de AD mediante la sincronización de hash de contraseñas (PHS)

![Crear](media/tutorial-password-hash-sync/diagram.png)

El siguiente tutorial le guiará a través de la creación de un entorno de identidad híbrida mediante la sincronización de hash de contraseñas.  Este entorno puede usarse para realizar pruebas o para familiarizarse con el funcionamiento de una identidad híbrida.

## <a name="prerequisites"></a>Requisitos previos
A continuación, se indican los requisitos previos necesarios para completar este tutorial:
- Un equipo con [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) instalado.  Se recomienda hacer esto en un equipo con [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) o [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- Un [adaptador de red externo](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network) para permitir que la máquina virtual se comunique con Internet.
- [Una suscripción de Azure](https://azure.microsoft.com/free)
- Una copia de Windows Server 2016.

> [!NOTE]
> En este tutorial se utilizan scripts de PowerShell para que pueda crear el entorno de tutorial de la forma más rápida posible.  Cada uno de los scripts usa variables que se declaran al principio de estos.  Puede y debe cambiar las variables para reflejar su entorno.
>
>Los scripts usados crean un entorno general de Active Directory antes de instalar Azure AD Connect.  Son relevantes para todos los tutoriales.
>
> Puede encontrar copias de los scripts de PowerShell que se usan en este tutorial disponibles en GitHub [aquí](https://github.com/billmath/tutorial-phs).

## <a name="create-a-virtual-machine"></a>de una máquina virtual
Lo primero que necesitamos hacer con el fin de poner en funcionamiento el entorno de identidad híbrida es crear una máquina virtual que se usará como nuestro servidor local de Active Directory.  Haga lo siguiente:

1. Abra PowerShell ISE como administrador.
2. Ejecute el siguiente script.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>Finalización de la implementación de sistema operativo
Para terminar de crear la máquina virtual, deberá finalizar la instalación del sistema operativo.

1. En el Administrador de Hyper-V, haga doble clic en la máquina virtual.
2. Haga clic en el botón Iniciar.
3. Se le solicitará que realice la siguiente acción: "Press any key to boot from CD or DVD" ("Presionar cualquier tecla para realizar un arranque desde CD o DVD"). Continúe.
4. En la pantalla de inicio de Windows Server, seleccione el idioma y haga clic en **Siguiente**.
5. Haga clic en **Instalar ahora**.
6. Escriba la clave de licencia y haga clic en **Siguiente**.
7. Marque **Acepto los términos de licencia y haga clic en **Siguiente**.
8. Seleccione **Personalizada:  instalar solo Windows (avanzado)**
9. Haga clic en **Siguiente**
10. Una vez completada la instalación, reinicie la máquina virtual, inicie sesión y ejecute las actualizaciones de Windows para asegurarse de que la versión de la máquina virtual es la más actualizada.  Instale las actualizaciones más recientes.

## <a name="install-active-directory-prerequisites"></a>Requisitos previos de la instalación de Active Directory
Ahora que tenemos una máquina virtual en funcionamiento, necesitamos realizar algunas acciones antes de instalar Active Directory.  Es necesario cambiar el nombre de la máquina virtual, establecer una dirección IP estática y la información de DNS e instalar las herramientas de administración remota del servidor.   Haga lo siguiente:

1. Abra PowerShell ISE como administrador.
2. Ejecute el siguiente script.

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Creación de un entorno de Windows Server AD
Ahora que hemos creado la máquina virtual, cambiado su nombre y establecido una dirección IP estática, podemos continuar con la instalación y configuración de Active Directory Domain Services.  Haga lo siguiente:

1. Abra PowerShell ISE como administrador.
2. Ejecute el siguiente script.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $SecureString -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Creación de un usuario de Windows Server AD
Ahora que tenemos nuestro entorno de Active Directory, necesitamos una cuenta de prueba.  Esta cuenta se creará en nuestro entorno local de AD y, a continuación, se sincronizará con Azure AD.  Haga lo siguiente:

1. Abra PowerShell ISE como administrador.
2. Ejecute el siguiente script.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-an-azure-ad-tenant"></a>Creación de un inquilino de Azure AD
Ahora debemos crear un inquilino de Azure AD para que podamos sincronizar nuestros usuarios con la nube.  Para crear un nuevo inquilino de Azure AD, haga lo siguiente:

1. Vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con una cuenta que tenga una suscripción de Azure.
2. Seleccione el **icono de signo más (+)** y busque **Azure Active Directory**.
3. En la lista de resultados, seleccione **Azure Active Directory**.
4. Seleccione **Crear**.</br>
![Creación](media/tutorial-password-hash-sync/create1.png)</br>
5. Proporcione un **nombre para la organización**, junto con el **nombre de dominio inicial**. Seleccione **Crear**. Se creará el directorio.
6. Una vez que se haya completado, haga clic en el vínculo **aquí** para administrar el directorio.

## <a name="create-a-global-administrator-in-azure-ad"></a>Creación de un administrador global en Azure AD
Ahora que tenemos un inquilino de Azure AD, crearemos una cuenta de administrador global.  Esta cuenta se usa para crear la cuenta del conector de Azure AD durante la instalación de Azure AD Connect.  La cuenta del conector de Azure AD se utiliza para escribir información en Azure AD.   Para crear la cuenta de administrador global, haga lo siguiente:

1.  En **Administrar**, seleccione **Usuarios**.</br>
![Creación](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  Seleccione **Todos los usuarios** y, a continuación, seleccione **+ Nuevo usuario**.
3.  Proporcione un nombre y el nombre de usuario para este usuario. Este será el administrador global del inquilino. También deberá cambiar el **rol del directorio** a **Administrador global**. También puede mostrar la contraseña temporal. Cuando termine, seleccione **Crear**.</br>
![Creación](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Una vez completado, abra un nuevo explorador web e inicie sesión en myapps.microsoft.com con la nueva cuenta de administrador global y la contraseña temporal.
5. Cambie la contraseña de administrador global por algo que pueda recordar.

## <a name="download-and-install-azure-ad-connect"></a>Descarga e instalación de Azure AD Connect
Ahora es el momento de descargar e instalar Azure AD Connect.  Una vez instalado, se ejecutará a través de la instalación rápida.  Haga lo siguiente:

1. Descargue [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).
2. Navegue hasta **AzureADConnect.msi**y haga doble clic en él.
3. En la pantalla de bienvenida, active la casilla que acepta los términos de licencia y haga clic en **Continuar**.  
4. En la pantalla Configuración rápida, haga clic en **Usar configuración rápida**.</br>  
![Creación](media/tutorial-password-hash-sync/express1.png)</br>
5. En la pantalla Conectar a Azure AD, escriba el nombre de usuario y la contraseña del administrador global de Azure AD. Haga clic en **Next**.  
6. En la pantalla Conectar a AD DS, escriba el nombre de usuario y la contraseña de una cuenta de administrador de empresa. Haga clic en **Next**.  
7. En la pantalla Listo para configurar, haga clic en **Instalar**.
8. Una vez completada la instalación, haga clic en **Salir**.
9. Una vez completada la instalación, cierre sesión e inicie sesión de nuevo antes de utilizar Synchronization Service Manager o el Editor de reglas de sincronización.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Comprobación de la creación y sincronización de los usuarios
Ahora comprobaremos que los usuarios que teníamos en nuestro directorio local se han sincronizado y que ya existen en el inquilino de Azure AD.  Tenga en cuenta que esta acción puede tardar unas horas en completarse.  Para comprobar que los usuarios están sincronizados, haga lo siguiente:


1. Vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con una cuenta que tenga una suscripción de Azure.
2. En la parte izquierda, seleccione **Azure Active Directory**.
3. En **Administrar**, seleccione **Usuarios**.
4. Compruebe que ve los usuarios nuevos en nuestro inquilino.</br>
![Synch](media/tutorial-password-hash-sync/synch1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>Prueba del inicio de sesión con uno de nuestros usuarios

1. Vaya a [https://myapps.microsoft.com](https://myapps.microsoft.com).
2. Inicie sesión con la cuenta de usuario que se creó en nuestro nuevo inquilino.  Deberá iniciar sesión mediante el formato siguiente: (user@domain.onmicrosoft.com). Use la misma contraseña que el usuario utiliza para iniciar sesión en el entorno local.</br>
   ![Verify](media/tutorial-password-hash-sync/verify1.png)</br>

Ahora tiene configurado correctamente un entorno de identidad híbrida que puede usar para probar y familiarizarse con lo que le ofrece Azure.

## <a name="next-steps"></a>Pasos siguientes


- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md)|

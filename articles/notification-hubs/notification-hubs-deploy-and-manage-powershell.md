---
title: Implementación y administración de Notification Hubs mediante PowerShell
description: Creación y administración de centros de notificaciones mediante PowerShell para Automation
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 4dbbaeea736dd46478ad9992201ea28bd7bfc2ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457845"
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Implementación y administración de Notification Hubs mediante PowerShell

## <a name="overview"></a>Información general

Este artículo muestra cómo crear y administrar Azure Notification Hubs mediante PowerShell. En este artículo se muestran las siguientes tareas de automatización más comunes.

- Creación de un centro de notificaciones
- Definición de credenciales

Si también necesita crear un nuevo espacio de nombres de Service Bus para sus Notification Hubs, consulte [Administración de Service Bus con PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

No se admite la administración de Centros de notificaciones directamente mediante los cmdlets incluidos con Azure PowerShell. El mejor enfoque en PowerShell es hacer referencia al ensamblado Microsoft.Azure.NotificationHubs.dll. El ensamblado se distribuye con el [paquete NuGet de Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Azure es una plataforma basada en suscripción. Para más información sobre cómo obtener una suscripción, consulte [Opciones de compra], [Ofertas para miembros] o [Evaluación gratuita].
- Un equipo con Azure PowerShell. Para obtener más información, consulte [Instalación y configuración de Azure PowerShell].
- Conocimientos generales sobre los scripts de PowerShell, paquetes de NuGet y .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Incluir una referencia al ensamblado .NET para Service Bus

La administración de Azure Notification Hubs no está incluida aún con los cmdlets de Azure PowerShell. Para aprovisionar centros de notificaciones, puede usar el cliente de .NET ofrecido en el [paquete NuGet de Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

En primer lugar, asegúrese de que el script puede encontrar el ensamblado **Microsoft.Azure.NotificationHubs.dll** , que se instala como un paquete NuGet en un proyecto de Visual Studio. Para ser flexible, el script llevará a cabo estos pasos:

1. Determina la ruta de acceso en la que se invocó.
2. Atraviesa la ruta de acceso hasta que encuentra una carpeta denominada `packages`. Esta carpeta se crea al instalar paquetes NuGet para proyectos de Visual Studio.
3. Se busca de forma recursiva en la carpeta `packages` un ensamblado denominado `Microsoft.Azure.NotificationHubs.dll`.
4. Hace referencia al ensamblado de modo que los tipos estarán disponibles para su uso posterior.

Aquí se explica cómo se implementan estos pasos en un script de PowerShell:

``` powershell
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Creación de la clase `NamespaceManager`

Para aprovisionar Notification Hubs, cree una instancia de la clase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) desde el SDK.

Puede usar el cmdlet [Get-AzureSBAuthorizationRule] que se incluye con Azure PowerShell para recuperar una regla de autorización que se usa para proporcionar una cadena de conexión. Se almacena una referencia a la instancia de `NamespaceManager` en la variable `$NamespaceManager`. Se usa `$NamespaceManager` para aprovisionar un centro de notificaciones.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-a-new-notification-hub"></a>Aprovisionamiento de un nuevo centro de notificaciones

Para aprovisionar un nuevo centro de notificaciones, use la [API de .NET para Notification Hubs].

En esta parte del script, configuramos cuatro variables locales.

1. `$Namespace`: Establezca esta variable en el nombre del espacio de nombres donde quiera crear un centro de notificaciones.
2. `$Path`: Establezca esta ruta de acceso en el nombre del nuevo centro de notificaciones.  Por ejemplo, "MyHub".
3. `$WnsPackageSid`: Establezca esta variable en el SID del paquete de la aplicación de Windows desde el [Centro de desarrollo de Windows](https://developer.microsoft.com/en-us/windows).
4. `$WnsSecretkey`: Establezca este valor en la clave secreta para la aplicación de Windows desde el [Centro de desarrollo de Windows](https://developer.microsoft.com/en-us/windows).

Estas variables se usan para conectar con el espacio de nombres y crear un nuevo centro de notificaciones configurado para controlar las notificaciones de los servicios de notificación de Windows (WNS) con las credenciales de WNS en una aplicación de Windows. Para información sobre cómo obtener el SID del paquete y la clave secreta, vea el tutorial [Introducción a Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .

- El fragmento de código de script usa el objeto `NamespaceManager` para comprobar si existe el Centro de notificaciones identificado por `$Path`.
- Si no existe, el script crea `NotificationHubDescription` con las credenciales de WNS y lo pasa al método `CreateNotificationHub` de la clase `NamespaceManager`.

``` powershell
$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```

## <a name="additional-resources"></a>Recursos adicionales

- [Administración de Service Bus con PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Cómo crear colas, temas y suscripciones de Service Bus con un script de PowerShell](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Cómo crear un espacio de nombres de Service Bus y un centro de eventos mediante un script de PowerShell](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Además, puede descargar algunos scripts listos para usar:

- [Scripts de PowerShell de Service Bus](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[Opciones de compra]: https://azure.microsoft.com/pricing/purchase-options/
[Ofertas para miembros]: https://azure.microsoft.com/pricing/member-offers/
[Evaluación gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Instalación y configuración de Azure PowerShell]: /powershell/azureps-cmdlets-docs
[API de .NET para Notification Hubs]: https://docs.microsoft.com/dotnet/api/overview/azure/notification-hubs?view=azure-dotnet
[Get-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbnamespace
[New-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace
[Get-AzureSBAuthorizationRule]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule

---
title: Administración de Azure CDN con PowerShell | Microsoft Docs
description: Aprenda a usar los cmdlets de Azure PowerShell para administrar Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: magattus
ms.openlocfilehash: 0ad3d1693e2dbf1c4f5329ec23265ea1b3469e2e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66125456"
---
# <a name="manage-azure-cdn-with-powershell"></a>Administración de Azure CDN con PowerShell
PowerShell proporciona uno de los métodos más flexibles para administrar los perfiles y puntos de conexión de Azure CDN.  Puede usar PowerShell interactivamente o mediante la escritura de scripts para automatizar tareas de administración.  Este tutorial muestra algunas de las tareas más comunes que puede realizar con PowerShell para administrar los perfiles y puntos de conexión de Azure CDN.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para usar PowerShell para administrar los perfiles y puntos de conexión de Azure CDN debe tener instalado el módulo de Azure PowerShell.  Para obtener información sobre cómo instalar Azure PowerShell y conectarlo a Azure mediante el cmdlet `Connect-AzAccount` consulte [Cómo instalar y configurar Azure PowerShell](/powershell/azure/overview).

> [!IMPORTANT]
> Debe iniciar sesión con `Connect-AzAccount` para poder ejecutar los cmdlets de Azure PowerShell.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Lista de los cmdlets de Azure CDN
Puede enumerar todos los cmdlets de Azure CDN mediante el cmdlet `Get-Command` .

```text
PS C:\> Get-Command -Module Az.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpointNameAvailability             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfileSsoUrl                        2.0.0      Az.Cdn
Cmdlet          New-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          New-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          New-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Publish-AzCdnEndpointContent                  2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnCustomDomain                      2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnEndpoint                          2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnProfile                           2.0.0      Az.Cdn
Cmdlet          Set-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Set-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Set-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Start-AzCdnEndpoint                           2.0.0      Az.Cdn
Cmdlet          Stop-AzCdnEndpoint                            2.0.0      Az.Cdn
Cmdlet          Test-AzCdnCustomDomain                        2.0.0      Az.Cdn
Cmdlet          Unpublish-AzCdnEndpointContent                2.0.0      Az.Cdn
```

## <a name="getting-help"></a>Ayuda
Puede obtener ayuda para cualquiera de estos cmdlets mediante el cmdlet `Get-Help` .  `Get-Help` proporciona el uso y la sintaxis y, opcionalmente, muestra ejemplos.

```text
PS C:\> Get-Help Get-AzCdnProfile

NAME
    Get-AzCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzCdnProfile -examples".
    For more information, type: "get-help Get-AzCdnProfile -detailed".
    For technical information, type: "get-help Get-AzCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Listado de perfiles existentes de Azure CDN
El cmdlet `Get-AzCdnProfile` sin parámetros permite recuperar todos los perfiles de red CDN existentes.

```powershell
Get-AzCdnProfile
```

Esta salida se puede canalizar a cmdlets para la enumeración.

```powershell
# Output the name of all profiles on this subscription.
Get-AzCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

También puede devolver un solo perfil especificando el grupo de recursos y el nombre del perfil.

```powershell
Get-AzCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Es posible tener varios perfiles de red CDN con el mismo nombre, siempre y cuando estén en distintos grupos de recursos.  Si se omite el parámetro `ResourceGroupName` se devolverán todos los perfiles con un nombre coincidente.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Listado de los puntos de conexión existentes de la red CDN
`Get-AzCdnEndpoint` puede recuperar un punto de conexión individual o todos los puntos de conexión de un perfil.  

```powershell
# Get a single endpoint.
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzCdnProfile | Get-AzCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzCdnProfile | Get-AzCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Creación de perfiles y puntos de conexión de red CDN
`New-AzCdnProfile` y `New-AzCdnEndpoint` se utilizan para crear perfiles y puntos de conexión de red CDN. Se admiten las SKU siguientes:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_Microsoft
- Standard_ChinaCdn

```powershell
# Create a new profile
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Comprobación de la disponibilidad del nombre de un punto de conexión
`Get-AzCdnEndpointNameAvailability` devuelve un objeto que indica si un nombre de punto de conexión está disponible.

```powershell
# Retrieve availability
$availability = Get-AzCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Incorporación de un dominio personalizado
`New-AzCdnCustomDomain` agrega un nombre de dominio personalizado a un punto de conexión existente.

> [!IMPORTANT]
> Necesita configurar el CNAME con su proveedor de DNS como se describe en [Cómo asignar un dominio personalizado al punto de conexión de Content Delivery Network (CDN)](cdn-map-content-to-custom-domain.md).  Puede probar la asignación antes de modificar el punto de conexión mediante `Test-AzCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Modificación de un punto de conexión
`Set-AzCdnEndpoint` modifica un punto de conexión ya existente.

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Purga o precarga de los recursos de red CDN
`Unpublish-AzCdnEndpointContent` purga los recursos de la caché, mientras que `Publish-AzCdnEndpointContent` precarga los recursos en puntos de conexión compatibles.

```powershell
# Purge some assets.
Unpublish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzCdnProfile | Get-AzCdnEndpoint | Unpublish-AzCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Inicio y detención de puntos de conexión de red CDN
`Start-AzCdnEndpoint` y `Stop-AzCdnEndpoint` se pueden utilizar para iniciar y detener los puntos de conexión individuales o los grupos de estos.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Stop-AzCdnEndpoint

# Start all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Start-AzCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Eliminación de recursos de red CDN
`Remove-AzCdnProfile` y `Remove-AzCdnEndpoint` se pueden utilizar para quitar perfiles y puntos de conexión.

```powershell
# Remove a single endpoint
Remove-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzCdnEndpoint | Remove-AzCdnEndpoint -Force

# Remove a single profile
Remove-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Pasos siguientes
Aprenda a automatizar Azure CDN con [.NET](cdn-app-dev-net.md) o [Node.js](cdn-app-dev-node.md).

Para obtener información sobre las características de la red CDN, vea [Información general de la red CDN](cdn-overview.md).


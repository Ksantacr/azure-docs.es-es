---
title: 'Ejemplo de script de Azure PowerShell: implementar una aplicación en un clúster | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: implementar una aplicación en un clúster de Service Fabric.'
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 6de7a721a5042a20537d3f09ea9b8de1cc0e7169
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622040"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Implementación de una aplicación en un clúster de Service Fabric

Este script de ejemplo copia un paquete de aplicación en un almacén de imágenes de clúster, registra el tipo de aplicación en el clúster, quita el paquete de aplicación innecesario y crea una instancia de la aplicación a partir del tipo de aplicación.  Si se definieron servicios predeterminados en el manifiesto de aplicación del tipo de aplicación de destino, también se crean en este momento. Personalice los parámetros según sea necesario. 

Si es necesario, instale el módulo Service Fabric PowerShell con el [SDK de Service Fabric](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Después de que el ejemplo de script se haya ejecutado, el script de [Remove an application](service-fabric-powershell-remove-application.md) (Quitar una aplicación) puede utilizarse para quitar la instancia de la aplicación, anular el registro del tipo de aplicación y eliminar el paquete de aplicación del almacén de imágenes.

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Crea una conexión a un clúster de Service Fabric. |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Copia un paquete de aplicación en el almacén de imágenes del clúster.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Registra el tipo y la versión de una aplicación en el clúster. |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Crea una aplicación a partir de un tipo de aplicación registrada. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Quita un paquete de aplicación de Service Fabric del almacén de imágenes.|

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el módulo Service Fabric PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Puede encontrar ejemplos de PowerShell para Azure Service Fabric en los [ejemplos de Azure PowerShell](../service-fabric-powershell-samples.md).

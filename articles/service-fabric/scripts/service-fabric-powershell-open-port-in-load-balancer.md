---
title: 'Script de ejemplo de Azure PowerShell: apertura del puerto de la aplicación en el equilibrador de carga | Microsoft Docs'
description: 'Script de ejemplo de Azure PowerShell: apertura de un puerto en el equilibrador de carga de Azure para una aplicación de Service Fabric.'
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
ms.date: 05/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 942dd995651f4c672d50163e5c380b62155b2227
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66161937"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Apertura de un puerto de aplicación en el equilibrador de carga de Azure

Las aplicaciones de Service Fabric que se ejecutan en Azure se ubican detrás del equilibrador de carga de Azure. Este script de ejemplo abre un puerto en un equilibrador de carga de Azure para que una aplicación de Service Fabric se comunique con los clientes externos. Personalice los parámetros según sea necesario. Si el clúster está en un grupo de seguridad de red, también [agregue una regla de grupo de seguridad de red de entrada](service-fabric-powershell-add-nsg-rule.md) para permitir el tráfico entrante.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si es necesario, instale el módulo Service Fabric PowerShell con el [SDK de Service Fabric](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtiene un recurso de Azure.  |
| [Get-AzLoadBalancer](/powershell/module/az.network/get-azloadbalancer) | Obtiene el equilibrador de carga de Azure. |
| [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig) | Agrega una configuración de sondeo a un equilibrador de carga.|
| [Get-AzLoadBalancerProbeConfig](/powershell/module/az.network/get-azloadbalancerprobeconfig) | Obtiene una configuración de sondeo para un equilibrador de carga. |
| [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig) | Agrega una configuración de regla a un equilibrador de carga. |
| [Set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer) | Establece el estado del objetivo para un equilibrador de carga. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Puede encontrar ejemplos de PowerShell para Azure Service Fabric en los [ejemplos de Azure PowerShell](../service-fabric-powershell-samples.md).

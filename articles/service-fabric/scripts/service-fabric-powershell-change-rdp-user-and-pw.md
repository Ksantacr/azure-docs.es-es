---
title: 'Ejemplo de script de Azure PowerShell: actualización del nombre de usuario y la contraseña RDP | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: actualización del nombre de usuario RDP y la contraseña para todos los nodos de clúster de Service Fabric de un tipo de nodo específico.'
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
ms.date: 03/19/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 2ed7176dd9073c41604a94e1b42a549de9a8e03d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159966"
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Actualización del nombre de usuario y la contraseña de administración de las VM en un clúster

Cada [tipo de nodo](../service-fabric-cluster-nodetypes.md) de un clúster de Service Fabric es un conjunto de escalado de máquinas virtuales. Este script de ejemplo actualiza el nombre de usuario y la contraseña de administración para las máquinas virtuales de clúster en un tipo de nodo específico.  Agregue la extensión de VMAccessAgent al conjunto de escalado, porque la contraseña de administración no es una propiedad de conjunto de escalado modificable.  Los cambios de nombre de usuario y contraseña se aplican a todos los nodos del conjunto de escalado. Personalice los parámetros según sea necesario.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si es necesario, instale PowerShell con la instrucción que se encuentra en la [Guía de instalación de Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Obtiene las propiedades de un tipo de nodo de clúster (un conjunto de escalado de máquinas virtuales).   |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension)| Agrega una extensión al conjunto de escalado de máquinas virtuales.|
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss)|Actualiza el estado de un conjunto de escalado de máquinas virtuales al estado de un objeto VMSS local.|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Puede encontrar ejemplos de Azure PowerShell para Azure Service Fabric en los [ejemplos de PowerShell](../service-fabric-powershell-samples.md).

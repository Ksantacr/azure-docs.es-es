---
title: Datos personales en Azure Resource Manager | Microsoft Docs
description: Aprenda a administrar los datos personales asociados a operaciones de Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: db4e1b8705b879fd5716763869462bafdf1f905c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128465"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Administración de datos personales asociados a Azure Resource Manager

Para no exponer información confidencial, elimine toda la información personal que pueda haber proporcionado en implementaciones, grupos de recursos o etiquetas. Azure Resource Manager proporciona operaciones que le permiten administrar los datos personales que haya proporcionado en implementaciones, grupos de recursos o etiquetas.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Eliminación de datos personales en el historial de implementaciones

En el caso de las implementaciones, Resource Manager mantiene los valores de los parámetros y los mensajes de estado en el historial de implementaciones. Estos valores se conservan hasta que se elimina la implementación del historial. Para ver si ha proporcionado datos personales en estos valores, enumere las implementaciones. Si encuentra datos personales, elimine las implementaciones del historial.

Para enumerar las **implementaciones** en el historial, use:

* [Enumerar por grupo de recursos](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az group deployment list](/cli/azure/group/deployment#az-group-deployment-list)

Para eliminar **implementaciones** del historial, use:

* [Eliminar](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Eliminación de datos personales en los nombres de los grupos de recursos

El nombre del grupo de recursos se conserva hasta que se elimina el grupo de recursos. Para ver si ha proporcionado datos personales en los nombres, enumere los grupos de recursos. Si encuentra datos personales, [mueva los recursos](resource-group-move-resources.md) a otro grupo de recursos y elimine el grupo de recursos que tenga los datos personales en el nombre.

Para enumerar los **grupos de recursos**, use:

* [Lista](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az group list](/cli/azure/group#az-group-list)

Para eliminar los **resource groups**, use:

* [Eliminar](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Eliminación de datos personales de etiquetas

Los nombres y valores de las etiquetas se conservan hasta que se elimina o modifica la etiqueta. Para ver si ha especificado datos personales en las etiquetas, enumérelas. Si encuentra datos personales, elimine las etiquetas.

Para enumerar las **etiquetas**, use:

* [Lista](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az tag list](/cli/azure/tag#az-tag-list)

Para eliminar las **etiquetas**, use:

* [Eliminar](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción de Azure Resource Manager, consulte [Información general de Azure Resource Manager](resource-group-overview.md)
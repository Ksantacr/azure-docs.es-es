---
title: Aplicación de seguridad con directivas en máquinas virtuales Linux en Azure | Microsoft Docs
description: Aplicación de una directiva a una máquina virtual Linux de Azure Resource Manager
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: 92aa81281c8b0a6accdcc7b76afefe4fef0488ef
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966188"
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Aplicación de directivas a máquinas virtuales con Linux con Azure Resource Manager
Mediante las directivas, una organización puede aplicar varias convenciones y reglas en toda la empresa. La aplicación del comportamiento deseado puede ayudar a reducir el riesgo a la vez que se contribuye al éxito de la organización. En este artículo, describimos cómo puede usar las directivas de Azure Resource Manager para definir el comportamiento deseado para las máquinas virtuales de su organización.

Si desea una introducción a las directivas, consulte [¿Qué es Azure Policy?](../../governance/policy/overview.md).

## <a name="permitted-virtual-machines"></a>Máquinas virtuales permitidas
Para asegurarse de que las máquinas virtuales de la organización son compatibles con una aplicación, puede restringir los sistemas operativos permitidos. En este ejemplo de directiva siguiente, permitirá que se creen solo máquinas virtuales Ubuntu 14.04.2-LTS.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Use un carácter comodín para modificar la directiva anterior para permitir cualquier imagen de Ubuntu LTS: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Para obtener información sobre los campos de directiva, vea [Alias de directiva](../../governance/policy/concepts/definition-structure.md#aliases).

## <a name="managed-disks"></a>Discos administrados

Para requerir el uso de discos administrados, use la siguiente directiva:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Imágenes para las máquinas virtuales

Por motivos de seguridad, puede obligar a que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno. Puede especificar el grupo de recursos que contiene las imágenes aprobadas o las imágenes aprobadas específicas.

En el ejemplo siguiente se requieren imágenes de un grupo de recursos aprobado:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

En el ejemplo siguiente se especifican los identificadores de las imágenes aprobadas:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Extensiones de máquina virtual

Puede que desee prohibir el uso de ciertos tipos de extensiones. Por ejemplo, una extensión puede no ser compatible con determinadas imágenes personalizadas de máquina virtual. En el ejemplo siguiente se muestra cómo bloquear una extensión específica. Utiliza el anunciante y el tipo para determinar qué extensiones se van a bloquear.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Pasos siguientes
* Después de definir una regla de directiva (como se muestra en los ejemplos anteriores), debe crear la definición de directiva y asignarla a un ámbito. El ámbito puede ser una suscripción, un grupo de recursos o un recurso. Para asignar directivas, consulte [Uso de Azure Portal para asignar y administrar directivas de recursos](../../governance/policy/assign-policy-portal.md), [Uso de PowerShell para asignar directivas](../../governance/policy/assign-policy-powershell.md) o [Uso de la CLI de Azure para asignar directivas](../../governance/policy/assign-policy-azurecli.md).
* Si desea una introducción a las directivas de recursos, consulte [¿Qué es Azure Policy?](../../governance/policy/overview.md).
* Para obtener instrucciones sobre cómo las empresas pueden utilizar Resource Manager para administrar eficazmente las suscripciones, vea [Scaffold empresarial de Azure: Gobernanza de suscripción prescriptiva](/azure/architecture/cloud-adoption-guide/subscription-governance).

---
title: Elemento de interfaz de usuario VirtualNetworkCombo de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Network.VirtualNetworkCombo para Azure Portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: b0437338b403ff19761173d08be3938d07f13f55
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60252492"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Elemento de la interfaz de usuario Microsoft.Network.VirtualNetworkCombo
Un grupo de controles para seleccionar una red virtual nueva o existente.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
Cuando el usuario selecciona una nueva red virtual, puede personalizar el nombre y el prefijo de dirección de cada subred. La configuración de subredes es opcional.

![Microsoft.Network.VirtualNetworkCombo nueva](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Cuando el usuario selecciona una red virtual existente, debe asignar cada subred que requiere la plantilla de implementación a una subred existente. En este caso, la configuración de subredes es obligatoria.

![Microsoft.Network.VirtualNetworkCombo existente](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentarios
- Si se ha especificado, el primer prefijo de dirección que no se superponga del tamaño `defaultValue.addressPrefixSize` se determina automáticamente en función de las redes virtuales existentes en la suscripción del usuario.
- El valor predeterminado de `defaultValue.name` y `defaultValue.addressPrefixSize` es **null**.
- `constraints.minAddressPrefixSize` debe especificarse. Las redes virtuales existentes con un espacio de direcciones menor que el valor especificado no se pueden seleccionar.
- `subnets` debe especificarse y `constraints.minAddressPrefixSize` debe especificarse para cada subred.
- Al crear una nueva red virtual, el prefijo de dirección de cada subred se calcula automáticamente basándose en el prefijo de dirección de la red virtual y en los respectivos `addressPrefixSize`.
- Si se usa una red virtual existente, las subredes menores que la `constraints.minAddressPrefixSize` respectiva no están disponibles para la selección. Además, si se especifica, las subredes que no tienen al menos `minAddressCount` direcciones disponibles no se podrán seleccionar. El valor predeterminado es **0**. Para asegurarse de que las direcciones disponibles son contiguas, especifique **true** en `requireContiguousAddresses`. El valor predeterminado es **true**.
- No se admite la creación de subredes en una red virtual existente.
- Si el valor de `options.hideExisting` es **true**, el usuario no puede elegir una red virtual existente. El valor predeterminado es **false**.

## <a name="sample-output"></a>Salida de ejemplo

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).

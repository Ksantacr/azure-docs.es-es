---
title: Elemento de interfaz de usuario PublicIpAddressCombo de Azure | Microsoft Docs
description: Describe el elemento de la interfaz de usuario Microsoft.Network.PublicIpAddressCombo para Azure Portal.
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
ms.openlocfilehash: c3e8c99f6648f0f4927140f3215978566afb9eb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251092"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Elemento de interfaz de usuario Microsoft.Network.PublicIpAddressCombo
Grupo de controles para seleccionar una dirección IP pública nueva o existente.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Si el usuario selecciona “None” para la dirección IP pública, se oculta el cuadro de texto de la etiqueta de nombre de dominio.
- Si el usuario selecciona una dirección IP pública, se deshabilita el cuadro de texto de la etiqueta de nombre de dominio. Su valor es la etiqueta de nombre de dominio de la dirección IP seleccionada.
- El sufijo de nombre de dominio (por ejemplo, westus.cloudapp.azure.com) se actualiza automáticamente en función de la ubicación seleccionada.

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentarios
- Si `constraints.required.domainNameLabel` está establecido en **true**, el usuario debe proporcionar una etiqueta de nombre de dominio al crear una dirección IP pública nueva. Las direcciones IP públicas existentes sin una etiqueta no están disponibles para la selección.
- Si `options.hideNone` está establecido en **true**, la opción para seleccionar **None** como dirección IP pública está oculta. El valor predeterminado es **false**.
- Si `options.hideDomainNameLabel` está establecido en **true**, se oculta el cuadro de texto de la etiqueta de nombre de dominio. El valor predeterminado es **false**.
- Si `options.hideExisting` es true, el usuario no puede elegir una dirección IP pública existente. El valor predeterminado es **false**.
- Para `zone`, solo están disponibles las direcciones IP pública para la zona especificada o direcciones IP públicas resistentes al uso de zonas.

## <a name="sample-output"></a>Salida de ejemplo
Si el usuario no selecciona ninguna dirección IP pública, el control devuelve la siguiente salida:

```json
{
  "newOrExistingOrNone": "none"
}
```

Si el usuario selecciona una dirección IP pública nueva o existente, el control devuelve la siguiente salida:

```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "sku": "Basic",
  "newOrExistingOrNone": "new"
}
```

- Cuando `options.hideNone` se especifica como **true**, `newOrExistingOrNone` solo tendrá un valor de **nuevo** o **existente**.
- Cuando `options.hideDomainNameLabel` se especifica como **verdadero**, `domainNameLabel` no se declara.

## <a name="next-steps"></a>Pasos siguientes
* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).

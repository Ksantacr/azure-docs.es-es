---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120890"
---
[!INCLUDE [resource group intro text](resource-group.md)]

En Cloud Shell, cree un grupo de recursos con el comando [`az group create`](/cli/azure/group?view=azure-cli-latest). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *Europa Occidental*. Para ver todas las ubicaciones que se admiten en App Service en el nivel **Gratis**, ejecute el comando [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Generalmente se crean el grupo de recursos y los recursos en una región cercana. 

Cuando finaliza el comando, una salida de JSON muestra las propiedades del grupo de recursos.
---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c3677e7897498aa06d7bd547988ad4dc0326f39b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150447"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitación del proveedor de recursos de Event Grid

Si aún no ha usado anteriormente Event Grid en su suscripción de Azure, puede que tenga que registrar el proveedor de recursos de Event Grid.

En Azure Portal:

1. Seleccione **Suscripciones**.
1. Seleccione la suscripción que está usando para Event Grid.
1. En **Configuración**, seleccione **Proveedores de recursos**.
1. Busque **Microsoft.EventGrid**.
1. Si no está registrado, seleccione **Registrar**. 

Puede tardar unos instantes en finalizarse el registro. Seleccione **Actualizar**  para actualizar el estado. Cuando el **estado** es **Registrado**, ya está listo para continuar.
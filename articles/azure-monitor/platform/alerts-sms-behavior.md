---
title: Comportamiento de las alertas por SMS en los grupos de acciones
description: Formato de mensajes SMS y respuesta a los mensajes SMS de cancelación de suscripción, segunda suscripción o solicitud de ayuda.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 74666149824627308b6c5b026e0c9ba7a7750ada
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60346304"
---
# <a name="sms-alert-behavior-in-action-groups"></a>Comportamiento de las alertas por SMS en los grupos de acciones
## <a name="overview"></a>Información general ##
Los grupos de acciones le permiten configurar una lista de acciones. Estos grupos se utilizan al definir alertas, lo que garantiza que cuando se activa una alerta, un grupo de acciones concreto recibe la notificación. Una de las acciones que se admiten es SMS; las notificaciones SMS admiten la comunicación bidireccional. Un usuario puede responder a un SMS para:

- **Cancelar la suscripción a alertas:** un usuario puede cancelar la suscripción a todas las alertas por SMS para todos los grupos de acciones o para un único grupo de acciones.
- **Volver a suscribirse a alertas:** un usuario puede volver a suscribirse a todas las alertas por SMS para todos los grupos de acciones o para un solo grupo de acciones.  
- **Solicitar ayuda:** un usuario puede pedir más información acerca del SMS. Se le redirigirá a este artículo.

En este artículo se trata el comportamiento de las alertas por SMS y las acciones de respuesta que el usuario puede realizar en función de su configuración regional:

## <a name="receiving-an-sms-alert"></a>Recepción de una alerta por SMS
Un receptor de SMS, que se configura como parte de un grupo de acciones, recibirá un SMS cuando se desencadene una alerta. El SMS contiene la siguiente información:
* El nombre corto del grupo de acciones al que se envió esta alerta
* El título de la alerta

| RESPUESTA | DESCRIPCIÓN |
| ----- | ----------- |
| DISABLE `<Action Group Short name>` | Deshabilita SMS adicionales del grupo de acciones |
| ENABLE `<Action Group Short name>` | Vuelve a habilitar los SMS en el grupo de acciones |
| STOP | Deshabilita SMS adicionales de todos los grupos de acciones |
| START | Vuelve a habilitar SMS en TODOS los grupos de acciones |
| HELP | Se enviará una respuesta al usuario con un vínculo a este artículo. |

>[!NOTE]
>Si un usuario ha cancelado la suscripción a las alertas por SMS, pero luego se agrega a un nuevo grupo de acciones; SE recibirán alertas por SMS para el grupo de acciones nuevo, pero permanecerán sin suscripción de todos los grupos de acciones anteriores.

## <a name="next-steps"></a>Pasos siguientes
Obtener una [introducción a las alertas del registro de actividad](alerts-overview.md) y aprender cómo puede recibir alertas  
Más información sobre la [limitación de la tasa de SMS](alerts-rate-limiting.md)  
Más información sobre los [grupos de acciones](../../azure-monitor/platform/action-groups.md)


---
title: Configuración de alertas de estado del servicio de Azure con PagerDuty | Microsoft Docs
description: Obtenga notificaciones personalizadas sobre los eventos del estado de servicio en la instancia de PagerDuty.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 11/14/2017
ms.openlocfilehash: b78c155fb2f3a13c27f4ff71c4dd37df2dbd2f36
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621019"
---
# <a name="configure-service-health-alerts-with-pagerduty"></a>Configuración de alertas de estado del servicio con PagerDuty

Este artículo muestra cómo configurar las notificaciones de estado del servicio de Azure con PagerDuty mediante un webhook. Mediante el tipo de integración de Microsoft Azure personalizado de [PagerDuty](https://www.pagerduty.com/), puede agregar fácilmente alertas de estado del servicio a los servicios de PagerDuty nuevos o existentes.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>Creación de una dirección URL de integración de estado del servicio en PagerDuty
1.  Asegúrese de que se ha registrado y ha iniciado sesión en su cuenta de [PagerDuty](https://www.pagerduty.com/).

1.  Vaya hasta la sección **Services** (Servicios) en PagerDuty.

    ![La sección "Services" (Servicios) en PagerDuty](./media/webhook-alerts/pagerduty-services-section.png)

1.  Seleccione **Add New Service** (Agregar nuevo servicio) o abra un servicio existente que haya configurado.

1.  En **Integration Settings** (Configuración de la integración), seleccione lo siguiente:

     a. **Tipo de integración**: Microsoft Azure

    b. **Nombre de integración**: \<Nombre\>

    !["Integration Settings" (Configuración de la integración) en PagerDuty](./media/webhook-alerts/pagerduty-integration-settings.png)

1.  Rellene los demás campos obligatorios y seleccione **Add** (Agregar).

1.  Abra esta nueva integración y copie y guarde la **dirección URL de la integración**.

    ![La "dirección URL de integración" en PagerDuty](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Creación de una alerta con PagerDuty en Azure Portal
### <a name="for-a-new-action-group"></a>Para un nuevo grupo de acciones:
1. Siga los pasos del 1 al 8 en [Creación de una alerta basada en una notificación de mantenimiento del servicio para un nuevo grupo de acciones con Azure Portal](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

1. Defina la lista de **acciones**:

     a. **Tipo de acción:** *Webhook*

    b. **Detalles:** la **dirección URL de integración** de PagerDuty guardada anteriormente.

    c. **Nombre:** el identificador, alias o nombre de webhook.

1. Seleccione **Guardar** cuando termine para crear la alerta.

### <a name="for-an-existing-action-group"></a>Para un grupo de acciones existentes:
1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisión**.

1. En la sección **Configuración**, seleccione **Grupos de acciones**.

1. Busque el grupo de acciones que desee editar.

1. Defina la lista de **acciones**:

     a. **Tipo de acción:** *Webhook*

    b. **Detalles:** la **dirección URL de integración** de PagerDuty guardada anteriormente.

    c. **Nombre:** el identificador, alias o nombre de webhook.

1. Cuando termine de actualizar el grupo de acciones, seleccione **Guardar**.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Prueba de la integración de webhook a través de una solicitud HTTP POST
1. Cree la carga de estado del servicio que desee enviar. Puede encontrar una carga de webhook de estado del servicio de ejemplo en [Webhooks para alertas del registro de actividad de Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Cree una solicitud HTTP POST de la siguiente manera:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Debería recibir `202 Accepted` con un mensaje que contiene el "identificador del evento."

1. Vaya a [PagerDuty](https://www.pagerduty.com/) para confirmar que la integración se ha configurado correctamente.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](service-health-alert-webhook-guide.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Más información acerca de las [Notificaciones del estado del servicio](../azure-monitor/platform/service-notifications.md).
- Más información sobre los [grupos de acciones](../azure-monitor/platform/action-groups.md).
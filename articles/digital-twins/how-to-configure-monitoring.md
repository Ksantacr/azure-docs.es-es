---
title: Configuración de la supervisión en Azure Digital Twins | Microsoft Docs
description: Configuración de la supervisión en Azure Digital Twins.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 3c35633c9b25eafdb738ed591c7c7022fbd8149a
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967810"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Configuración de la supervisión en Azure Digital Twins

Azure Digital Twins es compatible con un eficaz sistema de registro, supervisión y análisis. Los programadores de soluciones pueden usar los registros, registros de diagnóstico, el registro de actividad y otros servicios de Azure Monitor para admitir las complejas necesidades de supervisión de una aplicación de IoT. Las opciones de registro se pueden combinar para consultar o mostrar registros en diferentes servicios y ofrecer una cobertura de registro pormenorizada para muchos servicios.

En este artículo se resumen las opciones de registro y supervisión y cómo combinarlas de manera específica para Azure Digital Twins.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Revisión de registros de actividad

Los [registros de actividad](../azure-monitor/platform/activity-logs-overview.md) de Azure proporcionan información rápida sobre los eventos a nivel de suscripción y los historiales de operación para cada instancia de servicio de Azure.

Los eventos a nivel de suscripción incluyen:

* Creación de recursos
* Eliminación de recursos
* Creación de secretos de aplicación
* Integración con otros servicios

El registro de actividad de Azure Digital Twins está habilitado de forma predeterminada; para encontrarlo en Azure Portal, puede hacer lo siguiente:

1. Seleccionar la instancia de Azure Digital Twins.
1. Elegir **Registro de actividad** para que aparezca el panel de pantalla:

    ![Registro de actividades][1]

Para el registro de actividad avanzada:

1. Seleccione la opción **Registros** para mostrar la **información general de Activity Log Analytics**:

    ![Número de selección][2]

1. La **información general de Activity Log Analytics** resume los datos de registro de actividad esenciales:

    ![Introducción al análisis de registros de actividad][3]

>[!TIP]
>Use los **registros de actividad** para obtener información rápida sobre los eventos a nivel de suscripción.

## <a name="enable-customer-diagnostic-logs"></a>Habilitación de registros de diagnóstico de cliente

Es posible establecer la [configuración de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md) de Azure para cada instancia de Azure a fin de complementar el registro de actividad. Si bien los registros de actividad pertenecen a los eventos de nivel de suscripción, el registro de diagnóstico proporciona información sobre el historial de operaciones de los propios recursos.

Estos son algunos ejemplos del registro de diagnóstico:

* El tiempo de ejecución de una función definida por el usuario
* El código de estado de respuesta de una solicitud de API correcta
* La recuperación de una clave de aplicación de un almacén

Para habilitar los registros de diagnóstico para una instancia:

1. Abra el recurso en Azure Portal.
1. Haga clic en **Configuración de diagnóstico**:

    ![Configuración de diagnóstico número uno][4]

1. Haga clic en **Activar diagnóstico** para recopilar datos (si no se habilitó anteriormente).
1. Rellene los campos solicitados y seleccione cómo y dónde se guardarán los datos:

    ![Configuración de diagnóstico número dos][5]

    A menudo se guardan registros de diagnóstico mediante [Azure File Storage](../storage/files/storage-files-deployment-guide.md) y se comparte con [registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md). Ambas opciones se pueden seleccionar.

>[!TIP]
>Use los **registros de diagnóstico** para obtener información sobre las operaciones de recursos.

## <a name="azure-monitor-and-log-analytics"></a>Azure Monitor y Log Analytics

Las aplicaciones de IoT reúnen recursos dispares, dispositivos, ubicaciones y datos en un mismo lugar. El registro pormenorizado proporciona información detallada sobre cada pieza, servicio o componente específico de la arquitectura general de la aplicación, pero a menudo se requiere una perspectiva unificada para abordar el mantenimiento y la depuración.

Azure Monitor incluye el servicio de análisis de registros de gran eficacia, lo que permite el registro de orígenes para verse y analizarse en una ubicación. Azure Monitor, por tanto, es muy útil para analizar los registros dentro de sofisticadas aplicaciones de IoT.

Estos son algunos ejemplos de uso:

* Consultar varios historiales de registro de diagnóstico
* Ver los registros de varias funciones definidas por el usuario
* Mostrar los registros de dos o más servicios en un período de tiempo específico

Consultar el registro completo se proporciona a través de [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md). Para configurar estas eficaces características:

1. Busque **Log Analytics** en Azure Portal.
1. Verá su disponible **área de trabajo de Log Analytics** instancias. Elija una y seleccione **Registros** para consultar:

    ![Log Analytics][6]

1. Si aún no tiene un **área de trabajo de Log Analytics** instancia, puede crear un área de trabajo, haga clic en el **agregar** botón:

    ![Creación de OMS][7]

Una vez su **área de trabajo de Log Analytics** se aprovisiona la instancia, puede usar consultas eficaces para buscar entradas en los registros de múltiplos o buscar mediante criterios específicos mediante **administración de registros**:

   ![Administración de registros][8]

Para obtener más información acerca de estas eficaces operaciones de consulta, lea [Introducción a las consultas](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Puede experimentar un retraso de 5 minutos al enviar eventos a **área de trabajo de Log Analytics** por primera vez.

Registros de Azure Monitor proporciona también error eficaces y servicios de notificación de alerta, que pueden verse haciendo clic en **diagnosticar y resolver problemas**:

   ![Notificaciones de alerta y de error][9]

>[!TIP]
>Use **área de trabajo de Log Analytics** a los historiales de registro de consultas para varios servicios, suscripciones o las funcionalidades de la aplicación.

## <a name="other-options"></a>Otras opciones

Azure Digital Twins también admite el registro y la auditoría de seguridad específicos de una aplicación. Para obtener una introducción exhaustiva de todas las opciones de registro de Azure disponibles para la instancia de Azure Digital Twins, consulte el artículo [Registro y auditoría de Azure](../security/azure-log-audit.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [registros de actividad](../azure-monitor/platform/activity-logs-overview.md).

- Profundice en la configuración de diagnóstico de Azure consultando la [información general sobre los registros de diagnóstico de Azure](../azure-monitor/platform/diagnostic-logs-overview.md).

- Obtenga más información sobre [registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md).

<!-- Images -->
[1]: media/how-to-configure-monitoring/activity-log.png
[2]: media/how-to-configure-monitoring/activity-log-select.png
[3]: media/how-to-configure-monitoring/log-analytics-overview.png
[4]: media/how-to-configure-monitoring/diagnostic-settings-one.png
[5]: media/how-to-configure-monitoring/diagnostic-settings-two.png
[6]: media/how-to-configure-monitoring/log-analytics.png
[7]: media/how-to-configure-monitoring/log-analytics-oms.png
[8]: media/how-to-configure-monitoring/log-analytics-management.png
[9]: media/how-to-configure-monitoring/log-analytics-notifications.png

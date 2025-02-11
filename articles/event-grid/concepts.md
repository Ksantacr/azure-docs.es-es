---
title: Conceptos de Azure Event Grid
description: Describe Azure Event Grid y sus conceptos. Define varios componentes clave de Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: spelluru
ms.openlocfilehash: 0821c749a6cb718e1b8abb74a2925bc041850eaf
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305272"
---
# <a name="concepts-in-azure-event-grid"></a>Concepts de Azure Event Grid

En este artículo se describen los principales conceptos de Azure Event Grid.

## <a name="events"></a>Eventos

Una evento es la cantidad mínima de información que describe completamente algo que se ha producido en el sistema. Todos los eventos tienen información común, como: origen del evento, hora en que el evento ha tenido lugar e identificador único. Cada evento tiene además información específica que solo es relevante para el tipo de evento concreto. Por ejemplo, un evento sobre un nuevo archivo que se crea en Azure Storage contiene detalles sobre el archivo, como el valor `lastTimeModified`. O bien, un evento de Event Hubs tiene la dirección URL del archivo de captura. 

Un evento de tamaño de hasta 64 KB se tratan por acuerdo de nivel de servicio (SLA) de disponibilidad General (GA). El soporte técnico para un evento de tamaño de hasta 1 MB está actualmente en versión preliminar. Eventos de más de 64 KB se cobran en incrementos de 64 KB. 


Para obtener las propiedades que se envían en un evento, vea [Esquema de eventos de Azure Event Grid](event-schema.md).

## <a name="publishers"></a>Publicadores

Un publicador es el usuario o la organización que decide enviar eventos a Event Grid. Microsoft publica eventos para varios servicios de Azure. Puede publicar eventos desde su propia aplicación. Las organizaciones que hospedan servicios fuera de Azure pueden publicar eventos a través de Event Grid.

## <a name="event-sources"></a>Orígenes de eventos

Un origen de evento es donde se produce el evento. Cada origen de evento está relacionado con uno o más tipos de eventos. Por ejemplo, Azure Storage es el origen de evento de los eventos creados por blob. IoT Hub es el origen de evento de los eventos creados por dispositivo. La aplicación es el origen de evento de los eventos personalizados que defina. Los orígenes de evento son responsables de enviar eventos a Event Grid.

Para información sobre la implementación de cualquiera de los orígenes de Event Grid admitidos, consulte [Event sources in Azure Event Grid](event-sources.md) (Orígenes de eventos en Azure Event Grid).

## <a name="topics"></a>Temas

En el tema de Event Grid se proporciona un punto de conexión al que el origen envía los eventos. El publicador crea el tema de Event Grid y decide si un origen de evento necesita un tema o más de un tema. Un tema se usa para una colección de eventos relacionados. Para responder a determinados tipos de eventos, los suscriptores deciden los temas a los que se suscriben.

Los temas del sistema son temas integrados que ofrecen los servicios de Azure. No ve los temas del sistema en la suscripción de Azure porque el publicador posee los temas, pero puede suscribirse a ellos. Para suscribirse, proporciona información sobre el recurso del que quiere recibir eventos. Siempre que tengan acceso al recurso, puede suscribirse a sus eventos.

Los temas personalizados son temas de terceros y de aplicación. Cuando cree un tema personalizado, o se le asigne acceso al mismo, verá ese tema personalizado en su suscripción.

Cuando diseñe la aplicación, tiene flexibilidad al decidir cuántos temas se crean. Para soluciones grandes, cree un tema personalizado para cada categoría de eventos relacionados. Por ejemplo, considere una aplicación que envía eventos relacionados con la modificación de las cuentas de usuario y el procesamiento de pedidos. Es poco probable que algún controlador de eventos quiera ambas categorías de eventos. Cree dos temas personalizados y deje que los controladores de eventos se suscriban a uno que les interese. Para soluciones pequeñas, puede que prefiera enviar todos los eventos a un solo tema. Los suscriptores de eventos se pueden filtrar por los tipos de evento que desean.

## <a name="event-subscriptions"></a>Suscripciones a eventos

Las suscripciones le indican a Event Grid qué eventos de un tema les interesa recibir. Al crear la suscripción, proporciona un punto de conexión para controlar el evento. Puede filtrar los eventos que se envían al punto de conexión. Puede filtrar por tipo de evento o por patrón de asunto. Para más información, vea [Esquema de suscripción de Event Grid](subscription-creation-schema.md).

Para obtener ejemplos de creación de suscripciones, vea:

* [Ejemplos de la CLI de Azure para Event Grid](cli-samples.md)
* [Ejemplos de Azure PowerShell para Event Grid](powershell-samples.md)
* [Plantillas de Azure Resource Manager para Event Grid](template-samples.md)

Para obtener información sobre las suscripciones de Event Grid actuales, vea [Consulta de suscripciones de Event Grid](query-event-subscriptions.md).

## <a name="event-subscription-expiration"></a>Caducidad de la suscripción de eventos
La suscripción de eventos expira automáticamente después de esa fecha. Establezca una fecha de expiración para las suscripciones de eventos que solamente son necesarias durante un tiempo limitado y de cuya limpieza no quiere preocuparse. Por ejemplo, es posible que, al crear una suscripción de eventos para probar un escenario, desee establecer una fecha de expiración. 

Para ver un ejemplo acerca de cómo se establece una fecha de expiración, consulte [Suscripción con filtros avanzados](how-to-filter-events.md#subscribe-with-advanced-filters).

## <a name="event-handlers"></a>Controladores de eventos

Desde la perspectiva de Event Grid, un controlador de eventos es el lugar al que se envía el evento. El controlador realiza alguna acción adicional para procesar el evento. Event Grid admite varios tipos de controladores. Puede usar un servicio de Azure admitido o su propio webhook como controlador. Según el tipo de controlador, Event Grid sigue distintos procedimientos para garantizar la entrega del evento. En el caso de los controladores de eventos de webhook HTTP, el evento se reintenta hasta que el controlador devuelve un código de estado de `200 – OK`. En Azure Storage Queue, los eventos se reintentan hasta que Queue service procesa correctamente la inserción de mensajes en la cola.

Para información sobre cómo implementar cualquiera de los controladores admitidos de Event Grid, consulte [Event handlers in Azure Event Grid](event-handlers.md) (Controladores de eventos en Azure Event Grid).

## <a name="security"></a>Seguridad

Event Grid proporciona seguridad para suscribirse a temas y publicarlos. Para suscribirse, debe tener los permisos adecuados en el recurso o el tema de Event Grid. Para publicar, debe tener un token de SAS o autenticación de clave para el tema. Para más información, vea [Event Grid security and authentication](security-authentication.md) (Seguridad y autenticación de Event Grid).

## <a name="event-delivery"></a>Entrega de eventos

Si Event Grid no puede confirmar que un evento se ha recibido en el punto de conexión del suscriptor, vuelve a entregarlo. Para más información, vea [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

## <a name="batching"></a>Lotes

Cuando se usa un tema personalizado, los eventos siempre se deben publicar en una matriz. En los escenarios de bajo rendimiento, puede utilizarse un lote con un único evento; sin embargo, en los casos de uso de gran volumen, se recomienda agrupar varios eventos en cada publicación para lograr una mayor eficacia. Los lotes pueden tener hasta 1 MB. Cada evento todavía no debe superior a 64 KB (Disponibilidad General) o 1 MB (versión preliminar).

> [!NOTE]
> Un evento de tamaño de hasta 64 KB se tratan por acuerdo de nivel de servicio (SLA) de disponibilidad General (GA). El soporte técnico para un evento de tamaño de hasta 1 MB está actualmente en versión preliminar. Eventos de más de 64 KB se cobran en incrementos de 64 KB. 

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).

---
title: Supervisar eventos de Azure Media Services con Event Grid mediante el portal | Microsoft Docs
description: En este artículo se muestra cómo puede suscribirse a Event Grid para supervisar eventos de Azure Media Services.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, transmisión, difusión, en vivo, sin conexión
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: d4592c93cb7969c45a107d7365a1b9dabf11f412
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326536"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Crear y supervisar eventos de Media Services con Event Grid mediante Azure Portal

Azure Event Grid es un servicio de eventos para la nube. Este servicio utiliza [suscripciones a eventos](../../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Los eventos de Media Services contienen toda la información necesaria para responder a cualquier cambio que se produzca en los datos. Puede identificar un evento de Media Services porque la propiedad eventType comienza por "Microsoft.Media". Para más información, consulte el artículo sobre los [esquemas de eventos de Media Services](media-services-event-schemas.md).

En este artículo usará Azure Portal para suscribirse a eventos para su cuenta de Azure Media Services. A continuación, deberá desencadenar esos eventos para ver el resultado. Por lo general, se envían eventos a un punto de conexión que procesa los datos del evento y realiza acciones. En el artículo, enviamos eventos a una aplicación web que recopila y muestra los mensajes.

Cuando haya terminado, verá que los datos del evento se han enviado a la aplicación web.

## <a name="prerequisites"></a>Requisitos previos 

* Tener una suscripción de Azure activa.
* Cree una cuenta de Azure Media Services tal como se describe en [este inicio rápido](create-account-cli-quickstart.md).

## <a name="create-a-message-endpoint"></a>Creación de un punto de conexión de mensaje

Antes de suscribirse a los eventos de la cuenta de Media Services, vamos a crear el punto de conexión para el mensaje del evento. Normalmente, el punto de conexión realiza acciones en función de los datos del evento. En este artículo debe implementar una [aplicación web precompilada](https://github.com/Azure-Samples/azure-event-grid-viewer) que muestre los mensajes de los eventos. La solución implementada incluye un plan de App Service, una aplicación web de App Service y el código fuente desde GitHub.

1. Seleccione **Deploy to Azure** (Implementar en Azure) para implementar la solución en su suscripción. En Azure Portal, proporcione valores para los parámetros.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. La implementación puede tardar unos minutos en completarse. Después de que la implementación se haya realizado correctamente, puede ver la aplicación web para asegurarse de que se está ejecutando. En un explorador web, vaya a: `https://<your-site-name>.azurewebsites.net`

Si cambia al sitio "Visor de Azure Event Grid", verá que no tiene todavía ningún evento.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Suscribirse a los eventos de Media Services

Suscríbase a un tema que indique a Event Grid los eventos cuyo seguimiento desea realizar y el lugar al que deben enviarse los eventos.

1. En el portal, seleccione la cuenta de Media Services y seleccione **Eventos**.
1. Para enviar eventos a la aplicación de visor, use un webhook como punto de conexión. 

   ![Selección de webhook](./media/monitor-events-portal/select-web-hook.png)

1. La suscripción de eventos se rellena previamente con los valores de su cuenta de Media Services. 
1. Seleccione "Webhook" como **tipo de punto de conexión**.
1. En este tema deberá dejar la opción **Suscribirse a todos los tipos de evento** activada. Sin embargo, puede desactivarla y filtrar por tipos de eventos específicos. 
1. Haga clic en el vínculo **Seleccionar un punto de conexión**.

    Para el punto de conexión de webhook, proporcione la dirección URL de la aplicación web y agregue `api/updates` a la dirección URL de la página principal. 

1. Pulse **Confirmar la selección**.
1. Pulse **Crear**.
1. Asigne un nombre a su suscripción.

   ![Seleccionar Registros](./media/monitor-events-portal/create-subscription.png)

1. Vuelva a la aplicación web y observe que se ha enviado un evento de validación de suscripción. 

    Event Grid envía el evento de validación para que el punto de conexión pueda verificar que desea recibir datos de eventos. El punto de conexión debe establecer `validationResponse` en `validationCode`. Para más información, vea [Event Grid security and authentication](../../event-grid/security-authentication.md) (Seguridad y autenticación de Event Grid). Puede ver el código de la aplicación web para ver cómo valida la suscripción.

A continuación, desencadenaremos algunos eventos para ver cómo Event Grid distribuye el mensaje al punto de conexión.

## <a name="send-an-event-to-your-endpoint"></a>Envío de un evento al punto de conexión

Puede desencadenar eventos de la cuenta de Media Services si ejecuta un trabajo de codificación. Puede seguir [este inicio rápido](stream-files-dotnet-quickstart.md) para codificar un archivo y empezar a enviar eventos. Si está suscrito a todos los eventos, verá una pantalla similar a la siguiente:

> [!TIP]
> Seleccione el icono del ojo para expandir los datos del evento. No actualice la página si quiere ver todos los eventos.

![Visualización del evento de suscripción](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Pasos siguientes

[Carga, codificación y streaming](stream-files-tutorial-with-api.md)

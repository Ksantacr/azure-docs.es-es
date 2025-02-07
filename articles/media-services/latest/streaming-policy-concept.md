---
title: Directivas de streaming en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son las directivas de streaming y cómo las usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392921"
---
# <a name="streaming-policies"></a>Directivas de streaming

En Azure Media Services v3, las [directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) permiten definir los protocolos de streaming y las opciones de cifrado de los [localizadores de streaming](streaming-locators-concept.md). Media Services v3 proporciona que algunas directivas de transmisión por secuencias de predefinidas para que se pueden usar directamente para la versión de prueba o producción. 

Directivas de transmisión por secuencias predefinidas disponibles actualmente:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

El siguiente "árbol de decisión" le ayudará a elegir una directiva predefinida de transmisión por secuencias para su escenario.

> [!IMPORTANT]
> * Las propiedades de **directivas de streaming** que son del tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con los objetos StreamingLocator siempre que se necesiten las mismas opciones. Para más información, consulte [Cuotas y limitaciones](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Árbol de decisión

Haga clic en la imagen para verla a tamaño completo.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

Si el cifrado del contenido, deberá crear un [directiva de clave de contenido](content-key-policy-concept.md), el **directiva de clave de contenido** no es necesaria para borrar streaming o descarga. 

Si tiene requisitos especiales (por ejemplo, si desea especificar diferentes protocolos, debe usar un servicio de entrega de claves personalizados, o debe usar una pista de audio claro), puede [crear](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) una directiva personalizada de transmisión por secuencias. 

## <a name="get-a-streaming-policy-definition"></a>Obtener una definición de directiva de transmisión por secuencias  

Si desea ver la definición de una directiva de transmisión por secuencias, utilice [obtener](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) y especifique el nombre de la directiva. Por ejemplo:

### <a name="rest"></a>REST

Solicitud:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Respuesta:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Streaming de un archivo](stream-files-dotnet-quickstart.md)
* [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)

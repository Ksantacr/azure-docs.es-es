---
title: Punto de conexión de Bing Entity Search API
titlesuffix: Azure Cognitive Services
description: Más información sobre el punto de conexión de Bing Entity Search API y envío de solicitudes a este.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 43bca65810d09c87f7f473b3bbac71ca6a7f9bc2
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389007"
---
# <a name="bing-entity-search-api-endpoint"></a>Punto de conexión de Bing Entity Search API


Bing Entity Search API incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta. Estos resultados de búsqueda se devuelven en formato JSON.

## <a name="get-entity-results-from-the-endpoint"></a>Obtención de resultados de entidad a partir del punto de conexión

Para obtener resultados de entidades mediante **Bing API**, envíe una solicitud `GET` al punto de conexión siguiente. Use [encabezados](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) y [parámetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) para personalizar la solicitud de búsqueda. Se pueden enviar solicitudes de búsqueda mediante el parámetro `?q=`.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [¿Qué es Bing Entity Search API?](overview.md)

## <a name="see-also"></a>Vea también 

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc, consulte la referencia sobre [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).

---
title: 'Envío de consultas de imágenes: Bing Image Search API'
titleSuffix: Azure Cognitive Services
description: Información acerca de la personalización de las consultas de búsqueda que se envían a Bing Image Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 32ced1d06a10f33e9d71ef09ba51d22e9e406f73
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384397"
---
# <a name="send-queries-to-the-bing-image-search-api"></a>Envío de consultas a Bing Image Search API

Bing Image Search API proporciona una experiencia similar a Bing.com/Images. Puede usarla para enviar una consulta de búsqueda a Bing y obtener una lista de imágenes pertinentes.

## <a name="use-and-suggest-search-terms"></a>Uso y sugerencia de los términos de búsqueda

Después de escribir un término de búsqueda, codifique el término en formato de URL antes de establecer el parámetro de consulta [**q**](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query). Por ejemplo, si escribe *sailing dinghies*, establezca `q` en `sailing+dinghies` o `sailing%20dinghies`.

Si la aplicación tiene un cuadro de búsqueda donde se escriben los términos de búsqueda, puede usar [Bing Autosuggest API](../../bing-autosuggest/get-suggested-search-terms.md) para mejorar la experiencia. La API puede mostrar los términos de búsqueda sugeridos en tiempo real. La API devuelve sugerencias de cadenas de consulta basadas en términos de búsqueda parciales y Cognitive Services.

## <a name="pivot-the-query"></a>Dinamización de la consulta

Si Bing puede segmentar la consulta de búsqueda original, el objeto [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) devuelvo contiene el elemento `pivotSuggestions`. Las sugerencias dinámicas se pueden mostrar como términos de búsqueda opcionales para el usuario. Por ejemplo, si la consulta original era *Microsoft Surface*, Bing podría segmentar la consulta en *Microsoft* y *Surface* y proporcionar las sugerencias dinámicas de cada una. Estas se pueden mostrar como términos de consulta opcionales al usuario.

El ejemplo siguiente muestra las sugerencias dinámicas para *Microsoft Surface*:  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

El campo `pivotSuggestions` contiene la lista de segmentos (áreas dinámicas) en los que se ha dividido la consulta original. Para cada área dinámica, la respuesta contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj) que contienen las consultas sugeridas. El campo `text` contiene la consulta sugerida. El campo `displayText` contiene el término que reemplaza el área dinámica en la consulta original. Por ejemplo, Release Date of Surface (Fecha de lanzamiento de Surface).

Si la cadena de consulta dinámica es lo que el usuario busca, use los campos `text` y `thumbnail` para mostrar las cadenas de consulta dinámicas. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o `searchLink`. Use `webSearchUrl` para enviar al usuario los resultados de la búsqueda de Bing. Si proporciona su propia página de resultados, use `searchLink`.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expand-the-query"></a>Expansión de la consulta

Si Bing puede expandir la consulta para restringir la búsqueda original, el objeto [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) contiene el campo `queryExpansions`. Por ejemplo, si la consulta era *Microsoft Surface*, las consultas expandidas podrían ser:
- Microsoft Surface **Pro 3**.
- Microsoft Surface **RT**.
- Microsoft Surface **Phone**.
- Microsoft Surface **Hub**.

El ejemplo siguiente muestra las consultas expandidas para *Microsoft Surface*.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

El campo `queryExpansions` contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query_obj). El campo `text` contiene la consulta expandida. El campo `displayText` contiene el término de la expansión. Si la cadena de consulta expandida es la que el usuario busca, use los campos `text` y `thumbnail` para mostrar las cadenas de consulta expandidas. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o `searchLink`. Use `webSearchUrl` para enviar al usuario los resultados de la búsqueda de Bing. Si proporciona su propia página de resultados, use `searchLink`.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

Si no ha probado Bing Image Search API antes, consulte una [guía de inicio rápido](../quickstarts/csharp.md). Si busca algo más complejo, consulte el tutorial para crear una [aplicación web de una sola página](../tutorial-bing-image-search-single-page-app.md).

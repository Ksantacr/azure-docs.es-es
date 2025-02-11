---
title: 'Idiomas admitidos: Bing News Search API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales, países y regiones admitidos por Bing News Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: d15058126f43fff328acfc563ffd081164a69a90
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384986"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Compatibilidad de idiomas y regiones para Bing News Search API

Bing News Search API admite numerosos países o regiones, muchos de ellos con más de un idioma. La especificación de un país o región en una consulta sirve principalmente para refinar los resultados de la búsqueda en función de los intereses en ese país o región. Además, los resultados pueden contener vínculos a Bing, y estos vínculos pueden localizar la experiencia del usuario de Bing según el país o región especificado o el idioma.

Puede especificar un país o región utilizando el parámetro de consulta `cc`. Si especifica un país o región, también debe especificar uno o más códigos de idioma con el encabezado HTTP `Accept-Language`. Los idiomas admitidos varían según el país o región; se facilitan para cada país en la tabla Mercados.

Como alternativa, puede especificar el mercado con el parámetro de consulta `mkt` y un código de la tabla **Mercados**. La especificación de un mercado especifica al mismo tiempo un país o región y un idioma preferido. El parámetro de consulta `setLang` puede establecerse en un código de idioma en este caso; normalmente se trata del mismo idioma especificado por `mkt` a menos que el usuario prefiera ver Bing en otro idioma.

## <a name="supported-markets-for-news-search-endpoint"></a>Mercados compatibles para el punto de conexión de búsqueda de noticias

En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news/search` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Lenguaje|Código de mercado|  
|---------------------|--------------|-----------------|
|Dinamarca|Danés|da-DK|
|Austria|Alemán|de-AT|
|Suiza|Alemán|de-CH|
|Alemania|Alemán|de-DE|
|Australia|English|en-AU|
|Canadá|English|en-CA|
|Reino Unido|English|en-GB|
|Indonesia|English|en-ID|
|Irlanda|English|en-IE|
|India|English|en-IN|
|Malasia|English|en-MY|
|Nueva Zelanda|English|en-NZ|
|República de Filipinas|English|en-PH|
|Singapur|English|en-SG|
|Estados Unidos|English|en-US|
|English|general|en-WW|
|English|general|en-XA|
|Sudáfrica|English|en-ZA|
|Argentina|Español|es-AR|
|Chile|Español|es-CL|
|España|Español|es-ES|
|México|Español|es-MX|
|Estados Unidos|Español|es-US|
|Español|general|es-XL|
|Finlandia|Finés|fi-FI|  
|Francia|Francés|fr-BE|
|Canadá|Francés|fr-CA|
|Bélgica|Neerlandés|nl-BE|
|Suiza|Francés|fr-CH|
|Francia|Francés|fr-FR|  
|Italia|Italiano|it-IT|
|Hong Kong SAR|Chino tradicional|zh-HK|  
|Taiwán|Chino tradicional|zh-TW|
|Japón|Japonés|ja-JP|  
|Corea|Coreano|ko-KR|  
|Países Bajos|Neerlandés|nl-NL|  
|República Popular China|Chino|zh-CN|  
|Brasil|Portugués|pt-BR|
|Rusia|Ruso|ru-RU|  
|Suecia|Sueco|sv-SE|  
|Turquía|Turco|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Mercados compatibles para el punto de conexión de noticias
En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Lenguaje|Código de mercado|  
|---------------------|--------------|-----------------|
|Dinamarca|Danés|da-DK|
|Alemania|Alemán|de-DE|
|Australia|English|en-AU|
|Reino Unido|English|en-GB|
|Estados Unidos|English|en-US|
|English|general|en-WW|
|Chile|Español|es-CL|
|México|Español|es-MX|
|Estados Unidos|Español|es-US|
|Finlandia|Finés|fi-FI|  
|Canadá|Francés|fr-CA|
|Francia|Francés|fr-FR|  
|Italia|Italiano|it-IT|
|Brasil|Portugués|pt-BR|
|República Popular China|Chino|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Mercados compatibles para el punto de conexión de noticias populares
En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news/trendingtopics` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Lenguaje|Código de mercado|  
|---------------------|--------------|-----------------|
|Alemania|Alemán|de-DE|
|Australia|English|en-AU|
|Reino Unido|English|en-GB|
|Estados Unidos|English|en-US|
|Canadá|English|en-CA|
|India|English|en-IN|
|Francia|Francés|fr-FR|
|Canadá|Francés|fr-CA|
|Brasil|Portugués|pt-BR|
|República Popular China|Chino|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Códigos de país  

Los códigos de país o región siguientes se pueden especificar en el parámetro de consulta `cc`. La lista está sujeta a cambios.  

|País/región|Código de país|  
|---------------------|------------------|  
|Argentina|AR|  
|Australia|AU|  
|Austria|AT|  
|Bélgica|BE|  
|Brasil|BR|  
|Canadá|CA|  
|Chile|CL|  
|Dinamarca|DK|  
|Finlandia|FI|  
|Francia|FR|  
|Alemania|DE|  
|Hong Kong SAR|HK|  
|India|IN|  
|Indonesia|ID|  
|Italia|IT|  
|Japón|JP|  
|Corea|KR|  
|Malasia|MY|  
|México|MX|  
|Países Bajos|NL|  
|Nueva Zelanda|NZ|  
|Noruega|NO|  
|República Popular China|CN|  
|Polonia|PL|  
|Portugal|PT|  
|República de Filipinas|PH|  
|Rusia|RU|  
|Arabia Saudí|SA|  
|Sudáfrica|ZA|  
|España|ES|  
|Suecia|SE|  
|Suiza|CH|  
|Taiwán|TW|  
|Turquía|TR|  
|Reino Unido|GB|  
|Estados Unidos|US|

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los puntos de conexión de Bing News Search, consulte [News Search API v7 reference](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) (Referencia de News Search API v7).

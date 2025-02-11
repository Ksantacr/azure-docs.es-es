---
title: Actualización de Bing Video Search API v5 a v7
titlesuffix: Azure Cognitive Services
description: Identifica las partes de la aplicación que se deben actualizar para usar la versión 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 32dc928147af8fbb3c84bdb76e50cee4fdabe17d
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66386647"
---
# <a name="video-search-api-upgrade-guide"></a>Guía de actualización de Video Search API

Esta guía de actualización identifica los cambios entre las versiones 5 y 7 de Bing Video Search API. Utilice esta guía para identificar las partes de la aplicación que se deben actualizar para usar la versión 7.

## <a name="breaking-changes"></a>Cambios drásticos

### <a name="endpoints"></a>Puntos de conexión

- El número de versión del punto de conexión ha cambiado de v5 a v7. Por ejemplo, `https://api.cognitive.microsoft.com/bing/v7.0/videos/search`.

### <a name="error-response-objects-and-error-codes"></a>Objetos de la respuesta de error y códigos de error

- Todas las solicitudes con error deben incluir ahora un objeto `ErrorResponse` en el cuerpo de la respuesta.

- Se han agregado los campos siguientes al objeto `Error`.  
  - `subCode`&mdash;Divide el código de error en cubos discretos, si es posible.
  - `moreDetails`&mdash;Información adicional sobre el error descrito en el campo `message`.
   

- Se han reemplazado los códigos de error v5 por los posibles valores de `code` y `subCode` siguientes.

|Código|Subcódigo|DESCRIPCIÓN
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing devuelve ServerError cada vez que se produce alguna de las condiciones del subcódigo. La respuesta incluye estos errores si el código de estado HTTP es 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devuelve InvalidRequest siempre que alguna parte de la solicitud no es válida. Por ejemplo, falta un parámetro necesario o un valor de parámetro no es válido.<br/><br/>Si el error es ParameterMissing o ParameterInvalidValue, el código de estado HTTP es 400.<br/><br/>Si el error es HttpNotAllowed, el código de estado HTTP es 410.
|RateLimitExceeded||Bing devuelve RateLimitExceeded cada vez que se supera la cuota de consultas por segundo (QPS) o de consultas por mes (QPM).<br/><br/>Bing devuelve el código de estado HTTP 429 si supera la cuota QPS y 403 si supera la cuota QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devuelve InvalidAuthorization cuando no puede autenticar el autor de la llamada. Por ejemplo, falta el encabezado `Ocp-Apim-Subscription-Key` o la clave de suscripción no es válida.<br/><br/>Se produce redundancia si especifica más de un método de autenticación.<br/><br/>Si el error es InvalidAuthorization, el código de estado HTTP es 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devuelve InsufficientAuthorization cuando el autor de la llamada no tiene permisos para acceder al recurso. Esto puede ocurrir si la clave de suscripción se ha deshabilitado o ha expirado. <br/><br/>Si el error es InsufficientAuthorization, el código de estado HTTP es 403.

- A continuación se asignan los códigos de error anteriores a los nuevos códigos. Si tiene una dependencia de los códigos de error v5, actualice el código en consecuencia.

|Código de la versión 5|Código.subcódigo de la versión 7
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Bloqueado|InvalidRequest.Blocked

### <a name="query-parameters"></a>Parámetros de consulta

- Se ha cambiado el nombre del parámetro de consulta `modulesRequested` por [modules](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#modulesrequested).  

### <a name="object-changes"></a>Cambios en objetos

- Ha cambiado el nombre del campo `nextOffsetAddCount` de [Videos](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) a `nextOffset`. También ha cambiado la forma de usar el desplazamiento. Anteriormente, se establecía el parámetro de consulta [offset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#offset) en el valor `nextOffset` más el valor de desplazamiento anterior más el número de vídeos en el resultado. Ahora, basta con establecer el parámetro de consulta `offset` en el valor `nextOffset`.  
  
- Ha cambiado el tipo de datos del campo `relatedVideos` de `Video[]` a [VideosModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videosmodule) (consulte [VideoDetails](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videodetails)).


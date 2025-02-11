---
title: 'Inicio rápido: Búsqueda de entidades con el SDK de Bing Entity Search para C#'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para buscar entidades con el SDK de Bing Entity Search para C#.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 13ef0734345df17adb2303471b8cb4178f95a2f6
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813743"
---
# <a name="send-a-search-request-with-the-bing-entity-search-sdk-for-c"></a>Envío de una solicitud de búsqueda el SDK de Bing Entity Search para C#

Use este inicio rápido para empezar a buscar entidades con el SDK de Bing Entity Search para C#. Aunque Bing Entity Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch).


## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017 o posterior](https://www.visualstudio.com/downloads/).
* El marco [Json.NET](https://www.newtonsoft.com/json), disponible como un paquete NuGet.
* Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](https://www.mono-project.com/).
* El [paquete NuGet del SDK de Bing News Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). La instalación de este paquete también instala lo siguiente:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Para añadir el SDK de Bing Entity Search al proyecto de Visual Studio, use la opción **Administración de paquetes de NuGet** del **Explorador de soluciones** y agregue el paquete `Microsoft.Azure.CognitiveServices.Search.EntitySearch`.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Creación e inicialización de una aplicación

1. Cree una solución de consola de C# en Visual Studio. Después, agregue lo siguiente en el archivo de código principal.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>Creación de un cliente y envío de una solicitud de búsqueda

1. Cree un cliente de búsqueda. Agregue su clave de suscripción mediante la creación de un nuevo `ApiKeyServiceClientCredentials`.

    ```csharp
    var client = new EntitySearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. Use la función `Entities.Search()` del cliente para buscar la consulta:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Obtención e impresión de una descripción de entidad

1. Si la API devolvió los resultados de la búsqueda, obtenga la entidad principal de `entityData`.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Impresión de la descripción de la entidad principal 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorial-bing-entities-search-single-page-app.md)

* [¿Qué es Bing Entity Search API?](../overview.md )
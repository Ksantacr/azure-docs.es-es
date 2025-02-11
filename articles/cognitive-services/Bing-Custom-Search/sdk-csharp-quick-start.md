---
title: 'Inicio rápido: Llamada al punto de conexión de Bing Custom Search con el SDK para C# | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Custom Search para C#.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 9e13edce77819d5ef8cfc3b6becff9fb82224a83
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595963"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-c-sdk"></a>Inicio rápido: Llamada al punto de conexión de Bing Custom Search con el SDK para C# 

Use este documento de inicio rápido para comenzar a solicitar los resultados de búsqueda de la instancia de Bing Custom Search con el SDK para C#. Aunque Bing Custom Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK de Bing Custom Search proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de Bing Custom Search. Consulte [Quickstart: Creación de la primera instancia de Bing Custom Search](quick-start.md) para obtener más información.
- Microsoft [.NET Core](https://www.microsoft.com/net/download/core)
- Cualquier edición de [Visual Studio 2017 o versiones posteriores](https://www.visualstudio.com/downloads/).
- Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](https://www.mono-project.com/).
- El paquete [NuGet Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) instalado. 
    - En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet** en el menú. Instale el paquete `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. Al instalar el paquete NuGet Custom Search, también se instalarán los ensamblados siguientes:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-news-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree una aplicación de consola en C# mediante Visual Studio. Agregue los siguientes paquetes al proyecto.

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
    ```

2. En el método Main de la aplicación, cree una instancia del cliente de búsqueda con la clave de API.

    ```csharp
    var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));
    ```

## <a name="send-the-search-request-and-receive-a-response"></a>Envío de la solicitud de búsqueda y recepción de la respuesta
    
1. Envíe una consulta de búsqueda con el método `SearchAsync()` del cliente y guarde la respuesta. No olvide reemplazar `YOUR-CUSTOM-CONFIG-ID` por el identificador de configuración de la instancia (encontrará el identificador en el [portal de Bing Custom Search](https://www.customsearch.ai/)). En este ejemplo se busca "Xbox".

    ```csharp
    // This will look up a single query (Xbox).
    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
    ```

2. El método `SearchAsync()` devuelve un objeto `WebData`. Use el objeto para iterar por cualquier `WebPages` que se encuentre. Este código busca el primer resultado de página web e imprime los valores de `Name` y `URL` de la página web.

    ```csharp
    if (webData?.WebPages?.Value?.Count > 0)
    {
        // find the first web page
        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

        if (firstWebPagesResult != null)
        {
            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
            Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
            Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
        }
        else
        {
            Console.WriteLine("Couldn't find web results!");
        }
    }
    else
    {
        Console.WriteLine("Didn't see any Web data..");
    }
    ```csharp

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](./tutorials/custom-search-web-page.md)

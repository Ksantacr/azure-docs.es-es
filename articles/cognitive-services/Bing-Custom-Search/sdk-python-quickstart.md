---
title: 'Inicio rápido: Llamada al punto de conexión de Bing Custom Search con el SDK para Python | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Use el SDK de Bing Custom Search para Python a fin de obtener resultados de búsquedas personalizadas.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: aahi
ms.openlocfilehash: c4c5059bc57ea33357145f6b119456dc6c5bdb7b
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571822"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Inicio rápido: Llamada al punto de conexión de Bing Custom Search con el SDK para Python 

Use este inicio rápido para comenzar a solicitar los resultados de la búsqueda de la instancia de Bing Custom Search con el SDK para Python. Aunque Bing Custom Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK de Bing Custom Search proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) con anotaciones y control de errores adicionales.

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de Bing Custom Search. Consulte [Quickstart: Creación de la primera instancia de Bing Custom Search](quick-start.md) para obtener más información.
- Python [2.x o 3.x](https://www.python.org/) 
- [SDK de Bing Custom Search para Python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>Instalación del SDK para Python

Instale el SDK de Bing Custom Search con el siguiente comando.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Creación de una aplicación

Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes importaciones.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Creación de un cliente de búsqueda y envío de una solicitud

1. Cree una variable para la clave de suscripción.

    ```python
    subscription_key = 'your-subscription-key'
    ```

2. Cree una instancia de `CustomSearchClient` con un objeto `CognitiveServicesCredentials` mediante la clave de suscripción. 

    ```python
    client = CustomSearchClient(CognitiveServicesCredentials(subscription_key))
    ```

3. Envíe una solicitud de búsqueda con `client.custom_instance.search()`. Anexe el término de búsqueda al parámetro `query` y establezca `custom_config` en el identificador de configuración personalizada para usar la instancia de búsqueda. Puede obtener el identificador en el [portal de Bing Custom Search](https://www.customsearch.ai/), donde debe hacer clic en la pestaña **Producción**.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Visualización de los resultados de la búsqueda

Si se encuentran algunos resultados de la búsqueda de páginas web, obtenga el primero e imprima su nombre, la dirección URL y las páginas web totales encontradas.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una página web de Custom Search](./tutorials/custom-search-web-page.md)

---
title: 'Guía de inicio rápido: Creación de la base de conocimiento: REST y Python: QnA Maker'
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido basada en REST para Python le lleva por la creación de una base de conocimiento de QnA Maker de ejemplo mediante programación, que aparecerá en el panel de Azure de su cuenta de la API de Cognitive Services.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 9aa5b85a00de495478897ec70393f6c22ee20fc5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788748"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-python"></a>Guía de inicio rápido: Creación de una base de conocimiento en QnA Maker mediante Python

Esta guía de inicio rápido le lleva por la creación y la publicación de una base de conocimiento de QnA Maker de ejemplo mediante programación. QnA Maker extrae automáticamente preguntas y respuestas a partir de contenido semiestructurado, como las preguntas frecuentes, y de [orígenes de datos](../Concepts/data-sources-supported.md). El modelo para la base de conocimiento se define en el JSON que se envía en el cuerpo de la solicitud de API. 

En esta guía de inicio rápido se llama a las siguientes API de QnA Maker:
* [Creación de una base de conocimiento](https://go.microsoft.com/fwlink/?linkid=2092179)
* [Obtención de los detalles de la operación](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

## <a name="prerequisites"></a>Requisitos previos

* [Python 3.7](https://www.python.org/downloads/)
* Debe tener un servicio QnA Maker. Para recuperar la clave, seleccione Claves en Administración de recursos en el panel.

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-python-repo-note.md)]

## <a name="create-a-knowledge-base-python-file"></a>Creación de un archivo de Python de base de conocimiento

Cree un archivo llamado `create-new-knowledge-base-3x.py`.

## <a name="add-the-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior de `create-new-knowledge-base-3x.py`, agregue las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Incorporación de las constantes necesarias
Después de las dependencias necesarias anteriores, agregue las constantes necesarias para acceder a QnA Maker. Sustituya el valor de la variable `subscriptionKey` por su propia clave de QnA Maker.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=5-13 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>Incorporación de la definición del modelo de la base de conocimiento

Después de las constantes, agregue la siguiente definición de la base de conocimiento. El modelo se convertirá en una cadena después de la definición.

[!code-python[Add the KB model definition](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=15-41 "Add the KB model definition")]

## <a name="add-supporting-function"></a>Incorporación de funciones auxiliares

Agregue la siguiente función para imprimir el código JSON en un formato legible:

[!code-python[Add supporting function](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=43-45 "Add supporting function")]

## <a name="add-function-to-create-kb"></a>Incorporación de la función para crear una base de conocimiento

Agregue la siguiente función para realizar una solicitud HTTP POST a fin de crear la base de conocimiento. Esta llamada API devuelve una respuesta JSON que incluye el identificador de operación en el campo de encabezado **Location**. Use el identificador de operación para determinar si se ha creado correctamente la base de conocimiento. `Ocp-Apim-Subscription-Key` es la clave del servicio QnA Maker, usada para la autenticación. 

[!code-python[Add function to create KB](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=48-59 "Add function to create KB")]

Esta llamada API devuelve una respuesta JSON que incluye el identificador de operación. Use el identificador de operación para determinar si se ha creado correctamente la base de conocimiento. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-check-creation-status"></a>Incorporación de una función para comprobar el estado de creación

La siguiente función comprueba el envío del estado de creación en el identificador de operación al final de la ruta de dirección URL. La llamada a `check_status` está dentro del bucle _while_ principal.

[!code-python[Add function to check creation status](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=61-67 "Add function to check creation status")]

Esta llamada API devuelve una respuesta JSON que incluye el estado de la operación: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Repita la llamada hasta obtener éxito o error: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-main-code-block"></a>Incorporación del bloque de código principal
El siguiente bucle sondea el estado de la operación de creación periódicamente hasta que se completa. 

[!code-python[Add main code block](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=70-96 "Add main code block")]

## <a name="build-and-run-the-program"></a>Compilar y ejecutar el programa

Escriba el siguiente comando en una línea de comandos para ejecutar el programa. La solicitud se envía a QnA Maker API para crear la base de conocimiento y, luego, se sondean los resultados cada 30 segundos. Cada respuesta se imprime en la ventana de consola.

```bash
python create-new-knowledge-base-3x.py
```

Una vez creada la base de conocimiento, puede verla en el portal de QnA Maker en la página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Mis bases de conocimiento). Seleccione el nombre de la base de conocimiento, por ejemplo Preguntas más frecuentes de QnA Maker, para verla.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://go.microsoft.com/fwlink/?linkid=2092179)

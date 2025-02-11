---
title: ¿Qué es Ink Recognizer? API Ink Recognizer
titlesuffix: Azure Cognitive Services
description: Integre Ink Recognizer en las aplicaciones, sitios web, herramientas y otras soluciones para proporcionar...
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 0ed1a72a5dc61458200b72c768ad722656b820d8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65025609"
---
# <a name="what-is-the-ink-recognizer-api"></a>¿Qué es la API Ink Recognizer?


Ink Recognizer Cognitive Service proporciona una API REST en la nube que analiza y reconoce entradas de lápiz digital. A diferencia de los servicios que utilizan el reconocimiento óptico de caracteres (OCR), la API requiere datos de los trazos de lápiz digital como entrada. Los trazos de lápiz digital son conjuntos de puntos 2D (coordenadas X e Y) ordenados por tiempo que representan el movimiento de las herramientas de entrada, como lápices digitales o los dedos. Luego, reconoce las formas y el contenido manuscrito en la entrada y devuelve una respuesta JSON que contiene todas las entidades reconocidas.

![Diagrama de flujo que describe el envío de una entrada de lápiz a la API](media/ink-recognizer-pen-graph.png)

## <a name="features"></a>Características

Con la API Ink Recognizer, puede reconocer fácilmente el contenido manuscrito en las aplicaciones. 

|Característica  |DESCRIPCIÓN  |
|---------|---------|
| Reconocimiento de escritura a mano | Reconozca el contenido manuscrito en 63 [idiomas y configuraciones regionales](language-support.md). | 
| Reconocimiento de diseños | Obtenga información estructural acerca del contenido de las entradas de lápiz digital. Divida el contenido en la distintas regiones de escritura, párrafos, líneas, palabras o listas con viñetas. Luego, las aplicaciones pueden usar la información del diseño para crear características adicionales, como el formato automático de listas y la alineación de formas. |
| Reconocimiento de formas | Reconozca las [formas geométricas](concepts/send-ink-data.md#shapes-recognized-by-the-ink-recognizer-api) más usadas al tomar notas. |
| Reconocimiento de la combinación de texto y formas | Reconozca qué trazos de lápiz pertenecen a las formas o al contenido manuscrito y clasifíquelos por separado.|

## <a name="workflow"></a>Flujo de trabajo

La API Ink Recognizer es un servicio web RESTful, lo que significa que es fácil llamarlo desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

Después del registro:

1. Tome los datos de los trazos de lápiz y [asígneles un formato](concepts/send-ink-data.md#sending-ink-data) JSON válido.
1. Envíe una solicitud a la API Ink Recognizer con los datos.
1. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="next-steps"></a>Pasos siguientes

Pruebe un inicio rápido en los siguientes idiomas para empezar a realizar llamadas a API Ink Recognizer.
* [C#](quickstarts/csharp.md)
* [Java](quickstarts/java.md)
* [JavaScript](quickstarts/csharp.md)

Para ver el funcionamiento de la API Ink Recognition en una aplicación de entrada de lápiz digital, eche un vistazo a las siguientes aplicaciones de ejemplo en GitHub:
* [C# y Plataforma universal de Windows (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# y Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Aplicación para explorador web de JavaScript](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Aplicación móvil Java y Android](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Aplicación móvil iOS y SWIFT](https://go.microsoft.com/fwlink/?linkid=2089805)

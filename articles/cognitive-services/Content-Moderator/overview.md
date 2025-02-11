---
title: ¿Qué es Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Content Moderator para realizar el seguimiento, marcar, evaluar y filtrar material inapropiado en el contenido creado por los usuarios.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 7e9c12c7da701fb627c51373e57f870d3fc77ac5
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/13/2019
ms.locfileid: "65551310"
---
# <a name="what-is-azure-content-moderator"></a>¿Qué es Azure Content Moderator?

Azure Content Moderator API es un servicio Cognitive que comprueba el contenido de texto, imágenes y vídeo de material que es potencialmente ofensivo, arriesgado o no deseable por algún otro motivo. Cuando se encuentra dicho material, el servicio aplica las etiquetas adecuadas (marcas) al contenido. Luego, la aplicación puede controlar el contenido marcado para cumplir las normativas o mantener el entorno adecuado para los usuarios. Consulte la sección [API de moderación](#moderation-apis) para más información sobre lo que indican las diferentes marcas de contenido.

## <a name="where-it-is-used"></a>Dónde se usa

Los siguientes son algunos escenarios en los que un desarrollador de software o un equipo de desarrolladores usaría Content Moderator:

- Instancias de Marketplace en línea que moderan catálogos de productos y otro contenido generado por el usuario.
- Empresas de juegos que moderan los artefactos de juego generados por el usuario y las salas de chat.
- Plataformas de mensajería de las redes sociales que moderan las imágenes, el texto y los vídeos que agregan los usuarios.
- Empresas multimedia que implementan la moderación de centralizada de su contenido.
- Proveedores de soluciones educativas de tipo K-12 que filtran contenido que no es apropiado para alumnos y educadores.

## <a name="what-it-includes"></a>Qué incluye

El servicio Content Moderator consta de varias API de servicio de web disponibles mediante llamadas REST y un SDK de .NET. También incluye la herramienta de revisión humana, que permite a los revisores humanos ayudar el servicio y mejorar o ajustar su función de moderación.

## <a name="moderation-apis"></a>API de moderación

El servicio Content Moderator incluye las API de moderación, que comprueban el contenido en busca de material que sea potencialmente inadecuado o inapropiado.

![Diagrama de bloques de las API de moderación de Content Moderator](images/content-moderator-mod-api.png)

En la tabla siguiente se describen los distintos tipos de API de moderación.

| Grupo de API | DESCRIPCIÓN |
| ------ | ----------- |
|[**Moderación de texto**](text-moderation-api.md)| Examina si el texto contiene contenido ofensivo, sexualmente explícito o sugerente, blasfemias y datos personales.|
|[**Listas de términos personalizada**](try-terms-list-api.md)| Analiza el texto con respecto a una lista personalizada de términos, además de los términos integrados. Use listas personalizadas para bloquear o permitir el contenido en función de sus propias directivas de contenido.|  
|[**Moderación de imágenes**](image-moderation-api.md)| Analiza imágenes en busca de contenido para adultos o inapropiado, detecta texto en imágenes con la funcionalidad de reconocimiento óptico de caracteres (OCR) y detecta rostros.|
|[**Listas de imágenes personalizadas**](try-image-list-api.md)| Analiza imágenes con una lista personalizada de imágenes. Use las listas de imágenes personalizadas para filtrar el contenido comúnmente recurrente que no desea volver a clasificar.|
|[**Moderación en vídeos**](video-moderation-api.md)| Examina si los vídeos tienen contenido para adultos o inapropiado y devuelve los marcadores de tiempo de dicho contenido.|

## <a name="review-apis"></a>Revisión de las API

Las API de revisión le permiten integrar la canalización de moderación con revisores humanos. Use las operaciones [Jobs](review-api.md#jobs) (Trabajos), [Reviews](review-api.md#reviews) (Revisiones) y [Workflow](review-api.md#workflows) (Flujo de trabajo) para crear y automatizar los flujos de trabajo de revisión humana en bucle con la [herramienta de revisión](#the-review-tool) (a continuación).

> [!NOTE]
> La API de flujo de trabajo aún no está disponible en .NET SDK pero se puede usar con el punto de conexión REST.

![Diagrama de bloques de las API de revisión de Content Moderator](images/content-moderator-rev-api.png)

## <a name="the-review-tool"></a>Herramienta de revisión

El servicio Content Moderator también incluye la [herramienta de revisión](Review-Tool-User-Guide/human-in-the-loop.md) basada en web, que hospeda las revisiones del contenido para su procesamiento por parte de moderadores humanos. La entrada humana no entrena el servicio, pero el trabajo combinado del servicio y los equipos de revisión humana permite a los desarrolladores lograr un equilibrio adecuado entre eficacia y precisión. La herramienta de revisión también proporciona un servidor front-end sencillo para diversos recursos de Content Moderator.

![Página principal de la herramienta de revisión humana de Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Content Moderator deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Comience a usar el servicio Content Moderator siguiendo las instrucciones de [Try Content Moderator on the web](quick-start.md) (Probar Content Moderator en la web).
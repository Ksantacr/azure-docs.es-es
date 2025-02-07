---
title: Creación de clips con Azure Media Clipper | Microsoft Docs
description: Información general de Azure Media Clipper, una herramienta para crear clips multimedia a partir de recursos
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 35f1f359b44af00000ccd9047673b80ca541d376
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61243880"
---
# <a name="create-clips-with-azure-media-clipper"></a>Creación de clips con Azure Media Clipper 

Azure Media Clipper es una biblioteca gratuita de JavaScript que permite que los desarrolladores web proporcionen a sus usuarios una interfaz para crear clips multimedia. Esta herramienta se puede integrar en cualquier página web y proporciona API para cargar activos y enviar trabajos de creación de clips.

Azure Media Clipper permite:
- Recortar la careta previa y la careta posterior de los archivos activos 
- Crear vídeos destacados a partir de eventos en directo AMS, archivos activos o archivos VOD fMP4 
- Concatenar vídeos de distintos orígenes 
- Crear clips resumidos a partir de los activos multimedia AMS 
- Recortar vídeos con precisión de fotograma 
- Generar filtros de manifiesto dinámico sobre activos VOD o en directo existentes con precisión de grupo de imágenes (GOP) 
- Generar trabajos de codificación respecto de los activos en la cuenta de Media Services

Para solicitar características nuevas, proporcionar ideas o comentarios, envíelos a [UserVoice para Azure Media Services](https://aka.ms/amsvoice/). Si tiene problemas específicos, preguntas o encontró algún error, envíe un correo electrónico al equipo de Media Services a amcinfo@microsoft.com.

En la imagen siguiente se ilustra la interfaz de Clipper: ![Azure Media Clipper](media/media-services-azure-media-clipper-overview/media-services-azure-media-clipper-interface.PNG)

## <a name="release-notes"></a>Notas de la versión
Consulte la lista siguiente para ver la entrada de blog de Clipper, diversos problemas conocidos y el registro de cambios de la versión más reciente de Clipper:
- [Entrada de blog](https://azure.microsoft.com/blog/azure-media-clipper/)
- [Lista de problemas conocidos](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Registro de cambios](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Compatibilidad con exploradores
Azure Media Clipper se compila con las modernas tecnologías de HTML5 y admite los exploradores siguientes:

- Microsoft Edge 13+
- Internet Explorer 11+
- Chrome 54+
- Safari 10+
- Firefox 50+

> [!NOTE]
> Actualmente, solo se soporta la reproducción HTML5 de las transmisiones de Azure Media Services.

## <a name="language-support"></a>Compatibilidad con idiomas
El widget de Clipper está disponible en estos 18 idiomas:
- Chino (simplificado)
- Chino (tradicional)
- Checo
- Neerlandés, flamenco
- English
- Francés
- Alemán
- Húngaro
- Italiano
- Japonés
- Coreano
- Polaco
- Portugués (Brasil)
- Portugués (Portugal)
- Ruso
- Español
- Sueco
- Turco

## <a name="next-steps"></a>Pasos siguientes
Para comenzar a usar Azure Media Clipper, ea el artículo de [introducción](media-services-azure-media-clipper-getting-started.md) para detalles sobre cómo implementar el widget.

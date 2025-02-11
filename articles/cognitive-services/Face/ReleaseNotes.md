---
title: 'Notas de la versión: servicio Face API'
titleSuffix: Azure Cognitive Services
description: Las notas de la versión del servicio Face API incluyen un historial de cambios de versión para varias versiones.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: yluiu
ms.openlocfilehash: 63f705061db7c89a80578e741d02ab8f8b1041c2
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917097"
---
# <a name="face-api-release-notes"></a>Notas de versión de Face API

Este artículo pertenece a la versión 1.0 del servicio Face API.

### <a name="release-changes-in-april-2019"></a>Cambios de la versión de abril de 2019

* Mejora la precisión general de la `age` y `headPose` atributos. El `headPose` atributo también se actualiza con el `pitch` valor habilitada ahora. Usar estos atributos especificándolas en el `returnFaceAttributes` parámetro de [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parámetro. 

* Mejora la velocidad de [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [persona del grupo de personas - agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) y [ Persona LargePersonGroup - agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2019"></a>Cambios de la versión de marzo de 2019

* Agrega un nuevo modelo de reconocimiento de caras con una mayor precisión. Usar a través [enfrentan: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - crear](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [LargeFaceList - crear](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [grupo de personas - crear](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) y [ LargePersonGroup - crear](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) especificando el nuevo nombre de modelo de reconocimiento de caras `recognition_02` en `recognitionModel` parámetro. Para obtener más detalles en [cómo especificar un modelo de reconocimiento](Face-API-How-to-Topics/specify-recognition-model.md).

### <a name="release-changes-in-january-2019"></a>Cambios de la versión de enero de 2019

* Se ha agregado la característica de instantánea para admitir la migración de datos entre suscripciones: [Instantánea](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). Para obtener más detalles en [cómo migrar los datos de cara a una suscripción diferente de la cara](Face-API-How-to-Topics/how-to-migrate-face-data.md).

### <a name="release-changes-in-october-2018"></a>Cambios de la versión de octubre de 2018

* Se ha refinado la descripción de `status`, `createdDateTime`, `lastActionDateTime` y `lastSuccessfulTrainingDateTime` en [PersonGroup: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [LargePersonGroup: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5) y [LargeFaceList: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf).

### <a name="release-changes-in-may-2018"></a>Cambios de la versión de mayo de 2018

* Se mejoró significativamente el atributo `gender` y también se mejoraron los atributos `age`, `glasses`, `facialHair`, `hair` y `makeup`. Puede usarlos mediante el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

* Se ha aumentado el límite de tamaño del archivo de imagen de entrada de 4 MB a 6 MB en [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) y [Persona de LargePersonGroup: agregar cara ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Cambios de la versión de marzo de 2018

* Se ha agregado el contenedor con escala de millón: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) y [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Puede obtener más detalles en [How to use the large-scale feature](Face-API-How-to-Topics/how-to-use-large-scale.md) (Cómo usar la característica a gran escala).

* Se aumentó el parámetro `maxNumOfCandidatesReturned` de [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) de [1, 5] to [1, 100], y el valor predeterminado a 10.

### <a name="release-changes-in-may-2017"></a>Cambios de la versión de mayo de 2017

* Se agregaron los atributos `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure` y `noise` atributos en el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

* Se admiten 10 mil personas en un elemento PersonGroup y en [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Se admite la paginación en [Persona de PersonGroup: lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) con los parámetros opcionales: `start` y `top`.

* Simultaneidad compatible para agregar y eliminar caras de FaceLists diferentes y distintas personas en PersonGroup.

### <a name="release-changes-in-march-2017"></a>Cambios de la versión de marzo de 2017
* Se agregó el atributo `emotion` atributos en el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

* Se corrigió que la cara no se pudiera volver a detectar con el rectángulo devuelto desde [Face: detectar ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como `targetFace` en [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) y [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Se corrigió el tamaño de la cara detectable para garantizar que esté estrictamente entre 36 x 36 y 4096 x 4096 píxeles.

### <a name="release-changes-in-november-2016"></a>Cambios de versión de noviembre de 2016
* Se agregó una suscripción a la versión estándar de Almacenamiento de caras, para almacenar caras persistentes adicionales al usar [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) o [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) para la identificación o coincidencia de similitudes. Las imágenes almacenadas se cobran a 0,5 USD por 1000 caras y esta tarifa se prorratea por días. Las suscripciones del nivel Gratis siguen estando limitadas a un total de 1000 personas.

### <a name="release-changes-in-october-2016"></a>Cambios de la versión de octubre de 2016
* Se modificó el mensaje de error de más de una cara en targetFace de "Hay más caras en la imagen" a "Hay más de una cara en la imagen" en [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) y [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Cambios de la versión de julio de 2016
* Se admite la autenticación del objeto Face to Person en [Face:comprobar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Se agregó un parámetro `mode` opcional que permite seleccionar dos modos de trabajo: `matchPerson` y `matchFace` en [Face: buscar similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), y el valor predeterminado es `matchPerson`.

* Se agregó un parámetro `confidenceThreshold` opcional para que el usuario establezca el umbral que indicará si una cara pertenece a un objeto Person en [Face: identificar ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Se agregaron los parámetros `start` y `top` opcionales en [PersonGroup: lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) para permitir que el usuario especifique el punto inicial y el número total de elementos PersonGroups que se van a enumerar.

### <a name="v10-changes-from-v0"></a>Cambios en V1.0 desde la versión V0
* Se actualizó el punto de conexión raíz del servicio de ```https://westus.api.cognitive.microsoft.com/face/v0/``` a ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Cambios aplicados a: [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face:buscar similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) y [Face: grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Se actualizó el tamaño mínimo detectable de la cara a 36 x 36 píxeles. No se detectarán caras que tengan menos de 36 x 36 píxeles.

* Los datos de PersonGroup y Person en Face V0 están en desuso. No se puede acceder a esos datos con el servicio Face V1.0.

* El punto de conexión V0 de Face API quedó en desuso el 30 de junio de 2016.

---
title: Filtrado de blasfemias de Translator Text API
titlesuffix: Azure Cognitive Services
description: Use el filtrado de blasfemias de Translator Text API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-pawal
ms.openlocfilehash: 3ffe14450f0191b88a8d8a71b5f28ae4c59b68c1
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66387580"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Agregar el filtrado de blasfemias con Translator Text API

Normalmente, el servicio de Translator conserva las blasfemias que están presentes en el origen de la traducción. El grado de blasfemia y el contexto que hace que las palabras sean soeces difieren entre las distintas culturas. Como consecuencia, el grado de blasfemia en el lenguaje de destino podría ampliarse o reducirse.

Si quiere evitar obtener blasfemias en la traducción (independientemente de su presencia en el texto de origen), use la opción de filtrado de blasfemias disponible en el método Translate(). Esta opción le permite elegir si quiere ver las palabras soeces eliminadas o marcadas con etiquetas adecuadas, o bien si no quiere realizar ninguna acción.

El método Translate() toma el parámetro "options", que contiene el nuevo elemento "ProfanityAction". Los valores aceptados de ProfanityAction son "NoAction", "Marked" y "Deleted".

## <a name="accepted-values-of-profanityaction-and-examples"></a>Valores aceptados de ProfanityAction y ejemplos
|Valor de ProfanityAction | . | Ejemplo: Origen: japonés | Ejemplo: Destino: inglés|
| :---|:---|:---|:---|
| NoAction | Predeterminada. Igual que si no se configura la opción. Las blasfemias pasan del origen al destino. | 彼は変態です。 | Es un estúpido. |
| Marked | Las palabras soeces aparecerán rodeadas por etiquetas XML \<profanity> ... \</profanity>. | 彼は変態です。 | Es un \<profanity>estúpido\</profanity>. |
| Deleted | Las palabras soeces se quitan de la salida sin reemplazo. | 彼は。 | Es un. |

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Aplicar filtrado de blasfemias con la llamada a Translator API](reference/v3-0-translate.md)

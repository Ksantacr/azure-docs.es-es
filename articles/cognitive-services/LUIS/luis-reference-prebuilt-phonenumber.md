---
title: Entidades precompiladas Phonenumber
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de número de teléfono en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 43d0b855c25ed10b074d99b247ee56dc2ba7769b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146196"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>Entidad creada previamente número de teléfono para una aplicación de LUIS
La entidad `phonenumber` extrae una variedad de números de teléfono, incluido el código de país. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que a la aplicación. La entidad `phonenumber` se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-a-phone-number"></a>Tipos de un número de teléfono
`Phonenumber` se administra desde el [reconocedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) repositorio de GitHub

## <a name="resolution-for-this-prebuilt-entity"></a>Resolución para esta entidad creados previamente

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.phonenumber**.

```json
{
  "query": "my mobile is 1 (800) 642-7676",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versión de API preliminar 3.x

El siguiente JSON es con el `verbose` parámetro establecido en `false`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ]
        }
    }
}
```

El siguiente JSON es con el `verbose` parámetro establecido en `true`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ],
            "$instance": {
                "phonenumber": [
                    {
                        "type": "builtin.phonenumber",
                        "text": "1 (800) 642-7676",
                        "startIndex": 13,
                        "length": 16,
                        "score": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [porcentaje](luis-reference-prebuilt-percentage.md), [número de teléfono](luis-reference-prebuilt-number.md) y [temperatura](luis-reference-prebuilt-temperature.md). 

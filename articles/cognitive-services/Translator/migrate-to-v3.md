---
title: Migración a V3 Translator Text API
titlesuffix: Azure Cognitive Services
description: Aprenda a migrar de la versión V2 a la versión V3 de Translator Text API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-pawal
ms.openlocfilehash: 81b2e5c9c659a3811d7417d87b811a86f4350a52
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382929"
---
# <a name="translator-text-api-v2-to-v3-migration"></a>Migrar Translator Text API V2 a V3

> [!NOTE]
> Ha quedado en desuso v2 en el 30 de abril de 2018. Migre sus aplicaciones a V3 con el fin de aprovechar las ventajas de la nueva funcionalidad disponible exclusivamente en V3.
> 
> El centro de Microsoft Translator se retirará de 17 de mayo de 2019. [Ver las fechas y la información de migración importante](https://www.microsoft.com/translator/business/hub/).  

El equipo de Microsoft Translator ha lanzado la versión 3 (V3) de Translator Text API. En esta versión se incluyen nuevas características, métodos en desuso y un nuevo formato para enviar y recibir datos del servicio Microsoft Translator. Este documento proporciona información para cambiar las aplicaciones para que usen V3. 

El final de este documento contiene vínculos útiles para que pueda obtener más información.

## <a name="summary-of-features"></a>Resumen de características

* Sin seguimiento: en V3 se aplica la función Sin seguimiento a todos los niveles de precios en Azure Portal. Esta característica significa que Microsoft no guardará ningún texto enviado a la API V3.
* JSON: XML se reemplaza con JSON. Todos los datos enviados al servicio y recibidos desde el mismo están en formato JSON.
* Varios idiomas de destino en una única solicitud: el método de traducción acepta varios idiomas de destino ("a") para la traducción en una única solicitud. Por ejemplo, una sola solicitud puede traducirse "desde" el inglés "al" alemán, español y japonés, o a cualquier otro grupo de idiomas.
* Diccionario bilingüe: se ha agregado un método de diccionario bilingüe a la API. Este método incluye las opciones "búsqueda" y "ejemplos".
* Transliterar: se ha agregado un método de transliteración a la API. Este método convertirá las palabras y oraciones de un script (por ejemplo, en árabe) en otro script (por ejemplo, en latín).
* Idiomas: un nuevo método denominado "idiomas" ofrece información sobre el idioma en formato JSON, y se puede usar con los métodos "traducir", "diccionario" y "transliterar".
* Novedades en Traducir: se han agregado nuevas capacidades al método "traducir" para admitir algunas de las características que se encontraban en la API V2 como métodos separados. Un ejemplo es TranslateArray.
* Método leer: la funcionalidad de conversión de texto a voz ya no se admite en la API de Microsoft Translator. La funcionalidad de texto a voz está disponible en el [servicio Voz de Microsoft](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech).

La siguiente lista de métodos V2 y V3 identifica los métodos V3 y las API que proporcionarán la funcionalidad de V2.

| Método de API V2   | Compatibilidad de API V3 |
|:----------- |:-------------|
| `Translate`     | [Traducir](reference/v3-0-translate.md)          |
| `TranslateArray`      | [Traducir](reference/v3-0-translate.md)        |
| `GetLanguageNames`      | [Idiomas](reference/v3-0-languages.md)         |
| `GetLanguagesForTranslate`     | [Idiomas](reference/v3-0-languages.md)       |
| `GetLanguagesForSpeak`      | [Servicio Voz de Microsoft](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech)         |
| `Speak`     | [Servicio Voz de Microsoft](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech)          |
| `Detect`     | [Detectar](reference/v3-0-detect.md)         |
| `DetectArray`     | [Detectar](reference/v3-0-detect.md)         |
| `AddTranslation`     | Esta característica ya no se admite.       |
| `AddTranslationArray`    | Esta característica ya no se admite.          |
| `BreakSentences`      | [BreakSentence](reference/v3-0-break-sentence.md)       |
| `GetTranslations`      | Esta característica ya no se admite.         |
| `GetTranslationsArray`      | Esta característica ya no se admite.         |

## <a name="move-to-json-format"></a>Cambiar al formato JSON

Microsoft Translator Text Translation V2 aceptaba y devolvía datos en formato XML. En V3, todos los datos que se envían y se reciben mediante la API están en formato JSON. XML ya no se aceptará ni devolverá datos en V3.

Este cambio afectará a varios aspectos de una aplicación escrita para la versión V2 de Text Translation API. Por ejemplo: la API de idiomas devuelve información sobre el idioma que se usará en la traducción de texto, la transliteración y los dos métodos de diccionario. Puede solicitar toda la información del idioma para todos los métodos en una sola llamada o solicitarlos individualmente.

El método de idiomas no requiere autenticación; si hace clic en el siguiente vínculo podrá ver toda la información de idioma para V3 en JSON:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, diccionario, transliteración](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Clave de autenticación

La clave de autenticación que está utilizando para V2 se aceptará en V3. No necesita obtener una nueva suscripción. Podrá mezclar V2 y V3 en sus aplicaciones durante el período de migración de un año; así será más fácil lanzar de nuevas versiones mientras todavía está migrando de V2-XML a V3-JSON.

## <a name="pricing-model"></a>Modelo de precios

Microsoft Translator V3 tiene el mismo precio que V2; esto es, por carácter e incluyendo los espacios. Las nuevas características de V3 realizan algunos cambios en los caracteres que se cuentan para la facturación.

| Método V3   | Caracteres que se cuentan para la facturación |
|:----------- |:-------------|
| `Languages`     | Si no se envían caracteres, no se cuenta ninguno y no hay cargo.          |
| `Translate`     | El recuento se basa en la cantidad de caracteres que se envían para la traducción, y en el número de idiomas a los que se traducen los caracteres. 50 caracteres enviados más 5 idiomas solicitados, serán 50x5.           |
| `Transliterate`     | Se cuenta el número de caracteres que se piden para la transliteración.         |
| `Dictionary lookup & example`     | Se cuentan el número de caracteres enviados para la búsqueda de diccionario y los ejemplos.         |
| `BreakSentence`     | Sin cargo.       |
| `Detect`     | Sin cargo.      |

## <a name="v3-end-points"></a>Puntos de conexión de V3

Global

* api.cognitive.microsofttranslator.com

## <a name="v3-api-text-translations-methods"></a>Métodos de traducción de texto de API V3

[`Languages`](reference/v3-0-languages.md)

[`Translate`](reference/v3-0-translate.md)

[`Transliterate`](reference/v3-0-transliterate.md)

[`BreakSentence`](reference/v3-0-break-sentence.md)

[`Detect`](reference/v3-0-detect.md)

[`Dictionary/lookup`](reference/v3-0-dictionary-lookup.md)

[`Dictionary/example`](reference/v3-0-dictionary-examples.md)

## <a name="compatibility-and-customization"></a>Compatibilidad y personalización

> [!NOTE]
> 
> El centro de Microsoft Translator se retirará de 17 de mayo de 2019. [Ver las fechas y la información de migración importante](https://www.microsoft.com/translator/business/hub/).   

Microsoft Translator V3 usa la traducción automática neuronal por defecto. Por lo tanto, no puede utilizarse con Microsoft Translator Hub. Translator Hub solo admite traducción automática estadística heredada. La personalización de la traducción neuronal está disponible si usa el Traductor personalizado. [Obtenga más información sobre cómo personalizar la traducción automática neuronal](custom-translator/overview.md)

La traducción neuronal con Text API V3 no admite el uso de categorías estándar (SMT, voz, tech, generalnn).

| |Punto de conexión|    Compatible con el procesador GDPR|  Utiliza Translator Hub| Utiliza Traductor personalizado (versión preliminar)|
|:-----|:-----|:-----|:-----|:-----|
|Translator Text API versión 2| api.microsofttranslator.com|    No  |Sí    |Sin |
|Translator Text API versión 3| api.cognitive.microsofttranslator.com|  Sí|    Sin | Sí|

**Translator Text API versión 3**
* Está disponible con carácter general y es totalmente compatible.
* Es compatible con GDPR como procesador y satisface todas las estipulaciones de ISO 20001 y 20018, así como los requisitos de certificación de SOC 3. 
* Permite invocar los sistemas de traducción de redes neuronales que se han personalizado con Traductor personalizado (versión preliminar), la nueva característica de personalización NMT de Translator. 
* No proporciona acceso a los sistemas de traducción personalizada creados con Microsoft Translator Hub.

Si usa el punto de conexión api.cognitive.microsofttranslator.com, esta utilizando la versión 3 de Translator Text API.

**Translator Text API versión 2**
* No cumple todos los requisitos de certificación de ISO 20001, 20018 y SOC 3. 
* No permite invocar los sistemas de traducción de redes neuronales que se han personalizado con la característica Traductor personalizado.
* Proporciona acceso a los sistemas de traducción personalizada creados con Microsoft Translator Hub.
* Si usa el punto de conexión api.microsofttranslator.com, está utilizando la versión 2 de Translator Text API.

Ninguna versión de Translator API crea un registro de las traducciones. Las traducciones nunca se comparten con nadie. Obtenga más información en la página [No hay rastro de Translator](https://www.aka.ms/NoTrace).

## <a name="links"></a>Vínculos

* [Directiva de privacidad de Microsoft](https://privacy.microsoft.com/privacystatement)
* [Información legal de Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Términos de Online Services](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ver la documentación de V3.0](reference/v3-0-reference.md)

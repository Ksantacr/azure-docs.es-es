---
title: 'Asociación de un recurso de Cognitive Services con un conjunto de aptitudes: Azure Search'
description: Instrucciones para conectar una suscripción de Cognitive Services en uno a una canalización de enriquecimiento cognitivos en Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 44f16b3334b991e071fa85ca4cffbc0837f0a6ec
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244429"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search 

Unidad de algoritmos de inteligencia artificial el [cognitivas canalizaciones indexación](cognitive-search-concept-intro.md) utilizado con este fin del documento en Azure Search. Estos algoritmos se basan en recursos de Azure Cognitive Services, incluidos [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/) para análisis de imágenes y reconocimiento óptico de caracteres (OCR) y [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para reconocimiento de entidades extracción de frases clave y enriquecimientos de otros. Ya que usa Azure Search para fines de enriquecimiento del documento, los algoritmos se encapsulan dentro de un *aptitud*, colocados en un *aptitudes*y que se hace referencia por un *indizador* durante la indización.

Puede enriquecer un número limitado de documentos de forma gratuita. O bien, puede asociar un recurso de Cognitive Services facturable para una *aptitudes* para cargas de trabajo mayores y más frecuentes. En este artículo, obtendrá información sobre cómo conectar un recurso de Cognitive Services facturable para enriquecer documentos durante la búsqueda de Azure [indización](search-what-is-an-index.md).

> [!NOTE]
> Eventos facturables incluyen las llamadas a API de Cognitive Services y la imagen de extracción como parte de la fase de averiguación de documentos en Azure Search. No hay ningún cargo para la extracción de texto en documentos o las aptitudes que no llame a Cognitive Services.
>
> Ejecución de habilidades facturables será el [Cognitive Services paga a medida que vaya precio](https://azure.microsoft.com/pricing/details/cognitive-services/). Para imagen extracción precios, consulte el [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Requisito de la misma región

Necesitamos que Azure Search y Azure Cognitive Services existen en la misma región. De lo contrario, obtendrá este mensaje en tiempo de ejecución: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

No hay ninguna manera de mover servicios entre regiones. Si recibe este error, debe crear un nuevo recurso de Cognitive Services en la misma región que Azure Search.

## <a name="use-free-resources"></a>Uso de recursos gratis

Puede usar una opción de procesamiento limitada y gratuita para completar los ejercicios del tutorial y la guía de inicio rápido de Cognitive Search.

Recursos gratuitos (enriquecimientos limitado) están restringidos a 20 documentos al día por suscripción.

1. Abra al Asistente para importar datos:

   ![Abra el Asistente para importar datos](media/search-get-started-portal/import-data-cmd2.png "abrir el Asistente para importar datos")

1. Elija un origen de datos y seguir **agregar cognitive search (opcional)** . Para ver un tutorial paso a paso de este asistente, consulte [de importación, índice y consulta mediante el uso de las herramientas del portal](search-get-started-portal.md).

1. Expanda **adjuntar Cognitive Services** y, a continuación, seleccione **gratis (enriquecimientos limitados)** :

   ![Expande la sección Servicios cognitivos adjuntar](./media/cognitive-search-attach-cognitive-services/attach1.png "sección expandido adjuntar Cognitive Services")

1. Continúe con el paso siguiente, **Enriquecimientos de agregar**. Para obtener una descripción de habilidades disponibles en el portal, consulte [paso 2: Agregar conocimientos cognitivos](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en la Guía de inicio rápido de búsqueda cognitiva.

## <a name="use-billable-resources"></a>Uso de recursos facturables

Para cargas de trabajo que creación más de 20 enriquecimientos al día, asegúrese de asociar un recurso de Cognitive Services facturable. Se recomienda que siempre adjuntar un recurso de Cognitive Services facturable, incluso si no se pretende llamar a Cognitive Services APIs. Asociar un recurso, invalida el límite diario.

Se le cobrará solo para las habilidades que llamar a Cognitive Services APIs. No se le cobra para [habilidades personalizadas](cognitive-search-create-custom-skill-example.md), o como habilidades [combinación de texto](cognitive-search-skill-textmerger.md), [divisor de texto](cognitive-search-skill-textsplit.md), y [conformador](cognitive-search-skill-shaper.md), que no están basadas en API.

1. Abra el Asistente para importar datos, elija un origen de datos y seguir **agregar cognitive search (opcional)** .

1. Expanda **adjuntar Cognitive Services** y, a continuación, seleccione **crear nuevo recurso de Cognitive Services**. Se abrirá una nueva pestaña para que pueda crear el recurso:

   ![Crear un recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "crear un recurso de Cognitive Services")

1. En el **ubicación** lista, seleccione la región donde se encuentra el servicio Azure Search. Asegúrese de usar esta región por motivos de rendimiento. Uso de esta región también anula cargos de ancho de banda saliente entre regiones.

1. En el **tarifa** lista, seleccione **S0** para obtener la colección all-in-one de características de Cognitive Services, incluidas las funciones de visión y el idioma que respaldan las habilidades predefinidas usadas por Azure Search.

   Para el nivel S0, puede encontrar las tarifas para cargas de trabajo específicas en el [página de precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + En el **seleccione ofrecen** lista, asegúrese de que **Cognitive Services** está seleccionada.
   + En **lenguaje** características, las tasas de **estándar de análisis de texto** se aplican a la indexación de inteligencia artificial.
   + En **Vision** características, las tasas de **equipo Vision S1** aplicar.

1. Seleccione **crear** para aprovisionar el nuevo recurso de Cognitive Services.

1. Vuelva a la pestaña anterior, que contiene al Asistente para importar datos. Seleccione **actualizar** para mostrar el recurso de Cognitive Services y, a continuación, seleccione el recurso:

   ![Seleccione el recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/attach2.png "seleccione el recurso de Cognitive Services")

1. Expanda el **Enriquecimientos de agregar** sección para seleccionar los conocimientos cognitivos específicos que desee ejecutar en los datos. Complete el resto del asistente. Para obtener una descripción de habilidades disponibles en el portal, consulte [paso 2: Agregar conocimientos cognitivos](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en la Guía de inicio rápido de búsqueda cognitiva.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Asociación de un conjunto de aptitudes existentes con un recurso de Cognitive Services

Si ya tiene un conjunto de aptitudes, puede asociarlo a un recurso de Cognitive Services nuevo o distinto.

1. En el **información general del servicio** página, seleccione **conjuntos de habilidades**:

   ![Pestaña Conjuntos de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Skillsets tab")

1. Seleccione el nombre de las habilidades y, a continuación, seleccione un recurso existente o crear uno nuevo. Seleccione **Aceptar** para confirmar los cambios.

   ![Lista de recursos del conjunto de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lista de recursos del conjunto de aptitudes")

   Recuerde que el **gratis (enriquecimientos limitados)** opción le limita a 20 documentos diariamente, y que puede usar **crear nuevo recurso de Cognitive Services** para aprovisionar un nuevo recurso facturable. Si crea un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso.

## <a name="attach-cognitive-services-programmatically"></a>Asociación de Cognitive Services mediante programación

Cuando defina un conjunto de aptitudes mediante programación, agregue una sección `cognitiveServices` al conjunto de aptitudes. En esa sección, incluya la clave del recurso de Cognitive Services que desea asociar con el conjunto de habilidades. Recuerde que el recurso debe estar en la misma región que el recurso de Azure Search. Además incluya `@odata.type` y establézcalo en `#Microsoft.Azure.Search.CognitiveServicesByKey`.

En el siguiente ejemplo se muestra este patrón. Tenga en cuenta la `cognitiveServices` sección al final de la definición.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Ejemplo: cálculo de los costos

Para calcular los costos asociados con la indización de búsqueda cognitiva, comience con una idea del aspecto de un medio de los documentos para que pueda ejecutar algunos números. Por ejemplo, podría aproximado:

+ 1000 PDF.
+ Seis páginas cada uno.
+ Una imagen por página (6.000 imágenes).
+ 3.000 caracteres por página.

Suponga una canalización que consta de averiguación de documentos de cada extracción PDF, imagen y texto, reconocimiento óptico de caracteres (OCR) de imágenes y reconocimiento de entidades de las organizaciones.

Los precios mostrados en este artículo son hipotéticos. Se usan para ilustrar el proceso de estimación. Los costos podrían ser inferior. Para obtener los precios reales de transacciones, consulte [precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Para el descifrado de documentos con contenido de texto e imagen, la extracción de texto actualmente es gratis. 6.000 imágenes, se supone 1 USD cada 1000 imágenes extraídas. Es un costo de $6.00 para este paso.

2. Para realizar el reconocimiento óptico de caracteres (OCR) de 6000 imágenes en inglés, la aptitud de reconocimiento de OCR usa el mejor algoritmo (DescribeText). Suponiendo un precio de 2,50 USD por cada 1000 imágenes que se analizan, tendríamos que pagar 15,00 USD en este paso.

3. Extracción de entidades, tendría un total de tres registros de texto por página. Cada registro tiene 1000 caracteres. Tres registros de texto por página multiplicado por 6.000 páginas es igual a 18 000 registros de texto. Suponiendo un costo de 2,00 USD por cada 1,000 registros de texto, en este paso tendríamos un costo de 36,00 USD.

En resumen, pagaría aproximadamente 57,00 $ para la ingesta de 1.000 documentos PDF de este tipo con el conjunto de habilidades descrita.

## <a name="next-steps"></a>Pasos siguientes
+ [Página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)

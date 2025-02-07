---
title: 'Tutorial: Llamada a las API REST de Cognitive Services en una canalización de indexación de Azure Search'
description: Aquí se analiza un ejemplo de procesamiento de IA de imágenes, de extracción de datos y de lenguaje natural en la indexación de Azure Search para la extracción y la transformación de datos en blobs de JSON mediante Postman y la API REST.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: fb45d2e36939a53d6242cf7cd5a0b9f1990780c3
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299054"
---
# <a name="rest-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>Tutorial de REST: Llamada a Cognitive Services APIs en una canalización de indexación de Azure Search

En este tutorial, aprenderá la mecánica para programar el enriquecimiento de datos en Azure Search mediante *aptitudes cognitivas*. Las aptitudes tienen el respaldo de las funcionalidades del procesamiento de lenguaje natural (NLP) y del análisis de imágenes de Cognitive Services. A través de la composición y configuración del conjunto de aptitudes puede extraer texto y representaciones de texto de un archivo con una imagen o con un documento digitalizado. También puede detectar el idioma, las entidades, las frases clave, etc. El resultado final es un contenido adicional enriquecido en un índice de Azure Search, creado gracias a enriquecimientos por IA en una canalización de indexación. 

En este tutorial, se realizan llamadas a la API de REST para llevar a cabo las siguientes tareas:

> [!div class="checklist"]
> * Crear una canalización de indexación para enriquecer los datos de ejemplo en la ruta a un índice.
> * Aplicar las aptitudes integradas: reconocimiento de entidades, detección de idioma, manipulación de texto y extracción de frases clave.
> * Aprender a encadenar aptitudes mediante la asignación de las entradas a las salidas en conjunto de aptitudes.
> * Ejecutar solicitudes y revisar los resultados.
> * Restablecer el índice y los indexadores para su posterior desarrollo.

La salida es un índice que permite realizar búsquedas de texto completo en Azure Search. Puede mejorar el índice con otras funciones estándares, como [sinónimos](search-synonyms.md), [perfiles de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [analizadores](search-analyzers.md) y [filtros](search-filters.md).

Este tutorial se ejecuta en el servicio Gratis, pero el número de transacciones gratuitas está limitado a 20 documentos por día. Si desea ejecutar este tutorial más de una vez en el mismo día, use un conjunto de archivos más pequeño para que pueda realizar más ejecuciones.

> [!NOTE]
> A medida que expanda el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de IA, tendrá que [asociar un recurso facturable de Cognitive Services](cognitive-search-attach-cognitive-services.md). Los cargos se acumulan cuando se llama a las API de Cognitive Services y para la extracción de imágenes como parte de la fase de descifrado de documentos en Azure Search. No hay ningún cargo por la extracción de texto de documentos.
>
> La ejecución de aptitudes integradas se cobra según los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. La extracción de imágenes se cobra tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se usan los siguientes servicios, herramientas y datos. 

+ [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo. Asegúrese de que la cuenta de almacenamiento se encuentra en la misma región que Azure Search.

+ La [aplicación de escritorio Postman](https://www.getpostman.com/) se usa para hacer llamadas REST a Azure Search.

+ Los [datos de ejemplo](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) están formados por un pequeño conjunto de archivos de tipos diferentes. 

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas de REST requieren la dirección URL del servicio y una clave de acceso en cada solicitud. Con ambos se crea un servicio de búsqueda, por lo que si ha agregado Azure Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio. Tener una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que envía la solicitud y el servicio que se encarga de ella.

## <a name="prepare-sample-data"></a>Preparación de datos de ejemplo

La canalización de enriquecimiento extrae los orígenes de datos de Azure. Los datos de origen deben proceder de un tipo de origen de datos compatible de un [indexador de Azure Search](search-indexer-overview.md). Azure Table Storage no es compatible con la búsqueda cognitiva. Para realizar este ejercicio, usaremos Blob Storage para mostrar varios tipos de contenido.

1. [Inicie sesión en Azure Portal](https://portal.azure.com), vaya a su cuenta de Azure Storage, haga clic en **Blobs** y, después, en **+Contenedor**.

1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) que contenga los datos de ejemplo. Puede establecer el nivel de acceso público a cualquiera de sus valores válidos.

1. Una vez creado el contenedor, ábralo y seleccione **Cargar** en la barra de comandos para cargar los archivos de ejemplo que descargó en el paso anterior.

   ![Archivos de origen en Azure Blob Storage](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Una vez cargados los archivos de ejemplo, obtenga el nombre del contenedor y una cadena de conexión de Blob Storage. Para hacerlo puede navegar a la cuenta de almacenamiento en Azure Portal. Vaya a **Claves de acceso** y copie el valor del campo **Cadena de conexión**.

   La cadena de conexión debe ser una URL similar a la del ejemplo siguiente:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Hay otras maneras de especificar la cadena de conexión, como proporcionar una firma de acceso compartido. Para obtener más información acerca de las credenciales del origen de datos, consulte [Indexación de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="set-up-postman"></a>Configuración de Postman

Inicie Postman y configure una solicitud HTTP. Si no está familiarizado con esta herramienta, consulte [Exploración de las API REST de Azure Search mediante Postman](search-fiddler.md).

Los métodos de solicitud usados en este tutorial son **POST**, **PUT** y **GET**. Las claves de encabezado son "Content-type" establecido en "application/json" y "api-key", en una clave de administrador del servicio Azure Search. El cuerpo es donde se coloca el contenido real de la llamada. 

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/postmanoverview.png)

Estamos usando Postman para realizar cuatro llamadas API al servicio de búsqueda con el fin de crear un origen de datos, un conjunto de aptitudes, un índice y un indexador. El origen de datos incluye un puntero a la cuenta de almacenamiento y a los datos JSON. El servicio de búsqueda realiza la conexión al cargar los datos.


## <a name="create-a-data-source"></a>Creación de un origen de datos

Ahora que sus servicios y archivos de origen están preparados, comience a ensamblar los componentes de la canalización de indexación. Comience con un [objeto de origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source) que indique a Azure Search cómo recuperar datos de orígenes externos.

En el encabezado de la solicitud, proporcione el nombre del servicio que utilizó al crear el servicio Azure Search y la clave de API que se generó para el servicio de búsqueda. En el cuerpo de la solicitud, especifique la cadena de conexión y al nombre del contenedor de blobs.

### <a name="sample-request"></a>Solicitud de ejemplo
```http
POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]
```
#### <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud
```json
{
  "name" : "demodata",
  "description" : "Demo files to demonstrate cognitive search capabilities.",
  "type" : "azureblob",
  "credentials" :
  { "connectionString" :
    "DefaultEndpointsProtocol=https;AccountName=<your account name>;AccountKey=<your account key>;"
  },
  "container" : { "name" : "<your blob container name>" }
}
```
Envíe la solicitud. La herramienta de prueba web debe devolver un código de estado 201 que confirme el éxito de la operación. 

Puesto que se trata de su primera solicitud, consulte Azure Portal para confirmar el origen de datos se creó en Azure Search. En la página del panel del servicio de búsqueda, compruebe que la lista de Data Sources tiene un nuevo elemento. Debe esperar unos minutos a que la página del portal se actualice. 

  ![Icono de Data Sources del portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Icono de Data Sources del portal")

Si obtuvo un error 403 o 404, compruebe la construcción de la solicitud: `api-version=2019-05-06` debe estar en el punto de conexión, `api-key` debe estar en el encabezado después de `Content-Type` y su valor debe ser válido para un servicio de búsqueda. Puede volver a usar el encabezado para los pasos restantes de este tutorial.

## <a name="create-a-skillset"></a>Creación de un conjunto de aptitudes

En este paso, definirá un conjunto de pasos de enriquecimiento que desee aplicar a los datos. Llamará a cada paso de enriquecimiento una *aptitud* y al conjunto de pasos de enriquecimiento, un *conjunto de aptitudes*. Este tutorial usa un conjunto de [aptitudes cognitivas integradas](cognitive-search-predefined-skills.md):

+ [Detección de idioma](cognitive-search-skill-language-detection.md) para identificar el idioma del contenido.

+ [División de texto](cognitive-search-skill-textsplit.md) para dividir contenido de gran tamaño en fragmentos más pequeños antes de llamar a la aptitud de extracción de frases clave. La extracción de frases clave acepta entradas de 50 000 caracteres o menos. Algunos de los archivos de ejemplo deben dividirse para no superar este límite.

+ [Reconocimiento de entidades](cognitive-search-skill-entity-recognition.md) para extraer los nombres de las organizaciones del contenido del contenedor de blobs.

+ [Extracción de frases clave](cognitive-search-skill-keyphrases.md) para extraer las frases clave principales. 

### <a name="sample-request"></a>Solicitud de ejemplo
Antes de realizar esta llamada de REST, recuerde reemplazar el nombre del servicio y la clave de administrador en la solicitud siguiente si la herramienta no conserva el encabezado de la solicitud entre las llamadas. 

Esta solicitud crea un conjunto de aptitudes. Haga referencia al nombre del conjunto de aptitudes ```demoskillset``` para el resto de este tutorial.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud
```json
{
  "description":
  "Extract entities, detect language and extract key-phrases",
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
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "languageCode",
          "targetName": "languageCode"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "textSplitMode" : "pages",
      "maximumPageLength": 4000,
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        },
        {
          "name": "languageCode",
          "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "textItems",
          "targetName": "pages"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "context": "/document/pages/*",
      "inputs": [
        {
          "name": "text", "source": "/document/pages/*"
        },
        {
          "name":"languageCode", "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "keyPhrases",
          "targetName": "keyPhrases"
        }
      ]
    }
  ]
}
```

Envíe la solicitud. La herramienta de prueba web debe devolver un código de estado 201 que confirme el éxito de la operación. 

#### <a name="explore-the-request-body"></a>Exploración del cuerpo de la solicitud

Observe cómo se aplica la aptitud de extracción de frases clave para cada página. Al establecer el contexto en ```"document/pages/*"```, se ejecuta este enriquecedor para cada miembro de la matriz de documentos o páginas (para cada página del documento).

Cada aptitud se ejecuta en el contenido del documento. Durante el procesamiento, Azure Search extrae cada documento para leer el contenido de diferentes formatos de archivo. El texto encontrado procedente del archivo de origen se coloca en un campo ```content``` generado, uno para cada documento. Por lo tanto, establezca la entrada como ```"/document/content"```.

A continuación se muestra una representación gráfica del conjunto de aptitudes. 

![Comprender un conjunto de aptitudes](media/cognitive-search-tutorial-blob/skillset.png "Comprender un conjunto de aptitudes")

Las salidas se pueden asignar a un índice, usar como entrada para una aptitud descendente, o ambas cosas como sucede con el código de idioma. En el índice, un código de idioma es útil para el filtrado. Como entrada, el código de idioma se usa en las aptitudes de análisis de texto para informar a las reglas lingüísticas de la separación de palabras.

Para obtener más información sobre los conceptos básicos del conjunto de aptitudes, consulte el tema sobre la [definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md).

## <a name="create-an-index"></a>Creación de un índice

En esta sección, se define el esquema de índice. Para ello, se especifican los campos que se incluirán en el índice que permite búsquedas y los atributos de búsqueda de cada campo. Los campos tienen un tipo y pueden tener atributos que determinen cómo se utiliza el campo (si permite búsquedas, se puede ordenar, etc.). Los nombres de campos de un índice no tienen que coincidir exactamente con los nombres de campo del origen. En un paso posterior, agregará asignaciones de campos en un indexador para conectar los campos de origen y destino. Para este paso, defina el índice con convenciones de nomenclatura de campos adecuadas para la aplicación de búsqueda.

En este ejercicio se utilizan los siguientes campos y tipos de campos:

| field-names: | `id`       | contenido   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Solicitud de ejemplo
Antes de realizar esta llamada de REST, recuerde reemplazar el nombre del servicio y la clave de administrador en la solicitud siguiente si la herramienta no conserva el encabezado de la solicitud entre las llamadas. 

Esta solicitud crea un índice. Utilice el nombre del índice ```demoindex``` para el resto de este tutorial.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud

```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
Envíe la solicitud. La herramienta de prueba web debe devolver un código de estado 201 que confirme el éxito de la operación. 

Para más información acerca de cómo definir un índice, consulte [Crear índice (API de REST de Azure Search)](https://docs.microsoft.com/rest/api/searchservice/create-index).


## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Creación de un indexador, asignación de campos y ejecución de transformaciones

Hasta ahora, ha creado un origen de datos, un conjunto de aptitudes y un índice. Estos tres componentes pasan a formar parte de un [indexador](search-indexer-overview.md) que extrae todas las piezas juntas en una sola operación de varias fases. Para unirlas en un indexador, debe definir las asignaciones de campos. 

+ Los campos fieldMappings se procesan antes que el conjunto de aptitudes, lo que asigna los campos de origen del origen de datos a los campos de destino de un índice. Si los tipos y nombres de campo son los mismos en ambos extremos, no se requiere ninguna asignación.

+ Los campos outputFieldMappings se procesan después que el conjunto de aptitudes y hacen referencia a las asignaciones sourceFieldNames que no existen hasta que el descifrado o enriquecimiento de documentos las crean. targetFieldName es un campo de un índice.

Además de enlazar las entradas a las salidas, también puede usar las asignaciones de campos para aplanar las estructuras de datos. Para más información, consulte [Asignación de campos enriquecidos a un índice de búsqueda](cognitive-search-output-field-mapping.md).

### <a name="sample-request"></a>Solicitud de ejemplo

Antes de realizar esta llamada de REST, recuerde reemplazar el nombre del servicio y la clave de administrador en la solicitud siguiente si la herramienta no conserva el encabezado de la solicitud entre las llamadas. 

Proporcione también el nombre del indexador. Puede hacer referencia a este indexador como ```demoindexer``` para el resto de este tutorial.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
      "sourceFieldName" : "metadata_storage_path",
      "targetFieldName" : "id",
      "mappingFunction" :
        { "name" : "base64Encode" }
    },
    {
      "sourceFieldName" : "content",
      "targetFieldName" : "content"
    }
  ],
  "outputFieldMappings" :
  [
    {
      "sourceFieldName" : "/document/organizations",
      "targetFieldName" : "organizations"
    },
    {
      "sourceFieldName" : "/document/pages/*/keyPhrases/*",
      "targetFieldName" : "keyPhrases"
    },
    {
      "sourceFieldName": "/document/languageCode",
      "targetFieldName": "languageCode"
    }
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "maxFailedItemsPerBatch":-1,
    "configuration":
    {
      "dataToExtract": "contentAndMetadata",
      "imageAction": "generateNormalizedImages"
    }
  }
}
```

Envíe la solicitud. La herramienta de prueba web debe devolver un código de estado 201 que confirme el éxito del procesamiento. 

Este paso puede tardar varios minutos en completarse. Aunque el conjunto de datos es pequeño, las aptitudes analíticas realiza un uso intensivo de los recursos. Algunas aptitudes, como el análisis de imágenes, son de larga ejecución.

> [!TIP]
> La creación de un indexador invoca la canalización. Si hay problemas para conectar con los datos, las entradas y salidas de asignación o el orden de las operaciones, se muestran en esta fase. Para volver a ejecutar la canalización con los cambios de código o script, deberá quitar primero los objetos. Para más información, consulte [Restablecer y volver a ejecutar](#reset).

#### <a name="explore-the-request-body"></a>Exploración del cuerpo de la solicitud

El script establece ```"maxFailedItems"``` en -1, que indica al motor de indexación que ignore los errores durante la importación de datos. Esto es útil porque hay muy pocos documentos en el origen de datos de demostración. Para un origen de datos mayor, debería establecer un valor mayor que 0.

Observe también la instrucción ```"dataToExtract":"contentAndMetadata"``` en los parámetros de configuración. Esta instrucción indica al indexador que extraiga automáticamente el contenido de diferentes formatos de archivo, así como los metadatos relacionados con cada archivo. 

Una vez extraído el contenido, puede establecer ```imageAction``` para que se extraiga el texto de las imágenes que se encuentran en el origen de datos. La configuración de ```"imageAction":"generateNormalizedImages"```, en combinación con OCR Skill y Text Merge Skill, le dice al indexador que extraiga texto de las imágenes (por ejemplo, la palabra "stop" de una señal de Stop de tráfico) y lo inserte como parte del campo de contenido. Este comportamiento se aplica a las imágenes incrustadas en los documentos (piense en una imagen de un archivo PDF), así como a las imágenes que se encuentran en el origen de datos, como un archivo JPG.

## <a name="check-indexer-status"></a>Comprobación del estado del indexador

Una vez definido el indexador, se ejecuta automáticamente cuando se envía la solicitud. Dependiendo de las aptitudes cognitivas definidas, la indexación puede tardar más de lo esperado. Para averiguar si el indexador todavía se está ejecutando, envíe la solicitud siguiente para comprobar el estado del indexador.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

La respuesta indica si se está ejecutando el indexador. Una vez finalizada la indexación, use otro método HTTP GET para el punto de conexión STATUS (como se indica anteriormente) para ver los informes de errores y advertencias que se produjeron durante el enriquecimiento.  

Las advertencias son comunes con algunas combinaciones de aptitudes y archivos de origen y no siempre indican un problema. En este tutorial, las advertencias son benignas (por ejemplo, no hay entradas de texto de archivos JPEG). Puede revisar la respuesta de estado para obtener información detallada acerca de las advertencias generadas durante la indexación.
 
## <a name="query-your-index"></a>Consulta del índice

Una vez finalizada la indexación, ejecute consultas que devuelvan el contenido de los campos individuales. De forma predeterminada, Azure Search devuelve los primeros 50 resultados. Los datos de ejemplo tiene un tamaño reducido, por lo que la configuración predeterminada funciona bien. Sin embargo, si trabaja con conjuntos de datos más grandes, deberá incluir parámetros en la cadena de consulta para que se devuelvan más resultados. Para obtener instrucciones, consulte [Cómo paginar los resultados de la búsqueda en Azure Search](search-pagination-page-layout.md).

Como paso de verificación, consulte el índice para todos los campos.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

La salida es el esquema de índice, con el nombre, el tipo y los atributos de cada campo.

Envíe una segunda consulta para `"*"` a fin de devolver todo el contenido de un campo único, como `organizations`.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Repita el proceso para otros campos: contenido, languageCode, keyPhrases y organizaciones del ejercicio. Puede devolver varios campos a través de `$select` con una lista delimitada por comas.

Puede usar GET o POST, según la longitud y la complejidad de la cadena de consulta. Para más información, consulte la [Realización de una consulta al índice de Azure Search con la API de REST](https://docs.microsoft.com/rest/api/searchservice/search-documents).



<a name="reset"></a>

## <a name="reset-and-rerun"></a>Restablecer y volver a ejecutar

En las primeras etapas experimentales del desarrollo de canalizaciones, el enfoque más práctico para las iteraciones de diseño es eliminar los objetos de Azure Search y permitir que el código vuelva a generarlos. Los nombres de los recursos son únicos. La eliminación de un objeto permite volver a crearlo con el mismo nombre.

Para volver a indexar los documentos con las nuevas definiciones:

1. Elimine el índice para quitar los datos persistentes. Elimine el indexador para volver a crearlo en su servicio.
2. Modifique un conjunto de aptitudes y la definición del índice.
3. Vuelva a crear un índice y un indexador en el servicio para ejecutar la canalización. 

Puede usar el portal para eliminar los índices, los indizadores y los conjuntos de aptitudes.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Se devuelve el código de estado 204 si la eliminación se realiza correctamente.

A medida que el código evoluciona, puede refinar una estrategia de regeneración. Para más información, consulte [Volver a generar un índice](search-howto-reindex.md).

## <a name="takeaways"></a>Puntos clave

Este tutorial muestra los pasos básicos para la creación de una canalización de indexación enriquecida a través de la creación de componentes: un origen de datos, un conjunto de aptitudes, un índice y un indexador.

Las [aptitudes predefinidas](cognitive-search-predefined-skills.md) se presentan, junto con la definición del conjunto de aptitudes y los mecanismos de encadenamiento de aptitudes, a través de entradas y salidas. También ha aprendido que `outputFieldMappings` en la definición del indexador se necesita para enrutar los valores enriquecidos de la canalización a un índice que permita búsquedas en un servicio Azure Search.

Por último, ha aprendido cómo probar los resultados y restablecer el sistema para otras iteraciones. Ha aprendido que emitir consultas en el índice devuelve la salida creada por la canalización de indexación enriquecida. En esta versión, existe un mecanismo para ver las construcciones internas (documentos enriquecidos creados por el sistema). También ha aprendido cómo comprobar el estado del indexador y qué objetos se deben eliminar antes de volver a ejecutar una canalización.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contiene el servicio Azure Search y el servicio Azure Blob. Si decide colocar ambos servicios en el mismo grupo, elimine el grupo de recursos para eliminar de manera permanente todo lo que contiene, incluyendo los servicios y cualquier contenido almacenado que haya creado para este tutorial. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio.

## <a name="next-steps"></a>Pasos siguientes

Personalice o extienda la canalización con aptitudes personalizadas. Al crear una aptitud personalizada y agregarla a un conjunto de aptitudes, puede incorporar texto o análisis de imágenes que escriba usted mismo. 

> [!div class="nextstepaction"]
> [Ejemplo: creación de una aptitud personalizada](cognitive-search-create-custom-skill-example.md)

---
title: Copia de datos con un sistema de archivos como origen o destino mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos de cualquier sistema de archivos en almacenes de datos receptores o de almacenes de datos de origen compatibles en el sistema de archivos mediante Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: bdc3272cffb84b19bfbab8ffe61814b3f5ea8b1d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228569"
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Copia de datos con un sistema de archivos como origen o destino mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-onprem-file-system-connector.md)
> * [Versión actual](connector-file-system.md)

En este artículo se describe cómo copiar datos hacia y desde el sistema de archivos. Para información sobre Azure Data Factory, lea el [artículo de introducción](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de sistema de archivos es compatible con las siguientes actividades:

- [Actividad de copia](copy-activity-overview.md) con [admite la matriz de origen/receptor](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)

En concreto, este conector de sistema de archivos admite las siguientes funcionalidades:

- Copiar archivos con una máquina local o un recurso compartido de archivos de red como origen o destino. Para usar un recurso compartido de archivos de Linux, instale [Samba](https://www.samba.org/) en el servidor Linux.
- Copiar los archivos con la autenticación de **Windows**.
- Copiar los archivos tal cual, o bien analizarlos o generarlos, con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Requisitos previos

Para copiar datos con un sistema de archivos como origen o destino que no es accesible públicamente, debe configurar un entorno Integration Runtime (autohospedado). Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes, se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas de un sistema de archivos.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de sistema de archivos:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **FileServer**. | Sí |
| host | Especifica la ruta de acceso raíz de la carpeta que quiere copiar. Use el carácter de escape "\"" para los caracteres especiales de la cadena. Consulte los casos que se exponen en [Ejemplos de definiciones de servicio vinculado y conjunto de datos](#sample-linked-service-and-dataset-definitions) . | Sí |
| userid | Especifique el identificador del usuario que tiene acceso al servidor. | Sí |
| password | Especifique la contraseña del usuario (identificador de usuario). Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) o Azure Integration Runtime (si el almacén de datos es accesible públicamente). Si no se especifica, se usará Azure Integration Runtime. |No |

### <a name="sample-linked-service-and-dataset-definitions"></a>Ejemplos de definiciones de servicio vinculado y conjunto de datos

| Escenario | "host" en definición de servicio vinculado | "folderPath" en definición de conjunto de datos |
|:--- |:--- |:--- |
| Carpeta local en máquina Integration Runtime: <br/><br/>Ejemplos: D:\\\* o D:\folder\subfolder\\* |En JSON: `D:\\`<br/>En la interfaz de usuario: `D:\` |En JSON: `.\\` o `folder\\subfolder`<br>En la interfaz de usuario: `.\` o `folder\subfolder` |
| Carpeta compartida remota:  <br/><br/>Ejemplos: \\\\myserver\\share\\\* o \\\\myserver\\share\\folder\\subfolder\\* |En JSON: `\\\\myserver\\share`<br/>En la interfaz de usuario: `\\myserver\share` |En JSON: `.\\` o `folder\\subfolder`<br/>En la interfaz de usuario: `.\` o `folder\subfolder` |

>[!NOTE]
>Cuando se crean a través de la interfaz de usuario, no es necesario escribir dos barras diagonales (`\\`) de escape como a través de JSON, solo una barra diagonal inversa.

**Ejemplo:**

```json
{
    "name": "FileLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "<host>",
            "userid": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). 

- Para **Parquet y formato de texto delimitado**, consulte [conjunto de datos con formato Parquet y texto delimitado](#parquet-and-delimited-text-format-dataset) sección.
- Para otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro conjunto de datos de formato](#other-format-dataset) sección.

### <a name="parquet-and-delimited-text-format-dataset"></a>Conjunto de datos con formato parquet y texto delimitado

Para copiar datos hacia y desde el sistema de archivos en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo en el conjunto de datos de formato y configuraciones admitidas. Se admiten las siguientes propiedades para el sistema de archivos en `location` configuración en el conjunto de datos basado en el formato:

| Propiedad   | DESCRIPCIÓN                                                  | Obligatorio |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | La propiedad type en `location` en el conjunto de datos debe establecerse en **FileServerLocation**. | Sí      |
| folderPath | La ruta de acceso a la carpeta. Si desea usar el carácter comodín a la carpeta de filtro, omita esta configuración y especificar en la configuración del origen de actividad. | Sin        |
| fileName   | El nombre de archivo en folderPath determinado. Si desea usar el carácter comodín para filtrar los archivos, omita esta configuración y especificar en la configuración del origen de actividad. | No       |

> [!NOTE]
> **Recurso compartido de archivos** todavía se admite el tipo de conjunto de datos con formato Parquet/texto que se mencionan en la siguiente sección como-es para la actividad de copia/búsqueda/GetMetadata para compatibilidad con versiones anteriores, pero no funciona con la asignación de flujo de datos. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<File system linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FileServerLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Otro conjunto de datos de formato

Para copiar datos hacia y desde el sistema de archivos en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **FileShare** |Sí |
| folderPath | Ruta de acceso a la carpeta. Se admite el filtro de comodín, los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro. <br/><br/>Ejemplos: rootfolder/subcarpeta/ver más en [Ejemplos de definiciones de servicio vinculado y conjunto de datos](#sample-linked-service-and-dataset-definitions) y [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). |No |
| fileName | **Filtro de nombre o de comodín** para los archivos de la ruta "folderPath" especificada. Si no especifica ningún valor para esta propiedad, el conjunto de datos apunta a todos los archivos de la carpeta. <br/><br/>Para filtrar, los caracteres comodín permitidos son: `*` (equivale a cero o a varios caracteres) y `?` (equivale a cero o a un único carácter).<br/>- Ejemplo 1: `"fileName": "*.csv"`<br/>- Ejemplo 2: `"fileName": "???20180427.txt"`<br/>Use `^` como escape si el nombre de archivo real contiene un comodín o este carácter de escape.<br/><br/>Cuando fileName no se especifica para un conjunto de datos de salida y **preserveHierarchy** no se determina en el receptor de la actividad, la actividad de copia generará automáticamente el nombre de archivo con el siguiente patrón: "*Datos. [identificador GUID de ejecución de actividad]. [GUID si FlattenHierarchy]. [formato si configura]. [Si configura la compresión]* ", p. ej. "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; si realiza una copia desde el origen tabular utilizando el nombre de la tabla en lugar de la consulta, el patrón del nombre es "*[nombre de la tabla].[formato].[compresión si está configurada]*", p. ej. "MyTable.csv". |Sin  |
| modifiedDatetimeStart | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Tenga en cuenta que el rendimiento general del movimiento de datos se verán afectado por si habilita a esta configuración cuando desee del filtro de archivos de grandes cantidades de archivos. <br/><br/> Las propiedades pueden ser NULL que significa que no se aplicará ningún filtro de atributo de archivo para el conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| modifiedDatetimeEnd | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br/><br/> Tenga en cuenta que el rendimiento general del movimiento de datos se verán afectado por si habilita a esta configuración cuando desee del filtro de archivos de grandes cantidades de archivos. <br/><br/> Las propiedades pueden ser NULL que significa que no se aplicará ningún filtro de atributo de archivo para el conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora.| Sin  |
| format | Si desea **copiar los archivos tal cual** entre los almacenes basados en archivos (copia binaria), omita la sección de formato en las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar o generar archivos con un formato concreto, se admiten los siguientes tipos de formato de archivo: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Establezca la propiedad **type** de formato en uno de los siguientes valores. Para más información, consulte las secciones [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato Json](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |No (solo para el escenario de copia binaria) |
| compression | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Estos son los tipos que se admiten: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Estos son los niveles que se admiten: **Optimal** y **Fastest**. |Sin  |

>[!TIP]
>Para copiar todos los archivos en una carpeta, especifique solo **folderPath**.<br>Para copiar un único archivo con un nombre determinado, especifique **folderPath** con el elemento de carpeta y **fileName** con el nombre de archivo.<br>Para copiar un subconjunto de archivos en una carpeta, especifique **folderPath** con el elemento de carpeta y **fileName** con el filtro de comodín.

>[!NOTE]
>Si estaba usando la propiedad "fileFilter" para el filtro de archivos, esta todavía se admite como está, aunque se le sugiere que use la nueva funcionalidad de filtro agregada a "fileName" de ahora en adelante.

**Ejemplo:**

```json
{
    "name": "FileSystemDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<file system linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el receptor y el origen de sistema de archivos.

### <a name="file-system-as-source"></a>Sistema de archivos como origen

- Para copiar desde **Parquet y formato de texto delimitado**, consulte [Parquet y origen del formato de texto delimitado](#parquet-and-delimited-text-format-source) sección.
- Para copiar desde otros formatos como **formato ORC/Avro/JSON/binario**, consulte [otro origen de formato](#other-format-source) sección.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet y origen del formato de texto delimitado

Para copiar datos de sistema de archivos en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo sobre el origen de la actividad de copia basada en el formato configuraciones admitidas y. Se admiten las siguientes propiedades para el sistema de archivos en `storeSettings` configuración en el origen de copia basada en el formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | La propiedad type en `storeSettings` debe establecerse en **FileServerReadSetting**. | Sí                                           |
| recursive                | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establece en true y el receptor es un almacén basado en archivos, no se crea una carpeta o una subcarpeta vacía en el receptor. Los valores permitidos son: **True** (valor predeterminado) y **False**. | Sin                                             |
| wildcardFolderPath       | La ruta de acceso de carpeta con caracteres comodín para filtrar las carpetas de origen. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro. <br>Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sin                                             |
| wildcardFileName         | El nombre de archivo con caracteres comodín en el folderPath/wildcardFolderPath determinado para filtrar los archivos de origen. <br>Los caracteres comodín permitidos son: `*` (coincide con cero o más caracteres) y `?` (coincide con cero o carácter individual); use `^` para el escape si el nombre real de la carpeta tiene un carácter comodín o este carácter de escape dentro.  Ver más ejemplos en [Ejemplos de filtros de carpetas y archivos](#folder-and-file-filter-examples). | Sí si `fileName` no se especifica en el conjunto de datos |
| modifiedDatetimeStart    | Filtro de archivos basado en el atributo: Última modificación. Los archivos se seleccionarán si la hora de su última modificación está dentro del intervalo de tiempo entre `modifiedDatetimeStart` y `modifiedDatetimeEnd`. La hora se aplica a la zona horaria UTC en el formato "2018-12-01T05:00:00Z". <br> Las propiedades pueden ser NULL, lo que significa que no se aplicará ningún filtro de atributo de archivo al conjunto de datos.  Cuando `modifiedDatetimeStart` tiene el valor de fecha y hora, pero `modifiedDatetimeEnd` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea mayor o igual que el valor de fecha y hora.  Cuando `modifiedDatetimeEnd` tiene el valor de fecha y hora, pero `modifiedDatetimeStart` es NULL, significa que se seleccionarán los archivos cuyo último atributo modificado sea inferior al valor de fecha y hora. | Sin                                             |
| modifiedDatetimeEnd      | Igual que el anterior.                                               | Sin                                             |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | No                                            |

> [!NOTE]
> Para Parquet/texto delimitado con **FileSystemSource** todavía se admite como origen de la actividad de copia tipo mencionado en la siguiente sección: sirve para la compatibilidad con versiones anteriores. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "FileServerReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Otro origen de formato

Para copiar datos de sistema de archivos en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades en la actividad de copia **origen** sección:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **FileSystemSource** |Sí |
| recursive | Indica si los datos se leen de forma recursiva de las subcarpetas o solo de la carpeta especificada. Tenga en cuenta que cuando recursive se establezca en true y el receptor sea un almacén basado en archivos, la carpeta o subcarpeta vacías no se copiarán ni crearán en el receptor.<br/>Los valores permitidos son: **True** (valor predeterminado) y **False** | Sin  |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<file system input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="file-system-as-sink"></a>Sistema de archivos como receptor

- Para copiar en **Parquet y formato de texto delimitado**, consulte [Parquet y receptores de formato de texto delimitado](#parquet-and-delimited-text-format-sink) sección.
- Para copiar en otros formatos, como **formato ORC/Avro/JSON/binario**, consulte [otros receptores de formato](#other-format-sink) sección.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet y receptores de formato de texto delimitado

Para copiar datos en el sistema de archivos en **Parquet o formato de texto delimitado**, consulte [formato Parquet](format-parquet.md) y [formato de texto delimitado](format-delimited-text.md) artículo en el receptor de la actividad de copia basada en el formato y configuraciones admitidas. Se admiten las siguientes propiedades para el sistema de archivos en `storeSettings` configuración del receptor de copia basada en el formato:

| Propiedad                 | DESCRIPCIÓN                                                  | Obligatorio |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | La propiedad type en `storeSettings` debe establecerse en **FileServerWriteSetting**. | Sí      |
| copyBehavior             | Define el comportamiento de copia cuando el origen son archivos de un almacén de datos basados en archivos.<br/><br/>Los valores permitidos son:<br/><b>- PreserveHierarchy (valor predeterminado)</b>: conserva la jerarquía de archivos en la carpeta de destino. La ruta de acceso relativa del archivo de origen que apunta a la carpeta de origen es idéntica a la ruta de acceso relativa del archivo de destino que apunta a la carpeta de destino.<br/><b>- FlattenHierarchy</b>: todos los archivos de la carpeta de origen están en el primer nivel de la carpeta de destino. Los archivos de destino tienen nombres generados automáticamente. <br/><b>- MergeFiles</b>: combina todos los archivos de la carpeta de origen en un archivo. Si se especifica el nombre del archivo, el nombre de archivo combinado es el nombre especificado. De lo contrario, es un nombre de archivo generado automáticamente. | Sin        |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de datos al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | No       |

> [!NOTE]
> Para Parquet/texto delimitado con **FileSystemSink** todavía se admite como receptor de la actividad de copia tipo mencionado en la siguiente sección: sirve para la compatibilidad con versiones anteriores. Se sugieren para usar este nuevo modelo a partir de ahora, y ha cambiado la interfaz de usuario de creación de ADF para generar estos nuevos tipos.

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "FileServerWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Otros receptores de formato

Para copiar datos en el sistema de archivos en **formato ORC/Avro/JSON/binario**, se admiten las siguientes propiedades en el **receptor** sección:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del receptor de la actividad de copia debe establecerse en: **FileSystemSink** |Sí |
| copyBehavior | Define el comportamiento de copia cuando el origen son archivos del almacén de datos basados en archivos.<br/><br/>Los valores permitidos son:<br/><b>- PreserveHierarchy (valor predeterminado)</b>: conserva la jerarquía de archivos en la carpeta de destino. La ruta de acceso relativa del archivo de origen que apunta a la carpeta de origen es idéntica a la ruta de acceso relativa del archivo de destino que apunta a la carpeta de destino.<br/><b>- FlattenHierarchy:</b> todos los archivos de la carpeta de origen están en el primer nivel de la carpeta de destino. Los archivos de destino tienen un nombre generado automáticamente. <br/><b>- MergeFiles</b>: combina todos los archivos de la carpeta de origen en un archivo. Si se especifica el nombre de archivo/blob, el nombre de archivo combinado sería el nombre especificado; de lo contrario, sería el nombre de archivo generado automáticamente. | Sin  |
| maxConcurrentConnections | El número de conexiones para conectarse al almacén de almacenamiento al mismo tiempo. Especifique solo cuando desee limitar las conexiones simultáneas al almacén de datos. | Sin  |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<file system output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Ejemplos de filtros de carpetas y archivos

Esta sección describe el comportamiento resultante de la ruta de acceso de la carpeta y el nombre de archivo con los filtros de carácter comodín.

| folderPath | fileName | recursive | Resultado de estructura de carpeta de origen y filtro (se recuperan los archivos en **negrita**)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (vacío, usar el valor predeterminado) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (vacío, usar el valor predeterminado) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="recursive-and-copybehavior-examples"></a>Ejemplos de recursive y copyBehavior

En esta sección se describe el comportamiento resultante de la operación de copia para diferentes combinaciones de valores recursive y copyBehavior.

| recursive | copyBehavior | Estructura de carpetas de origen | Destino resultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la misma estructura que el origen:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5. |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 + File3 + File4 + File 5 se combina en un archivo con un nombre de archivo generado automáticamente |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Nombre de archivo generado automáticamente para File2<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;File5 | La carpeta de destino Folder1 se crea con la estructura siguiente:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;El contenido de File1 + File2 se combina en un archivo con un nombre de archivo generado automáticamente. Nombre de archivo generado automáticamente para File1<br/><br/>No se selecciona la subcarpeta Subfolder1, que contiene los archivos File3, File4 y File5. |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.

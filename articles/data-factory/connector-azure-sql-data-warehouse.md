---
title: Copia de datos en y desde Azure SQL Data Warehouse mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde cualquier almacén de origen compatible en Azure SQL Data Warehouse o desde SQL Data Warehouse en cualquier almacén de receptores compatible mediante Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: jingwang
ms.openlocfilehash: 5ce838897370430c388d74c3d356497f16efdc8d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245065"
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Copia de datos con Azure SQL Data Warehouse como origen o destino mediante Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Versión actual](connector-azure-sql-data-warehouse.md)

En este artículo se describe cómo copiar datos hacia y desde Azure SQL Data Warehouse. Para información sobre Azure Data Factory, lea el [artículo de introducción](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de blobs de Azure es compatible con las siguientes actividades:

- [Actividad de copia](copy-activity-overview.md) con [admite la matriz de origen/receptor](copy-activity-overview.md) tabla
- [Asignación de flujo de datos](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)

En concreto, este conector de Azure SQL Data Warehouse admite estas funciones:

- Copiar datos mediante la autenticación con SQL y la autenticación de tokens de aplicaciones de Azure Active Directory (Azure AD) con una entidad de servicio o identidades administradas para recursos de Azure.
- Como origen, la recuperación de datos mediante una consulta SQL o un procedimiento almacenado.
- Como receptor, cargue los datos con PolyBase o una inserción masiva. Se recomienda PolyBase para mejorar el rendimiento de copia.

> [!IMPORTANT]
> Si copia los datos mediante Azure Data Factory Integration Runtime, configure un [firewall de Azure SQL Server ](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para que los servicios de Azure puedan acceder al servidor.
> Si copia los datos mediante un IR autohospedado, configure el firewall del servidor de Azure SQL para permitir el intervalo IP apropiado. Dicho intervalo incluye la dirección IP del equipo que se utiliza para conectarse a Azure SQL Database.

## <a name="get-started"></a>Introducción

> [!TIP]
> Para obtener el mejor rendimiento posible, use PolyBase para cargar datos en Azure SQL Data Warehouse. Consulte [Movimiento de datos hacia y desde Azure SQL Data Warehouse mediante Azure Data Factory](#use-polybase-to-load-data-into-azure-sql-data-warehouse) para obtener más información. Para un tutorial con un caso de uso, consulte [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](load-azure-sql-data-warehouse.md) (Carga de 1 TB en Azure SQL Data Warehouse en 15 minutos con Azure Data Factory).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que definen las entidades de Data Factory específicas del conector de Azure SQL Data Warehouse.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con un servicio vinculado de Azure SQL Data Warehouse:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **AzureSqlDW**. | Sí |
| connectionString | Especifique la información necesaria para conectarse a la instancia de Azure SQL Data Warehouse para la propiedad **connectionString**. <br/>Marque este campo como SecureString para almacenarlo de forma segura en Data Factory. También puede colocar la contraseña o clave de la entidad de servicio en Azure Key Vault y, en el caso de la autenticación de SQL, extraer la configuración `password` de la cadena de conexión. Vea el ejemplo de JSON debajo de la tabla y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. | Sí |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarlo, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio. |
| connectVia | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Se puede usar Azure Integration Runtime o un IR autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica, se usará Azure Integration Runtime. | Sin  |

Para ver los distintos tipos de autenticación, consulte las secciones siguientes acerca de requisitos previos y ejemplos de JSON, respectivamente:

- [Autenticación de SQL](#sql-authentication)
- Autenticación de token de la aplicación de Azure AD: [Entidad de servicio](#service-principal-authentication)
- Autenticación de token de la aplicación de Azure AD: [Identidades administradas para recursos de Azure](#managed-identity)

>[!TIP]
>Si recibió un error con código de error como "UserErrorFailedToConnectToSqlServer" y un mensaje como "The session limit for the database is XXX and has been reached" (El límite de sesión de la base de datos es XXX y ya se ha alcanzado), agregue `Pooling=false` a la cadena de conexión e inténtelo de nuevo.

### <a name="sql-authentication"></a>Autenticación de SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de SQL

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Contraseña en Azure Key Vault:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Autenticación de entidad de servicio

Para usar la autenticación de tokens de aplicaciones de Azure AD basada en una entidad de servicio, realice estos pasos:

1. **[Cree una aplicación de Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** desde Azure Portal. Anote el nombre de la aplicación y los siguientes valores, que definen el servicio vinculado:

    - Identificador de aplicación
    - Clave de la aplicación
    - Id. de inquilino

2. **[Aprovisione un administrador de Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para el servidor Azure SQL Server desde Azure Portal, en caso de que aún no lo haya hecho. El administrador de Azure AD puede ser un usuario de Azure AD o un grupo de Azure AD. Si se concede al grupo con un rol de administrador de identidad administrada, omita los pasos 3 y 4. El administrador tendrá acceso total a la base de datos.

3. **[Cree usuarios de bases de datos independientes](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** para la entidad de servicio. Conéctese al almacenamiento de datos del que desea copiar datos (o en el que desea copiarlos) mediante alguna herramienta como SSMS, con una identidad de Azure AD que tenga al menos permiso para modificar cualquier usuario. Ejecute el siguiente T-SQL:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceda a la entidad de servicio los permisos necesarios**, tal como lo haría normalmente para los usuarios de SQL, u otros usuarios. Ejecute el siguiente código, o hacer referencia a más opciones [aquí](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Si desea usar PolyBase para cargar los datos, obtenga información sobre la [requiere permiso de base de datos](#required-database-permission).

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. En Azure Data Factory, **configure un servicio vinculado de Azure SQL Data Warehouse**.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de entidad de servicio

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Identidades administradas para la autenticación de recursos de Azure

Una factoría de datos se puede asociar con una [identidad administrada para recursos de Azure](data-factory-service-identity.md) que representa la factoría concreta. Puede usar esta identidad administrada para la autenticación de Azure SQL Data Warehouse. Con esta identidad, la fábrica designada puede obtener acceso y copiar datos de la base de datos o en ella.

Para usar la autenticación de identidad administrada, siga estos pasos:

1. **[Aprovisione un administrador de Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para el servidor Azure SQL Server desde Azure Portal, en caso de que aún no lo haya hecho. El administrador de Azure AD puede ser un usuario de Azure AD o un grupo de Azure AD. Si se concede al grupo con un rol de administrador de identidad administrada, omita los pasos 3 y 4. El administrador tendrá acceso total a la base de datos.

2. **[Crear usuarios de base de datos independiente](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para la identidad administrada de Data Factory. Conéctese al almacenamiento de datos del que desea copiar datos (o en el que desea copiarlos) mediante alguna herramienta como SSMS, con una identidad de Azure AD que tenga al menos permiso para modificar cualquier usuario. Ejecute el siguiente T-SQL. 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. **Conceder los permisos necesarios de la identidad administrada de Data Factory** como lo haría normalmente para los usuarios de SQL y otros. Ejecute el siguiente código, o hacer referencia a más opciones [aquí](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Si desea usar PolyBase para cargar los datos, obtenga información sobre la [requiere permiso de base de datos](#required-database-permission).

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

5. En Azure Data Factory, **configure un servicio vinculado de Azure SQL Data Warehouse**.

**Ejemplo:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que el conjunto de datos de Azure SQL Data Warehouse admite.

Para copiar datos desde o hacia Azure SQL Data Warehouse, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **AzureSqlDWTable**. | Sí |
| tableName | El nombre de la tabla o vista en la instancia de Azure SQL Data Warehouse a la que hace referencia el servicio vinculado. | No para el origen, sí para el receptor |

#### <a name="dataset-properties-example"></a>Ejemplo de propiedades de un conjunto de datos

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que el origen y el receptor de Azure SQL Data Warehouse admiten.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Azure SQL Data Warehouse como origen

Para copiar datos desde Azure SQL Data Warehouse, establezca la propiedad **type** del origen de la actividad de copia en **SqlDWSource**. La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **SqlDWSource**. | Sí |
| sqlReaderQuery | Use la consulta SQL personalizada para leer los datos. Ejemplo: `select * from MyTable`. | Sin  |
| sqlReaderStoredProcedureName | Nombre del procedimiento almacenado que lee datos de la tabla de origen. La última instrucción SQL debe ser una instrucción SELECT del procedimiento almacenado. | Sin  |
| storedProcedureParameters | Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | Sin  |

### <a name="points-to-note"></a>Puntos que se deben tener en cuenta

- Si se especifica **sqlReaderQuery** para **SqlSource**, la actividad de copia ejecuta la consulta en el origen de Azure SQL Data Warehouse para obtener los datos. O bien se puede especificar un procedimiento almacenado. Especifique **sqlReaderStoredProcedureName** y **storedProcedureParameters** si el procedimiento almacenado usa parámetros.
- Si no se especifican **sqlReaderQuery** ni **sqlReaderStoredProcedureName**, se usan las columnas que se definen en la sección **structure** del JSON del conjunto de datos para construir una consulta. `select column1, column2 from mytable` se ejecuta en Azure SQL Data Warehouse. Si la definición del conjunto de datos no tiene la sección **structure**, se seleccionan todas las columnas de la tabla.

#### <a name="sql-query-example"></a>Ejemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Ejemplo de procedimiento almacenado

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definición del procedimiento almacenado

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-data-warehouse-as-sink"></a> Azure SQL Data Warehouse como receptor

Para copiar datos en Azure SQL Data Warehouse, establezca el tipo de receptor de la actividad de copia en **SqlDWSink**. La sección **sink** de la actividad de copia admite las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del receptor de la actividad de copia debe establecerse en **SqlDWSink**. | Sí |
| allowPolyBase | Indica si se usa PolyBase, si procede, en lugar del mecanismo BULKINSERT. <br/><br/> Se recomienda usar PolyBase para cargar datos en SQL Data Warehouse. Consulte la sección [Use PolyBase to load data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) (Uso de PolyBase para cargar datos en SQL Data Warehouse) para ver restricciones y más información.<br/><br/>Los valores válidos son **True** y **False** (valor predeterminado).  | Sin  |
| polyBaseSettings | Un grupo de propiedades que se pueden especificar si el valor de la propiedad **allowPolybase** está establecido en **true**. | Sin  |
| rejectValue | Especifica el número o porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta.<br/><br/>Más información sobre las opciones de rechazo de PolyBase en la sección Argumentos de [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>Los valores permitidos son 0 (valor predeterminado), 1, 2, etc. |Sin  |
| rejectType | Especifica si la opción **rejectValue** es un valor literal o un porcentaje.<br/><br/>Los valores permitidos son **Value** (valor predeterminado) y **Percentage**. | Sin  |
| rejectSampleValue | Determina el número de filas que se van a recuperar antes de que PolyBase vuelva a calcular el porcentaje de filas rechazadas.<br/><br/>Los valores permitidos son 1, 2, etc. | Sí, si el valor de **rejectType** es **percentage**. |
| useTypeDefault | Especifica cómo administrar los valores que faltan en archivos de texto delimitados cuando PolyBase recupera datos del archivo de texto.<br/><br/>Más información sobre esta propiedad en la sección de argumentos de [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Los valores válidos son **True** y **False** (valor predeterminado).<br><br>**Consulte [sugerencias para solucionar problemas](#polybase-troubleshooting) relacionada con esta configuración.** | No |
| writeBatchSize | Número de filas que se inserta en la tabla SQL **por lote**. Se aplica únicamente cuando no se usa PolyBase.<br/><br/>El valor que se permite es un **entero** (número de filas). De forma predeterminada, Data Factory determinar dinámicamente el tamaño del lote adecuado en función del tamaño de fila. | Sin  |
| writeBatchTimeout | Tiempo que se concede a la operación de inserción por lotes para que finalice antes de que se agote el tiempo de espera. Se aplica únicamente cuando no se usa PolyBase.<br/><br/>El valor permitido es **intervalo de tiempo**. Ejemplo: "00:30:00" (30 minutos). | Sin  |
| preCopyScript | Especifique una consulta SQL para que la actividad de copia se ejecute antes de escribir datos en Azure SQL Data Warehouse en cada ejecución. Esta propiedad se usa para limpiar los datos cargados previamente. | Sin  |

#### <a name="sql-data-warehouse-sink-example"></a>Ejemplo de receptor de SQL Data Warehouse

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

En la siguiente sección obtendrá más información sobre cómo usar PolyBase para cargar en SQL Data Warehouse eficazmente.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Movimiento de datos hacia y desde Azure SQL Data Warehouse mediante Azure Data Factory

Usar [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) es una manera eficaz de cargar grandes cantidades de datos en Azure SQL Data Warehouse con un alto rendimiento. Verá una gran mejora en el rendimiento mediante el uso de PolyBase en lugar del mecanismo BULKINSERT predeterminado. Consulte [Performance reference](copy-activity-performance.md#performance-reference) (Referencia de rendimiento) para obtener una comparación detallada. Para un tutorial con un caso de uso, consulte [Load 1 TB into Azure SQL Data Warehouse](v1/data-factory-load-sql-data-warehouse.md) (Carga de 1 TB en Azure SQL Data Warehouse).

* Si los datos de origen están en **Azure Data Lake Storage Gen1, Gen2 de almacenamiento de Azure Data Lake o Azure Blob**y el **formato es compatible con PolyBase**, puede usar la actividad de copia para invocar directamente PolyBase para permitir que Azure SQL Data Warehouse extraer los datos de origen. Consulte **[Copia directa con PolyBase](#direct-copy-by-using-polybase)** para obtener detalles.
* Si el formato y el almacenamiento de datos de origen no es compatible originalmente con PolyBase, use en su lugar la característica **[Copia almacenada provisionalmente con PolyBase](#staged-copy-by-using-polybase)** . La característica de copia almacenada provisionalmente también proporciona un mejor rendimiento. Convierte automáticamente los datos a formato compatible con PolyBase. Y almacena los datos en Azure Blob Storage. Luego los carga en SQL Data Warehouse.

>[!TIP]
>Obtenga más información en [procedimientos recomendados para el uso de PolyBase](#best-practices-for-using-polybase).

### <a name="direct-copy-by-using-polybase"></a>Copia directa con PolyBase

PolyBase en SQL Data Warehouse admite directamente Azure Blob, Azure Data Lake Storage Gen1 y Gen2 de almacenamiento de Azure Data Lake. Si los datos de origen cumplen los criterios descritos en esta sección, use PolyBase para copiar directamente desde el almacén de datos de origen a Azure SQL Data Warehouse. De lo contrario, use [Copia almacenada provisionalmente con PolyBase](#staged-copy-by-using-polybase).

> [!TIP]
> Para copiar datos de forma eficaz en SQL Data Warehouse, obtenga más información en [Azure Data Factory hace incluso más fácil y cómodo el descubrimiento de conocimiento a partir de datos cuando se usa Data Lake Store con SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Si no se cumplen los requisitos, Azure Data Factory comprobará la configuración y volverá automáticamente al mecanismo BULKINSERT para realizar el movimiento de datos.

1. El **servicio vinculado de origen** es con los siguientes tipos y métodos de autenticación:

    | Tipo de almacén de datos de origen compatible | Admite el tipo de autenticación de origen |
    |:--- |:--- |
    | [Azure Blob](connector-azure-blob-storage.md) | Autenticación de clave de cuenta y autenticación de identidad administrada |
    | [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | Autenticación de entidad de servicio |
    | [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | Autenticación de clave de cuenta y autenticación de identidad administrada |

    >[!IMPORTANT]
    >Si el almacenamiento de Azure está configurado con el punto de conexión de servicio de red virtual, debe utilizar la autenticación de identidad administrada. Consulte [impacto del uso de puntos de conexión de servicio de red virtual con almacenamiento de Azure](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)

2. El **formato de datos de origen** es de **Parquet**, **ORC**, o **texto delimitado**, con las siguientes configuraciones:

   1. Ruta de acceso de carpeta no contienen el filtro de comodín.
   2. Nombre de archivo señala a un único archivo o es `*` o `*.*`.
   3. `rowDelimiter` debe ser **\n**.
   4. `nullValue` se establece en **cadena vacía** ("") o se deja como valor predeterminado, y `treatEmptyAsNull` se deja como valor predeterminado o se establece en true.
   5. `encodingName` se establece en **utf-8**, que es el valor predeterminado.
   6. `quoteChar`, `escapeChar`, y `skipLineCount` no se especifican. Fila de encabezado de omisión de compatibilidad de PolyBase que se puede configurar como `firstRowAsHeader` en ADF.
   7. `compression` puede ser **no compression**, **GZip** o **Deflate**.

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>Copia almacenada provisionalmente con PolyBase

Si los datos de origen no cumplen los criterios de la sección anterior, habilite la copia de datos a través de una instancia de Azure Blob Storage de almacenamiento provisional interino. No puede ser Azure Premium Storage. En este caso, Azure Data Factory ejecuta automáticamente transformaciones de datos para satisfacer los requisitos del formato de datos de PolyBase. A continuación, se usa PolyBase para cargar datos en SQL Data Warehouse. Por último, limpia los datos temporales del almacenamiento de blobs. Consulte [Copia almacenada provisionalmente](copy-activity-performance.md#staged-copy) para más información sobre la copia de datos por medio de una instancia de Azure Blob Storage de almacenamiento provisional.

Para utilizar esta característica, cree un [servicio vinculado de Azure Storage](connector-azure-blob-storage.md#linked-service-properties) que haga referencia a la cuenta de Azure Storage con el almacenamiento de blobs interino. Luego especifique las propiedades `enableStaging` y `stagingSettings` para la actividad de copia, tal como se muestra en el código siguiente:

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-for-using-polybase"></a>Prácticas recomendadas para usar PolyBase

En las secciones siguientes se proporcionan procedimientos recomendados además de los que se mencionan en [Procedimientos recomendados para Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Permiso de base de datos necesario

Para usar PolyBase, el usuario que carga los datos en SQL Data Warehouse debe tener el [permiso "CONTROL"](https://msdn.microsoft.com/library/ms191291.aspx) en la base de datos de destino. Una manera de conseguirlo es agregar el usuario como miembro del rol **db_owner**. Obtenga información sobre cómo hacerlo en la [información general de SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Límites del tipo de datos y del tamaño de fila

Las cargas de PolyBase están limitadas a filas más pequeñas que 1 MB. No se puede usar para cargar en varchr, nvarchar (max) o varbinary (max). Para más información, consulte [Límites de capacidad de SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Cuando los datos de origen tienen filas mayores de 1 MB, es aconsejable dividir verticalmente las tablas de origen en otras más pequeñas. Asegúrese de que el tamaño mayor de cada fila no excede el límite. Las tablas más pequeñas se pueden cargar con PolyBase y combinar en Azure SQL Data Warehouse.

Como alternativa, para los datos con estas columnas anchas, puede usar no PolyBase para cargar los datos de uso de ADF, si se desactiva "Permitir PolyBase" configuración.

### <a name="polybase-troubleshooting"></a>Solución de problemas de PolyBase

**Carga a una columna Decimal**

Si los datos de origen están en formato de texto que contiene un valor vacío para cargarse en una columna Decimal de SQL Data Warehouse, puede alcanzar el siguiente error:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

La solución consiste en anular la selección de "**predeterminado del tipo de uso**" opción (como false) en el receptor de la actividad de copia -> configuración de PolyBase. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)" es una configuración nativa de PolyBase que especifica cómo controlar los valores que faltan en archivos de texto delimitado cuando PolyBase recupera datos del archivo de texto. 

**Otros**

Para más knonw problemas de PolyBase, consulte [carga de solución de problemas de Azure SQL Data Warehouse PolyBase](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md#polybase).

### <a name="sql-data-warehouse-resource-class"></a>Clase de recursos de SQL Data Warehouse

Para obtener el mejor rendimiento posible, asigne una clase de recurso mayor al usuario que carga datos en SQL Data Warehouse a través de PolyBase.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** en Azure SQL Data Warehouse

En la siguiente tabla se proporcionan ejemplos de cómo especificar la propiedad **tableName** en el conjunto de datos JSON. Se muestran varias combinaciones de esquema y nombres de tabla.

| Esquema de base de datos | Nombre de tabla | Propiedad JSON **tableName** |
| --- | --- | --- |
| dbo | MyTable | MyTable o dbo.MyTable o [dbo].[MyTable] |
| dbo1 | MyTable | dbo1.MyTable o [dbo1].[MyTable] |
| dbo | My.Table | [My.Table] o [dbo].[My.Table] |
| dbo1 | My.Table | [dbo1].[My.Table] |

Si ve el siguiente error, el problema podría ser el valor especificado para la propiedad **tableName**. Consulte en la tabla anterior la forma correcta de especificar los valores para la propiedad **tableName** de JSON.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Columnas con valores predeterminados

Actualmente, la característica PolyBase en Data Factory solo acepta el mismo número de columnas que la tabla de destino. Un ejemplo sería una tabla con cuatro columnas y que una de ellas esté definida con un valor predeterminado. Los datos de entrada siguen necesitando cuatro columnas. Un conjunto de datos de entrada de tres columnas producirá un error parecido al siguiente mensaje:

```
All columns of the table must be specified in the INSERT BULK statement.
```

El valor NULL es una forma especial del valor predeterminado. Si la columna acepta valores nulos, los datos de entrada del blob para esa columna pueden estar vacíos. Pero no puede faltar del conjunto de datos de entrada. PolyBase insertará valores NULL para los valores que falten en Azure SQL Data Warehouse.

## <a name="mapping-data-flow-properties"></a>Propiedades de flujo de datos de asignación

Obtenga información detallada de [transformación del origen](data-flow-source.md) y [receptor transformación](data-flow-sink.md) en asignación de flujo de datos.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Asignación de tipos de datos para Azure SQL Data Warehouse

Al copiar datos de una instancia de Azure SQL Data Warehouse o en ella, se utilizan las siguientes asignaciones de tipos de datos de Azure SQL Data Warehouse en los tipos de datos provisionales de Azure Data Factory. Para obtener información acerca de la forma en que la actividad de copia asigna el esquema de origen y el tipo de datos al receptor, consulte [Asignación de esquemas en la actividad de copia](copy-activity-schema-and-type-mapping.md).

>[!TIP]
>Hacer referencia a [tabla de tipos de datos en Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md) artículo sobre SQL Data Warehouse admite los tipos de datos y las soluciones alternativas para no los admite.

| Tipo de datos de Azure SQL Data Warehouse | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| bigint | Int64 |
| binary | Byte[] |
| bit | Boolean |
| char | String, Char[] |
| date | DateTime |
| DateTime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| FILESTREAM attribute (varbinary(max)) | Byte[] |
| Float | Double |
| image | Byte[] |
| int | Int32 |
| money | Decimal |
| nchar | String, Char[] |
| numeric | Decimal |
| nvarchar | String, Char[] |
| real | Single |
| rowversion | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | Decimal |
| time | TimeSpan |
| tinyint | Byte |
| uniqueidentifier | Guid |
| varbinary | Byte[] |
| varchar | String, Char[] |

## <a name="next-steps"></a>Pasos siguientes
Consulte los [formatos y almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver una lista de los almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.

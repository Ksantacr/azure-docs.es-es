---
title: Trabajo con fechas en Azure Cosmos DB
description: Aprenda a trabajar con fechas en Azure Cosmos DB.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 99a807cf3918a8768231f88f846432df18fea9e8
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977233"
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Trabajo con fechas en Azure Cosmos DB
Azure Cosmos DB proporciona flexibilidad de esquema e indexación completa mediante un modelo de datos [JSON](https://www.json.org) nativo. Todos los recursos de Azure Cosmos DB, incluidas las bases de datos, los contenedores, los documentos y los procedimientos almacenados, están modelados y almacenados como documentos JSON. Como requisito para ser portátil, JSON (y Azure Cosmos DB) admite solo un pequeño conjunto de tipos básicos: String, Number, Boolean, Array, Object y Null. Sin embargo, JSON es flexible y permite que los desarrolladores y marcos de trabajo representen tipos más complejos mediante estos primitivos y los compongan como objetos o matrices. 

Además de los tipos básicos, muchas aplicaciones necesitan el tipo [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) para representar fechas y marcas de tiempo. En este artículo se explica cómo los desarrolladores pueden almacenar, recuperar y consultar fechas en Azure Cosmos DB mediante el SDK de .NET.

## <a name="storing-datetimes"></a>Almacenamiento de valores DateTime
De forma predeterminada, el [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md) serializa los valores DateTime como cadenas [ISO 8601](https://www.iso.org/iso/catalogue_detail?csnumber=40874). La mayoría de las aplicaciones pueden la representación de cadena predeterminada para DateTime por los siguientes motivos:

* Las cadenas se pueden comparar, y se conserva el orden relativo de los valores DateTime cuando se transforman en cadenas. 
* Este enfoque no requiere ninguna personalización del código o de los atributos para la conversión de JSON.
* Las fechas almacenadas en JSON son legibles para el usuario.
* Este enfoque puede aprovechar el índice de Azure Cosmos DB para aumentar el rendimiento de las consultas.

Por ejemplo, el fragmento de código siguiente almacena un objeto `Order` que contiene dos propiedades de DateTime: `ShipDate` y `OrderDate` como un documento mediante el SDK de .NET:

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

Este documento se almacena en Azure Cosmos DB de la manera siguiente:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Como alternativa, puede almacenar valores DateTime como marcas de tiempo de Unix, es decir, como un número que representa el número de segundos transcurridos desde el 1 de enero de 1970. La propiedad Timestamp (`_ts`) interna de Azure Cosmos DB sigue este enfoque. Puede usar la clase [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) clase para serializar los valores DateTime como números. 

## <a name="indexing-datetimes-for-range-queries"></a>Indexación de valores DateTime para consultas de rango
Las consultas de rango son comunes con valores DateTime. Por ejemplo, si necesita encontrar todos los pedidos que se crearon desde ayer o todos los pedidos enviados en los últimos cinco minutos, debe realizar las consultas de rango. Para ejecutar estas consultas de forma eficaz, debe configurar la colección para la indización de rangos en cadenas.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Puede aprender más sobre cómo configurar directivas de indexación en [¿Cómo funcionan los datos del índice de Azure Cosmos DB?](index-policy.md)

## <a name="querying-datetimes-in-linq"></a>Consulta de valores DateTimes en LINQ
El SDK de .NET para SQL admite automáticamente la consulta de datos almacenados en Azure Cosmos DB mediante LINQ. Por ejemplo, el fragmento de código siguiente muestra una consulta LINQ que ordena los filtros que se enviaron en los últimos tres días.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

Puede aprender más sobre el lenguaje de consulta SQL de Azure Cosmos DB y el proveedor LINQ en [Consulta de Cosmos DB](how-to-sql-query.md).

En este artículo se explica cómo almacenar, indexar y consultar valores DateTime en Azure Cosmos DB.

## <a name="next-steps"></a>Pasos siguientes
* Descargue y ejecute los [ejemplos de código en GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples).
* Obtener más información acerca de las [consultas de SQL](how-to-sql-query.md)
* Obtener más información sobre [Directivas de indexación de Azure Cosmos DB](index-policy.md)

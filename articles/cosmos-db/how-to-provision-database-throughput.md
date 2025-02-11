---
title: Aprovisionamiento del rendimiento de base de datos en Azure Cosmos DB
description: Aprenda a aprovisionar el rendimiento en el nivel de base de datos en Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: d73dd5ffe8cc8ed00288209b628d7175b795b335
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243768"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Aprovisionamiento del rendimiento en una base de datos de Azure Cosmos DB

En este artículo se explica cómo aprovisionar el rendimiento en una base de datos de Azure Cosmos DB. Puede aprovisionar el rendimiento de un único [contenedor](how-to-provision-container-throughput.md), o bien de una base de datos y compartir el rendimiento entre los contenedores que se incluyen en ella. Para saber cuándo se debe usar el rendimiento de nivel de contenedor y de nivel de base de datos, consulte el artículo [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md). Para aprovisionar el rendimiento en el nivel de base de datos, se puede usar Azure Portal o los SDK de Azure Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Aprovisionamiento del rendimiento mediante Azure Portal

### <a id="portal-sql"></a>SQL API (Core)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una cuenta de Azure Cosmos](create-sql-api-dotnet.md#create-account) o seleccione una ya existente.

1. Abra el panel de **Data Explorer** y seleccione **Nueva base de datos**. Especifique los detalles siguientes:

   * Escriba un identificador de base de datos. 
   * Seleccione **Aprovisionar rendimiento**.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Nueva base de datos](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Aprovisionamiento del rendimiento mediante el SDK para .NET

> [!Note]
> Puede usar los SDK de Cosmos para SQL API para aprovisionar el rendimiento de todas las API. También puede usar el ejemplo siguiente para Cassandra API.

### <a id="dotnet-all"></a>Todas las API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener más información sobre el rendimiento aprovisionado en Azure Cosmos DB:

* [Escalado global del rendimiento aprovisionado](scaling-throughput.md)
* [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md)
* [How to provision throughput for a container](how-to-provision-container-throughput.md) (Aprovisionamiento del rendimiento de un contenedor)
* [Rendimiento y unidades de solicitud en Azure Cosmos DB](request-units.md)
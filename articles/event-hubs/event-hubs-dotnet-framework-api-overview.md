---
title: Introducción a las API de .NET Framework de Azure Event Hubs | Microsoft Docs
description: Resumen de algunas de las principales API de cliente de .NET Framework de Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: f56882d2d73c85434c6f81972a06e5ea9ccdff99
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822042"
---
# <a name="event-hubs-net-framework-api-overview"></a>Introducción a las API de .NET Framework de Event Hubs

Este artículo resume algunas de las principales [API de cliente de .NET Framework](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) de Azure Event Hubs. Existen dos categorías: API de administración y de tiempo de ejecución. Las API de tiempo de ejecución están compuestas por todas las operaciones necesarias para enviar y recibir un mensaje. Las operaciones de administración le permiten administrar un estado de entidad de Event Hubs mediante la creación, actualización y eliminación de entidades.

Los [escenarios de supervisión](event-hubs-metrics-azure-monitor.md) abarcan la administración y el entorno de tiempo de ejecución. Para obtener documentación de referencia detallada sobre las API de .NET, consulte las referencias [.NET Framework](/dotnet/api/microsoft.servicebus.messaging.eventhubclient), [.NET Standard](/dotnet/api/microsoft.azure.eventhubs) y [API de EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor).

## <a name="management-apis"></a>API de administración

Para realizar las siguientes operaciones de administración debe tener permisos de **administración** en el espacio de nombres de Event Hubs:

### <a name="create"></a>Crear

```csharp
// Create the event hub
var ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
await namespaceManager.CreateEventHubAsync(ehd);
```

### <a name="update"></a>Actualizar

```csharp
var ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
var ruleName = "myeventhubmanagerule";
var ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
await namespaceManager.UpdateEventHubAsync(ehd);
```

### <a name="delete"></a>Eliminar

```csharp
await namespaceManager.DeleteEventHubAsync("event hub name");
```

## <a name="run-time-apis"></a>API de tiempo de ejecución
### <a name="create-publisher"></a>Crear publicador

```csharp
// EventHubClient model (uses implicit factory instance, so all links on same connection)
var eventHubClient = EventHubClient.Create("event hub name");
```

### <a name="publish-message"></a>Publicar mensaje

```csharp
// Create the device/temperature metric
var info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
var data = new EventData(new byte[10]); // Byte array
var data = new EventData(Stream); // Stream 
var data = new EventData(info, serializer) //Object and serializer 
{
    PartitionKey = info.DeviceId.ToString()
};

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Crear consumidor

```csharp
// Create the Event Hubs client
var eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
var defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index);

// From one day ago
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));

// From specific offset, -1 means oldest
var consumer = await defaultConsumerGroup.CreateReceiverAsync(partitionId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Consumir mensaje

```csharp
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)

// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>API de host del procesador de eventos

Estas API ofrecen resistencia a los procesos de trabajo que pueden dejar de estar disponibles, distribuyendo particiones entre trabajos disponibles.

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

var eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
var blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

var eventHubDescription = new EventHubDescription(EventHubName);
var host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
await host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
await host.UnregisterEventProcessorAsync();
```

La interfaz [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) se define de la manera siguiente:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            // Process messages here
        }

        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }

    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los escenarios de Event Hubs, visite estos vínculos:

* [¿Qué es Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Guía de programación de Event Hubs](event-hubs-programming-guide.md)

A continuación se incluyen referencias de API de .NET:

* [Microsoft.ServiceBus.Messaging](/dotnet/api/microsoft.servicebus.messaging)
* [Microsoft.Azure.EventHubs.EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)

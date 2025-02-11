---
title: Supervisión del mantenimiento de Azure IoT Hub | Microsoft Docs
description: Uso de Azure Monitor y Azure Resource Health para supervisar IoT Hub y diagnosticar problemas rápidamente
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: kgremban
ms.openlocfilehash: 6dea1add1e329cfc894068732898a856a69c9b4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66166208"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Supervisión del mantenimiento de Azure IoT Hub y diagnóstico de problemas rápidamente

Las empresas que implementan Azure IoT Hub esperan que sus recursos ofrezcan un rendimiento confiable. Para ayudarle a mantener una estrecha vigilancia de las operaciones, IoT Hub está totalmente integrado con [Azure Monitor](../azure-monitor/index.yml) y [Azure Resource Health](../service-health/resource-health-overview.md). Estos dos servicios trabajan para proporcionarle los datos que necesita para mantener las soluciones de IoT y que se ejecuta en un estado correcto.

Azure Monitor es un único origen para supervisar y registrar todos los servicios de Azure. Puede enviar los registros de diagnóstico que genera Azure Monitor para los registros de Azure Monitor, Event Hubs o Azure Storage para el procesamiento personalizado. La configuración de métricas y diagnósticos de Azure Monitor ofrece una visibilidad del rendimiento de los recursos. Continúe leyendo este artículo para obtener información sobre cómo [usar Azure Monitor](#use-azure-monitor) con IoT Hub. 

> [!IMPORTANT]
> No se garantiza que los eventos que emite el servicio IoT Hub mediante registros de diagnóstico de Azure Monitor sean confiables u ordenados. Algunos eventos podrían perderse o entregarse fuera del pedido. Los registros de diagnóstico tampoco están diseñados para que tengan lugar en tiempo real y los eventos pueden tardar varios minutos en registrarse en el destino elegido.

Azure Resource Health ayuda a diagnosticar si un problema de Azure afecta a los recursos y, en su caso, a obtener soporte técnico. Un panel proporciona el estado actual y pasado para cada uno de los centros de IoT. Continúe con la sección en la parte inferior de este artículo para obtener información sobre cómo [usar Azure Resource Health](#use-azure-resource-health) con IoT hub. 

IoT Hub también proporciona sus propias métricas que puede utilizar para consultar el estado de los recursos de IoT. Para obtener más información, consulte [métricas de IoT Hub de comprender](iot-hub-metrics.md).

## <a name="use-azure-monitor"></a>Uso de Azure Monitor

Azure Monitor proporciona información de diagnóstico para los recursos de Azure, lo que significa que puede supervisar las operaciones que tienen lugar dentro del centro de IoT.

La configuración de diagnóstico de Azure Monitor reemplaza al monitor de operaciones de IoT Hub. Si usa la supervisión de operaciones actualmente, debe migrar los flujos de trabajos. Para obtener más información, consulte [migrar desde las operaciones de configuración de supervisión para diagnósticos](iot-hub-migrate-to-diagnostics-settings.md).

Para obtener más información acerca de las métricas específicas y eventos que supervisa el Monitor de Azure, consulte [métricas compatibles con Azure Monitor](../azure-monitor/platform/metrics-supported.md) y [compatible de servicios, esquemas y categorías para los registros de diagnóstico de Azure](../azure-monitor/platform/diagnostic-logs-schema.md).

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Descripción de los registros

Azure Monitor supervisa diferentes operaciones que tienen lugar en IoT Hub. Cada categoría tiene un esquema que define cómo se notifican los eventos de la categoría de que se trate.

#### <a name="connections"></a>Conexiones

La categoría de conexiones realiza un seguimiento de eventos de conexión y desconexión de dispositivos desde un centro de IoT, y también de los errores. Esta categoría es útil para identificar intentos de conexión no autorizados o alertas cuando se pierde la conexión a dispositivos.

> [!NOTE]
> Comprobación de estado de conexión confiable de los dispositivos [latido de dispositivo](iot-hub-devguide-identity-registry.md#device-heartbeat).

```json
{
   "records":
   [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>Comandos de nube a dispositivo

La categoría de comandos de nube a dispositivo realiza el seguimiento de los errores que se producen en el centro de IoT y se relacionan con la canalización de mensajes de nube a dispositivo. Esta categoría incluye errores que se producen a partir de:

* Envío de mensajes de la nube al dispositivo (como errores de remitente no autorizados),
* Recepción de mensajes de la nube al dispositivo (por ejemplo, errores al superarse el recuento de entregas) y
* Recepción de comentarios de mensajes de la nube al dispositivo (como errores por comentarios expirados).

Esta categoría no detecta errores cuando el mensaje de la nube al dispositivo se entrega correctamente, pero el dispositivo lo controla incorrectamente.

```json
{
    "records":
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "messageExpired",
            "category": "C2DCommands",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddress\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-identity-operations"></a>Operaciones de identidad de dispositivos

La categoría de operaciones de identidad de dispositivo supervisa los errores que se producen cuando se intenta crear, actualizar o eliminar una entrada en el registro de identidades de IoT Hub. El seguimiento de esta categoría resulta útil para los escenarios de aprovisionamiento.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "get",
            "category": "DeviceIdentityOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="routes"></a>Rutas

La categoría de enrutamiento de mensajes realiza un seguimiento de los errores que se producen durante la evaluación de este proceso y el estado del punto de conexión según lo que observa IoT Hub. Esta categoría incluye eventos tales como:

* Una regla se evalúa como "sin definir",
* IoT Hub marca un punto de conexión como fallido, o
* Cualquier error recibido desde un punto de conexión. 

Esta categoría no incluye errores específicos de los mensajes (como los errores de limitación del dispositivo), que se notifican en la categoría de telemetría de dispositivo.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "endpointUnhealthy",
            "category": "Routes",
            "level": "Error",
            "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-telemetry"></a>Telemetría de dispositivo

La categoría de telemetría del dispositivo realiza el seguimiento de los errores que se producen en el centro de IoT y se relacionan con la canalización de telemetría. Esta categoría incluye los errores que se producen al enviar eventos de telemetría (por ejemplo, la limitación) y recibir eventos de telemetría (por ejemplo, un lector no autorizado). Esta categoría no puede detectar los errores causados por el código que se ejecuta en el propio dispositivo.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "DeviceTelemetry",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="file-upload-operations"></a>Operaciones de carga de archivos

La categoría de carga de archivos supervisa los errores que se producen en el centro de IoT y está relacionada con la funcionalidad de carga de archivos. Esta categoría incluye lo siguiente:

* Errores que se producen con el URI de SAS, como cuando caduca antes de que un dispositivo notifique al centro una carga completada.

* Cargas con errores notificadas por el dispositivo.

* Errores que se producen cuando no se encuentra un archivo en el almacenamiento durante la creación del mensaje de notificación de IoT Hub.

Esta categoría no puede detectar los errores que se producen directamente mientras el dispositivo está cargando un archivo a Storage.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "FileUploadOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>Operaciones gemelas de la nube al dispositivo

La categoría de operaciones gemelas de la nube al dispositivo supervisa los eventos iniciados por el servicio en dispositivos gemelos. Estas operaciones pueden incluir la opción de obtener operaciones gemelas y actualizar o reemplazar etiquetas y propiedades.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "read",
            "category": "C2DTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>Operaciones gemelas del dispositivo a la nube

La categoría de operaciones gemelas del dispositivo a la nube supervisa los eventos iniciados por el dispositivo en dispositivos gemelos. Estas operaciones pueden incluir la opción de obtener operaciones gemelas, actualizar las propiedades notificadas y suscribirse a las propiedades deseadas.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "update",
            "category": "D2CTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="twin-queries"></a>Consultas gemelas

La categoría de consultas gemelas informa sobre las solicitudes de consulta de los dispositivos gemelos que se inician en la nube.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "query",
            "category": "TwinQueries",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="jobs-operations"></a>Operaciones de trabajos

La categoría de operaciones de trabajos informa sobre las solicitudes de trabajo para actualizar los dispositivos gemelos o invocar métodos directos en varios dispositivos. Estas solicitudes se inician en la nube.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "jobCompleted",
            "category": "JobsOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="direct-methods"></a>Métodos directos

La categoría de métodos directos realiza un seguimiento de las interacciones de solicitud y respuesta enviadas a dispositivos individuales. Estas solicitudes se inician en la nube.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "send",
            "category": "DirectMethods",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":<messageSize>, \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="distributed-tracing-preview"></a>Seguimiento distribuido (versión preliminar)

La categoría de seguimiento distribuido realiza un seguimiento de los identificadores de correlación correspondientes a los mensajes que contienen el encabezado de contexto de seguimiento. Para habilitar estos registros, el código de cliente debe actualizarse siguiendo [analizar y diagnosticar IoT aplicaciones-to-end con seguimiento distribuido de IoT Hub (versión preliminar)](iot-hub-distributed-tracing.md).

Tenga en cuenta que `correlationId` se ajusta a la [contexto de seguimiento de W3C](https://github.com/w3c/trace-context) propuesta, que contiene un `trace-id` , así como un `span-id`.

##### <a name="iot-hub-d2c-device-to-cloud-logs"></a>Registros D2C (dispositivo a nube) de IoT Hub

IoT Hub anota este registro cuando le llega un mensaje que contiene propiedades de seguimiento válidas.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubD2C",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Receive message success",
            "durationMs": "",
            "properties": "{\"messageSize\": 1, \"deviceId\":\"<deviceId>\", \"callerLocalTimeUtc\": : \"2017-02-22T03:27:28.633Z\", \"calleeLocalTimeUtc\": \"2017-02-22T03:27:28.687Z\"}",
            "location": "Resource location"
        }
    ]
}
```

En este caso, no se calcula `durationMs` dado que es posible que el reloj de IoT Hub no esté sincronizado con el reloj del dispositivo y, por tanto, el cálculo de la duración puede ser engañoso. Se recomienda escribir la lógica mediante las marcas de tiempo de la sección `properties` para capturar los picos en la latencia de dispositivo a nube.

| Propiedad | Escriba | DESCRIPCIÓN |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **messageSize** | Integer | Tamaño del mensaje de dispositivo a nube en bytes. |
| **deviceId** | Cadena de caracteres alfanuméricos ASCII de 7 bits. | Identidad del dispositivo |
| **callerLocalTimeUtc** | Marca de tiempo UTC | Hora de creación del mensaje según el reloj local del dispositivo. |
| **calleeLocalTimeUtc** | Marca de tiempo UTC | Hora de llegada del mensaje a la puerta de enlace de IoT Hub según el reloj del lado de servicio de IoT Hub. |

##### <a name="iot-hub-ingress-logs"></a>Registros de entrada de IoT Hub

IoT Hub anota este registro cuando un mensaje que contiene propiedades de seguimiento válidas se escribe en un centro de eventos interno o integrado.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubIngress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Ingress message success",
            "durationMs": "10",
            "properties": "{\"isRoutingEnabled\": \"true\", \"parentSpanId\":\"0144d2590aacd909\"}",
            "location": "Resource location"
        }
    ]
}
```

En la `properties` sección, este registro contiene información adicional sobre la entrada de mensajes.

| Propiedad | Escriba | DESCRIPCIÓN |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **isRoutingEnabled** | String | Verdadero o falso, indica si está habilitado el enrutamiento de mensajes en IoT Hub. |
| **parentSpanId** | String | El valor de [span-id](https://w3c.github.io/trace-context/#parent-id) del mensaje principal, que, en este caso, sería el seguimiento de mensajes D2C. |

##### <a name="iot-hub-egress-logs"></a>Registros de salida de IoT Hub

IoT Hub anota este registro cuando está habilitado el [enrutamiento](iot-hub-devguide-messages-d2c.md) y el mensaje se escribe en un [punto de conexión](iot-hub-devguide-endpoints.md). Si no está habilitado el enrutamiento, IoT Hub no anota este registro.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubEgress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-98ac3578922acd26-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Egress message success",
            "durationMs": "10",
            "properties": "{\"endpointType\": \"EventHub\", \"endpointName\": \"myEventHub\", \"parentSpanId\":\"349810a9bbd28730\"}",
            "location": "Resource location"
        }
    ]
}
```

En la `properties` sección, este registro contiene información adicional sobre la entrada de mensajes.

| Propiedad | Escriba | DESCRIPCIÓN |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **endpointName** | String | Nombre del punto de conexión de enrutamiento. |
| **endpointType** | String | Tipo del punto de conexión de enrutamiento. |
| **parentSpanId** | String | El valor de [span-id](https://w3c.github.io/trace-context/#parent-id) del mensaje principal, que, en este caso, sería el seguimiento de mensajes de entrada de IoT Hub. |

### <a name="read-logs-from-azure-event-hubs"></a>Lectura de registros de Azure Event Hubs

Una vez configurados los registros mediante la configuración de diagnóstico, puede crear aplicaciones que lean los registros, a fin de poder actuar en función de la información que contienen. Este código de ejemplo recupera registros de un centro de eventos:

```csharp
class Program
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}";
    static string monitoringEndpointName = "{your AMS event hub endpoint name}";
    static EventHubClient eventHubClient;
    //This is the Diagnostic Settings schema
    class AzureMonitorDiagnosticLog
    {
        string time { get; set; }
        string resourceId { get; set; }
        string operationName { get; set; }
        string category { get; set; }
        string level { get; set; }
        string resultType { get; set; }
        string resultDescription { get; set; }
        string durationMs { get; set; }
        string callerIpAddress { get; set; }
        string correlationId { get; set; }
        string identity { get; set; }
        string location { get; set; }
        Dictionary<string, string> properties { get; set; }
    };

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }
        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));
            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
                var deserializer = new JavaScriptSerializer();
                //deserialize json data to azure monitor object
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result);
            }
        }
    }
}
```

## <a name="use-azure-resource-health"></a>Uso de Azure Resource Health

Use Azure Resource Health para supervisar si IoT Hub está en funcionamiento. También permite saber si una interrupción regional está afectando al mantenimiento de IoT Hub. Para conocer los detalles específicos sobre el estado de mantenimiento de Azure IoT Hub, se recomienda [utilizar Azure Monitor](#use-azure-monitor).

Azure IoT Hub indica el estado de mantenimiento a nivel regional. Si una interrupción regional afecta al centro de IoT, el estado de mantenimiento aparece como **Desconocido**. Para obtener más información, consulte [tipos de recursos y comprobaciones de estado en Azure resource health](../service-health/resource-health-checks-resource-types.md).

Para comprobar el mantenimiento de los recursos de IoT Hub, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Vaya a **Estado del servicio** > **Resource Health**.

3. En los cuadros de lista desplegable, seleccione su suscripción, a continuación, seleccione **IoT Hub** como el tipo de recurso.

Para obtener más información acerca de cómo interpretar los datos de estado, vea [información general sobre el estado de recursos de Azure](../service-health/resource-health-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Descripción de las métricas de IoT Hub](iot-hub-metrics.md)
* [Supervisión remota de IoT y notificaciones con Azure Logic Apps conectando IoT hub y el buzón de correo](iot-hub-monitoring-notifications-with-azure-logic-apps.md)
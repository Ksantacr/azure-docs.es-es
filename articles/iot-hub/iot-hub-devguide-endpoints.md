---
title: Información de los puntos de conexión de Azure IoT Hub | Microsoft Docs
description: 'Guía para desarrolladores: información de referencia sobre los puntos de conexión orientados a dispositivos y servicios IoT Hub.'
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: fe913f057d00fd18b1b163f124d9dd0b83abf0de
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925895"
---
# <a name="reference---iot-hub-endpoints"></a>Referencia: Puntos de conexión de IoT Hub

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>Nombres de IoT Hub

Puede encontrar el nombre de host de la instancia de IoT Hub que hospeda los puntos de conexión en el portal en la página **Información general** de la central. De forma predeterminada, el nombre DNS de una instancia de IoT Hub es similar a lo siguiente: `{your iot hub name}.azure-devices.net`.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lista de puntos de conexión de IoT Hub integrados

Azure IoT Hub es un servicio multiempresa que muestra su funcionalidad a diversos actores. En el siguiente diagrama se muestran los diferentes puntos de conexión expuestos por IoT Hub.

![Puntos de conexión de IoT Hub](./media/iot-hub-devguide-endpoints/endpoints.png)

En la lista siguiente se describen los puntos de conexión:

* **Proveedor de recursos**. El proveedor de recursos de IoT Hub expone una interfaz de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Esta interfaz permite que los propietarios de suscripciones de Azure creen y eliminen instancias de IoT Hub para actualizar sus propiedades. Las propiedades de IoT Hub controlan las [directivas de seguridad a nivel de centro](iot-hub-devguide-security.md#access-control-and-permissions), a diferencia del control de acceso de nivel de dispositivo y las opciones funcionales para la mensajería de nube a dispositivo y de dispositivo a nube. El proveedor de recursos también permite [exportar identidades de dispositivo](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

* **Administración de identidades de dispositivo**. Cada instancia de IoT Hub muestra un conjunto de puntos de conexión HTTPS REST para administrar las identidades de dispositivo (crear, recuperar, actualizar y eliminar). Las [identidades del dispositivo](iot-hub-devguide-identity-registry.md) se usan para autenticación de dispositivos y control de acceso.

* **Administración de dispositivo gemelo**. Cada instancia de IoT Hub muestra un conjunto de punto de conexión de HTTPS REST orientado a servicios para consultar y actualizar [dispositivos gemelos](iot-hub-devguide-device-twins.md) (actualización de etiquetas y propiedades).

* **Administración de trabajos**. Cada instancia de IoT Hub muestra un conjunto de punto de conexión de HTTPS REST orientado a servicios para consultar y administrar [trabajos](iot-hub-devguide-jobs.md).

* **Puntos de conexión de dispositivo**. Para cada dispositivo del registro de identidad, IoT Hub muestra un conjunto de puntos de conexión:

  * *Envío de mensajes de dispositivo a nube*. Un dispositivo usa este punto de conexión para [enviar mensajes de dispositivo a la nube](iot-hub-devguide-messages-d2c.md).

  * *Recepción de mensajes de nube a dispositivo*. El dispositivo usa ese punto de conexión para recibir [mensajes de nube a dispositivos](iot-hub-devguide-messages-c2d.md) dirigidos.

  * *Iniciar cargas de archivos*. Un dispositivo usa este punto de conexión para recibir un URI de SAS de Azure Storage de IoT Hub para [cargar un archivo](iot-hub-devguide-file-upload.md).

  * *Recuperación y actualización de las propiedades del dispositivo gemelo*. Un dispositivo usa este punto de conexión para tener acceso a las propiedades del [dispositivo gemelo](iot-hub-devguide-device-twins.md).

  * *Recepción de solicitudes de métodos directos*. Un dispositivo usa este punto de conexión para escuchar solicitudes de [métodos directos](iot-hub-devguide-direct-methods.md).

    Estos puntos de conexión se exponen mediante los protocolos[ MQTT v3.1.1](https://mqtt.org/), HTTPS 1.1 y [AMQP 1.0](https://www.amqp.org/). AMQP también está disponible sobre [WebSockets](https://tools.ietf.org/html/rfc6455) en el puerto 443.

* **Puntos de conexión de servicio**. Cada instancia de IoT Hub muestra un conjunto de puntos de conexión que el back-end de solución para comunicarse con los dispositivos. Con una excepción, estos puntos de conexión solo se muestran con el protocolo [AMQP](https://www.amqp.org/). El punto de conexión de invocación de método se muestra en el protocolo HTTPS.
  
  * *Recepción de mensajes de dispositivo a nube*. Este punto de conexión es compatible con [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Un servicio back-end puede usarse para leer los [mensajes de dispositivo a nube](iot-hub-devguide-messages-d2c.md) enviados por los dispositivos. Además de este punto de conexión integrado, puede crear puntos de conexión personalizados en el centro de IoT.
  
  * *Envío de mensajes de nube a dispositivo y recepción de confirmaciones de entrega*. Estos puntos de conexión permiten al back-end de aplicaciones enviar mensajes confiables [de nube a dispositivo](iot-hub-devguide-messages-c2d.md) y recibir las confirmaciones de entrega o expiración correspondientes.
  
  * *Recepción de notificaciones de archivos*. Este punto de conexión de mensajería le permite recibir notificaciones del momento en que los dispositivos cargan correctamente un archivo. 
  
  * *Invocación del método director*. Este punto de conexión permite que un servicio back-end invoque un [método directo](iot-hub-devguide-direct-methods.md) en un dispositivo.
  
  * *Recepción de eventos de supervisión de operaciones*. Este punto de conexión le permite recibir eventos de supervisión de operaciones si su centro de IoT se ha configurado para emitirlos. Para más información, consulte [Supervisión de operaciones de IoT Hub](iot-hub-operations-monitoring.md).

En el artículo [SDK IoT de Azure](iot-hub-devguide-sdks.md) se describen las distintas formas con las que se puede acceder a estos puntos de conexión.

Todos los puntos de conexión de IoT Hub usan el protocolo [TLS](https://tools.ietf.org/html/rfc5246) y ningún punto de conexión se expone en canales sin cifrar o no seguros.

## <a name="custom-endpoints"></a>Puntos de conexión personalizados

Puede vincular los servicios de Azure existentes en su centro de IoT para usarlos como puntos de conexión para el enrutamiento de mensajes. Estos puntos de conexión hacen de puntos de conexión de servicio y se usan como receptores para las rutas de los mensajes. Los dispositivos no pueden escribir directamente en los puntos de conexión adicionales. Más información sobre el [enrutamiento de mensajes](../iot-hub/iot-hub-devguide-messages-d2c.md).

IoT Hub admite actualmente los siguientes servicios de Azure como puntos de conexión adicionales:

* Contenedores de Azure Storage
* Event Hubs
* Colas de Service Bus
* Temas de Service Bus

Para conocer los límites del número de puntos de conexión que se pueden agregar, consulte [Cuotas y limitación](iot-hub-devguide-quotas-throttling.md).

Puede usar la API de REST [obtener estado de punto de conexión](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) para obtener el estado de mantenimiento de los puntos de conexión. Se recomienda usar la [métricas de IoT Hub](iot-hub-metrics.md) relacionados con la latencia de mensajes de enrutamiento para identificar y depurar errores cuando el estado de punto de conexión está inactiva o es incorrecto.

|Estado de mantenimiento|DESCRIPCIÓN|
|---|---|
|healthy|El punto de conexión está aceptando los mensajes según lo previsto.|
|Insalubre|El punto de conexión no acepta los mensajes según lo previsto y está reintentando la operación de IoT Hub para enviar datos a este punto de conexión. Se actualizará el estado de un punto de conexión en mal estado a correcto cuando IoT Hub ha establecido un estado coherente de mantenimiento.|
|unknown|IoT Hub no ha establecido una conexión con el punto de conexión. Se han entregado a ningún mensaje o rechazarse desde este punto de conexión.|
|problemas de entrega|El punto de conexión no acepta los mensajes, después de IoT Hub reintenta el envío de mensajes para el período retrial.|

## <a name="field-gateways"></a>Puertas de enlace de campo

En una solución de IoT, un *puerta de enlace de campo* se encuentra entre los dispositivos y los puntos de conexión de IoT Hub. Suele encontrarse cerca de los dispositivos. Los dispositivos se comunican directamente con la puerta de enlace de campo mediante un protocolo compatible con los dispositivos. La puerta de enlace de campo se conecta al punto de conexión de IoT Hub con un protocolo que es compatible con IoT Hub. Una puerta de enlace de campo puede ser un dispositivo de hardware dedicado o un equipo de bajo consumo de energía que ejecuta el software de puerta de enlace personalizado.

Puede usar [Azure IoT Edge](/azure/iot-edge/) para implementar una puerta de enlace de campo. IoT Edge ofrece la funcionalidad de multiplexar la comunicación desde varios dispositivos en la misma conexión de IoT Hub.

## <a name="next-steps"></a>Pasos siguientes

Otros temas de referencia en la Guía del desarrollador de IoT Hub son:

* [Lenguaje de consulta de IoT Hub para dispositivos gemelos, trabajos y enrutamiento de mensajes](iot-hub-devguide-query-language.md)
* [Cuotas y limitación](iot-hub-devguide-quotas-throttling.md)
* [Compatibilidad con MQTT de IoT Hub](iot-hub-mqtt-support.md)

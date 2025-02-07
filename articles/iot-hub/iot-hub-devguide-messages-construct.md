---
title: Información sobre el formato de mensaje de Azure IoT Hub | Microsoft Docs
description: 'Guía del desarrollador: describe el formato y el contenido esperado de mensajes de IoT Hub.'
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: e80fb136220330ddc53d513b22ebcfa19a35117b
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252794"
---
# <a name="create-and-read-iot-hub-messages"></a>Creación y lectura de mensajes de IoT Hub

Para admitir la interoperabilidad sin problemas entre protocolos, IoT Hub define un formato de mensaje común para todos los protocolos accesibles desde el dispositivo. Este formato de mensaje se usa tanto para [enrutamiento de dispositivo a nube](iot-hub-devguide-messages-d2c.md) como [de nube a dispositivo](iot-hub-devguide-messages-c2d.md). 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub implementa mensajería de dispositivo a nube mediante un patrón de mensajería de streaming. Los mensajes de dispositivo a nube de IoT Hub son más parecidos a *eventos* de [Event Hubs](/azure/event-hubs/) que a *mensajes* de [Service Bus](/azure/service-bus-messaging/) en que hay un gran volumen de eventos que se pasan mediante el servicio que pueden leer varios lectores.

Un mensaje IoT Hub consta de:

* Conjunto predeterminado de *propiedades del sistema* como el que se muestra a continuación.

* Un conjunto de *propiedades de la aplicación*. Diccionario de propiedades de cadena que la aplicación puede definir y a las que puede acceder sin necesidad de deserializar el cuerpo del mensaje. IoT Hub nunca modifica estas propiedades.

* Un cuerpo binario opaco.

Los valores y los nombres de propiedad solo pueden contener caracteres alfanuméricos ASCII, y ``{'!', '#', '$', '%, '&', ''', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` cuando envía mensajes del dispositivo a la nube usando el protocolo HTTPS o envía mensajes de la nube al dispositivo.

La mensajería de dispositivo a nube con IoT Hub tiene las siguientes características:

* Los mensajes de dispositivo a nube son duraderos y se conservan en el punto de conexión **messages/events** predeterminado de una instancia de IoT Hub hasta siete días.

* Los mensajes de dispositivo a nube pueden tener como máximo 256 KB y se pueden agrupar en lotes para optimizar los envíos. Los lotes pueden tener un tamaño máximo de 256 KB.

* IoT Hub no permite el particionamiento arbitrario. Los mensajes de dispositivo a nube se dividen en particiones en función de su valor de **deviceId**de origen.

* Como se explica en [Control del acceso a IoT Hub](iot-hub-devguide-security.md), IoT Hub habilita la autenticación y el control de acceso por dispositivo.

* Puede marcar los mensajes con información que figura en las propiedades de la aplicación. Para obtener más información, consulte [enriquecimientos de mensaje](iot-hub-message-enrichments-overview.md).

Para más información sobre cómo codificar y descodificar mensajes que se han enviado usando distintos protocolos, consulte [SDK de Azure IoT](iot-hub-devguide-sdks.md).

En la siguiente tabla, aparece el conjunto de propiedades del sistema en los mensajes de IoT Hub.

| Propiedad | DESCRIPCIÓN | ¿La puede definir el sistema? |
| --- | --- | --- |
| message-id |Un identificador configurable por el usuario para el mensaje utilizado para patrones de solicitud y respuesta. Formato: Una cadena que distingue mayúsculas y minúsculas (de hasta 128 caracteres) de caracteres alfanuméricos ASCII de 7 bits + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Sí |
| sequence-number |Un número (exclusivo para cada cola de dispositivo) asignado por IoT Hub a cada mensaje de nube a dispositivo. | No para los mensajes C2D. En caso contrario, sí. |
| to |Un destino especificado en los mensajes [de nube a dispositivo](iot-hub-devguide-c2d-guidance.md) . | No para los mensajes C2D. En caso contrario, sí. |
| absolute-expiry-time |Fecha y hora de la expiración del mensaje. | Sí |
| iothub-enqueuedtime |Fecha y hora en la que IoT Hub recibió el mensaje [de la nube al dispositivo](iot-hub-devguide-c2d-guidance.md). | No para los mensajes C2D. En caso contrario, sí. |
| correlation-id |Cadena de propiedad en un mensaje de respuesta que normalmente contiene el identificador del mensaje de la solicitud en los patrones de solicitud y respuesta. | Sí |
| user-id |Un identificador que se utiliza para especificar el origen de los mensajes. Cuando IoT Hub genera mensajes, se establece en `{iot hub name}`. | Sin  |
| iothub-ack |Un generador de mensajes de comentarios. Esta propiedad se usa en los mensajes de nube a dispositivo para solicitar a IoT Hub que genere mensajes de comentarios debido al consumo del mensaje por el dispositivo. Valores posibles: **none** (valor predeterminado): no se genera ningún mensaje de comentarios, **positive**: recibe un mensaje de comentarios si el mensaje se completó, **negative**: recibe un mensaje de comentarios si el mensaje expiró (o si se alcanzó el número máximo de entregas) sin que se complete en el dispositivo, y **full**: comentarios positivos y negativos. <!-- robinsh For more information, see [Message feedback][lnk-feedback].--> | Sí |
| iothub-connection-device-id |Un identificador establecido por IoT Hub en los mensajes de dispositivo a nube. Contiene el **deviceId** del dispositivo que envió el mensaje. | No para los mensajes D2C. En caso contrario, sí. |
| iothub-connection-auth-generation-id |Un identificador establecido por IoT Hub en los mensajes de dispositivo a nube. Contiene el **generationId** (como se indica en [Propiedades de identidad de dispositivos](iot-hub-devguide-identity-registry.md#device-identity-properties)) del dispositivo que envió el mensaje. | No para los mensajes D2C. En caso contrario, sí. |
| iothub-connection-auth-method |Un método de autenticación establecido por IoT Hub en los mensajes de dispositivo a nube. Esta propiedad contiene información sobre el método de autenticación usado para autenticar el dispositivo que envía el mensaje. <!-- ROBINSH For more information, see [Device to cloud anti-spoofing][lnk-antispoofing].--> | No para los mensajes D2C. En caso contrario, sí. |
| iothub-creation-time-utc | Fecha y hora en la que se creó el mensaje en un dispositivo. Un dispositivo debe establecer explícitamente este valor. | Sí |

## <a name="message-size"></a>Tamaño del mensaje

IoT Hub mide el tamaño de los mensajes de una manera independiente del protocolo, teniendo en cuenta solo la carga real. El tamaño en bytes se calcula como la suma de los siguientes elementos:

* El tamaño del cuerpo en bytes.
* El tamaño en bytes de todos los valores de las propiedades del sistema de mensajes.
* El tamaño en bytes de todos los valores y nombres de propiedades del usuario.

Solamente se pueden usar caracteres ASCII para los valores y los nombres de propiedades, por lo que la longitud de las cadenas es igual al tamaño en bytes.

## <a name="anti-spoofing-properties"></a>Propiedades contra la suplantación

Para evitar la suplantación de dispositivos en los mensajes de dispositivo a nube, Azure IoT Hub marca todos los mensajes con las siguientes propiedades:

* **iothub-connection-device-id**
* **iothub-connection-auth-generation-id**
* **iothub-connection-auth-method**

Las dos primeras contienen los valores **deviceId** y **generationId** del dispositivo de origen, tal como se indicó en [Propiedades de identidad del dispositivo](iot-hub-devguide-identity-registry.md#device-identity-properties).

La propiedad **iothub-connection-auth-method** contiene un objeto JSON serializado con las siguientes propiedades:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Pasos siguientes

* Para información acerca de los límites de tamaño de mensaje en IoT Hub, consulte [Cuotas y limitación de IoT Hub](iot-hub-devguide-quotas-throttling.md).

* Para información sobre cómo crear y leer mensajes de IoT Hub en varios lenguajes de programación, consulte las [guías de inicio rápido](quickstart-send-telemetry-node.md).

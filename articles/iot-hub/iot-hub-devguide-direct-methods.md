---
title: Información de los métodos directos de IoT Hub de Azure | Microsoft Docs
description: 'Guía del desarrollador: uso de métodos directos para invocar código en los dispositivos de una aplicación de servicio.'
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: nberdy
ms.openlocfilehash: d7c63ffe5a318507053f59bf3a18242ee8c327a0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327761"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Conocimiento e invocación de los métodos directos de IoT Hub

IoT Hub ofrece la posibilidad de invocar métodos directos en dispositivos desde la nube. Los métodos directos representan una interacción solicitud-respuesta con un dispositivo similar a una llamada HTTP en la cual se completan correctamente o generan un error de inmediato (tras un tiempo de espera que especifica el usuario). Este enfoque es útil en escenarios donde el curso de una acción inmediata es distinto en función de si el dispositivo pudo responder.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Cada método de dispositivo se dirige a un único dispositivo. [Programación de trabajos en varios dispositivos](iot-hub-devguide-jobs.md) muestra cómo proporcionar una manera de invocar métodos directos en varios dispositivos y de programar la invocación de métodos para los dispositivos desconectados.

Cualquier persona con permisos de **conexión de servicio** en IoT Hub pueden invocar un método en un dispositivo.

Los métodos directos siguen un patrón de solicitud-respuesta y se usan para las comunicaciones que necesitan confirmación inmediata de su resultado. Por ejemplo, el control interactivo del dispositivo, como encender un ventilador.

Si duda entre el uso de las propiedades deseadas, los métodos directos o los mensajes de la nube al dispositivo, consulte [Guía de comunicaciones de la nube al dispositivo](iot-hub-devguide-c2d-guidance.md).

## <a name="method-lifecycle"></a>Ciclo de vida de los métodos

Los métodos directos se implementan en el dispositivo y pueden requerir de ninguna entrada a varias en la carga de método para crear instancias correctamente. Se invoca un método directo mediante un URI orientado al servicio (`{iot hub}/twins/{device id}/methods/`). Un dispositivo recibe métodos directos a través de un tema MQTT específico del dispositivo (`$iothub/methods/POST/{method name}/`) o mediante vínculos AMQP (propiedades de la aplicación `IoThub-methodname` y `IoThub-status`). 

> [!NOTE]
> Cuando se invoca un método directo en un dispositivo, los valores y los nombres de propiedad solo pueden contener caracteres alfanuméricos US-ASCII imprimibles, excepto los del siguiente conjunto: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 

Los métodos directos son sincrónicos y se completan correctamente o producen un error tras el período de tiempo de espera (valor predeterminado: 30 segundos; valor máximo: 300 segundos). Los métodos directos son útiles en escenarios interactivos en los que quiere que un dispositivo actúe únicamente si está conectado y recibiendo comandos. Por ejemplo, encender una luz desde un teléfono. En estos escenarios, quiere saber de inmediato si la acción se ha completado o no para que el servicio en la nube pueda actuar lo antes posible en función del resultado. El dispositivo puede devolver un cuerpo de mensaje como resultado del método, pero no es necesario que el método lo haga. No hay ninguna garantía respecto al orden o la semántica de simultaneidad en las llamadas de método.

Los métodos directos son solo HTTP desde el lado de la nube y MQTT o AMQP desde el lado del dispositivo.

La carga útil de solicitudes y respuestas del método es un documento JSON de hasta 128 KB.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Invocación de un método directo desde una aplicación de back-end

Ahora, invoque un método directo desde una aplicación de back-end.

### <a name="method-invocation"></a>Invocación de método

Las invocaciones de método directo en un dispositivo son llamadas HTTPS compuestas por los elementos siguientes:

* El *URI de solicitud* específico del dispositivo junto con la [versión de la API](/rest/api/iothub/service/invokedevicemethod):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* El *método* POST

* *Encabezados* que contienen la autorización, el identificador de solicitud, el tipo de contenido y la codificación del contenido.

* Un *cuerpo* JSON transparente en el formato siguiente:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

El tiempo de espera se expresa en segundos. Si no se establece el tiempo de espera, el valor predeterminado es 30 segundos.

#### <a name="example"></a>Ejemplo

Consulte a continuación para obtener un ejemplo esencial con `curl`. 

```bash
curl -X POST \
  https://iothubname.azure-devices.net/twins/myfirstdevice/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

### <a name="response"></a>Response

La aplicación de back-end recibe una respuesta que se compone de los siguientes elementos:

* *Código de estado HTTP*, que se usa para errores procedentes de IoT Hub, incluido el error 404 para los dispositivos que no estén conectados.

* *Encabezados* que contienen la etiqueta de identidad, el identificador de solicitud, el tipo de contenido y la codificación del contenido.

* Un *cuerpo* JSON en el formato siguiente:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    El dispositivo proporciona tanto `status` como `body`, que se utilizan para responder con el código de estado o la descripción propios del dispositivo.

### <a name="method-invocation-for-iot-edge-modules"></a>Invocación de un método para los módulos de IoT Edge

La invocación de métodos directos mediante un identificador de módulo se admite en el [SDK de C# de cliente de servicio IoT](https://www.nuget.org/packages/Microsoft.Azure.Devices/).

Para ello, use el método `ServiceClient.InvokeDeviceMethodAsync()` y pase `deviceId` y `moduleId` como parámetros.

## <a name="handle-a-direct-method-on-a-device"></a>Control de un método directo en un dispositivo

Veamos cómo tratar un método directo en un dispositivo IoT.

### <a name="mqtt"></a>MQTT

La sección siguiente es el protocolo MQTT.

#### <a name="method-invocation"></a>Invocación de método

Los dispositivos reciben las solicitudes de método directo en el tema MQTT: `$iothub/methods/POST/{method name}/?$rid={request id}`. El número de suscripciones por dispositivo está limitado a 5. Por lo tanto, se recomienda no suscribirse individualmente a cada método directo. En su lugar, considere la posibilidad de suscribirse a `$iothub/methods/POST/#` y, después, filtrar los mensajes entregados basándose en los nombres de métodos deseados.

El cuerpo que recibe el dispositivo está en el formato siguiente:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Las solicitudes de método son QoS 0.

#### <a name="response"></a>Response

El dispositivo envía las respuestas a `$iothub/methods/res/{status}/?$rid={request id}`, donde:

* La propiedad `status` es el estado de la ejecución del método proporcionado por el dispositivo.

* La propiedad `$rid` es el identificador de solicitud de la invocación de método recibido de IoT Hub.

El dispositivo establece el cuerpo y puede tener cualquier estado.

### <a name="amqp"></a>AMQP

La siguiente sección es para el protocolo AMQP.

#### <a name="method-invocation"></a>Invocación de método

El dispositivo recibe solicitudes de método directo mediante la creación de un vínculo de recepción en la dirección `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

Llega el mensaje AMQP en el vínculo de recepción que representa la solicitud del método. Contiene las secciones siguientes:

* La propiedad del identificador de correlación, que contiene un identificador de solicitud que debe volver a pasarse con la respuesta del método correspondiente.

* Una propiedad de la aplicación denominada `IoThub-methodname`, que contiene el nombre del método que se invoca.

* El cuerpo del mensaje AMQP que contiene la carga del método como JSON.

#### <a name="response"></a>Response

El dispositivo crea un vínculo para devolver la respuesta del método en la dirección `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

La respuesta del método se devuelve en el vínculo de envío y presenta la estructura siguiente:

* La propiedad del identificador de correlación, que contiene el identificador de solicitud pasado en el mensaje de solicitud del método.

* Una propiedad de la aplicación denominada `IoThub-status`, que contiene el estado del método proporcionado por el usuario.

* El cuerpo del mensaje AMQP que contiene la respuesta del método como JSON.

## <a name="additional-reference-material"></a>Material de referencia adicional

Otros temas de referencia en la guía del desarrollador de IoT Hub son los siguientes:

* En [Puntos de conexión de IoT Hub](iot-hub-devguide-endpoints.md) se describen los diferentes puntos de conexión que expone cada centro de IoT Hub para las operaciones en tiempo de ejecución y de administración.

* En [Cuotas y limitación](iot-hub-devguide-quotas-throttling.md) se describen las cuotas que se aplican y el comportamiento de limitación que se espera al usar IoT Hub.

* En [SDK de dispositivo y servicio IoT de Azure](iot-hub-devguide-sdks.md) se muestran los diversos SDK de lenguaje que puede usar para desarrollar aplicaciones para dispositivo y de servicio que interactúen con IoT Hub.

* En [Referencia: Lenguaje de consulta de IoT Hub para dispositivos gemelos, trabajos y enrutamiento de mensajes](iot-hub-devguide-query-language.md) se describe el lenguaje de consulta de IoT Hub que se puede usar para recuperar información de IoT Hub sobre los dispositivos gemelos y trabajos.

* En [Compatibilidad con MQTT de IoT Hub](iot-hub-mqtt-support.md) se proporciona más información sobre la compatibilidad de IoT Hub con el protocolo MQTT.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar métodos directos, puede interesarle el siguiente artículo sobre la Guía del desarrollador de IoT Hub:

* [Programación de trabajos en varios dispositivos](iot-hub-devguide-jobs.md)

Si desea probar algunos de los conceptos descritos en este artículo, puede interesarle el siguiente tutorial de IoT Hub:

* [Uso de métodos directos](quickstart-control-device-node.md)
* [Administración de dispositivos con Azure IoT Tools para VS Code](iot-hub-device-management-iot-toolkit.md)

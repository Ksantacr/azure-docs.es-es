---
title: Administración de dispositivos de Azure IoT con Azure IoT Tools para Visual Studio Code | Microsoft Docs
description: Use Azure IoT Tools para Visual Studio Code para la administración de dispositivos de Azure IoT Hub, incluye métodos directos y opciones de administración de las propiedades deseadas de los dispositivos gemelos.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
origin.date: 01/04/2019
ms.date: 04/29/2019
ms.author: v-yiso
ms.openlocfilehash: 03df2ceb2df4d857e48f1790703a1d87647e43d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60401177"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-for-azure-iot-hub-device-management"></a>Uso de Azure IoT Tools para Visual Studio Code para la administración de dispositivos de Azure IoT Hub

![Diagrama integral](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) es una útil extensión de Visual Studio Code que facilita la administración de IoT Hub y el desarrollo de aplicaciones de IoT. Incluye opciones de administración que puede usar para realizar varias tareas.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Opción de administración          | Tarea                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Métodos directos             | Hacer que un dispositivo actúe, por ejemplo, para iniciar o detener el envío de mensajes o reiniciar el dispositivo.                                        |
| Leer dispositivo gemelo           | Obtener el estado notificado de un dispositivo. Por ejemplo, el dispositivo informa de que el LED está parpadeando.                                    |
| Actualizar dispositivo gemelo         | Poner un dispositivo en determinados estados, como establecer un indicador LED en verde o establecer el intervalo de envío de telemetría en 30 minutos.         |
| Mensajes de nube a dispositivo   | Enviar notificaciones a un dispositivo. Por ejemplo, "Es muy probable que llueva hoy. No olvide traerse un paraguas".              |

Para obtener una explicación más detallada acerca de las diferencias y orientación sobre el uso de estas opciones, consulte la [Guía de comunicación de dispositivo a nube](iot-hub-devguide-d2c-guidance.md) y la [Guía de comunicación de nube a dispositivo](iot-hub-devguide-c2d-guidance.md).

Los dispositivos gemelos son documentos JSON que almacenan información sobre el estado de los dispositivos (metadatos, configuraciones y condiciones). IoT Hub conserva un dispositivo gemelo por cada dispositivo que se conecta a él. Para más información acerca de los dispositivos gemelos, consulte [Introducción a los dispositivos gemelos](iot-hub-node-node-twin-getstarted.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Aprenderá a usar Azure IoT Tools para Visual Studio Code con distintas opciones de administración en la máquina de desarrollo.

## <a name="what-you-do"></a>Qué debe hacer

Ejecute Azure IoT Tools para Visual Studio Code con diversas opciones de administración.

## <a name="what-you-need"></a>Lo que necesita

* Una suscripción de Azure activa.
* Un centro de Azure IoT en su suscripción.
* [Visual Studio Code](https://code.visualstudio.com/)
* [Herramientas de Azure IoT para VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) o [abrir este vínculo en Visual Studio Code](vscode:extension/vsciot-vscode.azure-iot-tools).

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

1. En la vista **Explorador** de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) en la esquina inferior izquierda.
1. Haga clic en **Select IoT Hub** (Seleccionar IoT Hub) en el menú contextual.
1. Se mostrará una ventana emergente en la esquina inferior derecha que le permite iniciar sesión en Azure por primera vez.
1. Después de iniciar sesión, se mostrará la lista de suscripciones de Azure y luego podrá seleccionar la suscripción de Azure e IoT Hub.
1. En unos segundos, aparecerá la lista de dispositivos en la pestaña **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).

   > [!Note]
   > También puede completar la configuración seleccionando **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub). Escriba la cadena de conexión del centro de IoT al que se conecta el dispositivo IoT en la ventana emergente.

## <a name="direct-methods"></a>Métodos directos

1. Haga clic con el botón derecho en el dispositivo y seleccione **Invoke Direct Method** (Invocar método directo). 
1. Escriba el nombre del método y la carga en el cuadro de entrada.
3. Se mostrarán los resultados en la vista **SALIDA** > **Azure IoT Hub Toolkit**.

## <a name="read-device-twin"></a>Leer dispositivo gemelo

1. Haga clic con el botón derecho en el dispositivo y seleccione **Editar dispositivo gemelo**. 
1. Se abrirá un archivo **azure-iot-device-twin.json** con el contenido del dispositivo gemelo.

## <a name="update-device-twin"></a>Actualizar dispositivo gemelo

1. Realice algunas modificaciones de **etiquetas** o en el campo **properties.desired**.
1. Haga clic con el botón derecho en el archivo **azure-iot-device-twin.json**.
1. Seleccione **Update Device Twin** (Actualizar dispositivo gemelo) para actualizar el dispositivo gemelo.

## <a name="send-cloud-to-device-messages"></a>Envío de mensajes de nube a dispositivo

Para enviar un mensaje desde el IoT Hub al dispositivo, siga estos pasos:
 
1. Haga clic con el botón derecho en el dispositivo y seleccione **Send C2D Message to Device** (Enviar mensaje de C2D al dispositivo). 
1. Escriba el mensaje en el cuadro de entrada.
3. Se mostrarán los resultados en la vista **SALIDA** > **Azure IoT Hub Toolkit**.

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido a usar la extensión Azure IoT Tools para Visual Studio Code con diversas opciones de administración.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

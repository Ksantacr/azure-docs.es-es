---
title: Inicio rápido sobre los flujos de dispositivo de Azure IoT Hub (versión preliminar) en C para SSH/RDP | Microsoft Docs
description: En este tutorial, ejecutará una aplicación de C de ejemplo que actúa como proxy para habilitar escenarios de SSH/RDP mediante flujos de dispositivo de IoT Hub.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: ae5db52d7ac00080c2a740820debe6384cfa8dff
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872664"
---
# <a name="quickstart-sshrdp-over-an-iot-hub-device-stream-using-a-c-proxy-application-preview"></a>Inicio rápido: SSH/RDP mediante un flujo de dispositivo de IoT Hub con una aplicación proxy en C (versión preliminar)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub actualmente admite flujos de dispositivos como una [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Los flujos de dispositivo de IoT Hub](./iot-hub-device-streams-overview.md) permiten que las aplicaciones de servicio y de dispositivo se comuniquen de forma segura y apta para el firewall. Consulte la página [Ejemplo Proxy local](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) para obtener información general de la instalación.

En este documento se describe cómo configurar la tunelización del tráfico de SSH (en el puerto 22) mediante flujos de dispositivo. La configuración del tráfico de RDP es similar y solo requiere un cambio de configuración. Dado que los flujos de dispositivo son independientes del protocolo y de la aplicación, este inicio rápido se puede modificar (cambiar los puertos de comunicación) para dar cabida a otros tipos de tráfico de aplicación.

## <a name="how-it-works"></a>Cómo funciona

En la siguiente ilustración se muestra cómo configurar los programas de proxy local de dispositivo y local de servicio para permitir la conectividad de un extremo a otro entre los procesos de cliente y demonio de SSH. Durante la versión preliminar pública, el SDK para C solo admite flujos de dispositivo en el lado del dispositivo. Como resultado, en este inicio rápido solo se incluyen las instrucciones para ejecutar la aplicación de proxy local de dispositivo. Debe ejecutar uno de los siguientes inicios rápidos del servicio:

* [SSH/RDP mediante flujos de dispositivo de IoT Hub con aplicaciones proxy de C#](./quickstart-device-streams-proxy-csharp.md)

* [SSH/RDP mediante flujos de dispositivos de IoT Hub con aplicaciones proxy de Node.js](./quickstart-device-streams-proxy-nodejs.md)

![Configuración de proxy local](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. El proxy local de servicio se conecta a IoT Hub e inicia un flujo de dispositivo al dispositivo de destino.

2. El proxy local de dispositivo completa el protocolo de enlace de iniciación del flujo y establece un túnel de streaming de un extremo a otro hasta el lado del servicio mediante el punto de conexión de streaming de IoT Hub.

3. El proxy local de dispositivo se conecta al demonio SSH (SSHD) que escucha en el puerto 22 del dispositivo (este puerto se puede configurar, como se describe en la sección [*Ejecución de la aplicación de proxy local del dispositivo*](#run-the device-local-proxy-application)).

4. El proxy local de servicio espera nuevas conexiones SSH del usuario mediante la escucha en un puerto designado que, en este caso, es el puerto 2222 (esto también se puede configurar, como se describe [en la sección Ejecución de la aplicación de proxy local del dispositivo](#run-the-device-local-proxy-application)). Cuando el usuario se conecta a través del cliente de SSH, el túnel permite que el tráfico de la aplicación de SSH se transfiera entre los programas cliente y servidor de SSH.

> [!NOTE]
> El tráfico de SSH que se envía en un flujo de dispositivo se tuneliza mediante el punto de conexión de streaming de IoT Hub en lugar de enviarse directamente entre el servicio y el dispositivo. Para más información, lea sobre las [ventajas que aporta el uso de los flujos de dispositivo de IoT Hub](iot-hub-device-streams-overview.md#benefits). Además, en la ilustración se muestra que el demonio de SSH se ejecuta en el mismo dispositivo (o máquina) que el proxy local de dispositivo. En este inicio rápido, como se proporciona la dirección IP del demonio de SSH, el proxy local de dispositivo y el demonio se pueden ejecutar también en máquinas diferentes.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* La versión preliminar de los flujos de dispositivos solo se admite en este momento en instancias de IoT Hub creadas en las siguientes regiones:

   * **Centro de EE. UU.**

   * **EUAP de centro de EE. UU.**

* Instale [Visual Studio 2019](https://www.visualstudio.com/vs/) con la carga de trabajo ["Desarrollo para el escritorio con C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.
* Instale la última versión de [Git](https://git-scm.com/download/).

* Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

En esta guía de inicio rápido, usará el [SDK de dispositivo IoT de Azure para C](iot-hub-device-sdk-c-intro.md). Va a preparar un entorno de desarrollo utilizado para clonar y compilar el [SDK de IoT de Azure para C](https://github.com/Azure/azure-iot-sdk-c) desde GitHub. El SDK de GitHub incluye el código de ejemplo usado en este inicio rápido. 


1. Descargue el [sistema de compilación CMake](https://cmake.org/download/).

    **Antes** de comenzar la instalación de `CMake`, es importante que los requisitos previos de Visual Studio (Visual Studio y la carga de trabajo de desarrollo de escritorio con C++) estén instalados en la máquina. Una vez que los requisitos previos están en su lugar, y se ha comprobado la descarga, instale el sistema de compilación de CMake.

2. Abra un símbolo del sistema o el shell de Bash de Git. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):

    
   ```
   git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
   ```

   Esta operación puede tardar varios minutos en completarse.

3. Cree un subdirectorio `cmake` en el directorio raíz del repositorio de Git y vaya a esa carpeta. 

   ```
   cd azure-iot-sdk-c
   mkdir cmake
   cd cmake
   ```

4. Ejecute el siguiente comando en el directorio `cmake` para compilar una versión del SDK específica para su plataforma de cliente de desarrollo.

   * En Linux:

      ```bash
      cmake ..
      make -j
      ```

   * En Windows, ejecute los siguientes comandos en el símbolo del sistema para desarrolladores correspondiente a Visual Studio 2015 o 2017. Se generará una solución de Visual Studio para el dispositivo simulado en el directorio `cmake`.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta sección, usará Azure Cloud Shell con la [extensión de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

   **MyDevice**: nombre que se da al dispositivo registrado. Use MyCDevice tal como se muestra. Si elige un nombre distinto para el dispositivo, tendrá que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Ejecute los siguientes comandos en Azure Cloud Shell para obtener la _cadena de conexión del dispositivo_ que registró:

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Anote la cadena de conexión del dispositivo, que se parecerá a la del siguiente ejemplo:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido.

## <a name="ssh-to-a-device-via-device-streams"></a>Conexión mediante SSH a un dispositivo mediante flujos de dispositivo

En esta sección, establece una secuencia de un extremo a otro para tunelizar el tráfico SSH.

### <a name="run-the-device-local-proxy-application"></a>Ejecución de la aplicación de proxy local de dispositivo

1. Edite el archivo de origen `iothub_client_c2d_streaming_proxy_sample.c` en la carpeta `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/` y proporcione la cadena de conexión del dispositivo, el nombre de host o la dirección IP del dispositivo de destino, así como el puerto SSH 22:

   ```C
   /* Paste in the your iothub connection string  */
   static const char* connectionString = "[Connection string of IoT Hub]";
   static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

2. Compile el ejemplo:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to cmake at root of repository
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Ejecute el programa compilado en el dispositivo:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   ./iothub_client_c2d_streaming_proxy_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
   iothub_client_c2d_streaming_proxy_sample.exe
   ```

### <a name="run-the-service-local-proxy-application"></a>Ejecución de la aplicación de proxy local de servicio

Como se explicó en la [sección Cómo funciona](#how-it-works), para establecer un flujo de un extremo a otro a fin de tunelizar el tráfico de SSH, se necesita un proxy local en cada extremo (es decir, en el servicio y el dispositivo). Sin embargo, durante la versión preliminar pública, el SDK de IoT Hub para C solo admite flujos de dispositivo en el lado del dispositivo. Para compilar y ejecutar el proxy local de servicio, siga los pasos descritos para ejecutar el proxy de servicio local en uno de los siguientes inicios rápidos:

   * [SSH/RDP mediante flujos de dispositivo de IoT Hub con aplicaciones proxy de C#](./quickstart-device-streams-proxy-csharp.md)

   * [SSH/RDP mediante flujos de dispositivos de IoT Hub con aplicaciones proxy de Node.js](./quickstart-device-streams-proxy-nodejs.md)

### <a name="establish-an-ssh-session"></a>Establecimiento de una sesión de SSH

Con el proxy local de dispositivo y de servicio en ejecución, use su programa cliente de SSH y conéctese al proxy local de servicio en el puerto 2222 (en lugar de al demonio de SSH directamente).

```cmd/sh
ssh <username>@localhost -p 2222
```

En este momento, se mostrará el aviso de inicio de sesión de SSH para que escriba sus credenciales.

Salida de la consola en el proxy local de dispositivo que conecta con el demonio de SSH en `IP_address:22`: ![Salida del proxy local de dispositivo](./media/quickstart-device-streams-proxy-c/device-console-output.png)

Salida de la consola del programa cliente de SSH (el cliente de SSH se comunica con el demonio de SSH mediante la conexión al puerto 22 en el que escucha el proxy local de servicio): ![Resultado del cliente de SSH](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha configurado un centro de IoT, ha registrado un dispositivo, ha implementado un programa de proxy local de servicio y proxy local de dispositivo para establecer un flujo de dispositivo mediante IoT Hub, y ha usado los servidores proxy para tunelizar el tráfico de SSH.

Use los siguientes vínculos para más información sobre los flujos de dispositivo:

> [!div class="nextstepaction"]
> [Introducción a los flujos de dispositivo](./iot-hub-device-streams-overview.md)

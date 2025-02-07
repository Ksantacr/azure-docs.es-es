---
title: 'ESP8266 en la nube: conexión de Feather HUZZAH ESP8266 a Azure IoT Hub | Microsoft Docs'
description: Con este tutorial aprenderá a configurar y conectar Adafruit Feather HUZZAH ESP8266 a Azure IoT Hub para que envíe datos a la plataforma en la nube de Azure.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: 00d0e3a749c4af8faacfed5f725ce16e0492c399
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409074"
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Conexión de Adafruit Feather HUZZAH ESP8266 a Azure IoT Hub en la nube

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Conexión entre DHT22, Feather HUZZAH ESP8266 e IoT Hub](./media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Qué debe hacer

Cree una conexión entre Adafruit Feather HUZZAH ESP8266 e IoT Hub. Luego, ejecutará una aplicación de ejemplo en ESP8266 para recopilar datos de temperatura y humedad desde un sensor DHT22. Por último, envíe los datos del sensor a IoT Hub.

> [!NOTE]
> Aunque use otras placas ESP8266, puede seguir estos pasos para conectarse a IoT Hub. Dependiendo de la placa ESP8266 que use, puede que deba volver a configurar el `LED_PIN`. Por ejemplo, si usa ESP8266 de AI-Thinker, puede cambiarlo de `0` a `2`. ¿Aún no tiene un kit? Consígalo en el [sitio web de Azure](https://azure.com/iotstarterkits).

## <a name="what-you-learn"></a>Conocimientos que adquirirá

* Cómo crear una instancia de IoT Hub y registrar un dispositivo para Feather HUZZAH ESP8266
* Cómo conectar Feather HUZZAH ESP8266 con el sensor y el equipo
* Cómo recopilar datos del sensor mediante la ejecución de una aplicación de ejemplo en Feather HUZZAH ESP8266
* Cómo enviar los datos del sensor a IoT Hub

## <a name="what-you-need"></a>Lo que necesita

![Elementos necesarios para el tutorial](./media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Para realizar esta operación, necesita los siguientes elementos del kit de inicio de Feather HUZZAH ESP8266:

* La placa Feather HUZZAH ESP8266
* Un cable micro USB a USB tipo A

También necesitará lo siguiente para el entorno de desarrollo:

* Una suscripción de Azure activa. Si no tiene ninguna cuenta de Azure, [cree una cuenta de evaluación gratuita de Azure](https://azure.microsoft.com/free/) en solo unos minutos.
* Un equipo PC o Mac que ejecute Windows o Ubuntu.
* Una red inalámbrica a la que se conectará HUZZAH ESP8266.
* Conexión a Internet para descargar la herramienta de configuración.
* [Extensión de Visual Studio Code para Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> La extensión de Visual Studio Code para Arduino debe usar la versión 1.6.8 o posterior del IDE de Arduino. Las versiones anteriores no funcionan con la biblioteca de AzureIoT.

Los elementos siguientes son opcionales en caso de que no tenga un sensor. También tiene la opción de usar una simulación de datos del sensor.

* Un sensor de temperatura y humedad Adafruit DHT22
* Una placa de pruebas
* Cables de puente M/M

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Recuperación de la cadena de conexión del centro de IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Conexión de Feather HUZZAH ESP8266 con el sensor y el equipo

En esta sección se conectan los sensores a la placa. Luego se conecta el dispositivo al equipo para su uso posterior.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Conexión de un sensor de humedad y temperatura DHT22 a Feather HUZZAH ESP8266

Use la placa de pruebas y los cables de puente para realizar la conexión del modo siguiente. Si no dispone de un sensor, omita esta sección porque en su lugar puede usar una simulación de datos del sensor.

![Referencia de conexiones](./media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)

En las patillas del sensor, use el siguiente cableado:

| Inicio (sensor)           | Fin (placa)            | Color de cable   |
| -----------------------  | ---------------------- | ------------  |
| VDD (patilla 31F)            | 3V (patilla 58H)           | Cable rojo     |
| DATOS (patilla 32F)           | GPIO 2 (patilla 46)       | Cable azul    |
| GND (patilla 34F)            | GND (patilla 56I)          | Cable negro   |

Para más información, consulte la [instalación del sensor Adafruit DHT22](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) y el [patillaje de Adafruit Feather HUZZAH Esp8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).

Ahora su Feather Huzzah ESP8266 debe estar conectado con un sensor de trabajo.

![Conectar DHT22 con Feather Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Conexión de Feather HUZZAH ESP8266 a su equipo

Como se indica a continuación, use el cable micro USB a USB tipo A para conectar Feather HUZZAH ESP8266 a su equipo de la manera siguiente.

![Conectar Feather Huzzah al equipo](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Agregar permisos de puerto serie (solo Ubuntu)

Si usa Ubuntu, asegúrese de que tiene los permisos para trabajar en el puerto USB de Feather HUZZAH ESP8266. Para agregar permisos de puerto serie, siga estos pasos:

1. Ejecute los siguientes comandos en un terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Obtendrá una de las siguientes salidas:

   * crw-rw---- 1 root uucp xxxxxxxx
   * crw-rw---- 1 root dialout xxxxxxxx

   En la salida, observe que `uucp` o `dialout` es el nombre del propietario del grupo del puerto USB.

2. Agregue el usuario al grupo mediante la ejecución del comando siguiente:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` es el nombre del propietario del grupo que obtuvo en el paso anterior. `<username>` es el nombre de usuario de Ubuntu.

3. Cierre la sesión de Ubuntu y luego vuelva a iniciar sesión para que aparezca el cambio.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Recopilación de datos del sensor y envío al centro de IoT

En esta sección, implementará y ejecutará una aplicación de ejemplo en Feather HUZZAH ESP8266. La aplicación de ejemplo hace parpadear el LED en Feather HUZZAH ESP8266 y envía los datos de humedad y temperatura recopilados del sensor DHT22 a la instancia de IoT Hub.

### <a name="get-the-sample-application-from-github"></a>Obtención de la aplicación de ejemplo de Github

La aplicación de ejemplo se hospeda en GitHub. Clone el repositorio de ejemplos que contiene la aplicación de ejemplo de GitHub. Para clonar el repositorio de ejemplos, siga estos pasos:

1. Abra un símbolo del sistema o una ventana de terminal.

2. Vaya a la carpeta donde desea almacenar la aplicación de ejemplo.

3. Ejecute el siguiente comando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

   A continuación, instale el paquete de Feather HUZZAH ESP8266 en Visual Studio Code.

4. Abra la carpeta donde se almacena la aplicación de ejemplo.

5. Abra el archivo app.ino en la carpeta de la aplicación en Visual Studio Code.

   ![Apertura de la aplicación de ejemplo en Visual Studio Code](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

6. En Visual Studio Code, escriba `F1`.

7. Tipo **Arduino** y seleccione **Arduino: Board Manager**.

8. En la pestaña **Arduino Board Manager** (Administrador de placas de Arduino), haga clic en **Additional URLs** (Direcciones URL adicionales).

   ![Administrador de placas de Arduino de VS Code](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

9. En la ventana **User Settings** (Configuración del usuario), copie y pegue lo siguiente al final del archivo.

   ```json
   "arduino.additionalUrls": "https://arduino.esp8266.com/stable/package_esp8266com_index.json"
   ```

   ![Configuración de la dirección URL del paquete de Arduino en VS Code](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

10. Guarde el archivo y cierre la pestaña **User Settings** (Configuración del usuario).

11. Haga clic en **Refresh Package Indexes** (Actualizar índices de paquete). Cuando finalice la actualización, busque **esp8266**.

12. Haga clic en el botón **Install** (Instalar) para esp8266.

    El administrador de placas indica que está instalado ESP8266 con la versión 2.2.0 o posterior.

    ![Se instala el paquete esp8266](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

13. Escriba `F1`, a continuación, escriba **Arduino** y seleccione **Arduino: Configuración del panel**.

14. Haga clic en el cuadro de **Selected Board:** (Placa seleccionada:) y escriba **esp8266**. Luego, seleccione **Adafruit HUZZAH ESP8266 (esp8266)**.

    ![Selección de la placa esp8266](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Instalación de las bibliotecas necesarias

1. En Visual Studio Code, escriba `F1`, a continuación, escriba **Arduino** y seleccione **Arduino: Administrador de bibliotecas**.

2. Busque los nombres de biblioteca siguientes uno a uno. Para cada biblioteca que encuentre, haga clic en **Install** (Instalar).
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>¿No tiene un sensor DHT22 real?

La aplicación de ejemplo puede simular datos de humedad y temperatura en caso de que no disponga de un sensor DHT22 real. Para configurar la aplicación de ejemplo de modo que use datos simulados, siga estos pasos:

1. Abra el archivo `config.h` de la carpeta `app`.

2. Busque la siguiente línea de código y cambie el valor de `false` a `true`:

   ```c
   define SIMULATED_DATA true
   ```

   ![Configurar la aplicación de ejemplo para usar datos simulados](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

3. Guarde el archivo.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Implementación de la aplicación de ejemplo en Feather HUZZAH ESP8266

1. En el código de Visual Studio, haga clic en  **\<seleccionar puerto de serie >** en el estado, de barras y, a continuación, haga clic en el puerto serie para Feather HUZZAH ESP8266.

2. Escriba `F1`, a continuación, escriba **Arduino** y seleccione **Arduino: Cargar** para compilar e implementar la aplicación de ejemplo en Feather HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Introducir las credenciales

Cuando la carga finalice correctamente, siga estos pasos para escribir las credenciales:

1. Abra el IDE de Arduino, haga clic en **Tools** > **Serial Monitor** (Herramientas > Monitor serie).

2. En la ventana del monitor serie, observe las dos listas desplegables de la esquina inferior derecha.

3. Seleccione **No line ending** (Sin final de línea) para la lista desplegable de la izquierda.

4. Seleccione **115200 baud** (115200 baudios) para la lista desplegable derecha.

5. En el cuadro de entrada ubicado en la parte superior de la ventana del monitor serie, escriba la siguiente información si se le solicita y luego haga clic en **Send** (Enviar).

   * SSID de Wi-Fi
   * Contraseña de Wi-Fi
   * Cadena de conexión de dispositivo

> [!Note]
> La información de credenciales se almacena en la EEPROM de Feather HUZZAH ESP8266. Si hace clic en el botón de restablecimiento de la placa Feather HUZZAH ESP8266, la aplicación de ejemplo le pregunta si desea borrar la información. Escriba `Y` para borrar la información. Se le pide que proporcione la información una segunda vez.

### <a name="verify-the-sample-application-is-running-successfully"></a>Compruebe que la aplicación de ejemplo se ejecuta correctamente.

Si ve la siguiente salida de la ventana del monitor serie y el LED parpadeando en Feather HUZZAH ESP8266, la aplicación de ejemplo se ejecuta correctamente.

![Salida final en el IDE de Arduino](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Leer los mensajes recibidos por el centro

Es una forma de supervisar los mensajes recibidos por IoT hub del dispositivo usar las herramientas de IoT de Azure para Visual Studio Code. Para obtener más información, consulte [uso de herramientas de IoT de Azure para Visual Studio Code para enviar y recibir mensajes entre su dispositivo e IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Para que conocer más formas procesar los datos enviados por el dispositivo, continúe con la siguiente sección.

## <a name="next-steps"></a>Pasos siguientes

Ha conectado correctamente un dispositivo Feather HUZZAH ESP8266 a su instancia de IoT Hub y ha enviado los datos de sensor capturados a esa instancia.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
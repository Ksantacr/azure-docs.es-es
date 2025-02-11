---
title: Desarrollo de módulos para dispositivos Windows en Azure IoT Edge | Microsoft Docs
description: Este tutorial le guía por la configuración de la máquina de desarrollo y de los recursos en la nube para desarrollar módulos IoT Edge mediante contenedores de Windows para dispositivos Windows.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/20/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 631338c0217eb61f4f98cd06ffa16cb2500f246b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146733"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Tutorial: Desarrollo de módulos IoT Edge para dispositivos Windows

Use Visual Studio 2017 para desarrollar e implementar código en dispositivos Windows en los que se ejecute IoT Edge.

En el inicio rápido, ha creado un dispositivo IoT Edge mediante una máquina virtual Windows y ha implementado un módulo pregenerado desde Azure Marketplace. En este tutorial se explica todo lo necesario para desarrollar e implementar código propio en un dispositivo IoT Edge. Este tutorial es un requisito previo útil para los restantes tutoriales, en los que se proporcionarán más detalles acerca de lenguajes de programación o servicios de Azure concretos. 

En este tutorial se usa el ejemplo de cómo implementar un **módulo de C en un dispositivo Windows**. Se ha elegido este ejemplo porque su simplicidad, lo que permitirá obtener información de las herramientas de desarrollo sin tener que preocuparse por si tiene las bibliotecas correctas instaladas. Una vez que conozca los conceptos de desarrollo, puede elegir el lenguaje o servicio de Azure que prefiera, con el fin de profundizar en él. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una máquina de desarrollo.
> * Usar las herramientas de IoT Edge para Visual Studio 2017 para crear un proyecto.
> * Compilar el proyecto como un contenedor y almacenarlo en un registro de contenedor de Azure.
> * Implementar el código en un dispositivo IoT Edge. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Conceptos clave

En este tutorial se realiza un recorrido por el desarrollo de un módulo IoT Edge. Un *módulo IoT Edge*, o a veces simplemente *módulo* para abreviar, es un contenedor que alberga código ejecutable. En un dispositivo IoT Edge se pueden implementar uno o varios módulos. Los módulos realizan tareas concretas, como la ingesta de datos de sensores, la realización de análisis de datos o de operaciones de limpieza de datos, o el envío a un centro de IoT. Para más información, consulte [Información sobre los módulos Azure IoT Edge](iot-edge-modules.md).

Cuando se desarrollan módulos IoT Edge, es importante conocer la diferencia entre la máquina de desarrollo y el dispositivo IoT Edge de destino en el que finalmente se implementará el módulo. El contenedor que cree para contener el código del módulo debe coincidir con el sistema operativo del *dispositivo de destino*. Para el desarrollo del contenedor de Windows, este concepto es más sencillo porque los contenedores de Windows solo se ejecutan en sistemas operativos Windows. Pero podría, por ejemplo, usar la máquina de desarrollo Windows para crear módulos para dispositivos IoT Edge en Linux. En ese caso, tendría que asegurarse de que la máquina de desarrollo ejecuta contenedores de Linux. A medida que avance en este tutorial, tenga en cuenta la diferencia entre el *sistema operativo de la máquina de desarrollo* y el *sistema operativo del contenedor*.

Este tutorial va dirigido a dispositivos Windows que ejecutan IoT Edge. Los dispositivos IoT Edge de Windows usan contenedores de Windows. Se recomienda usar Visual Studio 2017 para el desarrollo de dispositivos Windows, así que es lo que se va a usar en este tutorial. También se puede usar Visual Studio Code, aunque hay diferencias en lo relativo a la compatibilidad entre ambas herramientas.

En la tabla siguiente se enumeran los escenarios de desarrollo compatibles con los **contenedores de Windows** en Visual Studio Code y Visual Studio 2017.

|   | Visual Studio Code | Visual Studio 2017 |
| - | ------------------ | ------------------ |
| **Servicios de Azure** | Azure Functions <br> Azure Stream Analytics |   |
| **Idiomas** | C# (no se admite la depuración) | C <br> C# |
| **Más información** | [Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IoT Edge Tools para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) |

En este tutorial se enseñan los pasos del desarrollo en Visual Studio 2017. Si prefiere usar Visual Studio Code, consulte las instrucciones de [Uso de Visual Studio Code para desarrollar y depurar los módulos Azure IoT Edge](how-to-vs-code-develop-module.md).

## <a name="prerequisites"></a>Requisitos previos

Una máquina de desarrollo:

* Windows 10 con actualización 1809 o posterior.
* Puede usar su propio equipo o una máquina virtual, según sus preferencias de desarrollo.
* Instale [Git](https://git-scm.com/). 
* Instale el SDK de C para Azure IoT para Windows x64 a mediante vcpkg:

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

<!--vcpkg only required for C development-->

Un dispositivo Azure IoT Edge en Windows:

* Se recomienda no ejecutar IoT Edge en la máquina de desarrollo, sino usar un dispositivo independiente. Esta distinción entre la máquina de desarrollo y el dispositivo IoT Edge refleja con mayor precisión un verdadero escenario de implementación y ayuda a tener claros los distintos conceptos.
* Si no tiene un segundo dispositivo disponible, use el artículo del inicio rápido para crear un dispositivo IoT Edge en Azure con una [máquina virtual Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 

## <a name="install-container-engine"></a>Instalación de un motor de contenedor

Los módulos IoT Edge se empaquetan como contenedores, por lo que necesita un motor de contenedor en la máquina de desarrollo para crear y administrar los contenedores. Se recomienda usar Docker Desktop para el desarrollo debido a gran cantidad de características y su popularidad como motor de contenedor. Con Docker Desktop en un equipo Windows, se puede cambiar entre contenedores de Linux y Windows, con el fin de poder desarrollar fácilmente módulos para los distintos tipos de dispositivos IoT Edge. 

Use la documentación de Docker para realizar la instalación en la máquina de desarrollo: 

* [Instalación de Docker Desktop para Windows](https://docs.docker.com/docker-for-windows/install/)

  * Cuando se instala Docker Desktop para Windows, se le pregunta si desea utilizar contenedores de Linux o de Windows. Para este tutorial, use los **contenedores de Windows**. Para más información, consulte [Switch between Windows and Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) (Cambio entre contenedores de Windows y Linux).


## <a name="set-up-visual-studio-and-tools"></a>Configuración de Visual Studio y sus herramientas

Use las extensiones de IoT para Visual Studio 2017 para desarrollar módulos con IoT Edge. Estas extensiones proporcionan plantillas de proyecto, automatizan la creación del manifiesto de implementación y permiten supervisar y administrar dispositivos IoT Edge. En esta sección, se instalan Visual Studio y la extensión de IoT Edge y luego se configura una cuenta de Azure para administrar recursos de IoT Hub desde Visual Studio. 

1. Si aún no tiene Visual Studio en la máquina de desarrollo, [instale Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio?view=vs-2017) con las cargas de trabajo siguientes: 

   * Desarrollo de Azure
   * Desarrollo para el escritorio con C++
   * Desarrollo multiplataforma de .NET Core

1. Si ya tiene Visual Studio 2017 en la máquina de desarrollo, asegúrese de que su versión sea 15.7 o superior. Siga los pasos del artículo [Modificación de Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) para agregar las cargas de trabajo necesarias si aún no las tiene.

2. Descargue e instale la extensión [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) para Visual Studio 2017. 

3. Cuando finalicen las instalaciones, abra Visual Studio.

4. Seleccione **Ver** > **Cloud Explorer**. 

5. Seleccione el icono del perfil en Cloud Explorer e inicie sesión en la cuenta de Azure si aún no lo ha hecho. 

6. Una vez que inicie sesión, se muestran las suscripciones de Azure. Elija las suscripciones con las que desea acceder a Cloud Explorer y seleccione después **Aplicar**. 

7. Expanda sucesivamente la suscripción, **IoT Hubs** y, finalmente su centro de IoT. Debería ver una lista de dispositivos IoT y puede usar este explorador para administrarlos. 

   ![Acceso a los recursos de IoT Hub en Cloud Explorer](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Creación de un nuevo proyecto de módulo

La extensión Azure IoT Edge Tools proporciona plantillas de proyecto para todos los idiomas admitidos del módulo IoT Edge en Visual Studio 2017. Estas plantillas tienen todos los archivos y el código que se necesita para implementar un módulo de trabajo y probar IoT Edge o le proporcionan a un punto de partida para personalizar la plantilla con su propia lógica de negocios. 

1. Ejecute Visual Studio como administrador.

2. Seleccione **Archivo** > **Nuevo** > **Proyecto**. 

3. En la ventana del nuevo proyecto, seleccione el tipo de proyecto **Azure IoT** y elija el proyecto **Azure IoT Edge**. Cambie el nombre de la solución y del proyecto o acepte el valor predeterminado **AzureIoTEdgeApp1**. Haga clic en **Aceptar** para crear el proyecto. 

   ![Creación de un nuevo proyecto de Azure IoT Edge](./media/tutorial-develop-for-windows/new-project.png)

4. En la ventana del módulo y la aplicación de IoT Edge, configure el proyecto con los valores siguientes: 

   | Campo | Valor |
   | ----- | ----- |
   | Plataforma de aplicaciones | Desactive la opción **Linux Amd64** y active **WindowsAmd64**. |
   | Seleccione una plantilla: | Seleccione **Módulo C**. | 
   | Nombre del proyecto de módulo | Acepte el valor predeterminado **IoTEdgeModule1**. | 
   | Repositorio de imágenes de Docker | Un repositorio de imágenes incluye el nombre del registro de contenedor y el nombre de la imagen de contenedor. La imagen de contenedor se rellena previamente con el valor del nombre del proyecto del módulo. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión del registro de contenedor de Azure. Puede recuperar el servidor de inicio de sesión de la página de información general del registro de contenedor en Azure Portal. <br><br> El repositorio de imágenes final será similar a \<nombre del Registro\>.azurecr.io/iotedgemodule1. |

   ![Configuración del proyecto para el dispositivo de destino, el tipo de módulo y el registro de contenedor](./media/tutorial-develop-for-windows/add-application-and-module.png)

5. Seleccione **Aceptar** para aplicar los cambios. 

Cuando el nuevo proyecto se cargue en la ventana de Visual Studio, dedique unos minutos a familiarizarse con los archivos que ha creado: 

* Un proyecto de IoT Edge denominado **AzureIoTEdgeApp1.Windows.Amd64**.
    * La carpeta **Modules** contiene punteros a los módulos incluidos en el proyecto. En este caso, debería ser solo IoTEdgeModule1. 
    * El archivo **deployment.template.json** es una plantilla para ayudarle a crear un manifiesto de implementación. Un *manifiesto de implementación* es un archivo que define exactamente qué módulos se desean implementar en un dispositivo, cómo se deben configurar y cómo se pueden comunicar tanto entre sí como con la nube. 
* Un proyecto del módulo IoT Edge denominado **IoTEdgeModule1**.
    * El archivo **main.c** contiene el código del módulo de C predeterminado que se incluye con la plantilla de proyecto. El módulo predeterminado toma la entrada de un origen y lo pasa a la instancia de IoT Hub. 
    * El archivo **module.json** contiene detalles sobre el módulo, incluido el repositorio de imágenes completo, la versión de la imagen y el Dockerfile que se va a utilizar para cada plataforma admitida.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Especificación de la credenciales del registro para el agente de IoT Edge

El entorno de ejecución de IoT Edge necesita las credenciales del registro para extraer las imágenes de contenedor en el dispositivo IoT Edge. Agregue estas credenciales a la plantilla de implementación. 

1. Abra el archivo **deployment.template.json**.

2. Busque la propiedad **registryCredentials** en las propiedades $edgeAgent que desee. 

3. Actualice la propiedad con sus credenciales, con este formato: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Review the sample code

The solution template that you created includes sample code for an IoT Edge module. This sample module simply receives messages and then passes them on. The pipeline functionality demonstrates an important concept in IoT Edge, which is how modules communicate with each other.

Each module can have multiple *input* and *output* queues declared in their code. The IoT Edge hub running on the device routes messages from the output of one module into the input of one or more modules. The specific language for declaring inputs and outputs varies between languages, but the concept is the same across all modules. For more information about routing between modules, see [Declare routes](module-composition.md#declare-routes).

1. In the **main.c** file, find the **SetupCallbacksForModule** function.

2. This function sets up an input queue to receive incoming messages. It calls the C SDK module client function [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback). Review this function and see that it initializes an input queue called **input1**. 

   ![Find the input name in the SetInputMessageCallback constructor](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Next, find the **InputQueue1Callback** function.

4. This function processes received messages and sets up an output queue to pass them along. It calls the C SDK module client function [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync). Review this function and see that it initializes an output queue called **output1**. 

   ![Find the output name in the SendEventToOutputAsync constructor](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Open the **deployment.template.json** file.

6. Find the **modules** property of the $edgeAgent desired properties. 

   There should be two modules listed here. The first is **tempSensor**, which is included in all the templates by default to provide simulated temperature data that you can use to test your modules. The second is the **IotEdgeModule1** module that you created as part of this project.

   This modules property declares which modules should be included in the deployment to your device or devices. 

7. Find the **routes** property of the $edgeHub desired properties. 

   One of the functions if the IoT Edge hub module is to route messages between all the modules in a deployment. Review the values in the routes property. The first route, **IotEdgeModule1ToIoTHub**, uses a wildcard character (**\***) to include any message coming from any output queue in the IoTEdgeModule1 module. These messages go into *$upstream*, which is a reserved name that indicates IoT Hub. The second route, **sensorToIotEdgeModule1**, takes messages coming from the tempSensor module and routes them to the *input1* input queue of the IotEdgeModule1 module. 

   ![Review routes in deployment.template.json](./media/tutorial-develop-for-windows/deployment-routes.png)


## Build and push your solution

You've reviewed the module code and the deployment template to understand some key deployment concepts. Now, you're ready to build the IotEdgeModule1 container image and push it to your container registry. With the IoT tools extension for Visual Studio, this step also generates the deployment manifest based on the information in the template file and the module information from the solution files. 

### Sign in to Docker

Provide your container registry credentials to Docker on your development machine so that it can push your container image to be stored in the registry. 

1. Open PowerShell or a command prompt.

2. Sign in to Docker with the Azure container registry credentials that you saved after creating the registry. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Puede recibir una advertencia de seguridad en la que se recomiende el uso de `--password-stdin`. Aunque ese procedimiento se recomienda para escenarios de producción, está fuera del ámbito de este tutorial. Para más información, consulte la referencia de [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin).

### <a name="build-and-push"></a>Compilación e inserción

La máquina de desarrollo ahora tiene acceso al registro de contenedores y también los dispositivos IoT Edge. Es hora de convertir el código del proyecto en una imagen de contenedor. 

1. Haga clic con el botón derecho en la carpeta **AzureIotEdgeApp1.Windows.Amd64** del proyecto y seleccione **Compilación e inserción de módulos IoT Edge**. 

   ![Compilación e inserción de módulos IoT Edge](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   El comando de compilación e inserción inicia tres operaciones. En primer lugar, se crea una nueva carpeta en la solución denominada **config** que contiene los archivos del manifiesto de la implementación completa, con la información de la plantilla de implementación y otros archivos de la solución. En segundo lugar, ejecuta `docker build` para generar la imagen de contenedor basándose en el Dockerfile adecuado para la arquitectura de destino. A continuación, ejecuta `docker push` para insertar el repositorio de imágenes en el registro de contenedor. 

   Este proceso puede tardar varios minutos la primera vez, pero es más rápido la próxima vez que ejecute los comandos. 

2. Abra el archivo **deployment.windows-amd64.json** en la carpeta de configuración recién creada. (Es posible que la carpeta de configuración no aparezca en el Explorador de soluciones de Visual Studio. Si ese es el caso, seleccione el icono **Mostrar todos los archivos** en la barra de tareas del Explorador de soluciones).

3. Busque el parámetro **image** de la sección IotEdgeModule1. Observe que la imagen contiene el repositorio de imágenes completo con el nombre, la versión y la etiqueta de arquitectura del archivo module.json.

4. Abra el archivo **module.json** de la carpeta IotEdgeModule1. 

5. Cambie el número de versión de la imagen del módulo. (La versión, no la versión $schema). Por ejemplo, incremente el número de versión de la revisión a **0.0.2** como si hubiéramos hecho una pequeña corrección en el código del módulo. 

   >[!TIP]
   >Las versiones de los módulos permiten el control de versiones y le permiten probar los cambios en un pequeño conjunto de dispositivos antes de implementar actualizaciones en la producción. Si no incrementa la versión del módulo antes de la compilación e inserción, entonces sobrescribirá el repositorio en el registro de contenedor. 

6. Guarde los cambios en el archivo module.json.

7. Vuelva a hacer clic con el botón derecho en la carpeta **AzureIotEdgeApp1.Windows.Amd64** del proyecto y seleccione de nuevo **Compilación e inserción de módulos IoT Edge**. 

8. Abra el archivo **deployment.windows-amd64.json** de nuevo. Observe que no se creó un nuevo archivo cuando ejecutó el comando de compilación e inserción de nuevo. Más bien, el mismo archivo se actualizó para reflejar los cambios. La imagen de IotEdgeModule1 ahora apunta a la versión 0.0.2 del contenedor. Este cambio en el manifiesto de implementación es cómo se puede saber el dispositivo IoT Edge que hay una nueva versión de un módulo de extracción. 

9. Para comprobar mejor lo que hizo el comando de compilación e inserción, vaya a [Azure Portal](https://portal.azure.com) y navegue al registro de contenedor. 

10. En el registro de contenedor, seleccione **Repositorios** y después **iotedgemodule1**. Compruebe que ambas versiones de la imagen se hayan insertado en el registro.

    ![Visualización de ambas versiones de la imagen en el registro de contenedor](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>Solución de problemas

Si encuentra errores al compilar e insertar la imagen del módulo, a menudo tiene que ver con la configuración de Docker en la máquina de desarrollo. Utilice las siguientes comprobaciones para revisar la configuración: 

* ¿Ha ejecutado el comando `docker login` mediante las credenciales que copió del registro de contenedor? Estas credenciales son diferentes a las que se utilizan para iniciar sesión en Azure. 
* ¿Es el repositorio de contenedor correcto? ¿Tiene el nombre correcto del registro de contenedor y el nombre correcto del módulo? Abra el archivo **module.json** de la carpeta IotEdgeModule1 que se va a comprobar. El valor del repositorio debería ser similar a **\<nombre del registro\>.azurecr.io/iotedgemodule1**. 
* Si utilizó un nombre diferente a **IotEdgeModule1** para el módulo, ¿es coherente ese nombre en toda la solución?
* ¿La máquina funciona con el mismo tipo de contenedores que está creando? Este tutorial es para dispositivos Windows IoT Edge, por lo que los archivos de Visual Studio deben tener la extensión **windows-amd64** y Docker Desktop debe ejecutar contenedores Windows. 

## <a name="deploy-modules-to-device"></a>Implementación de módulos en el dispositivo

Ha verificado que las imágenes de contenedor creadas se almacenan en el registro de contenedor, por lo que es el momento de implementarlas en un dispositivo. Asegúrese de que el dispositivo IoT Edge está en funcionamiento. 

1. Abra Cloud Explorer en Visual Studio y expanda los detalles del centro de IoT. 

2. Seleccione el nombre del dispositivo en el que desea realizar la implementación. En la lista **Acciones**, seleccione **Crear implementación**.

   ![Crear una implementación para un dispositivo individual](./media/tutorial-develop-for-windows/create-deployment.png)


3. En el Explorador de archivos, vaya a la carpeta de configuración del proyecto y seleccione el archivo **deployment.windows amd64.json**. Este archivo suele encontrarse en `C:\Users\<username>\source\repos\AzureIotEdgeApp1\AzureIotEdgeApp1.Windows.Amd64\config\deployment.windows-amd64.json`

   No utilice el archivo deployment.template.json, que no contiene todos los valores de imagen del módulo. 

4. Expanda los detalles para el dispositivo IoT Edge en Cloud Explorer para ver los módulos del dispositivo.

5. Use el botón **Actualizar** para actualizar el estado del dispositivo para ver que los módulos tempSensor y IotEdgeModule1 se implementan en el dispositivo. 


   ![Visualización de los módulos que se ejecutan en el dispositivo IoT Edge](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Visualización de los mensajes desde el dispositivo

El código IotEdgeModule1 recibe mensajes mediante la cola de entrada y los pasa por la cola de salida. El manifiesto de implementación declaraba las rutas que pasaban los mensajes de tempSensor a IotEdgeModule1, y luego enviaban los mensajes de IotEdgeModule1 a la instancia de IoT Hub. Las herramientas Azure IoT Edge Tools para Visual Studio permiten ver los mensajes que llegan a IoT Hub desde dispositivos individuales. 

1. En el explorador en la nube de Visual Studio, seleccione el nombre del dispositivo IoT Edge en el que se ha realizado la implementación. 

2. En el menú **Acciones**, seleccione **Iniciar supervisión del mensaje D2C**.

3. Consulte la sección **Salida** de Visual Studio para ver los mensajes que llegan al centro de IoT. 

   Ambos módulos pueden tardar unos minutos en iniciarse. El entorno de ejecución de Azure IoT Edge necesita recibir su nuevo manifiesto de implementación, extraer las imágenes de los módulos del entorno de ejecución del contenedor y, después, iniciar cada nuevo módulo. Si usted: 

   ![Visualización de los mensajes entrantes del dispositivo a la nube](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Visualización de los cambios en el dispositivo

Si desea ver lo que está ocurriendo en su propio dispositivo, utilice los comandos de esta sección para inspeccionar el entorno de ejecución de Azure IoT Edge y los módulos que se ejecutan en el dispositivo. 

Los comandos de esta sección son para el dispositivo IoT Edge, no en la máquina de desarrollo. Si usa una máquina virtual para el dispositivo IoT Edge, conéctese a ella ahora. En Azure, vaya a la página de información general de la máquina virtual y seleccione **Conectar** para acceder a la conexión del escritorio remoto. En el dispositivo, abra una ventana de comando o PowerShell para ejecutar los comandos `iotedge`.

* Vea todos los módulos implementados en el dispositivo y compruebe su estado:

   ```cmd
   iotedge list
   ```

   Debería ver cuatro módulos: los dos módulos personalizados del entorno de ejecución de IoT Edge, tempSensor e IotEdgeModule1. Los cuatro deben aparecer como en ejecución.

* Inspeccione los registros para un módulo específico:

   ```cmd
   iotedge logs <module name>
   ```

   Los módulos IoT Edge distinguen mayúsculas de minúsculas. 

   Los registros de tempSensor y IotEdgeModule1 deben mostrar los mensajes que están procesando. El módulo edgeAgent es responsable de iniciar los otros módulos, por lo que sus registros tendrá información sobre cómo implementar el manifiesto de implementación. Si algún módulo no aparece en la lista o no se está ejecutando, es probable que los registros de edgeAgent contengan los errores. El módulo edgeHub es responsable de las comunicaciones entre los módulos e IoT Hub. Si los módulos están en funcionamiento, pero los mensajes no llegan al centro de IoT, los registros de edgeHub probablemente contendrán los errores. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha configurado Visual Studio 2017 en la máquina de desarrollo y ha implementado el primer módulo IoT Edge desde él. Ahora que conoce los conceptos básicos, intente agregar funcionalidad a un módulo para que pueda analizar los datos que pasan por él. Elija el lenguaje que prefiera: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)
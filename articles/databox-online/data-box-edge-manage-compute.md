---
title: Administración de proceso de Azure Data Box Edge | Microsoft Docs
description: Describe cómo administrar la configuración del proceso perimetral, como desencadenador, módulos, ver la configuración del proceso, quitar la configuración a través de Azure Portal en una instancia de Azure Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/20/2019
ms.author: alkohli
ms.openlocfilehash: a9daf1d59b03d283be999aaab559c6d60f6405dd
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953125"
---
# <a name="manage-compute-on-your-azure-data-box-edge"></a>Administración de proceso en una instancia de Azure Data Box Edge

En este artículo se describe cómo administrar el proceso en una instancia de Azure Data Box Edge. Puede administrar el proceso a través de Azure Portal o mediante la interfaz de usuario web local. Use Azure Portal para administrar los módulos, los desencadenadores y la configuración de proceso, y la interfaz de usuario web local para administrar los valores del proceso.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Administrar desencadenadores
> * Administrar la configuración del proceso


## <a name="manage-triggers"></a>Administración de desencadenadores

Los eventos son cosas que ocurren dentro del entorno de nube o en el dispositivo sobre las cuales quizás quiera tomar medidas. Por ejemplo, cuando se crea un archivo en un recurso compartido, hablamos de un evento. Los desencadenadores provocan los eventos. En Data Box Edge, los desencadenadores pueden surgir en respuesta a eventos de archivo o según una programación.

- **Archivo**: estos desencadenadores surgen en respuesta a eventos de archivo como la creación de un archivo o su modificación.
- **Scheduled**: estos desencadenadores surgen en respuesta a una programación que puede definir con una fecha de inicio, una hora de inicio y el intervalo de repetición.


### <a name="add-a-trigger"></a>Agregar un desencadenador

Para crear un desencadenador, siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso Data Box Edge y luego a **Proceso perimetral > Desencadenador**. Seleccione **+ Agregar desencadenador** en la barra de comandos.

    ![Selección de Agregar desencadenador](media/data-box-edge-manage-compute/add-trigger-1.png)

2. En la hoja **Agregar desencadenador**, escriba un nombre único para el desencadenador.
    
    <!--Trigger names can only contain numbers, lowercase letters, and hyphens. The share name must be between 3 and 63 characters long and begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character.-->

3. Seleccione un **Tipo** para el desencadenador. Elija **Archivo** cuando el desencadenador sea la respuesta a un evento de archivo. Seleccione **Scheduled** (Programado) cuando quiera que el desencadenador se inicie a una hora determinada y se ejecuta según un intervalo de repetición especificado. Según la selección que haga, verá un conjunto distinto de opciones.

    - **File trigger** (Desencadenador de archivo): elija un recurso compartido montado en la lista desplegable. Cuando se active un evento de archivo en este recurso compartido, el desencadenador podría invocar una función de Azure.

        ![Incorporación de recurso compartido de SMB](media/data-box-edge-manage-compute/add-file-trigger.png)

    - **Scheduled trigger** (Desencadenador programado): especifique la fecha y hora de inicio y el intervalo de repetición en horas, minutos o segundos. Escriba también el nombre para un tema. Un tema le brindará la flexibilidad que necesita para enrutar el desencadenador a un módulo implementado en el dispositivo.

        Una cadena de ruta de ejemplo es: `"route3": "FROM /* WHERE topic = 'topicname' INTO BrokeredEndpoint("modules/modulename/inputs/input1")"`.

        ![Incorporación de un recurso compartido NFS](media/data-box-edge-manage-compute/add-scheduled-trigger.png)

4. Seleccione **Agregar** para crear el desencadenador. Una notificación muestra que la creación del desencadenador está en curso. Una vez creado el desencadenador, la hoja se actualiza para reflejar el desencadenador nuevo.
 
    ![Lista de desencadenadores actualizada](media/data-box-edge-manage-compute/add-trigger-2.png)

### <a name="delete-a-trigger"></a>Eliminación de un desencadenador

Para eliminar un desencadenador, siga estos pasos en Azure Portal.

1. En la lista de desencadenadores, seleccione el desencadenador que quiere eliminar.

    ![Seleccionar un desencadenador](media/data-box-edge-manage-compute/add-trigger-1.png)

2. Haga clic con el botón derecho y seleccione **Eliminar**.

    ![Seleccionar Eliminar](media/data-box-edge-manage-compute/add-trigger-1.png)

3. Cuando se le pida confirmación, haga clic en **Sí**.

    ![Confirmar eliminación](media/data-box-edge-manage-compute/add-trigger-1.png)

La lista de desencadenadores se actualiza para reflejar la eliminación.

## <a name="manage-compute-configuration"></a>Administración de la configuración del proceso

Use Azure Portal para ver la configuración del proceso, quitar una configuración de proceso existente o actualizar la configuración del proceso para sincronizar las claves de acceso del dispositivo IoT y el dispositivo IoT Edge de Data Box Edge.

### <a name="view-compute-configuration"></a>Vista de la configuración de proceso

Para ver la configuración de proceso del dispositivo, siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso Data Box Edge y luego a **Proceso perimetral > Módulos**. Seleccione **View compute** (Ver proceso) en la barra de comandos.

    ![Seleccionar Ver proceso](media/data-box-edge-manage-compute/view-compute-1.png)

2. Tome nota de la configuración de proceso del dispositivo. Cuando configuró el proceso, creó un recurso de IoT Hub. En ese recurso de IoT Hub, se configuró un dispositivo IoT y un dispositivo IoT Edge. Solo los módulos de Linux se admiten para ejecución en el dispositivo IoT Edge.

    ![Ver configuración](media/data-box-edge-manage-compute/view-compute-2.png)


### <a name="remove-compute-configuration"></a>Eliminación de la configuración de proceso

Para quitar la configuración de proceso perimetral existente del dispositivo, siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso Data Box Edge y luego a **Proceso perimetral > Get started** (Empezar). Seleccione **Remove compute** (Quitar proceso) en la barra de comandos.

    ![Seleccionar Quitar proceso](media/data-box-edge-manage-compute/remove-compute-1.png)

2. Si quita la configuración de proceso, deberá volver a configurar el dispositivo en caso de que nuevamente necesite usar el proceso. Cuando se le pida confirmación, seleccione **Sí**.

    ![Seleccionar Quitar proceso](media/data-box-edge-manage-compute/remove-compute-2.png)

### <a name="sync-up-iot-device-and-iot-edge-device-access-keys"></a>Sincronización de las claves de acceso de dispositivo IoT y dispositivo IoT Edge

Cuando se configura el proceso en una instancia de Data Box Edge, se crea un dispositivo IoT y un dispositivo IoT Edge. A estos dispositivos se les asigna automáticamente claves de acceso simétricas. Como procedimiento recomendado de seguridad, estas claves se rotan de manera periódica a través del servicio IoT Hub.

Para rotar estas claves, puede ir al servicio IoT Hub que creó y seleccionar el dispositivo IoT o el dispositivo IoT Edge. Cada dispositivo tiene una clave de acceso primaria y una clave de acceso secundaria. Asigne la clave de acceso primaria a la clave de acceso secundario y luego vuelva a generar la clave de acceso primaria.

Si las claves del dispositivo IoT y del dispositivo IoT Edge se rotaron, deberá actualizar la configuración en Data Box Edge para tener las claves de acceso más recientes. La sincronización permite que el dispositivo obtenga las claves más recientes del dispositivo IoT y del dispositivo IoT Edge. Data Box Edge solo usa las claves de acceso primarias.

Para sincronizar las claves de acceso del dispositivo, siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso Data Box Edge y luego a **Proceso perimetral > Get started** (Empezar). Seleccione **Refresh configuration** (Actualizar configuración) en la barra de comandos.

    ![Seleccionar Actualizar configuración](media/data-box-edge-manage-compute/refresh-configuration-1.png)

2. Seleccione **Sí** cuando se pida confirmación.

     ![Seleccionar Sí cuando se pida confirmación](media/data-box-edge-manage-compute/refresh-configuration-2.png)

3. Salga del cuadro de diálogo cuando haya finalizado la sincronización.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Edge administrar red a través del portal de Azure de proceso](data-box-edge-extend-compute-access-modules.md).

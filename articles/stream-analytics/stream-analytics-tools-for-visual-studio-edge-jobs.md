---
title: Trabajos de Stream Analytics borde del cuadro de datos en herramientas de Azure Stream Analytics para Visual Studio
description: En este artículo se describe cómo crear, depurar y crear los trabajos de borde del cuadro de datos de Stream Analytics mediante las herramientas de Stream Analytics para Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 242fb2daebfe9eb6e5a0c73c2c4c0e91a3131032
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304156"
---
# <a name="develop-stream-analytics-data-box-edge-jobs-using-visual-studio-tools"></a>Desarrollo de trabajos de borde del cuadro de datos de Stream Analytics mediante herramientas de Visual Studio

En este tutorial, obtendrá información sobre cómo usar las herramientas de Stream Analytics para Visual Studio. Aprenda a crear, depurar y crear los trabajos de borde del cuadro de datos de Stream Analytics. Después de crear y probar el trabajo, puede ir a Azure Portal para implementarlo en los dispositivos. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir estos requisitos previos:

* Instalar [2019 de Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), o [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=45326). Se admiten las ediciones Enterprise (Ultimate y Premium), Professional y Community. No se admite la edición Express.  

* Siga las [instrucciones de instalación](stream-analytics-tools-for-visual-studio-edge-jobs.md) para instalar las herramientas de Stream Analytics para Visual Studio.
 
## <a name="create-a-stream-analytics-data-box-edge-project"></a>Crear un proyecto de borde del cuadro de datos de Stream Analytics 

En Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**. Vaya a la lista **Plantillas** de la izquierda y expanda **Azure Stream Analytics** > **Stream Analytics Edge** >  (Stream Analytics para dispositivos perimetrales) **Azure Stream Analytics Edge Application** (Aplicación de Stream Analytics para dispositivos perimetrales). Proporcione un nombre, una ubicación y una solución para el proyecto y seleccione **Aceptar**.

![Nuevo proyecto de borde del cuadro de datos de Stream Analytics en Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-stream-analytics-edge-project.png)

Una vez que se crea el proyecto, vaya al **Explorador de soluciones** para ver la jerarquía de carpetas.

![Vista del explorador de soluciones de trabajo de borde del cuadro de datos de Stream Analytics](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Elegir la suscripción correcta

1. En el menú **Ver** de Visual Studio, seleccione **Explorador de servidores**.  

2. Haga clic con el botón derecho en **Azure** > seleccione **Connect to Microsoft Azure Subscription** (Conectarse a la suscripción de Microsoft Azure) > e inicie sesión con su cuenta de Azure.

## <a name="define-inputs"></a>Definir las entradas

1. En el **Explorador de soluciones**, expanda el nodo **Entradas**; debería ver una entrada llamada **EdgeInput.json**. Haga doble clic para ver su configuración.  

2. Establezca el tipo de origen en **Flujo de datos**. A continuación, establezca Origen en **Centro de IoT Edge**, Formato de serialización de eventos en **JSON** y Codificación en **UTF8**. Opcionalmente, puede cambiar el nombre del **Alias de entrada**, pero en este ejemplo lo dejaremos como está. En caso de cambiar el nombre del alias de entrada, use el nombre que especificó al definir la consulta. Haga clic en **Save** (Guardar) para guardar la configuración.  
   ![Configuración de la entrada del trabajo de Stream Analytics](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Definir las salidas

1. En el **Explorador de soluciones**, expanda el nodo **Salidas**; debería ver una salida llamada **EdgeOutput.json**. Haga doble clic para ver su configuración.  

2. Asegúrese de establecer el receptor para seleccionar **centro de Edge**, establezca el formato de serialización de eventos **Json**, establezca la codificación a **UTF8**y establezca el formato **matriz**. Opcionalmente, puede cambiar el nombre del **Alias de salida**, pero en este ejemplo lo dejaremos como está. En caso de cambiar el nombre del alias de salida, use el nombre que especificó al definir la consulta. Haga clic en **Save** (Guardar) para guardar la configuración. 
   ![Configuración de la salida del trabajo de Stream Analytics](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Definir la consulta de transformación

Trabajos de Stream Analytics implementados en los entornos de borde del cuadro de datos de Stream Analytics admiten la mayoría de [referencia del lenguaje de consulta de Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396). Sin embargo, los trabajos perimetrales de Stream Analytics datos cuadro aún no se admiten las siguientes operaciones: 


|**Categoría**  | **Comando**  |
|---------|---------|
|Operadores de geoespaciales |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Otros operadores | <ul><li>PARTITION BY</li><li>TIMESTAMP BY OVER</li><li>DISTINCT</li><li>Parámetro de expresión en el operador COUNT</li><li>Microsegundos en funciones DATE y TIME</li><li>UDA de JavaScript (esta característica se encuentra aún en versión preliminar para los trabajos implementados en la nube)</li></ul>   |

Cuando se crea un trabajo de borde del cuadro de datos de Stream Analytics en el portal, el compilador automáticamente le advertirá si no está utilizando un operador admitido.

En Visual Studio, defina la siguiente consulta de transformación en el editor de consultas (**archivo script.asaql**).

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>Probar el trabajo localmente

Para probar la consulta localmente, debe cargar los datos de ejemplo. Para obtener datos de ejemplo, descargue los datos de registro del [repositorio de GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) y guárdelos en el equipo local. 

1. Para cargar datos de ejemplo, haga clic con el botón derecho en el archivo **EdgeInput.json** y elija **Agregar entrada local**.  

2. En la ventana emergente, haga clic en **Examinar** para buscar los datos de ejemplo en la ruta de acceso local y seleccione **Guardar**.
   ![Configuración de entrada local en Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Un archivo denominado **local_EdgeInput.json** se agrega automáticamente a la carpeta de entradas.  
4. puede ejecutarlo localmente o enviar a Azure. Para probar la consulta, seleccione **ejecutar localmente**.  
   ![Opciones de ejecución de trabajos de Stream Analytics en Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-visual-stuidio-run-options.png)
 
5. La ventana de símbolo del sistema muestra el estado del trabajo. Cuando el trabajo se ejecuta correctamente, se crea una carpeta similar a "2018-02-23-11-31-42" en la ruta de acceso de la carpeta del proyecto "Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42". Vaya a la ruta de acceso de carpeta para ver los resultados de la carpeta local:

   También puede iniciar sesión en Azure Portal y comprobar que se ha creado el trabajo. 

   ![Carpeta de resultados de trabajos de Stream Analytics](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-job-result-folder.png)

## <a name="submit-the-job-to-azure"></a>Enviar el trabajo a Azure

1. Antes de enviar el trabajo a Azure, debe conectarse a su suscripción de Azure. Abra el **Explorador de servidores**, haga clic con el botón derecho en **Azure** > **Connect to Microsoft Azure subscription** (Conectarse a suscripción de Microsoft Azure) e inicie sesión en su suscripción de Azure.  

2. Para enviar el trabajo a Azure, vaya al editor de consultas y seleccione **Enviar a Azure**.  

3. Se abre una ventana emergente. Elija esta opción actualizar un trabajo de borde del cuadro de datos de Stream Analytics existente o crear uno nuevo. Cuando se actualiza un trabajo existente, reemplazará toda la configuración de trabajo, en este escenario, se publicará un nuevo trabajo. Seleccione **Create a New Azure Stream Analytics Job** (Crear un nuevo trabajo de Azure Stream Analytics), escriba un nombre para el trabajo (algo como **MyASAEdgeJob**), elija los valores adecuados de **Suscripción**, **Grupo de recursos** y **Ubicación** y seleccione **Enviar**.

   ![Enviar un trabajo de Stream Analytics a Azure desde Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-stream-analytics-job-to-azure.png)
 
   Ahora se ha creado el trabajo Edge de Stream Analytics Data cuadro. Puede hacer referencia a la [ejecutar trabajos en el tutorial de IoT Edge](stream-analytics-edge.md) para obtener información sobre cómo implementarla en los dispositivos. 

## <a name="manage-the-job"></a>Administrar el trabajo 

Puede ver el estado del trabajo y el diagrama de trabajos desde el Explorador de servidores. Desde **Stream Analytics** en **Explorador de servidores**, expanda la suscripción y el grupo de recursos donde implementó el trabajo del borde del cuadro de datos de Stream Analytics. Puede ver que MyASAEdgejob tiene el estado **Creado**. Expanda el nodo de trabajos y haga doble clic en él para abrir la vista de trabajos.

![Opciones de administración de trabajos del Explorador de servidores](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
La ventana de vista de trabajos proporciona operaciones como la actualización del trabajo, la eliminación del trabajo y la apertura del trabajo desde Azure Portal.

![Diagrama de trabajos y otras opciones en Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de Azure IoT Edge](../iot-edge/about-iot-edge.md)
* [Tutorial de ASA en IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Enviar comentarios al equipo mediante esta encuesta](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 

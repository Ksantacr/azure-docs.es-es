---
title: Carga de datos de Office 365 mediante Azure Data Factory | Microsoft Docs
description: Uso de Azure Data Factory para copiar datos de Office 365
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: jingwang
ms.openlocfilehash: fe3a3b673f6512856f3640b3e103db8623570a88
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60548290"
---
# <a name="load-data-from-office-365-by-using-azure-data-factory"></a>Carga de datos de Office 365 mediante Azure Data Factory

En este artículo se muestra cómo usar Data Factory para _cargar datos de Office 365 en Azure Blob Storage_. Puede seguir pasos similares para copiar datos en Azure Data Lake Gen1 o Gen2. Consulte el [artículo del conector de Office 365](connector-office-365.md) sobre la copia de datos de Office 365 en general.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú de la izquierda, seleccione **Crear un recurso** > **Datos y análisis** > **Data Factory**: 
   
   ![Selección de la factoría de datos en el panel Nuevo](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, proporcione los valores de los campos que se muestran en la imagen siguiente:
      
   ![Página New data factory (Nueva factoría de datos)](./media/load-office-365-data/new-azure-data-factory.png)
 
    * **Nombre**: escriba un nombre único global para la factoría de datos de Azure. Si recibe el error "El nombre \"LoadFromOffice365Demo\" de factoría de datos no está disponible", escriba un nombre diferente para la factoría de datos. Por ejemplo, podría usar el nombre _**su nombre**_**LoadFromOffice365Demo**. Intente crear de nuevo la factoría de datos. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
    * **Suscripción**: seleccione la suscripción de Azure donde desea crear la factoría de datos. 
    * **Grupo de recursos**: seleccione un grupo de recursos existente en la lista desplegable o seleccione la opción **Crear nuevo** y escriba el nombre de un grupo de recursos. Para obtener más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versión**: seleccione **V2**.
    * **Ubicación**: Seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos que las factorías de datos usan pueden estar en otras ubicaciones y regiones. Estos almacenes de datos incluyen Azure Data Lake Store, Azure Storage, Azure SQL Database, etc.

3. Seleccione **Crear**.
4. Una vez completada la creación, vaya a la factoría de datos. Verá la página principal de **Factoría de datos**, tal y como se muestra en la siguiente imagen:
   
   ![Página principal Factoría de datos](./media/load-office-365-data/data-factory-home-page.png)

5. Seleccione el icono **Author & Monitor** (Creación y supervisión) para iniciar la aplicación de integración de datos en una pestaña independiente.

## <a name="create-a-pipeline"></a>Crear una canalización

1. En la página de introducción seleccione **Create pipeline** (Crear canalización).
 
    ![Creación de una canalización](./media/load-office-365-data/create-pipeline-entry.png)

2. En la pestaña **General** de la canalización, escriba "CopyPipeline" como **nombre** de la canalización.

3. En el cuadro de herramientas Activities (Actividades) > categoría Move & Transform (Mover y transformar), arrastre la actividad **Copy** (Copia) desde el cuadro de herramientas hasta la superficie del diseñador de canalizaciones. Especifique "CopyFromOffice365ToBlob" como nombre de la actividad.

### <a name="configure-source"></a>Configuración del origen

1. Vaya a la canalización > **pestaña Source** (Origen) y haga clic en **+ New** (+Nuevo) para crear un conjunto de datos de origen. 

2. En la ventana de nuevo conjunto de datos, seleccione **Office 365** y, a continuación, seleccione **Finish** (Finalizar).

    ![Nuevo conjunto de datos de Office 365](./media/load-office-365-data/new-office-365-dataset.png)
 
3. Verá que se abre una nueva pestaña para el conjunto de datos de Office 365. En la pestaña **General** de la parte inferior de la ventana de propiedades, escriba "SourceOffice365Dataset" como nombre.

    ![Configuración de Office 365: conjunto de datos general](./media/load-office-365-data/config-office-365-dataset-general.png)
 
4. Vaya a la pestaña **Connection** (Conexión) de la ventana de propiedades. Junto al cuadro de texto del servicio vinculado, haga clic en **+ New** (+Nuevo).
 
    ![Configuración de Office 365: conexión del conjunto de datos](./media/load-office-365-data/config-office-365-dataset-connection.png)

5. En la ventana de nuevo servicio vinculado, escriba "Office365LinkedService" como nombre, escriba el identificador y la clave de la entidad de servicio y, luego, seleccione Save (Guardar) para implementar el servicio vinculado.

    ![Nuevo servicio vinculado de Office 365](./media/load-office-365-data/new-office-365-linked-service.png)
 
6. Una vez que se ha creado el servicio vinculado, volverá a la configuración del conjunto de datos. Junto a "Table" (Tabla), elija la flecha abajo para expandir la lista de conjuntos de datos de Office 365 disponibles y seleccione "BasicDataSet_v0. Contact_v0 "en la lista desplegable:

    ![Configuración de Office 365: tabla de conjunto de datos](./media/load-office-365-data/config-office-365-dataset-table.png)
 
7. Vaya a la pestaña **Schema** (Esquema) de la ventana de propiedades y seleccione **Import Schema** (Importar esquema).  Observe que se muestran los valores de ejemplo y esquema del conjunto de datos de contacto.

    ![Configuración de Office 365: esquema de conjunto de datos](./media/load-office-365-data/config-office-365-dataset-schema.png)

8. Ahora, vuelva a la canalización -> pestaña Source (Origen) y confirme que SourceBlobDataset está seleccionado.
 
### <a name="configure-sink"></a>Configuración del receptor

1. Vaya a la canalización > **pestaña Sink** (Receptor) y seleccione **+ New** (+Nuevo) para crear un conjunto de datos del receptor.
 
2. En la ventana de nuevo conjunto de datos, observe que solo se selecciona el destino admitido al copiar desde Office 365. Seleccione **Azure Blob Storage** y elija **Finish** (Finalizar).  En este tutorial, copiará datos de Office 365 en una instancia de Azure Blob Storage.

    ![Nuevo conjunto de datos de blob](./media/load-office-365-data/new-blob-dataset.png)

4. En la pestaña **General** de la ventana de propiedades, escriba "OutputBlobDataset" como nombre.

5. Vaya a la pestaña **Connection** (Conexión) de la ventana de propiedades. Junto al cuadro de texto de servicio vinculado, seleccione **+ New** (+Nuevo).

    ![Configuración del blob: conexión del conjunto de datos](./media/load-office-365-data/config-blob-dataset-connection.png) 

6. En la ventana de nuevo servicio vinculado, escriba "AzureStorageLinkedService" como nombre, seleccione la entidad de servicio en la lista desplegable de métodos de autenticación, rellene el punto de conexión de servicio, el identificador de la entidad de servicio del inquilino y la clave de la entidad de servicio y, luego, seleccione Save (Guardar) para implementar el servicio vinculado.  Consulte [aquí](connector-azure-blob-storage.md#service-principal-authentication) acerca de cómo configurar la autenticación de la entidad de servicio para Azure Blob Storage.

    ![Nuevo servicio vinculado de blob](./media/load-office-365-data/new-blob-linked-service.png)

7. Una vez que se ha creado el servicio vinculado, volverá a la configuración del conjunto de datos. Junto a la ruta de acceso de archivo, seleccione **Browse** (Examinar) para seleccionar la carpeta de salida donde se extraerán los datos de Office 365.  En "File Format Settings" (Configuración del formato de archivo), junto a File Format (Formato de archivo), elija "**JSON format**" (Formato JSON) y, junto a File Pattern (Patrón de archivo), elija "**Set of objects**" (Conjunto de objetos).

    ![Configuración de blob: ruta de acceso y formato de conjunto de datos](./media/load-office-365-data/config-blob-dataset-path-and-format.png) 

8. Vuelva a la canalización > pestaña Sink (Receptor) y confirme que OutputBlobDataset está seleccionado.

## <a name="validate-the-pipeline"></a>Comprobación de la canalización

Para validar la canalización, seleccione **Validar** en la barra de herramientas.

También puede ver el código JSON asociado a la canalización. Para ello, haga clic en Code (Código) en la parte superior derecha.

## <a name="publish-the-pipeline"></a>Publicación de la canalización

En la barra de herramientas superior, seleccione **Publish All** (Publicar todo). Esta acción publica las entidades (conjuntos de datos y canalizaciones) que creó para Data Factory.

![Publicación de cambios](./media/load-office-365-data/publish-changes.png) 

## <a name="trigger-the-pipeline-manually"></a>Desencadenamiento manual de la canalización

Seleccione **Trigger** (Desencadenar) en la barra de herramientas y, después, seleccione **Trigger Now** (Desencadenar ahora). En la página de ejecución de la canalización, seleccione **Finish** (Finalizar). 

## <a name="monitor-the-pipeline"></a>Supervisar la canalización

Vaya a la pestaña **Monitor** (Supervisar) de la izquierda. Verá una ejecución de canalización que se desencadena de forma manual. Puede usar los vínculos de la columna **Actions** (Acciones) para ver los detalles de la actividad y volver a ejecutar la canalización.

![Supervisión de la canalización](./media/load-office-365-data/pipeline-monitoring.png) 

Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna de acciones. En este ejemplo, solo hay una actividad, así que solo verá una entrada en la lista. Para ver detalles sobre la operación de copia, seleccione el vínculo **Details** (Detalles) (icono de gafas) en la columna de acciones.

![Supervisión de la actividad](./media/load-office-365-data/activity-monitoring.png) 

Si es la primera vez que solicita datos para este contexto (una combinación de los datos de tabla a los que se accede, la cuenta de destino en la que se cargan los datos y la identidad de usuario que realiza la solicitud de acceso a los datos), verá el estado de la actividad de copia como "**In Progress**" (En curso), y solo al hacer clic en el vínculo de detalles en Actions (Acciones), verá el estado como "**RequesetingConsent**".  Un miembro del grupo de aprobadores de acceso a datos debe aprobar la solicitud en Privileged Access Management antes de pasar a la extracción de datos.

_Estado como solicitando consentimiento:_
![Detalles de la ejecución de la actividad: solicitar consentimiento](./media/load-office-365-data/activity-details-request-consent.png) 

_Estado como extrayendo datos:_

![Detalles de ejecución de la actividad: extraer datos](./media/load-office-365-data/activity-details-extract-data.png) 

Una vez que se proporciona el consentimiento, la extracción de los datos continuará y, al cabo de algún tiempo, la ejecución de la canalización se mostrará como completada.

![Supervisión de la canalización: correcta](./media/load-office-365-data/pipeline-monitoring-succeeded.png) 

Ahora, vaya al destino Azure Blob Storage y compruebe que Office 365 se ha extraído en formato JSON.

## <a name="next-steps"></a>Pasos siguientes

Continúe en el artículo siguiente para información sobre la compatibilidad de Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Conector de Office 365](connector-office-365.md)
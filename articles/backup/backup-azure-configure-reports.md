---
title: Configuración de informes para Azure Backup
description: Configure los informes de Power BI para Azure Backup con el almacén de Recovery Services.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: adigan
ms.openlocfilehash: e3004a44958d75d18d608a2fbed7ccc44a00dc93
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60642781"
---
# <a name="configure-azure-backup-reports"></a>Configuración de informes de Azure Backup
En este artículo se muestran los pasos a seguir para configurar informes para Azure Backup mediante un almacén de Recovery Services. También muestra cómo acceder a informes mediante Power BI. Cuando haya completado estos pasos, puede ir directamente a Power BI para ver, personalizar y crear informes.

> [!IMPORTANT]
> Desde el 1 de noviembre de 2018, algunos clientes pueden ver problemas al cargar los datos en la aplicación de Azure Backup en Power BI, con el mensaje “Se encontraron caracteres adicionales al final de la entrada JSON. La interfaz IDataReader produjo la excepción.”.
Esto es debido a un cambio en el formato en el que los datos se cargan en la cuenta de almacenamiento.
Descargue la aplicación más reciente (versión 1.8) para evitar este problema.
>
>

## <a name="supported-scenarios"></a>Escenarios admitidos
- Los informes de Azure Backup se admiten para copias de seguridad de máquinas virtuales de Azure y copias de seguridad de archivos y carpetas con el Agente de Azure Recovery Services.
- Los informes para Azure SQL Database, recursos compartidos de Azure Files, Data Protection Manager y Azure Backup Server no se admiten en este momento.
- Puede ver los informes de los almacenes y las suscripciones, en caso de que la misma cuenta de almacenamiento esté configurada para cada uno de los almacenes. La cuenta de almacenamiento seleccionada debe estar en la misma región que el almacén de Recovery Services.
- La frecuencia de actualización programada para los informes es de 24 horas en Power BI. También puede realizar una actualización ad hoc de los informes en Power BI. En este caso, los últimos datos de la cuenta de almacenamiento del cliente se utilizan para representar los informes.

## <a name="prerequisites"></a>Requisitos previos
- Cree una [cuenta de Azure Storage](../storage/common/storage-quickstart-create-account.md) para informes. Esta cuenta de almacenamiento se usa para almacenar datos relacionados con los informes.
- [Cree una cuenta de Power BI](https://powerbi.microsoft.com/landing/signin/) para ver, personalizar y crear sus propios informes mediante el portal de Power BI.
- Registre el proveedor de recursos **Microsoft.insights**, si todavía no está registrado. Utilice las suscripciones para la cuenta de almacenamiento y el almacén de Recovery Services para que los datos de informes puedan fluir a la cuenta de almacenamiento. Para hacer lo mismo, debe ir a Azure Portal, seleccione **Suscripción** > **Proveedores de recursos** y busque este proveedor para registrarlo.

## <a name="configure-storage-account-for-reports"></a>Configuración de la cuenta de Storage para los informes
Siga estos pasos para configurar la cuenta de almacenamiento para el almacén de Recovery Services mediante Azure Portal. Esta configuración solo se realiza una vez. Después de configurar la cuenta de almacenamiento, puede ir directamente a Power BI para ver el paquete de contenido y usar los informes.

1. Si ya tiene abierto un almacén de Recovery Services, vaya al siguiente paso. Si no tiene abierto ningún almacén de Recovery Services, pero está en Azure Portal, seleccione **Todos los servicios**.

   * En la lista de recursos, escriba **Recovery Services**.
   * Cuando comience a escribir, la lista se filtrará en función de la entrada. Cuando vea la opción **Almacenes de Recovery Services**, haga clic en ella.
   * Aparece la lista de almacenes de Recovery Services. En la lista de almacenes de Recovery Services, seleccione un almacén.

     Se abre el panel del almacén seleccionado.
2. En la lista de elementos que aparece en el almacén, en la sección **Supervisión e informes**, seleccione **Informes de copia de seguridad** para configurar la cuenta de almacenamiento para los informes.

      ![Selección del elemento de menú Informes de copia de seguridad, paso 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. En la hoja **Informes de copia de seguridad**, seleccione el vínculo **Configuración de diagnóstico**. Este vínculo abre la interfaz de usuario de **Configuración de diagnóstico**, que se utiliza para insertar datos en la cuenta de almacenamiento del cliente.

      ![Habilitación de diagnósticos, paso 3](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Seleccione **Activar diagnóstico** para abrir una interfaz de usuario a usar para configurar una cuenta de almacenamiento.

      ![Activación de diagnósticos, paso 4](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. En el cuadro **Nombre** escriba uno. Active la casilla **Archivar en una cuenta de almacenamiento** para que los datos de los informes puedan empezar a fluir en la cuenta de almacenamiento.

      ![Habilitación de diagnósticos, paso 5](./media/backup-azure-configure-reports/select-setting-name.png)
6. Seleccione la **cuenta de almacenamiento**, la suscripción apropiada y la cuenta de almacenamiento en la lista para almacenar los datos de informes. A continuación, seleccione **Aceptar**.

      ![Selección de cuenta de Storage, paso 6](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. En la sección **Registro**, active la casilla **AzureBackupReport**. Mueva el control deslizante para seleccionar un período de retención para estos datos de informes. Los datos de informes de la cuenta de almacenamiento se mantienen durante el período seleccionado con este control deslizante.

      ![Guardar la cuenta de almacenamiento, paso 7](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Revise todos los cambios y seleccione **Guardar**. Esta acción garantiza que todos los cambios se guardan, y la cuenta de almacenamiento está ahora configurada para almacenar datos de informes.

9. La tabla **Configuración de diagnóstico** ahora muestra la nueva configuración habilitada para el almacén. Si no aparece, actualice la tabla para ver la configuración actualizada.

      ![Ver la configuración de diagnóstico, paso 9](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Después de configurar los informes al guardar la cuenta de almacenamiento, *espere 24 horas* para que finalice la inserción de datos iniciales. Importe la aplicación de Azure Backup en Power BI solo después de ese tiempo. Para más información, consulte la [sección de preguntas frecuentes](#frequently-asked-questions).
>
>

## <a name="view-reports-in-power-bi"></a>Visualización de informes en Power BI
Después de configurar la cuenta de almacenamiento para informes con el almacén de Recovery Services, los datos de informes tardarán unas 24 horas en empezar a fluir. Transcurridas 24 horas desde la configuración de la cuenta de almacenamiento, siga estos pasos para ver los informes en Power BI.
Si desea personalizar y compartir el informe, cree un área de trabajo y realice los pasos siguientes:

1. [Inicie sesión](https://powerbi.microsoft.com/landing/signin/) en Power BI.
2. Seleccione **Obtener datos**. En **Más formas de crear su propio contenido**, seleccione **Paquetes de contenido de servicio**. Siga los pasos de la [documentación de Power BI para conectarse a un servicio](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/).

3. En la barra **Búsqueda**, escriba **Azure Backup** y seleccione **Obtenerla ahora**.

      ![Obtención del paquete de contenido](./media/backup-azure-configure-reports/content-pack-get.png)
4. Escriba el nombre de la cuenta de almacenamiento que se configuró en el paso 5 anterior y seleccione **Siguiente**.

    ![Escribir el nombre de la cuenta de Storage](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Mediante el método de autenticación "Clave", escriba la clave de cuenta de almacenamiento de esta cuenta de Storage. Para [ver y copiar las claves de acceso de almacenamiento](../storage/common/storage-account-manage.md#access-keys), vaya a la cuenta de almacenamiento en Azure Portal.

     ![Escribir la cuenta de Storage](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>

6. Seleccione **Iniciar sesión**. Después de iniciar sesión correctamente, ve una notificación Importando datos.

    ![Importando paquete de contenido](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>

    Una vez finalizada la importación, verá la notificación **Correcto**. Si hay muchos datos en la cuenta de almacenamiento, la importación del paquete de contenido podría tardar un poco más.

    ![Importación correcta del paquete de contenido](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>

7. Cuando los datos se importan correctamente, el paquete de contenido de **Azure Backup** se muestra en **Aplicaciones** en el panel de navegación. En **Paneles**, **Informes** y **Conjuntos de datos**, la lista muestra ahora Azure Backup.

8. En **Paneles**, seleccione **Azure Backup**, donde se muestra un conjunto de informes clave anclados.

      ![Panel de Azure Backup](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Para ver el conjunto completo de informes, seleccione cualquier informe en el panel.

      ![Estado de la tarea de Azure Backup](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Seleccione cada pestaña de los informes para ver los informes de dicha área.

      ![Pestañas de informes de Azure Backup](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>¿Cómo se comprueba si los datos de informes han empezado a fluir en una cuenta de almacenamiento?
Puede ir a la cuenta de almacenamiento configurada y seleccione Containers. Si el contenedor tiene una entrada para insights-logs-azurebackupreport, esto indica que los datos de informes han empezado a fluir.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>¿Cuál es la frecuencia de inserción de datos en la cuenta de almacenamiento y en el paquete de contenido de Azure Backup en Power BI?
  Para los usuarios del día 0, la inserción de datos en la cuenta de almacenamiento tarda unas 24 horas aproximadamente. Cuando finalice esta inserción inicial, los datos se actualizan con la frecuencia mostrada en la ilustración siguiente.

  * Los datos relacionados con **trabajos**, **alertas**, **elementos de copia de seguridad**, **almacenes**, **servidores protegidos** y **directivas** se insertan en la cuenta de almacenamiento del cliente cuando esta se registra.

  * Los datos relacionados con **Storage** se insertan en la cuenta de almacenamiento del cliente cada 24 horas.

       ![Frecuencia de inserción de datos de informes de Azure Backup](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power BI tiene una [actualización programada una vez al día](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Puede realizar una actualización manual de los datos en Power BI para el paquete de contenido.

### <a name="how-long-can-i-retain-reports"></a>¿Cuánto tiempo se pueden conservar los informes?
Al configurar una cuenta de almacenamiento, puede seleccionar un período de retención para los datos de informe en la cuenta de almacenamiento. Siga el paso 6 de la sección [Configuración de la cuenta de almacenamiento para los informes](backup-azure-configure-reports.md#configure-storage-account-for-reports). También puede [analizar informes en Excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) y guardarlos durante un período de retención más prolongado, según sus necesidades.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>¿Aparecerán todos los datos en los informes después de configurar la cuenta de almacenamiento?
 Todos los datos generados después de configurar la cuenta de almacenamiento se insertarán en dicha cuenta y estarán disponibles en los informes. Los trabajos en curso no se insertan para los informes. Después de que el trabajo termina o se produce un error, se envía a los informes.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Si ya se ha configurado la cuenta de almacenamiento para ver los informes, ¿se puede cambiar la configuración para usar otra cuenta de almacenamiento?
Sí, puede cambiar la configuración para que se remita a una cuenta de Storage distinta. Use la cuenta de almacenamiento que acaba de configurar al conectarse al paquete de contenido de Azure Backup. Además, después de configurar una cuenta de Storage distinta, los nuevos datos fluyen a esta cuenta de almacenamiento. Los datos más antiguos (antes de cambiar la configuración) permanecen aún en la cuenta de almacenamiento anterior.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>¿Se pueden ver los informes en los almacenes y las suscripciones?
Sí, puede configurar la misma cuenta de Storage en varios almacenes para ver los informes entre ellos. Además, puede configurar la misma cuenta de Storage para almacenes a través de las suscripciones. Después, puede usar esta cuenta de almacenamiento al conectarse al paquete de contenido de Azure Backup en Power BI para ver los informes. La cuenta de almacenamiento seleccionada debe estar en la misma región que el almacén de Recovery Services.

## <a name="troubleshooting-errors"></a>Solución de errores
| Detalles del error | Resolución |
| --- | --- |
| Después de configurar la cuenta de almacenamiento para los informes de Backup, la **cuenta de almacenamiento** sigue mostrando **No configurado**. | Si ha configurado correctamente la cuenta de almacenamiento, los datos de los informes se transmiten a pesar de este problema. Para resolver este problema, vaya a Azure Portal y seleccione **Todos los servicios** > **Configuración de diagnóstico** > **Almacén de Recovery Services** > **Editar configuración**. Elimine el valor configurado previamente y cree una nueva configuración en la misma hoja. Esta vez, en el cuadro **Nombre**, seleccione **Servicio**. Ahora, se muestra la cuenta de almacenamiento configurada. |
|Después de importar el paquete de contenido de Azure Backup en Power BI, aparece un mensaje de error "404-no se encuentra el contenedor". | Como se ha mencionado anteriormente, debe esperar 24 horas después de configurar los informes en el almacén de Recovery Services para verlos correctamente en Power BI. Si intenta acceder a los informes antes de 24 horas, este error se muestra porque aún no hay datos completos para mostrar informes válidos. |

## <a name="next-steps"></a>Pasos siguientes
Después de configurar la cuenta de almacenamiento y ha importado el paquete de contenido de Azure Backup, el siguiente paso es personalizar los informes y usar el modelo de datos de informes para crear informes. Para obtener más información, consulte los siguientes artículos.

* [Uso del modelo de datos de informes de Azure Backup](backup-azure-reports-data-model.md)
* [Filtrado de informes en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Creación de informes en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

---
title: Solución de problemas de Azure Storage con diagnósticos y el analizador de mensajes | Microsoft Docs
description: Tutorial en el que se explica cómo solucionar problemas totalmente por medio del análisis de Azure Storage, AzCopy y el analizador de mensajes de Microsoft.
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: normesta
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 2707081adafa74237e3fb7730837f581e0c8b790
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154220"
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Solución de problemas integral con los registros y métricas de Azure Storage, AzCopy y el analizador de mensajes
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Poder diagnosticar y solucionar problemas es una habilidad clave a la hora de crear y mantener aplicaciones de cliente con el servicio Microsoft Azure Storage. Las aplicaciones de Azure suelen ser de naturaleza dispersa, de modo que diagnosticar y solucionar errores y problemas de rendimiento puede resultar más complicado que hacerlo en entornos tradicionales.

En este tutorial explicamos cómo reconocer algunos errores que pueden afectar al rendimiento y cómo solucionarlos completamente usando herramientas proporcionadas por Microsoft y por el servicio Azure Storage, todo ello para optimizar la aplicación cliente.

Aquí encontrará un análisis práctico de un escenario de solución integral de problemas. Para obtener una guía conceptual exhaustiva con la que solucionar problemas de aplicaciones de Azure Storage, vea [Supervisión, diagnóstico y solución de problemas de Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Herramientas para solucionar problemas de aplicaciones de Azure Storage
Para solucionar problemas en aplicaciones cliente que usan Microsoft Azure Storage, puede usar una combinación de herramientas que permita saber cuándo se produjo un problema y cuál puede ser la causa. Estas herramientas son:

* **Análisis de Azure Storage**. [Análisis de Azure Storage](/rest/api/storageservices/Storage-Analytics) proporciona las métricas y registros del servicio Azure Storage.

  * **métricas de almacenamiento** realizan un seguimiento de las métricas de transacciones y de capacidad relativas a la cuenta de almacenamiento. Con las métricas, puede conocer el rendimiento de su aplicación basándose en diversas medidas. Vea [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) para más información sobre los tipos de métricas de las que hace un seguimiento de Storage Analytics.
  * **registro de almacenamiento** deja constancia en un registro del servidor de cada solicitud realizada al servicio Azure Storage. Este registro hace un seguimiento de los datos detallados de cada solicitud, como la operación realizada, el estado de la operación y la información de latencia. Vea [Formato del registro de Storage Analytics](/rest/api/storageservices/Storage-Analytics-Log-Format) para más información sobre los datos de solicitud y de respuesta que se escriben en los registros de Storage Analytics.

* **Azure Portal**. Puede configurar las métricas y el registro de su cuenta de almacenamiento en [Azure Portal](https://portal.azure.com). Asimismo, también puede ver diagramas y gráficos que le mostrarán el rendimiento de su aplicación conforme avanza el tiempo, así como configurar alertas que le avisarán si el rendimiento de su aplicación es diferente a lo esperado según lo establecido en una métrica específica.

    Consulte [Supervisión de una cuenta de almacenamiento en Azure Portal](storage-monitor-storage-account.md) para obtener más información sobre la configuración de la supervisión en Azure Portal.
* **AzCopy**. Los registros del servidor de Azure Storage se almacenan como blobs, por lo que puede usar AzCopy para copiar estos blobs de registro en un directorio local y, luego, analizarlos con el analizador de mensajes de Microsoft. Consulte [Transferencia de datos con la utilidad en línea de comandos AzCopy](storage-use-azcopy.md) para obtener más información sobre AzCopy.
* **Analizador de mensajes de Microsoft**. El analizador de mensajes es una herramienta que usa archivos de registro y que muestra los datos de registro en un formato visual para que sean más fáciles de filtrar, buscar y agrupar en conjuntos útiles; gracias a esto, podrá analizar errores y problemas de rendimiento. Vea la [Guía de funcionamiento del analizador de mensajes de Microsoft](https://technet.microsoft.com/library/jj649776.aspx) para más información sobre el analizador de mensajes.

## <a name="about-the-sample-scenario"></a>Acerca del escenario de ejemplo
Para este tutorial, analizaremos un escenario donde las métricas de Azure Storage indican una tasa de éxito de bajo porcentaje de una aplicación que llama a Azure Storage. La métrica de tasa de éxito de bajo porcentaje (señalada como **PercentSuccess** en [Azure Portal](https://portal.azure.com) y en las tablas de métricas) hace un seguimiento de las operaciones que se realizaron correctamente, pero que devolvieron un código de estado HTTP superior a 299. En los archivos de registro de almacenamiento del servidor, estas operaciones se registran con el estado de transacción **ClientOtherErrors**. Para más información sobre la métrica de tasa de éxito de bajo porcentaje, vea [Las métricas muestran un PercentSuccess bajo o las entradas de registro de análisis tienen operaciones con el estado de transacción ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Como parte de su funcionalidad habitual, es posible que las operaciones de Azure Storage devuelvan códigos de estado HTTP mayores que 299. Aun así, en algunos casos estos errores indicarán que es posible que pueda optimizar su aplicación cliente para mejorar el rendimiento.

En este escenario, todo aquello que esté por debajo del 100% será considerado como una tasa de bajo porcentaje de éxito. Pero siempre puede elegir un nivel de métricas diferente acorde a sus necesidades. Le recomendamos que, mientras esté probando la aplicación, establezca una tolerancia de línea de base de las métricas de rendimiento clave. Según las pruebas que haga, puede establecer que la aplicación tenga una tasa de porcentaje de éxito constante de, por ejemplo, un 90% o de un 85%. Si los datos de las métricas arrojan que la aplicación se desvía de ese porcentaje, puede investigar para saber cuál es la causa del aumento.

En nuestro escenario de ejemplo, una vez que hayamos establecido la métrica de tasa de porcentaje de éxito por debajo del 100%, revisaremos los registros para buscar los errores que guardan relación con las métricas y usarlos para averiguar cuál es la causa de la tasa de bajo porcentaje de éxito. En concreto, revisaremos los errores del intervalo 400. Después, revisaremos con más detalle los errores 404 (no encontrado).

### <a name="some-causes-of-400-range-errors"></a>Algunas de las causas de los errores del intervalo 400
En los siguientes ejemplos se exponen algunas muestras de errores de intervalo 400 para solicitudes de Azure Blob Storage, así como sus posibles causas. Cualquiera de estos errores, además de los errores en el intervalo 300 y el intervalo 500, pueden ser la razón de una tasa de bajo porcentaje de éxito.

Tenga en cuenta que las siguientes listas no están ni mucho menos completas. Vea [Códigos de estado y de error](https://msdn.microsoft.com/library/azure/dd179382.aspx) en MSDN para más información sobre los errores generales de Azure Storage y sobre los errores específicos de cada uno de los servicios de almacenamiento.

**Ejemplos de código de estado 404 (no encontrado)**

Se produce cuando una operación de lectura en un contenedor o un blob falla porque no se puede encontrar el blob o el contenedor.

* Se produce cuando otro cliente elimina un contenedor o un blob antes de realizar la solicitud.
* Se produce si está usando una llamada API que crea el contenedor o el blob después de comprobar si existe. Las API de tipo CreateIfNotExists hacen primero una llamada HEAD para comprobar la existencia del contenedor o del blob; si no existe, se devuelve un error 404 y se realiza una segunda llamada PUT para escribir el contenedor o el blob.

**Ejemplos de código de estado de 409 (conflicto)**

* Se produce cuando usa una API de tipo Create para crear un contenedor o un blob sin comprobar si ya existen, o bien cuando ya hay otro contenedor u otro blob con el mismo nombre.
* Se produce si elimina un contenedor e intenta crear otro con el mismo nombre antes de que finalice la operación de eliminación.
* Se produce si especifica una concesión en un contenedor o un blob y ya hay una concesión.

**Ejemplos de código de estado 412 (error de condición previa)**

* Se produce cuando no se cumple la condición especificada por un encabezado condicional.
* Se produce cuando el identificador de concesión especificado no coincide con el identificador de concesión en el contenedor o el blob.

## <a name="generate-log-files-for-analysis"></a>Generar archivos de registro para el análisis
En este tutorial, usaremos el analizador de mensajes para trabajar con tres tipos diferentes de archivos de registro, aunque también puede trabajar con cualquiera de los siguientes elementos:

* El **registro del servidor**, que se crea cuando se habilita el registro de Azure Storage. El registro del servidor contiene datos sobre cada operación a la que haya llamado alguno de los servicios de Azure Storage: blob, cola, tabla y archivo. El registro del servidor indica a qué operación se llamó y qué código de estado fue devuelto, así como otros detalles sobre la solicitud y la respuesta.
* El **registro de cliente de .NET**, que se crea cuando se habilita el registro del lado cliente desde la aplicación .NET. El registro de cliente contiene información detallada sobre el modo en que el cliente prepara la solicitud y recibe y procesa la respuesta.
* El **registro de seguimiento de red HTTP**, que recopila datos sobre las solicitudes HTTP/HTTPS y datos de respuesta, incluidas las operaciones de Azure Storage. En este tutorial, crearemos un seguimiento de red a través del analizador de mensajes.

### <a name="configure-server-side-logging-and-metrics"></a>Configurar el registro y las métricas del lado servidor
En primer lugar, se deberá configurar el registro de Azure Storage y las métricas, por lo que tenemos datos desde el lado del servicio para analizar. El registro y las métricas se pueden configurar de varias maneras: a través de [Azure Portal](https://portal.azure.com), con PowerShell o mediante programación. Consulte [habilitar las métricas](storage-analytics-metrics.md#enable-metrics-using-the-azure-portal) y [habilitar el registro](storage-analytics-logging.md#enable-storage-logging) para obtener más información acerca de la configuración del registro y métricas.

### <a name="configure-net-client-side-logging"></a>Configurar el registro del lado cliente de .NET
Para configurar el registro del lado cliente de una aplicación .NET, habilite los diagnósticos .NET en el archivo de configuración de la aplicación (web.config o app.config). Consulte [Inicio de sesión del lado cliente con la Biblioteca del cliente de almacenamiento de .NET](https://msdn.microsoft.com/library/azure/dn782839.aspx) y [Registro del lado cliente con el SDK de Microsoft Azure Storage para Java](https://msdn.microsoft.com/library/azure/dn782844.aspx) en MSDN para más información.

El registro del lado cliente incluye información detallada sobre el modo en que el cliente prepara la solicitud y recibe y procesa la respuesta.

La biblioteca de cliente de almacenamiento almacena datos de registro del lado cliente en la ubicación que se especificó en el archivo de configuración de la aplicación (web.config o app.config).

### <a name="collect-a-network-trace"></a>Recopilar un seguimiento de red
Puede usar el analizador de mensajes para recopilar un seguimiento de red HTTP/HTTPS mientras la aplicación cliente se ejecuta. El analizador de mensajes usa [Fiddler](https://www.telerik.com/fiddler) en el back-end. Antes de recopilar el seguimiento de red, le recomendamos que configure Fiddler para registrar el tráfico HTTPS sin cifrar:

1. Instale [Fiddler](https://www.telerik.com/download/fiddler).
2. Inicie Fiddler.
3. Seleccione **Tools | Fiddler Options** (Herramientas | Opciones de Fiddler).
4. En el cuadro de diálogo de opciones, asegúrese de que las opciones **Capture HTTPS CONNECTs** (Capturar CONEXIONES HTTPS) y **Decrypt HTTPS Traffic** (Descifrar tráfico HTTPS) están seleccionadas, tal y como se muestra aquí.

![Configurar opciones de Fiddler](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

En este tutorial, primero deberá recopilar y guardar un seguimiento de red en el analizador de mensajes y, luego, crear una sesión de análisis para analizar el seguimiento y los registros. Para recopilar un seguimiento de red en el analizador de mensajes:

1. En el analizador de mensajes, seleccione **File | Quick Trace | Unencrypted HTTPS** (Archivo | Seguimiento rápido | HTTPS sin cifrar).
2. El seguimiento se iniciará inmediatamente. Seleccione **Stop** (Detener) para detener el seguimiento y poder configurarlo para que solo haga el seguimiento del tráfico de almacenamiento.
3. Seleccione **Edit** (Editar) para editar la sesión de seguimiento.
4. Seleccione el vínculo **Configure** (Configurar) que está a la derecha del proveedor ETW **Microsoft-Pef-WebProxy**.
5. En el cuadro de diálogo **Advanced Settings** (Configuración avanzada), haga clic en la pestaña **Provider** (Proveedor).
6. En el campo **Hostname Filter** (Filtro de nombre de host), especifique los puntos de conexión de almacenamiento, separados por espacios. Por ejemplo, puede especificar los extremos cambiando `storagesample` por el nombre de su cuenta de almacenamiento. Así:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Salga del cuadro de diálogo y haga clic en **Restart** (Reiniciar) para empezar a recopilar el seguimiento con el filtro de nombre de host activado, de modo que solo se incluya en el seguimiento el tráfico de red de Azure Storage.

> [!NOTE]
> Una vez finalizada la recopilación del seguimiento de red, le recomendamos restablecer la configuración inicial de Fiddler para descifrar el tráfico HTTPS. En el cuadro de diálogo de opciones de Fiddler, desactive las casillas **Capture HTTPS CONNECTs** (Capturar CONEXIONES HTTPS) y **Decrypt HTTPS Traffic** (Descifrar tráfico HTTPS).
>
>

Vea el tema sobre el [uso de las características de seguimiento de red](https://technet.microsoft.com/library/jj674819.aspx) en TechNet para más información.

## <a name="review-metrics-data-in-the-azure-portal"></a>Revisar los datos de las métricas en Azure Portal
Una vez que la aplicación haya estado en ejecución durante un rato, puede revisar los gráficos de las métricas que aparezcan en [Azure Portal](https://portal.azure.com) para ver el rendimiento de su servicio.

Primero, vaya a la cuenta de almacenamiento en Azure Portal. De forma predeterminada, se muestra un diagrama de supervisión con la métrica **Porcentaje de operaciones correctas** en la hoja de la cuenta. Si ha modificado el gráfico para mostrar diferentes métricas, agregue la métrica **Porcentaje de operaciones correctas**.

Ahora verá el **Porcentaje de operaciones correctas** en el diagrama de supervisión, junto con otras métricas que pueda haber agregado. En el escenario que pasaremos a examinar mediante el análisis de los registros del analizador de mensajes, podrá ver que la tasa de porcentaje de éxito es ligeramente inferior al 100 %.

Para obtener más información sobre cómo agregar y personalizar gráficos de métricas, vea [Personalización de gráficos de métricas](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Una vez habilitadas las métricas de almacenamiento, los datos de las métricas tardarán un rato en aparecer en Azure Portal. Hasta que no haya transcurrido la hora actual, las métricas de la hora anterior no se mostrarán en Azure Portal. Asimismo, recuerde que las métricas por minuto no se muestran en Azure Portal. así que es posible que tarde hasta dos horas en ver los datos de las métricas tras habilitarlas.
>
>

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Usar AzCopy para copiar registros del servidor en un directorio local
Azure Storage escribe datos de registro del servidor en blobs, mientras que las métricas se escriben en tablas. Los blobs de registro se encuentran disponibles en el conocido contenedor `$logs` de su cuenta de almacenamiento. Estos blobs de registro se pueden identificar de forma jerárquica por año, mes, día y hora, lo que hace que se pueda localizar fácilmente el intervalo de tiempo que quiera examinar. Por ejemplo, en la cuenta `storagesample`, el contenedor para los blobs de registro del 01/02/2015 de 8:00 a 9:00 de la mañana es `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Los blobs individuales en este contenedor poseen nombres secuenciales y comienzan por `000000.log`.

Puede usar la herramienta de línea de comandos AzCopy para descargar estos archivos de registro del lado servidor en una ubicación de su elección en el equipo local. Por ejemplo, puede usar el siguiente comando para descargar en la carpeta `C:\Temp\Logs\Server` los archivos de registro de las operaciones de blob que tuvieron lugar el 2 de enero de 2015; reemplace `<storageaccountname>` por el nombre de su cuenta de almacenamiento y `<storageaccountkey>`, por su clave de acceso de cuenta:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy está disponible para su descarga en la página de [descargas de Azure](https://azure.microsoft.com/downloads/) . Para obtener más información sobre cómo usar AzCopy, consulte [Transferencia de datos con la utilidad en línea de comandos AzCopy](storage-use-azcopy.md).

Para obtener más información sobre cómo descargar los registros del lado servidor, consulte [Descarga de datos de registro del registro de almacenamiento](https://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Usar el analizador de mensajes de Microsoft para analizar los datos de registro
El analizador de mensajes de Microsoft es una herramienta para capturar, mostrar y analizar el protocolo del tráfico de mensajes, eventos y otros mensajes del sistema o de una aplicación, usando para ello escenarios de diagnóstico y de solución de problemas. El analizador de mensajes también permite cargar, agregar y analizar los datos procedentes de registros y de archivos de seguimiento guardados. Para más información sobre el analizador de mensajes, vea la [Guía de funcionamiento del analizador de mensajes de Microsoft](https://technet.microsoft.com/library/jj649776.aspx).

El analizador de mensajes incluye herramientas del servicio Azure Storage que hacen más fácil analizar los registros de red, de cliente y de servidor. En esta sección, abordaremos el uso de estas herramientas para solucionar el problema del bajo porcentaje de éxito en los registros de almacenamiento.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Descargar e instalar el analizador de mensajes y las herramientas de Azure Storage
1. Descargue el [analizador de mensajes](https://www.microsoft.com/download/details.aspx?id=44226) del Centro de descarga de Microsoft y ejecute el programa de instalación.
2. Inicie el analizador de mensajes.
3. En el menú **Herramientas**, seleccione **Administrador de activos**. En el cuadro de diálogo **Administrador de activos**, seleccione **Descargas** y filtre por **Azure Storage**. Verá las herramientas de Azure Storage como se muestra en la imagen de abajo.
4. Haga clic en **Sync All Displayed Items** (Sincronizar todos los elementos que se muestran) para instalar las herramientas de Azure Storage. Tiene disponibles los siguientes recursos:
   * **Reglas de color de Azure Storage:** las reglas de color de Azure Storage permiten definir filtros especiales que usan estilos de color, texto y fuente para resaltar los mensajes que contengan información específica en un seguimiento.
   * **Gráficos de Azure Storage:** los gráficos de Azure Storage son gráficos predefinidos que en los que se trazan datos de los registro de servidor. Tenga en cuenta que, actualmente, el uso de los gráficos de Azure Storage se reduce únicamente a cargar el registro del servidor en la cuadrícula de análisis.
   * **Analizadores de Azure Storage:** los analizadores de Azure Storage analizan los registros de cliente, servidor y HTTP de Azure Storage para mostrarlos en la cuadrícula de análisis.
   * **Filtros de Azure Storage:** los filtros de Azure Storage son criterios predefinidos que sirven para consultar los datos en la cuadrícula de análisis.
   * **Diseños de vista de Azure Storage:** los diseños de vista de Azure Storage son diseños y agrupaciones de columna predefinidos en la cuadrícula de análisis.
5. Reinicie el analizador de mensajes después de haber instalado estas herramientas.

![Administrador de activos del analizador de mensajes](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Instale todas las herramientas de Azure Storage que se muestran para poder realizar este tutorial.
>
>

### <a name="import-your-log-files-into-message-analyzer"></a>Importar los archivos de registro al analizador de mensajes
Puede importar todos los archivos de registro guardados (del lado servidor, del lado cliente y de red) en una sola sesión en el analizador de mensajes de Microsoft para analizarlos.

1. En el menú **File** (Archivo) del analizador de mensajes de Microsoft, haga clic en **New Session** (Nueva sesión) y, luego, en **Blank Session** (Sesión en blanco). En el cuadro de diálogo **New Session** (Nueva sesión), escriba un nombre para la sesión de análisis. En el panel **Session Details** (Detalles de la sesión), haga clic en el botón **Files** (Archivos).
2. Para cargar los datos de seguimiento de red generados por el analizador de mensajes, haga clic en **Add Files** (Agregar archivos), vaya a la ubicación donde guardó el archivo .matp correspondiente a la sesión de seguimiento web, seleccione el archivo .matp y haga clic en **Open** (Abrir).
3. Para cargar los datos del registro del lado servidor, haga clic en **Add Files** (Agregar archivos), vaya a la ubicación donde descargó los registros del lado servidor, seleccione los archivos de registro para el intervalo de tiempo que desee analizar y haga clic en **Open** (Abrir). Tras esto, en el panel **Session Details** (Detalles de la sesión), establezca el elemento desplegable **Text Log Configuration** (Configuración del registro de texto) de cada archivo de registro del lado servidor en **AzureStorageLog**; así, se asegurará de que el analizador de mensajes de Microsoft puede analizar correctamente el archivo de registro.
4. Para cargar los datos del registro del lado cliente, haga clic en **Add Files** (Agregar archivos), vaya a la ubicación donde guardó los registros del lado cliente, seleccione los archivos de registro que quiera analizar y haga clic en **Open** (Abrir). Posteriormente, en el panel **Session Details** (Detalles de la sesión), establezca el elemento desplegable **Text Log Configuration** (Configuración del registro de texto) de cada archivo de registro del lado cliente en **AzureStorageClientDotNetV4**; así, se asegurará de que el analizador de mensajes de Microsoft puede analizar correctamente el archivo de registro.
5. Haga clic en **Start** (Iniciar) en el cuadro de diálogo **New Session** (Nueva sesión) para cargar y analizar los datos de registro. Los datos de registro se muestran en la cuadrícula de análisis del analizador de mensajes.

En la siguiente imagen se muestra una sesión de ejemplo configurada con archivos de registro de servidor, de cliente y seguimiento de red.

![Configurar una sesión del analizador de mensajes](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Tenga en cuenta que el analizador de mensajes carga los archivos en la memoria. Si tiene una gran cantidad de datos de registro, seguramente quiera filtrarlos para lograr un rendimiento óptimo del analizador de mensajes.

Primero, deberá indicar el intervalo de tiempo que desea revisar e intentar que este sea lo más reducido posible. Es probable que en la mayoría de casos solo quiera revisar un período de minutos o, como mucho, de horas. Importe el conjunto más pequeño de registros que le resulte más adecuado.

Si, aun así, la cantidad de datos de registro es demasiado grande, conviene delimitarlos con un filtro de sesión antes de cargarlos. En el cuadro **Session Filter** (Filtros de sesión), seleccione el botón **Library** (Biblioteca) para elegir un filtro predefinido; por ejemplo, elija **Global Time Filter I** (Filtro de tiempo global) de entre los filtros de Azure Storage para poder filtrar según un intervalo de tiempo. Tras ello, puede editar los parámetros del filtro para especificar la marca de tiempo de inicio y de fin del intervalo que quiera ver. También puede filtrar los datos según un código de estado específico; por ejemplo, puede cargar solo las entradas de registro que tengan un código de estado 404.

Para más información sobre cómo importar datos de registro al analizador de mensajes de Microsoft, vea el tema de [recuperación de datos de mensajes](https://technet.microsoft.com/library/dn772437.aspx) en TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Usar el identificador de solicitud de cliente para poner en correlación los datos de los archivos de registro
La biblioteca de cliente de Azure Storage crea de forma automática un identificador único de solicitud de cliente para cada solicitud. Este valor se escribe en los registros de cliente, de servidor y de seguimiento de red para que se pueda usar para poner en correlación los datos de los tres registros en el analizador de mensajes. Vea [Id. de solicitud de cliente](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) para más información sobre este identificador.

En las siguientes secciones describiremos cómo usar las vistas de diseño personalizadas y configuradas previamente para poner en correlación y agrupar datos según el identificador de solicitud de cliente.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Seleccionar un diseño de vista para mostrarla en la cuadrícula de análisis
Entre las herramientas de almacenamiento del analizador de mensajes encontramos los diseños de vista de Azure Storage, que son vistas ya configuradas que sirven para visualizar los datos y distribuirlos en grupos y columnas de utilidad según el escenario. También puede crear diseños de vista personalizados y guardarlos para volver a usarlos.

En la siguiente imagen se muestra el menú **View Layout** (Diseño de vista), al que puede tener acceso seleccionando **View Layout** (Diseño de vista) en la cinta de opciones de la barra de herramientas. Los diseños de vista de Azure Storage están en el nodo **Azure Storage** del menú. Puede buscar `Azure Storage` en el cuadro de búsqueda para filtrar únicamente por diseños de vista de Azure Storage. Además, puede seleccionar el icono con forma de estrella al lado de cada diseño de vista para marcarlo como favorito y que, de este modo, se muestre en la parte superior del menú.

![Menú de diseño de vista](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Para empezar, seleccione **Grouped by ClientRequestID and Module**(Agrupados por ClientRequestID y Module). Este diseño de vista agrupa los datos de registro de los tres registros de la siguiente manera: primero, por identificador de solicitud de cliente y, después, por archivo de registro de origen (o **Module** en el analizador de mensajes). Con esta vista, podrá explorar en profundidad un identificador de solicitud de cliente en particular y ver los datos de los tres archivos de registro relativos a ese identificador de solicitud de cliente.

En la siguiente imagen puede ver el diseño aplicado a los datos de registro de ejemplo, junto con un subconjunto de columnas. Puede observar que, para un identificador de solicitud de cliente en particular, la cuadrícula de análisis muestra los datos del registro de cliente, de servidor y de seguimiento de red.

![Diseño de vista de Azure Storage](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Los diferentes archivos de registro tienen columnas distintas, así que cuando se muestran los datos de varios archivos de registro en la cuadrícula de análisis, es posible que varias columnas no contengan los datos de alguna fila en particular. Por ejemplo, en la imagen anterior, las filas del registro de cliente no muestran ningún dato en las columnas **Timestamp**, **TimeElapsed**, **Source** y **Destination**, ya que estas columnas no existen en el registro de cliente, pero sí en el de seguimiento de red. Del mismo modo, la columna **Timestamp** muestra los datos de marca de tiempo del registro de servidor, pero no hay datos para las columnas **TimeElapsed**, **Source** y **Destination**, ya que no forman parte del registro de servidor.
>
>

Aparte de usar los diseños de vista de Azure Storage, también puede desarrollar y guardar sus propios diseños de vista. Es más, puede seleccionar otros campos para agrupar los datos y guardar esta agrupación como parte de su diseño personalizado.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Aplicar reglas de color a la cuadrícula de análisis
Las herramientas de almacenamiento incluyen una serie de reglas de color que ofrecen una forma más visual de identificar los diferentes tipos de errores que aparecen en la cuadrícula de análisis. Las reglas de color predefinidas se aplican a los errores HTTP, así que solo aparecerán en el registro de servidor y el seguimiento de red.

Para aplicar reglas de color, seleccione **Color Rules** (Reglas de color) de la cinta de opciones de la barra de herramientas. En el menú verá las reglas de color de Azure Storage. Para el tutorial, seleccione **Client Errors (StatusCode between 400 and 499)**(Errores del cliente [StatusCode entre 400 y 499]), tal y como se muestra en esta imagen.

![Diseño de vista de Azure Storage](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Además de las reglas de color de Azure Storage, también puede crear y guardar sus propias reglas de color.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Agrupar y filtrar datos de registro para encontrar errores de intervalo 400
Ahora agruparemos y filtraremos los datos de registro para encontrar todos los errores que estén dentro del intervalo 400.

1. Localice la columna **StatusCode** en la cuadrícula de análisis, haga clic con el botón secundario en el encabezado de la columna y, después, seleccione **Group** (Agrupar).
2. Tras ello, agrupe por la columna **ClientRequestId** . Verá que los datos de la cuadrícula de análisis están organizados por código de estado y por identificador de solicitud de cliente.
3. Abra la ventana de la herramienta de filtro de vista si aún no está abierta. En la cinta de opciones de la barra de herramientas, seleccione **Tool Windows** (Ventanas de herramientas) y, luego, **View Filter** (Filtro de vista).
4. Para filtrar los datos de registro para que solo se muestren los errores del intervalo 400, agregue el siguiente criterio de filtro a la ventana **View Filter** (Filtro de vista) y haga clic en **Apply** (Aplicar):

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

En la siguiente imagen puede ver los resultados de la agrupación y filtrado. Si, por ejemplo, expande el campo **ClientRequestID** que se encuentra debajo de la agrupación del código de estado 409, podrá ver la operación resultante de ese código de estado.

![Diseño de vista de Azure Storage](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Una vez aplicado este filtro, verá que las filas del registro de cliente se excluyeron, ya que este registro no incluye la columna **StatusCode** . Para empezar, revisaremos los registros de servidor y de seguimiento de red para localizar errores 404 y, después, volveremos al registro de cliente para examinar las operaciones de cliente que llevaron a esos errores.

> [!NOTE]
> Si agrega una expresión al filtro que incluya entradas de registro donde el código de estado sea nulo, puede filtrar la columna **StatusCode** y ver datos de los tres registros (incluido el registro de cliente). Para crear esta expresión de filtro, use:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Este filtro devuelve todas las filas del registro de cliente y solo aquellas filas del registro de servidor y HTTP cuyo código de estado sea mayor que 400. Si aplica esto al diseño de vista que se agrupó por identificador de solicitud de cliente y módulo, podrá buscar o desplazarse por las entradas de registro para encontrar aquellas donde estén representados los tres registros.   
>
>

### <a name="filter-log-data-to-find-404-errors"></a>Filtrar datos de registro para encontrar errores 404
Las herramientas de almacenamiento incluyen filtros predefinidos que puede usar para acotar los datos de registro y, así, dar con los errores o tendencias que esté buscando. Ahora aplicaremos dos filtros predefinidos: uno que filtre los registros de servidor y de seguimiento de red para los errores 404 y otro que filtre los datos de un intervalo de tiempo específico.

1. Abra la ventana de la herramienta de filtro de vista si aún no está abierta. En la cinta de opciones de la barra de herramientas, seleccione **Tool Windows** (Ventanas de herramientas) y, luego, **View Filter** (Filtro de vista).
2. En la ventana de filtro de vista, seleccione **Library** (Biblioteca) y busque en `Azure Storage` para encontrar los filtros de Azure Storage. Seleccione el filtro **404 (Not Found) messages in all logs**(Mensajes 404 [no encontrado] en todos los registros).
3. Vaya de nuevo al menú **Library** (Biblioteca) y localice y seleccione **Global Time Filter** (Filtro de tiempo global).
4. Edite las marcas de tiempo que se muestran en el filtro del intervalo que quiera ver. Esto servirá para reducir el intervalo de datos que va a analizar.
5. Su filtro debería ser similar al que aparece en el siguiente ejemplo. Haga clic en **Apply** (Aplicar) para aplicar el filtro a la cuadrícula de análisis.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Diseño de vista de Azure Storage](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analizar los datos de registro
Ahora que tenemos los datos agrupados y filtrados, ya podemos pasar a examinar los detalles de las solicitudes que ocasionaron errores 404. En el diseño de vista actual, los datos están agrupados por identificador de solicitud de cliente y por origen del registro. Como estamos filtrando solicitudes cuyo campo StatusCode contenga 404, solo veremos los datos de los registros de servidor y de seguimiento de red, no los del registro de cliente.

En la siguiente imagen, podrá ver una solicitud específica en la que una operación Get Blob provocó un error 404 porque el blob no existía. Tenga en cuenta que algunas columnas se quitaron de la vista estándar para poder mostrar los datos relevantes.

![Registros de servidor y de seguimiento de red filtrados](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Ahora, pondremos en correlación el identificador de solicitud de cliente con los datos del registro de cliente para ver qué estaba haciendo el cliente cuando ocurrió el error. Puede mostrar una nueva cuadrícula de análisis (en una segunda pestaña) para esta sesión para ver los datos del registro de cliente:

1. Primero, copie el valor del campo **ClientRequestId** en el portapapeles. Para ello, seleccione una fila, busque el campo **ClientRequestId**, haga clic con el botón secundario en el valor de datos y seleccione **Copy 'ClientRequestId'**.
2. En la cinta de opciones de la barra de herramientas, seleccione **New Viewer** (Nuevo visor) y, luego, **Analysis Grid** (Cuadrícula de análisis) para abrir una nueva pestaña. En la nueva ficha se recogen todos los datos de sus archivos de registro sin agrupar ni filtrar o sin reglas de color.
3. En la cinta de opciones de la barra de herramientas, seleccione **View Layout** (Vista de diseño) y, después, **All .NET Client Columns** (Todas las columnas de cliente .NET) en la sección correspondiente a **Azure Storage**. En este diseño de vista se muestran los datos del registro de cliente, así como los de los registros de servidor y de seguimiento de red. Los datos se ordenan de forma predeterminada por la columna **MessageNumber** .
4. Ahora, buscaremos el registro de cliente del identificador de solicitud de cliente. En la cinta de opciones de la barra de herramientas, seleccione **Find Messages** (Buscar mensajes) y especifique un filtro personalizado en el identificador de solicitud de cliente en el campo **Find** (Buscar). Use esta sintaxis para el filtro, indicando su propio identificador de solicitud de cliente:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

El analizador de mensajes encuentra y selecciona la primera entrada de registro en la que el criterio de búsqueda coincida con el identificador de solicitud de cliente. En el registro de cliente existen varias entradas por cada identificador de solicitud de cliente, así que puede agruparlas por el campo **ClientRequestId** para que sea más fácil revisarlas. En la siguiente imagen puede ver todos los mensajes del registro de cliente correspondientes al identificador de solicitud de cliente especificado.

![Registro de cliente con errores 404](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Si usa los datos que se muestran en los diseños de vista de estas dos pestañas, podrá analizar los datos de solicitud para averiguar qué es lo que provocó el error. También puede echar un vistazo a las solicitudes anteriores a esta para ver si algún evento previo fue el que causó el error 404. Por ejemplo, puede revisar las entradas del registro de cliente de este identificador de solicitud de cliente para saber si el blob se eliminó o si el error se produjo porque la aplicación cliente llamó a una API de tipo CreateIfNotExists en un contenedor o un blob. En el registro de cliente, encontrará las direcciones del blob en el campo de **Description** (Descripción), mientras que en los registros de servidor y de seguimiento de red esta información aparece recogida en el campo **Summary** (Resumen).

Cuando sepa qué dirección del blob produjo el error 404, podrá realizar un examen más exhaustivo. Si, en las entradas de registro, busca otros mensajes relacionados operaciones en el mismo blob, podrá saber si el cliente ya había eliminado la entidad.

## <a name="analyze-other-types-of-storage-errors"></a>Analizar otros tipos de errores de almacenamiento
Ahora que ya está familiarizado con el analizador de mensajes y su uso para analizar los datos de sus registros, podrá analizar otros tipos de errores usando diseños de vista, reglas de color y criterios de búsqueda o filtrado. En las siguientes tablas se muestran distintos problemas con los que podría encontrarse y los criterios de filtrado que puede usar para poder localizarlos. Para más información sobre cómo crear filtros y sobre el lenguaje de filtrado del analizador de mensajes, vea el tema sobre el [filtrado de datos de mensajes](https://technet.microsoft.com/library/jj819365.aspx).

| Para investigar... | Use la expresión de filtro... | La expresión se aplica al registro (de cliente, de servidor, de red, todos) |
| --- | --- | --- |
| Retrasos inesperados en la entrega de mensajes en una cola |AzureStorageClientDotNetV4.Description contiene "Intentando de nuevo la operación con error." |Cliente |
| Aumento de HTTP en PercentThrottlingError |HTTP.Response.StatusCode   == 500 &#124;&#124; HTTP.Response.StatusCode == 503 |Red |
| Aumento en PercentTimeoutError |HTTP.Response.StatusCode   == 500 |Red |
| Aumento en PercentTimeoutError (todos) |*StatusCode   == 500 |Todo |
| Aumento de PercentNetworkError |AzureStorageClientDotNetV4.EventLogEntry.Level   < 2 |Cliente |
| Mensajes HTTP 403 (Prohibido) |HTTP.Response.StatusCode   == 403 |Red |
| Mensajes HTTP 404 (No encontrado) |HTTP.Response.StatusCode   == 404 |Red |
| 404 (Todo) |*StatusCode   == 404 |Todo |
| Problema de autorización de la Firma de acceso compartido (SAS) |AzureStorageLog.RequestStatus ==  "SASAuthorizationError" |Red |
| Mensajes HTTP 409 (conflicto) |HTTP.Response.StatusCode   == 409 |Red |
| 409 (Todo) |*StatusCode   == 409 |Todo |
| Valor de PercentSuccess bajo o las entradas de registro de análisis tienen operaciones con el estado de transacción ClientOtherErrors |AzureStorageLog.RequestStatus ==   "ClientOtherError" |Server |
| Advertencia de Nagle |((AzureStorageLog.EndToEndLatencyMS   - AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS *   1.5)) y (AzureStorageLog.RequestPacketSize <1460) y (AzureStorageLog.EndToEndLatencyMS -   AzureStorageLog.ServerLatencyMS >= 200) |Server |
| Intervalo de tiempo en los registros de servidor y red |#Timestamp   >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39 |Servidor, red |
| Intervalo de tiempo en registros de servidor |AzureStorageLog.Timestamp   >= 2014-10-20T16:36:38 y AzureStorageLog.Timestamp <=   2014-10-20T16:36:39 |Server |

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre los escenarios de solución integral de problemas en Azure Storage, vea los siguientes recursos:

* [Supervisión, diagnóstico y solución de problemas de Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)
* [Storage Analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Supervisión de una cuenta de almacenamiento en Azure Portal](storage-monitor-storage-account.md)
* [Transferencia de datos con la utilidad en línea de comandos AzCopy](storage-use-azcopy.md)
* [Guía de funcionamiento del analizador de mensajes de Microsoft](https://technet.microsoft.com/library/jj649776.aspx)

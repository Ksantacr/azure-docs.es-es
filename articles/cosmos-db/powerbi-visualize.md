---
title: Tutorial de Power BI para el conector de Azure Cosmos DB
description: Use este tutorial de Power BI para importar JSON, crear informes muy precisos y visualizar datos mediante el conector de Azure Cosmos DB y Power BI.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: sngun
ms.openlocfilehash: 76531de279dfe6e9b73b3895f0ef63c4c88b63cd
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979004"
---
# <a name="visualize-azure-cosmos-db-data-by-using-the-power-bi-connector"></a>Visualizar datos de Azure Cosmos DB mediante el conector de Power BI

[Power BI](https://powerbi.microsoft.com/) es un servicio en línea donde puede crear y compartir paneles e informes. Power BI Desktop es una herramienta de creación de informes con la que puede recuperar datos desde diversos orígenes de datos. Azure Cosmos DB es uno de los orígenes de datos que puede usar con Power BI Desktop. Puede conectar Power BI Desktop a la cuenta de Azure Cosmos DB mediante el conector Azure Cosmos DB para Power BI.  Después de importar los datos de Azure Cosmos DB a Power BI, puede transformarlos, crear informes y publicarlos en Power BI.   

En este artículo se describen los pasos necesarios para conectar la cuenta de Azure Cosmos DB a Power BI Desktop. Después de conectarla, vaya a una colección, extraiga los datos, transforme los datos JSON en formato tabular y publique un informe en Power BI.

> [!NOTE]
> El conector de Power BI para Azure Cosmos DB se conecta a Power BI Desktop. Los informes que se crean con Power BI Desktop se pueden publicar en PowerBI.com. No se puede realizar la extracción directa de datos de Azure Cosmos DB desde PowerBI.com. 

> [!NOTE]
> Actualmente, la conexión a Azure Cosmos DB con el conector de Power BI solo se admite en las cuentas de Azure Cosmos DB SQL API y de Gremlin API.

## <a name="prerequisites"></a>Requisitos previos
Antes de seguir las instrucciones de este tutorial de Power BI, asegúrese de que tiene acceso a los siguientes recursos:

* [Descargue la versión más reciente de Power BI Desktop](https://powerbi.microsoft.com/desktop).

* Descargue los [datos de volcanes de ejemplo](https://github.com/Azure-Samples/azure-cosmos-db-sample-data/blob/master/SampleData/VolcanoData.json) de GitHub.

* [Cree una cuenta de base de datos de Azure Cosmos DB](https://azure.microsoft.com/documentation/articles/create-account/) e importe los datos de volcanes mediante la [herramienta de migración de datos de Azure Cosmos DB](import-data.md). Al importar datos, tenga en cuenta las opciones siguientes para el origen y los destinos en la herramienta de migración de datos:

   * **Parámetros de origen** 

       * **Importar desde:** Archivo JSON

   * **Parámetros de destino** 

      * **Cadena de conexión:** `AccountEndpoint=<Your_account_endpoint>;AccountKey=<Your_primary_or_secondary_key>;Database= <Your_database_name>` 

      * **Clave de partición:** /País 

      * **Rendimiento de la colección:** 1000 

Para compartir los informes en PowerBI.com, debe tener una cuenta en PowerBI.com.  Para más información sobre Power BI y Power BI Pro, visite [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Comencemos
En este tutorial, imaginemos que usted es un geólogo que estudia los volcanes de todo el mundo. Los datos de los volcanes se almacenan en una cuenta de Azure Cosmos DB y el formato de documento JSON tiene este aspecto:

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Recuperará los datos de los volcanes de la cuenta de Azure Cosmos DB y visualizará los datos en un informe de Power BI interactivo.

1. Ejecute Power BI Desktop.

2. También puede **obtener datos**, consultar **orígenes recientes** o **abrir otros informes** directamente desde la pantalla de bienvenida. Haga clic en la X de la esquina superior derecha para cerrar la pantalla. Se muestra la vista **Informe** de Power BI Desktop.
   
   ![Vista de informes de Power BI Desktop: conector de Power BI](./media/powerbi-visualize/power_bi_connector_pbireportview.png)

3. Seleccione la cinta de opciones **Inicio** y haga clic en **Obtener datos**.  Se muestra la pantalla **Obtener datos** .

4. Haga clic en **Azure**, seleccione **Azure Cosmos DB (beta)** y, a continuación, haga clic en **Conectar**. 

    ![Obtención de datos de Power BI Desktop: conector de Power BI](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   

5. En la página **Vista previa del conector**, haga clic en **Continuar**. Aparece la ventana de **Azure Cosmos DB**.

6. Especifique la dirección URL del punto de conexión de la cuenta de Azure Cosmos DB de la que desea recuperar los datos, tal como se muestra a continuación, y haga clic en **Aceptar**. Para usar su propia cuenta, puede recuperar la dirección URL del cuadro te texto Identificador URI de la hoja **Claves** de Azure Portal. Si lo prefiere, puede indicar el nombre de la base de datos, el nombre de la colección o usar el navegador para seleccionar la base de datos y la colección para identificar la procedencia de los datos.
   
7. Si se conecta a este punto de conexión por primera vez, se le pedirá la clave de cuenta. Para usar su propia cuenta, recupere la clave del cuadro **Clave principal** de la hoja **Claves de solo lectura** de Azure Portal. Escriba la clave adecuada y, a continuación, haga clic en **Conectar**.
   
   Se recomienda usar la clave de solo lectura al generar informes. De esta forma se evita una exposición innecesaria de la clave maestra a posibles riesgos de seguridad. La clave de solo lectura está disponible desde la hoja **Claves** de Azure Portal. 
    
8. Con la cuenta conectada correctamente, aparece el panel **Navegador**. El panel **Navegador** muestra la lista de bases de datos que hay en la cuenta.

9. Haga clic y expanda la base de datos de donde proceden los datos del informe; seleccione **volcanodb** (el nombre de la base de datos puede ser diferente).   

10. Seleccione una colección que contenga los datos que se van a recuperar, seleccione **volcano1** (el nombre de la colección puede ser diferente).
    
    El panel de vista previa muestra una lista de elementos **Registro** .  Un documento se representa como un tipo **Registro** en Power BI. De forma similar, un bloque JSON anidado dentro de un documento también es un **Registro**.
    
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Ventana del navegador](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Haga clic en **Editar** para iniciar el Editor de consultas en una nueva ventana para transformar los datos.

## <a name="flattening-and-transforming-json-documents"></a>Eliminación de formato y transformación de documentos JSON
1. Cambie a la ventana del Editor de consultas de Power BI, donde debería ver la columna **Documento** en el panel central.
   ![Editor de consultas de Power BI Desktop](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Haga clic en el botón de expansión en el lado derecho del encabezado de columna **Documento** .  Se muestra el menú contextual con una lista de campos.  Seleccione los campos necesarios para el informe, por ejemplo, nombre de volcán, país, región, ubicación, elevación, tipo, estado y última erupción conocida. Anule la selección del cuadro **Use original column name as prefix** (Usar nombre de columna original como prefijo) y haga clic en **Aceptar**.
   
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Expandir documentos](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. El panel central muestra una vista previa del resultado con los campos seleccionados.
   
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Resultados sin formato](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. En nuestro ejemplo, la propiedad Ubicación es un bloque de GeoJSON en un documento.  Como puede ver, la ubicación se representa como un tipo **Registro** en Power BI Desktop.  
5. Haga clic en el botón de expansión en el lado derecho del encabezado de columna Document.Location.  Aparece el menú contextual con los campos de tipo y coordenadas.  Vamos a seleccionar el campo de coordenadas, asegúrese de que **Use original column name as prefix** (Usar nombre de columna original como prefijo) no esté seleccionado y haga clic en **Aceptar**.
   
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Registro de ubicación](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. El panel central muestra ahora una columna de coordenadas de tipo **lista** .  Como se ha indicado al comienzo del tutorial, los datos de GeoJSON son de tipo Point con los valores de latitud y longitud registrados en la matriz de coordenadas.
   
    El elemento coordinates[0] representa la longitud, mientras que el elemento coordinates[1] representa la latitud.
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Lista de coordenadas](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Para eliminar el formato de la matriz de coordenadas, cree una **columna personalizada** llamada LatLong.  Seleccione la cinta de opciones **Agregar columna** y haga clic en **Columna personalizada**.  Aparece la ventana de **Columna personalizada**.
8. Dele un nombre a la nueva columna, por ejemplo, LatLong.
9. Después, especifique la fórmula personalizada para la nueva columna.  En nuestro ejemplo, concatenaremos los valores de latitud y longitud separados por comas, como se muestra a continuación mediante la fórmula siguiente: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Haga clic en **OK**.
   
    Para obtener más información sobre las expresiones de análisis de datos (DAX) incluidas las funciones DAX, visite [aspectos básicos de DAX en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-quickstart-learn-dax-basics).
   
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Agregar columna personalizada](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Ahora, el panel central muestra las nuevas columnas LatLong rellenadas con los valores.
    
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Columna LatLong personalizada](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Si recibe un error en la columna nueva, asegúrese de que los pasos aplicados en Configuración la consulta coinciden con la siguiente figura:
    
    ![Los pasos aplicados deben ser los siguientes: Source, Navigation, Expanded Document, Expanded Document.Location, Added Custom](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Si los pasos son distintos, elimine los pasos adicionales e intente agregar nuevamente la columna personalizada. 

11. Haga clic en **Cerrar y aplicar** para guardar el modelo de datos.
    
    ![Tutorial de Power BI para conector de Power BI de Azure Cosmos DB - Cerrar y aplicar](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Generación de informes
La vista de informe de Power BI Desktop es donde puede empezar a crear informes para visualizar datos.  Puede crear informes arrastrando y soltando campos en el lienzo del **informe** .

![Vista de informes de Power BI Desktop: conector de Power BI](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

En la vista de informe, debe buscar:

1. El panel **Campos** es donde verá una lista de los modelos de datos con campos que puede usar para los informes.
2. El panel **Visualizaciones** . Un informe puede contener una o varias visualizaciones.  Seleccione los tipos visuales que se ajusten a sus necesidades en el panel **Visualizaciones** .
3. El lienzo **Informe** es donde se compilan los objetos visuales para el informe.
4. La página **Informe** . Puede agregar varias páginas de informes en Power BI Desktop.

A continuación se muestran los pasos básicos para crear un sencillo informe de la vista de mapa interactivo.

1. En nuestro ejemplo, crearemos una vista del mapa que muestra la ubicación de cada volcán.  En el panel **Visualizaciones** , haga clic en el tipo de mapa visual que aparece destacado en la captura de pantalla anterior.  Debería ver el tipo de mapa visual pintado en el lienzo **Informe** .  El panel de **visualizaciones** también debe mostrar un conjunto de propiedades relacionadas con el tipo de mapa visual.
2. Ahora, arrastre y coloque el campo LatLong desde el panel de **campos** a la **bicación** del panel de **visualizaciones**.
3. A continuación, arrastre y coloque el campo del nombre del volcán en la propiedad **Leyenda** .  
4. Luego, arrastre y coloque el campo Elevación en la propiedad **Tamaño** .  
5. Ahora debería ver el mapa visual que muestra un conjunto de burbujas que indican la ubicación de cada volcán, con el tamaño de la burbuja correlacionado con la elevación del volcán.
6. Ahora ha creado un informe básico.  Puede personalizar aún más el informe agregando más visualizaciones.  En nuestro caso, hemos agregado una segmentación del tipo de volcán para que el informe sea interactivo.  
   
7. En el menú Archivo, haga clic en **Guardar** y guarde el archivo como PowerBITutorial.pbix.

## <a name="publish-and-share-your-report"></a>Publicación y uso compartido del informe
Para compartir el informe, debe tener una cuenta en PowerBI.com.

1. En Power BI Desktop, haga clic en la cinta de opciones de **inicio** .
2. Haga clic en **Publicar**.  Se le pedirá que escriba el nombre de usuario y la contraseña de su cuenta de PowerBI.com.
3. Una vez que se autentique la credencial, el informe se publicará en el destino que seleccione.
4. Haga clic en **Abrir 'PowerBITutorial.pbix' en Power BI** para ver y compartir su informe en PowerBI.com.
   
    ![La publicación en Power BI se realizó correctamente Abrir tutorial de Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Creación de un panel en PowerBi.com
Ahora que tiene un informe, vamos a compartirlo en PowerBi.com

Cuando publica su informe de Power BI Desktop en PowerBI.com, se genera un **informe** y un **conjunto de datos** en su inquilino de PowerBI.com. Por ejemplo, después de que publique un informe llamado **PowerBITutorial** en PowerBI.com, podrá ver PowerBITutorial tanto en la sección de **informes** como en la sección de **onjuntos de datos** en PowerBI.com.

   ![Captura de pantalla del nuevo Informe y base de datos en PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

Para crear un panel que se puede compartir, haga clic en el botón de la **página Anclar elemento activo** del informe PowerBI.com.

   ![Captura de pantalla del nuevo Informe y base de datos en PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Luego, siga las instrucciones que aparecen en [Anclar un icono de un informe](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) para crear un panel nuevo. 

También puede realizar modificaciones ad hoc en el informe antes de crear un panel. Sin embargo, se recomienda que use Power BI Desktop para realizar las modificaciones y vuelva a publicar el informe en PowerBI.com.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Pasos siguientes
* Para más información sobre Power BI, consulte [Introducción a Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Para más información sobre Azure Cosmos DB, consulte la [página de aterrizaje de la documentación de Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).


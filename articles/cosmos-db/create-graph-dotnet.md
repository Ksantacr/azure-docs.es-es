---
title: Creación de una aplicación .NET Framework o Core para Azure Cosmos DB mediante Gremlin API
description: En este tema se incluye un ejemplo de código .NET Framework o Core que puede usar para conectarse a Azure Cosmos DB y realizar consultas.
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: lbosq
ms.openlocfilehash: 24d5c11eb32350b2c11584ca5fc75ed4b619b6cf
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978747"
---
# <a name="quickstart-build-a-net-framework-or-core-application-using-the-azure-cosmos-db-gremlin-api-account"></a>Inicio rápido: Compilación de una aplicación .NET Framework o Core mediante la cuenta de Gremlin API de Azure Cosmos DB

> [!div class="op_single_selector"]
> * [Gremlin Console](create-graph-gremlin-console.md)
> * [.NET](create-graph-dotnet.md)
> * [Java](create-graph-java.md)
> * [Node.js](create-graph-nodejs.md)
> * [Python](create-graph-python.md)
> * [PHP](create-graph-php.md)
>  

Azure Cosmos DB es un servicio de base de datos con varios modelos y de distribución global de Microsoft. Puede crear rápidamente bases de datos de documentos, clave-valor y grafos, y realizar consultas en ellas. Todas las bases de datos se beneficiarán de las funcionalidades de distribución global y escala horizontal en Azure Cosmos DB. 

En esta guía de inicio rápido se muestra cómo crear una cuenta de [Gremlin API](graph-introduction.md), una base de datos y un grafo (contenedor) de Azure Cosmos DB mediante Azure Portal. Después, compile y ejecute una aplicación de consola compilada con el controlador [Gremlin.Net](https://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet) de código abierto.  

## <a name="prerequisites"></a>Requisitos previos

Si aún no tiene instalado Visual Studio 2019, puede descargar y usar la versión **gratuita** de [Visual Studio 2019 Community](https://www.visualstudio.com/downloads/). Asegúrese de que habilita **Desarrollo de Azure** durante la instalación de Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Agregar un grafo

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

Ahora, vamos a clonar una aplicación de Gremlin API desde GitHub, establecer la cadena de conexión y ejecutarla. Verá lo fácil que es trabajar con datos mediante programación. 

1. Abra un símbolo del sistema, cree una carpeta nueva denominada ejemplos de GIT y, después, cierre el símbolo del sistema.

    ```bash
    md "C:\git-samples"
    ```

2. Abra una ventana de terminal de Git, como git bash y utilice el comando `cd` para cambiar a la nueva carpeta para instalar la aplicación de ejemplo.

    ```bash
    cd "C:\git-samples"
    ```

3. Ejecute el comando siguiente para clonar el repositorio de ejemplo. Este comando crea una copia de la aplicación de ejemplo en el equipo.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-gremlindotnet-getting-started.git
    ```

4. A continuación, abra Visual Studio y el archivo de solución.

5. Restaure los paquetes de NuGet en el proyecto. Debe incluir el controlador Gremlin.Net, así como el paquete Newtonsoft.Json.


6. También puede instalar manualmente el controlador Gremlin.Net mediante el administrador de paquetes NuGet o la [utilidad de línea de comandos de NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools): 

    ```bash
    nuget install Gremlin.Net
    ```

## <a name="review-the-code"></a>Revisión del código

Este paso es opcional. Si está interesado en aprender cómo se crean los recursos de base de datos en el código, puede revisar los siguientes fragmentos de código. En caso contrario, puede ir directamente a [Actualización de la cadena de conexión](#update-your-connection-string). 

Los fragmentos de código siguientes se han tomado del archivo Program.cs.

* Establezca los parámetros de conexión en función de la cuenta creada anteriormente (línea 19): 

    ```csharp
    private static string hostname = "your-endpoint.gremlin.cosmosdb.azure.com";
    private static int port = 443;
    private static string authKey = "your-authentication-key";
    private static string database = "your-database";
    private static string collection = "your-graph-container";
    ```

* Se muestran los comandos de Gremlin que se van a ejecutar en un diccionario (línea 26):

    ```csharp
    private static Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
    {
        { "Cleanup",        "g.V().drop()" },
        { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
        { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
        { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
        { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
        { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
        { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
        { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
        { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
        { "CountVertices",  "g.V().count()" },
        { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
        { "Project",        "g.V().hasLabel('person').values('firstName')" },
        { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
        { "Traverse",       "g.V('thomas').out('knows').hasLabel('person')" },
        { "Traverse 2x",    "g.V('thomas').out('knows').hasLabel('person').out('knows').hasLabel('person')" },
        { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
        { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
        { "CountEdges",     "g.E().count()" },
        { "DropVertex",     "g.V('thomas').drop()" },
    };
    ```


* Cree un objeto de conexión `GremlinServer` con los parámetros proporcionados anteriormente (línea 52):

    ```csharp
    var gremlinServer = new GremlinServer(hostname, port, enableSsl: true, 
                                                    username: "/dbs/" + database + "/colls/" + collection, 
                                                    password: authKey);
    ```

* Cree un nuevo objeto `GremlinClient` (línea 56):

    ```csharp
    var gremlinClient = new GremlinClient(gremlinServer);
    ```

* Ejecute cada consulta de Gremlin con el objeto `GremlinClient` con una tarea asincrónica (línea 63). Leerá las consultas de Gremlin desde el diccionario definido anteriormente (línea 26):

    ```csharp
    var results = await gremlinClient.SubmitAsync<dynamic>(query.Value);
    ```

* Recupere el resultado y lea los valores, que tienen un formato de diccionario, usando la clase `JsonSerializer` de Newtonsoft.Json:

    ```csharp
    foreach (var result in results)
    {
        // The vertex results are formed as dictionaries with a nested dictionary for their properties
        string output = JsonConvert.SerializeObject(result);
        Console.WriteLine(String.Format("\tResult:\n\t{0}", output));
    }
    ```

## <a name="update-your-connection-string"></a>Actualización de la cadena de conexión

Ahora vuelva a Azure Portal para obtener la información de la cadena de conexión y cópiela en la aplicación.

1. En [Azure Portal](https://portal.azure.com/), vaya a la cuenta de base de datos de grafos. En la pestaña **Información general**, puede ver dos puntos de conexión. 
 
   **URI del SDK de .NET**: este valor se usa al conectarse a la cuenta de grafos mediante la biblioteca de Microsoft.Azure.Graphs. 

   **Punto de conexión de Gremlin**: este valor se usa cuando se conecta a la cuenta de grafos mediante la biblioteca Gremlin.Net.

    ![Copiar el punto de conexión](./media/create-graph-dotnet/endpoint.png)

   Para ejecutar este ejemplo, copie el valor de **Punto de conexión de Gremlin** y elimine el número de puerto del final con lo que el identificador URI se convertirá en `https://<your cosmos db account name>.gremlin.cosmosdb.azure.com`.

2. En Program.cs, pegue el valor por encima de `your-endpoint` en la variable `hostname` en la línea 19. 

    `"private static string hostname = "<your cosmos db account name>.gremlin.cosmosdb.azure.com";`

    El valor del punto de conexión debe tener el siguiente aspecto:

    `"private static string hostname = "testgraphacct.gremlin.cosmosdb.azure.com";`

3. A continuación, vaya a la pestaña **Claves** y copie el valor **PRIMARY KEY** del portal y péguelo en la variable `authkey`, reemplazando el marcador de posición `"your-authentication-key"` de la línea 21. 

    `private static string authKey = "your-authentication-key";`

4. Con la información de la base de datos que se ha creado anteriormente, pegue el nombre de la base de datos dentro de la variable `database` en la línea 22. 

    `private static string database = "your-database";`

5. De forma similar, con la información del contenedor que se ha creado anteriormente, pegue la colección (que también incluye el nombre del grafo) en la variable `collection` de la línea 23. 

    `private static string collection = "your-collection-or-graph";`

6. Guarde el archivo Program.cs. 

Ya ha actualizado la aplicación con toda la información que necesita para comunicarse con Azure Cosmos DB. 

## <a name="run-the-console-app"></a>Ejecutar la aplicación de consola

Haga clic en CTRL + F5 para ejecutar la aplicación. La aplicación imprimirá los resultados de la consola y los comandos de consulta de Gremlin.

   En la ventana de la consola se muestran los vértices y los bordes que se agregan al grafo. Cuando se complete el script, presione ENTRAR para cerrar la ventana de la consola.

## <a name="browse-using-the-data-explorer"></a>Examinar mediante el Explorador de datos

Ahora puede volver al Explorador de datos en Azure Portal para examinar y consultar los datos del nuevo grafo.

1. En el Explorador de datos, la nueva base de datos aparece en el panel Grafos. Expanda los nodos del contenedor y de la base de datos y, a continuación, haga clic en **Grafo**.

2. Haga clic en el botón **Aplicar filtro** para usar la consulta predeterminada para visualizar todos los vértices del grafo. Los datos generados por la aplicación de ejemplo se muestran en el panel grafos.

    Puede acercar o alejar el grafo, expandir el espacio de visualización del grafo, agregar vértices adicionales y mover los vértices sobre la superficie de visualización.

    ![Visualización del grafo en el Explorador de datos en Azure Portal](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Revisión de los SLA en Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha obtenido información sobre cómo crear una cuenta de Azure Cosmos DB, crear un grafo mediante el Explorador de datos y ejecutar una aplicación. Ahora puede crear consultas más complejas e implementar con Gremlin una lógica eficaz de recorrido del grafo. 

> [!div class="nextstepaction"]
> [Consulta mediante Gremlin](tutorial-query-graph.md)


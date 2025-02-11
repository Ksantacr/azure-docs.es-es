---
title: 'Cómo usar Azure Search desde una aplicación .NET: Azure Search'
description: Obtenga información sobre cómo usar Azure Search en una aplicación de .NET con C# y el SDK de. NET. Las tareas basadas en código incluyen la conexión al servicio, el contenido del índice y la consulta de un índice.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: 25a156c4403b7a89f7a7bf7f6acf22fa34216791
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025126"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Cómo usar Azure Search desde una aplicación .NET

> [!Important]
> Este contenido todavía está en construcción. La versión 9.0 del SDK de .NET de Azure Search está disponible en NuGet. Estamos trabajando en esta guía de migración para explicar cómo actualizar a la nueva versión de la actualización. Permanezca atento.
>

Este artículo es un tutorial para empezar a trabajar con el [SDK de Azure Search para .NET](https://aka.ms/search-sdk). Puede utilizar el SDK para .NET para implementar una experiencia de búsqueda enriquecida en la aplicación mediante Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Qué es el SDK de Azure Search
El SDK se compone de unas cuantas bibliotecas de cliente que le permiten administrar los índices, orígenes de datos, indexadores y asignaciones de sinónimos, así como cargar y administrar documentos y ejecutar consultas; todo ello sin tener que lidiar con los detalles de HTTP y JSON. Esas bibliotecas de cliente se distribuyen como paquetes de NuGet.

El paquete de NuGet principal es `Microsoft.Azure.Search`, que es un metapaquete que incluye el resto de paquetes a modo de dependencias. Use este paquete si está comenzando a trabajar con él o si sabe que la aplicación necesitará todas las características de Azure Search.

Estos son otros paquetes de NuGet que tiene el SDK:
 
  - `Microsoft.Azure.Search.Data`: use este paquete si va a desarrollar una aplicación de .NET mediante Azure Search y solo necesita consultar o actualizar los documentos en los índices. Si también tiene que crear o actualizar los índices, las asignaciones de sinónimos u otros recursos de nivel de servicio, use el paquete `Microsoft.Azure.Search`.
  - `Microsoft.Azure.Search.Service`: use este paquete si está desarrollando la automatización en .NET para administrar los índices, las asignaciones de sinónimos, los indexadores, los orígenes de datos u otros recursos de nivel de servicio de Azure Search. Si solo necesita consultar o actualizar los documentos de los índices, use el paquete `Microsoft.Azure.Search.Data` en su lugar. Si necesita toda la funcionalidad de Azure Search, use el paquete `Microsoft.Azure.Search`.
  - `Microsoft.Azure.Search.Common`: tipos comunes que necesitan las bibliotecas de .NET de Azure Search. No es necesario usar este paquete directamente en la aplicación; solo debe usarse como una dependencia.

Las diferentes bibliotecas de cliente definen clases como `Index`, `Field` y `Document`, además de operaciones como `Indexes.Create` y `Documents.Search` en las clases `SearchServiceClient` y `SearchIndexClient`. Estas clases están organizadas en los espacios de nombres siguientes:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

La versión actual del SDK de .NET para Azure Search ya está disponible con carácter general. Si desea enviarnos comentarios para que los tengamos en cuenta en próxima versión, visite nuestra [página de comentarios](https://feedback.azure.com/forums/263029-azure-search/).

El SDK para .NET es compatible con la versión `2017-11-11` de la [API de REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/). En esta versión se incluye la compatibilidad con sinónimos, así como mejoras incrementales para los indexadores. 

Este SDK no admite [operaciones de administración](https://docs.microsoft.com/rest/api/searchmanagement/) como la creación y el escalado de servicios de Search y las claves de API de administración. Si necesita administrar los recursos de Search desde una aplicación .NET, puede usar el [SDK de administración para .NET de Azure Search](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Actualización a la versión más reciente del SDK
Si ya utiliza una versión anterior del SDK de .NET para Azure Search y desea actualizar a la nueva versión disponible, en [este artículo](search-dotnet-sdk-migration-version-5.md) se explica el proceso.

## <a name="requirements-for-the-sdk"></a>Requisitos para el SDK
1. Visual Studio 2017.
2. Su propio servicio Azure Search. Para usar el SDK, será necesario el nombre del servicio y una o varias claves de API. [Crear un servicio en el portal](search-create-service-portal.md) le ayudará con estos pasos.
3. Descargue el [paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.Search) del SDK de Azure Search para .NET mediante "Administrar paquetes de NuGet" en Visual Studio. Simplemente busque el nombre del paquete `Microsoft.Azure.Search` en NuGet.org (o uno de los otros nombres de paquetes mencionados anteriormente si solo necesita un subconjunto de la funcionalidad).

El SDK de .NET de Azure Search es compatible con las aplicaciones destinadas a .NET Framework 4.5.2 y otras versiones superiores, así como a .NET Core.

## <a name="core-scenarios"></a>Escenarios principales
Hay varias tareas que debe realizar en su aplicación de búsqueda. En este tutorial, hablaremos sobre estos escenarios básicos:

* Creación de un índice
* Llenado del índice con documentos
* Búsqueda de documentos mediante filtros y búsqueda de texto completo

El código de ejemplo siguiente muestra cada uno de ellos. No dude en usar los fragmentos de código en su propia aplicación.

### <a name="overview"></a>Información general
La aplicación de ejemplo que vamos a explorar crea un nuevo índice denominado "hotels", lo rellena con varios documentos y, a continuación, ejecuta varias consultas de búsqueda. Este es el programa principal, que muestra el flujo general:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Puede encontrar el código fuente completo de la aplicación de ejemplo usada en este tutorial en [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Lo recorreremos paso a paso. Primero, debemos crear un nuevo `SearchServiceClient`. Este objeto le permite administrar índices. Para crear uno, deberá proporcionar el nombre de su servicio Azure Search y una clave de API de administración. Puede escribir esta información en el archivo `appsettings.json` de la [aplicación de ejemplo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Si proporciona una clave incorrecta (por ejemplo, una clave de consulta cuando se requiere una clave de administración), el `SearchServiceClient` producirá un `CloudException` con el mensaje de error "Forbidden" (Prohibido) cuando llame a un método de operación con él, como `Indexes.Create`. Si esto sucede, compruebe la clave de API.
> 
> 

Las siguientes líneas llaman a métodos para crear un índice llamado "hotels", eliminándolo antes si ya existe. Describiremos estos métodos más adelante.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

A continuación, el índice debe rellenarse. Para ello, necesitamos un `SearchIndexClient`. Hay dos maneras de obtener uno: se puede crear o se puede llamar a `Indexes.GetClient` en el `SearchServiceClient`. Usamos esta última por motivos de comodidad.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> En una aplicación de búsqueda típica, el llenado y la administración de índices se controlan mediante un componente independiente de las consultas de búsqueda. `Indexes.GetClient` resulta cómodo para rellenar un índice porque evita la molestia de dar otro `SearchCredentials`. Con este fin, pasa la clave de administrador que se usó para crear el `SearchServiceClient` al nuevo `SearchIndexClient`. Sin embargo, en la parte de la aplicación que ejecuta consultas, es mejor crear directamente el `SearchIndexClient` para poder pasar una clave de consulta en lugar de una clave de administración. Esto está en consonancia con el principio de privilegios mínimos y le ayudará a proteger su aplicación. Puede encontrar más información acerca de las claves de administración y consulta [aquí](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Ahora que tenemos un `SearchIndexClient`, podemos rellenar el índice. Para ello utilizaremos otro método que tratamos más adelante.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Finalmente, ejecutamos algunas consultas de búsqueda y mostramos los resultados. Esta vez usamos otro `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Echaremos un vistazo al método `RunQueries` más adelante. Este es el código para crear el nuevo elemento `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Esta vez usamos una clave de consulta dado que no necesitamos acceso de escritura al índice. Puede escribir esta información en el archivo `appsettings.json` de la [aplicación de ejemplo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Si ejecuta esta aplicación con un nombre de servicio válido y claves de API, la salida será parecida a esta:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

El código fuente completo de la aplicación se proporciona al final de este artículo.

A continuación, veremos más de cerca cada uno de los métodos llamados por `Main`.

### <a name="creating-an-index"></a>Creación de un índice
Después de crear un `SearchServiceClient`, `Main` elimina el índice "hotels" Si ya existe. Esto se lleva a cabo mediante el método siguiente:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Este método utiliza el `SearchServiceClient` proporcionado para comprobar si existe el índice y, en ese caso, eliminarlo.

> [!NOTE]
> El código de ejemplo de este artículo usa los métodos sincrónicos del SDK de Azure Search para .NET por motivos de simplicidad. En sus propias aplicaciones, recomendamos que use métodos asincrónicos para mantener su escalabilidad y capacidad de respuesta. Por ejemplo, en el método anterior podría utilizar `ExistsAsync` y `DeleteAsync` en lugar de `Exists` y `Delete`.
> 
> 

A continuación, `Main` crea un nuevo índice "hotels" mediante una llamada a este método:

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

Este método crea un nuevo objeto `Index` con una lista de objetos `Field` que define el esquema del nuevo índice. Cada campo tiene un nombre, un tipo de datos y varios atributos que definen su comportamiento de búsqueda. La clase `FieldBuilder` usa reflexión para crear una lista de objetos `Field` para el índice examinando las propiedades públicas y los atributos de la clase de modelo `Hotel` dada. Examinaremos más de cerca la clase `Hotel` más adelante.

> [!NOTE]
> En caso necesario, siempre puede crear la lista de objetos `Field` directamente en lugar de usar `FieldBuilder`. Por ejemplo, puede que no quiera usar una clase model o que necesite usar una clase model existente que no quiere modificar agregando atributos.
>
> 

Además de campos, puede agregar al índice perfiles de puntuación, proveedores de sugerencias u opciones de CORS (se omiten en el ejemplo para mayor brevedad). Puede encontrar más información sobre el objeto Index y sus partes constituyentes en la [referencia del SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), así como en la [referencia de la API de REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Llenado del índice
El siguiente paso en `Main` consiste en rellenar el índice recién creado. Esto se lleva a cabo mediante el método siguiente:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        { 
            HotelId = "1", 
            BaseRate = 199.0, 
            Description = "Best hotel in town",
            DescriptionFr = "Meilleur hôtel en ville",
            HotelName = "Fancy Stay",
            Category = "Luxury", 
            Tags = new[] { "pool", "view", "wifi", "concierge" },
            ParkingIncluded = false, 
            SmokingAllowed = false,
            LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
            Rating = 5, 
            Location = GeographyPoint.Create(47.678581, -122.131577)
        },
        new Hotel()
        { 
            HotelId = "2", 
            BaseRate = 79.99,
            Description = "Cheapest hotel in town",
            DescriptionFr = "Hôtel le moins cher en ville",
            HotelName = "Roach Motel",
            Category = "Budget",
            Tags = new[] { "motel", "budget" },
            ParkingIncluded = true,
            SmokingAllowed = true,
            LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
            Rating = 1,
            Location = GeographyPoint.Create(49.678581, -122.131577)
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

Este método tiene cuatro partes. La primera crea una matriz de objetos `Hotel` que serán los datos de entrada que cargaremos en el índice. Estos datos están incluidos en el código por motivos de simplicidad. En su propia aplicación, los datos provendrán normalmente de un origen de datos externo, como una base de datos SQL.

En la segunda parte se crea un `IndexBatch` que contiene los documentos. Especifique la operación que desee aplicar al lote en el momento de su creación, en este caso llamando a `IndexBatch.Upload`. A continuación, el lote se carga en el índice de Azure Search mediante el método `Documents.Index` .

> [!NOTE]
> En este ejemplo nos limitamos a cargar documentos. Si desea combinar los cambios en los documentos existentes o eliminar documentos, puede crear lotes llamando a `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` o `IndexBatch.Delete`. También puede combinar diferentes operaciones en un único lote llamando a `IndexBatch.New`, que selecciona una colección de objetos de `IndexAction`, que indican a Azure Search que realice una operación determinada en un documento. Puede crear cada `IndexAction` con su propia operación llamando al método correspondiente, como `IndexAction.Merge`, `IndexAction.Upload`, etc.
> 
> 

La tercera parte de este método es un bloque catch que controla un caso de error importante para la indización. Si su servicio Azure Search no logra indizar algunos de los documentos del lote, aparece una `IndexBatchException` producida por `Documents.Index`. Esto puede suceder si indiza documentos mientras el servicio está sobrecargado. **Recomendamos encarecidamente controlar este caso de forma explícita en el código.**  Puede retrasar la indización de los documentos que dieron error y volver a intentarlo; puede crear un registro y continuar, como hace el ejemplo, o puede adoptar otro enfoque según los requisitos de coherencia de datos de la aplicación.

> [!NOTE]
> Puede usar el método [`FindFailedActionsToRetry`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) para construir un nuevo lote que contenga solo las acciones que dieron error en una llamada anterior a `Index`. Hay una explicación sobre cómo se usa correctamente [en StackOverflow](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Por último, el método `UploadDocuments` se retrasa durante dos segundos. La indización ocurre de manera asincrónica en el servicio Azure Search, por lo que la aplicación de ejemplo debe esperar unos momentos para asegurarse de que los documentos estén disponibles para la búsqueda. Retrasos así solo suelen ser necesarios en las pruebas, demostraciones y aplicaciones de ejemplo.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>Gestión de documentos del SDK de .NET
Quizás se pregunte cómo consigue el SDK de Azure para .NET cargar en el índice las instancias de una clase definida por el usuario como `Hotel` . Para responder mejor a esa pregunta, echemos un vistazo a la clase `Hotel` :

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Lo primero que debe observar es que cada propiedad pública de `Hotel` corresponde a un campo en la definición del índice, pero con una diferencia fundamental: El nombre de cada campo comienza con una letra minúscula ("Camel Case"), mientras que el nombre de cada propiedad pública de `Hotel` comienza con una letra mayúscula ("Pascal Case"). Se trata de un escenario común en las aplicaciones .NET que realizan enlaces de datos cuando el esquema de destino está fuera del control del desarrollador de la aplicación. En lugar de tener que infringir las directrices de nomenclatura de .NET utilizando mayúsculas y minúsculas Camel para los nombres de las propiedades, puede usar el atributo `[SerializePropertyNamesAsCamelCase]` para indicar al SDK que asigne los nombres de las propiedades automáticamente a mayúsculas y minúsculas Camel.

> [!NOTE]
> El SDK de .NET para Azure Search usa la biblioteca [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para serializar y deserializar los objetos de modelo personalizados en JSON y de este. Puede personalizar esta serialización si es necesario. Para obtener más información, consulte [serialización personalizada con JSON.NET](#JsonDotNet).
> 
> 

La segunda cosa a tener en cuenta es los atributos que decoran cada propiedad pública (como `IsFilterable`, `IsSearchable`, `Key`, y `Analyzer`). Estos atributos se asignan directamente a los [atributos correspondientes del índice de Azure Search](https://docs.microsoft.com/rest/api/searchservice/create-index#request). La clase `FieldBuilder` usa estos para construir definiciones de campo para el índice.

El segundo aspecto importante sobre el `Hotel` clase es que los tipos de datos de las propiedades públicas. Los tipos .NET de estas propiedades se asignan a los tipos de campo equivalentes de la definición del índice. Por ejemplo, la propiedad de cadena `Category` se asigna al campo `category`, que es de tipo `Edm.String`. Se dan asignaciones de tipos semejantes entre `bool?` y `Edm.Boolean`, `DateTimeOffset?` y `Edm.DateTimeOffset`, etc. Las reglas específicas para la asignación de tipos se documentan con el método `Documents.Get` en la [referencia del SDK de Azure Search para .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get). Aunque la clase `FieldBuilder` se ocupa de esta asignación, todavía puede ser útil comprenderlo por si necesitara solucionar los problemas de serialización.

Esta posibilidad de usar sus propias clases como documentos funciona en ambas direcciones: también puede recuperar los resultados de la búsqueda y hacer que el SDK los deserialice automáticamente a un tipo de su elección, como veremos en la siguiente sección.

> [!NOTE]
> El SDK de Azure Search para .NET también admite documentos de tipo dinámico mediante la clase `Document`, que es una asignación clave/valor de nombres de campo a valores de campo. Esto es útil en escenarios en los que no se conoce el esquema del índice en el momento del diseño o en los que resulte inconveniente enlazar a clases de modelo específicas. Todos los métodos del SDK que se ocupan de los documentos tienen sobrecargas que funcionan con la clase `Document` , así como sobrecargas de asignación rigurosa que aceptan un parámetro de tipo genérico. En el código de ejemplo de este tutorial solo se utilizan las últimas. La [clase `Document`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) se hereda de `Dictionary<string, object>`.
> 
>

**¿Por qué debería usar tipos de datos que aceptan valores null?**

Al diseñar sus propias clases de modelo para asignar a un índice de Azure Search, es recomendable declarar las propiedades de tipos de valor como `bool` y `int` que aceptan valores NULL (por ejemplo: `bool?` en lugar de `bool`). Si usa un tipo de modelo con una propiedad que no acepta valores NULL, tendrá que **garantizar** que ningún documento del índice contiene un valor NULL para el campo correspondiente. Ni el SDK ni el servicio Azure Search le permitirá aplicar esto.

No solo se trata de una cuestión hipotética: imagine un escenario donde se agrega un nuevo campo a un índice existente que es de tipo `Edm.Int32`. Después de actualizar la definición del índice, todos los documentos tendrán un valor null para ese campo nuevo (ya que todos los tipos aceptan valores NULL en Azure Search). Si después usa una clase de modelo con una propiedad `int` que no acepta valores NULL para ese campo, obtendrá `JsonSerializationException` así al intentar recuperar documentos:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Por este motivo, recomendamos utilizar tipos que aceptan valores NULL en las clases de modelo como procedimiento recomendado.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Serialización personalizada con JSON.NET
El SDK usa JSON.NET para serializar y deserializar documentos. Puede personalizar la serialización y deserialización si lo necesita definiendo su propio `JsonConverter` o `IContractResolver` (consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles). Esto puede ser útil cuando desea adaptar una clase de modelo existente de la aplicación para usarla con Azure Search y otros escenarios más avanzados. Por ejemplo, con la serialización personalizada, puede:

* Incluir o excluir determinadas propiedades de la clase de modelo para que se almacenen como campos del documento.
* Asignar entre los nombres de propiedad del código y los nombres de campo del índice.
* Cree atributos personalizados que se puedan usar para asignar propiedades a campos de documento.

Puede encontrar ejemplos de implementación de serialización personalizada en las pruebas unitarias del SDK de .NET para Azure Search en GitHub. [Esta carpeta](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models) es un buen punto de partida. Contiene clases que las pruebas de serialización personalizada utilizan.

### <a name="searching-for-documents-in-the-index"></a>Búsqueda de documentos en el índice
El último paso de la aplicación de ejemplo es buscar algunos documentos en el índice. Esto es lo que hace el método siguiente:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "lastRenovationDate desc" },
            Select = new[] { "hotelName", "lastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

Cada vez que ejecuta una consulta, este método crea primero un nuevo objeto `SearchParameters`. Esto se utiliza para especificar opciones adicionales para la consulta, como el orden, los filtros, la paginación y las facetas. En este método, vamos a establecer la propiedad `Filter`, `Select`, `OrderBy` y `Top` para diferentes consultas. Todas las `SearchParameters`propiedades se documentan [aquí](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

El siguiente paso consiste en ejecutar la consulta de búsqueda. Esto se hace mediante el método `Documents.Search`: Para cada consulta, pasamos el texto de búsqueda para usarlo como cadena (o `"*"` si no hay ningún texto de búsqueda), además de los parámetros de búsqueda creados anteriormente. También especificamos `Hotel` como el parámetro de tipo para `Documents.Search`, lo que indica al SDK que deserialice los documentos de los resultados de búsqueda en objetos de tipo `Hotel`.

> [!NOTE]
> Puede encontrar más información acerca de la sintaxis de expresiones de consulta de búsqueda [aquí](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Por último, después de cada consulta este método recorre en iteración todas las coincidencias de los resultados de búsqueda e imprime cada documento en la consola:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Examinemos más de cerca cada una de las consultas de una en una. Este es el código para ejecutar la primera consulta:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

En este caso, estamos buscando hoteles que coincidan con la palabra "presupuesto" y queremos que los resultados solo devuelvan los nombres de hotel, como especifica el parámetro `Select`. Estos son los resultados:

    Name: Roach Motel

A continuación, queremos encontrar los hoteles con una tarifa por noche inferior a 150 dólares USA y que solo se devuelvan el identificador del hotel y la descripción:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Esta consulta usa una expresión `$filter` de OData, `baseRate lt 150`, para filtrar los documentos del índice. Puede encontrar más información acerca de la sintaxis de OData admitida por Azure Search [aquí](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Estos son los resultados de la consulta:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

Seguidamente, quiere encontrar los dos mejores hoteles que se han renovado más recientemente y mostrar el nombre del hotel y la última fecha de renovación. Este es el código: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

En este caso, usamos de nuevo la sintaxis de OData para especificar el parámetro `OrderBy` como `lastRenovationDate desc`. También establecemos `Top` en 2 para tener la seguridad de que solo obtenemos los dos documentos principales. Como antes, establecemos `Select` para especificar los campos que se deben devolver.

Estos son los resultados:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Por último, queremos encontrar todos los hoteles que coinciden con la palabra "motel":

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Y estos son los resultados, que incluyen todos los campos, ya que no se especificó la propiedad `Select`:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Este paso finaliza el tutorial, pero no se detenga aquí. ** Los siguientes pasos proporcionan recursos adicionales para obtener más información sobre Azure Search.

## <a name="next-steps"></a>Pasos siguientes
* Examine las referencias del [SDK para .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) y la [API de REST](https://docs.microsoft.com/rest/api/searchservice/).
* Consulte las [convenciones de nomenclatura](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) para conocer las reglas que deben seguir los nombres de diversos objetos.
* Consulte los [tipos de datos admitidos](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) en Azure Search.

---
title: Introducción a Azure Blob Storage y los servicios conectados de Visual Studio (ASP.NET) | Microsoft Docs
description: Cómo empezar a usar Azure Blob Storage en un proyecto ASP.NET en Visual Studio después de conectarse a una cuenta de almacenamiento mediante los servicios conectados de Visual Studio
services: storage
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: ghogen
ms.openlocfilehash: 557b47aab5cdadef79427c4da9c18477daac0689
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123049"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Introducción a Azure Blob Storage y los servicios conectados de Visual Studio (ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob Storage es un servicio que almacena datos no estructurados en la nube como objetos o blobs. El Almacenamiento de blobs puede almacenar cualquier tipo de datos binarios o texto, como un documento, un archivo multimedia o un instalador de aplicación. El Almacenamiento de blobs a veces se conoce como "almacenamiento de objetos". Para saber más de Blob Storage, consulte [Introducción a Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

En este tutorial se muestra cómo escribir código de ASP.NET para algunos escenarios comunes que utilizan Blob Storage. Los escenarios incluyen la creación de un contenedor de blob y la carga, enumeración, descarga y eliminación de blobs.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>Creación de un controlador MVC 

1. En el **Explorador de soluciones**, haga clic con el botón derecho en **Controladores**.

2. En el menú contextual, seleccione **Agregar** > **Controlador**.

    ![Captura de pantalla del Explorador de soluciones, en Agregar y Controlador destacados](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de MVC 5: en blanco** y **Agregar**.

    ![Captura de pantalla del cuadro de diálogo Agregar scaffold](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. En el cuadro de diálogo **Agregar controlador**, denomine *BlobsController* al controlador y seleccione **Agregar**.

    ![Captura de pantalla del cuadro de diálogo Agregar controlador](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Agregue las siguientes directivas `using` al archivo `BlobsController.cs`:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Conexión a una cuenta de almacenamiento y obtención de una referencia de contenedor

Un contenedor de blob es una jerarquía anidada de blobs y carpetas. El resto de los pasos que se describen en este documento requieren una referencia a un contenedor de blobs, por lo que el código debe colocarse en su propio método para poder reutilizarlo.

Con los pasos siguientes se crea un método para conectar con la cuenta de almacenamiento mediante la cadena de conexión de **Web.config**. Los pasos sirven también una referencia a un contenedor.  La configuración de la cadena de conexión de **Web.config** se denomina con el formato `<storageaccountname>_AzureStorageConnectionString`. 

1. Abra el archivo `BlobsController.cs` .

1. Agregue un método llamado **GetCloudBlobContainer** que devuelve un elemento **CloudBlobContainer**.  No olvide reemplazar `<storageaccountname>_AzureStorageConnectionString` por el nombre real de la clave en **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Aunque el *contenedor de blobs de prueba* no existe todavía, este código crea una referencia a él. Esto se hace así para que el contenedor pueda crearse con el método `CreateIfNotExists` que se muestra en el paso siguiente.

## <a name="create-a-blob-container"></a>Creación de un contenedor de blobs

Los siguientes pasos muestran cómo crear un contenedor de blobs:

1. Agregue un método llamado `CreateBlobContainer` que devuelve un elemento `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Obtenga un objeto `CloudBlobContainer` que representa una referencia al nombre del contenedor de blobs preferido. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Llame al método `CloudBlobContainer.CreateIfNotExists` para crear el contenedor si aún no existe. El método `CloudBlobContainer.CreateIfNotExists` devuelve **true** si el contenedor no existe y se crea correctamente. En caso contrario, el método devuelve **false**.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Actualice `ViewBag` con el nombre del contenedor de blobs.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    A continuación, se muestra el método `CreateBlobContainer` completado:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Vistas**.

1. Si no hay una carpeta **Blobs**, cree una. En el menú contextual, seleccione **Agregar** > **Nueva carpeta**. Asigne a la nueva carpeta el nombre *Blobs*. 
 
1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** y haga clic con el botón derecho en **Blobs**.

1. En el menú contextual, seleccione **Agregar** > **Vista**.

1. En el cuadro de diálogo **Agregar vista**, escriba **CreateBlobContainer** para el nombre de la vista y seleccione **Agregar**.

1. Abra `CreateBlobContainer.cshtml` y modifíquelo de modo que se parezca al siguiente fragmento de código:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** > **Compartido** y abra `_Layout.cshtml`.

1. Después del último **Html.ActionLink**, agregue el siguiente **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Ejecute la aplicación y seleccione **Crear contenedor de blobs** para ver resultados similares a la siguiente captura de pantalla:
  
    ![Captura de pantalla de la creación de contenedor de blobs](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Como se mencionó anteriormente, el método `CloudBlobContainer.CreateIfNotExists` devuelve **true** solo cuando el contenedor no existe y se crea. Por lo tanto, si la aplicación se ejecuta cuando el contenedor existe, el método devuelve **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Carga de un blob en un contenedor de blobs

Después de [crear el contenedor de blobs](#create-a-blob-container), cargue los archivos en ese contenedor. En esta sección se explica cómo cargar un archivo local en un contenedor de blobs. En estos pasos se supone que hay un contenedor de blobs denominado *test-blob-container*. 

1. Abra el archivo `BlobsController.cs` .

1. Agregue un método llamado `UploadBlob` que devuelve una cadena.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Dentro del método `UploadBlob`, obtenga un objeto `CloudBlobContainer` que representa una referencia al nombre del contenedor de blobs deseado. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Como se explicó anteriormente, Azure Storage es compatible con diferentes tipos de blob. En este tutorial se usan blobs en bloques. Para recuperar una referencia a un blob en bloques, llame al método `CloudBlobContainer.GetBlockBlobReference`.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > El nombre del blob es parte de la dirección URL utilizada para recuperar un blob y puede ser cualquier cadena, incluido el nombre del archivo.

1. Una vez que haya una referencia de blob, puede cargar en ella cualquier flujo de datos mediante una llamada al método `UploadFromStream` del objeto de referencia del blob. El método `UploadFromStream` crea el blob si no existe o lo sobrescribe si existe. (Cambie *&lt;file-to-upload>* por una ruta de acceso completa al archivo que se va a cargar).

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    A continuación, se muestra el método `UploadBlob` completado (con una ruta de acceso completa para el archivo que se va a cargar):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** > **Compartido** y abra `_Layout.cshtml`.

1. Después del último **Html.ActionLink**, agregue el siguiente **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Ejecute la aplicación y seleccione **Cargar blob**.  La palabra *success!* debería aparecer.
    
    ![Captura de pantalla de comprobación correcta](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Enumeración de blobs de un contenedor de blobs

Esta sección explica cómo enumerar los blobs de un contenedor de blobs. El código de ejemplo hace referencia al contenedor *test-blob-container* creado en la sección [Creación de un contenedor de blobs](#create-a-blob-container).

1. Abra el archivo `BlobsController.cs` .

1. Agregue un método llamado `ListBlobs` que devuelve un elemento `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. Dentro del método `ListBlobs`, obtenga un objeto `CloudBlobContainer` que representa una referencia al contenedor de blobs. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Para enumerar los blobs de un contenedor de blobs, use el método `CloudBlobContainer.ListBlobs`. El método `CloudBlobContainer.ListBlobs` devuelve un objeto `IListBlobItem` que se puede transmitir a un objeto `CloudBlockBlob`, `CloudPageBlob` o `CloudBlobDirectory`. El fragmento de código siguiente enumera todos los blobs en un contenedor de blobs. Cada blob se transmite a el objeto apropiado, según su tipo. Su nombre (o URI en el caso de **CloudBlobDirectory**) se agrega a una lista.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Además de blobs, los contenedores de blobs pueden contener directorios. Supongamos que hay un contenedor de blobs llamado *test-blob-container* con la siguiente jerarquía:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Mediante el ejemplo de código anterior, la lista de cadenas de **blobs** contiene valores similares al siguiente:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Como se muestra, la lista incluye solo las entidades de nivel superior, no las anidadas (*bar.png* y *baz.png*). Para enumerar todas las entidades dentro de un contenedor de blobs, cambie el código para que se pase **true** al método **CloudBlobContainer.ListBlobs** para el parámetro **useFlatBlobListing**.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Si establece el parámetro **useFlatBlobListing** en **true** se devolverá una lista plana de todas las entidades del contenedor de blobs. El resultado es este:

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    A continuación, se muestra el método **ListBlobs** completado:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** y haga clic con el botón derecho en **Blobs**.

2. En el menú contextual, seleccione **Agregar** > **Vista**.

1. En el cuadro de diálogo **Agregar vista**, escriba `ListBlobs` como nombre de vista y seleccione **Agregar**.

1. Abra `ListBlobs.cshtml` y reemplace el contenido por el código siguiente:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** > **Compartido** y abra `_Layout.cshtml`.

1. Después del último **Html.ActionLink**, agregue el siguiente **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Ejecute la aplicación y seleccione **Enumerar blobs** para ver resultados similares a la siguiente captura de pantalla:
  
    ![Captura de pantalla de Enumerar blobs](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Descargar blobs

En esta sección se muestra cómo descargar un blob. Puede conservarlo en el almacenamiento local o leer el contenido en una cadena. El código de ejemplo hace referencia al contenedor *test-blob-container* creado en la sección [Creación de un contenedor de blobs](#create-a-blob-container).

1. Abra el archivo `BlobsController.cs` .

1. Agregue un método llamado `DownloadBlob` que devuelve una cadena.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Dentro del método `DownloadBlob`, obtenga un objeto `CloudBlobContainer` que representa una referencia al contenedor de blobs.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Obtenga un objeto de referencia de blob mediante la llamada al método `CloudBlobContainer.GetBlockBlobReference`. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Para descargar un blob, use el método `CloudBlockBlob.DownloadToStream`. El código siguiente transfiere el contenido del blob en un objeto del flujo. Ese objeto se conserva en un archivo local. (Cambie *&lt;nombre del archivo local>* por el nombre completo de archivo que represente la ubicación en la que desea que descargar el blob). 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    A continuación, se muestra el método `DownloadBlob` completado (con una ruta de acceso completa al archivo local que se crea):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** > **Compartido** y abra `_Layout.cshtml`.

1. Después del último **Html.ActionLink**, agregue el siguiente **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Ejecute la aplicación y seleccione **Descargar blob** para descargar el blob. El blob especificado en la llamada al método `CloudBlobContainer.GetBlockBlobReference` se descarga en la ubicación especificada en la llamada al método `File.OpenWrite`.  El texto *"success!"* debería aparecer en el explorador. 

## <a name="delete-blobs"></a>Eliminar blobs

Los siguientes pasos muestran cómo eliminar un blob:

1. Abra el archivo `BlobsController.cs` .

1. Agregue un método llamado `DeleteBlob` que devuelve una cadena.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. Dentro del método `DeleteBlob`, obtenga un objeto `CloudBlobContainer` que representa una referencia al contenedor de blobs.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Obtenga un objeto de referencia de blob mediante la llamada al método `CloudBlobContainer.GetBlockBlobReference`. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Para eliminar un blob, use el método `Delete`.

    ```csharp
    blob.Delete();
    ```
    
    El método `DeleteBlob` completado debe aparecer de la manera siguiente:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas** > **Compartido** y abra `_Layout.cshtml`.

1. Después del último **Html.ActionLink**, agregue el siguiente **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Ejecute la aplicación y seleccione **Eliminar blob** para eliminar el blob especificado en la llamada al método `CloudBlobContainer.GetBlockBlobReference`. El texto *"success!"* debería aparecer en el explorador. Seleccione el botón **Atrás** del explorador y luego seleccione **Mostrar blobs** para comprobar que el blob ya no está en el contenedor.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a almacenar, enumerar y recuperar blobs en Azure Storage mediante ASP.NET. Consulte más guías de características para obtener información acerca de otras opciones del almacenamiento de datos en Azure.

  * [Introducción a Azure Table Storage y a Servicios conectados de Visual Studio (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Introducción a Azure Queue Storage y a Servicios conectados de Visual Studio (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)

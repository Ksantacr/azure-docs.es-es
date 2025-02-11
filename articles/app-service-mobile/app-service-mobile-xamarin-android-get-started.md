---
title: Introducción a Azure Mobile Apps para aplicaciones Xamarin.Android
description: Siga este tutorial para empezar a usar Azure Mobile Apps para el desarrollo de Xamarin Android.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: f3e8ca4f9736dffe4928fc8920b0890dff87367b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236037"
---
# <a name="create-a-xamarinandroid-app"></a>Creación de una aplicación Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio de back-end basado en la nube a una aplicación Xamarin.Android. Para obtener más información, consulte [¿Qué es Mobile Apps?](app-service-mobile-value-prop.md)

La siguiente captura de pantalla muestra la aplicación final:

![][0]

Completar este tutorial es un requisito previo para todos los tutoriales de Mobile Apps para aplicaciones Xamarin.Android.

## <a name="prerequisites"></a>Requisitos previos
Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio con Xamarin. Consulte [Configuración e instalación](/visualstudio/cross-platform/setup-and-install) para obtener instrucciones.

## <a name="create-an-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Azure Mobile App
Siga estos pasos para crear un back-end de Mobile App.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Ahora ha aprovisionado un back-end de aplicación móvil de Azure que puede usarse por las aplicaciones del cliente móvil. Después, descargue un proyecto de servidor para un back-end de "lista de tareas" sencillo y publíquelo en Azure.

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Crear una conexión de base de datos y configurar el proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>Ejecute la aplicación de Xamarin.Android
1. Abra el proyecto de Xamarin.Android.

2. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

3. Abra el archivo `ToDoActivity.cs` en esta carpeta - xamarin.android/ZUMOAPPNAME/ToDoActivity.cs. Es el nombre de la aplicación `ZUMOAPPNAME`.

4. En `ToDoActivity` class, reemplace `ZUMOAPPURL` variable con el punto de conexión público anterior.

    `const string applicationURL = @"ZUMOAPPURL";`

    se convierte en
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Presione la tecla F5 para implementar y ejecutar la aplicación.

6. En la aplicación, escriba un texto significativo, como *Realizar el tutorial* y luego haga clic en el botón **Agregar**.

    ![][10]

    Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de la aplicación móvil devuelve los elementos almacenados en la tabla y los datos aparecen en la lista.

   > [!NOTE]
   > Puede revisar el código de acceso al back-end de aplicación móvil para consultar e insertar datos; este se encuentra en el archivo de C# ToDoActivity.cs.
   
## <a name="troubleshooting"></a>solución de problemas
Si tiene problemas al compilar la solución, ejecute el administrador de paquetes NuGet y actualice los paquetes de compatibilidad de `Xamarin.Android`. Puede que los proyectos de inicio rápido no incluyan las versiones más recientes.

Tenga en cuenta que todos los paquetes de soporte a los que se hace referencia en el proyecto deben tener la misma versión. El [paquete NuGet para Azure Mobile Apps](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) tiene una dependencia `Xamarin.Android.Support.CustomTabs` para la plataforma Android, por lo que si el proyecto usa paquetes de compatibilidad más recientes, debe instalar este paquete con la versión requerida directamente para evitar conflictos.

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203

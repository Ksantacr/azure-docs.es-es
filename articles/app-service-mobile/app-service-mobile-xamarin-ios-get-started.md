---
title: Introducción a Azure App Service Mobile Apps para aplicaciones Xamarin.iOS | Microsoft Docs
description: Siga este tutorial para empezar a usar Mobile Apps para el desarrollo de Xamarin.iOS.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 559050cbc575fce5bdb5b32ec266e1cc3d09b2d5
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242709"
---
# <a name="create-a-xamarinios-app"></a>Creación de una aplicación Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio de back-end basado en la nube a una aplicación móvil Xamarin.iOS con un back-end de Aplicaciones móviles de Azure.  Creará tanto un back-end de aplicación móvil nuevo como una aplicación Xamarin.iOS simple de la *lista de tareas pendientes* que almacene los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los demás tutoriales de Xamarin.iOS sobre cómo usar la característica Mobile Apps en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos
Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, regístrese para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas que podrá seguir usando incluso después de que finalice la evaluación. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio para Mac. Consulte [configuración e instalación de Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Un equipo Mac con Xcode 9.0 o posterior.
  
## <a name="create-an-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Azure Mobile App
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Crear una conexión de base de datos y configurar el proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Ejecute la aplicación de Xamarin.iOS
1. Abra el proyecto de Xamarin.iOS.

2. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

3. Abra el archivo `QSTodoService.cs` en esta carpeta - xamarin.iOS/ZUMOAPPNAME. Es el nombre de la aplicación `ZUMOAPPNAME`.

4. En `QSTodoService` class, reemplace `ZUMOAPPURL` variable con el punto de conexión público anterior.

    `const string applicationURL = @"ZUMOAPPURL";`

    se convierte en
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Presione la tecla F5 para implementar y ejecutar la aplicación en un emulador de iPhone.

6. En la aplicación, escriba texto significativo, como *completar el tutorial* y, a continuación, haga clic en el botón +.

    ![][10]

    Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de la aplicación móvil devuelve los elementos almacenados en la tabla y los datos aparecen en la lista.

   > [!NOTE]
   > Puede revisar el código de acceso al back-end de aplicación móvil para consultar e insertar datos; este se encuentra en el archivo de C# ToDoActivity.cs.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png
---
title: Adición de notificaciones de inserción a una aplicación Android con Mobile Apps | Microsoft Docs
description: Obtenga información sobre cómo usar Azure Mobile Apps para enviar notificaciones push a su aplicación de Android.
services: app-service\mobile
documentationcenter: android
manager: crdun
editor: ''
author: conceptdev
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/17/2017
ms.author: crdun
ms.openlocfilehash: 352e64664e6796fb4e0a7941de91ef4045076aed
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104610"
---
# <a name="add-push-notifications-to-your-android-app"></a>Incorporación de notificaciones push a la aplicación de Android

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Información general

En este tutorial, agregará notificaciones de inserción al proyecto de [inicio rápido de Android] para que cada vez que se envíe una notificación de inserción al dispositivo, se inserte un registro.

Si no usa el proyecto de servidor de inicio rápido descargado, necesita el paquete de extensión de la notificación de inserción. Para más información, vea [Trabajar con el SDK de servidor de back-end de .NET para Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente:

* Un IDE que depende del back-end del proyecto:

  * [Android Studio](https://developer.android.com/sdk/index.html) si esta aplicación tiene un back-end de Node.js.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) o posterior si esta aplicación tiene un back-end. NET.
* Android 2.3 o posterior y revisión de Google Repository 27 o superior, Google Play Services 9.0.2 o superior para Firebase Cloud Messaging
* Completar el [inicio rápido de Android].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Creación de un proyecto que admita Firebase Cloud Messaging

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Configuración de un Centro de notificaciones

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Configuración de Azure para enviar notificaciones push

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Habilitar las notificaciones de inserción para el proyecto de servidor

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Incorporación de notificaciones de inserción a la aplicación

En esta sección, se actualiza la aplicación Android de cliente para controlar las notificaciones de inserción.

### <a name="verify-android-sdk-version"></a>Comprobación de la versión del SDK de Android

[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

El siguiente paso es instalar los servicios de Google Play. Firebase Cloud Messaging tiene algunos requisitos mínimos en el nivel de API para desarrollo y prueba, que debe cumplir la propiedad **minSdkVersion** del manifiesto.

Si va a realizar pruebas con un dispositivo antiguo, consulte [Agregue Firebase a su proyecto de Android] para determinar el valor mínimo que puede configurar y cómo configurarlo de forma adecuada.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Agregar Firebase Cloud Messaging al proyecto

[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Incorporación de código

[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Prueba de la aplicación con el servicio móvil publicado

Puede probar la aplicación conectando directamente un teléfono Android con un cable USB o utilizando un dispositivo virtual en el emulador.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha completado este tutorial, considere la posibilidad de continuar con uno de los siguientes tutoriales:

* [Adición de la autenticación a la aplicación de Android](app-service-mobile-android-get-started-users.md)
  Aprenda a agregar la autenticación al proyecto de inicio rápido todolist en Android con un proveedor de identidades admitido.
* [Habilitación de la sincronización sin conexión para la aplicación móvil de Android](app-service-mobile-android-get-started-offline-data.md)
  Aprenda a agregar compatibilidad sin conexión a una aplicación con un back-end de Mobile Apps. La sincronización sin conexión permite a los usuarios finales interactuar con una aplicación móvil&mdash;ver, agregar o modificar datos&mdash;incluso cuando no haya conexión de red.

<!-- URLs -->
[inicio rápido de Android]: app-service-mobile-android-get-started.md
[Agregue Firebase a su proyecto de Android]:https://firebase.google.com/docs/android/setup

---
title: Cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en Java | Microsoft Docs
description: Explicación detallada de cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en Java.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9a0a65ae1406e4c197370c7b11373603dc40c8ee
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964937"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-java"></a>Cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en Java

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android Java](create-locate-anchors-java.md)
> * [C++/NDK](create-locate-anchors-cpp-ndk.md)
> * [C++/WinRT](create-locate-anchors-cpp-winrt.md)

Azure Spatial Anchors permite compartir delimitadores entre diferentes dispositivos de todo el mundo. Admite varios entornos de desarrollo diferentes. En este artículo, analizaremos en profundidad cómo usar el SDK Azure delimitadores espaciales, en Java, para:

- Correctamente, configurar y administrar una sesión de Azure espacial delimitadores.
- Crear y establecer las propiedades de los anclajes locales.
- Cargarlos en la nube.
- Busque y elimine delimitadores espacial de la nube.

## <a name="prerequisites"></a>Requisitos previos

Para completar a esta guía, asegúrese de que tiene:

- Ha leído completamente [Introducción a Azure Spatial Anchors](../overview.md).
- Ha completado uno de los [inicios rápidos en 5 minutos](../index.yml).
- Conocimientos básicos de Java.
- Conocimiento básico sobre <a href="https://developers.google.com/ar/discover/" target="_blank">ARCore</a> 1.7.

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

Obtenga más información sobre la [CloudSpatialAnchorSession](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession) clase.

```java
    private CloudSpatialAnchorSession mCloudSession;
    // In your view handler
    mCloudSession = new CloudSpatialAnchorSession();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

Obtenga más información sobre la [Configuracióndesesión](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionconfiguration) clase.

```java
    mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```java
    mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

Obtenga más información sobre la [TokenRequiredListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.tokenrequiredlistener) interfaz.

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAccessToken("MyAccessToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAccessToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```java
    mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAuthenticationToken("MyAuthenticationToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAuthenticationToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

Obtenga más información sobre la [iniciar](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.start) método.

```java
    mCloudSession.setSession(mSession);
    mCloudSession.start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

Obtenga más información sobre la [processFrame](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.processframe) método.

```java
    mCloudSession.processFrame(mSession.update());
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

Obtenga más información sobre la [SessionUpdatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionupdatedlistener) interfaz.

```java
    mCloudSession.addSessionUpdatedListener(args -> {
        auto status = args->Status();
        if (status->UserFeedback() == SessionUserFeedback::None) return;
        NumberFormat percentFormat = NumberFormat.getPercentInstance();
        percentFormat.setMaximumFractionDigits(1);
        mFeedback = String.format("Feedback: %s - Recommend Create=%s",
            FeedbackToString(status.getUserFeedback()),
            percentFormat.format(status.getRecommendedForCreateProgress()));
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

Obtenga más información sobre la [CloudSpatialAnchor](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor) clase.

```java
    // Create a local anchor, perhaps by hit-testing and creating an ARAnchor
    Anchor localAnchor = null;
    List<HitResult> hitResults = mSession.update().hitTest(0.5f, 0.5f);
    for (HitResult hit : hitResults) {
        Trackable trackable = hit.getTrackable();
        if (trackable instanceof Plane) {
            if (((Plane) trackable).isPoseInPolygon(hit.getHitPose())) {
                localAnchor = hit.createAnchor();
                break;
            }
        }
    }

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    CheckForCompletion(createAnchorFuture, cloudAnchor);

    // ...

    private void CheckForCompletion(Future createAnchorFuture, CloudSpatialAnchor cloudAnchor) {
        new android.os.Handler().postDelayed(() -> {
            if (createAnchorFuture.isDone()) {
                try {
                    createAnchorFuture.get();
                    mFeedback = String.format("Created a cloud anchor with ID=%s", cloudAnchor.getIdentifier());
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(createAnchorFuture, cloudAnchor);
            }
        }, 500);
    }
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

Obtenga más información sobre la [getSessionStatusAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getsessionstatusasync) método.

```java
    Future<SessionStatus> sessionStatusFuture = mCloudSession.getSessionStatusAsync();
    CheckForCompletion(sessionStatusFuture);

    // ...

    private void CheckForCompletion(Future<SessionStatus> sessionStatusFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (sessionStatusFuture.isDone()) {
                try {
                    SessionStatus value = sessionStatusFuture.get();
                    if (value.getRecommendedForCreateProgress() < 1.0f) return;
                    // Issue the creation request...
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(sessionStatusFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

Obtenga más información sobre la [getAppProperties](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.getappproperties) método.

```java
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Map<String,String> properties = cloudAnchor.getAppProperties();
    properties.put("model-type", "frame");
    properties.put("label", "my latest picture");
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    // ...
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

Obtenga más información sobre la [updateAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.updateanchorpropertiesasync) método.

```java
    CloudSpatialAnchor anchor = /* locate your anchor */;
    anchor.getAppProperties().put("last-user-access", "just now");
    Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
    CheckForCompletion(updateAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future updateAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (updateAnchorPropertiesFuture.isDone()) {
                try {
                    updateAnchorPropertiesFuture.get();
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion1(updateAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

Obtenga más información sobre la [getAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getanchorpropertiesasync) método.

```java
    Future<CloudSpatialAnchor> getAnchorPropertiesFuture = mCloudSession.getAnchorPropertiesAsync("anchorId");
    CheckForCompletion(getAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future<CloudSpatialAnchor> getAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (getAnchorPropertiesFuture.isDone()) {
                try {
                    CloudSpatialAnchor anchor = getAnchorPropertiesFuture.get();
                    if (anchor != null) {
                        anchor.getAppProperties().put("last-user-access", "just now");
                        Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
                        // ...
                    }
                } catch (InterruptedException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                } catch (ExecutionException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                }
            } else {
                CheckForCompletion(getAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

Obtenga más información sobre la [setExpiration](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.setexpiration) método.

```java
    Date now = new Date();
    Calendar cal = Calendar.getInstance();
    cal.setTime(now);
    cal.add(Calendar.DATE, 7);
    Date oneWeekFromNow = cal.getTime();
    cloudAnchor.setExpiration(oneWeekFromNow);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

Obtenga más información sobre la [createWatcher](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.createwatcher) método.

```java
    AnchorLocateCriteria criteria = new AnchorLocateCriteria();
    criteria.setIdentifiers(new String[] { "id1", "id2", "id3" });
    mCloudSession.createWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

Obtenga más información sobre la [AnchorLocatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.anchorlocatedlistener) interfaz.

```java
    mCloudSession.addAnchorLocatedListener(args -> {
        switch (args.getStatus()) {
            case Located:
                CloudSpatialAnchor foundAnchor = args.getAnchor();
                // Go add your anchor to the scene...
                break;
            case AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

Obtenga más información sobre la [deleteAnchorAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.deleteanchorasync) método.

```java
    Future deleteAnchorFuture = mCloudSession.deleteAnchorAsync(cloudAnchor);
    // Perform any processing you may want when delete finishes (deleteAnchorFuture is done)
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

Obtenga más información sobre la [detener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.stop) método.

```java
    mCloudSession.stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

Obtenga más información sobre la [restablecer](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.reset) método.

```java
    mCloudSession.reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-java.md)]

Obtenga más información sobre la [cerrar](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.close) método.

```java
    mCloudSession.close();
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]

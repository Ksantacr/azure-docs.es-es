---
title: Cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en C++ / c++ / NDK | Microsoft Docs
description: Explicación detallada de cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en C++ / c++ / NDK.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7ed948bc0bbb109f2fef247beefb55a9c38fb677
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237392"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-cndk"></a>Cómo crear y localizar los anclajes mediante delimitadores espacial de Azure en C++ / c++ / NDK

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android Java](create-locate-anchors-java.md)
> * [C++/NDK](create-locate-anchors-cpp-ndk.md)
> * [C++/WinRT](create-locate-anchors-cpp-winrt.md)

Azure Spatial Anchors permite compartir delimitadores entre diferentes dispositivos de todo el mundo. Admite varios entornos de desarrollo diferentes. En este artículo, analizaremos en profundidad cómo usar el SDK de Azure espacial delimitadores, en C++ / c++ / NDK, para:

- Correctamente, configurar y administrar una sesión de Azure espacial delimitadores.
- Crear y establecer las propiedades de los anclajes locales.
- Cargarlos en la nube.
- Busque y elimine delimitadores espacial de la nube.

## <a name="prerequisites"></a>Requisitos previos

Para completar a esta guía, asegúrese de que tiene:

- Ha leído completamente [Introducción a Azure Spatial Anchors](../overview.md).
- Ha completado uno de los [inicios rápidos en 5 minutos](../index.yml).
- Conocimientos básicos sobre C++ y la <a href="https://developer.android.com/ndk/" target="_blank">Kit de desarrollo nativo de Android</a>.
- Conocimiento básico sobre <a href="https://developers.google.com/ar/discover/" target="_blank">ARCore</a> 1.7.

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

Obtenga más información sobre la [CloudSpatialAnchorSession](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession) struct.

```cpp
    std::shared_ptr<CloudSpatialAnchorSession> cloudSession_;
    // In your view handler
    cloudSession_ = std::make_shared<CloudSpatialAnchorSession>();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

Obtenga más información sobre la [Configuracióndesesión](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/sessionconfiguration) struct.

```cpp
    auto configuration = cloudSession_->Configuration();
    configuration->AccountKey(R"(MyAccountKey)");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```cpp
    auto configuration = cloudSession_->Configuration();
    configuration->AccessToken(R"(MyAccessToken)");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

Obtenga más información sobre la [TokenRequiredDelegate](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/tokenrequireddelegate) delegar.

```cpp
    auto accessTokenRequiredToken = cloudSession_->TokenRequired([](auto&&, auto&& args) {
        args->AccessToken(R"(MyAccessToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    auto accessTokenRequiredToken = cloudSession_->TokenRequired([this](auto&&, auto&& args) {
        std::shared_ptr<CloudSpatialAnchorSessionDeferral> deferral = args->GetDeferral();
        MyGetTokenAsync([&deferral, &args](std::string const& myToken) {
            if (myToken != nullptr) args->AccessToken(myToken);
            deferral->Complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```cpp
    auto configuration = cloudSession_->Configuration();
    configuration->AuthenticationToken(R"(MyAuthenticationToken)");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```cpp
    auto accessTokenRequiredToken = cloudSession_->AccessTokenRequired([](auto&&, auto&& args) {
        args->AuthenticationToken(R"(MyAuthenticationToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    auto accessTokenRequiredToken = cloudSession_->TokenRequired([this](auto&&, auto&& args) {
        std::shared_ptr<CloudSpatialAnchorSessionDeferral> deferral = args->GetDeferral();
        MyGetTokenAsync([&deferral, &args](std::string const& myToken) {
            if (myToken != nullptr) args->AuthenticationToken(myToken);
            deferral->Complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

Obtenga más información sobre la [iniciar](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#start) método.

```cpp
    cloudSession_->Session(ar_session_);
    cloudSession_->Start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

Obtenga más información sobre la [ProcessFrame](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#processframe) método.

```cpp
    cloudSession_->ProcessFrame(ar_frame_);
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

Obtenga más información sobre la [SessionUpdatedDelegate](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/sessionupdateddelegate) delegar.

```cpp
    auto sessionUpdatedToken = cloudSession_->SessionUpdated([this](auto&&, auto&& args) {
        auto status = args->Status();
        if (status->UserFeedback() == SessionUserFeedback::None) return;
        std::ostringstream str;
        str << std::fixed << std::setw(2) << std::setprecision(0)
            << R"(Feedback: )" << FeedbackToString(status.UserFeedback()) << R"( -)"
            << R"( Recommend Create=)" << (status->RecommendedForCreateProgress() * 100) << R"(%)";
        feedback_ = str.str();
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

Obtenga más información sobre la [CloudSpatialAnchor](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchor) struct.

```cpp
    // Create a local anchor, perhaps by hit-testing and creating an ARAnchor
    ArAnchor* localAnchor;
    ArHitResultList* hit_result_list = nullptr;
    ArHitResultList_create(ar_session_, &hit_result_list);
    CHECK(hit_result_list);
    ArFrame_hitTest(ar_session_, ar_frame_, 0.5, 0.5, hit_result_list);
    int32_t hit_result_list_size = 0;
    ArHitResultList_getSize(ar_session_, hit_result_list, &hit_result_list_size);
    if (hit_result_list_size == 0) {
        ArHitResultList_destroy(hit_result_list);
        return;
    }
    ArHitResult* ar_hit = nullptr;
    ArHitResult_create(ar_session_, &ar_hit);
    // The hitTest method sorts the resulting list by increasing distance from the camera
    // The first hit result will usually be the most relevant when responding to user input
    ArHitResultList_getItem(ar_session_, hit_result_list, 0, ar_hit);
    if (ArHitResult_acquireNewAnchor(ar_session_, ar_hit, &localAnchor) != AR_SUCCESS) return;
    ArTrackingState tracking_state = AR_TRACKING_STATE_STOPPED;
    ArAnchor_getTrackingState(ar_session_, localAnchor, &tracking_state);
    if (tracking_state != AR_TRACKING_STATE_TRACKING) {
        ArAnchor_release(localAnchor);
        ArHitResult_destroy(ar_hit);
        return;
    }
    ArHitResult_destroy(ar_hit);
    ar_hit = nullptr;
    ArHitResultList_destroy(hit_result_list);
    hit_result_list = nullptr;

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    std::shared_ptr<CloudSpatialAnchor> cloudAnchor = std::make_shared<CloudSpatialAnchor>();
    cloudAnchor->LocalAnchor(localAnchor);
    cloudSession_->CreateAnchorAsync(cloudAnchor, [this, cloudAnchor](Status status) {
        std::ostringstream str;
        if (status != Status::OK) {
            str << "Save Failed: " << std::to_string(static_cast<uint32_t>(status));
            feedback_ = str.str();
            return;
        }
        str << R"(Created a cloud anchor with ID=)" << cloudAnchor->Identifier();
        feedback_ = str.str();
    });
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

Obtenga más información sobre la [GetSessionStatusAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#getsessionstatusasync) método.

```cpp
    cloudSession_->GetSessionStatusAsync([this](Status status, const std::shared_ptr<SessionStatus>& value) {
        if (status != Status::OK) {
            std::ostringstream str;
            str << "Session status error: " << std::to_string(static_cast<uint32_t>(status));
            feedback_ = str.str();
            return;
        }
        if (value->RecommendedForCreateProgress() < 1.0f) return;
        // Issue the creation request ...
    });
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

Obtenga más información sobre la [valores de AppProperties](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchor#appproperties) método.

```cpp
    std::shared_ptr<CloudSpatialAnchor> cloudAnchor = std::make_shared<CloudSpatialAnchor>();
    cloudAnchor->LocalAnchor(localAnchor);
    auto properties = cloudAnchor->AppProperties();
    properties->Insert(R"(model-type)", R"(frame)");
    properties->Insert(R"(label)", R"(my latest picture)");
    cloudSession_->CreateAnchorAsync(cloudAnchor, [this, cloudAnchor](Status status) {
        // ...
    });
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

Obtenga más información sobre la [UpdateAnchorPropertiesAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#updateanchorpropertiesasync) método.

```cpp
    std::shared_ptr<CloudSpatialAnchor> anchor = /* locate your anchor */;
    auto properties = anchor->AppProperties();
    properties->Insert(R"(last-user-access)", R"(just now)");
    cloudSession_->UpdateAnchorPropertiesAsync(anchor, [this](Status status) {
        if (status != Status::OK) {
            std::ostringstream str;
            str << "Updating Properties Failed: " << std::to_string(static_cast<uint32_t>(status));
            feedback_ = str.str();
        }
    });
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

Obtenga más información sobre la [GetAnchorPropertiesAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#getanchorpropertiesasync) método.

```cpp
    cloudSession_->GetAnchorPropertiesAsync(R"(anchorId)", [this](Status status, const std::shared_ptr<CloudSpatialAnchor>& anchor) {
        if (status != Status::OK) {
            std::ostringstream str;
            str << "Getting Properties Failed: " << std::to_string(static_cast<uint32_t>(status));
            feedback_ = str.str();
            return;
        }
        if (anchor != nullptr) {
            auto properties = anchor->AppProperties();
            properties->Lookup(R"(last-user-access)") = R"(just now)";
            cloudSession_->UpdateAnchorPropertiesAsync(anchor, [this](Status status) {
                // ...
            });
        }
    });
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

Obtenga más información sobre la [caducidad](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchor#expiration) método.

```cpp
    std::chrono::system_clock::time_point now = std::chrono::system_clock::now();
    std::chrono::system_clock::time_point oneWeekFromNow = now + std::chrono::hours(7 * 24);
    const int64_t oneWeekFromNowUnixEpochTimeMs = std::chrono::duration_cast<std::chrono::milliseconds>(oneWeekFromNow.time_since_epoch()).count();
    cloudAnchor->Expiration(oneWeekFromNowUnixEpochTimeMs);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

Obtenga más información sobre la [CreateWatcher](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#createwatcher) método.

```cpp
    auto criteria = std::make_shared<AnchorLocateCriteria>();
    criteria->Identifiers({ R"(id1)", R"(id2)", R"(id3)" });
    auto cloudSpatialAnchorWatcher = cloudSession_->CreateWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

Obtenga más información sobre la [AnchorLocated](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/anchorlocateddelegate) delegar.

```cpp
    auto anchorLocatedToken = cloudSession_->AnchorLocated([this](auto&&, auto&& args) {
        switch (args->Status()) {
            case LocateAnchorStatus::Located: {
                std::shared_ptr<CloudSpatialAnchor> foundAnchor = args->Anchor();
                // Go add your anchor to the scene...
            }
                break;
            case LocateAnchorStatus::AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case LocateAnchorStatus::NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case LocateAnchorStatus::NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

Obtenga más información sobre la [DeleteAnchorAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#deleteanchorasync) método.

```cpp
    cloudSession_->DeleteAnchorAsync(cloudAnchor, [this](Status status) {
        // Perform any processing you may want when delete finishes
    });
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

Obtenga más información sobre la [detener](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#stop) método.

```cpp
    cloudSession_->Stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

Obtenga más información sobre la [restablecer](https://docs.microsoft.com/cpp/api/spatial-anchors/ndk/cloudspatialanchorsession#reset) método.

```cpp
    cloudSession_->Reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-others.md)]

```cpp
    cloudSession_ = nullptr;
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]

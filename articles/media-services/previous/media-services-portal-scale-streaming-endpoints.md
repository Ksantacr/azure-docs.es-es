---
title: Escalado de puntos de conexión de streaming con Azure Portal | Microsoft Docs
description: Este tutorial lo guiará a través de los pasos de escalado de puntos de conexión de streaming con el Portal de Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 23eb51428dcf4961febfb592bf957bb8beeeda57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463129"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Escalado de puntos de conexión de streaming con el Portal de Azure
## <a name="overview"></a>Información general

> [!NOTE]
> Para completar este tutorial, deberá tener una cuenta de Azure. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Los puntos de conexión de streaming **Premium** son adecuados para cargas de trabajo avanzadas y proporcionan una capacidad de ancho de banda dedicada y escalable. Los clientes que tienen un punto de conexión de streaming **Premium**, de forma predeterminada, obtienen una unidad de streaming (SU). El punto de conexión de streaming puede ampliarse agregando unidades de streaming. Cada unidad de streaming proporciona capacidad de ancho de banda adicional a la aplicación. Para obtener más información acerca de los puntos de conexión de streaming y la configuración de la red CDN, vea el tema [Streaming Endpoint overview](media-services-streaming-endpoints-overview.md) (Información general de puntos de conexión de streaming).
 
Este tema muestra cómo escalar un punto de conexión de streaming.

Para obtener más información acerca del precio, consulte la página sobre [información del precio de Media Services](https://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Escalar puntos de conexión de streaming

Para crear y cambiar el número de unidades de streaming, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com/), seleccione la cuenta de Azure Media Services.
2. En la ventana **Configuración**, haga clic en **Extremos de streaming**.
3. Haga clic en el punto de conexión de streaming que desea modificar. 

    > [!NOTE] 
    > Solo puede escalar puntos de conexión de streaming **Premium**.

4. Mueva el control deslizante para especificar el número de unidades de streaming.

    ![extremo de streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte las rutas de aprendizaje de Media Services.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


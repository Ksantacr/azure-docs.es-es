---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d7ac457041474f4e774414b1d5e6f9ed09dc856
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146514"
---
<!-- this tells how to get the connection string for your hub -->
<!-- This assumes the user is looking at his hub in the portal. -->

Una vez creado el centro, puede recuperar la cadena de conexión del centro. Se utiliza para conectar dispositivos y aplicaciones al centro. 

1. Haga clic en el centro para ver el panel de IoT Hub con la configuración y otros elementos. Haga clic en **Directivas de acceso compartido**.
   
2. En **Directivas de acceso compartido**, seleccione la directiva **iothubowner**. 

3. En **Claves de acceso compartido**, copie **Cadena de conexión: clave principal** para su uso posterior.

    ![Recuperación de la cadena de conexión](./media/iot-hub-include-find-connection-string/iot-hub-get-connection-string.png)

    Para más información, consulte [Control de acceso](../articles/iot-hub/iot-hub-devguide-security.md) en la "Guía del desarrollador de Azure IoT Hub".

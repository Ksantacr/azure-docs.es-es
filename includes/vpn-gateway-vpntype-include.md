---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b3907882df09bfae1d6453fbffbd3e7562554f7c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159328"
---
* **PolicyBased:** VPN basadas en directivas se llamaban puertas de enlace de enrutamientos estáticos en el modelo de implementación clásica. Las VPN basadas en directivas cifran y dirigen los paquetes a través de túneles de IPsec basados en las directivas de IPsec configuradas con las combinaciones de prefijos de dirección entre su red local y la red virtual de Azure. La directiva (o el selector de tráfico) se define normalmente como una lista de acceso en la configuración del dispositivo VPN. El valor de un tipo de VPN basada en directivas es *PolicyBased*. Cuando use una VPN basada en directivas, tenga en cuenta las siguientes limitaciones:
  
  * Las VPN basadas en directivas **solo** se utilizan en la SKU de puerta de enlace Básica. Este tipo de VPN no es compatible con otras SKU de puerta de enlace.
  * Solo puede tener 1 túnel cuando se usa una VPN basada en directivas.
  * Las VPN basadas en directivas solo se pueden usar para las conexiones S2S, y solo para determinadas configuraciones. La mayoría de las configuraciones de VPN Gateway requieren una VPN basada en enrutamiento.
* **RouteBased**: VPN basadas en enrutamiento se llamaban puertas de enlace de enrutamientos dinámicos en el modelo de implementación clásica. Las VPN basadas en enrutamiento utilizan "rutas" en la dirección IP de reenvío o en la tabla de enrutamiento para dirigir los paquetes a sus correspondientes interfaces de túnel. A continuación, las interfaces de túnel cifran o descifran los paquetes dentro y fuera de los túneles. La directiva o el selector de tráfico para las VPN basadas en enrutamiento se configura como una conectividad del tipo de cualquiera a cualquiera (o caracteres comodín). El valor de un tipo de VPN basada en enrutamiento es *RouteBased*.

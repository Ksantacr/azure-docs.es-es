---
title: Pruebas de penetración | Microsoft Docs
description: El artículo proporciona información general sobre el proceso de pruebas de penetración y cómo realizar dichas pruebas en sus aplicaciones que se ejecutan en la infraestructura de Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: 8835c4534b6dab1e8dbfb3546257ae4bc3b9d7af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610923"
---
# <a name="penetration-testing"></a>Pruebas de penetración
Una de las ventajas del uso de Azure para las pruebas y la implementación de aplicaciones es que puede crear entornos rápidamente. No tiene que preocuparse del pedido, la adquisición, la "instalación en bastidor y apilamiento" de su propio hardware local.

Esto es genial, pero debe asegurarse de realizar sus diligencias de seguridad normales. Una de las cosas que es probable que desee hacer es penetración probar las aplicaciones que implemente en Azure.

Es posible que ya sepa que realiza Microsoft realiza [pruebas de penetración de nuestro entorno de Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Esto permite impulsar las mejoras de Azure.

No prueba de penetración su aplicación, pero somos conscientes de que se desea y necesita realizar pruebas en sus propias aplicaciones. Que es algo bueno, ya que al mejorar la seguridad de las aplicaciones ayudan a proteger todo el ecosistema de Azure.

A partir del 15 de junio de 2017, Microsoft ya no requiere la aprobación previa para llevar a cabo una prueba de penetración con recursos de Azure. Se recomienda a los clientes que quieran documentar formalmente los compromisos de futuras pruebas de penetración con Microsoft Azure que completen el [formulario de notificación de pruebas de penetración de servicios de Azure](https://portal.msrc.microsoft.com/en-us/engage/pentest). Este proceso solo está relacionado con Microsoft Azure y no es aplicable ningún otro servicio de Microsoft Cloud.

>[!IMPORTANT]
>Aunque notificar a Microsoft de las actividades de pruebas de penetración ya no es obligatorio, los clientes deben cumplir las [reglas de compromiso de las pruebas de penetración unificadas de Microsoft Cloud](https://technet.microsoft.com/mt784683) de todos modos.

Entre las pruebas estándar que puede realizar se incluyen:

* Pruebas en los puntos de conexión para descubrir las [10 principales vulnerabilidades del Proyecto de seguridad de aplicación web abierta (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Pruebas de vulnerabilidad ante datos aleatorios o inesperados](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) de los puntos de conexión
* [Exploración de puertos](https://en.wikipedia.org/wiki/Port_scanner) de los puntos de conexión

Un tipo de prueba que no puede realizar es ningún tipo de ataque [de denegación de servicio (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . Esto incluye iniciar un ataque de denegación de servicio o la realización de pruebas relacionadas que puedan determinar, demostrar o simular cualquier tipo de ataque de denegación de servicio.

## <a name="next-steps"></a>Pasos siguientes

- Si desea documentar formalmente una próxima las pruebas de penetración contra sus aplicaciones hospedadas en Microsoft Azure, diríjase a la [reglas de compromiso de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) y rellene el formulario de notificación de prueba.

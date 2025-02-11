---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: 1d8340054ace25a0cdc36eef9c3d5b4238a6f99b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66124669"
---
El servicio y el tipo de suscripción determinan el número de consultas que puede realizar por segundo (QPS). Asegúrese de que la aplicación incluye la lógica necesaria para mantenerse dentro de su cuota. Si se alcanza el límite de consultas por segundo, o se supera, se produce un error en la solicitud y se devolverá el código de estado HTTP 429. La respuesta incluye el encabezado `Retry-After`, que indica cuánto tiempo debe esperar antes de enviar otra solicitud.

## <a name="denial-of-service-versus-throttling"></a>Denegación de servicio frente a Limitación

El servicio diferencia entre un ataque de denegación de servicio (DoS) y una infracción de las consultas por segundo. Si el servicio sospecha de un ataque de denegación de servicio, la solicitud se realiza correctamente (código de estado HTTP 200 OK). Sin embargo, el cuerpo de la respuesta está vacío.
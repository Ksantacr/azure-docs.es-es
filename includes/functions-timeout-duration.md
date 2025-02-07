---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: ffb29fc76313e8870b52cb0a63936da7853ea6ce
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131339"
---
## <a name="timeout"></a>Duración de tiempo de espera de aplicación de función 

La duración de tiempo de espera de una aplicación de función se define mediante la propiedad functionTimeout en el [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) archivo de proyecto. En la siguiente tabla muestra los valores predeterminado y máximo en minutos para ambos planes y en ambas versiones en tiempo de ejecución:

| Plan | Versión en tiempo de ejecución | Valor predeterminado | Máximo |
|------|---------|---------|---------|
| Consumo | 1.x | 5 | 10 |
| Consumo | 2.x | 5 | 10 |
| App Service | 1.x | Sin límite | Sin límite |
| App Service | 2.x | 30 | Sin límite |

> [!NOTE] 
> Independientemente de la configuración de tiempo de espera de aplicación de función, 230 segundos es la cantidad máxima de tiempo que puede llevar a una función desencadenada por HTTP para responder a una solicitud. Esto es debido el [predeterminado de tiempo de espera de inactividad del equilibrador de carga de Azure](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Para los tiempos de procesamiento más largos, considere el uso de la [patrón asincrónico de Durable Functions](../articles/azure-functions/durable/durable-functions-concepts.md#async-http) o [aplazar el trabajo real y devolver una respuesta inmediata](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).

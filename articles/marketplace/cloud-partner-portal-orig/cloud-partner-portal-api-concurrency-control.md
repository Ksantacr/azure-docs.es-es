---
title: Control de simultaneidad | Azure Marketplace
description: Estrategias de control de simultaneidad para las API de publicación de Cloud Partner Portal.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 8cdcfd84a2f3bd4f920b97392255237db173cbf9
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935594"
---
# <a name="concurrency-control"></a>Control de simultaneidad

Cada llamada a la API de publicación de Cloud Partner Portal debe especificar explícitamente la estrategia de control de simultaneidad que se usará. Si no se proporciona el encabezado **If-Match**, se producirá una respuesta de error HTTP 400. Ofrecemos dos estrategias para el control de simultaneidad.

-   **Optimista**: el cliente que realiza que la actualización comprueba si los datos han cambiado desde la última lectura de los datos.
-   **El último gana**: el cliente actualiza directamente los datos, independientemente de si otra aplicación los ha modificado desde la última lectura.

<a name="optimistic-concurrency-workflow"></a>Flujo de trabajo de simultaneidad optimista
-------------------------------

Se recomienda usar la estrategia de simultaneidad optimista, con el siguiente flujo de trabajo, para garantizar que no se realizan modificaciones inesperadas a los recursos.

1.  Recupere una entidad mediante las API. La respuesta incluye un valor de ETag que identifica la versión de la entidad almacenada actualmente (en el momento de la respuesta).
2.  En el momento de la actualización, incluya este valor de ETag en el encabezado de solicitud **If-Match** obligatorio.
3.  La API compara el valor ETag recibido en la solicitud con el valor ETag actual de la entidad en una transacción atómica.
    *   Si los valores de ETag son diferentes, la API devuelve un respuesta HTTP `412 Precondition Failed`. Este error indica que cualquier otro proceso ha actualizado la entidad desde que el cliente la recuperó por última vez, o que el valor de ETag especificado en la solicitud es incorrecto.
    *  Si los valores de ETag son iguales, o el encabezado **If-Match** contiene el carácter comodín asterisco (`*`), la API realiza la operación solicitada. La operación de API también actualiza el valor de ETag almacenado de la entidad.


> [!NOTE]
> Especificar el carácter comodín (*) en el encabezado **If-Match** lleva a que la API use la estrategia de simultaneidad El último gana. En este caso, no se realiza la comparación de ETag y el recurso se actualiza sin ninguna comprobación. 

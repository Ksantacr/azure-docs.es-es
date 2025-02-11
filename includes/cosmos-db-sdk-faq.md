---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66154568"
---
**1. ¿Cómo se notificará a los clientes la retirada del SDK?**

Microsoft notificará con 12 meses de antelación la finalización del soporte técnico del SDK que se retira para facilitar una transición sin problemas a un SDK compatible. Además, se notificará a los clientes a través de diversos canales de comunicación: Portal de administración de Azure, Centro para desarrolladores, entrada de blog y comunicación directa a los administradores asignados el servicio.

**2. ¿Pueden los clientes crear aplicaciones con un SDK de Azure Cosmos DB de "posible" retirada durante el período de 12 meses?** 

Sí, los clientes tendrán pleno acceso para crear, implementar y modificar aplicaciones mediante el SDK de Azure Cosmos DB de "posible" retirada durante el período de gracia de 12 meses. Durante este período, se recomienda a los clientes migrar a una versión compatible más reciente del SDK de Azure Cosmos DB, según corresponda.

**3. Tras el período de notificación de 12 meses, ¿pueden los clientes crear y modificar aplicaciones mediante un SDK de Azure Cosmos DB retirado?**

El SDK se retirará tras el período de notificación de 12 meses. La plataforma Azure Cosmos DB no permitirá el acceso de aplicaciones a Azure Cosmos DB mediante un SDK retirado. Además, Microsoft no prestará servicio al cliente sobre el SDK retirado.

**4. ¿Qué ocurre con las aplicaciones en ejecución del cliente que usen una versión no compatible del SDK de Azure Cosmos DB?**

Se rechazarán los intentos efectuados para conectar con el servicio Azure Cosmos DB mediante una versión del SDK retirada. 

**5. ¿Se aplicarán las nuevas características y funcionalidad a todos los SDK no retirados?**

Las nuevas características y funcionalidad solo se incorporarán a las nuevas versiones. Si usa una versión anterior del SDK que no se haya retirado, las solicitudes realizadas en Azure Cosmos DB seguirán funcionando como antes, pero no se tendrá acceso a las nuevas funcionalidades.  

**6. ¿Qué debo hacer si no puedo actualizar la aplicación antes de una fecha límite?**

Se recomienda que actualice al SDK más reciente tan pronto como sea posible. Una vez etiquetado un SDK para la retirada, tendrá 12 meses para actualizar la aplicación. Si por cualquier razón no puede completar la actualización de la aplicación en este período de tiempo, póngase en contacto con el [equipo de Cosmos DB](mailto:askcosmosdb@microsoft.com) y solicite su ayuda antes de la fecha límite.


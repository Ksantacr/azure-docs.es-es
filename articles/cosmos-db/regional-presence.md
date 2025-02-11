---
title: Presencia regional con Azure Cosmos DB
description: En este artículo se explica la presencia regional de Azure Cosmos DB y los distintos entornos en la nube.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 787bcc8f0db60868008ec93fcacdec1283946d2f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243730"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Presencia regional con Azure Cosmos DB

Azure Cosmos DB es un servicio fundamental en Azure y, de forma predeterminada, siempre está disponible en todas las regiones donde Azure está disponible. Actualmente, Azure está disponible en [54 regiones](https://azure.microsoft.com/global-infrastructure/regions/) en todo el mundo. 

[![Regiones donde está disponible Azure Cosmos DB](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB está disponible en los cinco entornos en la nube de Azure disponibles para los clientes:

* La nube **pública de Azure**, que está disponible globalmente.

* **Azure China 21Vianet** está disponible a través de una exclusiva asociación entre Microsoft y 21Vianet, uno de los mayores proveedores de internet del país en China.

* **Azure Alemania** proporciona servicios bajo un modelo de administrador de datos que garantiza que los datos de los clientes permanecen en Alemania bajo el control de T-Systems International GmbH, una subsidiaria de Deutsche Telekom, que actúa como administrador de datos para Alemania.

* **Azure Government** está disponible en cuatro regiones en los Estados Unidos para agencias gubernamentales de EE. UU. y sus asociados. 

* **Azure Government para el departamento de defensa (DoD)** está disponible en dos regiones de Estados Unidos para el departamento de defensa.

## <a name="regional-presence-with-global-distribution"></a>Presencia regional con distribución global

Todas las API expuestas por Azure Cosmos DB (incluido SQL, MongoDB, Cassandra, Gremlin y tabla) están disponibles en todas las regiones de Azure de forma predeterminada. Por ejemplo, puede tener MongoDB y Cassandra API expuesta por Azure Cosmos DB no solo en todas las regiones de Azure globales, sino también en las nubes independientes, como el departamento de defensa, gobierno, Alemania y China regiones (DoD).

Azure Cosmos DB es un [distribuida globalmente](distribute-data-globally.md) servicio base de datos. Puede asociar cualquier número de regiones de Azure con su cuenta de Azure Cosmos y los datos se replican de forma automática y transparente. Puede agregar o quitar una región a la cuenta de Azure Cosmos en cualquier momento. Con la funcionalidad de distribución global de llave en mano y el protocolo de replicación con arquitectura multimaestro, Azure Cosmos DB ofrece latencias de lectura y escritura de menos de 10 ms en el percentil 99, disponibilidad de lectura y escritura del 99,999 % y la capacidad de escalar de forma flexible el rendimiento aprovisionado para lectura y escritura en todas las regiones asociadas con su cuenta de Azure Cosmos. Azure Cosmos DB también ofrece cinco modelos de coherencia bien definidos y se puede aplicar un modelo específico a sus datos. Por último, Azure Cosmos DB es el único servicio de base de datos en el sector que proporciona un completo [acuerdo de nivel de servicio (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) incluyente rendimiento aprovisionado, una latencia en el percentil 99, alta disponibilidad, y coherencia. Estas capacidades están disponibles en todas las nubes de Azure.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede obtener información sobre los conceptos básicos de Azure Cosmos DB con los siguientes artículos:

* [Aplicaciones distribuidas globalmente](distribute-data-globally.md)
* [Administración de cuentas de base de datos en Azure Cosmos DB](manage-account.md)
* [Aprovisionamiento del rendimiento de contenedores y bases de datos de Cosmos DB](set-throughput.md)
* [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

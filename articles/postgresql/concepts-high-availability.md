---
title: 'Conceptos de alta disponibilidad en Azure Database for PostgreSQL: servidor único'
description: 'Este artículo proporciona información de alta disponibilidad con Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: f54c83099957b4d8795c4049be52d70e8a0e2a61
ms.sourcegitcommit: f4469b7bb1f380bf9dddaf14763b24b1b508d57c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "65073448"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql---single-server"></a>Conceptos de alta disponibilidad en Azure Database for PostgreSQL: servidor único
El servicio Azure Database for PostgreSQL garantiza un alto nivel de disponibilidad. El Acuerdo de Nivel de Servicio con respaldo financiero tiene un 99,99 % de disponibilidad general. Al usar este servicio, las aplicaciones prácticamente no tienen tiempo de inactividad.

## <a name="high-availability"></a>Alta disponibilidad
El modelo de alta disponibilidad se basa en los mecanismos de conmutación por error integrados cuando se produce una interrupción en el nivel de nodo. Una interrupción en el nivel de nodo podría producirse debido a un error de hardware o como respuesta a la implementación de un servicio.

En todo momento, se realizan cambios en un servidor de base de datos de Azure Database for PostgreSQL en el contexto de una transacción. Los cambios se registran de forma sincrónica en Azure Storage cuando se confirma la transacción. Si se produce una interrupción en el nivel de nodo, el servidor de bases de datos crea automáticamente un nodo nuevo y adjunta el almacenamiento de datos. Todas las conexiones activas se eliminarán y no se confirmará ninguna transacción en proceso.

## <a name="application-retry-logic-is-essential"></a>La lógica de reintento de la aplicación es esencial
Es importante que las aplicaciones de la base de datos PostgreSQL se creen para detectar y reintentar conexiones eliminadas y transacciones erróneas. Cuando la aplicación realiza un reintento, su conexión se redirige de forma transparente a la instancia recién creada, que sustituye a la instancia errónea.

Internamente en Azure, se usa una puerta de enlace para redirigir las conexiones a la instancia nueva. Tras una interrupción, todo el proceso de conmutación por error tarda normalmente decenas de segundos. Puesto que la redirección se controla internamente mediante la puerta de enlace, la cadena de conexión externa permanece igual en las aplicaciones cliente.

## <a name="scaling-up-or-down"></a>Escalado o reducción vertical
De forma similar al modelo de alta disponibilidad, cuando Azure Database for PostgreSQL se escala o reduce verticalmente, se crea una nueva instancia de servidor con el tamaño especificado. El almacenamiento de datos existente se separa de la instancia original y se agrega a una nueva instancia.

Durante la operación de escalado, se produce una interrupción en las conexiones de la base de datos. Las aplicaciones cliente se desconectan y las transacciones abiertas no confirmadas se cancelan. Una vez que la aplicación cliente reintenta la conexión o crea una conexión nueva, la puerta de enlace dirige la conexión a la instancia cuyo tamaño se modificó. 

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre el [control de errores de conectividad transitorios](concepts-connectivity.md)
- Más información sobre cómo [replicar los datos con réplicas de lectura](howto-read-replicas-portal.md)

---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d579e7a4fd83c1a0ce335e0b2357dcbafb217398
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165375"
---
En este paso, se probará el agente de escucha del grupo de disponibilidad mediante una aplicación de cliente que se ejecuta en la misma red.

La conectividad del cliente tiene los requisitos siguientes:

* Las conexiones de cliente para el agente de escucha tienen que proceder de máquinas que residan en un servicio en la nube diferente al que hospeda las réplicas de disponibilidad Always On.
* Si las réplicas AlwaysOn están en subredes diferentes, los clientes tendrán que especificar *MultisubnetFailover=True* en la cadena de conexión. Esta condición se produce en los intentos de conexión en paralelo con las réplicas de las diversas subredes. Este escenario incluye una implementación de grupo de disponibilidad Always On entre regiones.

Un ejemplo sería conectarse al agente de escucha desde una de las máquinas virtuales de la misma red virtual de Azure (pero no desde una que hospede una réplica). Una manera fácil de completar esta prueba consiste en intentar conectar SQL Server Management Studio al agente de escucha del grupo de disponibilidad. Otro método sencillo es ejecutar [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx) de la siguiente manera:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Si el valor de EndpointPort es *1433*, no es necesario especificarlo en la llamada. La llamada anterior también asume que el equipo cliente está unido al mismo dominio y que el llamador tiene concedidos los permisos en la base de datos mediante la autenticación de Windows.
> 
> 

Al probar el agente de escucha, asegúrese de conmutar por error el grupo de disponibilidad para asegurarte de que los clientes puedan conectarse al agente de escucha a través de las conmutaciones por error.


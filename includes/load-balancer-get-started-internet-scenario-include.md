---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: bf3aee30460e052db23cf9090f13e2af3b22628b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66122190"
---
En este escenario se realizarán las siguientes tareas:

* Crear un equilibrador de carga que reciba tráfico de red en el puerto 80 y envíe tráfico de carga equilibrada a las máquinas virtuales "web1" y "web2".
* Crear reglas NAT para el acceso de escritorio remoto / SSH para máquinas virtuales detrás del equilibrador de carga
* Crear sondeos de estado

![Escenario del equilibrador de carga](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)

---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 8b919608dfc562d8db77619d5215a6828a53a4aa
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165524"
---
1. Mientras está conectado a la máquina virtual con Escritorio remoto, busque **Administrador de configuración**:

    ![Abrir SSCM](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. En el panel de la consola del Administrador de configuración de SQL Server, expanda **Configuración de red de SQL Server**.

1. En el panel de la consola, haga clic en **Protocolos para MSSQLSERVER** (el nombre de instancia predeterminado). En el panel de detalles, haga clic con el botón derecho en **TCP** y, después, haga clic en **Habilitar** si no está habilitado aún.

    ![Habilitar TCP](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. En el panel de la consola, haga clic en **Servicios de SQL Server**. En el panel de detalles, haga clic con el botón derecho en **SQL Server (*nombre de la instancia*)** (la instancia predeterminada es **SQL Server (MSSQLSERVER)**) y, después, haga clic en **Reiniciar** para detener y reiniciar la instancia de SQL Server.

    ![Reiniciar el motor de base de datos](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Cierre el Administrador de configuración de SQL Server.

Para obtener más información acerca de la habilitación de protocolos para el motor de base de datos de SQL Server, consulte [Habilitar o deshabilitar un protocolo de red de servidor](https://msdn.microsoft.com/library/ms191294.aspx).

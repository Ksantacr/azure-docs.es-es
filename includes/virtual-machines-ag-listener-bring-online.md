---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 760bb5b62e9bba9b7a83f99760f7fe5d8c399dfb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165371"
---
1. En el Administrador de clústeres de conmutación por error, expanda **Roles** y, a continuación, resalte el grupo de disponibilidad.  

2. En la pestaña **Recursos**, haga clic con el botón derecho en el nombre del agente de escucha y, a continuación, haga clic en **Propiedades**.

3. Haz clic en la pestaña **Dependencias** . Si aparecen varios recursos, compruebe que las direcciones IP tienen dependencias OR y no AND.  

4. Haga clic en **OK**.

5. Haga clic con el botón derecho en el nombre del agente de escucha y luego haga clic en **Poner en línea**.

6. Una vez que el agente de escucha está en línea, en la pestaña **Recursos**, haga clic con el botón derecho en el grupo de disponibilidad y haga clic en **Propiedades**.
   
    ![Configurar el recurso del grupo de disponibilidad](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Cree una dependencia en el recurso del nombre del agente de escucha (no en el nombre de los recursos de dirección IP) y, a continuación, haga clic en **Aceptar**.
   
    ![Agregar dependencias en el nombre del agente de escucha](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Abra SQL Server Management Studio y conéctese a la réplica principal.

9. Vaya a **Alta disponibilidad AlwaysOn** > **Grupos de disponibilidad** > **\<nombre del grupo de disponibilidad\>** > **Agentes de escucha del grupo de disponibilidad**.  
    Debe mostrarse el nombre del agente de escucha que creó en el Administrador de clústeres de conmutación por error.

10. Haga clic con el botón derecho en el nombre del agente de escucha y luego en **Propiedades**.

11. En el cuadro de texto **Puerto**, especifique el número del puerto del agente de escucha del grupo de disponibilidad mediante el parámetro $EndpointPort usado anteriormente (en este tutorial, el valor predeterminado era 1433) y, después, haga clic en **Aceptar**.


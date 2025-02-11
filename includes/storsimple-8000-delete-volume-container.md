---
title: archivo de inclusión
description: archivo de inclusión
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 06/08/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: e683d17422321b780a1c01b3011292f2e2c631cb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156116"
---
Para eliminar un contenedor de volúmenes, debe
 - eliminar volúmenes del contenedor de volúmenes. Si el contenedor de volúmenes tiene volúmenes asociados, desconecte primero estos volúmenes. Siga los pasos de [Desconexión de un volumen](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Una vez que los volúmenes estén desconectados, puede eliminarlos. 
 - eliminar directivas de copia de seguridad e instantáneas en la nube asociadas. Compruebe si el contenedor de volúmenes tiene directivas de copia de seguridad e instantáneas en la nube asociadas. Si es así, [elimine las directivas de copia de seguridad](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). También se eliminarán las instantáneas en la nube. 
 
Si el contenedor de volúmenes no tiene volúmenes asociados, directivas de copia de seguridad ni instantáneas en la nube, puede eliminarlo. Realice el siguiente procedimiento para eliminar un contenedor de volúmenes.

#### <a name="to-delete-a-volume-container"></a>Para eliminar un contenedor de volúmenes
1. Vaya al servicio Administrador de dispositivos de StorSimple y haga clic en **Dispositivos**. Seleccione y haga clic en el dispositivo y, a continuación, vaya a **Configuración > Administrar > Contenedores de volúmenes**.

    ![Hoja Contenedores de volúmenes](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. En la lista tabular de contenedores de volúmenes, seleccione el contenedor que desea eliminar, haga clic con el botón derecho en **...**  y, a continuación, seleccione **Eliminar**.

    ![Eliminación de un contenedor de volúmenes](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Si un contenedor de volúmenes no tiene volúmenes asociados, directivas de copia de seguridad ni instantáneas en la nube, puede eliminarlo. Cuando se le pida confirmación, revise y seleccione la casilla que indica el impacto de la eliminación del contenedor de volúmenes. Haga clic en **Eliminar** y, a continuación, elimine el contenedor de volúmenes.

    ![Confirmar eliminación](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

La lista de contenedores de volúmenes se actualiza para reflejar el contenedor eliminado.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)



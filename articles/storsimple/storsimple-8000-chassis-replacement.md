---
title: Reemplazo del chasis del dispositivo de la serie 8000 de StorSimple | Microsoft Docs
description: Describe cómo quitar y reemplazar el chasis del gabinete EBOD y del gabinete principal de StorSimple.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 7dfc39f4d08c8a49d1564a0a5bd7e3ef4156e3fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61312452"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Reemplazar el chasis en el dispositivo StorSimple
## <a name="overview"></a>Información general
Este tutorial explica cómo quitar y reemplazar un chasis en un dispositivo de la serie 8000 de StorSimple. El modelo StorSimple 8100 es un dispositivo de un solo gabinete (un chasis), mientras que el 8600 es un dispositivo de dos gabinetes (dos chasis). Para un modelo 8600, hay potencialmente dos chasis que pueden producir un error en el dispositivo: el chasis para el gabinete principal o el chasis del gabinete EBOD.

En cualquier caso, el chasis de reemplazo que se distribuye por Microsoft está vacío. No se incluirán Módulos de alimentación y refrigeración (PCM), módulos de controlador, unidades de disco de estado sólido (SSD), unidades de disco duro (HDD) o módulos EBOD.

> [!IMPORTANT]
> Antes de quitar y reemplazar el chasis, revise la información de seguridad en [Reemplazo de componentes de hardware de StorSimple](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-chassis"></a>Quitar el chasis
Realice los pasos siguientes para quitar el chasis en el dispositivo StorSimple.

#### <a name="to-remove-a-chassis"></a>Para quitar un chasis
1. Asegúrese de que el dispositivo StorSimple esté apagado y desconectado de todas las fuentes de alimentación.
2. Quite todos los cables de red y SAS, si corresponde.
3. Quite la unidad del bastidor.
4. Quite cada una de las unidades de disco y tenga en cuenta las ranuras de las que se quitan. Para obtener más información, consulte [Quitar la unidad de disco](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. En el gabinete EBOD (si es el chasis que produjo un error), quite los módulos del controlador EBOD. Para obtener más información, consulte [Quitar un controlador EBOD](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    En el gabinete principal (si es el chasis que produjo un error), quite los controladores y tenga en cuenta las ranuras de las que se quitaron. Para obtener más información, consulte [Quitar un controlador](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Instalar el chasis
Realice los pasos siguientes para instalar el chasis en el dispositivo StorSimple.

#### <a name="to-install-a-chassis"></a>Para instalar un chasis
1. Monte el chasis en el bastidor. Para obtener más información, consulte [Montaje en bastidor del dispositivo StorSimple 8100](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) o [Montaje en bastidor del dispositivo StorSimple 8600](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Después de que el chasis está montado en el bastidor, instale los módulos de controlador en las mismas posiciones en las que estaban previamente instaladas.
3. Instale las unidades en las mismas posiciones y ranuras en las que estaban previamente instaladas.
   
   > [!NOTE]
   > Se recomienda que primero instale las SSD en las ranuras y, luego, instale los discos duros.
  
4. Con el dispositivo montado en el bastidor y los componentes instalados, conecte el dispositivo a las fuentes de alimentación adecuadas y encienda el dispositivo. Para obtener más información, consulte [Instalación de cables del dispositivo StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) o [Instalación de cables StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre el [Reemplazo de los componentes de hardware de StorSimple](storsimple-8000-hardware-component-replacement.md).


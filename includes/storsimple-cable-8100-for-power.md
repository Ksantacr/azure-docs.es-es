---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: db2104020e9478b1fedf68e1c9467f75e16044e2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155806"
---
#### <a name="to-cable-for-power"></a>Instalación de los cables de alimentación
1. Asegúrate de que el interruptor de alimentación de todos los módulos de alimentación y refrigeración (PCM) se encuentra en la posición de apagado.
2. Conecta los cables de alimentación a todos los PCM en el alojamiento principal.
3. Adjunta los cables de alimentación a las unidades de distribución de energía (PDU) del bastidor tal y como se muestra en la siguiente imagen. Asegúrate de que los dos PCM utilicen fuentes de alimentación independientes.
   
   > [!IMPORTANT]
   > Para garantizar una alta disponibilidad del sistema, se recomienda cumplir estrictamente el esquema de cableado de potencia que se muestra en el siguiente diagrama de cableado. 
   > 
   > 
   
    ![Colocación del cable de alimentación del dispositivo 2U](./media/storsimple-cable-8100-for-power/HCSCableYour2UDeviceforPower.png)
   
    **Cables de alimentación de un dispositivo 8100**
   
   | Etiqueta | DESCRIPCIÓN |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |Controlador 1 |
   | 3 |Controlador 0 |
   | 4 |PCM 1 |
   | 5 |PDU |
4. Para encender el sistema gira el interruptor de alimentación de ambos PCM a la posición de encendido.


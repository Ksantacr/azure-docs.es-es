---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: dc50f94ae9b207961a71480c2fc172e88db79cf4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66171914"
---
#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Para instalar actualizaciones regulares a través de Windows PowerShell para StorSimple
1. Abra la consola serie del dispositivo y seleccione la opción 1, **Iniciar sesión con acceso completo**. Escriba la contraseña. La contraseña predeterminada es *Password1*. 
2. En el símbolo del sistema, escriba:
   
     `Get-HcsUpdateAvailability`
   
    Se le notificará si hay actualizaciones disponibles y si son problemáticas.
3. En el símbolo del sistema, escriba:
   
     `Start-HcsUpdate`
   
    Dará comienzo el proceso de actualización.

> [!IMPORTANT]
> * Este comando solo se aplica a las actualizaciones regulares. Este comando se ejecuta en un solo controlador, pero se actualizarán ambos controladores. 
> * Es posible que observe una conmutación por error del controlador durante el proceso de actualización. Sin embargo, la conmutación por error no afectará a la disponibilidad o el funcionamiento del sistema.
> 
> 


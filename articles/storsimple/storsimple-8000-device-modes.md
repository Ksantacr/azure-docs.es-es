---
title: Cambio del modo del dispositivo StorSimple | Microsoft Docs
description: Describe los modos de dispositivo StorSimple y explica cómo usar Windows PowerShell para StorSimple para cambiar el modo del dispositivo.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e55964beff48df6ce24d99c01975d39b662f1612
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576098"
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Cambiar el modo del dispositivo en su dispositivo StorSimple

En este artículo se proporciona una breve descripción de los distintos modos en que puede funcionar el dispositivo StorSimple. El dispositivo StorSimple puede funcionar en tres modos: normal, de mantenimiento y de recuperación.

Después de leer este artículo, sabrá acerca de:

* ¿Qué son los modos del dispositivo StorSimple?
* Cómo determinar el modo en que está establecido el dispositivo StorSimple.
* Cómo cambiar del modo normal al modo de mantenimiento y *viceversa*

Las tareas de administración mencionadas solo pueden realizarse a través de la interfaz de Windows PowerShell del dispositivo StorSimple.

## <a name="about-storsimple-device-modes"></a>Acerca de los modos del dispositivo StorSimple.

El dispositivo StorSimple puede funcionar en modo normal, de mantenimiento o de recuperación. A continuación se describe brevemente cada uno de estos modos.

### <a name="normal-mode"></a>Modo Normal

Está definido como modo de funcionamiento normal de un dispositivo StorSimple totalmente configurado. De manera predeterminada, su dispositivo debería estar en modo normal.

### <a name="maintenance-mode"></a>Modo Mantenimiento

A veces puede ser necesario colocar el dispositivo StorSimple en el modo de mantenimiento. Este modo le permite realizar tareas de mantenimiento en el dispositivo e instalar actualizaciones potencialmente problemáticas, como las relacionadas con el firmware del disco.

Solo puede poner el sistema en modo de mantenimiento por medio de Windows PowerShell para StorSimple. En este modo, todas las solicitudes de E/S se interrumpen. También se detienen servicios como la memoria de acceso aleatorio no volátil (NVRAM) o el servicio de Cluster Server. Ambos controladores se reinician al entrar o salir de este modo. Al salir del modo de mantenimiento, todos los servicios se reanudan y deben funcionar correctamente. Esta operación puede tardar unos minutos.

> [!NOTE]
> **El modo Mantenimiento solo se admite en un dispositivo que funcione correctamente. No se admite en un dispositivo en el que uno o ambos controladores no funcionen.**


### <a name="recovery-mode"></a>Modo Recuperación

El modo Recuperación puede describirse como el "Modo seguro de Windows con compatibilidad de red". El modo Recuperación avisa al equipo de soporte técnico de Microsoft y le permite realizar diagnósticos en el sistema. El objetivo principal del modo de recuperación es recuperar los registros del sistema.

Si el sistema entra en modo de recuperación, debe ponerse en contacto con el soporte técnico de Microsoft para obtener asistencia. Para obtener más información, vaya a [Contactar al soporte técnico de Microsoft](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **No puede poner el dispositivo en modo de recuperación. Si el dispositivo está en mal estado, el modo de recuperación intenta llevar al dispositivo a un estado en el que el personal de soporte técnico de Microsoft pueda examinarlo.**

## <a name="determine-storsimple-device-mode"></a>Determinar el modo de dispositivo StorSimple

#### <a name="to-determine-the-current-device-mode"></a>Para determinar el modo del dispositivo actual

1. Inicie sesión en la consola serie del dispositivo siguiendo los pasos detallados en [Uso de PuTTY para conectarse a la consola serie del dispositivo](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Mire el mensaje de pancarta del menú de la consola serie del dispositivo. Este mensaje indica de forma explícita si el dispositivo está en modo de mantenimiento o de recuperación. Si el mensaje no contiene información específica relativa al modo del sistema, el dispositivo está en modo normal.

## <a name="change-the-storsimple-device-mode"></a>Cambiar el modo del dispositivo StorSimple

Puede colocar el dispositivo StorSimple en modo de mantenimiento (desde el modo normal) para realizar tareas de mantenimiento o instalar las actualizaciones del modo de mantenimiento. Lleve a cabo los siguientes procedimientos para entrar o salir del modo de mantenimiento.

> [!IMPORTANT]
> Antes de entrar en modo de mantenimiento, compruebe que ambos controladores de dispositivo están en buenas condiciones accediendo a **Configuración del dispositivo > Mantenimiento de hardware** para el dispositivo en Azure Portal. Si uno o ambos controladores no tienen un estado correcto, póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. Para obtener más información, vaya a [Contactar al soporte técnico de Microsoft](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="to-enter-maintenance-mode"></a>Para acceder al modo de mantenimiento

1. Inicie sesión en la consola serie del dispositivo siguiendo los pasos detallados en [Uso de PuTTY para conectarse a la consola serie del dispositivo](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. En el menú de la consola serie, seleccione la opción 1, **Iniciar sesión con acceso completo**. Cuando se le solicite, proporcione la **contraseña de administrador de dispositivo**. La contraseña predeterminada es: `Password1`.
3. En el símbolo del sistema, escriba 
   
    `Enter-HcsMaintenanceMode`
4. Verá un mensaje de advertencia que indica que el modo de mantenimiento interrumpirá todas las solicitudes de E/S y detendrá la conexión en Azure Portal. También se le solicitará la confirmación. Escriba **Y** para acceder al modo de mantenimiento.
5. Ambos controladores se reiniciarán. Una vez completado el reinicio, el banner de la consola de serie indicará que el dispositivo se encuentra en modo de mantenimiento. A continuación se muestra una salida de ejemplo.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="to-exit-maintenance-mode"></a>Para salir del modo de mantenimiento

1. Inicie sesión en la consola serie del dispositivo. En el mensaje de pancarta, verifique que el dispositivo esté en el modo Mantenimiento.
2. En el símbolo del sistema, escriba:
   
    `Exit-HcsMaintenanceMode`
3. Aparecerán un mensaje de advertencia y un mensaje de confirmación. Escriba **Y** para salir del modo de mantenimiento.
4. Ambos controladores se reiniciarán. Una vez completado el reinicio, el banner de la consola de serie indica que el dispositivo se encuentra en modo normal. A continuación se muestra una salida de ejemplo.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure to install on each controller could result in data corruption. Exiting maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to exit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [aplicar las actualizaciones del modo normal y del de mantenimiento](storsimple-update-device.md) en el dispositivo StorSimple.


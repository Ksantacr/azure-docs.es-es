---
title: Tutorial sobre la instalación del dispositivo físico de Azure Data Box Edge, cómo desempaquetarlo, montarlo en el bastidor y cablearlo | Microsoft Docs
description: El segundo tutorial sobre la instalación de Azure Data Box Edge incluye cómo desempaquetar el dispositivo físico, montarlo en el bastidor y cablearlo.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 7062e9d829242a62dfa4f661c21ce6c56b0db69c
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924535"
---
# <a name="tutorial-install-azure-data-box-edge"></a>Tutorial: Instalación de Azure Data Box Edge

Este tutorial describe cómo instalar un dispositivo físico de Data Box Edge. El procedimiento de instalación incluye el desempaquetado, el montaje en el bastidor y el cableado del dispositivo. 

La instalación puede tardar aproximadamente dos horas en completarse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desempaquetado del dispositivo
> * Montaje en bastidor del dispositivo
> * Cableado del dispositivo

## <a name="prerequisites"></a>Requisitos previos

Los requisitos previos para instalar un dispositivo físico son los siguientes:

### <a name="for-the-data-box-edge-resource"></a>Para el recurso de Data Box Edge

Antes de comenzar, asegúrese de que:

* Ha completado todos los pasos del tutorial [Preparación para la implementación de Azure Data Box Edge](data-box-edge-deploy-prep.md).
    * Ha creado un recurso de Data Box Edge para implementar el dispositivo.
    * Ha generado la clave de activación para activar el dispositivo con el recurso de Data Box Edge.

 
### <a name="for-the-data-box-edge-physical-device"></a>Para el dispositivo físico de Data Box Edge

Antes de implementar un dispositivo:

- Asegúrese de colocar el dispositivo de forma segura sobre una superficie de trabajo plana, estable y nivelada.
- Compruebe que el sitio donde desea realizar la instalación tiene:
    - Corriente alterna estándar de una fuente independiente

        O
    - Una unidad de distribución de energía (PDU) en bastidor con un sistema de alimentación ininterrumpida (UPS)
    - Una ranura 1U disponibles en el bastidor en el que se va a montar el dispositivo

### <a name="for-the-network-in-the-datacenter"></a>Para la red del centro de datos

Antes de empezar:

- Revise los requisitos de red para implementar Data Box Edge y configure la red del centro de datos según dichos requisitos. Para más información, consulte [Requisitos de red de Data Box Edge](data-box-edge-system-requirements.md#networking-port-requirements).

- Asegúrese de que el ancho de banda mínimo de Internet sea de 20 Mbps para que el dispositivo funcione de forma óptima.


## <a name="unpack-the-device"></a>Desempaquetado del dispositivo

Este dispositivo se suministra en una única caja. Realice los pasos siguientes para desempaquetar el dispositivo. 

1. Coloque la caja en una superficie plana y nivelada.
2. Compruebe si la caja y la espuma del embalaje presentan golpes, cortes, daños por agua o cualquier otro daño evidente. Si la caja o el embalaje están muy dañados, no los abra. Póngase en contacto con el soporte técnico de Microsoft para ayudarle a determinar si el dispositivo está en buen estado.
3. Desempaquete la caja. Después de desempaquetar la caja, asegúrese de que dispone de:
    - Un único dispositivo contenedor Edge
    - Dos cables de alimentación
    - Un ensamblaje de kit de raíl
    - Un folleto informativo sobre la seguridad, el entorno y las normativas

Si no recibió alguno de los elementos enumerados aquí, póngase en contacto con el soporte técnico de Data Box Edge. El paso siguiente es el montaje en bastidor del dispositivo.


## <a name="rack-the-device"></a>Montaje del dispositivo en el bastidor

El dispositivo debe instalarse en un bastidor estándar de 19 pulgadas. Use el siguiente procedimiento para montar el dispositivo en un bastidor estándar de 19 pulgadas.

> [!IMPORTANT]
> Los dispositivos de Data Box Edge deben estar montados en un bastidor para funcionar correctamente.


### <a name="prerequisites"></a>Requisitos previos

- Antes de comenzar, lea las instrucciones de seguridad en el folleto sobre la seguridad, el entorno y las normativas. Este folleto se envió junto con el dispositivo.
- Comience la instalación de los raíles en el espacio asignado más próximo a la parte inferior del armario del bastidor.
- Para la configuración de montaje del raíl con herramientas:
    -  deberá suministrar ocho tornillos: #10-32, #12-24, #M5 o #M6. El diámetro de los cabezales de los tornillos debe ser inferior a 10 mm (0,4 pulgadas).
    -  Necesita un destornillador de punta plana.

### <a name="identify-the-rail-kit-contents"></a>Identificación del contenido del kit de raíl

Ubique los componentes de instalación de ensamblaje del kit de raíl:
1. Dos ensamblajes de raíl deslizantes A7 Dell ReadyRails II
2. Dos correas anchas con cierre de velcro

![Identificación del contenido del kit de raíl](./media/data-box-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Instalación y retirada de los raíles no mecanizados (bastidores de agujeros cuadrados y redondos)

> [!TIP]
> Esta opción es sin herramientas, ya no requiere herramientas para instalar y quitar los raíles en los orificios cuadrados o redondos lisos de los bastidores.

1. Coloque las piezas del extremo de los raíles izquierdo y derecho con la etiqueta **FRONT** orientadas hacia dentro y fijadas en los agujeros anteriores de las pestañas del bastidor en vertical.
2. Alinee cada pieza del extremo en los agujeros inferiores y superiores de los espacios en U preferidos.
3. Ajuste el extremo posterior del raíl hasta que quede fijado en la pestaña del bastidor en vertical y el pestillo haga clic. Repita estos pasos para colocar y fijar la pieza del extremo anterior en la pestaña del bastidor en vertical.
4. Para quitar los carriles, suelte el botón de desbloqueo del pestillo de la parte central de la pieza del extremo y levante los raíles.

![Instalación y retirada de los raíles no mecanizados](./media/data-box-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Instalación y retirada de raíles mecanizados (bastidores con agujeros a rosca)

> [!TIP]
> Esta opción tiene herramientas porque requiere una herramienta (_un destornillador de punta plana_) para instalar y quitar los raíles en los orificios redondos con rosca de los bastidores.

1. Quite las fijaciones de los soportes de montaje anterior y posterior con un destornillador plano.
2. Tire de los submódulos de pestillo del raíl y gírelos para sacarlos de los soportes de montaje.
3. Encaje los raíles de montaje izquierdo y derecho a las pestañas anteriores del bastidor en vertical con 4 tornillos.
4. Deslice los soportes posteriores izquierdo y derecho hacia delante contra las pestañas posteriores del bastidor en vertical y encájelos con 4 tornillos.

![Instalación y retirada de raíles no mecanizados](./media/data-box-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Instalación del sistema en un bastidor

1. Tire de los raíles deslizantes interiores hacia fuera del bastidor hasta que se bloqueen en su sitio.
2. Ubique los separadores de los raíles posteriores laterales del sistema y encájelos en las ranuras en J de los ensamblajes deslizantes. Gire el sistema hacia abajo hasta que todos los separadores de los raíles queden fijos en las ranuras en J.
3. Inserte el sistema hasta que las pestañas de cierre hagan clic.
4. Presione los botones de bloqueo deslizantes de los dos raíles y deslice el sistema para que encaje en el bastidor.

![Instalación del sistema en un bastidor](./media/data-box-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Extracción del sistema del bastidor

1. Ubique las pestañas de cierre laterales en los raíles internos.
2. Desbloquee las pestañas; para ello, gírelas hasta la posición de desbloqueo.
3. Sujete firmemente los laterales del sistema e inserte este hasta que los separadores de los raíles queden frente a las ranuras en J. Levante el sistema y sáquelo del bastidor para colocarlo en una superficie nivelada.

![Extracción del sistema del bastidor](./media/data-box-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Bloqueo y desbloqueo del pestillo de golpe

> [!NOTE]
> Si su sistema no dispone de pestillo de golpe, fíjelo con tornillos tal y como se describe en el paso 3 de este procedimiento.

1. De cara a la parte anterior, ubique el pestillo de golpe de alguno de los laterales del sistema.
2. Los pestillos se encajan automáticamente al insertar el sistema en el bastidor y se desbloquean al tirar de ellos.
3. Para fijar el sistema para el transporte en el bastidor o en otros entornos inestables, ubique el tornillo de montaje rígido bajo cada pestillo y apriételo con un destornillador Phillips del número 2.

![Bloqueo y desbloqueo del pestillo de golpe](./media/data-box-edge-deploy-install/engaging-releasing-slam-latch.png)

### <a name="route-the-cables"></a>Orientación de los cables

> [!NOTE]
>  Si no solicitó el soporte de orientación de cables (CMA) opcional, use las dos correas anchas con cierre de velcro suministradas con el kit del raíl para orientar los cables en la parte posterior del sistema.

1. Ubique los soportes del CMA externos en los laterales interiores de las pestañas de los bastidores.
2. Agrupe los cables con cuidado, bien extendidos a partir de los conectores del sistema de los laterales izquierdo y derecho.
3. Enrosque cintas de velcro en las ranuras de los soportes del CMA externos de cada lateral del sistema para fijar los mazos de cables.


![Orientación de los cables](./media/data-box-edge-deploy-install/routing-cables.png)

## <a name="cable-the-device"></a>Cableado del dispositivo

Los siguientes procedimientos describen cómo realizar el cableado del dispositivo de Edge para la conexión a la corriente y a la red.

Antes de empezar el cableado del dispositivo, necesita lo siguiente:

- El dispositivo físico de Edge, desempaquetado y montado en el bastidor.
- Dos cables de alimentación. 
- Al menos un cable de red RJ-45 de 1-GbE cable para conectarse a la interfaz de administración de red. Hay dos interfaces de red de 1-GbE, uno de administración y otro de datos, en el dispositivo.
- Un cable de cobre SFP+ de 25 GbE para cada interfaz de red de datos que se va a configurar. Al menos una interfaz de red de datos de entre los PUERTOS 2, 3, 4, 5 o 6, debe estar conectada a Internet (para la conectividad a Azure).  
- Acceso a dos unidades de distribución de energía (recomendable).

> [!NOTE]
> - Si va a conectar solo una interfaz de red de datos, es recomendable que use una interfaz de red de 25/10 GbE como la de los PUERTOS 3, 4, 5 o 6 para enviar datos a Azure. 
> - Para obtener el mejor rendimiento y controlar grandes volúmenes de datos, considere la posibilidad de conectar todos los puertos de datos.
> - El dispositivo de Edge debe estar conectado a la red del centro de datos para poder ingerir datos desde los servidores de origen de datos.

El dispositivo de Edge tiene 8 SSD de NVMe. El panel frontal incluye también indicadores LED de estado y botones de alimentación. El dispositivo incluye fuentes de alimentación (PSU) redundantes en la parte posterior. El dispositivo tiene seis interfaces de red:

- Dos interfaces de 1 Gbps
- Cuatro interfaces de 25 Gbps que también sirven como interfaces de 10 Gbps.
- Un controlador de administración de placa base (BMC). 

Identifique los distintos puertos de la placa posterior del dispositivo.
 
  ![Placa posterior de un dispositivo cableado](./media/data-box-edge-deploy-install/backplane-cabled.png)

El dispositivo tiene dos tarjetas de red correspondientes a los 6 puertos: 

 - QLogic FastLinQ 41264
 - QLogic FastLinQ 41262

Para una lista completa de los cables, enchufes y transceptores para estas tarjetas de red, vaya al artículo sobre la [matriz de interoperabilidad de la serie Cavium FastlinQ 41000](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Realice los pasos siguientes para realizar el cableado de los cables de alimentación y de red del dispositivo.

1. Conecte los cables de alimentación a cada una de las fuentes de alimentación del receptáculo. Para garantizar una alta disponibilidad, instale y conecte ambas fuentes de alimentación a distintas tomas de alimentación.

2. Conecte los cables de alimentación a las unidades de distribución de energía (PDU) del bastidor. Asegúrese de que las dos PSU usen tomas de alimentación independientes.

3. Conecte el PUERTO 1 de la interfaz de red de 1-GbE al equipo que se usa para configurar el dispositivo físico. El PUERTO 1 es la interfaz de administración dedicada.

4. Conecte uno o varios de los PUERTOS 2, 3, 4, 5 o 6 a la red del centro de datos o Internet. Si conecta el PUERTO 2, utilice el cable de red RJ-45. Para las interfaces de red de 10/25 GbE, use cables de cobre SFP+.  

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Desempaquetado del dispositivo
> * Montaje del dispositivo en el bastidor
> * Cableado del dispositivo

Pase al siguiente tutorial para aprender a conectar, configurar y activar el dispositivo.

> [!div class="nextstepaction"]
> [Conexión y configuración de Data Box Edge](./data-box-edge-deploy-connect-setup-activate.md)



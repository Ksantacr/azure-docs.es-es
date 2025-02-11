---
title: Reprotección de máquina virtuales de Azure conmutadas por recuperación en la región principal de Azure con Azure Site Recovery | Microsoft Docs
description: Se describe cómo reproteger las máquinas virtuales de Azure en una región secundaria, después de la conmutación por error desde una región principal, con Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: eabb7d194a3ef65282befab1ae59e85ba56f2f5b
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472154"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Reprotección de máquinas virtuales de Azure conmutadas por error en la región principal


Al [conmutar por error](site-recovery-failover.md) las máquinas virtuales de Azure desde una región a otra con [Azure Site Recovery](site-recovery-overview.md), las máquinas virtuales se inician en la región secundaria, con un estado desprotegido. Si conmuta por recuperación las máquinas virtuales en la región principal, necesita hacer lo siguiente:

- Vuelva a proteger las máquinas virtuales en la región secundaria, para que empiecen a replicarse en la región primaria.
- Una vez completada la reprotección y después de que las máquinas virtuales se estén replicando, puede realizar una conmutación por error de ellas desde la región secundaria a la principal.

## <a name="prerequisites"></a>Requisitos previos
1. La conmutación por error de la máquina virtual de la región principal a la secundaria se debe confirmar.
2. El sitio de destino principal debe estar disponible y, además, debe tener la posibilidad de acceder o crear recursos en dicha región.

## <a name="reprotect-a-vm"></a>Reprotección de una máquina virtual

1. En **Almacén** > **Elementos replicados**, haga clic con el botón derecho en la máquina virtual que se conmutó por error y, luego, seleccione **Volver a proteger**. La dirección de reprotección debe mostrarse desde la ubicación secundaria a la principal.

   ![Reprotección](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Revise el grupo de recursos, la red, el almacenamiento y los conjuntos de disponibilidad. A continuación, haga clic en **Aceptar**. Si hay algún recurso marcado como nuevo, se crea como parte del proceso de reprotección.
3. Este trabajo de reprotección inicializa el sitio de destino con los datos más recientes. Una vez finalizado el proceso, se produce la replicación diferencial. Después, puede conmutar por recuperación al sitio principal. Puede seleccionar la cuenta de almacenamiento o la red que desea usar durante la reprotección mediante la opción de personalización.

   ![Opción de personalización](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Personalización de la configuración de reprotección

Puede personalizar las siguientes propiedades de la máquina virtual de destino durante la reprotección.

![Personalizar](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Propiedad |Notas  |
|---------|---------|
|Grupo de recursos de destino     | Modifique el grupo de recursos de destino en que se crea la máquina virtual. Como parte de la reprotección, se elimina la máquina virtual de destino. Puede elegir un grupo de recursos nuevo en el que crear la máquina virtual tras la conmutación por error.        |
|Red virtual de destino     | No se puede cambiar la red de destino durante el trabajo de reprotección. Para cambiar la red, vuelva a hacer la asignación de red.         |
|Almacenamiento de destino (la VM secundaria no utiliza discos administrados)     | Puede cambiar la cuenta de almacenamiento que la máquina virtual usa tras la conmutación por error.         |
|Discos administrados de réplica (la VM secundaria utiliza discos administrados)    | Site Recovery crea discos administrado de réplica en la región principal para crear el reflejo de discos administrados de la VM secundaria.         |
|Almacenamiento en caché     | Puede especificar una cuenta de almacenamiento en caché para usarla durante la replicación. De forma predeterminada, se crea una cuenta de almacenamiento en caché en caso de que no exista.         |
|Conjunto de disponibilidad     |Si la máquina virtual de la región secundaria forma parte de un conjunto de disponibilidad, puede elegir un conjunto de disponibilidad para la máquina virtual de destino en la región principal. De forma predeterminada, Site Recovery intenta encontrar el conjunto de disponibilidad existente en la región principal para usarlo. Durante la personalización, puede especificar un nuevo conjunto de disponibilidad.         |


### <a name="what-happens-during-reprotection"></a>¿Qué ocurre durante la reprotección?

De forma predeterminada, sucede lo siguiente:

1. Se crea una cuenta de almacenamiento en caché en la región donde se está ejecutando la máquina virtual conmutada por error.
2. Si la cuenta de almacenamiento de destino (la cuenta de almacenamiento original de la región principal) no existe, se crea una. El nombre asignado para la cuenta de almacenamiento se corresponde con el nombre de la cuenta de almacenamiento que usa la máquina virtual secundaria, con el sufijo "asr".
3. Si la máquina virtual utiliza discos administrados, los discos administrados de réplica se crean en la región principal para almacenar los datos replicados de discos de la máquina virtual secundaria.
4. Si el conjunto de disponibilidad de destino no existe, se crea uno como parte del trabajo de reprotección, en caso de que sea necesario. Si personalizó la configuración de reprotección, entonces se usa el conjunto seleccionado.

Cuando se desencadena un trabajo de reprotección y la máquina virtual de destino ya existe, ocurre lo siguiente:

1. La máquina virtual de destino se apaga en caso de que esté en ejecución.
2. Si la máquina virtual está usando discos administrados, se crea una copia de los discos originales con el sufijo "-ASRReplica". Los discos originales se eliminan. Las copias de "-ASRReplica" se usan para la replicación.
3. Si la máquina virtual usa discos no administrados, los discos de datos de la máquina virtual de destino se separan y se usan para la replicación. Se crea una copia del disco del sistema operativo y se adjunta a la máquina virtual. El disco del sistema operativo original se separa y se usa para la replicación.
4. Solo se sincronizan los cambios entre el disco de origen y el disco de destino. Las copias de seguridad diferenciales se calculan comparando los discos y, a continuación, se transfieren. Para buscar la siguiente comprobación de tiempo estimado.
5. Una vez que finaliza el trabajo de sincronización, comienza la replicación diferencial y se crea un punto de recuperación según la directiva de replicación.

Cuando se desencadena un trabajo de reprotección y la máquina virtual y los discos de destino no existen, ocurre lo siguiente:
1. Si usa discos administrados en la máquina virtual, los discos de réplica se crean con el sufijo "-ASRReplica". Las copias de "-ASRReplica" se usan para la replicación.
2. Si usa discos no administrados en la máquina virtual, los discos de réplica se crean en la cuenta de almacenamiento de destino.
3. Todos los discos se copian de la región que ha conmutado por error a la nueva región de destino.
4. Una vez que finaliza el trabajo de sincronización, comienza la replicación diferencial y se crea un punto de recuperación según la directiva de replicación.

#### <a name="estimated-time--to-do-the-reprotection"></a>Tiempo estimado para realizar la reprotección 

En la mayoría de los casos, Azure Site Recovery no replica los datos completos en la región de origen. A continuación se muestran las condiciones que determina la cantidad de datos se replicarán:

1.  Si el origen de datos de la máquina virtual es eliminada, está dañado o inaccesible por algún motivo, como el grupo de recursos cambia o elimina, a continuación, durante la reprotección completa IR se realizará porque no hay ningún dato disponible en la región de origen para usar.
2.  Si el origen de datos de la máquina virtual es accesible solo copias de seguridad diferenciales se calcula comparando los discos y, a continuación, se transfieren. Compruebe la siguiente tabla para obtener el tiempo estimado 

|** Situación de ejemplo ** | ** Tiempo necesario para volver a proteger ** |
|--- | --- |
|Región de origen tiene 1 máquina virtual con discos estándar de 1 TB<br/>-Solo 127 GB se utilizan datos y el resto del disco está vacío<br/>-Tipo de disco es estándar con un rendimiento de 60 MiB/S<br/>-Ningún cambio de datos después de la conmutación por error| Hora aproximada en 45 minutos – 1,5 horas<br/> -Durante la reprotección, Site Recovery se llenará la suma de comprobación de datos completa que le llevarán a 127 GB / 45 MB aproximadamente 45 minutos<br/>-Un tiempo de sobrecarga es necesario para la recuperación del sitio automáticamente la escala que es de 20 a 30 minutos<br/>-No hay cargos de salida |
|Región de origen tiene 1 máquina virtual con discos estándar de 1 TB<br/>-Solo 127 GB se utilizan datos y el resto del disco está vacío<br/>-Tipo de disco es estándar con un rendimiento de 60 MiB/S<br/>-Los cambios de datos de 45 GB tras la conmutación por error| Tiempo aproximado de 1 horas: 2 horas<br/>-Durante la reprotección, Site Recovery se llenará la suma de comprobación de datos completa que le llevarán a 127 GB / 45 MB aproximadamente 45 minutos<br/>-Tiempo para aplicar los cambios de 45 GB es 45 GB de transferencia / 45 MBps ~ 17 minutos<br/>-Los cargos de salida sería únicamente para datos de 45 GB no para la suma de comprobación|
 



## <a name="next-steps"></a>Pasos siguientes

Una vez protegida la máquina virtual, se puede iniciar una conmutación por error. La conmutación por error apaga la máquina virtual en la región secundaria y se crea y arranca la máquina virtual en la región principal, con un breve tiempo de inactividad. Se recomienda elegir una hora según corresponda y ejecutar una prueba de conmutación por error antes de iniciar una conmutación por error completa al sitio principal. [Más información](site-recovery-failover.md) sobre la conmutación por error

---
title: Azure Backup de SAP HANA basada en instantáneas de almacenamiento | Microsoft Docs
description: Existen dos posibilidades de copia de seguridad principales para SAP HANA en Azure Virtual Machines. Este artículo trata sobre la copia de seguridad de SAP HANA basada en instantáneas de almacenamiento
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 74f47344afff630a8633b340ea4ce21db28db7ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60936608"
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento

## <a name="introduction"></a>Introducción

Esto forma parte de una serie de tres partes de artículos relacionados sobre copia de seguridad de SAP HANA. [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md) proporciona una introducción e información sobre los pasos iniciales, y [Azure Backup de SAP HANA en el nivel de archivo](sap-hana-backup-file-level.md) trata la opción de copia de seguridad basada en archivos.

Cuando se usa una característica de copia de seguridad de VM para un sistema de demostración todo en uno de instancia única, debe considerar la posibilidad de realizar una copia de seguridad de VM en lugar de administrar copias de seguridad para HANA en el nivel de sistema operativo. Una alternativa consiste en tomar instantáneas de blobs de Azure para crear copias de discos virtuales individuales, que se asocian a una máquina virtual, y conservan los archivos de datos HANA. Sin embargo, un punto crítico es coherencia de la aplicación cuando se crea una instantánea de disco o copia de seguridad de VM cuando el sistema está en funcionamiento. Vea _Coherencia de datos de SAP HANA al tomar instantáneas de almacenamiento_ en el artículo relacionado [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md). SAP HANA tiene una característica que admite estos tipos de instantáneas de almacenamiento.

## <a name="sap-hana-snapshots-as-central-part-of-application-consistent-backups"></a>Instantáneas de SAP HANA como parte central de las copias de seguridad coherentes con las aplicaciones

Hay una funcionalidad en SAP HANA que admite la toma de una instantánea de almacenamiento. Hay una restricción a los sistemas de contenedor único. Los escenarios de MCS de SAP HANA con más de un inquilino no admiten este tipo de instantánea de base de datos de SAP HANA [consulte [Creación de una instantánea de almacenamiento (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)].

Funciona como se indica a continuación:

- Preparar para una instantánea de almacenamiento mediante la realización de la instantánea de SAP HANA
- Ejecutar la instantánea de almacenamiento (instantánea de blobs de Azure, por ejemplo)
- Confirmar la instantánea de SAP HANA

![Esta captura de pantalla muestra que se puede crear una instantánea de datos SAP HANA a través de una instrucción SQL](media/sap-hana-backup-storage-snapshots/image011.png)

Esta captura de pantalla muestra que se puede crear una instantánea de datos SAP HANA a través de una instrucción SQL.

![La instantánea también aparece en el catálogo de copia de seguridad en SAP HANA Studio](media/sap-hana-backup-storage-snapshots/image012.png)

La instantánea también aparece en el catálogo de copia de seguridad en SAP HANA Studio.

![En el disco, la instantánea aparece en el directorio de datos de SAP HANA](media/sap-hana-backup-storage-snapshots/image013.png)

En el disco, la instantánea aparece en el directorio de datos de SAP HANA.

Tiene que asegurarse de que también se garantiza la coherencia del sistema de archivos antes de ejecutar la instantánea de almacenamiento mientras SAP HANA está en el modo de preparación de la instantánea. Vea _Coherencia de datos de SAP HANA al tomar instantáneas de almacenamiento_ en el artículo relacionado [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md).

Una vez que se realiza la instantánea de almacenamiento, es fundamental confirmar la instantánea de SAP HANA. Hay una instrucción SQL correspondiente que ejecutar: INSTANTÁNEA DE CIERRE DE DATOS DE COPIA DE SEGURIDAD (vea [Instrucción de INSTANTÁNEA DE CIERRE DE DATOS DE COPIA DE SEGURIDAD (Copia de seguridad y restauración)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Confirme la instantánea de HANA. Debido a la &quot;copia en escritura&quot;, SAP HANA puede requerir espacio en disco adicional en el modo de preparación de la instantánea y que no sea posible iniciar nuevas copias de seguridad hasta que se confirme la instantánea de SAP HANA.

## <a name="hana-vm-backup-via-azure-backup-service"></a>Copia de seguridad de VM de HANA a través del servicio Azure Backup

El agente de copia de seguridad del servicio Azure Backup no está disponible para máquinas virtuales Linux. Además, Linux no tiene una funcionalidad similar a la de Windows con VSS.  Para hacer uso de Azure Backup en el nivel de archivo o directorio, podría copiar archivos de copia de seguridad para SAP HANA en una VM Windows y, a continuación, utilizar el agente de copia de seguridad. 

En caso contrario, solo una copia de seguridad completa de VM de Linux es posible a través del servicio Azure Backup. Vea [Introducción a las características de Azure Backup](../../../backup/backup-introduction-to-azure-backup.md) para obtener más información.

El servicio Azure Backup ofrece una opción para realizar la copia de seguridad y restauración de una VM. Obtenga más información sobre este servicio y cómo funciona en el artículo [Planeación de la infraestructura de copia de seguridad de máquinas virtuales en Azure](../../../backup/backup-azure-vms-introduction.md).

Existen dos consideraciones importantes según dicho artículo:

_&quot;En las máquinas virtuales Linux, solo se pueden realizar copias de seguridad coherentes con el archivo, dado que Linux no dispone de una plataforma equivalente a VSS.&quot;_

_&quot;Las aplicaciones deben implementar su propio mecanismo de &quot;reparación&quot; en los datos restaurados.&quot;_

Por lo tanto, tiene que asegurarse de que SAP HANA está en un estado coherente en el disco cuando se inicia la copia de seguridad. Vea _Instantáneas de SAP HANA_, que se describe anteriormente en el documento. Sin embargo, hay un posible problema cuando SAP HANA permanece en este modo de preparación de la instantánea. Vea [Creación de una instantánea de almacenamiento (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) para obtener más información.

El artículo establece lo siguiente:

_&quot;Se recomienda encarecidamente confirmar o abandonar una instantánea de almacenamiento tan pronto como sea posible después de haberse creado. Mientras se prepara o crea la instantánea de almacenamiento, los datos relevantes para la instantánea están inmovilizados. Cuando los datos relevantes para la instantánea están inmovilizados, todavía pueden realizarse cambios en la base de datos. Estos cambios no harán que cambien los datos relevantes para la instantánea inmovilizados. En su lugar, los cambios se escriben en posiciones en el área de datos que son independientes de la instantánea de almacenamiento. Los cambios también se escriben en el registro. Sin embargo, conforme más tiempo estén inmovilizados los datos relevantes para la instantánea, más podrá aumentar el volumen de datos.&quot;_

Azure Backup se encarga de la coherencia del sistema de archivos a través de las extensiones de máquina virtual de Azure. Estas extensiones no están disponible de forma independiente y solo funcionan en combinación con el servicio Azure Backup. Sin embargo, sigue siendo un requisito proporcionar scripts para crear y eliminar una instantánea de SAP HANA para garantizar la coherencia de la aplicación.

Azure Backup tiene cuatro fases principales:

- Ejecutar el script de preparación: el script debe crear una instantánea de SAP HANA
- Tomar la instantánea
- Ejecutar el script posterior a la instantánea: el script necesita eliminar el SAP HANA creado por el script de preparación.
- Transferir los datos al almacén

Para más información sobre dónde copiar estos scripts y detalles sobre cómo funciona exactamente Azure Backup, consulte los siguientes artículos:

- [Planeación de la infraestructura de copia de seguridad de VM en Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)
- [Copias de seguridad coherentes con la aplicación de las máquinas virtuales Linux de Azure](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)



En este momento, Microsoft no ha publicado scripts de preparación ni scripts posteriores a la instantánea para SAP HANA. Como cliente o integrador de sistemas necesitaría crear esos scripts y configurar el procedimiento basado en la documentación mencionada anteriormente.


## <a name="restore-from-application-consistent-backup-against-a-vm"></a>Restauración desde una copia de seguridad coherente con la aplicación en una máquina virtual
El proceso de restauración de una copia de seguridad coherente con la aplicación tomada por la copia de seguridad de Azure está documentado en el artículo [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm). 

> [!IMPORTANT]
> En el artículo [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm) se muestra una lista de excepciones y de pasos enumerados al usar conjuntos de franjas del disco. Los discos con franjas son probablemente la configuración de máquina virtual normal para SAP HANA. Por lo tanto, es esencial leer el artículo y probar el proceso de restauración para los casos enumerados en el artículo. 



## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>Restauración de VM y de la clave de licencia de HANA a través del servicio Azure Backup

El servicio Azure Backup se ha diseñado para crear una nueva VM durante la restauración. No hay ningún plan ahora para realizar una restauración &quot;in situ&quot; de una máquina virtual de Azure.

![Esta ilustración muestra la opción de restauración de Azure Service en Azure Portal](media/sap-hana-backup-storage-snapshots/image019.png)

Esta ilustración muestra la opción de restauración de Azure Service en Azure Portal. Puede elegir entre crear una VM durante la restauración o restaurar los discos. Después de restaurar los discos, sigue siendo necesario crear una nueva VM en la parte superior. Cada vez que se cree una nueva VM en Azure, el identificador único de VM cambia (vea [Acceso y uso del identificador único de máquina virtual de Azure](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Esta ilustración muestra el identificador único de la máquina virtual de Azure antes y después de la restauración a través del servicio Azure Backup](media/sap-hana-backup-storage-snapshots/image020.png)

Esta ilustración muestra el identificador único de la máquina virtual de Azure antes y después de la restauración a través del servicio Azure Backup. La clave de hardware SAP, que se utiliza para la concesión de licencias SAP, usa este identificador único de VM. Por lo tanto, debe instalarse una nueva licencia SAP después de la restauración de VM.

Se presentó una nueva característica de Azure Backup en el modo de vista previa durante la creación de esta guía de copia de seguridad. Permite una restauración de nivel de archivo en la instantánea de VM que se tomó para la copia de seguridad de VM. Esto evita la necesidad de implementar una nueva VM y, por lo tanto, el identificador único de VM sigue siendo el mismo y no se requiere una nueva clave de licencia de SAP HANA. Una vez está probado completamente, se proporcionará más documentación sobre esta característica.

Azure Backup finalmente permitirá la copia de seguridad de los discos virtuales de Azure individuales, además de archivos y directorios desde dentro de la VM. Una ventaja fundamental de Azure Backup es la administración de todas las copias de seguridad, que evita que el cliente tenga que hacer dicha tarea. Si la restauración es necesaria, Azure Backup seleccionará la copia de seguridad correcta que utilizar.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>Copia de seguridad de VM de SAP HANA a través de una instantánea de disco manual

En lugar de usar el servicio Azure Backup, puede configurar una solución de copia de seguridad individual mediante la creación de instantáneas de blob de VHD de Azure manualmente a través de PowerShell. Vea [Uso de instantáneas de blob con PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) para obtener una descripción de los pasos.

Proporciona mayor flexibilidad, pero no resuelve los problemas explicados anteriormente en este documento:

- Puede que tenga que asegurarse de que SAP HANA tiene un estado coherente al crear una instantánea de SAP HANA.
- El disco del SO no puede sobrescribirse incluso si la VM se desasigna debido a un error que establece que hay una concesión. Solo funciona después de eliminar la VM, lo que provocaría que hubiera un nuevo ID de VM único y que fuese necesario instalar una nueva licencia SAP.

![Es posible restaurar solo los discos de datos de una máquina virtual de Azure](media/sap-hana-backup-storage-snapshots/image021.png)

Es posible restaurar solo los discos de datos de una máquina virtual de Azure y evitar el problema de obtener un nuevo identificador único de VM y, por lo tanto, la invalidación de la licencia SAP en los siguientes casos:

- Para la prueba, se asociaron dos discos de datos de Azure a una VM y se definió el RAID de software en la parte superior de ellos 
- Se confirmó que SAP HANA se encontraba en un estado coherente mediante la característica de la instantánea de SAP HANA
- Inmovilización del sistema de archivos (vea _Coherencia de datos de SAP HANA al tomar instantáneas de almacenamiento_ en el artículo relacionado en la [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md))
- Se toman instantáneas de blob desde ambos discos de datos
- Cancelación de la inmovilización del sistema de archivos
- Confirmación de la instantánea de SAP HANA
- Para restaurar los discos de datos, se cerró la VM y se desasociaron ambos discos
- Después de la desasociación de los discos, se sobrescribieron con las instantáneas de blob anteriores
- Después, los discos virtuales restaurados se asociaron nuevamente a la VM
- Después de iniciar la VM, todo en el RAID de software funcionaba correctamente y se volvió a establecer en la hora de la instantánea de blob
- HANA se volvió a establecer en la instantánea de HANA

Si fuese posible apagar SAP HANA antes de las instantáneas de blob, el procedimiento sería menos complejo. En ese caso, podría omitir la instantánea de HANA y, si no ocurre nada más en el sistema, omitir también la inmovilización del sistema de archivos. Esta situación es aún más compleja cuando es necesario realizar instantáneas mientras todo está en línea. Vea _Coherencia de datos de SAP HANA al tomar instantáneas de almacenamiento_ en el artículo relacionado [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Pasos siguientes
* [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](sap-hana-backup-guide.md) ofrece una introducción e información sobre los pasos iniciales.
* [Azure Backup de SAP HANA en el nivel de archivo](sap-hana-backup-file-level.md) describe la opción de copia de seguridad basada en archivos.
* Para obtener información sobre cómo establecer la alta disponibilidad y planear la recuperación ante desastres de SAP HANA en Azure (instancias grandes), vea [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](hana-overview-high-availability-disaster-recovery.md).

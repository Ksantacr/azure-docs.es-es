---
title: Ejemplos de la CLI de Azure para Windows | Microsoft Docs
description: Ejemplos de la CLI de Azure para Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8bc151089f76e3f27ababb479f5e893ca9a99365
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844064"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Ejemplos de la CLI de Azure para máquinas virtuales Windows

En la tabla siguiente se incluyen vínculos a scripts de Bash creados con la CLI de Azure que implementan máquinas virtuales Windows.

| | |
|---|---|
|**Creación de máquinas virtuales**||
| [Creación de una máquina virtual](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea una máquina virtual Windows con una configuración mínima. |
| [Creación una máquina virtual completamente configurada](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea un grupo de recursos, una máquina virtual y todos los recursos relacionados.|
| [Creación de máquinas virtuales de alta disponibilidad](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea varias máquinas virtuales de alta disponibilidad y una configuración de equilibrio de carga. |
| [Creación de una máquina virtual y ejecución de un script de configuración](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea una máquina virtual y usa la extensión de script personalizado de Azure para instalar IIS. |
| [Creación de una máquina virtual y ejecución de una configuración de DSC](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea una máquina virtual y usa la extensión de configuración de estado deseado (DSC) de Azure para instalar IIS. |
|**Administrar el almacenamiento**||
| [Crear disco administrado desde un disco duro virtual](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea un disco administrado desde un disco duro virtual especializado como disco del sistema operativo o desde un VHD como disco de datos de datos.  |
| [Crear un disco administrado a partir de una instantánea](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea un disco administrado a partir de una instantánea. |
| [Copiar el disco administrado en la misma suscripción o en otra](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Copia el disco administrado en la misma u otra suscripción pero en la misma región que el disco administrado primario. 
| [Exportar una instantánea como un disco duro virtual a una cuenta de almacenamiento](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporta una instantánea administrada como un disco duro virtual a una cuenta de almacenamiento en una región diferente. |
| [Exportar el disco duro virtual de un disco administrado a una cuenta de almacenamiento](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporta el disco duro virtual subyacente de un disco administrado a una cuenta de almacenamiento de otra región. |
| [Copiar una instantánea en la misma suscripción o en otra](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Copia la instantánea en la misma u otra suscripción, pero en la misma región que la instantánea primaria. |
|**Máquinas virtuales de red**||
| [Protección del tráfico de red entre máquinas virtuales](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea dos máquinas virtuales, todos los recursos relacionados y grupos de seguridad de red internos y externos (NSG). |
|**Protección de máquinas virtuales**||
| [Cifrado de una máquina virtual y discos de datos](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea una instancia de Azure Key Vault, una clave de cifrado y una entidad de servicio, y luego cifra una máquina virtual. |
|**Supervisión de máquinas virtuales**||
| [Supervisar una máquina virtual con Azure Monitor](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Crea una máquina virtual, instala el agente de Log Analytics e inscribe la máquina virtual en un área de trabajo de Log Analytics.  |
| | |

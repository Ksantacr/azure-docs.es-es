---
title: 'Ejemplo de script de Azure PowerShell: creación de un disco administrado a partir de una instantánea | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: creación de un disco administrado a partir de una instantánea'
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 055a24aac01b58178e8a4e24986786eeffff3430
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249698"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Creación de un disco administrado a partir de una instantánea con PowerShell

Este script crea un disco administrado a partir de una instantánea. Úselo para restaurar una máquina virtual a partir de las instantáneas de discos de sistema operativo y datos. Cree los discos de sistema operativo y datos a partir de las instantáneas correspondientes y, luego, cree una nueva máquina virtual conectando los discos administrados. También puede restaurar los discos de datos de una máquina virtual existente conectando los discos de datos creados a partir de las instantáneas.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un disco administrado a partir de una instantánea. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/Get-AzSnapshot) | Obtiene las propiedades de la instantánea.  |
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | Crea la configuración de disco que se usa para la creación del disco. Incluye el identificador de recurso de la instantánea principal y una ubicación igual a la de la instantánea principal y el tipo de almacenamiento.  |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Crea un disco mediante la configuración de disco, el nombre del disco y el nombre del grupo de recursos que se pasan como parámetros. |

## <a name="next-steps"></a>Pasos siguientes


[Crear una máquina virtual a partir de un disco administrado](./virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

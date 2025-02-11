---
title: Comandos comunes de PowerShell para Azure Virtual Machines | Microsoft Docs
description: Comandos comunes de PowerShell para comenzar a crear y administrar máquinas virtuales Windows en Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 9d0e27c2427d53554b454e0c319ce9cf180f1633
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820797"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Comandos comunes de PowerShell para crear y administrar Azure Virtual Machines

En este artículo se tratan algunos de los comandos de Azure PowerShell que puede usar para crear y administrar máquinas virtuales de su suscripción de Azure.  Para más ayuda con opciones y modificadores concretos de la línea de comandos, puede utilizar el **comando** *Get-Help*.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

Estas variables podrían serle útiles si ejecuta más de uno de los comandos de este artículo:

- $location: la ubicación de la máquina virtual. Puede usar [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) para buscar una [región geográfica](https://azure.microsoft.com/regions/) que se adapte a sus necesidades.
- Valor predeterminado personalizado: Nombre del grupo de recursos que contiene las máquinas virtuales.
- $myVM: el nombre de la máquina virtual.

## <a name="create-a-vm---simplified"></a>Creación de una máquina virtual: método simplificado

| Tarea | Get-Help |
| ---- | ------- |
| Creación de una máquina virtual simple | [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -Name $myVM <BR></BR><BR></BR> New-AzVM tiene un conjunto de parámetros *simplificados* donde todo lo que se requiere es un nombre. El valor de -Name se usará como nombre de todos los recursos necesarios para crear una máquina virtual. Se pueden especificar más, pero esto es todo lo necesario.|
| Crear una imagen personalizada a partir de una máquina virtual | New-AzVm -ResourceGroupName $myResourceGroup -Name $myVM ImageName "miImagen" -Location $location  <BR></BR><BR></BR>Debe haber creado su propia [imagen administrada](capture-image-resource.md). Puede usar una sola imagen para crear varias máquinas virtuales idénticas. |



## <a name="create-a-vm-configuration"></a>Creación de una configuración de máquina virtual

| Tarea | Get-Help |
| ---- | ------- |
| Creación de una configuración de máquina virtual |$vm = [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>La configuración de máquina virtual se utiliza para definir o actualizar la configuración de la máquina virtual. La configuración se inicializa con el nombre de la VM y su [tamaño](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Adición de valores de configuración |$vm = [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>La configuración del sistema operativo que incluye [credenciales](https://technet.microsoft.com/library/hh849815.aspx) se agrega al objeto de configuración que creó anteriormente mediante New-AzVMConfig. |
| Adición de una interfaz de red |$vm = [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Una VM debe tener un [interfaz de red](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para comunicarse en una red virtual. También puede usar [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) para recuperar un objeto de interfaz de red existente. |
| Especificación de una imagen de plataforma |$vm = [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) -VM $vm -PublisherName "nombre_publicador" -Offer "oferta_publicador" -Skus "sku_producto" -Version "más_reciente"<BR></BR><BR></BR>[La información de la imagen](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) se agrega al objeto de configuración que creó anteriormente mediante New-AzVMConfig. El objeto que se devuelve con este comando solo se utiliza cuando se establece el disco del sistema operativo que utilizará una imagen de plataforma. |
| Crear una VM |[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Todos los recursos se crean en un [grupo de recursos](../../azure-resource-manager/manage-resource-groups-powershell.md). Antes de ejecutar este comando, ejecute New-AzVMConfig, Set-AzVMOperatingSystem, Set-AzVMSourceImage, Add-AzVMNetworkInterface y Set-AzVMOSDisk. |
| Actualización de una máquina virtual |[Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Obtenga la configuración de VM actual mediante Get-AzVM, cambie los valores de configuración del objeto de VM y, luego, ejecute este comando. |

## <a name="get-information-about-vms"></a>Obtención de información sobre VM

| Tarea | Get-Help |
| ---- | ------- |
| Enumeración de las máquinas virtuales de una suscripción |[Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) |
| Enumeración de las máquinas virtuales de un grupo de recursos |Get-AzVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Para obtener una lista de grupos de recursos de la suscripción, use [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup). |
| Obtención información acerca de una máquina virtual |Get-AzVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Administración de máquinas virtuales
| Tarea | Get-Help |
| --- | --- |
| Inicio de una máquina virtual |[Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Detención de una máquina virtual |[Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Reinicio de una máquina virtual en ejecución |[Restart-AzVM](https://docs.microsoft.com/powershell/module/az.compute/restart-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Eliminación de una máquina virtual |[Remove-AzVM](https://docs.microsoft.com/powershell/module/az.compute/remove-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |


## <a name="next-steps"></a>Pasos siguientes
* Consulte los pasos básicos para crear una máquina virtual en [Creación de una máquina virtual de Windows con Resource Manager y PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


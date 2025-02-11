---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244664"
---
Estos son los tamaños de los objetos de Azure que se pueden escribir. Asegúrese de que todos los archivos que se cargan se ajustan a estos límites.

| Tipo de objeto de Azure | Límite predeterminado                                             |
|-------------------|-----------------------------------------------------------|
| Blob en bloques        | ~4,75 TiB                                                 |
| Blob en páginas         | 8 TiB <br> Todos los archivos cargados en formato de blob en páginas deben tener 512 bytes alineados (un múltiplo entero), de lo contrario, se produce un error en la carga. <br> VHD y VHDX tienen 512 bytes alineados. |
| Archivos de Azure        | 1 TiB                                                      |
| Discos administrados     | 4 TiB <br> Para obtener más información sobre el tamaño y los límites, consulte: <li>[Objetivos de escalabilidad de SSDs estándar](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Objetivos de escalabilidad de Premium SSD](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Objetivos de escalabilidad de unidades de disco duro estándar](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Precios y facturación de discos administrados](../articles/virtual-machines/windows/disks-types.md#billing)</li>  

---
title: 'Compatibilidad con el administrador de representación: Azure Batch'
description: Uso de Azure para representar mediante la integración del administrador de representación de Azure Batch
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: bcc66a73e3d7986b177b13eb309ad664a006b960
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118593"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Uso de Azure Batch con administradores de granja de representación

Si usa una granja de representaciones locales existente, es muy probable que un administrador de representación controle la capacidad de la granja de representaciones y represente los trabajos.

Azure proporciona compatibilidad integrada o complementos para administradores de representación populares. Después, puede agregar y quitar máquinas virtuales de Azure, incluidas las máquinas virtuales con las licencias de aplicación de pago por uso y las máquinas virtuales de prioridad baja.

Se admiten los siguientes administradores de representación:

* [¡PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](http://www.royalrender.de/)
* [Fecha límite Thinkbox](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Uso de Azure con PipelineFX Qube

Los scripts y las instrucciones para habilitar las máquinas virtuales de grupos de Azure Batch que se usarán como trabajos de Qube se encuentran en [el repositorio de GitHub](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Uso de Azure con Royal Render

Render Royal tiene incorporada la integración de Azure y Azure Batch, lo que permite ampliar una granja de representaciones con las máquinas virtuales basadas en Azure. Para un resumen, consulte [los archivos de ayuda](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Para un ejemplo de un cliente de Royal Render que usa la integración de Azure, consulte la [historia de cliente Jellyfish Pictures](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Uso de Azure con Thinkbox Deadline

Los scripts y las instrucciones para habilitar las máquinas virtuales de grupos de Azure Batch que se usarán como esclavos de Deadline se encuentran en [el repositorio de GitHub](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Pasos siguientes

Pruebe la integración de Azure Batch para el administrador de representación, mediante el complemento y las instrucciones apropiados en GitHub, si procede.
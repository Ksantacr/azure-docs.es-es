---
title: Guía de publicación de la oferta de plantilla de solución de aplicaciones de Azure | Azure Marketplace
description: En este artículo se describen los requisitos para publicar una plantilla de solución en Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.topic: article
ms.date: 11/15/2018
ms.author: ellacroi
ms.openlocfilehash: c2393b6ea9f1a2c2b35be63272743e081f4ae240
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64937749"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Aplicaciones de Azure: guía de publicación de ofertas de plantillas de solución

Las plantillas de solución son uno de los principales mecanismos para publicar una solución en Marketplace. Use esta guía para comprender los requisitos para esta oferta. 

Use el tipo de oferta Aplicación de Azure: plantilla de solución cuando la solución requiera automatización adicional de implementación y configuración para algo más que una sola máquina virtual. Puede automatizar el aprovisionamiento de una o más VM con plantillas de solución de aplicaciones de Azure. También puede aprovisionar los recursos de redes y almacenamiento. El tipo de oferta Aplicaciones de Azure: plantillas de solución ofrece ventajas de automatización para VM únicas y soluciones completas basadas en IaaS.

Estas plantillas de solución son ofertas de transacción que se implementan y facturan a través de Marketplace. La llamada a la acción que el usuario ve es "Obtener ahora".


## <a name="requirements-for-solution-templates"></a>Requisitos para las plantillas de solución

| **Requisitos** | **Detalles**  |
| ---------------  | -----------  |
|Facturación y medición    |  Los recursos se aprovisionarán en la suscripción a Azure del cliente. Las transacciones de máquinas virtuales de pago por uso (PAYGO) se realizarán con el cliente mediante Microsoft y se facturarán a través de la suscripción a Azure del cliente (PAYGO).  <br/> En el caso del modelo denominado traiga su propia licencia (BYOL), Microsoft facturará los costos de infraestructura derivados de la suscripción del cliente, mientras que usted realizará la transacción de las tarifas de licencia de software directamente con el cliente.   |
|Disco duro virtual (VHD) compatible con Azure  |   Las máquinas virtuales deben estar basadas en Windows o Linux.  Para más información, consulte [Creación de un disco duro virtual compatible con Azure](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md). |
| Atribución de uso del cliente | Se requiere habilitar la atribución de uso del cliente en todas las plantillas de solución publicadas en Azure Marketplace. Para más información sobre la atribución de uso del cliente y cómo habilitarla, consulte [Atribución de uso del cliente para asociados de Azure](./azure-partner-customer-usage-attribution.md).  |
|  |  |

## <a name="next-steps"></a>Pasos siguientes
Si no lo ha hecho ya, [regístrese](https://azuremarketplace.microsoft.com/sell) en Marketplace.

Si ya lo está y va a crear una oferta o trabaja en una existente, inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com) para crear o completar la oferta.

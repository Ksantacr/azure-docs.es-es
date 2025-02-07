---
title: Entrega de contenido en China con Azure CDN | Microsoft Docs
description: Obtenga información acerca del uso de Azure Content Delivery Network (CDN) para entregar contenido a los usuarios de China.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: da59e9bb5cfffea734cb1dc4725cef9ea6296aa2
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872929"
---
# <a name="china-content-delivery-with-azure-cdn"></a>Entrega de contenido en China con Azure CDN

La red global Azure Content Delivery Network (CDN) puede servir contenido a los usuarios de China con ubicaciones de punto de presencia (POP) cerca de China o cualquier POP que proporcione mejor rendimiento para las solicitudes procedentes de China. Sin embargo, si China es un mercado significativo para los clientes y necesitan un rendimiento rápido, considere la posibilidad de usar Azure CDN China en su lugar.

La red Azure CDN China difiere de la red CDN de Azure global en que ofrece el contenido desde POP situados dentro de China mediante la asociación con un número de proveedores locales. Debido a las regulaciones y normas de cumplimiento chinas, debe registrar una suscripción independiente para usar Azure CDN China y los sitios web deben tener una licencia ICP. La experiencia del portal y la API para habilitar y administrar el contenido de entrega es idéntica entre la red CDN de Azure global y Azure CDN China.

## <a name="comparison-of-azure-cdn-global-and-azure-cdn-china"></a>Comparación de la red CDN de Azure global y Azure CDN China

La red CDN de Azure global y Azure CDN China tienen las siguientes características:

- Azure CDN global:

     - Portal: https://portal.azure.com  

     - Realiza la entrega de contenido fuera de China

     - Cuatro niveles de precios: Estándar de Microsoft, estándar, premium de Verizon y Akamai estándar

     - [Documentación](https://docs.microsoft.com/azure/cdn/)

- Azure CDN China:

     - Portal: https://portal.azure.cn

     - Realiza la entrega de contenido dentro de China

     - Dos niveles de precios: Standard y premium

     - [Documentación](https://docs.azure.cn/en-us/cdn/)
 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure CDN China, consulte:

- [Características de Azure Content Delivery Network](https://www.azure.cn/en-us/home/features/cdn/)

- [Introducción a Azure Content Delivery Network](https://docs.azure.cn/en-us/cdn/cdn-overview)

- [Uso de Azure Content Delivery Network](https://docs.azure.cn/en-us/cdn/cdn-how-to-use)

- [Disponibilidad del servicio de Azure en China](https://docs.microsoft.com/azure/china/concepts-service-availability)




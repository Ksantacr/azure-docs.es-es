---
title: Oferta de Azure IoT Edge Module información general de publicación | Azure Marketplace
description: Introducción al proceso de publicación de una oferta de módulo IoT Edge en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: pabutler
ms.openlocfilehash: 319031ec99d449ea5866bb5234cc617145954173
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942610"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>Introducción a la publicación de ofertas de módulos IoT Edge

<table> <tr> <td>En esta sección se explica cómo publicar una nueva oferta de módulo de Azure IoT Edge en Microsoft <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>. Un módulo IoT Edge es un contenedor compatible con Docker diseñado para ejecutarse en un dispositivo IoT Edge. Los módulos de Azure IoT Edge son la unidad más pequeña de cálculo que administra IoT Edge y pueden contener servicios de Azure o su propio código personalizado de la solución. </td> <td><img src="./media/iotedge-icon1.png"  alt="Azure IoT Edge module icon" /></td> </tr> </table>

## <a name="publishing-process"></a>Proceso de publicación

Los pasos generales para publicar una oferta de módulo IoT Edge son:

1. Crear la oferta<br> Proporcione información detallada sobre la oferta. Esta información incluye: la descripción de la oferta, materiales de marketing, información de soporte técnico y especificaciones de recursos.

2. Crear los recursos técnicos y empresariales<br> Cree los recursos empresariales (documentos legales y materiales de marketing) y los recursos técnicos de la solución asociada (las imágenes de módulo IoT Edge hospedadas en Azure Container Registry).

3. Crear la SKU<br> Cree las SKU asociadas a la oferta. Se necesita una SKU única para cada imagen que se vaya a publicar.

4. Certificar y publicar la oferta <br>Después de completar la oferta y los recursos técnicos, la puede enviar. Este envío inicia el proceso de publicación. Durante este proceso, la solución se prueba, se valida, se certifica y luego se publica en Azure Marketplace.

## <a name="parts-of-an-offer"></a>Partes de una oferta

En los artículos siguientes se tratan las partes principales de una oferta de módulo IoT Edge.

- [Requisitos previos](./cpp-prerequisites.md) <br>En este artículo se enumeran los requisitos técnicos y empresariales para poder crear o publicar una oferta de módulo IoT Edge.
- [Preparar los recursos técnicos del módulo IoT Edge](./cpp-create-technical-assets.md) <br>En este artículo se explica cómo preparar los recursos técnicos de un módulo IoT Edge. Estos recursos deben cumplir todos los criterios técnicos necesarios para que el módulo IoT Edge se pueda publicar en Azure Marketplace.
- [Creación de una oferta de módulo de IoT Edge](./cpp-create-offer.md) <br>En este artículo se enumeran los pasos necesarios para crear una nueva entrada de oferta de módulo IoT Edge mediante [Cloud Partner Portal](https://cloudpartner.azure.com).
- [Publicación de una oferta del módulo de IoT Edge](./cpp-publish-offer.md)<br> En este artículo se explica cómo enviar la oferta para su publicación en Azure Marketplace.

## <a name="next-steps"></a>Pasos siguientes

Vea los [requisitos técnicos y empresariales](./cpp-prerequisites.md) para publicar un módulo IoT Edge en Microsoft Azure Marketplace.

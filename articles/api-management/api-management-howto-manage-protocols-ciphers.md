---
title: Administración de protocolos y cifrados en Azure API Management | Microsoft Docs
description: Obtenga información sobre cómo administrar protocolos (TLS, SSL) y cifrados (DES) en Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2019
ms.author: apimpm
ms.openlocfilehash: f809aaf872607e7fa61368832a3df74318f2a858
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66141499"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Administración de protocolos y cifrados en Azure API Management

Azure API Management admite varias versiones del protocolo TLS para los lados cliente y back-end, así como el cifrado 3DES.

Esta guía muestra cómo administrar la configuración de los protocolos y cifrados en una instancia de Azure API Management.

![Administración de protocolos y cifrados en APIM](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos de este artículo, debe tener:

* Una instancia de API Management

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>Cómo administrar protocolos TLS y el cifrado 3DES

1. Vaya a la instancia de **API Management** en Azure Portal.
2. Seleccione **configuración del protocolo** en el menú.  
3. Habilite o deshabilite los protocolos o los cifrados que quiera.
4. Haga clic en **Save**(Guardar). Los cambios se aplicarán en el plazo de una hora.  

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [TLS (Seguridad de la capa de transporte)](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
* Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
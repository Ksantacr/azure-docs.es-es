---
title: Creación de suscripciones en Azure API Management | Microsoft Docs
description: Aprenda a crear suscripciones en Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: bc791fea1dfd184749e84cb7b7a912972c6a9f12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657624"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Creación de suscripciones en Azure API Management

Al publicar las API mediante Azure API Management, es sencillo y frecuente proteger el acceso a ellas mediante claves de suscripción. Las aplicaciones cliente que necesiten usar las API publicadas deben incluir una clave de suscripción válida en las solicitudes HTTP al realizar llamadas a esas API. Para obtener una clave de suscripción para acceder a las API se necesita una suscripción. Para más información sobre las suscripciones, consulte [Suscripciones en Azure API Management](api-management-subscriptions.md).

Este artículo le guiará por los pasos necesarios para crear suscripciones en Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para aprovechar los pasos de este artículo, los requisitos previos son los siguientes:

+ [Crear una instancia de API Management](get-started-create-service-instance.md).
+ Entender las [suscripciones en API Management](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Crear una nueva suscripción

1. Seleccione **Suscripciones** en el menú de la izquierda.
2. Seleccione **Agregar suscripción**.
3. Proporcione un nombre de suscripción y seleccione el ámbito.
4. Si lo desea, elija si se debe asociar con un usuario de la suscripción.
5. Seleccione **Guardar**.

![Suscripciones flexibles](./media/api-management-subscriptions/flexible-subscription.png)

Después de crear la suscripción, se proporcionan dos claves para acceder a las API. Una clave es la principal y la otra, la secundaria. 

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre API Management:

+ Aprenda otros [conceptos](api-management-terminology.md) en API Management.
+ Siga nuestros [tutoriales](import-and-publish.md) para más información sobre API Management.
+ Consulte nuestra [página de preguntas frecuentes](api-management-faq.md) para ver las preguntas habituales.
---
title: Revisar el acceso a grupos o aplicaciones en las revisiones de acceso - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo revisar el acceso a los miembros del grupo o aplicación en las revisiones de acceso de Azure Active Directory.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cd5bbba681acaa0c32e681f7cb4809142fe11f9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113243"
---
# <a name="review-access-to-groups-or-applications-in-azure-ad-access-reviews"></a>Revisar el acceso a los grupos o las revisiones de acceso de las aplicaciones de Azure AD

Azure Active Directory (Azure AD) simplifica el modo en que las empresas administran el acceso a grupos y aplicaciones de Azure AD y revisiones de otros servicios en línea de Microsoft con una característica denominada acceso de Azure AD.

En este artículo se describe cómo un revisor designado lleva a cabo una revisión de acceso para los miembros de un grupo o los usuarios con acceso a una aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Azure AD Premium P2

Para obtener más información, consulte [qué usuarios deben tener licencias?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="open-the-access-review"></a>Abra la revisión de acceso

Es el primer paso para realizar una revisión de acceso buscar y abrir la revisión de acceso.

1. Busque un correo electrónico de Microsoft que le pide que revise el acceso. Este es un correo electrónico de ejemplo para revisar el acceso para un grupo.

    ![Correo electrónico de revisión de acceso](./media/perform-access-review/access-review-email.png)

1. Haga clic en el **iniciar revisión** vínculo para abrir la revisión de acceso.

Si no tiene el correo electrónico, puede encontrar que revisa su acceso pendiente siguiendo estos pasos.

1. Inicie sesión en el portal de MyApps en [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![Portal de MyApps](./media/perform-access-review/myapps-access-panel.png)

1. En la esquina superior derecha de la página, haga clic en el símbolo de usuario que muestra la organización predeterminada y el nombre de usuario. Si aparece más de una organización, seleccione la que haya solicitado una revisión de acceso.

1. Haga clic en el **las revisiones de acceso** icono para ver una lista de las revisiones de acceso pendientes.

    Si el icono no está visible, no hay ninguna revisión de acceso para esa organización y no se requiere ninguna acción en este momento.

    ![Lista de revisiones de acceso](./media/perform-access-review/access-reviews-list.png)

1. Haga clic en el **comenzar revisión** vínculo para la revisión de acceso que desea realizar.

## <a name="perform-the-access-review"></a>Realizar la revisión de acceso

Una vez que ha abierto la revisión de acceso, verá los nombres de los usuarios que deban revisarse.

Si la solicitud es para revisar su propio acceso, la página tendrá un aspecto diferente. Para obtener más información, consulte [revisar el acceso por sí mismo a grupos o aplicaciones](review-your-access.md).

![Realizar la revisión de acceso](./media/perform-access-review/perform-access-review.png)

Hay dos formas que puede aprobar o denegar el acceso:

- Puede aprobar o denegar el acceso para uno o más usuarios, o
- Puede aceptar las recomendaciones del sistema, que es la manera más fácil y rápida.

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Aprobar o denegar el acceso para uno o varios usuarios

1. Revise la lista de los usuarios decidan si aprobar o denegar un acceso continuo.

1. Para aprobar o denegar el acceso para un solo usuario, haga clic en la fila para abrir una ventana para especificar la acción que se realizará. Para aprobar o denegar el acceso de varios usuarios, agregue marcas de verificación junto a los usuarios y, a continuación, haga clic en el **revisión X usuarios** botón para abrir una ventana para especificar la acción que se realizará.

1. Haga clic en **aprobar** o **denegar**. Si no está seguro, haga clic en **desconoce**. Si lo hace, se producirá en el usuario mantiene el acceso, pero la selección se reflejarán en los registros de auditoría.

    ![Realizar la revisión de acceso](./media/perform-access-review/approve-deny.png)

1. Si es necesario, escriba un motivo en el **motivo** cuadro.

    El Administrador de la revisión de acceso puede requerir que proporcione un motivo para aprobar un acceso continuo o una pertenencia a grupos.

1. Una vez que ha especificado la acción a realizar, haga clic en **guardar**.

    Si desea cambiar la respuesta, seleccione la fila y la respuesta de actualización. Por ejemplo, puede aprobar un usuario denegado anteriormente o denegar a un usuario aprobado antes. Puede cambiar su respuesta en cualquier momento hasta que haya finalizado la revisión de acceso.

    Si hay varios revisores, se registra la última respuesta enviada. Considere un ejemplo donde un administrador designa dos revisores: Alice y Bob. Alice abre primero la revisión de acceso y aprueba el acceso. Antes de que finalice la revisión, Bob abre la revisión de acceso y deniega el acceso. El último denegar la respuesta es lo que está registrado.

    > [!NOTE]
    > Si un usuario tiene acceso denegado, no se quitan inmediatamente. Se quitan cuando la revisión ha finalizado o cuando un administrador detiene la revisión.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Aprobar o denegar el acceso según las recomendaciones

Para realizar las revisiones de acceso rápido y sencillo para usted, también proporcionamos las recomendaciones que puede aceptar con un solo clic. Las recomendaciones se generan en función de actividad de inicio de sesión del usuario.

1. En la barra azul en la parte inferior de la página, haga clic en **Aceptar recomendaciones**.

    ![Aceptar recomendaciones](./media/perform-access-review/accept-recommendations.png)

    Ver un resumen de las acciones recomendadas.

    ![Aceptar recomendaciones resumen](./media/perform-access-review/accept-recommendations-summary.png)

1. Haga clic en **Aceptar** para aceptar las recomendaciones.

## <a name="next-steps"></a>Pasos siguientes

- [Revisión de acceso de grupos o aplicaciones](complete-access-review.md)

---
title: 'Incorporación de información de privacidad de una organización: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo agrear información de privacidad de su organización en el área de propiedades de Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 98b4e3585f25a0b081e30c5524678a0f7bb27942
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60248559"
---
# <a name="add-your-organizations-privacy-info-using-azure-active-directory"></a>Incorporación de información de privacidad de su organización con Azure Active Directory
Este artículo explica cómo un administrador de inquilinos puede agregar información relacionada con la privacidad al inquilino de Azure Active Directory (Azure AD) de una organización, a través de Azure Portal.

Se recomienda agregar su contacto de privacidad global y la declaración de privacidad de su organización, de modo que los empleados internos e invitados externos puedan revisar las directivas. Dado que las declaraciones de privacidad se crean de forma única y específica para cada negocio, es recomendable ponerse en contacto con un abogado para obtener ayuda.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="add-your-privacy-info-on-azure-ad"></a>Incorporación de la información de privacidad en Azure AD
Puede agregar información de privacidad de su organización en el área de **Propiedades** de Azure AD.

### <a name="to-access-the-properties-area-and-add-your-privacy-information"></a>Para acceder al área de propiedades y agregar la información de privacidad

1.  Inicie sesión en Azure Portal como administrador de inquilinos.

2.  En la barra de navegación izquierda, seleccione **Azure Active Directory** y, luego, seleccione **Propiedades**.

    Se muestra el área de **Propiedades**.

    ![Área de propiedades de Azure AD, donde se resalta el área de información de privacidad](media/active-directory-properties-area/properties-area.png)

3.  Agregar la información de privacidad de sus empleados:

    - **Contacto técnico.** Escriba la dirección de correo electrónico de la persona de contacto para el soporte técnico de su organización.
    
    - **Contacto de privacidad global.** Escriba la dirección de correo electrónico de la persona de contacto para consultas sobre privacidad de los datos personales. Esta persona también es quien se pone en contacto con Microsoft si se produce una vulneración de datos. Si no aparece ninguna persona aquí, Microsoft se pone en contacto con los administradores globales.

    - **URL de la declaración de privacidad.** Escriba el vínculo del documento de su organización que describe la forma en que su organización controla la privacidad de datos interna y externa del invitado.

        >[!Important]
        >Si no incluye su propia declaración de privacidad o su contacto de privacidad, los invitados externos verán el texto en el cuadro de diálogo **Permisos de revisión** que dice:  **< _nombre de su organización_> no ha proporcionado vínculos de sus términos que pueda revisar**. Por ejemplo, un usuario invitado verá este mensaje cuando reciba una invitación para acceder a una organización a través de la colaboración B2B.

        ![Cuadro de diálogo de permisos de revisión de colaboración B2B con el mensaje](media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes
- [Canje de invitación de colaboración B2B de Azure Active Directory](https://aka.ms/b2bredemption)
- [Adición o modificación de la información de perfil de un usuario en Azure Active Directory](active-directory-users-profile-azure-portal.md)

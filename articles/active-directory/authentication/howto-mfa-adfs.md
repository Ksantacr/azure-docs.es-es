---
title: 'Proteger los recursos de nube con Azure MFA y AD FS: Azure Active Directory'
description: En esta página de Azure Multi-Factor Authentication se describe cómo empezar a trabajar con Azure MFA y AD FS 2.0 en la nube.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0abf2eca52616638f0c4dce89691c0d4f7875106
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60415370"
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Protección de recursos en la nube con Azure Multi-Factor Authentication y AD FS

Si su organización está federada con Azure Active Directory, use Azure Multi-Factor Authentication o los servicios de federación de Active Directory (AD FS) para proteger los recursos a los que se accede mediante Azure AD. Utilice los siguientes procedimientos para proteger recursos de Azure Active Directory mediante Azure Multi-Factor Authentication o Servicios de federación de Active Directory.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Protección de los recursos de Azure AD mediante AD FS

Para proteger los recursos de la nube, configure una regla de notificaciones para que los servicios de federación de Active Directory emitan la notificación multipleauthn cuando un usuario realice correctamente la verificación en dos pasos. Esta notificación se transmitirá a Azure AD. Siga este procedimiento para realizar los pasos:

1. Abra Administración de AD FS.
2. A la izquierda, seleccione **Relaciones de confianza para usuario autenticado**.
3. Haga clic con el botón derecho en **Plataforma de identidad de Microsoft Office 365** y seleccione **Editar reglas de notificaciones**.

   ![Consola de AD FS: usuario autenticado](./media/howto-mfa-adfs/trustedip1.png)

4. En Reglas de transformación de emisión, haga clic en **Agregar regla**.

   ![Edición de reglas de transformación de emisión](./media/howto-mfa-adfs/trustedip2.png)

5. En el Asistente para agregar regla de notificaciones de transformación, seleccione **Pasar por una notificación entrante o filtrarla** en la lista desplegable y haga clic en **Siguiente**.

   ![Asistente para agregar reglas de notificación de transformación](./media/howto-mfa-adfs/trustedip3.png)

6. Asigne un nombre a la regla. 
7. Seleccione **Referencias de métodos de autenticación** como tipo de notificación entrante.
8. Seleccione **Pasar a través todos los valores de notificaciones**.
    ![Asistente para agregar regla de notificación de transformación](./media/howto-mfa-adfs/configurewizard.png)
9. Haga clic en **Finalizar** Cierre la consola de administración de AD FS.

## <a name="trusted-ips-for-federated-users"></a>Direcciones IP de confianza para usuarios federados

Las direcciones IP de confianza permiten a los administradores omitir la verificación en dos pasos para una dirección IP específica o para usuarios federados que tienen solicitudes que se originan dentro de su propia intranet. En las secciones siguientes se describe cómo configurar IP fiables de Azure Multi-Factor Authentication con usuarios federados y omitir la verificación en dos pasos cuando una solicitud se origina en una intranet de usuarios federados. Esto se consigue configurando AD FS para usar un paso a través o filtrar una plantilla de notificación entrante con el tipo de notificación dentro de la red corporativa.

En este ejemplo se utiliza Office 365 para las relaciones de confianza para usuario autenticado .

### <a name="configure-the-ad-fs-claims-rules"></a>Configurar las reglas de notificaciones de AD FS

Utilice el procedimiento siguiente para configurar las notificaciones de AD FS. Cree dos reglas de notificaciones, una para el tipo de notificación dentro de la red corporativa y otra adicional para mantener a nuestros usuarios con la sesión iniciada.

1. Abra Administración de AD FS.
2. A la izquierda, seleccione **Relaciones de confianza para usuario autenticado**.
3. Haga doble clic en **plataforma de identidad de Microsoft Office 365** y seleccione **editar reglas de notificación... ** 
    ![Consola de AD FS: editar reglas de notificación](./media/howto-mfa-adfs/trustedip1.png)
4. En reglas de transformación de emisión, haga clic en **Agregar regla.** 
    ![Agregar una regla de notificación](./media/howto-mfa-adfs/trustedip2.png)
5. En el Asistente para agregar regla de notificaciones de transformación, seleccione **Pasar por una notificación entrante o filtrarla** en la lista desplegable y haga clic en **Siguiente**.
   ![Asistente para agregar reglas de notificación de transformación](./media/howto-mfa-adfs/trustedip3.png)
6. En el cuadro situado junto al nombre de la regla de notificación, asigne un nombre a la regla. Por ejemplo:  InsideCorpNet.
7. En la lista desplegable, junto a Tipo de notificación entrante, seleccione **Dentro de la red corporativa**.
   ![Notificación de adición dentro de la red corporativa](./media/howto-mfa-adfs/trustedip4.png)
8. Haga clic en **Finalizar**
9. En Reglas de transformación de emisión, haga clic en **Agregar regla**.
10. En el Asistente para agregar regla de notificaciones de transformación, seleccione **Enviar notificaciones mediante regla personalizada** en la lista desplegable y haga clic en **Siguiente**.
11. En el cuadro situado bajo el nombre de la regla de notificación: escriba *Mantener a los usuarios con la sesión iniciada*.
12. En el cuadro de regla personalizada, escriba:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Crear una notificación personalizada para que los usuarios inicien sesión](./media/howto-mfa-adfs/trustedip5.png)
13. Haga clic en **Finalizar**
14. Haga clic en **Aplicar**.
15. Haga clic en **Aceptar**.
16. Cierre Administración de AD FS.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Configuración de las IP de confianza de Azure Multi-Factor Authentication con usuarios federados

Ahora que las notificaciones están listas, podemos configurar direcciones IP de confianza.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Azure Active Directory** > **Acceso condicional** > **Ubicaciones con nombre**.
3. Desde la hoja **Acceso condicional: ubicaciones con nombre**, seleccione **Configurar direcciones IP de confianza de MFA**

   ![Acceso condicional de Azure AD, ubicaciones con nombre, Configurar direcciones IP de confianza de MFA](./media/howto-mfa-adfs/trustedip6.png)

4. En la página Configuración del servicio, en **direcciones IP de confianza**, seleccione **Omitir autenticación multifactor para solicitudes de usuarios federados en mi intranet**.  
5. Haga clic en **guardar**.

¡Ya está! En este punto, los usuarios federados de Office 365 solo deberán usar MFA cuando una notificación se origine fuera de la intranet corporativa.

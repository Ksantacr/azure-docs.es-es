---
title: 'Tutorial: Configuración de Asana para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario para Asana.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10d5ceedfd1f0fad0c6f03feba2150968aecf02b
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964225"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Tutorial: Configuración de Asana para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es explicar los pasos que debe realizar en Asana y Azure Active Directory (Azure AD) para aprovisionar y desaprovisionar automáticamente las cuentas de usuario de Azure AD a Asana.

## <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure AD
* Un inquilino de Asana con el plan [Enterprise](https://www.asana.com/pricing) o uno superior habilitado
* Una cuenta de usuario de Asana con permisos de administrador

> [!NOTE]
> La integración del aprovisionamiento de Azure AD depende de la [API de Asana](https://asana.com/developers/api-reference/users), que está disponible para Asana.

## <a name="assign-users-to-asana"></a>Asignación de usuarios a Asana

Azure AD usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios asignados a una aplicación en Azure AD.

Antes de configurar y habilitar el servicio de aprovisionamiento, tiene que decidir qué usuarios de Azure AD necesitan acceso a la aplicación Asana. A continuación, puede asignar estos usuarios a la aplicación Asana siguiendo estas instrucciones:

[Asignación de un usuario a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Sugerencias importantes para asignar usuarios a Asana

Se recomienda asignar un único usuario de Azure AD a Asana para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios.

## <a name="configure-user-provisioning-to-asana"></a>Configuración del aprovisionamiento de usuarios para Asana

Esta sección le guía en el proceso de conexión de Azure AD a la API de aprovisionamiento de cuentas de usuario de Asana. También configurará el servicio de aprovisionamiento para crear, actualizar y deshabilitar las cuentas de usuario asignadas en Asana en función de las asignaciones de usuarios en Azure AD.

> [!TIP]
> Para habilitar el inicio de sesión único basado en SAML para Asana, siga las instrucciones proporcionadas en [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Para configurar el aprovisionamiento automático de cuentas de usuario para Asana en Azure AD, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory**  > **Aplicaciones empresariales** > **Todas las aplicaciones**.

1. Si ya ha configurado Asana para el inicio de sesión único, busque la instancia de Asana mediante el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **Asana** en la galería de aplicaciones. Seleccione **Asana** en los resultados de búsqueda y agrégalo a la lista de aplicaciones.

1. Seleccione la instancia de Asana y, después, la pestaña **Aprovisionamiento**.

1. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Asana](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

1. En la sección **Credenciales de administrador**, siga estas instrucciones para generar el token y escríbalo en el cuadro de texto **Token secreto**:

     a. Inicie sesión en [Asana](https://app.asana.com) mediante la cuenta de administrador.

    b. Seleccione la foto de perfil de la barra superior y seleccione la configuración del nombre de organización actual.

    c. Vaya a la pestaña **Service Accounts** (Cuentas de servicio).

    d. Seleccione **Add Service Account** (Agregar cuenta de servicio).

    e. Actualice los valores de **Name** (Nombre) y **About** (Acerca de) y la foto de perfil, según sea necesario. Copie el token en **Token** y selecciónelo en **Save Changes** (Guardar cambios).

1. En Azure Portal, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación Asana. Si se produce un error de conexión, asegúrese de que su cuenta de Asana tenga permisos de administrador y vuelva a intentar el paso **Probar conexión**.

1. Escriba la dirección de correo electrónico de una persona o grupo que quiera que reciba las notificaciones de error de aprovisionamiento en **Correo electrónico de notificación**. Active la casilla situada debajo.

1. Seleccione **Guardar**.

1. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Box** (Sincronizar usuarios de Azure Active Directory con Asana).

1. En la sección **Asignaciones de atributos**, revise los atributos de usuario que se van a sincronizar entre Azure AD y Asana. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Asana con el objetivo de realizar operaciones de actualización. Para confirmar los cambios, seleccione **Guardar**. Para más información, consulte [Personalización de asignaciones de atributos de aprovisionamiento de usuarios](../manage-apps/customize-application-attributes.md).

1. Para habilitar el servicio de aprovisionamiento de Azure AD para Asana, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

1. Seleccione **Guardar**.

Ahora, se inicia la sincronización inicial para todos los usuarios asignados a Asana en la sección **Usuarios**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si se está ejecutando el servicio. Use la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos a los registros de actividad de aprovisionamiento. En los registros de auditoría se describen todas las acciones realizadas por el servicio de aprovisionamiento en la aplicación Asana.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del inicio de sesión único](asana-tutorial.md)

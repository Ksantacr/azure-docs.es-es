---
title: 'Tutorial: Integración de Azure Active Directory con OpsGenie | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y OpsGenie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 98e4d1870c57c9718e2d4293157b21ead8ea44e1
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987489"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Tutorial: Integración de Azure Active Directory con OpsGenie

En este tutorial, aprenderá a integrar Pluralsight con Azure Active Directory (Azure AD).
La integración de OpsGenie con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a OpsGenie.
* Puede permitir que los usuarios inicien sesión automáticamente en OpsGenie (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con OpsGenie, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para inicio de sesión único en OpsGenie

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* OpsGenie admite el inicio de sesión único iniciado por **SP**

## <a name="adding-opsgenie-from-the-gallery"></a>Adición de OpsGenie desde la galería

Para configurar la integración de OpsGenie en Azure AD, deberá agregar OpsGenie desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar OpsGenie desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **OpsGenie**, seleccione **OpsGenie** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![OpsGenie en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con OpsGenie utilizando un usuario de prueba llamado **B. Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de OpsGenie.

Para configurar y probar el inicio de sesión único de Azure AD con OpsGenie, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de OpsGenie](#configure-opsgenie-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B. Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que B. Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de OpsGenie](#create-opsgenie-test-user)** , para tener un homólogo de B. Simon en OpsGenie que esté vinculado a la representación del usuario de Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con OpsGenie, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **OpsGenie**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de OpsGenie](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://app.opsgenie.com/auth/login`

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

6. En la sección **Configurar OpsGenie**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-opsgenie-single-sign-on"></a>Configuración del inicio de sesión único de OpsGenie

1. Abra otra instancia del explorador y después inicie sesión en OpsGenie como administrador.

2. Haga clic en **Configuración** y después en la pestaña **Inicio de sesión único**.
   
    ![Inicio de sesión único de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

3. Para habilitar SSO, seleccione **Habilitado**.
   
    ![Configuración de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

4. En la sección **Proveedor**, haga clic en la pestaña **Azure Active Directory**.
   
    ![Configuración de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

5. En la página de diálogo de Azure Active Directory, realice los siguientes pasos:
   
    ![Configuración de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
     a. En el cuadro de texto **SAML 2.0 Endpoint** (Punto de conexión SAML 2.0), pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.
    
    b. En el cuadro de texto **URL de metadatos**, pegue el valor de **Dirección URL de metadatos de federación de la aplicación** que copió en Azure Portal.
    
    c. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba llamado "B. Simon" en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **B. Simon**.
  
    b. En el campo **Nombre de usuario**, escriba **bsimon@yourcompanydomain.extension**  
    Por ejemplo: BSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B. Simon acceda a OpsGenie utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **OpsGenie**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **OpsGenie**.

    ![Vínculo a OpsGenie en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B. Simon** en la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-opsgenie-test-user"></a>Creación de un usuario de prueba de OpsGenie

El objetivo de esta sección es crear un usuario de prueba llamado B. Simon en OpsGenie. 

1. En una ventana del explorador web, inicie sesión en el inquilino de OpsGenie como administrador.

2. Vaya a la lista Usuarios haciendo clic en **Usuarios** en el panel izquierdo.
   
    ![Configuración de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. Haga clic en **Agregar usuario**.

4. En el cuadro de diálogo **Agregar usuario** , realice los pasos siguientes:
   
    ![Configuración de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
     a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de B. Simon en Azure Active Directory.
   
    b. En el cuadro de texto **Nombre completo**, escriba **B. Simon**.
   
    c. Haga clic en **Save**(Guardar). 

>[!NOTE]
>B. Simon recibirá un correo electrónico con instrucciones para configurar su perfil.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de OpsGenie en el panel de acceso y debería iniciar sesión automáticamente en la versión de OpsGenie para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


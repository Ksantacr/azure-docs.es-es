---
title: 'Tutorial: Integración de Azure Active Directory con SAML SSO for Bitbucket by resolution GmbH | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAML SSO for Bitbucket by resolution GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: eaa00690cab2cf8bc1ef7c1c3ab279a9f59fcabf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65858621"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Tutorial: Integración de Azure Active Directory con SAML SSO for Bitbucket by resolution GmbH

En este tutorial, aprenderá a integrar SAML SSO for Bitbucket by resolution GmbH con Azure Active Directory (Azure AD).
La integración de SAML SSO for Bitbucket by resolution GmbH con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a SAML SSO for Bitbucket by resolution GmbH.
* Puede permitir que los usuarios inicien sesión automáticamente en SAML SSO for Bitbucket by resolution GmbH (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SAML SSO for Bitbucket by resolution GmbH, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en SAML SSO for Bitbucket by resolution GmbH

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SAML SSO for Bitbucket by resolution GmbH admite el inicio de sesión único iniciado por **SP e IDP**
* SAML SSO for Bitbucket by resolution GmbH admite el aprovisionamiento de usuarios **Just In Time**


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Adición de SAML SSO for Bitbucket by resolution GmbH desde la galería

Para configurar la integración de SAML SSO for Bitbucket by resolution GmbH en Azure AD, debe agregar esta aplicación desde la galería a la lista de aplicaciones administradas de SaaS.

**Para agregar SAML SSO for Bitbucket by resolution GmbH desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **SAML SSO for Bitbucket by resolution GmbH**, seleccione **SAML SSO for Bitbucket by resolution GmbH** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

     ![SAML SSO for Bitbucket by resolution GmbH en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con SAML SSO for Bitbucket by resolution GmbH con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SAML SSO for Bitbucket by resolution GmbH.

Para configurar y probar el inicio de sesión único de Azure AD con SAML SSO for Bitbucket by resolution GmbH, se deben completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SAML SSO for Bitbucket by resolution GmbH](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)**: para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de SAML SSO for Bitbucket by resolution GmbH](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**: para tener un homólogo de Britta Simon en SAML SSO for Bitbucket by resolution GmbH que esté vinculado a la representación de este usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SAML SSO for Bitbucket by resolution GmbH, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **SAML SSO for Bitbucket by resolution GmbH**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Dominio y direcciones URL de SAML SSO for Bitbucket by resolution GmbH: información de inicio de sesión](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<server-base-url>/plugins/servlet/samlsso`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Dominio y direcciones URL de SAML SSO for Bitbucket by resolution GmbH: información de inicio de sesión](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de SAML SSO for Bitbucket by resolution GmbH](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>Configuración del inicio de sesión único de SAML SSO for Bitbucket by resolution GmbH

1. Inicie sesión en el sitio de empresa de SAML SSO for Bitbucket by resolution GmbH como administrador.

2. En el lado derecho de la barra de herramientas principal, haga clic en **Settings** (Configuración).

3. Vaya a la sección ACCOUNTS (CUENTAS) y haga clic en **SAML SingleSignOn** (Inicio de sesión único de SAML) en la barra de menús.

    ![El inicio de sesión único de SAML](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. En la página **SAML SIngleSignOn Plugin Configuration** (Configuración de complementos de inicio de sesión único de SAML), haga clic en **Add idp** (Agregar IdP). 

    ![Add idp (Agregar IdP)](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. En la página **Choose your SAML Identity Provider** (Elegir el proveedor de identidades SAML), siga estos pasos:

    ![El proveedor de identidades](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

     a. Para **Idp Type** (Tipo de IdP), elija **AZURE AD**.

    b. En el cuadro de texto **Name** (Nombre), escriba un nombre.

    c. En el cuadro de texto **Description** (Descripción), escriba la descripción.

    d. Haga clic en **Next**.

6. En la página **Identity provider configuration** (Configuración del proveedor de identidades), haga clic en el botón **Next** (Siguiente).

    ![La configuración de identidad](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  En la página **Import SAML Idp Metadata** (Importar metadatos de IdP de SAML), haga clic en **Load File** (Cargar archivo) para cargar el archivo **METADATA XML** que ha descargado de Azure Portal.

    ![Los metadatos de IdP](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. Haga clic en **Next**.

9. Haga clic en **Save settings** (Guardar configuración).

    ![La operación de guardar](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure, para lo cual le concederá acceso a SAML SSO for Bitbucket by resolution GmbH.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **SAML SSO for Bitbucket by resolution GmbH**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **SAML SSO for Bitbucket by resolution GmbH**.

    ![El vínculo de SAML SSO for Bitbucket by resolution GmbH en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Creación de un usuario de prueba de SAML SSO for Bitbucket by resolution GmbH

El objetivo de esta sección es crear un usuario llamado Britta Simon en SAML SSO for Bitbucket by resolution GmbH. SAML SSO for Bitbucket by resolution GmbH admite aprovisionamiento Just-In-Time y creación manual de usuarios, según sus necesidades. Para más información póngase en contacto con el [equipo de atención al cliente de SAML SSO for Bitbucket by resolution GmbH](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SAML SSO for Bitbucket by resolution GmbH en el Panel de acceso, debería iniciar sesión automáticamente en la solución SAML SSO for Bitbucket by resolution GmbH para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


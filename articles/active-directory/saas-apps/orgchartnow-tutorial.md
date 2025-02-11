---
title: 'Tutorial: Integración de Azure Active Directory con OrgChart Now | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y OrgChart Now.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: cc2bbd0c1220a37de640bde6294eb096b25e5398
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65870550"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Tutorial: Integración de Azure Active Directory con OrgChart Now

En este tutorial, aprenderá a integrar OrgChart Now con Azure Active Directory (Azure AD).
La integración de OrgChart Now con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a OrgChart Now.
* Puede permitir que los usuarios inicien sesión automáticamente en OrgChart Now (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con OrgChart Now, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en OrgChart Now

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* OrgChart Now admite el inicio de sesión único iniciado por **SP** e **IDP**

## <a name="adding-orgchart-now-from-the-gallery"></a>Adición de OrgChart Now desde la galería

Para configurar la integración de OrgChart Now en Azure AD, deberá agregar OrgChart Now de la galería a la lista de aplicaciones SaaS administradas.

**Para agregar OrgChart Now desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **OrgChart Now**, seleccione **OrgChart Now** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![OrgChart Now en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con OrgChart Now con un usuario de prueba denominado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de OrgChart Now.

Para configurar y probar el inicio de sesión único de Azure AD con OrgChart Now, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en OrgChart Now](#configure-orgchart-now-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba en OrgChart Now](#create-orgchart-now-test-user)**: para tener un homólogo de Britta Simon en OrgChart Now vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con OrgChart Now, siga estos pasos:

1. En la página de integración de la aplicación [OrgChart Now](https://portal.azure.com/) de **Azure Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si quiere configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de OrgChart Now](common/idp-identifier.png)

    En el cuadro de texto **Identificador**, escriba una dirección URL: `https://sso2.orgchartnow.com`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![imagen](common/both-preintegrated-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`

    > [!NOTE]
    > `<YourEntityID>` es el **identificador de Azure AD** que se copió de la sección **Set up OrgChart Now** (Configurar OrgChart Now) que se describe más adelante en el tutorial.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Set up OrgChart Now** (Configurar OrgChart Now), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-orgchart-now-single-sign-on"></a>Configuración del inicio de sesión único en OrgChart Now

Para configurar el inicio de sesión único en **OrgChart Now**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de OrgChart Now](mailto:ocnsupport@officeworksoftware.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a OrgChart Now.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **OrgChart Now**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **OrgChart Now**.

    ![El vínculo de OrgChart Now en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-orgchart-now-test-user"></a>Creación del usuario de prueba en OrgChart Now

Para permitir que los usuarios de Azure AD inicien sesión en OrgChart Now, deben aprovisionarse en OrgChart Now. 

1. OrgChart Now admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. Al intentar acceder a OrgChart Now, se crea un nuevo usuario, en caso de que no exista. La característica de aprovisionamiento de usuarios Just-in-Time solo crea un usuario de **solo lectura** cuando la solicitud de inicio de sesión único procede de un IDP reconocido y el correo electrónico de la aserción de SAML no se encuentra en la lista de usuarios. Para esta característica de aprovisionamiento automático, es necesario crear un grupo de acceso llamado **General** en OrgChart Now. Siga los pasos siguientes para crear un grupo de acceso:

     a. Después de hacer clic en el **engranaje** en la esquina superior derecha de la interfaz de usuario, vaya a la opción **Manage Groups** (Administrar grupos).

    ![Grupos de OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_manage.png)    

    b. Seleccione el icono **Add** (Agregar) y asigne al grupo el nombre **General**; a continuación, haga clic en **OK** (Aceptar). 

    ![OrgChart Now: agregar](./media/orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Seleccione las carpetas a las que desea que tengan acceso los usuarios generales o de solo lectura:

    ![Carpetas de OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Bloquee** las carpetas para que solo los usuarios administrativos pueden modificarlas. Después, presione **OK** (Aceptar).

    ![OrgChart Now: bloquear](./media/orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Para crear usuarios **administradores** y usuarios de **lectura/escritura**, debe crear manualmente un usuario con el fin de obtener acceso a su nivel de privilegios mediante inicio de sesión único. Para aprovisionar una cuenta de usuario, realice estos pasos:

     a. Inicie sesión ahora en OrgChart Now como administrador de seguridad.

    b.  Haga clic en **Settings** (Configuración) en la esquina superior derecha y, luego, vaya a **Manage Users** (Administrar usuarios).

    ![Configuración de OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Haga clic en **Add** (Agregar) y realice los siguientes pasos:

    ![OrgChart Now: administrar](./media/orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * En el cuadro de texto **User ID** (Id. de usuario), escriba el identificador de usuario, por ejemplo, **brittasimon\@contoso.com**.

    * En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **brittasimon\@contoso.com**.

    * Haga clic en **Agregar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de OrgChart Now en el panel de acceso, debería iniciar sesión automáticamente en la versión de OrgChart Now para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


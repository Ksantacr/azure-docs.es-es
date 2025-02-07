---
title: 'Tutorial: Integración de Azure Active Directory con IdeaScale | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e IdeaScale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88d181c2e761679d7f52208b2086404411bc2012
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898173"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Tutorial: Integración de Azure Active Directory con IdeaScale

En este tutorial, aprenderá a integrar IdeaScale con Azure Active Directory (Azure AD).
La integración de IdeaScale con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a IdeaScale.
* Puede habilitar a los usuarios para que inicien sesión automáticamente en IdeaScale (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con IdeaScale, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en IdeaScale

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* IdeaScale admite el inicio de sesión único iniciado por **SP**

## <a name="adding-ideascale-from-the-gallery"></a>Adición de IdeaScale desde la galería

Para configurar la integración de IdeaScale en Azure AD, deberá agregar IdeaScale desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar IdeaScale desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **IdeaScale**, seleccione **IdeaScale** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![IdeaScale en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección configurará y probará el inicio de sesión único de Azure AD con IdeaScale con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de IdeaScale.

Para configurar y probar el inicio de sesión único de Azure AD con IdeaScale, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de IdeaScale](#configure-ideascale-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de IdeaScale](#create-ideascale-test-user)**: para tener un homólogo de Britta Simon en IdeaScale vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con IdeaScale, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **IdeaScale**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de IdeaScale](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.ideascale.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón:
    
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de clientes de IdeaScale](https://support.ideascale.com/). También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Configurar IdeaScale**, copie las direcciones URL adecuadas según sus requisitos.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-ideascale-single-sign-on"></a>Configuración del inicio de sesión único de IdeaScale

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de IdeaScale como administrador.

2. Vaya a **Configuración de la comunidad**.

    ![Configuración de la Comunidad](./media/ideascale-tutorial/ic790847.png "Configuración de la Comunidad")

3. Vaya a **Security \> Single Signon Settings** (Seguridad > Configuración de inicio de sesión único).

    ![Configuración de inicio de sesión único](./media/ideascale-tutorial/ic790848.png "Configuración de inicio de sesión único")

4. En **Single-Signon Type** (Tipo de inicio de sesión único), seleccione **SAML 2.0**.

    ![Tipo de inicio de sesión único](./media/ideascale-tutorial/ic790849.png "Tipo de inicio de sesión único")

5. En el cuadro de diálogo **Configuración de inicio de sesión único** , siga estos pasos:

    ![Configuración de inicio de sesión único](./media/ideascale-tutorial/ic790850.png "Configuración de inicio de sesión único")

     a. En el cuadro de texto **SAML IdP Entity ID** (Identificador de la entidad de IdP de SAML), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    b. Abra el archivo de metadatos descargado de Azure Portal en el Bloc de notas, copie el contenido y péguelo en el cuadro de texto **IdP Metadata** (Metadatos del IdP de SAML).

    c. En el cuadro de texto **Logout Success URL** (Dirección URL de cierre de sesión correcto), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    d. Haga clic en **Guardar cambios**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a IdeaScale.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** e **IdeaScale**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **IdeaScale**.

    ![Vínculo a IdeaScale en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-ideascale-test-user"></a>Creación de un usuario de prueba de IdeaScale

Para permitir que los usuarios de Azure AD inicien sesión en IdeaScale, deben aprovisionarse en IdeaScale. En el caso de IdeaScale, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **IdeaScale** como administrador.

2. Vaya a **Configuración de la comunidad**.

    ![Configuración de la Comunidad](./media/ideascale-tutorial/ic790847.png "Configuración de la Comunidad")

3. Vaya a **Basic Settings \> Member Management** (Configuración básica > Administración de miembros).

4. Haga clic en **Agregar miembro**.

    ![Administración de miembros](./media/ideascale-tutorial/ic790852.png "Administración de miembros")

5. En la sección Add New Member (Agregar nuevo miembro), lleve a cabo estos pasos:

    ![Agregar nuevo miembro](./media/ideascale-tutorial/ic790853.png "Agregar nuevo miembro")

     a. En el cuadro de texto **Direcciones de correo electrónico**, escriba la dirección de correo electrónico de la cuenta de Azure AD válida que quiera aprovisionar.

    b. Haga clic en **Guardar cambios**.

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibe un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de IdeaScale que proporcione IdeaScale para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de IdeaScale en el panel de acceso, debería iniciar sesión automáticamente en la instancia de IdeaScale para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


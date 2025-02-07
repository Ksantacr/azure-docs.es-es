---
title: 'Tutorial: Integración de Azure Active Directory con Egnyte | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 2/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ff1718211fa298e4daee694795a0b2438033fe2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65861950"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Tutorial: Integración de Azure Active Directory con Egnyte

En este tutorial, aprenderá a integrar Egnyte con Azure Active Directory (Azure AD).
Integrar Egnyte con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Egnyte.
* Puede permitir que los usuarios inicien sesión automáticamente en Egnyte (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Egnyte, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Egnyte

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Egnyte admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-egnyte-from-the-gallery"></a>Adición de Egnyte desde la galería

Para configurar la integración de Egnyte en Azure AD, deberá agregar Egnyte desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Egnyte desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Egnyte**, seleccione **Egnyte** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Egnyte en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Egnyte con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Egnyte.

Para configurar y probar el inicio de sesión único de Azure AD con Egnyte, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Egnyte](#configure-egnyte-single-sign-on)**: para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Egnyte](#create-egnyte-test-user)**: para tener un homólogo de Britta Simon en Egnyte que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Egnyte, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Egnyte**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Egnyte](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.egnyte.com`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Egnyte](https://www.egnyte.com/corp/contact_egnyte.html) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

5. En la sección **Set up Egnyte** (Configurar Egnyte), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-egnyte-single-sign-on"></a>Configuración del inicio de sesión único en Egnyte

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Egnyte como administrador.

2. Haga clic en **Configuración**.
   
    ![Configuración](./media/egnyte-tutorial/ic787819.png "Configuración")

3. En el menú, haga clic en **Configuración**.

    ![Configuración](./media/egnyte-tutorial/ic787820.png "Configuración")

4. Haga clic en la pestaña **Configuración** y luego haga clic en **Seguridad**.

    ![Seguridad](./media/egnyte-tutorial/ic787821.png "Seguridad")

5. En la sección **Autenticación de inicio de sesión único** , siga estos pasos:

    ![Autenticación de inicio de sesión único](./media/egnyte-tutorial/ic787822.png "Autenticación de inicio de sesión único")   
    
     a. En **Autenticación de inicio de sesión único**, seleccione **SAML 2.0**.
   
    b. En **Proveedor de identidades**, seleccione **AzureAD**.
   
    c. Pegue la **URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **URL de inicio de sesión del proveedor de identidades**.
   
    d. Pegue el **Identificador de Azure AD** que copió de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).
      
    e. Abra el certificado codificado en base 64 descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades**.
   
    f. En **Asignación de usuario predeterminada**, seleccione **Dirección de correo electrónico**.
   
    g. En **Usar valor de emisor específico del dominio**, seleccione **Deshabilitado**.
   
    h. Haga clic en **Save**(Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Egnyte.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **Egnyte**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Egnyte**.

    ![Vínculo a Configurar Egnyte en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-egnyte-test-user"></a>Creación de usuario de prueba de Egnyte

Para permitir que los usuarios de Azure AD inicien sesión en Egnyte, deben aprovisionarse en Egnyte. En el caso de Egnyte, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de **Egnyte** como administrador.

2. Vaya a **Configuración \> Usuarios y grupos**.

3. Haga clic en **Agregar nuevo usuario**y luego seleccione el tipo de usuario que quiera agregar.
   
    ![Usuarios](./media/egnyte-tutorial/ic787824.png "Usuarios")

4. En la sección **Nuevo usuario avanzado**, lleve a cabo estos pasos:
    
    ![Nuevo usuario estándar](./media/egnyte-tutorial/ic787825.png "Nuevo usuario estándar")   

     a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon\@contoso.com**.

    b. En el cuadro de texto **Nombre de usuario**, escriba el nombre de un usuario; por ejemplo, **Britta Simon**.

    c. Seleccione **Inicio de sesión único** como **Tipo de autenticación**.
   
    d. Haga clic en **Save**(Guardar).
    
    >[!NOTE]
    >El titular de la cuenta de Azure Active Directory recibirá una notificación por correo electrónico.
    >

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Egnyte que proporcione Egnyte para aprovisionar cuentas de usuario de AAD.
>

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Egnyte en el Panel de acceso, debería iniciar sesión automáticamente en la versión de Egnyte para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


---
title: 'Tutorial: Integración de Azure Active Directory con BambooHR | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y BambooHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a054274da0c83fa5f079e05a1c648c1fc693fd7
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900331"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Tutorial: Integración de Azure Active Directory con BambooHR

En este tutorial, obtendrá información sobre cómo integrar BambooHR con Azure Active Directory (Azure AD).
La integración de BambooHR con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a BambooHR.
* Puede permitir que los usuarios inicien sesión automáticamente en BambooHR (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con BambooHR, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en BambooHR

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* BambooHR admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-bamboohr-from-the-gallery"></a>Incorporación de BambooHR desde la galería

Para configurar la integración de BambooHR en Azure AD, deberá agregar BambooHR desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar BambooHR desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **BambooHR**, seleccione **BambooHR** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![BambooHR en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con BambooHR con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de BambooHR.

Para configurar y probar el inicio de sesión único de Azure AD con BambooHR, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de BambooHR](#configure-bamboohr-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de BambooHR](#create-bamboohr-test-user)**: para tener un homólogo de Britta Simon en BambooHR que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con BambooHR, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **BambooHR**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de BambooHR](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company>.bamboohr.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `BambooHR-SAML`

    > [!NOTE]
    > El valor de la **dirección URL de inicio de sesión** no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de cliente de BambooHR](https://www.bamboohr.com/contact.php) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up BambooHR** (Configurar BambooHR), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-bamboohr-single-sign-on"></a>Configuración de inicio de sesión único de BambooHR

1. En otra ventana, inicie sesión como administrador en el sitio de la compañía de BambooHR.

2. En la página principal, haga lo siguiente:
   
    ![Página de inicio de sesión único de BambooHR](./media/bamboo-hr-tutorial/ic796691.png "Inicio de sesión único")   

     a. Seleccione **Aplicaciones**.
   
    b. En el panel **Aplicaciones**, seleccione **Single Sign-On** (Inicio de sesión único).
   
    c. Seleccione **SAML Single Sign-On** (Inicio de sesión único de SAML).

3. En el panel **SAML Single Sign-On** (Inicio de sesión único de SAML), haga lo siguiente:
   
    ![El panel SAML Single Sign-O](./media/bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
     a. En el cuadro **SSO Login URL** (URL de inicio de sesión de SSO), pegue la **dirección URL de inicio de sesión** que copió de Azure Portal en el paso 6.
      
    b. En el Bloc de notas, abra el certificado codificado en Base 64 que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509).
   
    c. Seleccione **Guardar**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a BambooHR.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **BambooHR**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **BambooHR**.

    ![Vínculo a BambooHR en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-bamboohr-test-user"></a>Creación de un usuario de prueba de BambooHR

Para permitir que los usuarios de Azure AD inicien sesión en BambooHR, configúrelos manualmente en BambooHR haciendo lo siguiente:

1. Inicie sesión en su sitio de **BambooHR** como administrador.

2. En la barra de herramientas en la parte superior, seleccione **Configuración**.
   
    ![El botón Configuración](./media/bamboo-hr-tutorial/IC796694.png "Configuración")

3. Seleccione **Información general**.

4. En el panel izquierdo, seleccione **Seguridad** > **Usuarios**.

5. Escriba el nombre de usuario, la contraseña y la dirección de correo electrónico de la cuenta válida de Azure AD que desee configurar.

6. Seleccione **Guardar**.
        
>[!NOTE]
>Para configurar cuentas de usuario de Azure AD, también puede utilizar las herramientas de creación de cuentas de usuario de BambooHR o las API.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de BambooHR en el panel de acceso, debería iniciar sesión automáticamente en la versión de BambooHR para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


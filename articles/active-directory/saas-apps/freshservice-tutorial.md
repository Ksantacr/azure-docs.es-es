---
title: 'Tutorial: Integración de Azure Active Directory con Freshservice | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Freshservice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c87b23c338788804be22639f73acfb61ce8d6973
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989413"
---
# <a name="tutorial-integrate-freshservice-with-azure-active-directory"></a>Tutorial: Integración de Freshservice con Azure Active Directory

En este tutorial, aprenderá a integrar Freshservice con Azure Active Directory (Azure AD). Cuando integre Freshservice con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Freshservice.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Freshservice con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para el inicio de sesión único en FreshService (SSO).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Freshservice admite el inicio de sesión único iniciado por **SP**

## <a name="adding-freshservice-from-the-gallery"></a>Agregar Freshservice desde la galería

Para configurar la integración de Freshservice en Azure AD, deberá agregar Freshservice desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Freshservice** en el cuadro de búsqueda.
1. Seleccione **Freshservice** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Freshservice utilizando un usuario de prueba llamado **Britta Simon**. Para que el SSO funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Freshservice.

Para configurar y probar el inicio de sesión único de Azure AD con Freshservice, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único de Freshservice](#configure-freshservice-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Freshservice](#create-freshservice-test-user)** : para tener un homólogo de Britta Simon en Freshservice que esté vinculado a la representación de Azure AD de usuario.
6. **[Comprobación del inicio de sesión único](#test-sso)** , para verificar que la configuración funciona correctamente.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Freshservice**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configuración básica de SAML**, especifique los valores de los siguientes campos:

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<democompany>.freshservice.com`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<democompany>.freshservice.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de clientes de Freshservice](https://support.freshservice.com/). También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

1. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

1. En la sección **Set up Freshservice** (Configurar Freshservice), copie las direcciones URL adecuada según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-freshservice-sso"></a>Configuración del inicio de sesión único de Freshservice

1. Para automatizar la configuración en Freshservice, debe instalar la **extensión del explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Setup Freshservice** (Configurar Freshservice) para acceder a la aplicación Freshservice. Una vez allí, especifique las credenciales del administrador para iniciar sesión en Freshservice. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 6.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar Freshservice manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Freshservice como administrador y siga estos pasos:

4. En el menú de la parte superior, haga clic en **Administrador**.

    ![Administración](./media/freshservice-tutorial/ic790814.png "Administración")

5. En el **Portal del cliente**, haga clic en **Seguridad**.

    ![Seguridad](./media/freshservice-tutorial/ic790815.png "Seguridad")

6. En la sección **Seguridad** , realice estos pasos:

    ![Inicio de sesión único](./media/freshservice-tutorial/ic790816.png "Inicio de sesión único")

     a. Cambie a **Inicio de sesión único**.

    b. Seleccione **Inicio de sesión único de SAML**.

    c. En el cuadro de texto **Dirección URL de inicio de sesión de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    d. En el cuadro de texto **URL de cierre de sesión**, pegue el valor de **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal.

    e. En el cuadro de texto **Security Certificate Fingerprint** (Huella digital de certificado de seguridad), pegue el valor de **HUELLA DIGITAL** del certificado que haya copiado de Azure Portal.

    f. Haga clic en **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `Britta Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Freshservice.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Freshservice**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En la lista de usuarios del cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-freshservice-test-user"></a>Creación de un usuario de prueba de Freshservice

Para permitir que los usuarios de Azure AD inicien sesión en FreshService, deben aprovisionarse en FreshService. En el caso de FreshService, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de **FreshService** como administrador.

2. En el menú de la parte superior, haga clic en **Administrador**.

    ![Administración](./media/freshservice-tutorial/ic790814.png "Administración")

3. En la sección **User Management** (Administración de usuarios), haga clic en **Requesters** (Solicitantes).

    ![Solicitantes](./media/freshservice-tutorial/ic790818.png "Solicitantes")

4. Haga clic en **Nuevo solicitante**.

    ![Nuevos solicitantes](./media/freshservice-tutorial/ic790819.png "Nuevos solicitantes")

5. En la sección **New Requester** (Nuevo solicitante), lleve a cabo estos pasos:

    ![Nuevo solicitante](./media/freshservice-tutorial/ic790820.png "Nuevo solicitante")  

     a. Escriba los atributos **First Name** (Nombre) y **Email** (Correo electrónico) de una cuenta de Azure Active Directory válida que desee aprovisionar en los cuadros de texto relacionados.

    b. Haga clic en **Save**(Guardar).

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibe un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active.
    >  

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de FreshService que proporcione FreshService para aprovisionar cuentas de usuario de AAD.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Freshservice en el panel de acceso, debería iniciar sesión automáticamente en la versión de Freshservice para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

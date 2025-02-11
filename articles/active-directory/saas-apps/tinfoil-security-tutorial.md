---
title: 'Tutorial: Integración de Azure Active Directory con TINFOIL SECURITY | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TINFOIL SECURITY.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 135b5719422d1b28a82ac2eda06f76d6dd746800
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813731"
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Tutorial: Integración de Azure Active Directory con TINFOIL SECURITY

En este tutorial, aprenderá a integrar TINFOIL SECURITY con Azure Active Directory (Azure AD).
Integrar TINFOIL SECURITY con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a TINFOIL SECURITY.
* Puede habilitar a los usuarios para que inicien sesión automáticamente en TINFOIL SECURITY (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con TINFOIL SECURITY, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en TINFOIL SECURITY

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* TINFOIL SECURITY admite el inicio de sesión único iniciado por **IDP**.

## <a name="adding-tinfoil-security-from-the-gallery"></a>Adición de TINFOIL SECURITY desde la galería

Para configurar la integración de TINFOIL SECURITY en Azure AD, debe agregar TINFOIL SECURITY desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar TINFOIL SECURITY desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **TINFOIL SECURITY**, seleccione **TINFOIL SECURITY** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![TINFOIL SECURITY en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con TINFOIL SECURITY con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de TINFOIL SECURITY.

Para configurar y probar el inicio de sesión único de Azure AD con TINFOIL SECURITY, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de TINFOIL SECURITY](#configure-tinfoil-security-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de TINFOIL SECURITY](#create-tinfoil-security-test-user)**: para tener un homólogo de Britta Simon en TINFOIL SECURITY que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con TINFOIL SECURITY, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **TINFOIL SECURITY**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de TINFOIL SECURITY](common/preintegrated.png)

5. La aplicación TINFOIL SECURITY espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo  **Atributos de usuario** .

        ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación TINFOIL SECURITY espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Atributo de origen |
    | ------------------- | -------------|
    | accountid | UXXXXXXXXXXXXX |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En el cuadro de texto **Atributo de origen**, pegue el valor del identificador de cuenta que obtendrá más adelante en el tutorial.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

8. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

9. En la sección **Configurar TINFOIL SECURITY**, copie las direcciones URL adecuadas según sus requisitos.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-tinfoil-security-single-sign-on"></a>Configuración del inicio de sesión único de TINFOIL SECURITY

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de TINFOIL SECURITY.

2. En la barra de herramientas de la parte superior, haga clic en el icono de **Mi cuenta**.
   
    ![Panel](./media/tinfoil-security-tutorial/ic798971.png "Panel")

3. Haga clic en **Seguridad**.
   
    ![Seguridad](./media/tinfoil-security-tutorial/ic798972.png "Seguridad")

4. Siga estos pasos en la página de configuración **Inicio de sesión único** :
   
    ![Inicio de sesión único](./media/tinfoil-security-tutorial/ic798973.png "Inicio de sesión único")
   
     a. Seleccione **Habilitar SAML**.
   
    b. Haga clic en **Configuración manual**.
   
    c. En el cuadro de texto **Dirección URL de publicación de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.
   
    d. En el cuadro de texto **SAML Certificate Fingerprint** (Huella digital de certificado de SAML), pegue el valor de **Huella digital** del certificado que haya copiado de la sección **Certificado de firma de SAML**.
  
    e. Copie el valor del **identificador de la cuenta** y péguelo en el cuadro de texto **Valor de atributo** de la sección **Agregar atributo** de Azure Portal.
   
    f. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a TINFOIL SECURITY.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **TINFOIL SECURITY**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **TINFOIL SECURITY**.

    ![Vínculo a TINFOIL SECURITY en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-tinfoil-security-test-user"></a>Creación de un usuario de prueba de TINFOIL SECURITY

Para permitir que los usuarios de Azure AD inicien sesión en TINFOIL SECURITY, deben aprovisionarse en TINFOIL SECURITY. En el caso de TINFOIL SECURITY, el aprovisionamiento es una tarea manual.

**Siga estos pasos para que se aprovisione un usuario:**

1. Si el usuario forma parte de una cuenta de empresa, deberá [ponerse en contacto con el equipo de soporte técnico de TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) para que se cree la cuenta del usuario.

1. Si el usuario es usuario habitual de SaaS de TINFOIL SECURITY, puede agregar un colaborador a cualquiera de sus sitios. Esta acción desencadena un proceso para enviar una invitación al correo electrónico especificado y crear una cuenta de usuario de TINFOIL SECURITY.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de TINFOIL SECURITY que ofrezca TINFOIL SECURITY para aprovisionar cuentas de usuario de Azure AD.
> 

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TINFOIL SECURITY en el panel de acceso, debería iniciar sesión automáticamente en la instancia de TINFOIL SECURITY para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


---
title: 'Autenticación de usuario final: Azure Data Lake Storage Gen1 con Azure Active Directory | Microsoft Docs'
description: Aprenda a lograr la autenticación del usuario final con Azure Data Lake Storage Gen1 mediante Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 4c2b774c304e46f9fc68f3beaf64218e614ecad1
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234051"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Autenticación de usuario final en Azure Data Lake Storage Gen1 con Azure Active Directory
> [!div class="op_single_selector"]
> * [Autenticación de usuario final](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Autenticación entre servicios](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Storage Gen1 usa Azure Active Directory para la autenticación. Antes de crear una aplicación que funcione con Data Lake Storage Gen1 o Azure Data Lake Analytics, debe decidir cómo autenticar la aplicación con Azure Active Directory (Azure AD). Las dos principales opciones disponibles son:

* Autenticación de usuario final (este artículo)
* Autenticación entre servicios (elija esta opción en la lista desplegable anterior)

Con ambas opciones, la aplicación recibe un token de OAuth 2.0 que se adjunta a cada solicitud realizada a Data Lake Storage Gen1 o Azure Data Lake Analytics.

En este artículo se habla de cómo crear una **aplicación nativa de Azure AD para la autenticación de usuario final**. Para obtener instrucciones sobre la configuración de aplicaciones de Azure AD para la autenticación entre servicios, vea [Autenticación entre servicios con Data Lake Storage Gen1 mediante Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción de Azure. Consulte [How to get Azure Free trial for testing Hadoop in HDInsight (Obtención de una versión de prueba gratuita de Azure para probar Hadoop en HDInsight)](https://azure.microsoft.com/pricing/free-trial/).

* El identificador de suscripción. Puede recuperarlo en Azure Portal. Por ejemplo, está disponible en la hoja de la cuenta de Data Lake Storage Gen1.
  
    ![Obtener id. de suscripción](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* El nombre de dominio de Azure AD. Para recuperarlo, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. En la siguiente captura de pantalla, el nombre de dominio es **contoso.onmicrosoft.com** y el GUID entre paréntesis es el identificador del inquilino. 
  
    ![Obtener dominio de AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* El identificador del inquilino de Azure. Para obtener instrucciones sobre cómo recuperar el identificador de inquilino, consulte [Obtención del identificador de inquilino](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

## <a name="end-user-authentication"></a>Autenticación de usuario final
Este mecanismo de autenticación es el enfoque recomendado si desea que un usuario final para iniciar sesión en su aplicación a través de Azure AD. La aplicación puede acceder a los recursos de Azure con el mismo nivel de acceso que el usuario final que ha iniciado sesión. El usuario final tiene que proporcionar sus credenciales periódicamente para que la aplicación conserve el acceso.

El resultado del usuario final inicie sesión en es que la aplicación recibe un token de acceso y un token de actualización. El token de acceso se adjunta a cada solicitud hecha a Data Lake Storage Gen1 o Data Lake Analytics y es válido, de manera predeterminada, durante 1 hora. El token de actualización se puede usar para obtener un nuevo token de acceso y es válido, de manera predeterminada, hasta dos semanas. Puede usar dos enfoques diferentes para el inicio de sesión del usuario final.

### <a name="using-the-oauth-20-pop-up"></a>Uso de la ventana emergente de OAuth 2.0
La aplicación puede desencadenar una ventana emergente de autorización de OAuth 2.0 en la que el usuario final puede escribir sus credenciales. Esta ventana emergente también funciona con el proceso de autenticación en dos fases (2FA) de Azure AD, si fuera necesario. 

> [!NOTE]
> Este método todavía no es compatible con la Biblioteca de autenticación de Active Directory (ADAL) para Python o Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Transmisión directa de credenciales de usuario
Su aplicación puede proporcionar directamente las credenciales de usuario a Azure AD. Este método solo funciona con cuentas de usuario de identificador de organización; no es compatible con cuentas de usuario personales de tipo "Live ID", incluidas las que terminan en @outlook.com o @live.com. Además, este método no es compatible con las cuentas de usuario que requieren la autenticación en dos pasos (2FA) de Azure AD.

### <a name="what-do-i-need-for-this-approach"></a>¿Qué se necesita para este enfoque?
* El nombre de dominio de Azure AD. Este requisito ya aparece en los requisitos previos de este artículo.
* Identificador de inquilino de Azure AD. Este requisito ya aparece en los requisitos previos de este artículo.
* **Aplicación nativa** de Azure AD
* Identificador de la aplicación nativa de Azure AD
* URI de redirección de la aplicación nativa de Azure AD
* Establecimiento de permisos delegados


## <a name="step-1-create-an-active-directory-native-application"></a>Paso 1: Crear una aplicación nativa de Active Directory

Cree y configure una aplicación nativa de Azure AD para la autenticación de usuario final con Data Lake Storage Gen1 mediante Azure Active Directory. Para obtener instrucciones, vea cómo [crear una aplicación de Azure AD](../active-directory/develop/howto-create-service-principal-portal.md).

Al seguir las instrucciones del vínculo, asegúrese de seleccionar **Nativa** como tipo de aplicación, como se muestra en la captura de pantalla siguiente:

![Crear aplicación de web](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Crear aplicación nativa")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Paso 2: Obtener Id. de aplicación y URI de redireccionamiento

Si necesita recuperar el identificador de la aplicación, consulte [Obtener el identificador de la aplicación](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

Para recuperar el URI de redireccionamiento, siga estos pasos.

1. En Azure Portal, seleccione **Azure Active Directory**, haga clic en **Registros de aplicaciones** y después busque la aplicación nativa de Azure AD que acaba de crear y haga clic en ella.

2. En la hoja **Configuración** de la aplicación, haga clic en **URI de redirección**.

    ![Obtener URI de redirección](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Copie el valor mostrado.


## <a name="step-3-set-permissions"></a>Paso 3: Establecer permisos

1. En Azure Portal, seleccione **Azure Active Directory**, haga clic en **Registros de aplicaciones** y después busque la aplicación nativa de Azure AD que acaba de crear y haga clic en ella.

2. En la hoja **Configuración** de la aplicación, haga clic en **Permisos necesarios** y después en **Agregar**.

    ![Identificador de cliente](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. En la hoja **Agregar acceso de API**, haga clic en **Seleccionar una API**, después en **Azure Data Lake** y, por último, en **Seleccionar**.

    ![Identificador de cliente](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  En la hoja **Agregar acceso de API**, haga clic en **Seleccionar permisos**, active la casilla de verificación para conceder **acceso total a Data Lake Store** y después haga clic en **Seleccionar**.

    ![Identificador de cliente](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Haga clic en **Done**(Listo).

5. Repita los dos últimos pasos para conceder permisos también para **Service Management API de Windows Azure**.
   
## <a name="next-steps"></a>Pasos siguientes
En este artículo se ha creado una aplicación nativa de Azure AD y se ha recopilado la información que necesita en las aplicaciones cliente creadas mediante el SDK de .NET, el SDK de Java, la API de REST, etc. Ahora puede continuar en los artículos siguientes, que hablan de cómo usar la aplicación web de Azure AD para autenticarse primero en Data Lake Storage Gen1 y luego realizar otras operaciones en el almacén.

* [Autenticación de usuario final con Data Lake Storage Gen1 mediante el uso del SDK de Java](data-lake-store-end-user-authenticate-java-sdk.md)
* [Autenticación de usuario final con Data Lake Storage Gen1 mediante el uso del SDK de .NET](data-lake-store-end-user-authenticate-net-sdk.md)
* [Autenticación de usuario final con Data Lake Storage Gen1 mediante el uso de Python](data-lake-store-end-user-authenticate-python.md)
* [Autenticación de usuario final con Data Lake Storage Gen1 mediante el uso de la API de REST](data-lake-store-end-user-authenticate-rest-api.md)


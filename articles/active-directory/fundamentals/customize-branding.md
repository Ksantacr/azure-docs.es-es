---
title: 'Incorporación de la personalización de marca en la página de inicio de sesión de la organización: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo agregar la personalización de marca de la organización a la página de inicio de sesión de Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: lizross
ms.reviewer: kexia
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 277e7663c978e64ee1440e14583e884b768b3139
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441643"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-sign-in-page"></a>Incorporación de la personalización de marca en la página de inicio de sesión de Azure Active Directory de la organización
Use el logotipo de la organización y combinaciones de colores personalizadas para proporcionar un aspecto coherente en las páginas de inicio de sesión de Azure Active Directory (Azure AD). Las páginas de inicio de sesión aparecen cuando los usuarios inician sesión en las aplicaciones web de su organización, como Office 365, que usan Azure AD como proveedor de identidades.

>[!Note]
>Para agregar personalización de marca, es necesario usar las ediciones de Azure Active Directory Premium 1, Premium 2 o Basic, o bien tener una licencia de Office 365. Para obtener más información acerca de las ediciones y licencias, consulte [Suscripción a Azure AD Premium](active-directory-get-started-premium.md).<br><br>Las ediciones Azure AD Premium y Basic están disponibles para los clientes de China que utilizan la instancia de Azure Active Directory en todo el mundo. Las ediciones Azure AD Premium y Basic no se admiten actualmente en el servicio de Azure administrado por 21Vianet en China. Para más información, póngase en contacto con nosotros en el [foro de Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customize-your-azure-ad-sign-in-page"></a>Personalización de la página de inicio de sesión de Azure AD
Puede personalizar las páginas de inicio de sesión de Azure AD, que aparecen cuando los usuarios inician sesión en aplicaciones específicas del inquilino de la organización, como [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com), o al pasar una variable de dominio, como [*https://passwordreset.microsoftonline.com/?whr=contoso.com*](https://passwordreset.microsoftonline.com/?whr=contoso.com).

La personalización de marca no aparecerá inmediatamente cuando los usuarios acceder a los sitios, como www\.office.com. En su lugar, el usuario tiene que iniciar sesión para que aparezca la personalización de marca.

> [!NOTE]
> Todos los elementos de personalización de marca son opcionales. Por ejemplo, si especifica un logotipo del banner sin ninguna imagen de fondo, se mostrará en la página de inicio de sesión su logotipo con una imagen de fondo predeterminada del sitio de destino (por ejemplo, Office 365).<br><br>Además, la personalización de marca de la página de inicio de sesión no se incluye en las cuentas Microsoft personales. Si los usuarios o los invitados de la empresa inician sesión con una cuenta Microsoft personal, la página de inicio de sesión no reflejará la personalización de marca de la organización.

### <a name="to-customize-your-branding"></a>Para personalizar su marca
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory**, después, seleccione **Personalización de marca de empresa** y, a continuación, seleccione **Configurar**.

    ![Contoso: página de personalización de marca de empresa con la opción Configurar resaltada](media/customize-branding/company-branding-configure-button.png)

3. En la página **Configurar personalización de marca de empresa**, proporcione parte o la totalidad de la siguiente información.

    >[!Important]
    >Todas las imágenes personalizadas que agregue en esta página tienen restricciones de tamaño de imagen (píxeles) y del posible tamaño de archivo (KB). Debido a estas restricciones, lo más probable es que necesite usar un editor de fotografía para crear imágenes con el tamaño adecuado.

    - **Configuración general**

        ![Página de configuración de personalización de marca de empresa con la configuración general completada](media/customize-branding/configure-company-branding-general-settings.png)

        - **Idioma**. El idioma se establece automáticamente como valor predeterminado y no se puede cambiar.
        
        - **Imagen de fondo de la página de inicio de sesión.** Seleccione un archivo de imagen .png o .jpg para que aparezca como fondo de las páginas de inicio de sesión. 
        
            La imagen no puede tener más de 1920 x 1080 píxeles de tamaño y debe tener un tamaño de archivo inferior a 300 KB.

        - **Logotipo del banner**. Seleccione una versión .png o .jpg del logotipo para que aparezca en la página de inicio de sesión después de que el usuario escriba un nombre de usuario y en la página del portal **Mis aplicaciones**.
            
            La imagen no puede tener más de 36 píxeles de alto ni más de 245 píxeles de ancho. Se recomienda usar una imagen transparente, ya que el fondo podría no coincidir con el fondo del logotipo. También se recomienda no agregar relleno alrededor de la imagen, ya que podría reducir la apariencia del logotipo.

        - **Sugerencia de nombre de usuario**. Escriba el texto de sugerencia que se muestra a los usuarios en caso de que olviden su nombre de usuario. Este texto debe ser Unicode, sin código ni vínculos y no puede superar los 64 caracteres. Si un invitado inicia sesión en la aplicación, se recomienda no agregar esta sugerencia.

        - **Texto de la página de inicio de sesión**. Escriba el texto que aparece en la parte inferior de la página de inicio de sesión. Puede usar este texto para comunicar información adicional, como el número de teléfono de su departamento de soporte técnico o una declaración legal. Este texto debe ser Unicode y no superar los 256 caracteres. También se recomienda no incluir vínculos ni etiquetas HTML.

    - **Configuración avanzada**
            
        ![Página de configuración de personalización de marca de empresa con la configuración avanzada completada](media/customize-branding/configure-company-branding-advanced-settings.png)   

        - **Color de fondo de la página de inicio de sesión**. Especifique el color hexadecimal (por ejemplo, el blanco es #FFFFFF) que aparecerá en lugar de la imagen de fondo en situaciones de conexión de ancho de banda bajo. Se recomienda usar el color principal del logotipo del banner o el color de la organización.

        - **Imagen de logotipo cuadrado**. Seleccione una imagen .png (formato preferido) o .jpg del logotipo de la organización para mostrar a los usuarios durante el proceso de configuración de los nuevos dispositivos Windows 10 Enterprise. Esta imagen se utiliza únicamente para la autenticación de Windows y aparece solo en inquilinos que usan [Windows Autopilot]( https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) para la implementación o para páginas de entrada de contraseñas en otras experiencias de Windows 10. En algunos casos también pueden aparecer en el cuadro de diálogo de consentimiento.
        
            La imagen no puede tener más de 240 x 240 píxeles de tamaño y debe tener un tamaño de archivo inferior a 10 KB. Se recomienda usar una imagen transparente, ya que el fondo podría no coincidir con el fondo del logotipo. También se recomienda no agregar relleno alrededor de la imagen, ya que podría reducir la apariencia del logotipo.
    
        - **Logotipo cuadrado, tema oscuro**. Igual que la imagen de logotipo cuadrado anterior. Esta imagen de logotipo ocupa el lugar de la imagen de logotipo cuadrado cuando se usa con un fondo oscuro, como con las pantallas unidas a Azure AD de Windows 10 en la configuración rápida (OOBE).  Si el logotipo se ve bien en fondos blancos y en fondos de color azul oscuro o negro, no es necesario agregar esta imagen. 
        
        - **Visualización de la opción para seguir conectado**. Puede optar por permitir que los usuarios permanezcan con la sesión iniciada en Azure AD hasta que cierren sesión explícitamente. Si elige **No**, esta opción se oculta y los usuarios deberán iniciar sesión cada vez que el explorador se cierre y se vuelva a abrir.
        
            >[!Note]
            >Algunas características de SharePoint Online y Office 2010 dependen de que los usuarios puedan elegir seguir conectados. Si establece esta opción en **No**, puede que los usuarios reciban solicitudes adicionales e inesperadas de inicio de sesión.
   

3. Una vez haya terminado de agregar la personalización de marca, seleccione **Guardar**.

    Si este proceso supone la creación de la primera configuración de personalización de marca personalizada, se convertirá en el valor predeterminado del inquilino. Si tiene configuraciones adicionales, podrá elegir la configuración predeterminada.
    
    >[!Important]
    >Para agregar más configuraciones de personalización de marca corporativa al inquilino, debe elegir **Nuevo idioma** en la página **Contoso: personalización de marca de empresa**. Se abrirá la página **Configurar personalización de marca de empresa**, donde puede seguir los pasos anteriores.

## <a name="update-your-custom-branding"></a>Actualización de la personalización de marca personalizada
Después de crear la personalización de marca personalizada, puede volver atrás y cambiar todo lo que quiera.

### <a name="to-edit-your-custom-branding"></a>Para editar la personalización de marca personalizada
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory**, después, seleccione **Personalización de marca de empresa** y, a continuación, seleccione **Configurar**.

    ![Contoso: página de personalización de marca de empresa que muestra la configuración predeterminada](media/customize-branding/company-branding-default-config.png)

3. En la página **Configurar personalización de marca de empresa**, agregue, quite o cambie información basándose en las descripciones de la sección [Personalización de la página de inicio de sesión de Azure AD](#customize-your-azure-ad-sign-in-page) de este artículo.

4. Seleccione **Guardar**.

   Puede transcurrir hasta una hora para que los cambios efectuados se muestren en la personalización de marca de la página de inicio de sesión.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Incorporación de personalización de marca de empresa específica de un idioma a su directorio
No se puede cambiar el idioma de la configuración original del idioma predeterminado. Sin embargo, si necesita una configuración en otro idioma, puede crear una configuración nueva.

### <a name="to-add-a-language-specific-branding-configuration"></a>Para agregar una configuración de personalización de marca específica del idioma

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory**, después, seleccione **Personalización de marca de empresa** y, a continuación, seleccione **Nuevo idioma**.

    ![Contoso: página de personalización de marca de empresa con la opción Nuevo idioma resaltada](media/customize-branding/company-branding-new-language.png)

3. En la página **Configurar personalización de marca de empresa**, seleccione el idioma (por ejemplo, francés) y, a continuación, agregue la información traducida en función de las descripciones que aparecen en la sección [Personalización de la página de inicio de sesión de Azure AD](#customize-your-azure-ad-sign-in-page) de este artículo.

4. Seleccione **Guardar**.

    La página **Contoso: personalización de marca de empresa** se actualiza para mostrar la nueva configuración en francés.

    ![Contoso: página de personalización de marca de empresa que muestra la configuración predeterminada](media/customize-branding/company-branding-french-config.png)

## <a name="add-your-custom-branding-to-pages"></a>Incorporación de la personalización de marca personalizada a páginas
Agregue la personalización de marca personalizada a páginas mediante la modificación de la parte final de la dirección URL con el texto `?whr=yourdomainname`. Esta modificación funciona en varias páginas, incluidas la página de configuración de Multi-Factor Authentication (MFA), la página de configuración de autoservicio de restablecimiento de contraseña (SSPR) y la página de inicio de sesión.

**Ejemplos:**

**Dirección URL original:** https://aka.ms/MFASetup<br>
**Dirección URL personalizada:** https://account.activedirectory.windowsazure.com/proofup.aspx?whr=contoso.com

**Dirección URL original:** https://aka.ms/SSPR<br>
**Dirección URL personalizada:** https://passwordreset.microsoftonline.com/?whr=contoso.com

 

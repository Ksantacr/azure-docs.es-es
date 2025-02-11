---
title: ContentDefinitions - Azure Active Directory B2C | Microsoft Docs
description: Especifique el elemento ContentDefinitions de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 6807ae8d9c8b8458b9ac4552a90119a8f709d330
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64693359"
---
# <a name="contentdefinitions"></a>ContentDefinitions

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede personalizar la apariencia de cualquier [perfil técnico autodeclarado](self-asserted-technical-profile.md). Azure Active Directory (Azure AD) B2C ejecuta código en el explorador del cliente y usa un método moderno denominado CORS (uso compartido de recursos entre orígenes). 

Para personalizar la interfaz de usuario, especifique una URL en el elemento **ContentDefinition** con el contenido HTML personalizado. En el perfil técnico autodeclarado o **OrchestrationStep**, apunte a ese identificador de definición de contenido. La definición de contenido puede contener un elemento **LocalizedResourcesReferences** que especifique una lista de los recursos localizados que se cargarán. Azure AD B2C combina elementos de la interfaz de usuario con el contenido HTML cargado desde la URL y, después, muestra la página al usuario.

El elemento **ContentDefinitions** contiene direcciones URL a plantillas HTML5 que pueden usarse en un recorrido del usuario. El URI de la página HTML5 se usa para un paso especificado de la interfaz de usuario. Por ejemplo, páginas de error, registro o inicio de sesión, o restablecimiento de contraseña. Para modificar la apariencia, invalide el elemento LoadUri del archivo HTML5. Puede crear nuevas definiciones de contenido según sus necesidades. Este elemento puede contener una referencia a recursos localizados, al identificador de localización especificado en el elemento [Localization](localization.md).

En el ejemplo siguiente, se muestra el identificador de definición de contenido y la definición de los recursos localizados:

```XML
<ContentDefinition Id="api.localaccountsignup">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Local account sign up page</Item>
  </Metadata>
  <LoalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
    ...
```

Los metadatos del perfil técnico autodeclarado **LocalAccountSignUpWithLogonEmail** contienen el identificador de definición de contenido **ContentDefinitionReferenceId** establecido en `api.localaccountsignup`.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    ...
  </Metadata>
  ...
```


## <a name="contentdefinition"></a>ContentDefinition

El elemento **ContentDefinition** contiene el atributo siguiente:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| Id | Sí | Un identificador de una definición de contenido. El valor se especifica en la sección **Id. de definición de contenido** más adelante en esta página. |

El elemento **ContentDefinition** contiene los elementos siguientes:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| LoadUri | 1:1 | Cadena que contiene la URL de la página HTML5 de la definición de contenido. |
| RecoveryUri | 0:1 | Cadena que contiene la URL de la página HTML para mostrar un error relacionado con la definición de contenido. | 
| DataUri | 1:1 | Cadena que contiene la URL relativa de un archivo HTML que proporciona la experiencia del usuario que se invocará para el paso. |  
| Metadatos | 1:1 | Colección de pares de clave y valor que contiene los metadatos usados por la definición de contenido. | 
| LocalizedResourcesReferences | 0:1 | Colección de referencias de recursos localizados. Use este elemento para personalizar la localización de la interfaz de usuario y el atributo de notificaciones. |

### <a name="datauri"></a>DataUri

El elemento **DataUri** se usa para especificar el identificador de página. Azure AD B2C usa el identificador de página para cargar e iniciar elementos de la interfaz de usuario y código JavaScript del lado cliente. El formato del valor es `urn:com:microsoft:aad:b2c:elements:page-name:version`.  En la tabla siguiente, se muestra una lista de los identificadores de página que puede usar.

| Valor |   DESCRIPCIÓN |
| ----- | ----------- |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | Muestra una página de error cuando se produce una excepción o un error. |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | Muestra una lista de los proveedores de identidades que los usuarios pueden elegir durante el inicio de sesión. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | Muestra un formulario para iniciar sesión con una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. Este valor también proporciona la función “Mantener iniciada la sesión” y el vínculo "¿Ha olvidado la contraseña?" | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | Muestra un formulario para iniciar sesión con una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | Verifica los números de teléfono mediante mensajes de texto o llamadas de voz durante el registro o inicio de sesión. |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | Muestra un formulario que permite a los usuarios crear o actualizar su perfil. | 


### <a name="localizedresourcesreferences"></a>LocalizedResourcesReferences

El elemento **LocalizedResourcesReferences** contiene los elementos siguientes:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| LocalizedResourcesReference | 1:n | Una lista de referencias de recursos localizados para la definición de contenido. | 

El elemento **LocalizedResourcesReferences** contiene los atributos siguientes:

| Atributo | Obligatorio | DESCRIPCIÓN |
| --------- | -------- | ----------- |
| Idioma | Sí | Una cadena que contiene un idioma admitido para la directiva según RFC 5646 (etiquetas para identificar idiomas). |
| LocalizedResourcesReferenceId | Sí | El identificador del elemento **LocalizedResources**. |

En el ejemplo siguiente, se muestra una definición de contenido de registro o inicio de sesión:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
</ContentDefinition>
```

En el ejemplo siguiente, se muestra una definición de contenido de registro o inicio de sesión con una referencia a la localización de inglés, francés y español:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.signuporsignin.en" />
    <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="api.signuporsignin.rf" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.signuporsignin.es" />
</LocalizedResourcesReferences>
</ContentDefinition>
```

Para obtener información sobre cómo agregar la compatibilidad de localización a sus definiciones de contenido, vea [Localization](localization.md).

## <a name="content-definition-ids"></a>Id. de definición de contenido

El id. de atributo del elemento **ContentDefinition** especifica el tipo de página relacionado con la definición de contenido. El elemento define el contexto que se aplicará en una plantilla personalizada de HTML5/CSS. En la tabla siguiente, se describe el conjunto de id. de definición de contenido reconocido por el Marco de experiencia de identidad y los tipos de página relacionados. Puede crear sus propias definiciones de contenido con un id. arbitrario.

| ID | Plantilla predeterminada | DESCRIPCIÓN | 
| -- | ---------------- | ----------- |
| **api.error** | [exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Página de error**: muestra una página de error cuando se produce una excepción o un error. |
| **api.idpselections** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Página de selección de proveedor de identidades**: muestra una lista de los proveedores de identidades que los usuarios pueden elegir durante el inicio de sesión. Las opciones normalmente son proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. |
| **api.idpselections.signup** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Selección de proveedor de identidades para registro**: muestra una lista de los proveedores de identidades que los usuarios pueden elegir durante el registro. Las opciones normalmente son proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. |
| **api.localaccountpasswordreset** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Página ¿Olvidó la contraseña?**: muestra un formulario que los usuarios necesitan completar para iniciar un restablecimiento de contraseña. |
| **api.localaccountsignin** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Página de inicio de sesión en cuenta local**: muestra un formulario para iniciar sesión con una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener un cuadro de entrada de texto y un cuadro de entrada de contraseña. |
| **api.localaccountsignup** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Página de registro en cuenta local**: muestra un formulario para registrar una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener varios controles de entrada, como un cuadro de entrada de texto, un cuadro de entrada de contraseña, un botón de radio, cuadros desplegables de selección única y casillas de selección múltiple. |
| **api.phonefactor** | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Página de autenticación multifactor**: verifica los números de teléfono mediante mensajes de texto o llamadas de voz durante el registro o inicio de sesión. |
| **api.selfasserted** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Página de registro de cuenta de red social**: muestra un formulario que los usuarios necesitan completar al registrarse mediante una cuenta existente de un proveedor de identidades de redes sociales. Esta página es similar a la página anterior de registro en una cuenta social, excepto por los campos de entrada de contraseña. |
| **api.selfasserted.profileupdate** | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Página de actualización de perfil**: muestra un formulario al que pueden acceder los usuarios para actualizar su perfil. Esta página es similar a la página de registro en una cuenta social, excepto por los campos de entrada de contraseña. |
| **api.signuporsignin** | [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Página unificada de registro o inicio de sesión**: controla el proceso de registro o inicio de sesión del usuario. Los usuarios pueden utilizar proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. |
 

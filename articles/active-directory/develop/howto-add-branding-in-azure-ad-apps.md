---
title: Directrices de personalización de marca para aplicaciones | Microsoft Docs
description: Una guía completa sobre recursos orientados al desarrollador para Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: arielgo
ms.custom: aaddev, signin_art
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac14572210b80ac1415d129c5810f0e5443012c9
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540107"
---
# <a name="branding-guidelines-for-applications"></a>Directrices de personalización de marca para aplicaciones

Cuando desarrolle aplicaciones con Azure Active Directory (Azure AD), deberá dirigir a los clientes cuando estos quieran usar su cuenta profesional o educativa (administrada en Azure AD), o su cuenta personal, para suscribirse a la aplicación o iniciar sesión en ella.

En este artículo:

- Obtendrá información sobre los dos tipos de cuentas de usuario administradas por Microsoft y cómo hacer referencia a las cuentas de Azure AD en su aplicación.
- Averiguará qué debe hacer para agregar el logotipo de Microsoft para usarlo en su aplicación.
- Descargue las imágenes de **Iniciar sesión** o **Iniciar sesión con Microsoft** oficiales para usarlas en su aplicación
- Obtenga información sobre la personalización de marca y qué debe hacer y qué no debe hacer en la navegación.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Cuentas personales frente a cuentas profesionales o educativas de Microsoft

Microsoft administra dos tipos de cuentas de usuario:

- **Cuentas personales** (anteriormente conocidas como Windows Live ID). Estas cuentas representan la relación entre los usuarios *individuales* y Microsoft. Se usan para tener acceso a servicios y dispositivos de consumidor de Microsoft. Estas cuentas están pensadas para un uso personal.
- **Cuentas profesionales o educativas.**  Estas cuentas las administra Microsoft en nombre de las organizaciones que usan Azure Active Directory. Estas cuentas se utilizan para iniciar sesión en Office 365 y otros servicios empresariales de Microsoft.

Las cuentas profesionales o educativas de Microsoft normalmente se asignan las organizaciones (compañías, escuelas, administraciones públicas, etc.).a los usuarios finales (empleados, estudiantes, empleados de las administraciones, etc.). Estas cuentas se controlan directamente en la nube (en la plataforma de Azure AD) o se sincronizan con Azure AD desde un directorio local, como Windows Server Active Directory. Microsoft mantiene la *custodia* de las cuentas profesionales o educativas, pero son propiedad la organización y es esta quien las controla.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Cómo hacer referencia a las cuentas de Azure AD en la aplicación

Microsoft no expone a los usuarios finales a los nombres de marca de Azure ni de Active Directory, y usted tampoco debería hacerlo.

- Una vez que los usuarios han iniciado sesión, use el nombre y el logotipo de la organización tanto como sea posible. Esto es mejor que usar términos genéricos como "la organización".
- Cuando los usuarios no han iniciado sesión, refiérase a sus cuentas como "cuentas profesionales o educativas" y use el logotipo de Microsoft para transmitir que Microsoft administra estas cuentas. No utilice términos como "cuenta de la empresa", "cuenta empresarial" ni "cuenta corporativa", ya que estos crearían confusión entre los usuarios.

## <a name="user-account-pictogram"></a>Pictograma de una cuenta de usuario

En una versión anterior de estas directrices, se recomendaba usar un pictograma con un "distintivo azul". Según los comentarios de los usuarios y los desarrolladores, ahora se recomienda usar el logotipo de Microsoft en su lugar. El logotipo de Microsoft ayudará a los usuarios a comprender que pueden reutilizar la cuenta que usan con Office 365 u otros servicios empresariales de Microsoft para iniciar sesión en la aplicación.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Inscripción e inicio de sesión con Azure AD

La aplicación puede presentar rutas diferentes para la suscripción y el inicio de sesión. En las secciones siguientes, se proporciona una guía visual para ambos escenarios.

**Si la aplicación admite el registro de usuarios finales (por ejemplo, evaluación gratuita o modelo "freemium")**: puede mostrar un botón de **inicio de sesión** que permita a los usuarios acceder a la aplicación con su cuenta profesional o educativa. Azure AD mostrará una petición de consentimiento la primera vez que accedan a la aplicación.

**Si su aplicación requiere permisos que solo los administradores pueden otorgar o requiere licencias para organizaciones**: separe la adquisición de administrador del inicio de sesión como usuario. El **botón "obtener esta aplicación"** redirigirá a los administradores al inicio de sesión. Después, se les pedirá que manifiesten su consentimiento en nombre de los usuarios de su organización, que cuenta con la ventaja añadida de suprimir las peticiones de consentimiento de usuario final en su aplicación.

## <a name="visual-guidance-for-app-acquisition"></a>Guía visual para la adquisición de la aplicación

El vínculo "Obtener la aplicación" debe redirigir al usuario a la página de concesión de acceso (autorización) de Azure AD, para permitir que el administrador de una organización pueda autorizar a su aplicación para que tenga acceso a los datos de la organización que hospeda Microsoft. En el artículo [Integración de aplicaciones con Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) se tratan los detalles sobre cómo solicitar acceso.

Después de que los administradores den su consentimiento a la aplicación, pueden optar por agregarla a la experiencia del iniciador de aplicaciones de Office 365 de los usuarios (accesible desde el botón tipo "gofre" y desde [https://portal.office.com/myapps](https://portal.office.com/myapps)). Si quiere anunciar esta capacidad, puede usar términos como "Agregue esta aplicación a su organización" y mostrar un botón como el del ejemplo siguiente:

![Tipos de aplicaciones y escenarios](./media/howto-add-branding-in-azure-ad-apps/add-to-my-org.png)

Sin embargo, se recomienda que escriba un texto explicativo en lugar de confiar en los botones. Por ejemplo: 

> *Si ya usa Office 365 u otro servicio de negocio de Microsoft, puede conceder acceso a <nombre_su_aplicación> para acceder a los datos de su organización. De esta forma, los usuarios podrán acceder a <nombre_su_aplicación> con sus cuentas profesionales.*

Para descargar el logotipo oficial de Microsoft para su uso en la aplicación, haga clic con el botón derecho en el que quiera usar y guárdelo en su equipo.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Logotipo de Microsoft  | ![Logotipo de Microsoft en formato PNG](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_mssymbol_19.png) | ![Logotipo de Microsoft en formato SVG](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_mssymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Guía visual para el inicio de sesión

La aplicación debe mostrar un botón de inicio de sesión que redirige a los usuarios al punto de conexión de inicio de sesión que corresponda al protocolo que se utiliza para la integración con Azure AD. La siguiente sección proporciona detalles sobre el aspecto que debería tener ese botón.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictograma e Iniciar sesión en Microsoft

Se trata de la asociación del logotipo de Microsoft y el término "Iniciar sesión en Microsoft" que distingue exclusivamente a Azure AD de otros proveedores de identidades que pueden ser compatibles con su aplicación. Si no tiene suficiente espacio para Iniciar sesión en Microsoft, puede acortarlo a "Iniciar sesión". Puede usar una combinación de colores claros u oscuros para los botones.

En el diagrama siguiente se muestran las líneas rojas recomendadas por Microsoft al usar los recursos con la aplicación. Las líneas rojas se aplican a "Inicio de sesión en Microsoft" o a la versión más corta "Inicio de sesión".

![Inicio de sesión con líneas rojas de Microsoft](./media/howto-add-branding-in-azure-ad-apps/sign-in-with-microsoft-redlines.png)

Para descargar las imágenes oficiales para su uso en la aplicación, haga clic con el botón derecho en la que quiera usar y guárdela en su equipo.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Inicio de sesión en Microsoft (tema oscuro)  | ![PNG del tema oscuro del botón de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark.png) | ![SVG del tema oscuro del botón de inicio de sesión en Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark.svg) |
| Inicio de sesión en Microsoft (tema claro) | ![PNG del tema claro del botón de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light.png) | ![SVG del tema claro del botón de inicio de sesión en Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light.svg) |
| Inicio de sesión (tema oscuro)                 | ![PNG del tema oscuro del botón corto de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark_short.png) | ![SVG del tema oscuro del botón corto de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark_short.svg) |
| Inicio de sesión (tema claro)                | ![PNG del tema claro del botón corto de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light_short.png) | ![SVG del tema claro del botón corto de inicio de sesión](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light_short.svg) |

## <a name="branding-dos-and-donts"></a>Personalización de marca: qué se debe hacer y qué no

**USE** la cuenta profesional o educativa en combinación con el botón Iniciar sesión en Microsoft para proporcionar una explicación adicional que ayude a los usuarios finales a saber si pueden utilizarla. **NO USE** otros términos como "cuenta empresarial", "cuenta de negocio" o "cuenta corporativa".

**NO USE** "Office 365 ID" ni "Azure ID". Office 365 es también el nombre de un producto para consumidores de Microsoft que no usa Azure AD para la autenticación.

**No:** no modifique el logotipo de Microsoft.

**NO EXPONGA** a los usuarios finales a las marcas Azure o Active Directory. Sin embargo, es correcto usar estos términos con los desarrolladores, profesionales de TI y administradores.

## <a name="navigation-dos-and-donts"></a>Navegación: qué se debe hacer y qué no

**Sí:** proporcione a los usuarios una manera de cerrar la sesión y cambiar a otra cuenta de usuario. La mayoría de los usuarios tiene una sola cuenta personal de Microsoft, Facebook, Google o Twitter, pero pueden estar asociados con varias organizaciones. Próximamente se admitirá el inicio de sesión simultáneo para varios usuarios.

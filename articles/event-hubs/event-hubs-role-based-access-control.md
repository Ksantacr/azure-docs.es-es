---
title: 'Versión preliminar del control de acceso basado en rol (RBAC): Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona información sobre el control de acceso basado en rol para Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/21/2019
ms.author: shvija
ms.openlocfilehash: ae970b9612154a6463c4bf44a65da71a20c81635
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978309"
---
# <a name="active-directory-role-based-access-control-preview"></a>Control de acceso basado en rol de Active Directory (versión preliminar)

Microsoft Azure proporciona una administración integrada del control de acceso para recursos y aplicaciones basados en Azure Active Directory (Azure AD). Con Azure AD, puede administrar las cuentas y aplicaciones del usuario específicamente para sus aplicaciones basadas en Azure, o puede federar la infraestructura existente de Active Directory con Azure AD para un inicio de sesión único para toda la empresa que también abarque los recursos de Azure y las aplicaciones hospedadas en Azure. Posteriormente, puede asignar esas identidades de usuario y de aplicación de Azure AD a roles globales y específicos del servicio para otorgar acceso a los recursos de Azure.

Para Azure Event Hubs, la administración de los espacios de nombres y de todos los recursos relacionados mediante Azure Portal y la API de administración de recursos de Azure, ya se ha protegido mediante el modelo de *control de acceso basado en rol* (RBAC). El control de acceso basado en rol de las operaciones en tiempo de ejecución es una característica que está ahora en versión preliminar pública. 

Una aplicación que use el control de acceso basado en rol de Azure AD no necesita controlar las reglas y claves de SAS ni ningún otro token de acceso específico a Event Hubs. La aplicación cliente permite interactuar con Azure AD para establecer un contexto de autenticación y adquiere un token de acceso para Event Hubs. Con respecto a las cuentas de usuario de dominio que requieren un inicio de sesión interactivo, la aplicación no controla nunca ninguna credencial directamente.

## <a name="event-hubs-roles-and-permissions"></a>Roles y permisos de Event Hubs
Azure proporciona los siguientes roles RBAC integrados para autorizar el acceso a un espacio de nombres de Event Hubs:

El [propietario de los datos de Event Hubs (versión preliminar)](../role-based-access-control/built-in-roles.md#service-bus-data-owner) rol permite el acceso de datos a un espacio de nombres de Event Hubs y sus entidades (colas, temas, suscripciones y filtros)

>[!IMPORTANT]
> Anteriormente, se admite agregar una identidad administrada para el **propietario** o **colaborador** rol. Sin embargo, los datos de acceso privilegios para **propietario** y **colaborador** rol ya no se respetan. Si usas el **propietario** o **colaborador** rol, el conmutador a utilizar el **propietario de los datos de Event Hubs** rol.


## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Uso de Event Hubs con una cuenta de usuario de dominio de Azure AD

En la siguiente sección se describen los pasos necesarios para crear y ejecutar una aplicación de ejemplo que solicita que un usuario interactivo de Azure AD inicie sesión, cómo conceder acceso a Event Hubs para esa cuenta de usuario y cómo utilizar esa identidad para acceder a Event Hubs. 

En esta introducción se describe una aplicación de consola simple, y el [código para esta aplicación se encuentra en GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/).

### <a name="create-an-active-directory-user-account"></a>Creación de una cuenta de usuario de Active Directory

El primer paso es opcional. Cada suscripción de Azure se empareja automáticamente con un inquilino de Azure Active Directory y, si tiene acceso a una suscripción de Azure, su cuenta de usuario ya está registrada. Esto significa que solamente puede usar su cuenta. 

Si desea crear una cuenta específica para este escenario, [siga estos pasos](../automation/automation-create-aduser-account.md). Debe tener permiso para crear cuentas en el inquilino de Azure Active Directory, lo cual puede que no sea el caso para escenarios empresariales más grandes.

### <a name="create-an-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

A continuación, [crear un espacio de nombres de Event Hubs](event-hubs-create.md). 

Una vez creado el espacio de nombres, vaya a su página de **Control de acceso (IAM)** en Azure Portal y, a continuación, haga clic en **Agregar asignación de roles** para agregar la cuenta de usuario de Azure AD al rol de propietario. Si usa su propia cuenta de usuario y ha creado el espacio de nombres, ya está en el rol de propietario. Para agregar una cuenta diferente al rol, busque el nombre de la aplicación web en el campo **Seleccionar** del panel **Agregar permisos** y, a continuación, haga clic en la entrada. A continuación, haga clic en **Guardar**. Ahora, la cuenta de usuario ya tiene acceso al espacio de nombres de Event Hubs y a la cola que creó anteriormente.
 
### <a name="register-the-application"></a>Registro de la aplicación

Antes de poder ejecutar la aplicación de ejemplo, regístrela en Azure AD y apruebe la petición de consentimiento que permite que la aplicación pueda acceder a Event Hubs en su nombre. 

Dado que la aplicación de ejemplo es una aplicación de consola, debe registrar una aplicación nativa y agregar permisos de API para **Microsoft.EventHub** en el conjunto de "permisos necesarios". Las aplicaciones nativas también necesitan un **URI de redireccionamiento** en Azure AD que actúe como identificador. No es necesario que este URI sea un destino de red. Use `https://eventhubs.microsoft.com` para este ejemplo, dado que el ejemplo de código ya utiliza ese URI.

Se proporciona información detallada sobre los pasos de registro en [este tutorial](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Siga los pasos para registrar una aplicación **nativa** y, a continuación, siga las instrucciones de actualización para agregar **Microsoft.EventHub** API a los permisos necesarios. A medida que vaya realizando los pasos, tome nota del valor de **TenantId** y **ApplicationId**, ya que los necesitará para ejecutar la aplicación.

### <a name="run-the-app"></a>Ejecución de la aplicación

Antes de poder ejecutar el ejemplo, edite el archivo App.config y, según el escenario, establezca los siguientes valores:

- `tenantId`: establézcalo en el valor de **TenantId**.
- `clientId`: establézcalo en el valor de **ApplicationId**. 
- `clientSecret`: si desea iniciar sesión mediante el secreto de cliente, créelo en Azure AD. Además, utilice una aplicación web o API en lugar de una aplicación nativa. Además, agregue la aplicación en **Control de acceso (IAM)** en el espacio de nombres que creó anteriormente.
- `eventHubNamespaceFQDN`: establézcalo en el nombre DNS completo del espacio de nombres de Event Hubs recién creado. Por ejemplo, `example.servicebus.windows.net`.
- `eventHubName`: establézcalo en el nombre del centro de eventos que creó.
- El URI de redireccionamiento que especificó en la aplicación en los pasos anteriores.
 
Al ejecutar la aplicación de consola, deberá seleccionar un escenario. Haga clic en **Interactive User Login** (Inicio de sesión de usuario interactivo). Para ello, escriba su número y presione ENTRAR. La aplicación muestra una ventana de inicio de sesión, solicita su consentimiento para acceder a Event Hubs y, a continuación, utiliza el servicio para ejecutarse en el escenario de envío o recepción mediante la identidad de inicio de sesión.

La aplicación usa `ServiceAudience.EventHubsAudience` como audiencia del token. Al usar otros lenguajes o SDK donde la audiencia no está disponible como una constante, el valor correcto que se debe usar es `https://eventhubs.azure.net/`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Event Hubs, visite los vínculos siguientes:

* Empiece por un [tutorial de Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
* [Detalles de precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

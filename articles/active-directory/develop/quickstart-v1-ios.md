---
title: Compilación de una aplicación de iOS que se integra en Azure AD para el inicio de sesión y las llamadas a API protegidas con OAuth 2.0 | Microsoft Docs
description: Aprenda a hacer que los usuarios inicien sesión y a llamar a Microsoft Graph API desde una aplicación de iOS.
services: active-directory
documentationcenter: ios
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: brandwe
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8538a96e1919fbff9f800a785788ccaa41a68392
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121930"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Inicio rápido: Hacer que los usuarios inicien sesión y llamar a Microsoft Graph API desde una aplicación de iOS

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Azure Active Directory (Azure AD) proporciona la Biblioteca de autenticación de Active Directory (ADAL) para los clientes de iOS que necesitan tener acceso a recursos protegidos. ADAL simplifica el proceso que la aplicación usa para obtener tokens de acceso. 

En este tutorial de inicio rápido, compilará una aplicación de lista de tareas pendientes en Objective-C:

* Obtiene tokens de acceso para llamar a Azure AD Graph API mediante el protocolo de autenticación OAuth 2.0.
* Busca un directorio para los usuarios con un alias determinado.

Para crear la aplicación de trabajo completa, tendrá que:

1. Registrar la aplicación con Azure AD
1. Instalar y configurar ADAL.
1. Usar ADAL para obtener tokens de Azure AD.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, complete estos requisitos previos:

* [Descargue el esqueleto de la aplicación](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) o [el ejemplo completado](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).
* Necesita un inquilino de Azure AD en el que pueda crear usuarios y registrar una aplicación. Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).

> [!TIP]
> Pruebe el [portal para desarrolladores](https://identity.microsoft.com/Docs/iOS) para empezar a trabajar con Azure AD en tan solo unos minutos. El portal para desarrolladores lo guiará a través del proceso de registro de una aplicación y de integración de Azure AD en el código. Cuando haya terminado, tendrá una aplicación sencilla que podrá autenticar a los usuarios del inquilino, así como un back-end con capacidad para aceptar tokens y realizar validaciones.

## <a name="step-1-determine-what-your-redirect-uri-is-for-ios"></a>Paso 1: Determinación de cuál es el URI de redireccionamiento para iOS

Para iniciar de forma segura sus aplicaciones en ciertos escenarios SSO, es necesaria la creación de un *URI de redireccionamiento* en un formato determinado. Un URI de redireccionamiento se usa para garantizar que los tokens vuelvan a la aplicación correcta que los solicitó.

El formato de iOS para un URI de redireccionamiento es:

```
<app-scheme>://<bundle-id>
```

* **app-scheme**: está registrado en el proyecto de XCode y es como otras aplicaciones pueden llamarlo. Puede encontrar app-scheme en **Info.plist -> Tipos de URL -> Identificador de URL**. Cree un app-scheme si aún no tiene ninguno configurado.
* **bundle-id**: es el identificador de paquete que se encuentra en **identity** de la configuración del proyecto de XCode.

Ejemplo de este código de inicio rápido:

***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***.

## <a name="step-2-register-the-directorysearcher-application"></a>Paso 2: Registro de la aplicación DirectorySearcher

Para configurar la aplicación para que obtenga tokens, debe registrarla en el inquilino de Azure AD y concederle permiso de acceso a Graph API de Azure AD.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la barra superior, seleccione su cuenta. En la lista **Directorio** lista, elija el inquilino de Active Directory donde desea registrar la aplicación.
3. Seleccione **Todos los servicios** en el panel de navegación izquierdo y seleccione **Azure Active Directory**.
4. Seleccione **Registros de aplicaciones** y luego **Nuevo registro**.
5. Siga las indicaciones para crear una aplicación de cliente.
    * **Nombre** es el nombre de la aplicación y describe la aplicación a los usuarios finales.
    * El **URI de redirección** es una combinación de esquema y cadena que usará Azure AD para devolver respuestas de tokens. Escriba un valor que sea específico de la aplicación y que se base en la información del anterior URI de redireccionamiento. Seleccione también **Public client (mobile and desktop)** (Cliente público [dispositivos móviles y escritorio]) en la lista desplegable.
6. Después de haber completado el registro, Azure AD le asigna a la aplicación un identificador de aplicación único. Necesitará este valor en las secciones siguientes, así que cópielo de la pestaña de la aplicación.
7. En la página **Permisos de API**, seleccione **Agregar un permiso**. En **Seleccionar una API**, seleccione ***Microsoft Graph***.
8. En **Permisos delegados**, seleccione el permiso **User.Read** y presione **Agregar** para guardar. Este permiso configura la aplicación de modo que consulte Graph API de Azure AD para los usuarios.

## <a name="step-3-install-and-configure-adal"></a>Paso 3: Instalación y configuración de ADAL

Ahora que tiene una aplicación en Azure AD, puede instalar ADAL y escribir el código relacionado con la identidad. Para que ADAL se comunique con Azure AD, hay que proporcionarle información sobre el registro de la aplicación.

1. Comience agregando ADAL al proyecto de DirectorySearcher mediante CocoaPods.

    ```
    $ vi Podfile
    ```
1. Agregue lo siguiente a este podfile:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

1. Cargue el archivo podfile mediante CocoaPods. Mediante este paso se crea un área de trabajo de XCode que cargará.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

1. En el proyecto QuickStart, abra el archivo plist `settings.plist`.
1. Reemplace los valores de los elementos de la sección para usar los mismo valores indicados en Azure Portal. El código hace referencia a estos valores cada vez que usa ADAL.
    * `tenant` es el dominio del inquilino de Azure AD, por ejemplo, contoso.onmicrosoft.com.
    * `clientId` es el identificador de cliente de la aplicación que copió del portal.
    * `redirectUri` es la dirección URL de redireccionamiento que ha registrado en el portal.

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Paso 4: Uso de ADAL para obtener tokens de Azure AD

El principio básico inherente a ADAL es que cada vez que la aplicación necesita un token de acceso, simplemente llama a un completionBlock `+(void) getToken :` y ADAL se encarga del resto.

1. En el proyecto `QuickStart`, abra `GraphAPICaller.m` y busque el comentario `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` en la parte superior.

    Este es el lugar en el que se pasan a ADAL mediante CompletionBlock las coordenadas necesarias para comunicarse con Azure AD e indicarle cómo almacenar en caché los tokens.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Debe utilizar este token para buscar usuarios en el grafo. Busque el comentario `// TODO: implement SearchUsersList`. Este método realiza una solicitud GET a Graph API de Azure AD para realizar una consulta sobre los usuarios cuyo UPN comienza con el término de búsqueda especificado. 

    Para realizar una consulta a Graph API de Azure AD, tiene que incluir un access_token en el encabezado `Authorization` de la solicitud. Aquí es donde entra en juego AAL.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```

3. Cuando la aplicación llama a `getToken(...)` para solicitar un token, ADAL intenta devolver uno sin pedir credenciales al usuario. Si ADAL determina que el usuario debe iniciar sesión para obtener un token, mostrará un cuadro de diálogo de inicio de sesión, recopilará las credenciales del usuario y devolverá un token tras una autenticación correcta. Si ADAL no puede devolver un token por alguna razón, mostrará una `AdalException`.

> [!NOTE]
> El objeto `AuthenticationResult` contiene un objeto `tokenCacheStoreItem` que puede usar para recopilar información que la aplicación pueda necesitar. En QuickStart, `tokenCacheStoreItem` se usa para determinar si ya se ha producido la autenticación.

## <a name="step-5-build-and-run-the-application"></a>Paso 5: Compilación y ejecución de la aplicación

Felicidades. Ahora tiene una aplicación de iOS operativa que puede autenticar usuarios, realizar llamadas seguras a las API web mediante OAuth 2.0 y obtener información básica sobre el usuario.

Si todavía no lo ha hecho, ahora es el momento de completar el inquilino con algunos usuarios.

1. Inicie la aplicación QuickStart e inicie sesión con uno de esos usuarios.
1. Busque otros usuarios según su UPN.
1. Cierre la aplicación y vuelva a abrirla. Observe que la sesión del usuario permanece intacta.

ADAL facilita la incorporación de todas estas características comunes de identidad en la aplicación. Hace el trabajo sucio por usted: administración en caché, compatibilidad con el protocolo OAuth, presentación al usuario de una interfaz de usuario de inicio de sesión y actualización de tokens expirados. Todo lo que necesita saber es una única llamada de API, `getToken`.

Como referencia, en [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip) puede ver el ejemplo finalizado (sin sus valores de configuración).

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a otros escenarios. Se recomienda probar los siguientes:

* [Proteger una API web de Node.js con Azure AD](quickstart-v1-nodejs-webapi.md)
* Obtener información sobre [cómo habilitar el inicio de sesión único entre aplicaciones en iOS mediante ADAL](howto-v1-enable-sso-ios.md)  

---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 83a0adf98298225b52d3b4fdfa2ca861ebb70bb9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140680"
---
**Back-end de .NET (C#)**:
  
1. En Visual Studio, haga clic con el botón derecho en el proyecto de servidor, haga clic en **Administrar paquetes de NuGet**, busque `Microsoft.Azure.NotificationHubs` y, por último, haga clic en **Instalar**. Esto instala la biblioteca de Notification Hubs para enviar notificaciones desde el back-end.
2. En el proyecto de Visual Studio del back-end, abra **Controladores** > **TodoItemController.cs**. Al principio del archivo, agregue la siguiente instrucción `using` :

    ```csharp
    using Microsoft.Azure.Mobile.Server.Config;
    using Microsoft.Azure.NotificationHubs;
    ```

3. Reemplace el método `PostTodoItem` por el código siguiente:  

    ```csharp
    public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
    {
        TodoItem current = await InsertAsync(item);
        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;

        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // iOS payload
        var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }
        return CreatedAtRoute("Tables", new { id = current.Id }, current);
    }
    ```

4. Vuelva a publicar el proyecto de servidor.

**Back-end de Node.js**:

1. Si aún no lo ha hecho, [descargue el proyecto de inicio rápido](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) o utilice el [editor en línea de Azure Portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).    

2. Reemplace el script de la tabla todoitem.js por el código siguiente:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    // When adding record, send a push notification via APNS
    table.insert(function (context) {
        // For details of the Notification Hubs JavaScript SDK, 
        // see https://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Create a payload that contains the new item Text.
        var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";

        // Execute the insert; Push as a post-execute action when results are returned as a Promise.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    context.push.apns.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
    });

    module.exports = table;
    ```

3. Cuando edite el archivo en el equipo local, vuelva a publicar el proyecto de servidor.

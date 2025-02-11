---
title: archivo de inclusión
description: archivo de inclusión
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 28d003e123069c47d87d81570b4a5b69b3b9d64b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66128218"
---
1. Para crear un recurso de Azure SignalR Service, inicie sesión en [Azure Portal](https://portal.azure.com). En la parte superior izquierda de la página, seleccione **+ Crear un recurso**. En el cuadro de texto **Buscar en Marketplace**, escriba **SignalR Service**.

2. Seleccione **SignalR Service** en los resultados y seleccione **Crear**.

3. En la página de configuración del **SignalR** nuevo, agregue la configuración siguiente para el recurso SignalR nuevo:

    | NOMBRE | Valor recomendado | DESCRIPCIÓN |
    | ---- | ----------------- | ----------- |
    | Nombre del recurso | *testsignalr* | Escriba un nombre de recurso único para usarlo en el recurso SignalR. El nombre debe ser una cadena de entre 1 y 63 caracteres y solo puede contener números, letras y el carácter de guion (`-`). El nombre no puede empezar ni terminar con el carácter de guion y no se pueden usar varios guiones consecutivos.|
    | Subscription | Elija una suscripción |  Seleccione la suscripción de Azure que quiera usar para probar SignalR. Si su cuenta solo dispone de una suscripción, esta se seleccionará automáticamente y la lista desplegable **Suscripción** no aparecerá.|
    | Grupos de recursos | Cree un grupo de recursos denominado *SignalRTestResources*| Seleccione o cree un grupo de recursos para el recurso SignalR. Este grupo es útil para organizar los distintos recursos que quiera eliminar al mismo tiempo mediante la eliminación del grupo de recursos. Para obtener más información, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../articles/azure-resource-manager/resource-group-overview.md). |
    | Ubicación | *Este de EE. UU.* | Use **Ubicación** para especificar la ubicación geográfica en la que se hospeda el recurso SignalR. Para optimizar el rendimiento, recomendamos que cree el recurso en la misma región que los demás componentes de la aplicación. |
    | Plan de tarifa | *Gratis* | Actualmente, están disponibles las opciones **Gratis** y **Estándar**. |
    | Anclar al panel | ✔ | Active esta casilla para anclar el recurso al panel y que sea más fácil de encontrar. |

4. Seleccione **Crear**. La implementación puede tardar unos minutos en finalizar.

5. Una vez que se complete la implementación, seleccione **Claves** en **CONFIGURACIÓN**. Copie la cadena de conexión de la clave principal. Más adelante usará esta cadena para configurar la aplicación para que use el recurso Azure SignalR Service.

    La cadena de conexión tendrá la forma siguiente:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;

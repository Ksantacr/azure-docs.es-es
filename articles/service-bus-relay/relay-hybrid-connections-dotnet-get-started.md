---
title: Introducción a los Websockets de Conexiones híbridas de Azure Relay en .NET | Microsoft Docs
description: Escriba una aplicación de consola en C# para Websockets de Conexiones híbridas de Azure Relay.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 88d540f274d8b07bfbdbb566c670adf6ddfefd74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60749739"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-net"></a>Introducción a WebSockets de Conexiones híbridas de Relay en .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

En esta guía de inicio rápido, va a crear aplicaciones de remitente y receptor de .NET que envían y reciben mensajes mediante WebSockets de Conexiones híbridas en Azure Relay. Para información acerca de Azure Relay en general, consulte [Azure Relay](relay-what-is-it.md). 

En esta guía de inicio rápido, realizará los siguientes pasos:

1. Creación de un espacio de nombres de Relay mediante Azure Portal.
2. Creación de una conexión híbrida en dicho espacio de nombres mediante Azure Portal.
3. Escritura de una aplicación de consola de servidor (de escucha) para recibir mensajes.
4. Escritura de una aplicación de consola de cliente (remitente) para enviar mensajes.
5. Ejecución de aplicaciones. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* [Visual Studio 2015 o posterior](https://www.visualstudio.com). En los ejemplos de este tutorial se usa Visual Studio 2017.
* Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-namespace"></a>Creación de un espacio de nombres
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Create a hybrid connection
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Creación de una aplicación de servidor (agente de escucha)
En Visual Studio, escriba una aplicación de consola en C# para escuchar y recibir mensajes de Relay.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Creación de una aplicación de cliente (remitente)
En Visual Studio, escriba una aplicación de consola en C# para enviar mensajes a Relay.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Ejecución de las aplicaciones
1. Ejecute la aplicación de servidor.
2. Ejecute la aplicación de cliente y escriba algún texto.
3. Asegúrese de que la consola de aplicación de servidor muestra el texto que se escribió en la aplicación cliente.

    ![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Enhorabuena, ha creado una aplicación de conexiones híbridas de extremo a extremo.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado aplicaciones de cliente y servidor de .NET que han usado WebSockets para enviar y recibir mensajes. La característica Conexiones híbridas de Azure Relay también admite el uso de HTTP para enviar y recibir mensajes. Para aprender a usar HTTP con Conexiones híbridas de Azure Relay, consulte la [guía de inicio rápido de HTTP](relay-hybrid-connections-http-requests-dotnet-get-started.md).

En esta guía de inicio rápido ha usado .NET Framework para crear aplicaciones cliente y servidor. Para aprender a escribir aplicaciones cliente y servidor con Node.js, consulte la [Guía de inicio rápido de WebSockets en Node.js](relay-hybrid-connections-node-get-started.md) o la [Guía de inicio rápido de HTTP para Node.js](relay-hybrid-connections-http-requests-dotnet-get-started.md).


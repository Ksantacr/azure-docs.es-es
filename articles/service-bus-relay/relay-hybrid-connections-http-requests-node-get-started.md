---
title: Introducción a las solicitudes HTTP de Conexiones híbridas de Azure Relay en Node | Microsoft Docs
description: Escriba una aplicación de consola en Node.js para solicitudes HTTP de Conexiones híbridas de Azure Relay en Node.
services: service-bus-relay
documentationcenter: node
author: clemensv
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 11/01/2018
ms.author: clemensv
ms.openlocfilehash: e54a096bd27efddaa9eafb8619e787178550a6e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553952"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-node"></a>Introducción a las solicitudes HTTP de Conexiones híbridas de Relay en Node

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

En esta guía de inicio rápido, creará aplicaciones de remitente y receptor de Node.js que envían y reciben mensajes mediante el protocolo HTTP. Las aplicaciones usan la característica Conexiones híbridas de Azure Relay. Para información acerca de Azure Relay en general, consulte [Azure Relay](relay-what-is-it.md). 

En esta guía de inicio rápido, realizará los siguientes pasos:

1. Creación de un espacio de nombres de Relay mediante Azure Portal.
2. Creación de una conexión híbrida en dicho espacio de nombres mediante Azure Portal.
3. Escritura de una aplicación de consola de servidor (de escucha) para recibir mensajes.
4. Escritura de una aplicación de consola de cliente (remitente) para enviar mensajes.
5. Ejecución de aplicaciones.

## <a name="prerequisites"></a>Requisitos previos
- [Node.js](https://nodejs.org/en/).
- Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-namespace-using-the-azure-portal"></a>Creación de un espacio de nombres mediante Azure Portal
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection-using-the-azure-portal"></a>Creación de una conexión híbrida mediante Azure Portal
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Creación de una aplicación de servidor (agente de escucha)
Para escuchar y recibir mensajes desde Relay, escriba una aplicación de consola en Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-http-requests-node-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Creación de una aplicación de cliente (remitente)

Para enviar mensajes a Relay, puede usar cualquier cliente HTTP o escribir una aplicación de consola en Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-http-requests-node-get-started-client.md)]

## <a name="run-the-applications"></a>Ejecución de las aplicaciones

1. Ejecute la aplicación de servidor: en un símbolo del sistema de Node.js escriba `node listener.js`.
2. Ejecute la aplicación de cliente: en un símbolo del sistema de Node.js escriba `node sender.js` y escriba texto.
3. Asegúrese de que la consola de aplicación de servidor genera el texto que escribió en la aplicación de cliente.

Ya ha creado una aplicación de Conexiones híbridas de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, creó aplicaciones de cliente y servidor de Node.js que usaban HTTP para enviar y recibir mensajes. La característica Conexiones híbridas de Azure Relay también admite el uso de WebSockets para enviar y recibir mensajes. Para aprender a usar WebSockets con Conexiones híbridas de Azure Relay, consulte la [Guía de inicio rápido de WebSockets](relay-hybrid-connections-node-get-started.md).

En esta guía de inicio rápido, ha usado Node.js para crear aplicaciones cliente y servidor. Para aprender a escribir aplicaciones cliente y servidor con .NET Framework, consulte la [Guía de inicio rápido de WebSockets para .NET](relay-hybrid-connections-dotnet-get-started.md) o la [Guía de inicio rápido de HTTP para .NET](relay-hybrid-connections-http-requests-dotnet-get-started.md).

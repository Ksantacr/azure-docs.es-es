---
title: Introducción al desarrollo de aplicaciones de base de datos para Azure Database for MySQL
description: Se presentan las consideraciones de diseño que un desarrollador debe seguir al escribir código de aplicación para conectarse a Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 946f7011c51b7c6844e023d03e01e4c2043d2578
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615650"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Introducción al desarrollo de aplicaciones para Azure Database for MySQL 
En este artículo se describen las consideraciones de diseño que debería seguir un desarrollador al escribir código de aplicación para conectarse a Azure Database for MySQL. 

> [!TIP]
> Para obtener un tutorial que muestre cómo crear un servidor, crear un firewall basado en servidor, ver propiedades de servidor, crear un base de datos y conectarse y consultar mediante Workbench y mysql.exe, consulte [Diseño de la primera base de datos de Azure Database for MySQL](tutorial-design-database-using-portal.md).

## <a name="language-and-platform"></a>Plataforma y lenguaje
Existen ejemplos de código para diferentes lenguajes de programación y plataformas. Puede encontrar vínculos a los ejemplos de código en: [Bibliotecas de conectividad usadas para conectarse a Azure Database for MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Herramientas
Azure Database for MySQL usa la versión de la comunidad de MySQL, compatible con herramientas de administración comunes de MySQL, como Workbench, o utilidades de MySQL como mysql.exe, [phpMyAdmin ](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql) y otras. También puede usar el portal de Azure, la CLI de Azure y la API de REST para interactuar con el servicio de base de datos.

## <a name="resource-limitations"></a>Limitaciones de recursos
Azure Database for MySQL administra los recursos disponibles para un servidor mediante dos mecanismos diferentes: 
- Gobernanza de recursos.
- Aplicación de límites

## <a name="security"></a>Seguridad
Azure Database for MySQL proporciona recursos para limitar el acceso, proteger los datos, configurar usuarios y roles y supervisar las actividades en una base de datos MySQL.

## <a name="authentication"></a>Authentication
Azure Database for MySQL admite la autenticación de servidor de usuarios e inicios de sesión.

## <a name="resiliency"></a>Resistencia
Cuando se produce un error transitorio durante la conexión a la base de datos MySQL, el código debe reintentar la llamada. Recomendamos que en la lógica de reintento se haga uso de la lógica de interrupción; de este modo no se sobrecargará la base de datos SQL con los reintentos de varios clientes a la vez.

- Ejemplos de código: Para obtener ejemplos de código que ilustran la lógica de reintento, consulte los ejemplos para el idioma de su elección en: [Bibliotecas de conectividad usadas para conectarse a Azure Database for MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Administración de conexiones
Las conexiones de base de datos son un recurso limitado, por lo que recomendamos un uso razonable de conexiones al acceder a la base de datos MySQL con el fin de lograr un mejor rendimiento.
- Acceda a la base de datos mediante la agrupación de conexiones o conexiones persistentes.
- Acceda a la base de datos usando la duración de conexión corta. 
- Use la lógica de reintento en su aplicación en el momento del intento de conexión, para detectar errores debidos a conexiones simultáneas que hayan alcanzado el número máximo permitido. En la lógica de reintento, establezca un retraso breve y, a continuación, espere un tiempo aleatorio antes de los intentos de conexiones adicionales.
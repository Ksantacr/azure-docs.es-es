---
title: Control de errores de conectividad transitorios para Azure Database for MySQL | Microsoft Docs
description: Obtenga información sobre cómo controlar los errores de conectividad transitorios de Azure Database for MySQL.
keywords: mysql connection,connection string,connectivity issues,transient error,connection error
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 8942223ce233d424e2368e90d2fbac92b1a443f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525463"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mysql"></a>Control de errores de conectividad transitorios para Azure Database for MySQL

En este artículo se describe cómo controlar los errores transitorios en la conexión a Azure Database for MySQL.

## <a name="transient-errors"></a>Errores transitorios

Un error transitorio es un error que se solucionará automáticamente. Normalmente, estos errores se manifiestan como la interrupción de una conexión con el servidor de base de datos. Tampoco se pueden abrir las nuevas conexiones a un servidor. Los errores transitorios pueden producirse, por ejemplo, cuando se produce un error de hardware o de red. Otra razón podría ser que se está implantando una nueva versión de un servicio PaaS. El sistema mitiga automáticamente la mayoría de estos eventos en menos de 60 segundos. Un procedimiento recomendado a la hora de diseñar y desarrollar aplicaciones en la nube es esperar errores transitorios. Tenga en cuenta que pueden ocurrir en cualquier componente y en cualquier momento, por lo que debe disponer de la lógica adecuada para controlar estas situaciones.

## <a name="handling-transient-errors"></a>Control de errores transitorios

Los errores transitorios se deben controlar con lógica de reintento. Situaciones que se deben tener en cuenta:

* Se produce un error al intentar abrir una conexión.
* Se interrumpe una conexión inactiva en el lado servidor. Cuando se intenta emitir un comando, no se puede ejecutar.
* Se interrumpe una conexión activa que actualmente ejecuta un comando.

El primer y segundo caso son bastante sencillos de controlar. Pruebe a abrir la conexión de nuevo. Si se abre correctamente, el sistema ha mitigado el error transitorio. Puede usar de nuevo la instancia de Azure Database for MySQL. Se recomienda esperar antes de reintentar la conexión. Retroceda si los reintentos iniciales generan errores. De este modo, el sistema puede utilizar todos los recursos disponibles para solucionar la situación de error. Un buen patrón que seguir es el siguiente:

* Espere 5 segundos antes del primer reintento.
* Para cada intento siguiente, aumente la espera exponencialmente, hasta 60 segundos.
* Establezca un número máximo de reintentos como momento en que la aplicación considera que se produjo un error en la operación.

Cuando se produce un error en una conexión con una transacción activa, la recuperación es más difícil de controlar correctamente. Hay dos casos: si la transacción era de solo lectura por naturaleza, es seguro volver a abrir la conexión y volver a intentar la transacción. No obstante, si la transacción también estaba escribiendo en la base de datos, debe determinar si la transacción se revirtió o si se ha ejecutado correctamente antes de producirse el error transitorio. En ese caso, es posible que no haya recibido el reconocimiento de confirmación desde el servidor de bases de datos.

Una manera de hacerlo consiste en generar un id. único en el cliente que se usará para todos los reintentos. Este id. único se pasa como parte de la transacción al servidor y para almacenarlo en una columna con una restricción única. De esta forma, puede reintentar la transacción de forma segura. Se realizará correctamente si se ha revertido la transacción anterior y el id. único del cliente generado aún no existe en el sistema. Producirá un error que indica una infracción de clave duplicada si el id. único se almacenó previamente porque la transacción anterior se completó correctamente.

Si el programa se comunica con Azure Database for MySQL a través de un middleware de otros fabricantes, consulte con el proveedor si el middleware contiene lógica de reintento para errores transitorios.

Asegúrese de probar la lógica de reintento. Por ejemplo, pruebe a ejecutar el código mientras realiza el escalado o reducción vertical de los recursos de proceso del servidor Azure Database for MySQL. La aplicación debe controlar sin problemas el breve tiempo de inactividad que se encuentre durante esta operación.

## <a name="next-steps"></a>Pasos siguientes

* [Troubleshoot connection issues to Azure Database for MySQL](howto-troubleshoot-common-connection-issues.md) (Solución de problemas de conexión a Azure Database for MySQL)

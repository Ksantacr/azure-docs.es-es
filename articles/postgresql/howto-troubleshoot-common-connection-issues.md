---
title: 'Solucionar problemas de conexión a Azure Database for PostgreSQL: servidor único'
description: 'Obtenga información sobre cómo solucionar problemas de conexión a Azure Database for PostgreSQL: servidor único.'
keywords: postgresql connection,connection string,connectivity issues,transient error,connection error
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 8a0fe87703c9fb471174c761a6e8296e6e7a37ec
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952109"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---single-server"></a>Solucionar problemas de conexión a Azure Database for PostgreSQL: servidor único

Los problemas de conexión pueden deberse a una variedad de causas, como:

* Configuración de firewall
* Tiempo de espera de conexión agotado
* Información de inicio de sesión incorrecta
* Límite máximo alcanzado en algunos recursos de Azure Database for PostgreSQL
* Problemas con la infraestructura del servicio
* Mantenimiento en curso en el servicio
* Cambio de la asignación de proceso del servidor mediante el escalado del número de núcleos virtuales o el traslado a otro nivel de servicio

Por lo general, los problemas de conexión de Azure Database for PostgreSQL se pueden clasificar en los siguientes:

* Errores transitorios (corta duración o intermitentes)
* Errores persistentes o no transitorios (errores que se repiten con frecuencia)

## <a name="troubleshoot-transient-errors"></a>Solución de problemas de errores transitorios

Los errores transitorios se producen cuando se realiza el mantenimiento, el sistema encuentra un error con el hardware o software, o cambia el nivel de servicio o núcleos virtuales del servidor. El servicio Azure Database for PostgreSQL tiene alta disponibilidad integrada y está diseñado para mitigar estos tipos de problemas automáticamente. Sin embargo, la aplicación pierde su conexión con el servidor durante un breve período de tiempo que, normalmente, no supera los 60 segundos. Ocasionalmente, estos eventos pueden tardar más tiempo en mitigarse; por ejemplo, cuando una transacción grande requiere una recuperación de larga duración.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Pasos para resolver los problemas de conectividad transitorios

1. Compruebe el [panel de servicios de Microsoft Azure](https://azure.microsoft.com/status) para comprobar si se produjeron interrupciones durante el tiempo en el que la aplicación informó de los errores.
2. Para las aplicaciones que se conectan a un servicio en la nube, como Azure Database for PostgreSQL, se deben prever errores transitorios e implementar la lógica de reintento para controlar estos errores en lugar de mostrarlos como errores de la aplicación a los usuarios. Revise en [Control de errores de conectividad transitorios para Azure Database for PostgreSQL](concepts-connectivity.md) los procedimientos recomendados y las directrices de diseño para controlar los errores transitorios.
3. Conforme un servidor se acerca a sus límites de recursos, los errores pueden parecer un problema de conectividad transitorio. Vea [Limitaciones en Azure Database for PostgreSQL](concepts-limits.md).
4. Si los problemas de conectividad continúan, si el tiempo de detección del error por parte de la aplicación supera los 60 segundos o si el error se repite varias veces en un día determinado, realice una solicitud de soporte técnico a Azure; para ello, seleccione **Obtener soporte** en el sitio [Soporte técnico de Azure](https://azure.microsoft.com/support/options) .

## <a name="troubleshoot-persistent-errors"></a>Solución de problemas de los errores persistentes

Si la aplicación no se puede conectar a Azure Database for PostgreSQL de forma persistente, normalmente indica un problema con uno de los siguientes elementos:

* Configuración del firewall de servidor: Asegúrese de que el firewall del servidor de Azure Database for PostgreSQL esté configurado para permitir conexiones desde el cliente, incluidas puertas de enlace y servidores proxy.
* Configuración del firewall del cliente: El firewall en el cliente debe permitir las conexiones con el servidor de base de datos. Las direcciones IP y los puertos del servidor se deben permitir, así como los nombres de aplicación como PostgreSQL en algunos firewalls.
* Error del usuario: Es posible que haya escrito incorrectamente los parámetros de conexión, como el nombre del servidor en la cadena de conexión o falta un  *\@servername* sufijo en el nombre de usuario.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Pasos para resolver los problemas de conectividad persistentes

1. Configure las [reglas de firewall](howto-manage-firewall-using-portal.md) para permitir la dirección IP del cliente. Con fines temporales de prueba solo, configure una regla de firewall empleando 0.0.0.0 como dirección IP inicial y 255.255.255.255 como dirección IP final. Se abrirá el servidor a todas las direcciones IP. Si se resuelve el problema de conectividad, quite esta regla y cree una regla de firewall para una dirección IP o intervalo de direcciones apropiadamente limitados.
2. En todos los firewalls entre el cliente e internet, asegúrese de que el puerto 5432 está abierto para las conexiones salientes.
3. Compruebe la cadena de conexión y otras opciones de conexión.
4. Compruebe el estado del servicio en el panel. Si cree que hay una interrupción regional, consulte [Introducción a la continuidad empresarial con Azure Database for PostgreSQL](concepts-business-continuity.md) para obtener los pasos de recuperación en una región nueva.

## <a name="next-steps"></a>Pasos siguientes

* [Control de errores de conectividad transitorios para Azure Database for PostgreSQL](concepts-connectivity.md)

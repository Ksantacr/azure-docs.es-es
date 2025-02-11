---
title: Crear y administrar reglas de firewall de MariaDB en Azure Database for MariaDB
description: Crear y administrar reglas de firewall de Azure Database for MariaDB mediante Azure Portal
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: e9ab243692f5a4a1ec7de25774f5bad867698fc3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746409"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>Crear y administrar reglas de firewall de Azure Database for MariaDB mediante Azure Portal
Las reglas de firewall de nivel de servidor pueden usarse para administrar el acceso a una base de datos de Azure para el servidor de MariaDB desde una dirección IP especificada o un intervalo de direcciones IP.

Reglas virtuales Network (VNet) también pueden usarse para proteger el acceso a su servidor. Obtenga más información sobre [reglas mediante el portal de Azure y los puntos de conexión de servicio creación y administración de red Virtual](howto-manage-vnet-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Creación de una regla de firewall de nivel a servidor en Azure Portal

1. En la página del servidor de MariaDB, en el encabezado Configuración, haga clic en **Seguridad de conexión** para abrir la página de seguridad de conexión para Azure Database for MariaDB.

   ![Azure Portal: haga clic en Seguridad de conexión](./media/howto-manage-firewall-portal/1-connection-security.png)

2. Haga clic en **Agregar mi dirección IP** en la barra de herramientas. Se crea automáticamente una regla de firewall con la dirección IP pública del equipo, según la percibe el sistema de Azure.

   ![Azure Portal: haga clic en Agregar mi dirección IP](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. Compruebe la dirección IP antes de guardar la configuración. En algunos casos, la dirección IP observada por Azure Portal difiere de la dirección IP utilizada para acceder a Internet y a los servidores de Azure. Por tanto, puede necesitar cambiar las direcciones IP inicial y final para que la regla funcione según lo previsto.

   Use un motor de búsqueda u otra herramienta en línea para comprobar su propia dirección IP. Por ejemplo, busque "¿cuál es mi dirección IP?".

4. Agregue intervalos de direcciones adicionales. En las reglas de firewall de Azure Database for MariaDB, puede especificar una sola dirección IP o un intervalo de direcciones. Si desea limitar la regla a una única dirección IP, escriba la misma dirección en los campos de dirección IP inicial y dirección IP final. Abra el firewall que permite a los administradores, los usuarios y la aplicación obtener acceso a cualquier base de datos del servidor MariaDB para el que tengan credenciales válidas.

   ![Azure Portal: reglas de firewall](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. Haga clic en **Guardar**, en la barra de herramientas, para guardar esta regla de firewall de nivel de servidor. Espere la confirmación de que la actualización de las reglas de firewall se ha realizado correctamente.

   ![Azure Portal: haga clic en Guardar](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Conexión desde Azure
Para permitir que las aplicaciones de Azure se conecten al servidor Azure Database for MariaDB, deben habilitarse las conexiones de Azure. Por ejemplo, para hospedar una aplicación Azure Web Apps o una aplicación que se ejecute en una VM de Azure, o para conectarse desde una puerta de enlace de administración de datos de Azure Data Factory. No es necesario que los recursos se encuentren en la misma red virtual (VNET) o grupo de recursos para que la regla de firewall habilite las conexiones. Cuando una aplicación desde Azure intenta conectarse a su servidor de s de datos, el firewall comprueba que se permiten las conexiones de Azure. Hay un par de métodos para habilitar estos tipos de conexiones. Una configuración del firewall con dirección inicial y final igual a 0.0.0.0 indica que se permiten estas conexiones. Como alternativa, puede establecer la opción **Permitir el acceso a servicios de Azure** como **ACTIVADO** en el portal desde el panel **Seguridad de conexión** y seleccionar **Guardar**. Si no se permite el intento de conexión, la solicitud no llega al servidor Azure Database for MariaDB.

> [!IMPORTANT]
> Esta opción configura el firewall para permitir todas las conexiones de Azure, incluidas las de las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>Administrar las reglas de firewall en Azure Portal
Repita los pasos para administrar las reglas de firewall.
* Para agregar el equipo actual, haga clic en **+ Agregar mi dirección IP**. Haga clic en **Guardar** para guardar los cambios.
* Para agregar direcciones IP adicionales, escriba el **nombre de la regla**, la **dirección IP inicial** y la **dirección IP final**. Haga clic en **Guardar** para guardar los cambios.
* Para modificar una regla existente, haga clic en cualquiera de los campos de la regla y luego realice la modificación. Haga clic en **Guardar** para guardar los cambios.
* Para eliminar una regla existente, haga clic en los puntos suspensivos […] y luego haga clic en **Eliminar**. Haga clic en **Guardar** para guardar los cambios.

## <a name="next-steps"></a>Pasos siguientes
 - De forma similar, puede utilizar scripts para [crear y administrar Azure Database for MariaDB reglas de firewall mediante la CLI de Azure](howto-manage-firewall-cli.md).
 - Proteger aún más el acceso a su servidor [reglas mediante el portal de Azure y los puntos de conexión de servicio creación y administración de red Virtual](howto-manage-vnet-portal.md).
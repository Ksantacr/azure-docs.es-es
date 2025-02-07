---
title: 'Crear y administrar reglas y puntos de conexión de servicio de red virtual para Azure Database for PostgreSQL: solo servidor mediante la CLI de Azure'
description: En este artículo se describe cómo crear y administrar los puntos de conexión de servicio de red virtual y reglas para Azure Database for PostgreSQL mediante la línea de comandos de CLI de Azure.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: df2af0240852b23203e504d8233de4af48475438
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067566"
---
# <a name="create-and-manage-vnet-service-endpoints-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Crear y administrar los puntos de conexión de servicio de red virtual para Azure Database for PostgreSQL: solo servidor mediante la CLI de Azure
Las reglas y los puntos de conexión de servicios de red virtual (VNet) amplían el espacio de direcciones privadas de una red virtual al servidor de Azure Database for PostgreSQL. Con los comandos de la interfaz de la línea de comandos (CLI) de Azure adecuados, puede crear, actualizar, eliminar, enumerar y mostrar reglas y puntos de conexión de servicio de red virtual para administrar el servidor. Para obtener información general sobre los puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL, incluidas las limitaciones, consulte [Azure Database for PostgreSQL Server VNet service endpoints](concepts-data-access-and-security-vnet.md) (Puntos de conexión de servicio de red virtual del servidor de Azure Database for PostgreSQL). Los puntos de conexión de servicio de red virtual están disponibles en todas las regiones admitidas para Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- Instale [la CLI de Azure](/cli/azure/install-azure-cli) o use Azure Cloud Shell en el explorador.
- Un [servidor y una base de datos de Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.
> En el caso de emparejamiento de VNET, si el tráfico fluye a través de una instancia de VNet Gateway común con puntos de conexión de servicio y se supone que lo hace al elemento del mismo nivel, cree una regla de ACL o red virtual para permitir que Microsoft Azure Virtual Machines en la red virtual de puerta de enlace acceda al servidor Azure Database for PostgreSQL.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Configuración de puntos de conexión de servicio de red virtual para Azure Database for PostgreSQL
Los comandos [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) se usan para configurar redes virtuales.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

Si ejecuta localmente la CLI, debe iniciar sesión en su cuenta mediante el comando [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest). Anote la propiedad **id** de la salida del comando para el nombre de la suscripción correspondiente.
```azurecli-interactive
az login
```

Si tiene varias suscripciones, elija la suscripción adecuada en la que se debe facturar el recurso. Seleccione el identificador de suscripción específico en su cuenta mediante el comando [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Sustituya la propiedad **id** de la salida **az login** para su suscripción en el marcador de posición de id. de suscripción.

- La cuenta debe tener todos los permisos necesarios para crear una red virtual y un punto de conexión de servicio.

Los puntos de conexión de servicio pueden configurarse en redes virtuales de forma independiente por un usuario con acceso de escritura a la red virtual.

Para proteger los recursos de servicio de Azure en una red virtual, el usuario debe tener permiso en "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" para las subredes que se agregan. De forma predeterminada, este permiso se incluye en los roles de administrador de servicios integrado y puede modificarse mediante la creación de roles personalizados.

Obtenga más información sobre los [roles integrados](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) y la asignación de permisos específicos a [roles personalizados](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Las redes virtuales y los recursos de servicio de Azure pueden encontrarse en la misma o en diferentes suscripciones. Si los recursos de servicio de Azure y de red virtual se encuentran en distintas suscripciones, los recursos deben estar en el mismo inquilino de Active Directory (AD).

> [!IMPORTANT]
> Se recomienda encarecidamente que lea este artículo sobre las configuraciones y las consideraciones de los puntos de conexión de servicio antes de ejecutar el script de ejemplo siguiente, o configurar los puntos de conexión de servicio. **Punto de conexión de servicio de red virtual:** un [punto de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. Los puntos de conexión de servicio de red virtual usan el nombre de tipo de servicio **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database. Esta etiqueta de servicio también se aplica a los servicios Azure SQL Database, Azure Database for PostgreSQL y MySQL. Es importante tener en cuenta que, al aplicar la etiqueta de servicio de **Microsoft.Sql** a un punto de conexión de servicio de red virtual, se configura el tráfico de punto de conexión de servicio de todos los servicios de Azure Database, incluidos los servidores de Azure SQL Database, Azure Database for PostgreSQL y Azure Database for MySQL de la subred. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Script de ejemplo para crear una base de datos de Azure Database for PostgreSQL, crear una red virtual, un punto de conexión de servicio de red virtual y proteger el servidor a la subred con una regla de red virtual
En este script de ejemplo, cambie las líneas resaltadas para personalizar el nombre de usuario de administrador y la contraseña. Reemplace el identificador de suscripción usado en el comando `az account set --subscription` por su propio identificador de suscripción.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]

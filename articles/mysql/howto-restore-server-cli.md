---
title: Copia de seguridad y restauración de un servidor en Azure Database for MySQL
description: Copia de seguridad y restauración de un servidor en Azure Database for MySQL mediante la CLI de Azure.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/01/2018
ms.openlocfilehash: f3850623f5918ea9405131edb1821b941019ac34
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66160438"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-cli"></a>Copia de seguridad y restauración de un servidor en Azure Database for MySQL mediante la CLI de Azure

## <a name="backup-happens-automatically"></a>Las copias de seguridad se realizan automáticamente
Periódicamente, se realizan copias de seguridad de los servidores de Azure Database for MySQL para habilitar las características de restauración. Con esta característica, puede restaurar el servidor y todas sus bases de datos en un servidor nuevo a un momento dado anterior.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for MySQL y una base de datos](quickstart-create-mysql-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Esta guía de procedimientos requiere el uso de la CLI de Azure versión 2.0 o posterior. Para confirmar la versión, en el símbolo del sistema de la CLI de Azure, escriba `az --version`. Para la instalación o la actualización, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Configuración de copia de seguridad

Elija la configuración del servidor para copias de seguridad con redundancia local o con redundancia geográfica en el momento de crear el servidor. 

> [!NOTE]
> Después de crear un servidor, no se puede cambiar el tipo de redundancia elegido, redundancia geográfica o redundancia local.
>

Al crear un servidor mediante el comando `az mysql server create`, el parámetro `--geo-redundant-backup` decide la opción de redundancia de copia de seguridad. Si `Enabled`, se toman las copias de seguridad con redundancia geográfica. O si `Disabled`, se toman las copias de seguridad con redundancia local. 

El período de retención de la copia de seguridad se configura mediante el parámetro `--backup-retention`. 

Para más información acerca de cómo establecer estos valores durante la creación, consulte la [guía de inicio rápido de la CLI del servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-cli.md).

El período de retención de copia de seguridad de un servidor se puede cambiar de la forma siguiente:

```azurecli-interactive
az mysql server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

En el ejemplo anterior se cambia el período de retención de copia de seguridad de mydemoserver a 10 días.

El período de retención de copia de seguridad rige durante cuánto tiempo se puede realizar una restauración a un momento dado, porque se basa en las copias de seguridad disponibles. La restauración a un momento dado se describe con más detalle en la sección siguiente.

## <a name="server-point-in-time-restore"></a>Restauración del servidor a un momento dado
Puede restaurar el servidor a un momento dado anterior. Los datos restaurados se copian en un nuevo servidor y el existente se queda tal cual. Por ejemplo, si una tabla se eliminó por error hoy a mediodía, puede restaurar hasta el momento justo antes del mediodía. Así podrá recuperar la tabla y los datos que faltan de la copia restaurada del servidor. 

Para restaurar el servidor, utilice el comando [az mysql server restore](/cli/azure/mysql/server#az-mysql-server-restore) de la CLI de Azure.

### <a name="run-the-restore-command"></a>Ejecutar el comando restore

Para restaurar el servidor, en el símbolo del sistema de la CLI de Azure, escriba el siguiente comando:

```azurecli-interactive
az mysql server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

El comando `az mysql server restore` requiere los siguientes parámetros:

| Configuración | Valor sugerido | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupo de recursos donde existe el servidor de origen.  |
| Nombre | mydemoserver-restored | Nombre del nuevo servidor que se crea mediante el comando de restauración. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Seleccione un momento dado anterior para restaurar. Esta fecha y hora debe estar dentro del período de retención de copia de seguridad del servidor de origen. Use el formato de fecha y hora ISO8601. Por ejemplo, puede usar su propia zona horaria, como `2018-03-13T05:59:00-08:00`. También puede utilizar el formato de hora Zulú UTC, por ejemplo, `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Nombre o identificador del servidor de origen desde el que se va a restaurar. |

Cuando se restaura un servidor a un momento dado anterior, se crea un servidor. El servidor de origen y las bases de datos de ese momento dado anterior se copian en el servidor nuevo.

Los valores de ubicación y plan de tarifa del servidor restaurado son los mismos que los del servidor de origen. 

Una vez finalizada la restauración, busque el servidor nuevo y compruebe que los datos se restauraron según lo previsto.

## <a name="geo-restore"></a>Restauración geográfica
Si ha configurado el servidor para copias de seguridad con redundancia geográfica, se puede crear un nuevo servidor a partir de la copia de seguridad de ese servidor existente. Este nuevo servidor puede crearse en cualquier región en la que Azure Database for MySQL esté disponible.  

Para crear un servidor con una copia de seguridad con redundancia geográfica, use el comando `az mysql server georestore` de la CLI de Azure.

> [!NOTE]
> Al crear por primera vez un servidor, puede que no esté disponible para la restauración geográfica inmediatamente. Los metadatos pueden tardar unas horas en rellenarse.
>

Para restaurar geográficamente el servidor, en el símbolo del sistema de la CLI de Azure, escriba el siguiente comando:

```azurecli-interactive
az mysql server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen5_8 
```
Este comando crea un nuevo servidor denominado *mydemoserver georestored* en la zona horaria del Este de EE. UU. que pertenecerá a *myresourcegroup*. Se trata de un servidor Gen 5 de uso general con ocho núcleos virtuales. El servidor se crea a partir de la copia de seguridad con redundancia geográfica de *mydemoserver*, que también está en el grupo de recursos *myresourcegroup*.

Si desea crear el nuevo servidor en otro grupo de recursos desde el servidor existente y, después, en el parámetro `--source-server` debería calificar el nombre del servidor como en el ejemplo siguiente:

```azurecli-interactive
az mysql server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMySQL/servers/mydemoserver" --location eastus --sku-name GP_Gen5_8

```

El comando `az mysql server georestore` requiere los siguientes parámetros:

| Configuración | Valor sugerido | DESCRIPCIÓN  |
| --- | --- | --- |
|resource-group| myresourcegroup | Nombre del grupo de recursos al que pertenece el nuevo servidor.|
|Nombre | mydemoserver-georestored | Nombre del nuevo servidor. |
|source-server | mydemoserver | Nombre del servidor existente cuyas copias de seguridad con redundancia geográfica se usan. |
|location | estado | Ubicación del nuevo servidor. |
|sku-name| GP_Gen5_8 | Este parámetro establece el plan de tarifa, la generación del proceso y el número de núcleos virtuales del nuevo servidor. GP_Gen5_8 se asigna a un servidor Gen 5 de uso general con ocho núcleos virtuales.|


>[!Important]
>Al crear un nuevo servidor mediante una restauración geográfica, hereda el mismo tamaño de almacenamiento y plan de tarifa que el servidor de origen. Estos valores no se pueden cambiar durante la creación. Después de crea el nuevo servidor, se puede escalar verticalmente su tamaño de almacenamiento.

Una vez finalizada la restauración, busque el servidor nuevo y compruebe que los datos se restauraron según lo previsto.

## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de las [copias de seguridad](concepts-backup.md) del servicio.
- Más información sobre las opciones de [continuidad del negocio](concepts-business-continuity.md).

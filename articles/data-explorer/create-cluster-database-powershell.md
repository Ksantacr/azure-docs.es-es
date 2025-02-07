---
title: 'Inicio rápido: Creación de un clúster y una base de datos de Azure Data Explorer mediante PowerShell'
description: Aprenda a crear un clúster y una base de datos de Azure Data Explorer mediante PowerShell
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/25/2019
ms.openlocfilehash: 28785bb99dcdb767a64ae977e8326b80130fb135
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240188"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>Creación de un clúster y una base de datos de Azure Data Explorer mediante PowerShell

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde aplicaciones, sitios web, dispositivos IoT, etc. Para usar Azure Data Explorer, cree primero un clúster y una o varias bases de datos en ese clúster. A continuación, ingerirá (cargará) los datos en una base de datos para que pueda ejecutar consultas en ella. En este inicio rápido, se crean un clúster y una base de datos mediante PowerShell. Los scripts y cmdlets de PowerShell se pueden ejecutar en Windows, Linux, o en [Azure Cloud Shell](../cloud-shell/overview.md) con [Az.Kusto](/powershell/module/az.kusto/?view=azps-1.4.0#kusto) para crear y configurar clústeres y bases de datos de Azure Data Explorer.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI de Azure en un entorno local, en este inicio rápido se requiere la versión 2.0.4 o posterior de la CLI de Azure. Ejecute `az --version` para comprobar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-parameters"></a>Configuración de parámetros

Los pasos siguientes no son necesarios si ejecuta comandos en Azure Cloud Shell. Si ejecuta la CLI localmente, siga los pasos 1 y 2 para iniciar sesión en Azure y establecer su suscripción actual:

1. Ejecute el siguiente comandos para iniciar sesión en Azure:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Establezca la suscripción en el lugar en que desee que se cree el clúster:

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
1. Si ejecuta la CLI de Azure localmente o en Azure Cloud Shell, es preciso que instale el módulo Az.Kusto en el dispositivo:
    
    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto  
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Creación del clúster de Azure Data Explorer

1. Cree el clúster mediante el siguiente comando:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | NOMBRE | *mykustocluster* | Nombre que quiere para el clúster.|
   | SKU | *D13_v2* | La SKU que se usará para el clúster. |
   | ResourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |

    Hay varios parámetros opcionales que puede usar, como la capacidad del clúster, etcétera.

1. Ejecute el siguiente comando para comprobar si el clúster se creó correctamente:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster -ResourceGroupName testrg
    ```

Si el resultado contiene `provisioningState` con el valor `Succeeded`, significa que el clúster se ha creado correctamente.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Creación de la base de datos en el clúster de Azure Data Explorer

1. Cree la base de datos con el siguiente comando:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | ClusterName | *mykustocluster* | Nombre del clúster donde se creará la base de datos.|
   | NOMBRE | *mykustodatabase* | Nombre de la base de datos.|
   | ResourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |
   | SoftDeletePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos estarán disponibles para consulta. |
   | HotCachePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos se conservarán en la caché. |

1. Ejecute el siguiente comando para ver la base de datos que ha creado:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster -ResourceGroupName testrg -Name mykustodatabase
    ```

Ahora cuenta con un clúster y una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

* Si tiene previsto seguir nuestros tutoriales y guías de inicio rápido, conserve los recursos que creó.
* Para limpiar los recursos, elimine el clúster. Cuando se elimina un clúster, también se eliminan todas las bases de datos en él. Use el siguiente comando para eliminar el clúster:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Comandos de Az.Kusto adicionales](/powershell/module/az.kusto/?view=azps-1.7.0#kusto)
* [Inicio rápido: Ingesta de datos mediante el SDK de .NET Standard de Azure Data Explorer (versión preliminar)](net-standard-ingest-data.md)

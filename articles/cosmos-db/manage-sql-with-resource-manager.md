---
title: Crear y administrar Azure Cosmos DB mediante plantillas de Azure Resource Manager
description: Usar plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB para SQL API (principal)
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: mjbrown
ms.openlocfilehash: 5683fd072961c7793d8f4bbeb9ecc16a93dd7373
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242587"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de la API de SQL de Azure Cosmos DB (Core) mediante plantillas de Azure Resource Manager

## Crear una cuenta de Azure Cosmos, base de datos y contenedor <a id="create-resource"></a>

Crear recursos de Azure Cosmos DB mediante una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos con dos contenedores que compartan el rendimiento de 400 RU/s en el nivel de base de datos. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

> [!NOTE]
> Actualmente no se puede implementar Functions(UDFs) definido de usuario, procedimientos almacenados y desencadenadores mediante el uso de plantillas de Resource Manager. 

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Implementar a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$container1Name = Read-Host -Prompt "Enter the first container name"
$container2Name = Read-Host -Prompt "Enter the second container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -container1Name $container1Name `
    -container2Name $container2Name

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Si decide usar una versión instalada localmente de PowerShell en lugar de desde Azure Cloud shell, deberá [instalar](/powershell/azure/install-az-ps) el módulo Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión.

### <a name="deploy-via-azure-cli"></a>Implementación a través de la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first container name: ' container1Name
read -p 'Enter the second container name: ' container2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   container1Name=$container1Name container2Name=$container2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El `az cosmosdb show` comando muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión de CLI de Azure instalada localmente en lugar de usar CloudShell, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) artículo.

## Actualización de procesamiento (RU/s) en una base de datos <a id="database-ru-update"></a>

La siguiente plantilla actualizará el rendimiento de una base de datos. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-database-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-sql-database-ru-update](~/quickstart-templates/101-cosmosdb-sql-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-powershell"></a>Implementar la plantilla de base de datos a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$throughput = Read-Host -Prompt "Enter new throughput for database"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -throughput $throughput
```

### <a name="deploy-database-template-via-azure-cli"></a>Implementar la plantilla de base de datos mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Actualización de procesamiento (RU/s) en un contenedor <a id="container-ru-update"></a>

La siguiente plantilla actualizará el procesamiento de un contenedor. Copie la plantilla e implementar tal y como se muestra a continuación o visite [Galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-ru-update/) e implementar desde el portal de Azure. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el `--template-file` parámetro.

[!code-json[cosmosdb-sql-container-ru-update](~/quickstart-templates/101-cosmosdb-sql-container-ru-update/azuredeploy.json)]

### <a name="deploy-container-template-via-powershell"></a>Implementar la plantilla de contenedor a través de PowerShell

Para implementar la plantilla de Resource Manager mediante PowerShell, **copia** el script y seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"
$throughput = Read-Host -Prompt "Enter new throughput for container"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -containerName $containerName `
    -throughput $throughput
```

### <a name="deploy-container-template-via-azure-cli"></a>Implementar la plantilla de contenedor mediante la CLI de Azure

Para implementar la plantilla de Resource Manager mediante la CLI de Azure, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName containerName=$containerName throughput=$throughput
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema de proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de Azure Cosmos DB Quickstart](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)
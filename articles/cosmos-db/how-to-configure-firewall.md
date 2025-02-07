---
title: Configure an IP firewall for your Azure Cosmos DB account (Configuración de un firewall de dirección IP para la cuenta de Azure Cosmos DB)
description: Aprenda a configurar directivas de control de acceso por IP para la compatibilidad con el firewall en cuentas de base de datos de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 24ebc7eb4c9abc72a89419611e4b4b3fa2db88b4
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241966"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Configuración del firewall de IP en Azure Cosmos DB

Puede proteger los datos almacenados en su cuenta de Azure Cosmos DB mediante el uso de firewalls de dirección IP. Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Puede establecer un firewall de dirección IP en la cuenta de Azure Cosmos DB mediante una de las maneras siguientes:

* Desde Azure Portal
* Mediante declaración al usar una plantilla de Azure Resource Manager
* Mediante programación a través de la CLI de Azure o Azure PowerShell al actualizar la propiedad **ipRangeFilter**

## <a id="configure-ip-policy"></a> Configuración de un firewall de dirección IP mediante Azure Portal

Para establecer la directiva de control de acceso de IP en Azure Portal, vaya a la página de la cuenta de Azure Cosmos DB y seleccione **Firewall y redes virtuales** en el menú de navegación. Cambie el valor **Permitir acceso desde** por **Redes seleccionadas** y, a continuación, seleccione **Guardar**. 

![Captura de pantalla que muestra cómo abrir la página Firewall en Azure Portal](./media/how-to-configure-firewall/azure-portal-firewall.png)

Cuando el control de acceso de IP se activa, Azure Portal proporciona la capacidad para especificar direcciones IP, intervalos de direcciones IP y modificadores. Los modificadores permiten el acceso a otros servicios de Azure y a Azure Portal. En las secciones siguientes se proporcionan detalles acerca de estos modificadores.

> [!NOTE]
> Después de habilitar una directiva de control de acceso de dirección IP para su cuenta de Azure Cosmos DB, se rechazan todas las solicitudes a la cuenta de Azure Cosmos DB desde máquinas fuera de la lista permitida de rangos de direcciones IP. La exploración de los recursos de Azure Cosmos DB desde el portal también se bloquea para garantizar la integridad del control de acceso.

### <a name="allow-requests-from-the-azure-portal"></a>Permitir las solicitudes desde Azure Portal

Cuando se habilita una directiva de control de acceso de IP mediante programación, debe agregar la dirección IP correspondiente a la propiedad **ipRangeFilter** de Azure Portal para mantener el acceso. Las direcciones IP del portal son las siguientes:

|Region|Dirección IP|
|------|----------|
|Alemania|51.4.229.218|
|China|139.217.8.252|
|Gobierno de EE. UU.|52.244.48.71|
|Todas las demás regiones|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Para habilitar el acceso a Azure Portal, seleccione la opción **Permitir el acceso desde Azure Portal**, tal como se muestra en la captura de pantalla siguiente: 

![Captura de pantalla que muestra cómo habilitar el acceso a Azure Portal](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Permitir las solicitudes de los centros de datos globales de Azure u otros orígenes dentro de Azure

Si tiene acceso a su cuenta de Azure Cosmos DB desde los servicios que no proporcionan un IP estático (por ejemplo, Azure Stream Analytics y Azure Functions), aún podrá usar el firewall de dirección IP para limitar el acceso. Para permitir el acceso a la cuenta de Azure Cosmos DB desde estos servicios, agregue la dirección IP 0.0.0.0 a la lista de direcciones IP permitidas. La dirección 0.0.0.0 restringe las solicitudes a la cuenta de Azure Cosmos DB desde el intervalo IP del centro de datos de Azure. Esta configuración no permite que ningún otro intervalo IP tenga acceso a la cuenta de Azure Cosmos DB.

> [!NOTE]
> Esta opción configura el firewall para permitir todas las solicitudes de Azure, incluidas las solicitudes de las suscripciones de otros clientes implementados en Azure. La lista de IP permitidos por esta opción es amplia, por lo que limita la eficacia de una directiva de firewall. Use esta opción solo si las solicitudes no se originan en IP estáticos ni subredes de redes virtuales. Si se elige esta opción, se permite automáticamente el acceso desde Azure Portal, ya que Azure Portal se implementa en Azure.

Para habilitar el acceso a Azure Portal, seleccione la opción **Aceptar conexiones desde centros de datos públicos de Azure**, tal como se muestra en la captura de pantalla siguiente: 

![Captura de pantalla que muestra cómo abrir la página Firewall en Azure Portal](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Solicitudes desde la dirección IP actual

Para simplificar el desarrollo, Azure Portal le ayuda a identificar y agregar la dirección IP de su equipo cliente a la lista de permitidos. Las aplicaciones que ejecutan su máquina, a continuación, pueden obtener acceso a su cuenta de Azure Cosmos DB. 

El portal detecta automáticamente la dirección IP del cliente. Puede que sea la dirección IP cliente de la máquina, pero también la dirección IP de la puerta de enlace de red. Asegúrese de quitar esta dirección IP antes de llevar las cargas de trabajo a producción. 

Para agregar su dirección IP actual a la lista de direcciones IP, seleccione **Agregar mi IP actual**. Después, seleccione **Guardar**.

![Captura de pantalla que muestra cómo configurar un firewall para la dirección IP actual](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Solicitudes desde servicios en la nube

En Azure, los servicios en la nube son una forma muy común de hospedar la lógica de servicio de nivel intermedio mediante Azure Cosmos DB. Para permitir el acceso a su cuenta de Azure Cosmos DB desde un servicio en la nube, debe agregar la dirección IP pública del servicio en la nube a la lista de direcciones IP permitida asociada a dicha cuenta de Azure Cosmos DB mediante la [configuración de la directiva de control de acceso a la dirección IP](#configure-ip-policy). De esta forma se garantiza que todas las instancias de rol de servicios en la nube tengan acceso a la cuenta de Azure Cosmos DB. 

Puede recuperar las direcciones IP de los servicios en la nube en Azure Portal, tal como se muestra en la siguiente captura de pantalla:

![Captura de pantalla con la dirección IP pública de un servicio en la nube que se muestra en Azure Portal](./media/how-to-configure-firewall/public-ip-addresses.png)

Al escalar horizontalmente su servicio en la nube mediante la adición de instancias de rol, esas nuevas instancias tendrán acceso automáticamente a la cuenta de Azure Cosmos DB que forman parte del mismo servicio en la nube.

### <a name="requests-from-virtual-machines"></a>Solicitudes desde máquinas virtuales

También puede usar [máquinas virtuales](https://azure.microsoft.com/services/virtual-machines/) o [conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para hospedar servicios de nivel intermedio mediante Azure Cosmos DB. Para configurar la cuenta de Cosmos DB para que permita el acceso desde máquinas virtuales, debe configurar la dirección IP pública de la máquina virtual o el conjunto de escalado de máquinas virtuales como una de las direcciones IP permitidas para la cuenta de Azure Cosmos DB mediante la [configuración de la directiva de control de acceso IP](#configure-ip-policy). 

Puede recuperar las direcciones IP para máquinas virtuales en Azure Portal, como se muestra en la captura de pantalla siguiente:

![Captura de pantalla con una dirección IP pública de una máquina virtual que se muestra en Azure Portal](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Al agregar instancias de máquina virtual al grupo, reciben automáticamente acceso a su cuenta de Azure Cosmos DB.

### <a name="requests-from-the-internet"></a>Solicitudes desde Internet

Cuando se obtiene acceso a la cuenta de Azure Cosmos DB desde un equipo de Internet, la dirección IP del cliente o el intervalo de direcciones IP de la máquina se debe agregar a la lista de direcciones IP permitida para dicha cuenta.

## <a id="configure-ip-firewall-arm"></a>Configuración de un firewall de dirección IP mediante una plantilla de Resource Manager

Para configurar el control de acceso a su cuenta de Azure Cosmos DB, asegúrese de que la plantilla de Resource Manager especifica el atributo **ipRangeFilter** con una lista de intervalos IP permitidos. Por ejemplo, agregue el siguiente código JSON a la plantilla:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
       "databaseAccountOfferType": "Standard",
       "name": "[parameters('databaseAccountName')]",
       "ipRangeFilter":"183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
     }
   }
```

## <a id="configure-ip-firewall-cli"></a>Configuración de una directiva de control de acceso IP con la CLI de Azure

El comando siguiente muestra cómo crear una cuenta de Azure Cosmos DB con control de acceso por IP: 

```azurecli-interactive

name="<Azure Cosmos DB account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

Para actualizar la configuración de firewall para una cuenta existente, ejecute el siguiente comando:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="configure-ip-firewall-ps"></a>Configuración de una directiva de control de acceso IP mediante PowerShell

En el script siguiente se muestra cómo crear una cuenta de Azure Cosmos DB con control de acceso IP:

```azurepowershell-interactive

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

# Add local machine's IP address to firewall, InterfaceAlias is your Network Adapter's name
$ipRangeFilter = Get-NetIPConfiguration | Where-Object InterfaceAlias -eq "Ethernet 2" | Select-Object IPv4Address

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "ipRangeFilter"=$ipRangeFilter
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="troubleshoot-ip-firewall"></a>Solución de problemas con una directiva de control de acceso por IP

Puede solucionar problemas con una directiva de control de acceso por IP mediante las siguientes opciones: 

### <a name="azure-portal"></a>Portal de Azure 
Al habilitar una directiva de control de acceso por IP para su cuenta de Azure Cosmos DB, bloquea todas las solicitudes a la cuenta desde máquinas fuera de la lista permitida de rangos de direcciones IP. Para habilitar las operaciones en el plano de los datos del portal, como explorar los contenedores y consultar los documentos, debe permitir expresamente el acceso a Azure Portal mediante el panel **Firewall** del portal.

### <a name="sdks"></a>SDK 
Cuando tenga acceso a recursos de Azure Cosmos DB mediante SDK desde máquinas que no están en la lista de permitidos, se devolverá una respuesta genérica**403 Prohibido** sin más detalles. Compruebe la lista de direcciones IP permitidas para su cuenta y asegúrese de que se aplique la configuración de directiva correcta a su cuenta de Azure Cosmos DB. 

### <a name="source-ips-in-blocked-requests"></a>Direcciones IP de origen en solicitudes bloqueadas
Habilite el registro de diagnóstico en su cuenta de Azure Cosmos DB. Estos registros muestran cada solicitud y respuesta. Los mensajes relacionados con el firewall se registran con un código de retorno 403. Al filtrar estos mensajes, puede ver las direcciones IP de origen para las solicitudes bloqueadas. Consulte [Registro de diagnóstico de Azure Cosmos DB](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Solicitudes de una subred con un punto de conexión de servicio para la base de datos Azure Cosmos DB habilitada
Las solicitudes de una subred en una red virtual que tiene habilitado un punto de conexión de servicio para Azure Cosmos DB envían la identidad de la red virtual y la subred a las cuentas de Azure Cosmos DB. Estas solicitudes no tienen la dirección IP pública del origen, de modo que los filtros de IP las rechazan. Para permitir el acceso desde subredes específicas en redes virtuales, agregue una lista de control de acceso que se describe en [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md). Las reglas de firewall pueden tardar hasta 15 minutos en aplicarse.


## <a name="next-steps"></a>Pasos siguientes

Para configurar un punto de conexión de servicio de red virtual para la cuenta de Azure Cosmos DB, consulte los artículos siguientes:

* [Control de acceso a una red virtual y subred para la cuenta de Azure Cosmos DB](vnet-service-endpoint.md)
* [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md)


---
title: 'Configurar agentes de escucha de grupo de disponibilidad AlwaysOn: Microsoft Azure | Microsoft Docs'
description: Configure agentes de escucha de grupo de disponibilidad en el modelo de Azure Resource Manager mediante un equilibrador de carga interno con una o varias direcciones IP.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/06/2019
ms.author: mikeray
ms.openlocfilehash: 5b647af7925ceb81c524deb0accf90f9e895080e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66165805"
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Configuración de uno o varios agentes de escucha de grupo de disponibilidad AlwaysOn: Resource Manager
En este tema se muestra cómo llevar a cabo dos tareas:

* Crear un equilibrador de carga interno para grupos de disponibilidad de SQL Server mediante cmdlets de PowerShell
* Agregar más direcciones IP a un equilibrador de carga para admitir más de un grupo de disponibilidad 

Un agente de escucha del grupo de disponibilidad es un nombre de red virtual al que se conectan los clientes para acceder a la base de datos. En las máquinas virtuales de Azure, un equilibrador de carga contiene la dirección IP del agente de escucha. El equilibrador de carga enruta el tráfico a la instancia de SQL Server que está escuchando en el puerto de sondeo. Normalmente, un grupo de disponibilidad usa un equilibrador de carga interno. Un equilibrador de carga interno de Azure puede hospedar una o varias direcciones IP. Cada una usa un puerto de sondeo específico. En este documento, se muestra cómo usar PowerShell para crear un equilibrador de carga o para agregar direcciones IP a un equilibrador de carga existente para grupos de disponibilidad de SQL Server. 

La capacidad para asignar varias direcciones IP a un equilibrador de carga interno es nueva en Azure y solo está disponible en el modelo de Resource Manager. Para completar esta tarea, debe tener un grupo de disponibilidad de SQL Server implementado en las máquinas virtuales de Azure con el modelo de Resource Manager. Las dos máquinas virtuales de SQL Server deben pertenecer al mismo conjunto de disponibilidad. Puede usar la [plantilla de Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) para crear automáticamente el grupo de disponibilidad en Azure Resource Manager. Esta plantilla crea automáticamente el grupo de disponibilidad, incluido el equilibrador de carga interno. Si lo prefiere, puede [configurar manualmente un grupo de disponibilidad AlwaysOn](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

En este tema, es necesario que los grupos de disponibilidad ya estén configurados.  

Temas relacionados:

* [Configuración de Grupos de disponibilidad AlwaysOn en la máquina virtual de Azure (GUI)](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Configuración de una conexión entre dos redes virtuales mediante Azure Resource Manager y PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="verify-powershell-version"></a>Verificación de la versión de PowerShell

Los ejemplos de este artículo se prueban con la versión 5.4.1 del módulo de Azure PowerShell.

Verifique que el módulo de PowerShell sea 5.4.1 o posterior.

Consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="configure-the-windows-firewall"></a>Configuración de Firewall de Windows

Configure Firewall de Windows para permitir el acceso de SQL Server. Las reglas de firewall permiten las conexiones TCP para el uso de puertos por la instancia de SQL Server, así como el sondeo del agente de escucha. Para instrucciones detalladas, consulte [Configurar Firewall de Windows para el acceso al motor de base de datos](https://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Cree una regla de entrada para el puerto de SQL Server y para el puerto de sondeo.

Si restringe el acceso con Azure Network Security Group, asegúrese de que las reglas de permiso incluyan las direcciones IP de back-end de VM con SQL Server, y direcciones IP flotantes del equilibrador de carga para la escucha de grupo de disponibilidad y la dirección IP principal del clúster, si corresponde.

## <a name="determine-the-load-balancer-sku-required"></a>Determinar la SKU requerida del equilibrador de carga

[Azure Load Balancer](../../../load-balancer/load-balancer-overview.md) está disponible en 2 SKU: Básico y Estándar. Se recomienda el equilibrador de carga estándar. Si las máquinas virtuales están en un conjunto de disponibilidad, se permite el equilibrador de carga básico. El equilibrador de carga estándar requiere que todas las direcciones IP de las VM usen direcciones IP estándar.

La [plantilla de Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) actual para un grupo de disponibilidad usa un equilibrador de carga básico con direcciones IP básicas.

En los ejemplos de este artículo se especifica un equilibrador de carga estándar. En los ejemplos, el script incluye `-sku Standard`.

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe -sku Standard
```

Para crear un equilibrador de carga básico, quite `-sku Standard` de la línea que crea el equilibrador de carga. Por ejemplo:

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe
```

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Script de ejemplo: creación de un equilibrador de carga interno con PowerShell

> [!NOTE]
> Si ha creado el grupo de disponibilidad con la [plantilla Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), ya se ha creado el equilibrador de carga interno.

El siguiente script de PowerShell crea un equilibrador de carga interno, configura las reglas de equilibrio de carga y establece una dirección IP para el equilibrador de carga. Para ejecutar el script, abra Windows PowerShell ISE y pegue el script en el panel Script. Use `Connect-AzAccount` para iniciar sesión en PowerShell. Si tiene varias suscripciones de Azure, puede usar `Select-AzSubscription` para establecer la suscripción. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzNetworkInterface -NetworkInterface $NIC
        start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a> Script de ejemplo: agregar una dirección IP a un equilibrador de carga existente con PowerShell
Para utilizar más de un grupo de disponibilidad, agregue una dirección IP adicional al equilibrador de carga. Cada dirección IP requiere su regla de equilibrio de carga, puerto de sondeo y puerto de front-end propios.

El puerto de front-end es el que las aplicaciones usan para conectarse a la instancia de SQL Server. Las direcciones IP para los diferentes grupos de disponibilidad pueden usar el mismo puerto de front-end.

> [!NOTE]
> Para los grupos de disponibilidad de SQL Server, cada dirección IP requiere un puerto de sondeo específico. Por ejemplo, si una dirección IP en un equilibrador de carga utiliza el puerto de sondeo 59999, no puede usarlo ninguna otra dirección IP de ese equilibrador de carga.

* Para información sobre los límites del equilibrador de carga, consulte **IP de front-end privada por equilibrador de carga** en [Límites de redes - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Para información acerca de los límites de los grupos de disponibilidad, consulte [Restricciones (grupos de disponibilidad)](https://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

El script siguiente agrega una nueva dirección IP a un equilibrador de carga existente. El equilibrador de carga interno usa el puerto del agente de escucha para el puerto de front-end de equilibrio de carga. Este puerto puede ser aquel en el que SQL Server escucha. Para las instancias predeterminadas de SQL Server, se trata del puerto 1433. La regla de equilibrio de carga para un grupo de disponibilidad requiere una dirección IP flotante (Direct Server Return), así que el puerto de back-end es el mismo que el puerto de front-end. Actualice las variables para su entorno. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzLoadBalancer 

$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzLoadBalancer   
```

## <a name="configure-the-listener"></a>Configuración del agente de escucha

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>Establecimiento del puerto del agente de escucha en SQL Server Management Studio

1. Abra SQL Server Management Studio y conéctese a la réplica principal.

1. Vaya a **Alta disponibilidad de AlwaysOn** | **Grupos de disponibilidad** | **Agentes de escucha del grupo de disponibilidad**. 

1. Ahora tienes que ver el nombre del agente de escucha que creaste en el Administrador de clústeres de conmutación por error. Haga clic con el botón derecho en el nombre del agente de escucha y luego en **Propiedades**.

1. En el cuadro **Puerto**, especifique el número de puerto del agente de escucha del grupo de disponibilidad mediante el valor $EndpointPort que ha establecido antes (1433 era el valor predeterminado) y haga clic en **Aceptar**.

## <a name="test-the-connection-to-the-listener"></a>Comprobación de la conexión con el agente de escucha

Para probar la conexión:

1. Conéctese con RDP a un servidor SQL Server que esté en la misma red virtual, pero que no sea el propietario de la réplica; por ejemplo, a otro servidor SQL Server del clúster.

1. Use la utilidad **sqlcmd** para probar la conexión. Por ejemplo, el siguiente script establece una conexión **sqlcmd** con la réplica principal por medio del agente de escucha con autenticación de Windows:
   
    ```
    sqlcmd -S <listenerName> -E
    ```
   
    Si el agente de escucha usa un puerto distinto del predeterminado (1433), especifíquelo en la cadena de conexión. Por ejemplo, el siguiente comando sqlcmd se conecta a un agente de escucha en el puerto 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

La conexión SQLCMD se establece automáticamente con la instancia de SQL Server en la que se hospede la réplica principal. 

> [!NOTE]
> Asegúrese de que el puerto especificado esté abierto en el firewall de los dos servidores SQL Server. En estos dos servidores, es necesario definir una regla de entrada para el puerto TCP. Consulte [Agregar o editar regla de firewall](https://technet.microsoft.com/library/cc753558.aspx) para más información. 
> 
> 

## <a name="guidelines-and-limitations"></a>Pautas y limitaciones
Cuando utilice un equilibrador de carga interno, tenga en cuenta las siguientes instrucciones que se aplican al agente de escucha del grupo de disponibilidad de Azure:

* Como solo hay un equilibrador de carga interno, el acceso al agente de escucha se realizará desde la misma red virtual.

* Si restringe el acceso con Azure Network Security Group, asegúrese de que las reglas de permiso incluyan las direcciones IP de back-end de VM con SQL Server, y direcciones IP flotantes del equilibrador de carga para la escucha de grupo de disponibilidad y la dirección IP principal del clúster, si corresponde.

## <a name="for-more-information"></a>Para obtener más información
Para más información, consulte [Configuración manual de grupos de disponibilidad AlwaysOn en máquinas virtuales de Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

## <a name="powershell-cmdlets"></a>Cmdlets de PowerShell
Use los siguientes cmdlets de PowerShell para crear un equilibrador de carga interno para máquinas virtuales de Azure.

* [New-AzLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) crea un equilibrador de carga. 
* [New-AzLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) crea una configuración IP de front-end para un equilibrador de carga. 
* [New-AzLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) crea una configuración de regla para un equilibrador de carga. 
* [New-AzLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) crea una configuración de grupo de direcciones de back-end para un equilibrador de carga. 
* [New-AzLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) crea una configuración de sondeo para un equilibrador de carga.
* [Remove-AzLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) quita un equilibrador de carga de un grupo de recursos de Azure.

---
title: Escalado o reducción horizontal de un clúster de Service Fabric | Microsoft Docs
description: Escalar horizontalmente o de un clúster de Service Fabric para satisfacer la demanda configurando reglas de escalado automático para cada conjunto de escalado de máquinas virtuales/tipo de nodo. Incorporación o eliminación de nodos de un clúster de Service Fabric
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2019
ms.author: aljo
ms.openlocfilehash: 400e4653800d445506d4854e70034a707dcc4629
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66161800"
---
# <a name="scale-a-cluster-in-or-out"></a>Escalar o reducir un clúster horizontalmente

> [!WARNING]
> Lea esta sección antes de escalar

Escalar los recursos de proceso para obtener la carga de trabajo de la aplicación requiere una planificación intencional; esta planificación necesitará casi siempre más de una hora para completarse en un entorno de producción, y requiere que comprenda su carga de trabajo y contexto comercial. Es más, si nunca antes ha realizado esta actividad, le recomendamos que empiece leyendo las [consideraciones de planificación de la capacidad del clúster de Service Fabric](service-fabric-cluster-capacity.md), antes de continuar con el resto de este documento. Esta recomendación es para evitar problemas imprevistos de LiveSite; también se recomienda que pruebe las operaciones que decida realizar en un entorno que no sea de producción. Puede notificar [problemas en el entorno de producción o bien solicite soporte técnico de pago para Azure](service-fabric-support.md#report-production-issues-or-request-paid-support-for-azure) en cualquier momento. En cuanto a los ingenieros asignados para realizar estas operaciones que poseen el contexto apropiado, en este artículo se describen las operaciones de escalado, pero debe decidir qué operaciones son apropiadas para su caso de uso; por ejemplo, debe saber qué recursos escalar (CPU, almacenamiento, memoria), qué dirección de escalar (vertical u horizontal) y qué operaciones realizar (implementación de plantillas de recursos, Portal, PowerShell/CLI).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Escalado o reducción horizontal de un clúster de Service Fabric mediante reglas de escalado automático o manualmente
Los conjuntos de escalas de máquinas virtuales son un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo que se define en un clúster de Service Fabric está configurado como un conjunto de escalado de máquinas virtuales independiente. Cada tipo de nodo se puede escalar o reducir horizontalmente de forma independiente. Cada uno cuenta con diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. Obtenga más información al respecto en el [tipos de nodo de Service Fabric](service-fabric-cluster-nodetypes.md) documento. Puesto que los tipos de nodo de Service Fabric en el clúster están formados por conjuntos de escalado de máquinas virtuales en el back-end, deberá configurar las reglas de escalado automático para cada conjunto de escalado de máquinas virtuales/tipo de nodo.

> [!NOTE]
> La suscripción debe contar con núcleos suficientes para agregar las nuevas máquinas virtuales que compondrán este clúster. En estos momentos, no hay ningún ninguna validación del modelo, así que si se alcanza algún límite de cuota, se producirá un error de tiempo de implementación. Asimismo, un tipo de nodo único no puede superar sin más 100 nodos por conjunto de escalado de máquinas virtuales. Debe agregar conjuntos de escalado de máquinas virtuales para lograr la escala de destino, y el escalado automático no puede agregar automáticamente los conjuntos de escalado de máquinas virtuales. La adición de conjuntos de escalado de máquinas virtuales locales en un clúster en vivo es una tarea difícil y, normalmente, provoca que los usuarios aprovisionen nuevos clústeres con los tipos de nodo adecuados en el momento de la creación. Por ello, debe [planear la capacidad del clúster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) en consecuencia. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Selección del tipo de nodo o conjunto de escalado de máquinas virtuales para escalar
Actualmente, no es posible especificar reglas de escalado automático para conjuntos de escalado de máquinas virtuales mediante el portal para crear un clúster de Service Fabric, por lo que vamos a usar Azure PowerShell (1.0 +) para mostrar los tipos de nodo y luego agregarles reglas de escalado automático.

Para la lista de conjuntos de escalado de máquinas virtuales que componen el clúster, ejecute los siguientes cmdlets:

```powershell
Get-AzResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzVmss -ResourceGroupName <RGname> -VMScaleSetName <virtual machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Conjunto de reglas de escalado automático para el conjunto de escalado de máquinas virtuales/tipo de nodo
Si el clúster tiene varios tipos de nodo, repita que este procedimiento para cada escalado de máquinas virtuales y los tipos de nodo establece que desea escalar (entrada o salida). Antes de configurar el escalado automático, tenga en cuenta el número de nodos que debe tener. El número mínimo de nodos que debe tener para el tipo de nodo principal está controlado por el nivel de confiabilidad que haya elegido. Más información sobre los [niveles de confiabilidad](service-fabric-cluster-capacity.md).

> [!NOTE]
> Reducir verticalmente el tipo de nodo principal a un número inferior al mínimo hará que el clúster sea inestable o que se desactive. Como consecuencia, se puede producir la pérdida de datos de las aplicaciones y los servicios del sistema.
> 
> 

Actualmente, la característica de escalado automático no depende de las cargas que las aplicaciones pueden notificar a Service Fabric. En este momento, se controla mediante los contadores de rendimiento que emiten cada una de las instancias del conjunto de escalado de máquinas virtuales.  

Siga estas instrucciones [para configurar el escalado automático para cada conjunto de escalado de máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> En un escenario de reducción vertical, a menos que el tipo de nodo tenga un [nivel de durabilidad] [ durability] Gold o Silver debe llamar a la [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) con el nombre de nodo adecuado. La durabilidad Bronze, no se recomienda para reducir verticalmente más de un nodo a la vez.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Agregar manualmente las máquinas virtuales a un conjunto de escalado de máquinas virtuales/tipo de nodo

Al escalar horizontalmente, agrega más instancias de máquina virtual al conjunto de escalado. Estas instancias se convierten en los nodos que usa Service Fabric. Service Fabric sabe cuándo el conjunto de escalado tiene varias instancias agregadas (mediante la escalabilidad horizontal) y reacciona automáticamente. 

> [!NOTE]
> Adición de máquinas virtuales lleva tiempo, por lo que no esperan que las adiciones que sea instantáneo. Por lo que se va a agregar capacidad con antelación para permitir más de 10 minutos antes de la capacidad de máquina virtual esté disponible para las instancias de servicio o réplicas colocar.
> 

### <a name="add-vms-using-a-template"></a>Agregar máquinas virtuales con una plantilla
Siga las instrucciones de ejemplo o en el [Galería de plantillas de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) para cambiar el número de máquinas virtuales en cada tipo de nodo. 

### <a name="add-vms-using-powershell-or-cli-commands"></a>Agregar máquinas virtuales mediante comandos de PowerShell o CLI
El código siguiente obtiene un conjunto de escalado por el nombre y aumenta la **capacidad** de dicho conjunto en 1.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Este código establece la capacidad en 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="manually-remove-vms-from-a-node-typevirtual-machine-scale-set"></a>Quitar manualmente las máquinas virtuales de un conjunto de escalado de máquinas virtuales/tipo de nodo
Al escalar en un tipo de nodo, quite instancias de máquina virtual del conjunto de escalado. Si el tipo de nodo es el nivel de durabilidad bronce, Service Fabric es consciente de lo que ha sucedido y se informa de que un nodo ha desaparecido. Service Fabric informa entonces de un estado incorrecto del clúster. Para evitar ese estado incorrecto, explícitamente debe quitar el nodo del clúster y quitar el estado del nodo.

Los servicios de sistema de service fabric se ejecutan en el tipo de nodo principal del clúster. Al reducir verticalmente el tipo de nodo principal, nunca reducir verticalmente el número de instancias a menos de lo que el [nivel de confiabilidad](service-fabric-cluster-capacity.md) lo garantiza. 
 
Para los servicios con estado, necesita que un determinado número de nodos estén siempre activos para mantener la disponibilidad y preservar el estado del servicio. Como mínimo, necesita que el número de nodos sea igual al recuento de conjuntos de réplicas de destino de la partición o el servicio.

### <a name="remove-the-service-fabric-node"></a>Eliminación del nodo de Service Fabric

Los pasos para quitar manualmente el estado del nodo se aplican solo a los tipos de nodo con un *bronce* nivel de durabilidad.  Para *Silver* y *Gold* nivel de durabilidad, estos pasos se realizan automáticamente la plataforma. Para obtener más información sobre la durabilidad, consulte [planeamiento de capacidad del clúster de Service Fabric][durability].

Para mantener los nodos del clúster distribuidos uniformemente entre los dominios de actualización y error y, por lo tanto, permitir su uso homogéneo, primero se debe quitar el nodo creado más recientemente. En otras palabras, los nodos se deben quitar en orden inverso al que se crearon. El nodo creado más recientemente es aquel con el valor de propiedad `virtual machine scale set InstanceId` más grande. Los ejemplos de código siguientes devuelven el nodo creado más recientemente.

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

El clúster de Service Fabric debe saber que este nodo se va a quitar. Debe realizar tres pasos:

1. Deshabilite el nodo para que ya no se realice ninguna réplica de los datos.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Detenga el nodo para que el entorno de ejecución de Service Fabric se cierre sin problemas y para que la aplicación obtenga una solicitud de finalización.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Quite el nodo del clúster.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Una vez que estos tres pasos se han aplicado al nodo, este último se puede quitar del conjunto de escalado. Si utiliza cualquier nivel de durabilidad además de [bronce][durability], estos pasos se realizan para cuando se quita la instancia del conjunto de escalado.

El bloque de código siguiente obtiene el último nodo creado, deshabilita, detiene y quita el nodo del clúster.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

En el código de **sfctl** siguiente, el comando siguiente se usa para obtener el valor de **node-name** del último nodo creado: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Use las siguientes consultas de **sfctl** para comprobar el estado de cada paso
>
> **Comprobar el estado de desactivación**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Comprobar el estado de detención**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>Reducción horizontal del conjunto de escalado

Ahora que ya se ha quitado el nodo de Service Fabric del clúster, se puede reducir horizontalmente el conjunto de escalado de máquinas virtuales. En el ejemplo siguiente, la capacidad del conjunto de escalado se reduce en 1.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Este código establece la capacidad en 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Comportamientos que se pueden observar en Service Fabric Explorer
Al escalar verticalmente un clúster, Service Fabric Explorer refleja el número de nodos (instancias del conjunto de escalado de máquinas virtuales) que forman parte del clúster.  Sin embargo, al reducir un clúster horizontalmente verá que el nodo o la instancia de máquina virtual quitados se mostrarán con un estado incorrecto, a menos que llame a [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) con el nombre de nodo apropiado.   

Esta es la explicación de este comportamiento.

Los nodos que se muestran en Service Fabric Explorer son el reflejo de lo que los servicios del sistema de Service Fabric (en concreto FM) saben acerca del número de nodos que tiene o ha tenido el clúster. Al reducir verticalmente el conjunto de escalado de máquinas virtuales, la máquina virtual se elimina pero el servicio del sistema FM sigue pensando que el nodo (que se asignó a la máquina virtual que se ha eliminado) volverá. Por lo tanto, Service Fabric Explorer sigue mostrando el nodo (aunque el estado de mantenimiento puede indicar error o desconocido).

Para tener la seguridad de que un nodo se elimina cuando una máquina virtual se elimina, tiene dos opciones:

1. Elija un nivel de durabilidad Gold o Silver para los tipos de nodo del clúster. Esto permitirá realizar la integración de la infraestructura. Con esto, a su vez, se eliminan automáticamente los nodos del estado de nuestros servicios del sistema (FM) cuando reduzca verticalmente.
Vea [los detalles sobre los niveles de durabilidad aquí](service-fabric-cluster-capacity.md).

2. Cuando haya reducido verticalmente la instancia de máquina virtual, necesitará llamar al [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> Los clústeres de Service Fabric requieren que un cierto número de nodos estén activos en todo momento con el fin de mantener la disponibilidad y conservar el estado (esto se conoce como "mantenimiento del cuórum"). Por lo tanto, normalmente no es seguro apagar todas las máquinas del clúster, a menos que antes haya realizado una [copia de seguridad completa del estado](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Lea la información siguiente para aprender sobre el planeamiento de la capacidad del clúster, la actualización de un clúster y los servicios de creación de particiones:

* [Consideraciones de planeación de capacidad del clúster de Service Fabric](service-fabric-cluster-capacity.md)
* [Actualización de un clúster de Service Fabric](service-fabric-cluster-upgrade.md)
* [Partición de Reliable Services de Service Fabric](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster

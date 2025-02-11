---
title: Actualizar y reiniciar los nodos de Linux con kured en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo actualizar los nodos de Linux y reiniciarlas automáticamente con kured en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: 1702d9558e27452006a2f015fd3312ac19362871
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849868"
---
# <a name="apply-security-and-kernel-updates-to-linux-nodes-in-azure-kubernetes-service-aks"></a>Aplicar actualizaciones de kernel y de seguridad a los nodos de Linux en Azure Kubernetes Service (AKS)

Para proteger los clústeres, las actualizaciones de seguridad se aplican automáticamente a los nodos de Linux en AKS. Estas actualizaciones incluyen las revisiones de seguridad del sistema operativo o las actualizaciones del kernel. Algunas de estas actualizaciones requieren un reinicio del nodo para completar el proceso. AKS no reiniciar automáticamente estos nodos de Linux para completar el proceso de actualización.

El proceso para mantener actualizados los nodos de Windows Server (actualmente en versión preliminar de AKS) es un poco diferente. Nodos de Windows Server no reciben actualizaciones diarias. En su lugar, realice una actualización AKS que implementa nuevos nodos con la imagen de Windows Server base más reciente y las revisiones. Para clústeres AKS que usan los nodos de Windows Server, vea [actualizar un grupo de nodos de AKS][nodepool-upgrade].

Este artículo muestra cómo usar el código abierto [kured (demonio de reinicio de KUbernetes)] [ kured] ver para los nodos de Linux que requieren un reinicio, a continuación, controlar automáticamente la reprogramación de pods y nodos en ejecución Reinicie el proceso.

> [!NOTE]
> `Kured` es un proyecto de código abierto de Weaveworks. La asistencia para este proyecto en AKS se proporciona dentro de lo posible. Soporte técnico adicional puede encontrarse en el canal de slack #weave-Comunidad.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También necesita la CLI de Azure versión 2.0.59 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Información sobre la experiencia de actualización del nodo AKS

En un clúster de AKS, los nodos de Kubernetes se ejecutan como máquinas virtuales de Azure. Estas máquinas virtuales basadas en Linux, usan una imagen de Ubuntu, con el sistema operativo configurado para buscar automáticamente actualizaciones cada noche. Si hay disponibles actualizaciones de kernel o de seguridad, estas se descargan e instalan automáticamente.

![Actualización de nodo AKS y proceso de reinicio con Kured](media/node-updates-kured/node-reboot-process.png)

Algunas actualizaciones de seguridad, como las actualizaciones del kernel, requieren un reinicio del nodo para finalizar el proceso. Un nodo de Linux que requiere un reinicio crea un archivo denominado */var/run/reboot-required*. Este proceso de reinicio no se realiza automáticamente.

Puede usar sus propios flujos de trabajo y procesos para controlar los reinicios de nodo, o usar `kured` para coordinar el proceso. Con `kured`, un [DaemonSet] [ DaemonSet] implementada que ejecuta un pod en cada nodo del clúster de Linux. Estos pods en el DaemonSet comprueban si existe el archivo */var/run/reboot-required* y, a continuación, inician un proceso para reiniciar los nodos.

### <a name="node-upgrades"></a>Actualizaciones de nodo

Hay un proceso adicional en AKS que le permite *actualizar* un clúster. Una actualización consiste generalmente en el cambio a una versión más reciente de Kubernetes, no solo en aplicar las actualizaciones de seguridad de nodo. Una actualización de AKS realiza las acciones siguientes:

* Se implementa un nuevo nodo con las últimas actualizaciones de seguridad y se aplica la versión de Kubernetes.
* Se acordona y vacía un nodo anterior.
* Los pods se programan en el nuevo nodo.
* El nodo anterior se elimina.

No puede permanecer en la misma versión de Kubernetes durante un evento de actualización. Tiene que especificar una versión más reciente de Kubernetes. Para actualizar a la versión más reciente de Kubernetes, puede [actualizar el clúster de AKS][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Implementación de Kured en un clúster de AKS

Para implementar el DaemonSet de `kured`, aplique el siguiente manifiesto YAML de ejemplo desde su página de proyecto de GitHub. Este manifiesto crea un rol y un rol de clúster, enlaces y una cuenta de servicio y, después, implementa el DaemonSet mediante `kured` versión 1.1.0, que es compatible con clústeres AKS 1.9 o posteriores.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.2.0/kured-1.2.0-dockerhub.yaml

You can also configure additional parameters for `kured`, such as integration with Prometheus or Slack. For more information about additional configuration parameters, see the [kured installation docs][kured-install].

## Update cluster nodes

By default, Linux nodes in AKS check for updates every evening. If you don't want to wait, you can manually perform an update to check that `kured` runs correctly. First, follow the steps to [SSH to one of your AKS nodes][aks-ssh]. Once you have an SSH connection to the Linux node, check for updates and apply them as follows:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Si se aplicaron actualizaciones que requieren un reinicio del nodo, se escribe un archivo en */var/run/reboot-required*. `Kured` comprueba si hay nodos que requieran un reinicio cada 60 minutos de forma predeterminada.

## <a name="monitor-and-review-reboot-process"></a>Supervisión y revisión del proceso de reinicio

Cuando una de las réplicas en el DaemonSet ha detectado que se requiere el reinicio de un nodo, se coloca un bloqueo en el nodo mediante la API de Kubernetes. Este bloqueo impide que se programen pods adicionales en el nodo. El bloqueo también indica que solo un nodo debe reiniciarse cada vez. Con el nodo acordonado, los pods en ejecución se purgan desde el nodo y este se reinicia.

Puede supervisar el estado de los nodos mediante el comando [kubectl get nodes][kubectl-get-nodes]. La salida del ejemplo siguiente muestra un nodo con el estado *SchedulingDisabled* mientras el nodo se prepara para el proceso de reinicio:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Una vez completado el proceso de actualización, puede ver el estado de los nodos mediante el comando [kubectl get nodes][kubectl-get-nodes] con el parámetro `--output wide`. Esta salida adicional le permite ver la diferencia en *KERNEL-VERSION* de los nodos subyacentes, como se muestra en la siguiente salida de ejemplo. El *aks-nodepool1-28993262-0* se actualizó en un paso anterior y mostrará la versión de kernel *4.15.0-1039-azure*. El nodo *aks-nodepool1-28993262-1* que no ha sido actualizado mostrará la versión de kernel *4.15.0-1037-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se detallaba cómo utilizar `kured` para los nodos de Linux se reinicie automáticamente como parte del proceso de actualización de seguridad. Para actualizar a la versión más reciente de Kubernetes, puede [actualizar el clúster de AKS][aks-upgrade].

Para clústeres AKS que usan los nodos de Windows Server, vea [actualizar un grupo de nodos de AKS][nodepool-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool

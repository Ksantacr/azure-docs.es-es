---
title: 'Conceptos: Escalado de aplicaciones en Azure Kubernetes Service (AKS)'
description: Obtenga información sobre el escalado en Azure Kubernetes Service (AKS), incluidos Horizontal Pod Autoscaler, Cluster Autoscaler y el conector de Azure Container Instances.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: zarhoads
ms.openlocfilehash: 2070c79a6ce0627280b1793e412002783f385cc0
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074034"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Opciones de escalado de aplicaciones en Azure Kubernetes Service (AKS)

A medida que ejecuta aplicaciones en Azure Kubernetes Service (AKS), es posible que deba aumentar o disminuir la cantidad de recursos de proceso. Del mismo modo que debe cambiar el número de instancias de la aplicación, es posible que deba cambiar también el número de nodos de Kubernetes subyacentes. También es posible que deba aprovisionar rápidamente un gran número de instancias de aplicación adicionales.

En este artículo se presentan los conceptos básicos para ayudarle a escalar aplicaciones en AKS:

- [Escalado manual](#manually-scale-pods-or-nodes)
- [Horizontal Pod Autoscaler (HPA)](#horizontal-pod-autoscaler)
- [Cluster Autoscaler](#cluster-autoscaler)
- [Integración de Azure Container Instances (ACI) con AKS](#burst-to-azure-container-instances)

## <a name="manually-scale-pods-or-nodes"></a>Escalado manual de pods o nodos

Puede escalar manualmente las réplicas (pods) y nodos para probar cómo responde la aplicación a un cambio en los recursos disponibles y el estado. El escalado manual de recursos también le permite definir una cantidad establecida de recursos que se usarán para mantener un costo fijo como el número de nodos. Para realizar el escalado manual, defina la réplica o el número de nodos, y la API de Kubernetes programará la creación de pods adicionales o el drenaje de nodos.

Para empezar con el escalado manual de pods y nodos, consulte [Scale applications in AKS][aks-scale] (Escalar aplicaciones en AKS).

## <a name="horizontal-pod-autoscaler"></a>Horizontal Pod Autoscaler

Kubernetes utiliza Horizontal Pod Autoscaler (HPA) para supervisar la demanda de recursos y escalar automáticamente el número de réplicas. De forma predeterminada, Horizontal Pod Autoscaler comprueba la API de métricas cada 30 segundos para ver si se requieren cambios en el número de réplicas. Cuando se necesitan cambios, el número de réplicas aumenta o disminuye en consecuencia. Horizontal Pod Autoscaler funciona con los clústeres de AKS que han implementado el servidor de métricas para Kubernetes 1.8 y versiones posteriores.

![Escalado automático de pod horizontal de Kubernetes](media/concepts-scale/horizontal-pod-autoscaling.png)

Si configura Horizontal Pod Autoscaler para una implementación determinada, defina el número mínimo y máximo de réplicas que se pueden ejecutar. Defina también la métrica que se supervisará y en la que se basarán todas las decisiones de escalado, como el uso de la CPU.

Para empezar a usar Horizontal Pod Autoscaler en AKS, consulte [Autoscale pods in AKS][aks-hpa] (Escalado automático de pods en AKS).

### <a name="cooldown-of-scaling-events"></a>Recuperación de eventos de escalado

Dado que Horizontal Pod Autoscaler comprueba cada 30 segundos la API de métricas, es posible que los eventos de escalado anteriores no se hayan completado correctamente antes de realizar otra comprobación. Este comportamiento puede provocar que Horizontal Pod Autoscaler cambie el número de réplicas antes de que el evento de escalado anterior haya podido recibir la carga de trabajo de la aplicación y las demandas de recursos para ajustarlas en consecuencia.

Para minimizar estos eventos de carrera, se pueden establecer valores de recuperación o retraso. Estos valores definen cuánto tiempo debe esperar Horizontal Pod Autoscaler después de un evento de escalado antes de que se pueda desencadenar otro evento de escalado. Este comportamiento permite que el nuevo recuento de réplicas surta efecto y que la API de métricas refleje la carga de trabajo distribuida. De forma predeterminada, el retraso en los eventos de escalado vertical es de 3 minutos y, en los eventos de reducción vertical, de 5 minutos.

Es posible que deba ajustar estos valores de recuperación. Los valores de recuperación predeterminados pueden dar la impresión de que Horizontal Pod Autoscaler no escala el recuento de réplicas con la velocidad suficiente. Por ejemplo, para aumentar más rápidamente el número de réplicas en uso, reduzca el valor `--horizontal-pod-autoscaler-upscale-delay` al crear sus definiciones de Horizontal Pod Autoscaler mediante `kubectl`.

## <a name="cluster-autoscaler"></a>Cluster Autoscaler

Para responder a las cambiantes exigencias de pod, Kubernetes tiene un clúster Escalador automático (actualmente en versión preliminar de AKS) que ajusta el número de nodos en función de los recursos de proceso solicitado en el grupo de nodos. De forma predeterminada, Cluster Autoscaler comprueba en el servidor de API cada 10 segundos los cambios necesarios en el recuento de nodos. Si Cluster Autoscaler determina que es necesario un cambio, el número de nodos del clúster de AKS aumenta o disminuye en consecuencia. Clúster Autoscaler funciona con clústeres de AKS habilitados para RBAC que ejecutan Kubernetes 1.10.x o una versión superior.

![Cluster Autoscaler de Kubernetes](media/concepts-scale/cluster-autoscaler.png)

Cluster Autoscaler se usa normalmente junto con Horizontal Pod Autoscaler. Cuando se combinan, Horizontal Pod Autoscaler aumenta o disminuye el número de pods según la demanda de la aplicación, mientras que Cluster Autoscaler ajusta el número de nodos según sea necesario para ejecutar los pods adicionales en consecuencia.

Solo se debe probar el Escalador automático del clúster en versión preliminar en clústeres AKS con un grupo de nodo único.

Para empezar a usar Cluster Autoscaler en AKS, consulte [Cluster Autoscaler on AKS][aks-cluster-autoscaler] (Cluster Autoscaler en AKS).

### <a name="scale-up-events"></a>Escalado vertical de eventos

Si un nodo no tiene suficientes recursos de proceso para ejecutar un pod solicitado, ese pod no puede avanzar en el proceso de programación. No se puede iniciar el pod a menos que existan recursos de proceso adicionales en el grupo de nodos.

Si Cluster Autoscaler detecta pods que no se pueden programar debido a restricciones de recursos del grupo de nodos, el número de nodos del grupo de nodos aumenta para proporcionar los recursos de proceso adicionales. Cuando esos nodos adicionales están correctamente implementados y disponibles para su uso en el grupo de nodos, los pods se programan para ejecutarse en estos.

Si su aplicación necesita escalar rápidamente, algunos de los pods pueden permanecer en un estado de espera de programación hasta que los nodos adicionales implementados por Cluster Autoscaler puedan aceptar los pods programados. Para las aplicaciones que tienen una alta demanda de ráfaga, puede realizar el escalado con nodos virtuales y Azure Container Instances.

### <a name="scale-down-events"></a>Reducción vertical de eventos

Cluster Autoscaler también supervisa el estado de programación de pods de los nodos que no han recibido recientemente nuevas solicitudes de programación. Este escenario indica que el grupo de nodos tiene más recursos de proceso de los necesarios y que se puede reducir el número de nodos.

Un nodo que supera un umbral que indica que ya no es necesario durante 10 minutos, está programado de forma predeterminada para su eliminación. Cuando se produce esta situación, los pods se programan para ejecutarse en otros nodos del grupo de nodos y Cluster Autoscaler reduce el número de nodos.

Las aplicaciones pueden experimentar interrupciones, ya que los pods se programan en distintos nodos cuando Cluster Autoscaler reduce el número de nodos. Para minimizar las interrupciones, evite las aplicaciones que usan una instancia de un solo pod.

## <a name="burst-to-azure-container-instances"></a>Ráfaga en Azure Container Instances

Para escalar rápidamente el clúster de AKS, puedes realizar la integración con Azure Container Instances (ACI). Kubernetes tiene componentes integrados para escalar el recuento de réplicas y nodos. Sin embargo, si la aplicación se debe escalar rápidamente, Horizontal Pod Autoscaler puede programar más pods que los recursos de proceso existentes puedan proporcionar en el grupo de nodos. Si está configurado, este escenario desencadenará Cluster Autoscaler para la implementación de nodos adicionales en el grupo de nodos, pero es posible que se necesiten unos minutos para aprovisionar correctamente esos nodos y permitir que el programador de Kubernetes ejecute pods en estos.

![Escalado de ráfaga de Kubernetes en ACI](media/concepts-scale/burst-scaling.png)

ACI le permite implementar rápidamente instancias de contenedor sin sobrecarga de infraestructura adicional. Cuando se conecta con AKS, ACI se convierte en una extensión lógica y segura de su clúster de AKS. El componente Virtual Kubelet se instala en el clúster de AKS que presenta ACI como un nodo de Kubernetes virtual. Kubernetes puede programar pods que se ejecuten como instancias de ACI a través de los nodos virtuales, no como pods en nodos de máquina virtual directamente en el clúster de AKS. Los nodos virtuales están actualmente en versión preliminar de AKS.

La aplicación no requiere ninguna modificación para usar los nodos virtuales. Puede escalar las implementaciones en AKS y ACI sin ningún retraso, ya que Cluster Autoscaler implementa los nodos nuevos en el clúster de AKS.

Los nodos virtuales se implementan en una subred adicional en la misma red virtual que el clúster de AKS. Esta configuración de red virtual permite proteger el tráfico entre ACI y AKS. Al igual que un clúster de AKS, una instancia de ACI es un recurso de proceso lógico y seguro que se aísla de otros usuarios.

## <a name="next-steps"></a>Pasos siguientes

Para empezar con el escalado de aplicaciones, siga la [guía de inicio rápido para crear un clúster de AKS con la CLI de Azure][aks-quickstart]. A continuación, puede empezar a escalar aplicaciones de manera manual o automática en el clúster de AKS:

- Escalado manual de [pods][aks-manually-scale-pods] o [nodos][aks-manually-scale-nodes]
- Uso de [Horizontal Pod Autoscaler][aks-hpa]
- Uso de [Cluster Autoscaler][aks-cluster-autoscaler]

Para obtener más información sobre los conceptos básicos de Kubernetes y AKS, consulte los artículos siguientes:

- [Kubernetes / AKS clusters and workloads][aks-concepts-clusters-workloads] (Clústeres y cargas de trabajo de Kubernetes/AKS)
- [Kubernetes / AKS access and identity][aks-concepts-identity] (Acceso e identidad de Kubernetes/AKS)
- [Kubernetes / AKS security][aks-concepts-security] (Seguridad de Kubernetes/AKS)
- [Kubernetes / AKS virtual networks][aks-concepts-network] (Redes virtuales de Kubernetes/AKS)
- [Kubernetes / AKS storage][aks-concepts-storage] (Almacenamiento de Kubernetes/AKS)

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes
[aks-cluster-autoscaler]: autoscaler.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md

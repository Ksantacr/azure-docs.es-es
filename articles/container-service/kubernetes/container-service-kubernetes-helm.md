---
title: (EN DESUSO) Implementación de contenedores con Helm en Azure Kubernetes
description: Uso de la herramienta de empaquetado de Helm para implementar contenedores en un clúster de Kubernetes en Azure Container Service
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 05edbf40e8cd5f8edbdc8b74b540962b1a25c8de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712311"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>(EN DESUSO) Uso de Helm para implementar contenedores en un clúster de Kubernetes

> [!TIP]
> Para la versión actualizada de este artículo que utiliza Azure Kubernetes Service, consulte [Instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)](../../aks/kubernetes-helm.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helm](https://github.com/kubernetes/helm/) es una herramienta de empaquetado de código abierto que ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como get Apt y Yum, se utiliza para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados. Este artículo muestra cómo trabajar con Helm en un clúster de Kubernetes implementado en Azure Container Service.

Helm tiene dos componentes: 
* La **CLI de Helm** es un cliente que se ejecuta en el equipo local o en la nube  

* **Tiller** es un servidor que se ejecuta en el clúster de Kubernetes y administra el ciclo de vida de las aplicaciones de Kubernetes 
 
## <a name="prerequisites"></a>Requisitos previos

* [Creación de un clúster de Kubernetes](container-service-kubernetes-walkthrough.md) en Azure Container Service

* [Instalación y configuración `kubectl`](../container-service-connect.md) en un equipo local

* [Instalación de Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) en un equipo local

## <a name="helm-basics"></a>Aspectos básicos de Helm 

Para ver información sobre el clúster de Kubernetes en el que está instalando Tiller e implementando las aplicaciones, escriba el siguiente comando:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Después de haber instalado Helm, instale Tiller en el clúster de Kubernetes escribiendo el comando siguiente:

```bash
helm init --upgrade
```
Una vez completado correctamente, verá el siguiente resultado:

![Instalación de Tiller](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Para ver todos los gráficos de Helm disponibles en el repositorio, escriba el siguiente comando:

```bash 
helm search 
```

El resultado debe ser parecido al siguiente:

![Búsqueda de Helm](./media/container-service-kubernetes-helm/helm-search.png)
 
Para actualizar los gráficos para obtener las versiones más recientes, escriba:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Implementación de un gráfico de controlador de entrada de Nginx 
 
Para implementar un gráfico de controlador de entrada de Nginx, escriba un comando único:

```bash
helm install stable/nginx-ingress 
```
![Implementación del controlador de entrada](./media/container-service-kubernetes-helm/nginx-ingress.png)

Si escribe `kubectl get svc` para ver todos los servicios que se ejecutan en el clúster, verá que se asigna una dirección IP al controlador de entrada. (Mientras que la asignación está en curso, verá `<pending>`. Tarda unos minutos en completarse). 

Después de que se asigne la dirección IP, vaya hasta el valor de la dirección IP externa para ver la ejecución de back-end de Nginx. 
 
![Dirección IP de entrada](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Para ver una lista de gráficos instalados en el clúster, escriba:

```bash
helm list 
```

Puede abreviar el comando en `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Implementación de un cliente y un gráfico de MariaDB

Ahora implemente un gráfico y un cliente de MariaDB para conectarse a la base de datos.

Para implementar el gráfico de MariaDB, escriba el siguiente comando:

```bash
helm install --name v1 stable/mariadb
```

donde `--name` es una etiqueta que se usa para las versiones.

> [!TIP]
> Si se produce un error en la implementación, ejecute `helm repo update` e inténtelo de nuevo.
>
 
 
Para ver todos los gráficos implementados en el clúster, escriba:

```bash 
helm list
```
 
Para ver todas las implementaciones que se ejecutan en el clúster, escriba:

```bash
kubectl get deployments 
``` 
 
 
Por último, para ejecutar un pod para tener acceso al cliente, escriba:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Para conectar con el cliente, escriba el siguiente comando, reemplazando `v1-mariadb` por el nombre de la implementación:

```bash
sudo mysql –h v1-mariadb
```
 
 
Ahora puede usar los comandos SQL estándar para crear bases de datos, tablas, etc. Por ejemplo, `Create DATABASE testdb1;` crea una base de datos vacía. 
 
 
 
## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo administrar gráficos de Kubernetes, vea la [Documentación de Helm](https://github.com/kubernetes/helm/blob/master/docs/index.md). 


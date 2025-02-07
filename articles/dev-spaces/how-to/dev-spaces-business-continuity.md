---
title: Continuidad empresarial y recuperación ante desastres en Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/28/2019
ms.topic: conceptual
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
manager: jeconnoc
ms.openlocfilehash: 7b463be143ed3f89c1b10424dafc7a0e841ecbfc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60686980"
---
# <a name="business-continuity-and-disaster-recovery-in-azure-dev-spaces"></a>Continuidad empresarial y recuperación ante desastres en Azure Dev Spaces

## <a name="review-disaster-recovery-guidance-for-azure-kubernetes-service-aks"></a>Revisión de la guía de recuperación ante desastres para Azure Kubernetes Service (AKS)

Azure Dev Spaces es una característica de Azure Kubernetes Service (AKS). Debe tener en cuenta las directrices para la recuperación ante desastres en AKS y tener en cuenta si se aplican a los clústeres de AKS que usa para Dev Spaces. Para obtener más información, consulte [Procedimientos recomendados para continuidad empresarial y recuperación ante desastres en Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region)

## <a name="enable-dev-spaces-on-aks-clusters-in-different-regions"></a>Habilitación de Dev Spaces en clústeres de AKS en diferentes regiones

La habilitación de Dev Spaces en clústeres de AKS en regiones diferentes le permite reanudar inmediatamente mediante Dev Spaces después de un error de región de Azure.

Para obtener información general acerca de las implementaciones de varias regiones de AKS, consulte [Planeamiento de la implementación en varias regiones](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region#plan-for-multi-region-deployment).

Para obtener información sobre cómo implementar un clúster de AKS compatible con Azure Dev Spaces, consulte [Creación de un clúster de Kubernetes mediante Azure Cloud Shell](https://docs.microsoft.com/azure/dev-spaces/how-to/create-cluster-cloud-shell)

### <a name="enable-dev-spaces-via-the-azure-portal"></a>Habilitación de Dev Spaces mediante Azure Portal

Haga clic en el elemento de navegación de **Dev Spaces** en las propiedades de cada clúster en Azure Portal. A continuación, elija la opción para habilitar Dev Spaces.

![Habilitación de Dev Spaces mediante Azure Portal](../media/common/enable-dev-spaces.jpg)

Repita este proceso para cada clúster.

### <a name="enable-dev-spaces-via-the-azure-cli"></a>Habilitación de Dev Spaces mediante la CLI de Azure

También puede habilitar Dev Spaces en la línea de comandos:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

## <a name="deploy-your-teams-baseline-to-each-cluster"></a>Implementación de la línea de base del equipo en cada clúster

Al trabajar con Dev Spaces, normalmente implementa toda la aplicación en un espacio de desarrollo principal en el clúster de Kubernetes. De forma predeterminada, se usa el espacio `default`. La implementación inicial incluye todos los servicios, así como los recursos externos que dependen de ellos, como bases de datos o colas. Esto se conoce como la *línea de base*. Una vez establecida una línea de base en el espacio de desarrollo principal, depure los servicios individuales de los espacios de desarrollo secundarios e itere en ellos.

Debe implementar las versiones más recientes del conjunto de servicios de línea de base en clústeres en varias regiones. Al actualizar los servicios de línea de base de esta manera, se garantiza que pueda seguir usando Dev Spaces si se produce un error de región de Azure. Por ejemplo, si implementa la línea de base a través de una canalización de CI/CD, modifique la canalización para que se implemente en varios clústeres en diferentes regiones.

## <a name="select-the-correct-aks-cluster-to-use-for-dev-spaces"></a>Selección del clúster de AKS correcto que se usará para Dev Spaces

Tras configurar correctamente un clúster de copia de seguridad que ejecuta la línea de base del equipo, puede cambiar rápidamente al clúster de copia de seguridad en cualquier momento. A continuación, vuelva a ejecutar los servicios individuales en los que trabaja en Dev Spaces.

Seleccione otro clúster con el siguiente comando de la CLI:

```cmd
az aks use-dev-spaces -g <new resource group name> -n <new cluster name>
```

Puede enumerar los espacios de desarrollo disponibles en el nuevo clúster con el comando siguiente:

```cmd
azds space list
```

Puede crear un nuevo espacio de desarrollo, o bien seleccionar uno existente, con el siguiente comando:

```cmd
azds space select -n <space name>
```

Después de ejecutar estos comandos, el clúster seleccionado y el espacio de desarrollo se utilizarán para las operaciones posteriores de la CLI, así como para la depuración de proyectos con la extensión de Visual Studio Code para Azure Dev Spaces.

Si utiliza Visual Studio, puede cambiar el clúster usado por un proyecto existente a través de los pasos siguientes:

1. Abra el proyecto en Visual Studio.
1. Haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y, a continuación, haga clic en **Propiedades**.
1. En el panel izquierdo, haga clic en **Depurar**.
1. En la página de las propiedades de depuración, haga clic en la lista desplegable **Perfil** y elija **Azure Dev Spaces**.
1. Haga clic en el botón **Cambiar**.
1. En el cuadro de diálogo que aparece, seleccione el clúster de AKS que quiere usar. También puede elegir un espacio de desarrollo diferente en el que trabajar, o bien crear uno nuevo, mediante la selección de la opción adecuada de la lista desplegable **Espacio**.

Una vez haya seleccionado el clúster y el espacio correctos, puede presionar F5 para ejecutar el servicio en Dev Spaces.

Repita estos pasos para todos los proyectos configurados para usar el clúster original.

## <a name="access-a-service-on-a-backup-cluster"></a>Acceso a un servicio en un clúster de copia de seguridad

Si ha configurado el servicio para utilizar un nombre DNS público, el servicio tendrá una dirección URL diferente si se ejecuta en un clúster de copia de seguridad. Los nombres DNS públicos están siempre en el formato siguiente: `<space name>.s.<root space name>.<service name>.<cluster GUID>.<region>.azds.io`. Si cambia a otro clúster, cambiará el GUID del clúster y, posiblemente, la región.

Dev Spaces siempre muestra la dirección URL correcta para el servicio al ejecutar `azds up` o en la ventana de salida de Visual Studio en **Azure Dev Spaces**.

También puede encontrar la dirección URL mediante la ejecución del comando `azds list-uris`:
```
$ azds list-uris
Uri                                                     Status
------------------------------------------------------  ---------
http://default.mywebapi.d05afe7e006a4fddb73c.eus.azds.io/  Available
```

Use esta dirección URL al obtener acceso al servicio.

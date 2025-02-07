---
title: Uso directivas de reinicio con tareas en contenedores en Azure Container Instances
description: Aprenda a usar Azure Container Instances para ejecutar tareas hasta su terminación, como compilaciones, pruebas o trabajos de representación de imágenes.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 06872eefd0d500a22214109ad5055dd236b5a6ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60608127"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Ejecución de tareas en contenedores con directivas de reinicio

La facilidad y rapidez con que se implementan contenedores convierten a Azure Container Instances en una plataforma convincente para la ejecución de tareas que se ejecutan una única vez, como compilaciones, pruebas y representaciones de imágenes en una instancia de contenedor.

Gracias a una directiva de reinicio configurable, puede especificar que los contenedores se detengan cuando sus procesos se hayan completado. Dado que las instancias de contenedor se facturan por segundo, solo se le cobra por los recursos de proceso usados mientras el contenedor que ejecuta su tarea está en funcionamiento.

En los ejemplos presentados en este artículo se usa la CLI de Azure. Debe tener [instalada localmente][azure-cli-install] la CLI de Azure 2.0.21 o posterior o usar la CLI de [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Directiva de reinicio de contenedor

Cuando se crea un [grupo de contenedores](container-instances-container-groups.md) en Azure Container Instances, se puede especificar una de tres configuraciones de directiva de reinicio.

| Directiva de reinicio   | DESCRIPCIÓN |
| ---------------- | :---------- |
| `Always` | Siempre se reinician los contenedores del grupo de contenedores. Este es el valor de configuración **predeterminado** aplicado cuando no se especifica ninguna directiva de reinicio durante la creación del contenedor. |
| `Never` | Nunca se reinician los contenedores del grupo de contenedores. Los contenedores se ejecutan al menos una vez. |
| `OnFailure` | Los contenedores del grupo de contenedores se reinician solo cuando se produce un error en el proceso ejecutado en el contenedor (cuando se cierra con un código de salida distinto de cero). Los contenedores se ejecutan al menos una vez. |

## <a name="specify-a-restart-policy"></a>Especificación de una directiva de reinicio

El modo en que especifique una directiva de reinicio depende de cómo cree sus instancias de contenedor, es decir, con la CLI de Azure, los cmdlets de Azure PowerShell o Azure Portal. En la CLI de Azure, especifique el parámetro `--restart-policy` cuando llame a [az container create][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Ejemplo de ejecución hasta terminación

Para ver la directiva de reinicio en acción, cree una instancia del contenedor de Microsoft [aci-wordcount] [ aci-wordcount-image] la imagen y especifique el `OnFailure` directiva de reinicio. Este contenedor de ejemplo ejecuta un script de Python que, de forma predeterminada, analiza el texto de la obra [Hamlet](http://shakespeare.mit.edu/hamlet/full.html) de Shakespeare, escribe las 10 palabras más comunes en STDOUT y, a continuación, se cierra.

Ejecute el contenedor de ejemplo con el siguiente comando [az container create][az-container-create]:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances inicia el contenedor y, luego, lo detiene cuando su aplicación (o script, en este caso) se cierra. Cuando Azure Container Instances detiene un contenedor cuya directiva de reinicio es `Never` o `OnFailure`, el estado del contenedor se establece en **Terminado**. Puede comprobar el estado de un contenedor con el comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Salida de ejemplo:

```bash
"Terminated"
```

Una vez que el estado del contenedor de ejemplo muestra *Terminado*, puede ver la salida de las tareas en los registros del contenedor. Ejecute el comando [az container logs][az-container-logs] para ver la salida del script:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Salida:

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

En este ejemplo se muestra la salida que el script envía a STDOUT. No obstante, puede que las tareas en contenedor escriban su salida en almacenamiento persistente para su posterior recuperación. Por ejemplo, en un [recurso compartido de archivos de Azure](container-instances-mounting-azure-files-volume.md).

## <a name="next-steps"></a>Pasos siguientes

Escenarios basados en tareas, como un conjunto de datos grande con varios contenedores, de procesamiento por lotes pueden aprovechar las ventajas de personalizado [variables de entorno](container-instances-environment-variables.md) o [líneas de comandos](container-instances-start-command.md) en tiempo de ejecución.

Para más información acerca de cómo conservar la salida de los contenedores que se ejecutan hasta completarse, consulte [Montaje de un recurso compartido de archivos de Azure en Azure Container Instances](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli

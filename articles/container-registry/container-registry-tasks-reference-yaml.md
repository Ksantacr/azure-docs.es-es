---
title: 'Referencia de Azure Container Registry Tasks: YAML'
description: Referencia para definir tareas en YAML para ACR Tasks, como propiedades de tareas, tipos de pasos, propiedades de pasos y variables integradas.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: bdf88657c11bdb5ab5bcde97c155780328065c7e
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954474"
---
# <a name="acr-tasks-reference-yaml"></a>Referencia de ACR Tasks: YAML

La definición de tareas de varios pasos en ACR Tasks proporciona un primitivo de proceso centrado en el contenedor enfocado a compilar y probar contenedores, y aplicarles revisiones. En este artículo se trata los comandos, parámetros, propiedades y sintaxis de los archivos YAML que definen las tareas de varios pasos.

Este artículo contiene la referencia para crear archivos YAML de tarea de varios pasos para ACR Tasks. Para una introducción a ACR Tasks, consulte la [introducción a ACR Tasks](container-registry-tasks-overview.md).

## <a name="acr-taskyaml-file-format"></a>formato de archivo acr-task.yaml

ACR Tasks admite la declaración de tareas de varios pasos en la sintaxis estándar de YAML. Definir los pasos de una tarea en un archivo YAML. A continuación, puede ejecutar la tarea, pase el archivo a la [acr az ejecutar] [ az-acr-run] comando. O bien, use el archivo para crear una tarea con [crear tarea de az acr] [ az-acr-task-create] que se desencadena automáticamente en una actualización de imagen base o de confirmación de Git. Aunque en este artículo se hace referencia a `acr-task.yaml` como el archivo que contiene los pasos, ACR Tasks admite cualquier nombre de archivo válido con una [extensión admitida](#supported-task-filename-extensions).

Los primitivos `acr-task.yaml` de nivel superior son **propiedades de tareas**, **tipos de pasos** y **propiedades de pasos**:

* Las [propiedades de tareas](#task-properties) se aplican a todos los pasos a lo largo de la ejecución de tareas. Hay varias propiedades de tareas globales, incluidos:
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* Los [tipos de pasos de tareas](#task-step-types) representan los tipos de acciones que se pueden realizar en una tarea. Hay tres tipos de pasos:
  * `build`
  * `push`
  * `cmd`
* Las [propiedades de pasos de tareas](#task-step-properties) son parámetros que se aplican a un paso individual. Hay varias propiedades de pasos, como:
  * `startDelay`
  * `timeout`
  * `when`
  * y muchas más.

Le sigue el formato base de un archivo `acr-task.yaml`, incluidas algunas propiedades de pasos comunes. Aunque no es una representación exhaustiva de todo el uso de propiedades de pasos o tipos de pasos, proporciona una información general rápida del formato de archivo básico.

```yml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
steps: # A collection of image or container actions.
  - build: # Equivalent to "docker build," but in a multi-tenant environment
  - push: # Push a newly built or retagged image to a registry.
    when: # Step property that defines either parallel or dependent step execution.
  - cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
    startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Extensiones de nombre de archivo de tareas admitidas

ACR Tasks ha reservado varias extensiones de nombre de archivo, como `.yaml`, que se procesarán como un archivo de tarea. Cualquier extensión que *no* se encuentre en la siguiente lista se considera un archivo Dockerfile en ACR Tasks: .yaml, .yml, .toml, .json, .sh, .bash, .zsh,. ps1, .ps, .cmd, .bat, TS, .js, .php, .py,. RB, .lua.

YAML es el único formato de archivo compatible actualmente con ACR Tasks. Las demás extensiones de nombre de archivo están reservadas para posible compatibilidad futura.

## <a name="run-the-sample-tasks"></a>Ejecución de las tareas de ejemplo

Son varios los archivos de tareas de ejemplo a los que se hace referencia en las siguientes secciones de este artículo. Las tareas de ejemplo están en un repositorio público de GitHub, [Azure-Samples/acr-tasks][acr-tasks]. Puede ejecutarlas con el comando [az acr run][az-acr-run] de la CLI de Azure. Los comandos de ejemplo son parecidos a:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

En el formato de los comandos de ejemplo se da por hecho que ha configurado un registro de forma predeterminada en la CLI de Azure, por lo que se omite el parámetro `--registry`. Para configurar un registro predeterminado, use el comando [az configure][az-configure] con el parámetro `--defaults`, que acepta un valor `acr=REGISTRY_NAME`.

Por ejemplo, para configurar la CLI de Azure con un registro predeterminado llamado "myregistry":

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Propiedades de tareas

Propiedades de la tarea suelen aparecerán en la parte superior de un `acr-task.yaml` de archivos, y son propiedades globales que se aplican durante la ejecución completa de los pasos de la tarea. Algunas de estas propiedades globales se pueden reemplazar dentro de un paso individual.

| Propiedad | Type | Opcional | DESCRIPCIÓN | Invalidación admitida | Valor predeterminado |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | string | Sí | La versión del archivo `acr-task.yaml` analizada por el servicio ACR Tasks. Si bien ACR Tasks se esfuerza por mantener la compatibilidad con versiones anteriores, este valor permite que ACR Tasks mantenga la compatibilidad dentro de una versión definida. Si no se especifica, el valor predeterminado es la versión más reciente. | No | None |
| `stepTimeout` | int (segundos) | Sí | El número máximo de segundos que se puede ejecutar un paso. Si se especifica la propiedad en una tarea, Establece el valor predeterminado `timeout` propiedad de todos los pasos. Si el `timeout` propiedad se especifica en un paso, invalida la propiedad proporcionada por la tarea. | Sí | 600 (10 minutos) |
| `workingDirectory` | string | Sí | El directorio de trabajo del contenedor en tiempo de ejecución. Si se especifica la propiedad en una tarea, Establece el valor predeterminado `workingDirectory` propiedad de todos los pasos. Si se especifica en un paso, invalida la propiedad proporcionada por la tarea. | Sí | `$HOME` |
| `env` | [string, string, ...] | Sí |  Matriz de cadenas en `key=value` formato que definen las variables de entorno para la tarea. Si se especifica la propiedad en una tarea, Establece el valor predeterminado `env` propiedad de todos los pasos. Si se especifica en un paso, reemplaza las variables de entorno heredadas de la tarea. | None |
| `secrets` | [secreta, secreta,...] | Sí | Matriz de [secreto](#secret) objetos. | None |
| `networks` | [red, red,...] | Sí | Matriz de [red](#network) objetos. | None |

### <a name="secret"></a>clave

El objeto secreto tiene las siguientes propiedades.

| Propiedad | Type | Opcional | DESCRIPCIÓN | Valor predeterminado |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | string | No | El identificador del secreto. | None |
| `keyvault` | string | Sí | La URL de secreto de Azure Key Vault. | None |
| `clientID` | string | Sí | El identificador de cliente de la asignada por el usuario administra identidades para los recursos de Azure. | None |

### <a name="network"></a>red

El objeto de red tiene las siguientes propiedades.

| Propiedad | Type | Opcional | DESCRIPCIÓN | Valor predeterminado |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | string | No | El nombre de la red. | None |
| `driver` | string | Sí | El controlador para administrar la red. | None |
| `ipv6` | booleano | Sí | Si está habilitada redes IPv6. | `false` |
| `skipCreation` | booleano | Sí | Si se omite la creación de la red. | `false` |
| `isDefault` | booleano | Sí | Si la red es una red predeterminada que se proporcionan con Azure Container Registry | `false` |

## <a name="task-step-types"></a>Tipos de pasos de tareas

ACR Tasks admite tres tipos de pasos. Cada tipo de paso admite varias propiedades, que se detallan en la sección para cada tipo de paso.

| Tipo de paso | DESCRIPCIÓN |
| --------- | ----------- |
| [`build`](#build) | Compila una imagen de contenedor mediante la conocida sintaxis `docker build`. |
| [`push`](#push) | Ejecuta una acción `docker push` de imágenes recién compiladas o etiquetadas de nuevo en un registro de contenedor. Se admite Azure Container Registry, otros registros privados y Docker Hub público. |
| [`cmd`](#cmd) | Ejecuta un contenedor como un comando, con parámetros que se pasan al elemento `[ENTRYPOINT]` del contenedor. El `cmd` tipo de paso es compatible con parámetros como `env`, `detach`y otro familiar `docker run` opciones de comandos, habilitar pruebas de unidades y funcional con la ejecución del contenedor simultáneo. |

## <a name="build"></a>build

Compila una imagen de contenedor. El tipo de paso `build` representa un medio multiinquilino seguro de ejecutar `docker build` en la nube como un primitivo de primera clase.

### <a name="syntax-build"></a>Sintaxis: build

```yml
version: v1.0.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

El tipo de paso `build` admite los parámetros de la tabla siguiente. El tipo de paso `build` también es compatible con todas las opciones del comando [compilación de docker](https://docs.docker.com/engine/reference/commandline/build/), como `--build-arg` para establecer las variables en tiempo de compilación.

| Parámetro | DESCRIPCIÓN | Opcional |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Define el elemento completo `image:tag` de la imagen compilada.<br /><br />Como se pueden usar imágenes para las validaciones de tareas internas, no todas las imágenes requieren la acción `push` en un registro. Sin embargo, para crear una instancia de una imagen dentro de una ejecución de tareas, la imagen necesita un nombre al que hacer referencia.<br /><br />A diferencia de `az acr build`, ejecutar tareas de ACR no proporciona comportamiento predeterminado de inserción. Con ACR Tasks, en el escenario predeterminado se da por hecho la posibilidad de compilar, validar y luego insertar una imagen. Consulte [push](#push) para ver como insertar opcionalmente imágenes compiladas. | Sí |
| `-f` &#124; `--file` | Especifica el archivo Dockerfile que se pasa a `docker build`. Si no se especifica, se da por hecho el archivo Dockerfile predeterminado en la raíz del contexto. Para especificar un archivo Dockerfile, pase el nombre de archivo relativa a la raíz del contexto. | Sí |
| `context` | El directorio raíz pasado a `docker build`. El directorio raíz de cada tarea se establece en un directorio [workingDirectory](#task-step-properties) compartido, e incluye la raíz del directorio clonado de Git asociado. | No |

### <a name="properties-build"></a>Propiedades: build

El tipo de paso `build` admite las siguientes propiedades. Encontrar detalles de estas propiedades en el [propiedades de paso de tarea](#task-step-properties) sección de este artículo.

| | | |
| -------- | ---- | -------- |
| `detach` | booleano | Opcional |
| `disableWorkingDirectoryOverride` | booleano | Opcional |
| `entryPoint` | string | Opcional |
| `env` | [string, string, ...] | Opcional |
| `expose` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `isolation` | string | Opcional |
| `keep` | booleano | Opcional |
| `network` | objeto | Opcional |
| `ports` | [string, string, ...] | Opcional |
| `pull` | booleano | Opcional |
| `repeat` | int | Opcional |
| `retries` | int | Opcional |
| `retryDelay` | int (segundos) | Opcional |
| `secret` | objeto | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |
| `workingDirectory` | string | Opcional |

### <a name="examples-build"></a>Ejemplos: build

#### <a name="build-image---context-in-root"></a>Imagen de compilación: contexto en raíz

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Imagen de compilación: contexto en el subdirectorio

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>push

Inserta una o varias imágenes compiladas o etiquetadas de nuevo en un registro de contenedor. Admite la inserción en registros privados como Azure Container Registry o Docker Hub público.

### <a name="syntax-push"></a>Sintaxis: push

El tipo de paso `push` admite una colección de imágenes. La sintaxis de la colección YAML admite formatos alineados y anidados. La inserción de una sola imagen se representa normalmente mediante sintaxis alineada:

```yml
version: v1.0.0
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Para una mejor legibilidad, use sintaxis anidada al insertar varias imágenes:

```yml
version: v1.0.0
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Propiedades: push

El tipo de paso `push` admite las siguientes propiedades. Encontrar detalles de estas propiedades en el [propiedades de paso de tarea](#task-step-properties) sección de este artículo.

| | | |
| -------- | ---- | -------- |
| `env` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |

### <a name="examples-push"></a>Ejemplos: push

#### <a name="push-multiple-images"></a>Insertar varias imágenes

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Compilar, insertar y ejecutar

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

El tipo de paso `cmd` ejecuta un contenedor.

### <a name="syntax-cmd"></a>Sintaxis: cmd

```yml
version: v1.0.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Propiedades: cmd

El tipo de paso `cmd` admite las siguientes propiedades:

| | | |
| -------- | ---- | -------- |
| `detach` | booleano | Opcional |
| `disableWorkingDirectoryOverride` | booleano | Opcional |
| `entryPoint` | string | Opcional |
| `env` | [string, string, ...] | Opcional |
| `expose` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `isolation` | string | Opcional |
| `keep` | booleano | Opcional |
| `network` | objeto | Opcional |
| `ports` | [string, string, ...] | Opcional |
| `pull` | booleano | Opcional |
| `repeat` | int | Opcional |
| `retries` | int | Opcional |
| `retryDelay` | int (segundos) | Opcional |
| `secret` | objeto | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |
| `workingDirectory` | string | Opcional |

Puede encontrar detalles de estas propiedades en la sección [Propiedades de pasos de tareas](#task-step-properties) de este artículo.

### <a name="examples-cmd"></a>Ejemplos: cmd

#### <a name="run-hello-world-image"></a>Ejecutar la imagen hello world

Este comando ejecuta el archivo de tarea `hello-world.yaml`, que hace referencia a la imagen [hello world](https://hub.docker.com/_/hello-world/) de Docker Hub.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Ejecutar imagen de Bash y repetir "hello world"

Este comando ejecuta el archivo de tarea `bash-echo.yaml`, que hace referencia a la imagen [bash](https://hub.docker.com/_/bash/) de Docker Hub.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Ejecutar etiqueta de imagen de Bash específica

Para ejecutar una versión de imagen concreta, especifique la etiqueta en `cmd`.

Este comando ejecuta el archivo de tarea `bash-echo-3.yaml`, que hace referencia a la imagen [bash:3.0](https://hub.docker.com/_/bash/) de Docker Hub.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Ejecutar imágenes personalizadas

El tipo de paso `cmd` hace referencia a las imágenes mediante el formato estándar `docker run`. Se da por hecho que las imágenes que no comienzan con un registro se originan en docker.io. El ejemplo anterior se podría representar igualmente como:

```yml
version: v1.0.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

Mediante el estándar `docker run` convención de referencia, la imagen `cmd` puede ejecutar imágenes desde cualquier registro privado o público Docker Hub. Si va a hacer referencia a imágenes que se encuentran en el mismo registro donde se ejecutan las tareas de ACR, no es necesario especificar credenciales de registro.

* Ejecución de una imagen de Azure container registry

    Reemplace `[myregistry]` por el nombre del registro.

    ```yml
    version: v1.0.0
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Generalizar la referencia del registro con una variable de ejecución

    En lugar de codificar de forma rígida el nombre del registro en un archivo `acr-task.yaml`, puede hacer que sea más fácil de trasladar mediante una [variable de ejecución](#run-variables). La variable `Run.Registry` se expande en tiempo de ejecución al nombre del registro en el que se ejecuta la tarea.

    Para generalizar la tarea anterior de modo que funcione en cualquier registro de contenedor de Azure, haga referencia a la variable [Run.Registry](#runregistry) en el nombre de imagen:

    ```yml
    version: v1.0.0
    steps:
      - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Propiedades de pasos de tareas

Cada tipo de paso admite varias propiedades adecuadas para su tipo. En la tabla siguiente se definen todas las propiedades de pasos disponibles. No todos los tipos de pasos admiten todas las propiedades. Para ver cuál de estas propiedades está disponible para cada tipo de paso, consulte las acciones de referencia de tipos de pasos [cmd](#cmd), [build](#build) y [push](#push).

| Propiedad | Type | Opcional | DESCRIPCIÓN | Valor predeterminado |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | booleano | Sí | Si el contenedor se debe desasociar cuando se ejecuta. | `false` |
| `disableWorkingDirectoryOverride` | booleano | Sí | Si se deshabilita `workingDirectory` reemplazar funcionalidad. Utilícelo en combinación con `workingDirectory` tener control total sobre el directorio de trabajo del contenedor. | `false` |
| `entryPoint` | string | Sí | Invalida el elemento `[ENTRYPOINT]` del contenedor de un paso. | None |
| `env` | [string, string, ...] | Sí | Matriz de cadenas en formato `key=value` que define las variables de entorno del paso. | None |
| `expose` | [string, string, ...] | Sí | Matriz de los puertos que se exponen desde el contenedor. |  None |
| [`id`](#example-id) | string | Sí | Identifica el paso de forma única dentro de la tarea. Otros pasos de la tarea pueden hacer referencia al elemento `id` del paso, por ejemplo, para la comprobación de dependencias con `when`.<br /><br />`id` también es el nombre del contenedor en ejecución. Los procesos que se ejecutan en otros contenedores de la tarea pueden hacer referencia a `id` como su nombre de host DNS, o para acceder a él con registros de docker [id], por ejemplo. | `acb_step_%d`, donde `%d` es el índice de base 0 del paso de arriba a abajo en el archivo YAML |
| `ignoreErrors` | booleano | Sí | Si se debe marcar el paso como correcta, independientemente de si se produjo un error durante la ejecución del contenedor. | `false` |
| `isolation` | string | Sí | El nivel de aislamiento del contenedor. | `default` |
| `keep` | booleano | Sí | Si se debe mantener el contenedor del paso después de la ejecución. | `false` |
| `network` | objeto | Sí | Identifica una red en el que se ejecuta el contenedor. | None |
| `ports` | [string, string, ...] | Sí | Matriz de los puertos que se publican desde el contenedor en el host. |  None |
| `pull` | booleano | Sí | Si se debe forzar una extracción del contenedor antes de ejecutarlo para evitar cualquier comportamiento de almacenamiento en caché. | `false` |
| `privileged` | booleano | Sí | Si se debe ejecutar el contenedor en modo privilegiado. | `false` |
| `repeat` | int | Sí | El número de reintentos para repetir la ejecución de un contenedor. | 0 |
| `retries` | int | Sí | El número de reintentos si se produce un error en un contenedor de su ejecución. Si el código de salida de un contenedor es distinto de cero, solo se realiza un reintento. | 0 |
| `retryDelay` | int (segundos) | Sí | El retardo en segundos entre reintentos de ejecución de un contenedor. | 0 |
| `secret` | objeto | Sí | Identifica un secreto de Azure Key Vault o una identidad administrada para recursos de Azure. | None |
| `startDelay` | int (segundos) | Sí | Número de segundos para retrasar la ejecución de un contenedor. | 0 |
| `timeout` | int (segundos) | Sí | Número máximo de segundos que se puede ejecutar un paso antes de terminar. | 600 |
| [`when`](#example-when) | [string, string, ...] | Sí | Configura la dependencia de un paso de uno o varios pasos dentro de la tarea. | None |
| `user` | string | Sí | El nombre de usuario o UID de un contenedor | None |
| `workingDirectory` | string | Sí | Establece el directorio de trabajo de un paso. De forma predeterminada, ACR Tasks crea un directorio raíz como directorio de trabajo. Sin embargo, si la compilación tiene varios pasos, los pasos anteriores pueden compartir artefactos con los pasos posteriores mediante la especificación del mismo directorio de trabajo. | `$HOME` |

### <a name="examples-task-step-properties"></a>Ejemplos: Propiedades de pasos de tareas

#### <a name="example-id"></a>Ejemplo: id

Compila dos imágenes mediante la creación de una instancia de una imagen de prueba funcional. Cada paso se identifica mediante un valor de `id` único al que otros pasos de la tarea hacen referencia en su propiedad `when`.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Ejemplo: when

La propiedad `when` especifica la dependencia de un paso de otros pasos dentro de la tarea. Admite dos valores de parámetro:

* `when: ["-"]`: no indica ninguna dependencia de otros pasos. Un paso que especifica `when: ["-"]` comenzará inmediatamente la ejecución y permitirá la ejecución de pasos simultáneos.
* `when: ["id1", "id2"]`: indica que el paso depende de los pasos con `id` "id1" e `id` "id2". Este paso no se ejecuta hasta que los pasos "id1" e "id2" se han completado.

Si `when` no se especifica en un paso, este paso depende de la finalización del paso anterior en el archivo `acr-task.yaml`.

Ejecución secuencial de pasos sin `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Ejecución secuencial de pasos con `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Compilación de imágenes paralelas:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Compilación de imágenes paralelas y pruebas de dependencia:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Variables de ejecución

ACR Tasks incluye un conjunto predeterminado de variables que están disponibles para los pasos de la tarea cuando se ejecutan. Se puede acceder a estas variables con el formato `{{.Run.VariableName}}`, donde `VariableName` es uno de los siguientes:

* `Run.ID`
* `Run.Registry`
* `Run.Date`
* `Run.Commit`
* `Run.Branch`

### <a name="runid"></a>Run.ID

Cada ejecución, mediante `az acr run`, o la ejecución basada en desencadenador de las tareas creadas por medio de `az acr task create`, tiene un identificador único. El identificador representa la ejecución actualmente en marcha.

Se usa normalmente para etiquetar de forma única una imagen:

```yml
version: v1.0.0
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

El nombre completo de servidor del registro. Se usa normalmente para hacer referencia genérica al registro donde se ejecuta la tarea.

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

La hora UTC actual a la que comenzó la ejecución.

### <a name="runcommit"></a>Run.Commit

Para una tarea desencadenada por una confirmación en un repositorio de GitHub, el identificador de confirmación.

### <a name="runbranch"></a>Run.Branch

Para una tarea desencadenada por una confirmación en un repositorio de GitHub, el nombre de rama.

## <a name="next-steps"></a>Pasos siguientes

Para información general sobre las tareas de varios pasos, consulte [Ejecución de tareas de varios pasos de compilación, prueba y aplicación de revisiones en ACR Tasks](container-registry-tasks-multi-step.md).

Para compilaciones paso a paso, consulte la [Introducción a ACR Tasks](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure

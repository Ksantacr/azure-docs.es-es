---
title: 'Procedimientos recomendados del operador: seguridad de clústeres en Azure Kubernetes Service (AKS)'
description: Obtenga información sobre los procedimientos recomendados del operador de clústeres para saber cómo administrar la seguridad y las actualizaciones de los clústeres en Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: a468c2f3b1b3034c817ac19988420b68e18deb83
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849849"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para administrar la seguridad y las actualizaciones de los clústeres en Azure Kubernetes Service (AKS)

A medida que administra los clústeres en Azure Kubernetes Service (AKS), es clave considerar la seguridad de los datos y de las cargas de trabajo. Especialmente cuando ejecuta clústeres multiinquilino con aislamiento lógico, debe proteger el acceso a los recursos y a las cargas de trabajo. Para minimizar el riesgo de ataques, también debe asegurarse de aplicar las actualizaciones de seguridad más recientes de Kubernetes y del SO del nodo.

En este artículo se indica cómo proteger el clúster de AKS. Aprenderá a:

> [!div class="checklist"]
> * Usar Azure Active Directory y los controles de acceso basado en roles para proteger el acceso al servidor de API
> * Proteger el acceso del contenedor a los recursos del nodo
> * Actualización de un clúster AKS a la última versión de Kubernetes
> * Mantener actualizados los nodos y aplicar automáticamente los parches de seguridad.

También puede leer las prácticas recomendadas para la [administración de imágenes de contenedor][best-practices-container-image-management] y para [seguridad de pod][best-practices-pod-security].

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>Proteger el acceso a los nodos del clúster y al servidor de API

**Orientación con procedimientos recomendados**: asegurar el acceso al servidor de API de Kubernetes es una de las cosas más importantes que puede hacer para proteger su clúster. Integre el control de acceso basado en rol (RBAC) de Kubernetes con Azure Active Directory para controlar el acceso al servidor de API. Estos controles le permiten proteger AKS del mismo modo que protege el acceso a las suscripciones a Azure.

El servidor de API de Kubernetes proporciona un punto de conexión único para las solicitudes para realizar acciones dentro de un clúster. Para proteger y auditar el acceso al servidor de API, limitar el acceso y proporcionar los permisos de acceso con privilegios mínimos necesarios. Este enfoque no es exclusivo de Kubernetes, pero es especialmente importante cuando el clúster de AKS está lógicamente aislado para el uso multiinquilino.

Azure Active Directory (AD) proporciona una solución de administración de identidades para el ámbito empresarial que se puede integrar con los clústeres de AKS. Como Kubernetes no proporciona una solución de administración de identidades, puede ser difícil proporcionar una forma pormenorizada para restringir el acceso al servidor de API. Con los clústeres integrados con Azure AD en AKS, usted utiliza sus cuentas de usuario y de grupo existentes para autenticar a los usuarios en el servidor de API.

![Integración de Azure Active Directory para clústeres de AKS](media/operator-best-practices-cluster-security/aad-integration.png)

Use RBAC de Kubernetes y la integración de Azure AD para proteger el servidor de API y proporcionar los privilegios mínimos necesarios para un determinado conjunto de recursos, como un único espacio de nombres. A diferentes usuarios o grupos de Azure AD se les pueden asignar diferentes roles RBAC. Estos permisos granulares permiten restringir el acceso al servidor de API y proporcionan una pista de auditoría clara de las acciones realizadas.

La práctica recomendada consiste en usar grupos para proporcionar acceso a archivos y carpetas frente a identidades individuales, use la pertenencia al *grupo* de Azure AD para enlazar a los usuarios a roles de RBAC en lugar de a *usuarios* individuales. A medida que cambie la pertenencia al grupo de un usuario, también lo harán sus permisos de acceso en el clúster de AKS. Si enlaza al usuario directamente a un rol, su función de trabajo puede cambiar. Las pertenencias al grupo de Azure AD se actualizarán, pero esto no se reflejará en los permisos en el clúster de AKS. En este escenario, al usuario se le otorgan más permisos de los que necesita.

Para obtener más información acerca de la integración de Azure AD y RBAC, consulte [Best practices for authentication and authorization in AKS][aks-best-practices-identity] (Procedimientos recomendados para la autenticación y autorización en AKS).

## <a name="secure-container-access-to-resources"></a>Proteger el acceso del contenedor a los recursos

**Orientación con procedimientos recomendados**: limite el acceso a las acciones que los contenedores pueden realizar. Proporcione el menor número de permisos, y evite el uso de privilegios escalados o acceso a raíz.

De la misma manera, debería conceder a los usuarios o a los grupos el menor número de privilegios requeridos, los contenedores también deberían limitarse a las acciones y procesos que necesiten. Para minimizar el riesgo de ataques, no configure las aplicaciones ni los contenedores que requieren privilegios escalados o acceso a raíz. Por ejemplo, establezca `allowPrivilegeEscalation: false` en el manifiesto de pod. Estos *contextos de seguridad de pod* están integrados en Kubernetes y le permiten definir permisos adicionales, como el usuario o grupo como el que se ejecutará, o qué funcionalidades de Linux se expondrán. Para ver más recomendaciones, consulte [Secure pod access to resources][pod-security-contexts] (Acceso seguro de pod a recursos).

Para un control más detallado de las acciones de los contenedores, también puede usar las características de seguridad incorporadas de Linux como *AppArmor*y *seccomp*. Estas características se definen en el nivel de nodo y, después, se implementan a través de un manifiesto de pod. Características de seguridad integradas de Linux solo están disponibles en los pods y los nodos de Linux.

> [!NOTE]
> Los entornos de Kubernetes, tanto en AKS como en cualquier otro lugar, no están completamente seguros ante el uso de varios inquilinos hostiles. Utilizar otras características de seguridad adicionales, como *AppArmor*, *seccomp*, *Pod Security Policies* o los controles de acceso basados en roles (RBAC) más específicos, puede hacer que las vulnerabilidades de seguridad sean menos frecuentes. Sin embargo, para que la seguridad resulte efectiva cuando se ejecutan cargas de trabajo multiinquilino hostiles, el hipervisor es el único nivel de seguridad en el que debe confiar. El dominio de seguridad de Kubernetes se convierte en todo el clúster, no en un nodo específico. En el caso de estos tipos de cargas de trabajo multiinquilino hostiles, debe usar clústeres que estén físicamente aislados.

### <a name="app-armor"></a>AppArmor

Para limitar las acciones que pueden llevar a cabo los contenedores, puede usar el módulo de seguridad del kernel de Linux [AppAmour][k8s-apparmor]. AppArmor está disponible como parte del SO del nodo de AKS subyacente del sistema operativo y está habilitado de forma predeterminada. Puede crear perfiles de AppArmor para restringir acciones como leer, escribir o ejecutar, o funciones del sistema como montar sistemas de archivos. Los perfiles de AppArmor predeterminados restringen el acceso a diferentes ubicaciones de `/proc` y `/sys`, y proporcionan un medio para aislar lógicamente los contenedores desde el nodo subyacente. AppArmor funciona para cualquier aplicación que se ejecuta en Linux, no solo para los pods de Kubernetes.

![Perfiles de AppArmor en uso en un clúster AKS para limitar las acciones del contenedor](media/operator-best-practices-container-security/apparmor.png)

Para ver AppArmor en acción, en el ejemplo siguiente se crea un perfil que impide la escritura en archivos. [SSH][aks-ssh] a un nodo de AKS y, después, cree un archivo denominado *deny-write.profile* y pegue el siguiente contenido:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

Los perfiles de AppArmor se agregan con el comando `apparmor_parser`. Agregue el perfil a AppArmor y especifique el nombre del perfil que se creó en el paso anterior:

```console
sudo apparmor_parser deny-write.profile
```

No se devuelve ningún resultado si el perfil se analiza y se aplica a AppArmor correctamente. Se le redirige al símbolo del sistema.

Desde la máquina local, puede crear un manifiesto de pod denominado *aks apparmor.yaml* y pegar el contenido siguiente. Este manifiesto define una anotación para `container.apparmor.security.beta.kubernetes` y hacer referencia al perfil *deny-write* que se creó en los pasos anteriores:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

Implemente el pod de ejemplo con el comando [kubectl apply][kubectl-apply]:

```console
kubectl apply -f aks-apparmor.yaml
```

Con el pod implementado, use el comando [kubectl exec][kubectl-exec] para escribir en un archivo. No se puede ejecutar el comando, como se muestra en la salida del ejemplo siguiente:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

Para obtener más información sobre AppArmor, vea [AppArmor profiles in Kubernetes][k8s-apparmor] (Perfiles de AppArmor en Kubernetes).

### <a name="secure-computing"></a>Informática segura

Si bien AppArmor funciona con cualquier aplicación de Linux, [seccomp (*sec*ure *comp*uting)][seccomp] funciona en el nivel de proceso. Seccomp también es un módulo de seguridad del kernel de Linux, y es compatible de forma nativa con el tiempo de ejecución de Docker que utilizan los nodos de AKS. Con seccomp, las llamadas de proceso que pueden llevar a cabo los contenedores están limitadas. Puede crear filtros que definen qué acciones permitir o denegar y, después, usar anotaciones dentro de un manifiesto YAML de pod para asociar el filtro seccomp. Esto se alinea a la práctica recomendada de solo conceder al contenedor los permisos mínimos necesarios para ejecutar.

Para ver seccomp en acción, cree un filtro que evite el cambio de permisos en un archivo. [SSH][aks-ssh] a un nodo de AKS y, después, cree un filtro seccomp denominado */var/lib/kubelet/seccomp/prevent-chmod* y pegue el siguiente contenido:

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "syscalls": [
    {
      "name": "chmod",
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

Desde la máquina local, puede crear un manifiesto de pod denominado *aks seccomp.yaml* y pegar el contenido siguiente. Este manifiesto define una anotación para `seccomp.security.alpha.kubernetes.io` y hace referencia al filtro *prevent-chmod* que se creó en el paso anterior:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: chmod-prevented
  annotations:
    seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
spec:
  containers:
  - name: chmod
    image: busybox
    command:
      - "chmod"
    args:
     - "777"
     - /etc/hostname
  restartPolicy: Never
```

Implemente el pod de ejemplo con el comando [kubectl apply][kubectl-apply]:

```console
kubectl apply -f ./aks-seccomp.yaml
```

Vea el estado de los pod mediante el comando [kubectl get pods][kubectl-get]. El pod indica un error. El filtro seccomp impide que se ejecute el comando `chmod`, tal como se muestra en la salida del ejemplo siguiente:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Para obtener más información acerca de los filtros disponibles, vea [Seccomp security profiles for Docker][seccomp] (Perfiles de seguridad de Seccomp para Docker).

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Actualización regular a la versión más reciente de Kubernetes

**Orientación con procedimientos recomendados**: para mantenerse al día de las nuevas características y correcciones de errores, actualice regularmente la versión de Kubernetes en el clúster de AKS.

Kubernetes presenta nuevas características a un ritmo más rápido que otras plataformas de infraestructura más tradicionales. Las actualizaciones de Kubernetes incluyen nuevas características y correcciones de seguridad o errores. Las características nuevas pasan normalmente de un estado *alfa* a un estado *beta*, hasta que acaban siendo *estables*, están disponibles en general y se recomienda su uso en producción. Este ciclo de lanzamiento debería permitirle actualizar Kubernetes sin encontrar regularmente cambios importantes ni tener que ajustar las implementaciones ni las plantillas.

AKS es compatible con cuatro versiones secundarias de Kubernetes. Esto significa que cuando se introduce una nueva versión secundaria de parche, se retirarán la versión secundaria y la versión de revisión compatibles más antiguas. Las actualizaciones secundarias en Kubernetes se realizan periódicamente. Asegúrese de que dispone de un proceso de gobernanza para comprobar y actualizar según sea necesario, de modo que no se quede sin soporte. Para más información, consulte [Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)][aks-supported-versions].

Para comprobar las versiones que están disponibles para el clúster, use el comando [az aks get-upgrades][az-aks-get-upgrades] tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

A continuación, puede actualizar el clúster de AKS con el comando [az aks upgrade][az-aks-upgrade]. El proceso de actualización acordona y drena de forma segura un nodo de cada vez, programa pods en los nodos restantes y, después, implementa un nuevo nodo que ejecute las versiones más recientes del sistema operativo y de Kubernetes.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.8
```

Para obtener más información sobre las actualizaciones de AKS, consulte [Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)][aks-supported-versions] y [Actualización de un clúster de AKS][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Nodo de proceso de Linux se actualiza y reinicia utilizando kured

**Mejor orientación práctica** : AKS se descarga automáticamente y seguridad de las instalaciones correcciones en cada nodos de Linux, pero no se reinicia automáticamente si es necesario. Use `kured` para prestar atención a los reinicios pendientes, luego acordone y drene el nodo de forma segura para permitir que se reinicie, se apliquen las actualizaciones y esté lo más seguro posible con respecto al sistema operativo. Para los nodos de Windows Server (actualmente en versión preliminar de AKS), realice periódicamente una operación de actualización de AKS acordonar y agotar pods y implementar los nodos actualizados de forma segura.

Cada noche, los nodos de Linux en AKS obtención las revisiones de seguridad disponibles a través de su canal de actualización de la distribución. Este comportamiento se configura automáticamente cuando se implementan los nodos en un clúster de AKS. Para minimizar las interrupciones y el posible impacto sobre las cargas de trabajo en ejecución, los nodos no se reinician automáticamente si lo requiere una revisión de seguridad o una actualización de kernel.

El proyecto [kured (KUbernetes REboot Daemon)][kured] de código abierto de Weaveworks vigila los reinicios pendientes del nodo. Cuando un nodo de Linux aplica a las actualizaciones que requieren un reinicio, el nodo sin ningún riesgo es acordonan y vacían para mover y programar los pods en otros nodos del clúster. Una vez que se reinicia el nodo, se vuelve a agregar al clúster y Kubernetes vuelve a reanudar la programación de pods en él. Para minimizar las interrupciones, solo se permite que `kured` reinicie un único nodo a la vez.

![Proceso de reinicio del nodo de AKS con kured](media/operator-best-practices-cluster-security/node-reboot-process.png)

Si desea un control más preciso sobre cuándo se producen los reinicios, `kured` puede integrarse con Prometheus para evitar reinicios si hay otros eventos de mantenimiento o problemas de clúster en curso. Esta integración minimiza las complicaciones adicionales al reiniciar los nodos mientras está solucionando activamente otros problemas.

Para obtener más información acerca de cómo controlar los inicios del nodo, vea [Apply security and kernel updates to nodes in AKS][aks-kured] (Aplicación de actualizaciones de kernel y de seguridad en los nodos en AKS).

## <a name="next-steps"></a>Pasos siguientes

En este artículo se indica cómo proteger el clúster de AKS. Para implementar algunas de estas áreas, consulte los artículos siguientes:

* [Integración de Azure Active Directory con Azure Kubernetes Service][aks-aad]
* [Actualización de un clúster AKS a la última versión de Kubernetes][aks-upgrade]
* [Procesamiento de actualizaciones de seguridad y reinicios del nodo con kured][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://docs.docker.com/engine/security/seccomp/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: azure-ad-integration.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md

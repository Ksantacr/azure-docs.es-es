---
title: Uso de una identidad administrada con Azure Container Instances
description: Aprenda a usar una identidad administrada para autenticar con otros servicios de Azure desde Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 10/22/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: ac0a84aa3121c6ebb91860c96c0f6692827c8a3f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66152337"
---
# <a name="how-to-use-managed-identities-with-azure-container-instances"></a>Cómo utilizar una identidad administrada con Azure Container Instances

Utilice [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) si quiere ejecutar código en Azure Container Instances que interactúe con otros servicios de Azure, sin mantener secretos ni credenciales en el código. La característica facilita una implementación de Azure Container Instances con una identidad administrada automáticamente en Azure Active Directory.

En este artículo, conocerá mejor las identidades administradas de Azure Container Instances y aprenderá a realizar las siguientes tareas:

> [!div class="checklist"]
> * Habilitación de una identidad asignada por el usuario o asignada por el sistema en un grupo de contenedores
> * Concesión del acceso de identidad a una instancia de Azure Key Vault
> * Utilización de la identidad administrada para tener acceso a Key Vault desde un contenedor en ejecución

Adaptación de los ejemplos para habilitar y utilizar las identidades en Azure Container Instances para tener acceso a otros servicios de Azure. Estos ejemplos son interactivos. Aunque, en la práctica, las imágenes de contenedor ejecutarían el código para tener acceso a servicios de Azure.

> [!NOTE]
> Actualmente no se puede usar una identidad administrada en un grupo de contenedores que se implementa en una red virtual.

## <a name="why-use-a-managed-identity"></a>¿Por qué usar una identidad administrada?

Utilice una identidad administrada en un contenedor en ejecución para autenticarse en cualquier [servicio que admita la autenticación de Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) sin tener que administrar credenciales en el código. En el caso de los servicios que no son compatibles con la autenticación de Azure AD, puede almacenar secretos en Azure Key Vault y usar la identidad administrada para tener acceso a Key Vault para recuperar las credenciales. Para más información sobre las identidades administradas, vea [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md)

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general. Actualmente, solo se admiten identidades administradas en las instancias de contenedor de Linux.
>  

### <a name="enable-a-managed-identity"></a>Habilitación de una entidad administrada

 En Azure Container Instances, se admiten identidades administradas para los recursos de Azure a partir de la API de REST versión 2018-10-01 y los SDK y las herramientas correspondientes. Al crear un grupo de contenedores, habilite una o varias identidades administradas estableciendo una propiedad [ContainerGroupIdentity](/rest/api/container-instances/containergroups/createorupdate#containergroupidentity). También puede habilitar o actualizar las identidades administradas después de que se ejecute un grupo de contenedores; cualquiera de estas acciones hace que el grupo de contenedores se reinicie. Para establecer las identidades en un grupo de contenedores nuevo o existente, use la CLI de Azure, una plantilla de Resource Manager o un archivo YAML. 

Azure Container Instances admite ambos tipos de identidades administradas de Azure: asignada por el usuario y asignada por el sistema. En un grupo de contenedores, puede habilitar una identidad asignada por el sistema y una o más identidades asignadas por el usuario. 

* Una identidad administrada **asignada por el usuario** se crea como recurso independiente de Azure en el inquilino de Azure AD de confianza de la suscripción en uso. Una vez creada, la identidad puede asignarse a uno o varios recursos de Azure (en Azure Container Instances o en otros servicios de Azure). El ciclo de vida de una identidad asignada por el usuario no se administra junto con el ciclo de vida de los grupos de contenedores o de otros recursos de servicio a los que se asigna. Este comportamiento resulta especialmente útil en Azure Container Instances. Dado que la identidad se prolonga más allá de la duración de un grupo de contenedores, puede reutilizarla con otras configuraciones estándares para que las implementaciones de grupos de contenedores sean altamente repetibles.

* Las identidades administradas **asignadas por el sistema** se habilitan directamente en un grupo de contenedores de Azure Container Instances. Cuando se habilitan, Azure crea una identidad para el grupo en el inquilino de Azure AD de confianza de la suscripción de la instancia. Una vez creada la identidad, las credenciales se aprovisionan en cada contenedor del grupo de contenedores. El ciclo de vida de una identidad administrada asignada por el sistema se vincula directamente al grupo de contenedores de Azure en que se habilita. Cuando se elimina el grupo, Azure limpia automáticamente las credenciales y la identidad en Azure AD.

### <a name="use-a-managed-identity"></a>Uso de una identidad administrada

Para usar una identidad administrada, la identidad debe tener acceso inicialmente a uno o más recursos de servicio de Azure (como una aplicación web, una instancia de Key Vault o una cuenta de almacenamiento) en la suscripción. Para obtener acceso a los recursos de Azure desde un contenedor en ejecución, el código debe adquirir un *token de acceso* de un punto de conexión de Azure AD. Luego, el código envía el token de acceso en una llamada a un servicio que admite la autenticación de Azure AD. 

El uso de una identidad administrada en un contenedor en ejecución es básicamente igual al uso de una identidad en una máquina virtual de Azure. Vea las instrucciones de la máquina virtual sobre el uso de un [token](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md), la [CLI de Azure o de Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md) o los [SDK de Azure](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.49 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-key-vault"></a>Crear una instancia de Azure Key Vault

En los ejemplos de este artículo se utiliza una identidad administrada de Azure Container Instances para tener acceso a un secreto de Azure Key Vault. 

En primer lugar, cree un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus* con el comando [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) siguiente:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Utilice el comando [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) para crear una instancia de Key Vault. Asegúrese de especificar un nombre de Key Vault único. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Almacene un secreto de ejemplo en Key Vault con el comando [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set):

```azurecli-interactive
az keyvault secret set --name SampleSecret --value "Hello Container Instances!" --description ACIsecret  --vault-name mykeyvault
```

Continúe con los siguientes ejemplos para tener acceso a Key Vault mediante una identidad administrada asignada por el usuario o asignada por el sistema en Azure Container Instances.

## <a name="example-1-use-a-user-assigned-identity-to-access-azure-key-vault"></a>Ejemplo 1: Uso de una identidad asignada por el usuario para tener acceso a Azure Key Vault

### <a name="create-an-identity"></a>Creación de una identidad

En primer lugar, cree una identidad en la suscripción con el comando [az identity create](/cli/azure/identity?view=azure-cli-latest#az-identity-create). Puede usar el mismo grupo de recursos que usó para crear la instancia de Key Vault o usar otro distinto.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACIId
```

Para utilizar la identidad en los pasos siguientes, use el comando [az identity show](/cli/azure/identity?view=azure-cli-latest#az-identity-show) para almacenar en variables los identificadores de la entidad de servicio y del recurso de la identidad.

```azurecli-interactive
# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACIId --query principalId --output tsv)

# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACIId --query id --output tsv)
```

### <a name="enable-a-user-assigned-identity-on-a-container-group"></a>Habilitación de una identidad asignada por el usuario en un grupo de contenedores

Ejecute el comando [az container create](/cli/azure/container?view=azure-cli-latest#az-container-create) siguiente para implementar una instancia de contenedor basada en Ubuntu Server. En este ejemplo se ofrece un grupo de contenedores único que puede usarse para el acceso interactivo a otros servicios de Azure. El parámetro `--assign-identity` pasa la identidad administrada asignada por el usuario al grupo. El comando de ejecución prolongada mantiene el contenedor en ejecución. En este ejemplo se utiliza el mismo grupo de recursos usado para crear la instancia de Key Vault, pero puede especificarse otro distinto.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity $resourceID --command-line "tail -f /dev/null"
```

En unos segundos, debería recibir una respuesta de la CLI de Azure que indica que la implementación ha finalizado. Compruebe su estado con el comando [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show).

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

La sección `identity` de la salida tiene un aspecto similar al siguiente, que muestra que la identidad está establecida en el grupo de contenedores. El valor de `principalID` en `userAssignedIdentities` corresponde a la entidad de servicio de la identidad que creó en Azure Active Directory:

```console
...
"identity": {
    "principalId": "null",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-0903-4b79-a55a-xxxxxxxxxxxx/resourcegroups/danlep1018/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACIId": {
        "clientId": "xxxxxxxx-5523-45fc-9f49-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-f25b-4895-b828-xxxxxxxxxxxx"
      }
    }
  },
...
```

### <a name="grant-user-assigned-identity-access-to-the-key-vault"></a>Concesión de acceso a la identidad asignada por el usuario en Key Vault

Ejecute el comando [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest) siguiente para establecer una directiva de acceso en Key Vault. En el ejemplo siguiente se permite que la identidad asignada por el usuario obtenga los secretos de Key Vault:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-user-assigned-identity-to-get-secret-from-key-vault"></a>Uso de la identidad asignada por el usuario para obtener el secreto de Key Vault

Ahora puede usar la identidad administrada para tener acceso a Key Vault en la instancia de contenedor en ejecución. En este ejemplo, inicie primero un shell de Bash en el contenedor:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Ejecute los siguientes comandos del shell de Bash en el contenedor. Para obtener un token de acceso para usar Azure Active Directory para autenticarse en Key Vault, ejecute el siguiente comando:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Salida:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Para almacenar el token de acceso en una variable para usar en los siguientes comandos para autenticarse, ejecute el siguiente comando:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Ahora puede usar el token de acceso para autenticarse en Key Vault y leer un secreto. No olvide sustituir el nombre del almacén de claves en la dirección URL (*https://mykeyvault.vault.azure.net/...*):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

La respuesta será similar a la siguiente, que muestra el secreto. En el código, podría analizar esta salida para obtener el secreto. Utilice luego el secreto en una operación posterior para tener acceso a otro recurso de Azure.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="example-2-use-a-system-assigned-identity-to-access-azure-key-vault"></a>Ejemplo 2: Uso de una identidad asignada por el sistema para tener acceso a Azure Key Vault

### <a name="enable-a-system-assigned-identity-on-a-container-group"></a>Habilitación de una identidad asignada por el sistema en un grupo de contenedores

Ejecute el comando [az container create](/cli/azure/container?view=azure-cli-latest#az-container-create) siguiente para implementar una instancia de contenedor basada en Ubuntu Server. En este ejemplo se ofrece un grupo de contenedores único que puede usarse para el acceso interactivo a otros servicios de Azure. El parámetro `--assign-identity` sin ningún valor adicional habilita una identidad administrada asignada por el sistema en el grupo. El comando de ejecución prolongada mantiene el contenedor en ejecución. En este ejemplo se utiliza el mismo grupo de recursos usado para crear la instancia de Key Vault, pero puede especificarse otro distinto.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity --command-line "tail -f /dev/null"
```

En unos segundos, debería recibir una respuesta de la CLI de Azure que indica que la implementación ha finalizado. Compruebe su estado con el comando [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show).

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

La sección `identity` de la salida tiene un aspecto similar al siguiente, que muestra que se crea una identidad asignada por el sistema en Azure Active Directory:

```console
...
"identity": {
    "principalId": "xxxxxxxx-528d-7083-b74c-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
},
...
```

Establezca una variable en el valor de `principalId` (identificador de la entidad de servicio) de la identidad para su uso en pasos posteriores.

```azurecli-interactive
spID=$(az container show --resource-group myResourceGroup --name mycontainer --query identity.principalId --out tsv)
```

### <a name="grant-container-group-access-to-the-key-vault"></a>Concesión de acceso al grupo de contenedores en Key Vault

Ejecute el comando [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest) siguiente para establecer una directiva de acceso en Key Vault. En el ejemplo siguiente se permite que la identidad administrada asignada por el sistema obtenga los secretos de Key Vault:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-container-group-identity-to-get-secret-from-key-vault"></a>Uso de la identidad del grupo de contenedores para obtener el secreto de Key Vault

Ahora puede usar la identidad administrada para tener acceso a Key Vault en la instancia de contenedor en ejecución. En este ejemplo, inicie primero un shell de Bash en el contenedor:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Ejecute los siguientes comandos del shell de Bash en el contenedor. Para obtener un token de acceso para usar Azure Active Directory para autenticarse en Key Vault, ejecute el siguiente comando:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Salida:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Para almacenar el token de acceso en una variable para usar en los siguientes comandos para autenticarse, ejecute el siguiente comando:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Ahora puede usar el token de acceso para autenticarse en Key Vault y leer un secreto. No olvide sustituir el nombre del almacén de claves en la dirección URL (*https:\//mykeyvault.vault.azure.net/...* ):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

La respuesta será similar a la siguiente, que muestra el secreto. En el código, podría analizar esta salida para obtener el secreto. Utilice luego el secreto en una operación posterior para tener acceso a otro recurso de Azure.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="enable-managed-identity-using-resource-manager-template"></a>Habilitación de la identidad administrada con una plantilla de Resource Manager

Para habilitar una identidad administrada en un grupo de contenedores con una [plantilla de Resource Manager](container-instances-multi-container-group.md), establezca la propiedad `identity` del objeto `Microsoft.ContainerInstance/containerGroups` con un objeto `ContainerGroupIdentity`. Los siguientes fragmentos de código muestran la propiedad `identity` configurada para diferentes escenarios. Vea [Referencia de plantilla de Resource Manager](/azure/templates/microsoft.containerinstance/containergroups). Especifique un `apiVersion` de `2018-10-01`.

### <a name="user-assigned-identity"></a>Identidad asignada por el usuario

Una identidad asignada por el usuario es un identificador de recurso del formulario:

```
"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}"
``` 

Puede habilitar una o más identidades asignadas por el usuario.

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
```

### <a name="system-assigned-identity"></a>Identidad asignada por el sistema

```json
"identity": {
    "type": "SystemAssigned"
    }
```

### <a name="system--and-user-assigned-identities"></a>Identidades asignadas por el sistema y por el usuario

En un grupo de contenedores, puede habilitar una identidad asignada por el sistema y una o más identidades asignadas por el usuario.

```json
"identity": {
    "type": "System Assigned, UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
...
```

## <a name="enable-managed-identity-using-yaml-file"></a>Habilitación de una identidad administrada con un archivo YAML

Para habilitar una identidad administrada en un grupo de contenedores implementados con un [archivo YAML](container-instances-multi-container-yaml.md), incluya el siguiente código YAML.
Especifique un `apiVersion` de `2018-10-01`.

### <a name="user-assigned-identity"></a>Identidad asignada por el usuario

Una identidad asignada por el usuario es un identificador de recurso del formulario. 

```
'/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'
```

Puede habilitar una o más identidades asignadas por el usuario.

```YAML
identity:
  type: UserAssigned
  userAssignedIdentities:
    {'myResourceID1':{}}
```

### <a name="system-assigned-identity"></a>Identidad asignada por el sistema

```YAML
identity:
  type: SystemAssigned
```

### <a name="system--and-user-assigned-identities"></a>Identidades asignadas por el sistema y por el usuario

En un grupo de contenedores, puede habilitar una identidad asignada por el sistema y una o más identidades asignadas por el usuario.

```YAML
identity:
  type: SystemAssigned, UserAssigned
  userAssignedIdentities:
   {'myResourceID1':{}}
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha conocido las identidades administradas de Azure Container Instances y ha aprendido a realizar las siguientes tareas:

> [!div class="checklist"]
> * Habilitación de una identidad asignada por el usuario o asignada por el sistema en un grupo de contenedores
> * Concesión del acceso de identidad a una instancia de Azure Key Vault
> * Utilización de la identidad administrada para tener acceso a Key Vault desde un contenedor en ejecución

* Obtenga más información sobre las [identidades administradas para recursos de Azure](/azure/active-directory/managed-identities-azure-resources/).

* Vea un [ejemplo Azure SDK para Go](https://medium.com/@samkreter/c98911206328) sobre el uso de una identidad administrada para tener acceso a una instancia de Key Vault en Azure Container Instances.

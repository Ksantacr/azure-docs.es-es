---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 64290aad2d9f98006a715b480be8cb96965abbaf
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161707"
---
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Cree la máquina virtual con el comando [az vm create](/cli/azure/vm). 

En el ejemplo siguiente, se crea una máquina virtual denominada *myVM* y las claves SSH si aún no existen en una ubicación de claves predeterminada. Para utilizar un conjunto específico de claves, utilice la opción `--ssh-key-value`. El comando también establece *azureuser* como nombre de usuario de administrador. Use este nombre más adelante para conectarse a la VM. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Cuando se ha creado la máquina virtual, la CLI de Azure muestra información similar al ejemplo siguiente. Anote el valor de `publicIpAddress`. Esta dirección se utiliza para acceder a la VM en los pasos posteriores.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a>Apertura del puerto 80 para el tráfico web 

De forma predeterminada, solo se permiten conexiones mediante SSH con las máquinas virtuales Linux implementadas en Azure. Dado que esta máquina virtual va a ser un servidor web, debe abrir el puerto 80 desde Internet. Use el comando [az vm open-port](/cli/azure/vm) para abrir el puerto deseado.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>Conexión SSH con la máquina virtual


Si no conoce la dirección IP pública de la máquina virtual, ejecute el comando [az network public-ip list](/cli/azure/network/public-ip). Necesitará esta dirección IP para varios pasos posteriores.


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Ejecute el comando siguiente para crear una sesión SSH con la máquina virtual. Sustituya la dirección IP pública correcta de la máquina virtual. En este ejemplo, la dirección IP es *40.68.254.142*. *azureuser* se establece el nombre de usuario de administrador establecido al crear la VM.

```bash
ssh azureuser@40.68.254.142
```


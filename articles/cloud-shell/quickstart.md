---
title: Guía de inicio rápido de Bash en Azure Cloud Shell | Microsoft Docs
description: Guía de inicio rápido para Bash en Cloud Shell
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: b8f96de7214a46c9e38182c141343a46c0e28139
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60199624"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Guía de inicio rápido para Bash en Azure Cloud Shell

En este documento se detalla cómo usar Bash en Azure Cloud Shell en [Azure Portal](https://ms.portal.azure.com/).

> [!NOTE]
> También hay disponible una guía de inicio rápido de [PowerShell en Azure Cloud Shell](quickstart-powershell.md).

## <a name="start-cloud-shell"></a>Inicio de Cloud Shell
1. Inicie **Cloud Shell** en la navegación superior de Azure Portal. <br>
![](media/quickstart/shell-icon.png)

2. Seleccione una suscripción para crear una cuenta de almacenamiento y un recurso compartido de Microsoft Azure Files.
3. Seleccione "Create storage" (Creación de almacenamiento)

> [!TIP]
> Se autentica automáticamente para la CLI de Azure en todas las sesiones.

### <a name="select-the-bash-environment"></a>Selección del entorno Bash
Compruebe que en el menú desplegable de entornos que se encuentra al lado izquierdo de la ventana del shell pone `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Establecimiento de la suscripción
1. Enumere las suscripciones a las que tiene acceso.
   ```azurecli-interactive
   az account list
   ```

2. Establezca su suscripción preferida: <br>
```azurecli-interactive
az account set --subscription 'my-subscription-name'
```

> [!TIP]
> La suscripción se recordará para sesiones futuras mediante `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos
Cree un nuevo grupo de recursos en la región oeste de EE. UU. llamado "MyRG".
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Creación de una máquina virtual Linux
Cree una máquina virtual con Ubuntu en su nuevo grupo de recursos. La CLI de Azure creará claves SSH y configurará la máquina virtual con ellas. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Usar `--generate-ssh-keys` indica a la CLI de Azure que debe crear y configurar las claves públicas y privadas en la máquina virtual y el directorio `$Home`. De forma predeterminada, las claves se colocan en Cloud Shell en `/home/<user>/.ssh/id_rsa` y `/home/<user>/.ssh/id_rsa.pub`. La carpeta `.ssh` se conserva en la imagen de 5 GB adjunta del recurso compartido de archivos utilizada para conservar `$Home`.

Su nombre de usuario en esta máquina virtual será el nombre de usuario utilizado en Cloud Shell ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>Conexión SSH con la máquina virtual Linux
1. Busque el nombre de la máquina virtual en la barra de búsqueda de Azure Portal.
2. Haga clic en "Conectar" para obtener el nombre de máquina virtual y la dirección IP pública. <br>
   ![](media/quickstart/sshcmd-copy.png)

3. SSH en la máquina virtual con el cmd `ssh`.
   ```
   ssh username@ipaddress
   ```

Al establecer la conexión SSH, debería ver el aviso de bienvenida de Ubuntu. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Limpiar 
1. Cierre la sesión de SSH.
   ```azurecli-interactive
   exit
   ```

2. Elimine el grupo de recursos y cualquier recurso dentro del mismo.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Pasos siguientes
[Información sobre la persistencia de los archivos para Bash en Cloud Shell](persisting-shell-storage.md) <br>
[Más información acerca de la CLI de Azure](https://docs.microsoft.com/cli/azure/) <br>
[Más información sobre el almacenamiento en Azure Files](../storage/files/storage-files-introduction.md) <br>

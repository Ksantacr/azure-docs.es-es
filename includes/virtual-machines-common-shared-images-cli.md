---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 841027fe8d6b97e661faa038dc9381edbb3d4cd8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226029"
---
## <a name="before-you-begin"></a>Antes de empezar

Para completar el ejemplo de este artículo, debe tener una imagen administrada existente de una VM generalizada. Para más información, consulte [Tutorial: Creación de una imagen personalizada de una máquina virtual de Azure con la CLI de Azure 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Si la imagen administrada contiene un disco de datos, el tamaño del disco de datos no puede tener más de 1 TB.

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/bash](https://shell.azure.com/bash) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

Si prefiere instalar y usar la CLI localmente, consulte [instalar la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="create-an-image-gallery"></a>Creación de una galería de imágenes 

Una galería de imágenes es el recurso principal que se usa para habilitar el uso compartido de imágenes. Los caracteres permitidos para el nombre de la Galería son letras mayúsculas o minúsculas, números y puntos. El nombre de la Galería no puede contener guiones.   Los nombres de las galerías deben ser únicos dentro de su suscripción. 

Cree una galería de imágenes mediante [az sig az create](/cli/azure/sig#az-sig-create). En el ejemplo siguiente se crea una galería denominada *myGallery* en *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Creación de la definición de una imagen

Las definiciones de la imagen creación una agrupación lógica de imágenes. Se utilizan para administrar la información sobre las versiones de la imagen que se crean dentro de ellos. Los nombres de definición de imagen pueden estar formados por letras en mayúsculas o minúsculas, dígitos, puntos, guiones y períodos. Para obtener más información acerca de los valores que se puede especificar para una definición de la imagen, consulte [definiciones de la imagen](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Cree la definición de una imagen inicial en la galería mediante [az sig image-definition create](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```


## <a name="create-an-image-version"></a>Creación de la versión de una imagen 

Cree versiones de la imagen según sea necesario con [az image gallery create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create). Deberá pasar el identificador de la imagen administrada que se usará como base para crear la versión de la imagen. Puede usar [az image list](/cli/azure/image?view#az-image-list) para obtener información sobre las imágenes que se encuentran en un grupo de recursos. 

Los caracteres permitidos para la versión de una imagen son números y puntos. Los números deben estar dentro del rango de un entero de 32 bits. Formato: *MajorVersion*. *MinorVersion*. *Revisión*.

En este ejemplo, es la versión de nuestra imagen *1.0.0* y vamos a crear 2 réplicas en el *centro occidental de Ee.uu.* región, 1 réplica en el *South Central US* región y 1 réplica en el *East US 2* región mediante almacenamiento con redundancia de zona.


```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1=Standard_ZRS" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> Deberá esperar a que finalice por completo que se compila y se replican para poder usar la misma imagen administrada para crear otra versión de la imagen la versión de imagen.
>
> También puede almacenar todas las réplicas de la versión de imagen en [almacenamiento con redundancia de zona](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) agregando `--storage-account-type standard_zrs` al crear la versión de la imagen.
>

## <a name="share-the-gallery"></a>Compartir la Galería

Se recomienda que comparta con otros usuarios en el nivel de la galería. Para obtener el identificador de objeto de la galería, use [show de az sig](/cli/azure/sig#az-sig-show).

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Use el identificador de objeto como un ámbito, junto con una dirección de correo electrónico y [crear asignación de roles az](/cli/azure/role/assignment#az-role-assignment-create) otorgar a un usuario acceso a la Galería de imágenes compartidas.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```



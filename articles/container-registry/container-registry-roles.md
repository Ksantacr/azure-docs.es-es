---
title: 'Azure Container Registry: roles y permisos'
description: Use el control de acceso basado en roles (RBAC) de Azure y la administración de identidades y acceso (IAM) para proporcionar la personalización avanzada de permisos a recursos en una instancia de Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 03/20/2019
ms.author: danlep
ms.openlocfilehash: b6e26bfa476c5c13e6e478f40c39978af61d83e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427027"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Roles y permisos de Azure Container Registry

El servicio Azure Container Registry admite un conjunto de roles de Azure que proporcionan distintos niveles de permisos a una instancia de Azure Container Registry. Use el [control de acceso basado en roles](../role-based-access-control/index.yml) (RBAC) de Azure para asignar permisos específicos a usuarios o entidades de servicio que necesiten interactuar con un registro.

| Rol/permiso       | [Acceso a Resource Manager](#access-resource-manager) | [Crear o eliminar un registro](#create-and-delete-registry) | [Insertar imagen](#push-image) | [Extraer imagen](#pull-image) | [Eliminar datos de imagen](#delete-image-data) | [Cambiar directivas](#change-policies) |   [Firmar imágenes](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Propietario | X | X | X | X | X | X |  |  
| Colaborador | X | X | X |  X | X | X |  |  
| Lector | X |  |  | X |  |  |  |
| AcrPush |  |  | X | X | |  |  |  
| AcrPull |  |  |  | X |  |  |  |  
| AcrDelete |  |  |  |  | X |  |  |
| AcrImageSigner |  |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Diferenciar usuarios y servicios

Cada vez que se aplican permisos, el procedimiento recomendado es proporcionar el conjunto de permisos más limitados para que una persona o servicio puedan completar una tarea. Los siguientes conjuntos de permisos representan un conjunto de funciones para usuarios humanos y servicios desatendidos.

### <a name="cicd-solutions"></a>Soluciones de CI/CD

Al automatizar comandos de `docker build` de soluciones de CI/CD, necesita funciones de `docker push`. En estos escenarios de servicios desatendidos, le sugerimos que asigne el rol **AcrPush**. Este rol, al contrario que el rol **Colaborador** (que es más amplio), impide que la cuenta pueda realizar otras operaciones de registro o que acceda a Azure Resource Manager.

### <a name="container-host-nodes"></a>Nodos de host de contenedor

Del mismo modo, los nodos que ejecuten los contenedores han de tener asignado el rol **AcrPull**, pero no necesitan de forma obligatoria funciones de **Lector**.

### <a name="visual-studio-code-docker-extension"></a>Extensión de Docker de Visual Studio Code

Para herramientas como la [extensión de Docker](https://code.visualstudio.com/docs/azure/docker) de Visual Studio Code, se necesita acceso adicional de proveedor de recursos para mostrar la lista de registros de contenedor de Azure. En este caso, proporcione a los usuarios acceso al rol **Lector** o **Colaborador**. Estos roles permiten `docker pull`, `docker push`, `az acr list`, `az acr build` y otras funciones. 

## <a name="access-resource-manager"></a>Acceso a Resource Manager

Se necesita obtener acceso a Azure Resource Manager para Azure Portal y a la administración de registros con la [CLI de Azure](/cli/azure/). Por ejemplo, para obtener una lista de registros mediante el comando `az acr list`, necesita este conjunto de permisos. 

## <a name="create-and-delete-registry"></a>Crear y eliminar registros

Capacidad para crear y eliminar registros de contenedor de Azure.

## <a name="push-image"></a>Imagen de inserción

La capacidad para `docker push` una imagen o insertar otro [artefacto compatible](container-registry-image-formats.md), como un gráfico de Helm, en un registro. Se necesita [autenticación](container-registry-authentication.md) con el registro mediante la identidad autorizada. 

## <a name="pull-image"></a>Extraer imagen

La capacidad para `docker pull` una imagen que no esté en cuarentena o extraer otro [artefacto compatible](container-registry-image-formats.md), como un gráfico de Helm, de un registro. Se necesita [autenticación](container-registry-authentication.md) con el registro mediante la identidad autorizada.

## <a name="delete-image-data"></a>Eliminación de los datos de la imagen

La capacidad de [eliminar imágenes de contenedor](container-registry-delete.md), o eliminar otros [admiten artefactos](container-registry-image-formats.md) como gráficos de Helm, desde un registro.

## <a name="change-policies"></a>Cambiar directivas

Capacidad para configurar directivas en un registro. En las directivas, se incluye la purga de imágenes, la activación de la cuarentena y la firma de imágenes.

## <a name="sign-images"></a>Firmar imágenes

Capacidad para firmar imágenes, que suelen estar asignadas a un proceso automatizado, lo que usaría una entidad de servicio. Este permiso suele combinarse con la inserción de imágenes para permitir [insertar una imagen](#push-image) de confianza en un registro. Para obtener más información, vea [Confianza de contenido en Azure Container Registry](container-registry-content-trust.md).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre cómo asignar roles RBAC a una identidad de Azure mediante [Azure Portal](../role-based-access-control/role-assignments-portal.md), la [CLI de Azure](../role-based-access-control/role-assignments-cli.md) u otras herramientas de Azure.

* Obtenga información sobre las [opciones de autenticación](container-registry-authentication.md) de Azure Container Registry.

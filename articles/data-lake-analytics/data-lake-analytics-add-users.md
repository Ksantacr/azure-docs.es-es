---
title: Agregar usuarios a una cuenta de Azure Data Lake Analytics
description: Obtenga información sobre cómo agregar correctamente usuarios a su cuenta de Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 8323c4e1b236444f55dab826d2567491f5f0f736
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60629329"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Agregar un usuario en Azure Portal

## <a name="start-the-add-user-wizard"></a>Inicio del Asistente para agregar usuarios
1. Abra Azure Data Lake Analytics mediante https://portal.azure.com.
2. Haga clic en **Asistente para agregar usuarios**.
3. En el paso **Seleccionar usuario**, busque al usuario que quiera agregar. Haga clic en **Seleccionar**.
4. En el paso **Seleccionar rol**, seleccione **Desarrollador de Data Lake Analytics**. Este rol tiene el conjunto mínimo de permisos necesarios para enviar, supervisar y administrar trabajos de U-SQL. Asigne este rol si el grupo no está pensado para administrar servicios de Azure.
5. En el paso **Seleccionar permisos del catálogo**, seleccione cualquier base de datos adicional a la que pueda necesitar acceder el usuario. Es necesario que el usuario disponga de acceso de lectura y escritura para poder enviar trabajos. Cuando haya terminado, haga clic en **Aceptar**.
6. En el paso final, denominado **Asignar los permisos seleccionados**, revise los cambios que realizará el asistente. Haga clic en **OK**.


## <a name="configure-acls-for-data-folders"></a>Configuración de las ACL para carpetas de datos
Conceda "R-X" o "RWX", según sea necesario, a las carpetas con datos de entrada y salida.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>Opcionalmente, puede agregar el usuario al rol **Lector** de Azure Data Lake Storage Gen1.
1.  Busque la cuenta de Azure Data Lake Storage Gen1.
2.  Haga clic en **Usuarios**.
3. Haga clic en **Agregar**.
4.  Seleccione un rol RBAC de Azure para asignarlo a este grupo.
5.  Asigne el rol Lector. Este rol tiene el conjunto mínimo de permisos necesarios para examinar y administrar datos almacenados en ADLSGen1. Asigne este rol si el grupo no está pensado para administrar servicios de Azure.
6.  Escriba el nombre del grupo.
7.  Haga clic en **OK**.

## <a name="adding-a-user-using-powershell"></a>Adición de un usuario mediante PowerShell

1. Siga las instrucciones de esta guía: [Cómo instalar y configurar Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Descargue el script de PowerShell [Add-AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1).
3. Ejecute el script de PowerShell. 

El comando de ejemplo para conceder acceso a los usuarios para enviar trabajos, ver los metadatos de trabajos nuevos y ver antiguos metadatos es el siguiente:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Pasos siguientes

* [Información general de Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Introducción al uso de Azure Portal por parte de Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
* [Manage Azure Data Lake Analytics by using Azure PowerShell](data-lake-analytics-manage-use-powershell.md) (Administración de Azure Data Lake Analytics mediante Azure PowerShell)


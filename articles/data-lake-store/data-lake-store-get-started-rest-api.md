---
title: 'API REST: Operaciones de administración en Azure Data Lake Storage Gen1 cuentas | Microsoft Docs'
description: Use Azure Data Lake Storage Gen1 y la API REST de WebHDFS para realizar operaciones de administración de cuentas en la cuenta de Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 97fe33309f36cd7545f8c9d6c2d34671641caa1f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60877118"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>Operaciones de administración de cuentas en Azure Data Lake Storage Gen1 mediante API REST
> [!div class="op_single_selector"]
> * [SDK de .NET](data-lake-store-get-started-net-sdk.md)
> * [API DE REST](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

En este artículo aprenderá a realizar operaciones de administración de cuentas en Azure Data Lake Storage Gen1 con la API REST. Las operaciones de administración de cuentas incluyen,por ejemplo, la creación o la eliminación de cuentas de Data Lake Storage Gen1. Para obtener instrucciones sobre cómo realizar operaciones del sistema de archivos en Data Lake Storage Gen1 con la API REST, consulte [Filesystem operations on Data Lake Storage Gen1 using REST API](data-lake-store-data-operations-rest-api.md) (Operaciones del sistema de archivos en Data Lake Storage Gen1 con la API REST).

## <a name="prerequisites"></a>Requisitos previos
* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](https://curl.haxx.se/)**. En este artículo se usa cURL para demostrar cómo realizar llamadas de la API de REST en una cuenta de Data Lake Storage Gen1.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>¿Cómo se puede autenticar mediante Azure Active Directory?
Puede usar dos enfoques para autenticar con Azure Active Directory.

* Para la autenticación del usuario final para la aplicación (interactiva), consulte el artículo sobre la [autenticación del usuario final con Data Lake Storage Gen1 mediante el SDK de .NET](data-lake-store-end-user-authenticate-rest-api.md).
* Para la autenticación entre servicios para la aplicación (no interactiva), consulte el artículo sobre la [autenticación entre servicios con Data Lake Storage Gen1 mediante el SDK de .NET](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-storage-gen1-account"></a>Creación de una cuenta de Data Lake Storage Gen1
Esta operación se basa en la llamada de la API de REST que se define [aquí](https://docs.microsoft.com/rest/api/datalakestore/accounts/create).

Use el siguiente comando cURL. Reemplace **\<yourstoragegen1name>** con el nombre de Data Lake Storage Gen1.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

En el comando anterior, reemplace \<`REDACTED`\> por el token de autorización que recuperó anteriormente. La carga útil de la solicitud de este comando se encuentra en el archivo **input.json** que se proporciona para el parámetro `-d` anterior. El contenido del archivo input.json es similar al siguiente fragmento de código:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-storage-gen1-account"></a>Eliminación de una cuenta de Data Lake Storage Gen1
Esta operación se basa en la llamada de la API de REST que se define [aquí](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete).

Para eliminar una cuenta de Data Lake Storage Gen1 use el siguiente comando cURL. Reemplace **\<yourstoragegen1name>** con el nombre de cuenta de Data Lake Storage Gen1.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview

Debe ver algo parecido al siguiente fragmento de código:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Pasos siguientes
* [Filesystem operations on Data Lake Storage Gen1 using REST API](data-lake-store-data-operations-rest-api.md) (Operaciones del sistema de archivos en Data Lake Storage Gen1 mediante la API REST).

## <a name="see-also"></a>Vea también
* [Azure Data Lake Storage Gen1 REST API Reference](https://docs.microsoft.com/rest/api/datalakestore/) (Referencia sobre la API de REST de Azure Data Lake Storage Gen1)
* [Open Source Big Data applications compatible with Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md) (Abrir aplicaciones Big Data de origen que funcionan con Azure Data Lake Storage Gen1)


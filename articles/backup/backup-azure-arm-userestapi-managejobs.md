---
title: 'Azure Backup: Administrar trabajos de copia de seguridad mediante la API de REST'
description: Administración de trabajos de copia de seguridad y restauración de Azure Backup mediante API REST
services: backup
author: pvrk
manager: shivamg
keywords: API REST; copia de seguridad de Azure VM; restauración de Azure VM;
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b234533e-ac51-4482-9452-d97444f98b38
ms.openlocfilehash: eb8b7dc77d180eb56c2585e93e60a36742f6c84c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60646629"
---
# <a name="track-backup-and-restore-jobs-using-rest-api"></a>Seguimiento de los trabajos de copia de seguridad y restauración mediante API REST

El servicio Azure Backup desencadena los trabajos que se ejecutan en segundo plano en diversos escenarios, como al desencadenar la copia de seguridad, en las operaciones de restauración o al deshabilitar la copia de seguridad. Estos trabajos se pueden seguir mediante sus identificadores.

## <a name="fetch-job-information-from-operations"></a>Captura de información sobre el trabajo de las operaciones

Una operación como desencadenar la copia de seguridad siempre devolverá un valor de jobID. Para p. ej.: La respuesta final de un [desencadenar la operación de API de REST de copia de seguridad](backup-azure-arm-userestapi-backupazurevms.md#example-responses-3) es como sigue:

```http
{
  "id": "cd153561-20d3-467a-b911-cc1de47d4763",
  "name": "cd153561-20d3-467a-b911-cc1de47d4763",
  "status": "Succeeded",
  "startTime": "2018-09-12T02:16:56.7399752Z",
  "endTime": "2018-09-12T02:16:56.7399752Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "41f3e94b-ae6b-4a20-b422-65abfcaf03e5"
  }
}
```

El trabajo de copia de seguridad de las máquinas virtuales de Azure se identifica mediante el campo "jobId" y se puede seguir como se mencionó [aquí](https://docs.microsoft.com/rest/api/backup/jobdetails/) mediante una simple solicitud *GET*.

## <a name="tracking-the-job"></a>Seguimiento del trabajo

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2017-07-01
```

El `{jobName}` es el "jobId" mencionado anteriormente. La respuesta es siempre 200 (OK) y el campo "status" indica el estado actual del trabajo. Una vez pasa a "Completed" o "CompletedWithWarnings", la sección "extendedInfo" revela más detalles sobre el trabajo.

### <a name="response"></a>Response

|NOMBRE  |Type  |DESCRIPCIÓN  |
|---------|---------|---------|
|200 OK     | [JobResource](https://docs.microsoft.com/rest/api/backup/jobdetails/get#jobresource)        | OK        |

#### <a name="example-response"></a>Respuesta de ejemplo

Una vez que se emita el identificador URI de *GET*, se devuelve una respuesta 200 (OK).

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-client-request-id: ba4dff71-1655-4c1d-a71f-c9869371b18b; ba4dff71-1655-4c1d-a71f-c9869371b18b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14989
x-ms-correlation-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-routing-request-id: SOUTHINDIA:20180521T102317Z:e9702101-9da2-4681-bdf3-a54e17329a56
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:23:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-RecoveryServices-ResourceGroup-centralindia/providers/microsoft.recoveryservices/vaults/abdemovault/backupJobs/7ddead57-bcb9-4269-ac31-6a1b57588700",
  "name": "7ddead57-bcb9-4269-ac31-6a1b57588700",
  "type": "Microsoft.RecoveryServices/vaults/backupJobs",
  "properties": {
    "jobType": "AzureIaaSVMJob",
    "duration": "00:20:23.0896697",
    "actionsInfo": [
      1
    ],
    "virtualMachineVersion": "Compute",
    "extendedInfo": {
      "tasksList": [
        {
          "taskId": "Take Snapshot",
          "duration": "00:00:00",
          "status": "Completed"
        },
        {
          "taskId": "Transfer data to vault",
          "duration": "00:00:00",
          "status": "Completed"
        }
      ],
      "propertyBag": {
        "VM Name": "uttestvmub1",
        "Backup Size": "2332 MB"
      }
    },
    "entityFriendlyName": "uttestvmub1",
    "backupManagementType": "AzureIaasVM",
    "operation": "Backup",
    "status": "Completed",
    "startTime": "2018-05-21T08:35:40.9488967Z",
    "endTime": "2018-05-21T08:56:04.0385664Z",
    "activityId": "7df8e874-1d66-4f81-8e91-da2fe054811d"
  }
}
}

```
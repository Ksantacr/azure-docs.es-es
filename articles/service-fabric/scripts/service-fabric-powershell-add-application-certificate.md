---
title: 'Ejemplo de script de Azure PowerShell: agregar certificado de aplicación a un clúster | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: agregar un certificado de aplicación a un clúster de Service Fabric.'
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 313a1b7e4f0109a0a5cb94214918d97c25b5cf5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66153845"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Incorporación de un certificado de aplicación a un clúster de Service Fabric

Este script de ejemplo crea un certificado autofirmado en el almacén de claves de Azure especificado y lo instala en todos los nodos del clúster de Service Fabric. El certificado también se descarga en una carpeta local. El nombre del certificado descargado es el mismo que el nombre del certificado del almacén de claves. Personalice los parámetros según sea necesario.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si es necesario, instale Azure PowerShell con la instrucción que se encuentra en la [guía de Azure PowerShell](/powershell/azure/overview) y luego ejecute `Connect-AzAccount` para crear una conexión con Azure. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Add-AzServiceFabricApplicationCertificate](/powershell/module/az.servicefabric/Add-azServiceFabricApplicationCertificate) | Agregue un nuevo certificado de aplicación al conjunto de escalado de máquinas virtuales que componen el clúster.  |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Puede encontrar ejemplos de Azure PowerShell para Azure Service Fabric en los [ejemplos de PowerShell](../service-fabric-powershell-samples.md).

---
title: Visualización de la entidad de servicio de una identidad administrada mediante PowerShell
description: Instrucciones detalladas para ver la entidad de servicio de una identidad administrada mediante PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5aa5ab96ae85833e59ed40161cafcc1eb88379be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60441787"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Visualización de la entidad de servicio de una identidad administrada mediante PowerShell

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, obtendrá información sobre cómo visualizar la entidad de servicio de una identidad administrada mediante PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para obtener una cuenta gratuita](https://azure.microsoft.com/free/).
- Habilite la [identidad asignada por el sistema en una máquina virtual](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) o [aplicación](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- [Instalar la versión más reciente de Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="view-the-service-principal"></a>Visualización de la entidad de servicio

El comando siguiente muestra cómo ver la entidad de servicio de una aplicación o máquina virtual con la identidad asignada por el sistema habilitada. Reemplazar `<VM or application name>` por sus propios valores.

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo ver las entidades de servicio de Azure AD mediante PowerShell, consulte [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).



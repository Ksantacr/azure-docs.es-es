---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165820"
---
## <a name="start-your-powershell-session"></a>Inicio de una sesión de PowerShell
 

Ejecute el cmdlet [**Connect-Az Account**](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) y aparecerá una pantalla de inicio de sesión donde podrá especificar sus credenciales. Use las mismas credenciales que las utilizadas para iniciar sesión en el Portal de Azure.

```powershell
Connect-AzAccount
```

Si tiene varias suscripciones, use el cmdlet [**Set-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) para seleccionar la suscripción que se debe usar en la sesión de PowerShell. Para ver la suscripción que se usa en la sesión actual de PowerShell, ejecute [**Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Para ver todas las suscripciones, ejecute [**Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```


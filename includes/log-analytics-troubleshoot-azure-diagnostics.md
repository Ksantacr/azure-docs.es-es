---
author: mgoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 11/09/2018
ms.author: magoedte
ms.openlocfilehash: 6890c71ac7c265d46cc77751786fea4d0b228588
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66129701"
---
### <a name="troubleshoot-azure-diagnostics"></a>Solución de problemas de Diagnósticos de Azure

Si recibe el siguiente mensaje de error, significa que el proveedor de recursos Microsoft.insights no está registrado:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Para registrar el proveedor de recursos, realice los pasos siguientes en el portal de Azure:

1.  En el panel de navegación de la izquierda, haga clic en *Suscripciones*
2.  Selección de la suscripción identificada en el mensaje de error
3.  Haga clic en *Proveedores de recursos*
4.  Busque el proveedor *Microsoft.insights*.
5.  Haga clic en el vínculo *Registrar*.

![Registro del proveedor de recursos de microsoft.insights](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Una vez registrado el proveedor de recursos *Microsoft.insights*, intente de nuevo los diagnósticos de configuración.


En PowerShell, si recibe el siguiente mensaje de error, debe actualizar su versión de PowerShell:

`Set-AzDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Actualice su versión de Azure PowerShell, siga las instrucciones de la [instalar Azure PowerShell](/powershell/azure/install-az-ps) artículo.

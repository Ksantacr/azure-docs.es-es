---
title: Reenviar datos de informes a los registros de Azure Monitor de configuración de estado de Azure Automation
description: Este artículo muestra cómo enviar Desired State Configuration (DSC) datos de informes de estado de configuración de Azure Automation a los registros de Azure Monitor para ofrecer información adicional y administración.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 0dad74f75fd7b73e7dab0b2dddbdfda193d5b2ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61073944"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-azure-monitor-logs"></a>Reenviar datos de informes a los registros de Azure Monitor de configuración de estado de Azure Automation

Configuración de estado de automatización de Azure conserva los datos de estado del nodo durante 30 días.
Puede enviar datos de estado del nodo en el área de trabajo de Log Analytics si prefiere conservar esos datos durante un período más largo.
El estado de cumplimiento se puede ver en Azure Portal, o con PowerShell, tanto para los nodos como para los recursos de DSC individuales en las configuraciones de los nodos.
Con los registros de Azure Monitor, puede:

- Obtener información de cumplimiento de nodos administrados y recursos individuales
- Desencadenar un correo electrónico o una alerta basados en el estado de cumplimiento
- Escribir consultas avanzadas en los nodos administrados
- Correlacionar el estado de cumplimiento en las cuentas de Automation
- Visualizar el historial de cumplimiento de los nodo con el paso del tiempo

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

Para empezar a enviar los informes de configuración de estado de automatización a los registros de Azure Monitor, necesita:

- La versión de noviembre de 2016, o una posterior, de [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
- Una cuenta de Azure Automation Para más información, consulte [Introducción a Azure Automation](automation-offering-get-started.md)
- Un área de trabajo de Log Analytics con una oferta de servicio de **Automation & Control**. Para obtener más información, consulte [empezar a trabajar con registros de Azure Monitor](../log-analytics/log-analytics-get-started.md).
- Al menos un nodo de Azure Automation State Configuration. Para más información, consulte [Incorporación de máquinas para administrarlas con Azure Automation State Configuration](automation-dsc-onboarding.md)

## <a name="set-up-integration-with-azure-monitor-logs"></a>Configurar la integración con los registros de Azure Monitor

Para comenzar a importar datos de DSC de automatización de Azure en los registros de Azure Monitor, complete los pasos siguientes:

1. Inicie sesión en su cuenta de Azure en PowerShell. Consulte [Inicio de sesión con Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)
1. Obtenga el valor de _ResourceId_ de su cuenta de Automation mediante la ejecución del siguiente comando de PowerShell: (si tiene más de una cuenta de Automation, elija el valor de _ResourceID_ de la cuenta que desea configurar).

   ```powershell
   # Find the ResourceId for the Automation Account
   Get-AzureRmResource -ResourceType 'Microsoft.Automation/automationAccounts'
   ```

1. Obtenga el valor de _ResourceId_ del área de trabajo de Log Analytics mediante la ejecución del siguiente comando de PowerShell: (si tiene más de un área de trabajo, elija el valor de _ResourceID_ del área de trabajo que desea configurar).

   ```powershell
   # Find the ResourceId for the Log Analytics workspace
   Get-AzureRmResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
   ```

1. Ejecute el siguiente comando de PowerShell, pero reemplace `<AutomationResourceId>` y `<WorkspaceResourceId>` por los valores de _ResourceId_ valores de cada uno de los pasos anteriores:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories 'DscNodeStatus'
   ```

Si desea detener la importación de datos de configuración de estado de automatización de Azure en los registros de Azure Monitor, ejecute el siguiente comando de PowerShell:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories 'DscNodeStatus'
```

## <a name="view-the-state-configuration-logs"></a>Visualización de los registros de State Configuration

Después de configurar la integración con los registros de Azure Monitor para los datos de configuración de estado de Automation, un **búsqueda de registros** botón aparecerá en la **nodos DSC** hoja de su cuenta de automation. Haga clic en el botón **Búsqueda de registros** para ver los registros de los datos del nodo DSC.

![Botón Búsqueda de registros](media/automation-dsc-diagnostics/log-search-button.png)

Se abre la hoja **Búsqueda de registros**, donde se ve una operación **DscNodeStatusData** para cada nodo de State Configuration y una operación **DscResourceStatusData** para cada [recurso de DSC](/powershell/dsc/resources) llamado en la configuración que se aplica a un nodo.

La operación **DscResourceStatusData** contiene información de los recursos de DSC en que se produjeron errores.

Haga clic en la operación que desee de la lista para ver los datos de la misma.

También puede ver los registros mediante la búsqueda en los registros de Azure Monitor.
Consulte [Búsqueda de datos mediante búsquedas de registros](../log-analytics/log-analytics-log-searches.md).
Escriba la siguiente consulta para buscar los registros de State Configuration:`Type=AzureDiagnostics ResourceProvider='MICROSOFT.AUTOMATION' Category='DscNodeStatus'`

La consulta también se puede restringir por nombre de operación. Por ejemplo: `Type=AzureDiagnostics ResourceProvider='MICROSOFT.AUTOMATION' Category='DscNodeStatus' OperationName='DscNodeStatusData'`

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Envío de un correo electrónico cuando se produce un error en cualquier comprobación de cumplimiento de State Configuration

Uno de nuestros principales clientes solicita la capacidad para enviar un correo electrónico o un mensaje de texto cuando aparece algún problema en una configuración de DSC.

Para crear una regla de alerta, primero debe crear una búsqueda de registros para los registros del informe de State Configuration que deben invocar la alerta. Haga clic en el botón **+ Nueva regla de alertas** para crear y configurar la regla de alerta.

1. En la página de información general del área de trabajo de Log Analytics, haga clic en **registros**.
1. Cree una consulta de búsqueda de registros para la alerta; para ello, escriba la siguiente búsqueda en el campo de la consulta: `Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Si ha configurado registros de más de una cuenta de Automation o suscripción a su área de trabajo, puede agrupar las alertas por suscripción o cuenta de Automation.  
   El nombre de la cuenta de Automation puede derivarse del campo Resource (Recurso) en la búsqueda de DscNodeStatusData.  
1. Para abrir la pantalla **Crear regla**, haga clic en **+ Nueva regla de alertas** en la parte superior de la página. Para obtener más información sobre las opciones para configurar la alerta, consulte [crear una regla de alerta](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Búsqueda de recursos de DSC con errores en todos los nodos

Una ventaja de usar los registros de Azure Monitor es que puede buscar comprobaciones con errores en todos los nodos.
Para buscar todas las instancias de recursos de DSC con errores:

1. En la página de información general del área de trabajo de Log Analytics, haga clic en **registros**.
1. Cree una consulta de búsqueda de registros para la alerta; para ello, escriba la siguiente búsqueda en el campo de la consulta: `Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Visualización del historial de estado del nodo de DSC

Por último, puede desear visualizar el historial de estado del nodo de DSC con el paso del tiempo.  
Esta consulta se puede usar para buscar el estado del nodos de DSC con el paso del tiempo.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Se mostrará un gráfico del estado del nodo con el paso del tiempo.

## <a name="azure-monitor-logs-records"></a>Azure Monitor registra registros

Diagnósticos de Azure Automation crea dos categorías de registros en los registros de Azure Monitor.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Propiedad | DESCRIPCIÓN |
| --- | --- |
| TimeGenerated |Fecha y hora en que se ejecutó la comprobación de cumplimiento. |
| OperationName |DscNodeStatusData |
| ResultType |Si el nodo es compatible. |
| NodeName_s |El nombre del nodo administrado. |
| NodeComplianceStatus_s |Si el nodo es compatible. |
| DscReportStatus |Si la comprobación de cumplimiento se ejecutó correctamente. |
| ConfigurationMode | Cómo se aplica la configuración al nodo. Los valores posibles son __"ApplyOnly"__, __"ApplyandMonitior"__ y __"ApplyandAutoCorrect"__. <ul><li>__ApplyOnly__: DSC se aplica la configuración y es lo único que hace, salvo que se inserte una nueva configuración en el nodo de destino o cuando se extrae una nueva configuración de un servidor. Después de la aplicación inicial de una nueva configuración, DSC no comprueba si se ha producido una desviación desde un estado configurado previamente. DSC intenta aplicar la configuración hasta que sea la correcta antes de __ApplyOnly__ surta efecto. </li><li> __ApplyAndMonitor__: Este es el valor predeterminado. El LCM aplica las nuevas configuraciones. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desvía del estado deseado, DSC notifica la discrepancia en los registros. DSC intenta aplicar la configuración hasta que sea la correcta antes de __ApplyAndMonitor__ surta efecto.</li><li>__ApplyAndAutoCorrect__: DSC aplica las nuevas configuraciones. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desvía del estado deseado, DSC notifica la discrepancia en los registros y vuelve a aplicar la configuración actual.</li></ul> |
| HostName_s | El nombre del nodo administrado. |
| IPAddress | La dirección IPv4 del nodo administrado. |
| Category | DscNodeStatus |
| Resource | El nombre de la cuenta de Azure Automation. |
| Tenant_g | GUID que identifica al inquilino para el llamador. |
| NodeId_g |GUID que identifica el nodo administrado. |
| DscReportId_g |GUID que identifica el informe. |
| LastSeenTime_t |Fecha y hora en que el informe se vio por última vez. |
| ReportStartTime_t |Fecha y hora en que el informe se inició. |
| ReportEndTime_t |Fecha y hora en que el informe se completó. |
| NumberOfResources_d |El número de recursos de DSC que se llama en la configuración que se aplica al nodo. |
| SourceSystem | Cómo los registros de Azure Monitor recopilan los datos. Siempre *Azure* para Diagnósticos de Azure. |
| ResourceId |Especifica la cuenta de Azure Automation. |
| ResultDescription | La descripción de esta operación. |
| SubscriptionId | El identificador de la suscripción de Azure (GUID) para la cuenta de Automation. |
| ResourceGroup | Nombre del grupo de recursos de la cuenta de Automation. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID que es el identificador de correlación del informe de cumplimiento. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Propiedad | DESCRIPCIÓN |
| --- | --- |
| TimeGenerated |Fecha y hora en que se ejecutó la comprobación de cumplimiento. |
| OperationName |DscResourceStatusData|
| ResultType |Si el recurso es compatible. |
| NodeName_s |El nombre del nodo administrado. |
| Category | DscNodeStatus |
| Resource | El nombre de la cuenta de Azure Automation. |
| Tenant_g | GUID que identifica al inquilino para el llamador. |
| NodeId_g |GUID que identifica el nodo administrado. |
| DscReportId_g |GUID que identifica el informe. |
| DscResourceId_s |El nombre de la instancia del recurso de DSC. |
| DscResourceName_s |El nombre del recurso de DSC. |
| DscResourceStatus_s |Si el recurso de DSC es compatible. |
| DscModuleName_s |El nombre del módulo de PowerShell que contiene el recurso de DSC. |
| DscModuleVersion_s |La versión del módulo de PowerShell que contiene el recurso de DSC. |
| DscConfigurationName_s |El nombre de la configuración aplicada al nodo. |
| ErrorCode_s | El código de error si se produjo un error en el recurso. |
| ErrorMessage_s |El mensaje de error si se produjo un error en el recurso. |
| DscResourceDuration_d |El tiempo, en segundos, durante el que se ejecutó el recurso de DSC. |
| SourceSystem | Cómo los registros de Azure Monitor recopilan los datos. Siempre *Azure* para Diagnósticos de Azure. |
| ResourceId |Especifica la cuenta de Azure Automation. |
| ResultDescription | La descripción de esta operación. |
| SubscriptionId | El identificador de la suscripción de Azure (GUID) para la cuenta de Automation. |
| ResourceGroup | Nombre del grupo de recursos de la cuenta de Automation. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID que es el identificador de correlación del informe de cumplimiento. |

## <a name="summary"></a>Resumen

Mediante el envío de los datos de configuración de estado de automatización a los registros de Azure Monitor, puede obtener una visión mejor el estado de los nodos de configuración de estado de automatización por:

- Establecimiento de alertas que le notificarán cuando haya un problema
- Uso de vistas personalizadas y de consultas de búsqueda para visualizar los resultados de runbook, el estado del trabajo de runbook y otras métricas o indicadores clave relacionados.  

Registros de Azure Monitor proporciona mayor visibilidad operativa para los datos de configuración de estado de Automation y puede ayudar a resolver incidentes más rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general, consulte [Azure Automation State Configuration](automation-dsc-overview.md).
- Para empezar a usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md)
- Para obtener información acerca de la compilación de configuraciones de DSC para que poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en Azure Automation State Configuration](automation-dsc-compile.md)
- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para obtener información de precios, consulte [Precios de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/)
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)
- Para obtener más información sobre cómo crear diferentes consultas de búsqueda y a revisar los registros de configuración de estado de automatización con registros de Azure Monitor, consulte [búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md)
- Para obtener más información acerca de los registros de Azure Monitor y orígenes de recopilación de datos, vea [información general sobre los registros de datos de recopilación de Azure storage en Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md)

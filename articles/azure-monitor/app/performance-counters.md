---
title: Contadores de rendimiento en Application Insights | Microsoft Docs
description: Supervise los contadores de rendimiento de .NET, tanto del sistema como personalizados, en Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mbullwin
ms.openlocfilehash: 0ec64a5ae412fb4a1900021fefcb7d9112b1b019
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255340"
---
# <a name="system-performance-counters-in-application-insights"></a>Contadores de rendimiento de sistema en Application Insights

Windows proporciona una amplia variedad de [contadores de rendimiento](https://docs.microsoft.com/windows/desktop/PerfCtrs/about-performance-counters), como la ocupación de la CPU, memoria, disco y uso de la red. También puede definir sus propios contadores de rendimiento. Se admite la recopilación de contadores de rendimiento siempre y cuando la aplicación se está ejecutando bajo IIS en un host local o máquina virtual a la que tiene acceso administrativo. Sin embargo aplicaciones que se ejecutan como Azure Web Apps no tiene acceso directo a los contadores de rendimiento, un subconjunto de contadores disponibles son recopilados por Application Insights.

## <a name="view-counters"></a>Visualización de contadores

El panel Métricas muestra el conjunto predeterminado de contadores de rendimiento.

![Contadores de rendimiento notificados en Application Insights](./media/performance-counters/performance-counters.png)

Los contadores predeterminados actual que están configurados para recopilarse para aplicaciones web ASP.NET/ASP.NET Core son:

         - % Process\\Processor Time
         - % Process\\Processor Time Normalized
         - Memory\\Available Bytes
         - ASP.NET Requests/Sec
         - .NET CLR Exceptions Thrown / sec
         - ASP.NET ApplicationsRequest Execution Time
         - Process\\Private Bytes
         - Process\\IO Data Bytes/sec
         - ASP.NET Applications\\Requests In Application Queue
         - Processor(_Total)\\% Processor Time

## <a name="add-counters"></a>Adición de contadores

Si el contador de rendimiento que quiere no está incluido en la lista de métricas, puede agregarlo.

1. Averigüe qué contadores están disponibles en el servidor mediante este comando de PowerShell en el servidor local:

    `Get-Counter -ListSet *`

    (Consulte [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx)).
2. Abra ApplicationInsights.config.

   * Si agrega Application Insights a la aplicación durante el desarrollo, edite ApplicationInsights.config en el proyecto y vuelva a implementarlo en los servidores.
3. Edite la directiva del recopilador de rendimiento:

```XML

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

> [!NOTE]
> Aplicaciones de ASP.NET Core no tienen `ApplicationInsights.config`, y por lo tanto, el método anterior no es válido para las aplicaciones de ASP.NET Core.

Puede capturar tanto los contadores estándar como los que ha implementado usted mismo. `\Objects\Processes` es un ejemplo de contador estándar que está disponible en todos los sistemas Windows. `\Sales(photo)\# Items Sold` es un ejemplo de contador personalizado que podría implementarse en un servicio web.

El formato es `\Category(instance)\Counter"` o, para las categorías que no tienen instancias, simplemente `\Category\Counter`.

`ReportAs` es necesario para los nombres de contadores que no coinciden `[a-zA-Z()/-_ \.]+`: es decir, que contienen caracteres que no están en los siguientes conjuntos: letras, paréntesis, barra diagonal, guión, subrayado, espacio, punto.

Si especifica una instancia, se recopilará como una dimensión "CounterInstanceName" de la métrica notificada.

### <a name="collecting-performance-counters-in-code-for-aspnet-web-applications-or-netnet-core-console-applications"></a>Recopilar contadores de rendimiento en el código para aplicaciones Web de ASP.NET o aplicaciones de consola de.NET/.NET Core
Para recopilar contadores de rendimiento del sistema y enviarlos a Application Insights, puede adaptar el siguiente fragmento:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Process([replace-with-application-process-name])\Page Faults/sec", "PageFaultsPerfSec")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

También puede hacer lo mismo con las métricas personalizadas que haya creado:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="collecting-performance-counters-in-code-for-aspnet-core-web-applications"></a>Recopilar los contadores de rendimiento en el código para aplicaciones Web de ASP.NET Core

Modificar `ConfigureServices` método en su `Startup.cs` clase como sigue.

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures PerformanceCollectorModule.
  services.ConfigureTelemetryModule<PerformanceCollectorModule>((module, o) =>
            {
                // the application process name could be "dotnet" for ASP.NET Core self-hosted applications.
                module.Counters.Add(new PerformanceCounterCollectionRequest(
    @"\Process([replace-with-application-process-name])\Page Faults/sec", "DotnetPageFaultsPerfSec"));
            });
    }
```

## <a name="performance-counters-in-analytics"></a>Contadores de rendimiento en Analytics
Puede buscar y mostrar informes de contador de rendimiento en [Analytics](../../azure-monitor/app/analytics.md).

El esquema **performanceCounters** expone `category`, el nombre de `counter` y el nombre de `instance` de cada contador de rendimiento.  En la telemetría de cada aplicación, solo se ven los contadores de dicha aplicación. Por ejemplo, para ver qué contadores están disponibles: 

![Contadores de rendimiento en Application Insights Analytics](./media/performance-counters/analytics-performance-counters.png)

(Aquí "Instance" hace referencia a la instancia de contador de rendimiento, no al rol ni a la instancia de máquina de servidor. El nombre de la instancia del contador de rendimiento normalmente segmenta contadores, como el tiempo de procesador por el nombre del proceso o la aplicación).

Para obtener un gráfico de la memoria disponible en un período reciente: 

![Gráfico de tiempo de la memoria in Application Insights Analytics](./media/performance-counters/analytics-available-memory.png)

Al igual que otros datos de telemetría, **performanceCounters** también tiene una columna `cloud_RoleInstance` que indica la identidad de la instancia del servidor host en el que se ejecuta la aplicación. Por ejemplo, para comparar el rendimiento de una aplicación en distintas máquinas: 

![Rendimiento segmentado por instancia de rol en Application Insights Analytics](./media/performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>Recuentos de ASP.NET y Application Insights

*¿En qué se diferencian la tasa de excepciones y las métricas de excepciones?*

* *tasa de excepciones* es un contador de rendimiento del sistema. El CLR cuenta todas las excepciones controladas y no controladas que se producen, y divide el total de un intervalo de muestreo entre la duración del intervalo. El SDK de Application Insights recopila este resultado y lo envía al portal.

* *Excepciones* es un recuento de los informes de TrackException recibidos a través del portal en el intervalo de muestreo del gráfico. Solo incluye las excepciones controladas para las que ha escrito llamadas a TrackException en el código y no incluye todas las [excepciones no controladas](../../azure-monitor/app/asp-net-exceptions.md). 

## <a name="performance-counters-for-applications-running-in-azure-web-apps"></a>Contadores de rendimiento para aplicaciones que se ejecutan en Azure Web Apps

Las aplicaciones de ASP.NET y ASP.NET Core implementadas en Azure Web Apps se ejecutan en un entorno de recinto de seguridad especiales. Este entorno no permite el acceso directo a los contadores de rendimiento del sistema. Sin embargo, un subconjunto limitado de los contadores se exponen como variables de entorno tal como se describe [aquí](https://github.com/projectkudu/kudu/wiki/Perf-Counters-exposed-as-environment-variables). SDK de Application Insights para ASP.NET y ASP.NET Core recopila contadores de rendimiento de Azure Web Apps de estas variables de entorno especiales. Solo un subconjunto de contadores están disponibles en este entorno, y se puede encontrar la lista completa [aquí.](https://github.com/microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/PerformanceCollector/Perf.Shared/Implementation/WebAppPerformanceCollector/CounterFactory.cs)

## <a name="performance-counters-in-aspnet-core-applications"></a>Contadores de rendimiento en aplicaciones de ASP.NET Core

* [SDK de ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versión 2.4.1 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Azure Web Apps (Windows)

* SDK versión 2.7.0-beta3 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Windows y destinadas a `NETSTANDARD2.0` o superior.
* Para las aplicaciones destinadas a .NET Framework, los contadores de rendimiento se admiten en todas las versiones SDK de.
* En este artículo se actualizará cuando se agrega compatibilidad con contadores de rendimiento en los que no sean Windows.

## <a name="alerts"></a>Alertas
Al igual que otras métricas, puede [establecer una alerta](../../azure-monitor/app/alerts.md) para advertirle si un contador de rendimiento queda fuera de un límite especificado. Abra el panel de alertas y haga clic en Agregar alerta.

## <a name="next"></a>Pasos siguientes

* [Seguimiento de dependencias](../../azure-monitor/app/asp-net-dependencies.md)
* [Seguimiento de excepciones](../../azure-monitor/app/asp-net-exceptions.md)


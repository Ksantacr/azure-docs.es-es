---
title: Exploración de registros de seguimiento de .NET en Application Insights
description: Busque registros generados por el seguimiento, NLog o Log4Net.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbullwin
ms.openlocfilehash: d366f363b7bd1d5306d598c9b38258eb78076b7c
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472048"
---
# <a name="explore-netnet-core-trace-logs-in-application-insights"></a>Exploración de los registros de seguimiento de .NET y .NET Core en Application Insights

Envío de registros de seguimiento de diagnóstico para la aplicación ASP.NET/ASP.NET Core desde ILogger, NLog, log4Net o System.Diagnostics.Trace para [Azure Application Insights][start]. A continuación, puede explorar y buscar en ellos. Estos registros se combinarán con otros archivos de registro de la aplicación, para que pueda identificar los seguimientos que están asociadas con cada solicitud de usuario y correlacionan con otros eventos e informes de excepción.

> [!NOTE]
> ¿Necesita el módulo de captura de registros? Es un adaptador útil para registradores de terceros. Pero si ya no usa NLog, log4Net o System.Diagnostics.Trace, considere la posibilidad de llamar [ **Application Insights TrackTrace()** ](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) directamente.
>
>
## <a name="install-logging-on-your-app"></a>Instalación del registro en la aplicación
Instale el marco de registro elegido en el proyecto, que debería dar una entrada en app.config o web.config.

```XML
    <configuration>
      <system.diagnostics>
    <trace autoflush="true" indentsize="0">
      <listeners>
        <add name="myAppInsightsListener" type="Microsoft.ApplicationInsights.TraceListener.ApplicationInsightsTraceListener, Microsoft.ApplicationInsights.TraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
   </configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Configuración de Application Insights para recopilar registros
[Agregar Application Insights al proyecto](../../azure-monitor/app/asp-net.md) si aún no lo ha hecho aún. Verá una opción para incluir el compilador de registros.

O haga clic en el proyecto en el Explorador de soluciones para **configurar Application Insights**. Seleccione el **configurar la recopilación de seguimiento** opción.

> [!NOTE]
> ¿Ninguna opción de recopilador Application Insights menú o de registro? Pruebe la [solución de problemas](#troubleshooting).

## <a name="manual-installation"></a>Instalación manual
Utilice este método si el tipo de proyecto no es compatible con el programa de instalación de Application Insights (por ejemplo, un proyecto de escritorio de Windows).

1. Si planea usar log4Net o NLog, instálelo en su proyecto.
2. En el Explorador de soluciones, haga clic en el proyecto y seleccione **administrar paquetes de NuGet**.
3. Busque "Application Insights".
4. Seleccione uno de los siguientes paquetes:

   - Para ILogger: [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.Extensions.Logging.ApplicationInsights.svg)](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
   - Para NLog: [Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.NLogTarget.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
   - Para Log4Net: [Microsoft.ApplicationInsights.Log4NetAppender](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.Log4NetAppender.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
   - Para System.Diagnostics: [Microsoft.ApplicationInsights.TraceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.TraceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
   - [Microsoft.ApplicationInsights.DiagnosticSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.DiagnosticSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
   - [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EtwCollector.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
   - [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EventSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)

El paquete NuGet instala a los ensamblados necesarios y modifica el archivo web.config o app.config si es aplicable.

## <a name="ilogger"></a>ILogger

Para obtener ejemplos del uso de la implementación de Application Insights ILogger con aplicaciones de consola y ASP.NET Core, vea [ApplicationInsightsLoggerProvider para .NET Core ILogger registra](ilogger.md).

## <a name="insert-diagnostic-log-calls"></a>Insertar llamadas de registro de diagnóstico
Si usa System.Diagnostics.Trace, una llamada típica sería:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Si prefiere log4net o NLog, use:

    logger.Warn("Slow response - database01");

## <a name="use-eventsource-events"></a>Usar eventos EventSource
Puede configurar eventos [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) para enviarse a Application Insights como seguimientos. Primero, instale el paquete de NuGet `Microsoft.ApplicationInsights.EventSourceListener`. Seguidamente, edite la sección `TelemetryModules` del archivo [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

En cada origen, puede establecer los siguientes parámetros:
 * **Nombre** especifica el nombre del evento EventSource para recopilar.
 * **Nivel** especifica el nivel de registro para recopilar: *Crítica*, *Error*, *informativo*, *LogAlways*, *detallado*, o *advertencia*.
 * **Palabras clave** (opcional) Especifique el valor entero de combinaciones de palabras clave para usar.

## <a name="use-diagnosticsource-events"></a>Usar eventos de DiagnosticSource
Puede configurar eventos [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) para enviarlos a Application Insights como seguimientos. Primero, instale el paquete de NuGet [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener). A continuación, edite la sección "TelemetryModules" de la [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) archivo.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Para cada instancia de DiagnosticSource que quiera seguir, agregue una entrada con el **nombre** atributo establecido en el nombre de la instancia DiagnosticSource.

## <a name="use-etw-events"></a>Usar eventos ETW
Puede configurar eventos de seguimiento de eventos para Windows (ETW) para enviarse a Application Insights como seguimientos. Primero, instale el paquete de NuGet `Microsoft.ApplicationInsights.EtwCollector`. A continuación, edite la sección "TelemetryModules" de la [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) archivo.

> [!NOTE] 
> Solo se pueden recopilar eventos ETW si el proceso que hospeda el SDK se ejecuta bajo una identidad que sea miembro de usuarios del registro de rendimiento o los administradores.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

En cada origen, puede establecer los siguientes parámetros:
 * **ProviderName** es el nombre del proveedor de ETW para recopilar.
 * **ProviderGuid** especifica el GUID del proveedor de ETW para recopilar. Se puede usar en lugar de `ProviderName`.
 * **Nivel** establece el nivel de registro para recopilar. Puede ser *crítico*, *Error*, *informativo*, *LogAlways*, *detallado*, o *Advertencia*.
 * **Palabras clave** (opcional) establezca el valor de entero de combinaciones de palabras clave para usar.

## <a name="use-the-trace-api-directly"></a>Usar la API de seguimiento directamente
Puede llamar directamente a la API de seguimiento de Application Insights. Los adaptadores de registro usan esta API.

Por ejemplo:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Una ventaja de TrackTrace es que puede colocar datos relativamente largos en el mensaje. Por ejemplo, aquí puede codificar datos POST.

También puede agregar un nivel de gravedad al mensaje. Y, al igual que otros datos de telemetría, puede agregar valores de propiedad para ayudar a filtrar o buscar distintos conjuntos de seguimientos. Por ejemplo:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Esto permitiría filtrar fácilmente en [búsqueda] [ diagnostic] todos los mensajes de un determinado nivel de gravedad que se relacionan con una base de datos determinada.

## <a name="explore-your-logs"></a>Explorar los registros
Ejecute la aplicación en modo de depuración o implementarlo en vivo.

En el panel de información general de la aplicación en [el portal de Application Insights][portal], seleccione [búsqueda][diagnostic].

Por ejemplo, puede:

* Filtrar seguimientos de registros o elementos con determinadas propiedades.
* Inspeccionar un elemento específico en detalle
* Buscar otros datos de registro del sistema que se relaciona con la misma solicitud de usuario (tiene el mismo OperationId).
* Guarde la configuración de una página como favorita.

> [!NOTE]
>Si la aplicación envía una gran cantidad de datos y usa el SDK de Application Insights para ASP.NET versión 2.0.0-beta3 o posterior, el *muestreo adaptable* característica puede operar y enviar sólo una parte de los datos de telemetría. [Aprenda más sobre el muestreo.](../../azure-monitor/app/sampling.md)
>

## <a name="troubleshooting"></a>solución de problemas
### <a name="how-do-i-do-this-for-java"></a>¿Cómo se puede hacer para Java?
Utilice los [adaptadores de registro de Java](../../azure-monitor/app/java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>No aparece la opción de Application Insights en el menú contextual del proyecto
* Asegúrese de que Developer Analytics Tools está instalado en el equipo de desarrollo. En Visual Studio **herramientas** > **extensiones y actualizaciones**, busque **Developer Analytics Tools**. Si no se encuentra en la **instalado** pestaña, abra el **Online** pestaña e instalarlo.
* Podría tratarse de un tipo de proyecto que no es compatible con las herramientas de análisis Devloper. Use la [instalación manual](#manual-installation).

### <a name="theres-no-log-adapter-option-in-the-configuration-tool"></a>No hay ninguna opción de adaptador de registro en la herramienta de configuración
* Instale primero la plataforma de registro.
* Si usa System.Diagnostics.Trace, asegúrese de que haya [configurado en *web.config*](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Asegúrese de que tiene la versión más reciente de Application Insights. En Visual Studio, vaya a **herramientas** > **extensiones y actualizaciones**y abra el **actualizaciones** ficha. Si **Developer Analytics Tools** está allí, selecciónela para actualizarlo.

### <a name="emptykey"></a>Puede obtener el mensaje de error "clave de instrumentación no puede estar vacía"
Probablemente ha instalado el paquete de Nuget de adaptador de registro sin tener que instalar Application Insights. En el Explorador de soluciones, haga clic en *ApplicationInsights.config*y seleccione **actualizar Application Insights**. Se le pedirá que inicie sesión en Azure y crear un recurso de Application Insights o use uno existente. Que debe corregir el problema.

### <a name="i-can-see-traces-but-not-other-events-in-diagnostic-search"></a>Puedo ver los seguimientos, pero no otros eventos en búsqueda de diagnóstico
Puede tardar un rato para todos los eventos y las solicitudes para obtener a través de la canalización.

### <a name="limits"></a>¿Qué cantidad de datos se conserva?
Varios factores afectan a la cantidad de datos que se conservarán. Para obtener más información, consulte el [límites](../../azure-monitor/app/api-custom-events-metrics.md#limits) sección de la página de métricas de eventos de cliente.

### <a name="i-dont-see-some-log-entries-that-i-expected"></a>No veo algunas entradas del registro que esperaba
Si la aplicación envía grandes volúmenes de datos y usa el SDK de Application Insights para ASP.NET versión 2.0.0-beta3 o posterior, la característica de muestreo adaptativo puede operar y enviar sólo una parte de los datos de telemetría. [Aprenda más sobre el muestreo.](../../azure-monitor/app/sampling.md)

## <a name="add"></a>Pasos siguientes

* [Diagnóstico de errores y excepciones en ASP.NET][exceptions]
* [Más información sobre la búsqueda][diagnostic]
* [Configuración de pruebas de disponibilidad y de capacidad de respuesta][availability]
* [Solución de problemas][qna]

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md
---
title: 'Optimización de la supervisión y el rendimiento: Azure SQL Database | Microsoft Docs'
description: Sugerencias para la optimización del rendimiento de Azure SQL Database a través de la evaluación y la mejora.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 2fa43fcd48736a3d044deb07ed690af580c3b987
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416278"
---
# <a name="monitoring-and-performance-tuning"></a>Optimización de la supervisión y el rendimiento

Habilita la base de datos de SQL Azure supervisar con facilidad de uso, agregar o quitar recursos (CPU, memoria y E/S), solucionar los posibles problemas y buscar recomendaciones que pueden mejorar el rendimiento de la base de datos. Azure SQL Database tiene muchas características que pueden corregir automáticamente los problemas en las bases de datos si desea permitir que la base de datos de adaptarse a la carga de trabajo y optimizar el rendimiento. Sin embargo, hay algunos problemas personalizados que puede necesitar solucionar problemas. En este artículo se explica algunas prácticas recomendadas y herramientas que puede usar para solucionar los problemas de rendimiento.

Hay dos actividades principales que necesita hacer para asegurarse de que esa base de datos se está ejecutando sin problemas:
- [Supervisión del rendimiento de la base de datos](#monitoring-database-performance) con el fin de asegurarse de que los recursos asignados a la base de datos pueden controlar la carga de trabajo. Si ve que está alcanzando los límites de recursos, necesitaría para identificar las consultas que consumen más recursos y optimizarlas o agregar más recursos mediante la actualización de nivel de servicio.
- [Solucionar problemas de rendimiento](#troubleshoot-performance-issues) con el fin de identificar por qué se produjo algún problema, identificar la causa raíz del problema y la acción que se solucionará el problema.

## <a name="monitoring-database-performance"></a>Supervisar el rendimiento de la base de datos

La supervisión del rendimiento de una base de datos SQL en Azure comienza con la supervisión del uso de recursos, en relación con el nivel de rendimiento elegido para la base de datos. Debe supervisar los siguientes recursos:
 - **Uso de CPU** -deberá comprobar están llegando a 100% de uso de CPU en un período de tiempo más largo. Esto podría indicar que es posible que deba actualizar base de datos o instancia o identificar y optimizar las consultas que usan la mayoría de la capacidad de proceso.
 - **Estadísticas de espera** -deba comprobar qué por qué las consultas están esperando para algunos recursos. Queriesmig esperar que se captura o se guardan en los archivos de base de datos, porque se alcanza algún límite de recursos en espera, etcetera de datos.
 - **El uso de E/S** -deberá comprobar están llegando a los límites de E/S de almacenamiento subyacente.
 - **Uso de memoria** -es la cantidad de memoria disponible para su base de datos o la instancia es proporcional al número de núcleos virtuales, y debe comprobar lo suficiente como para la carga de trabajo. Esperanza de vida es uno de los parámetros que pueden indicar que las páginas rápidamente quitan de la memoria.

La base de datos de SQL Azure **proporciona los avisos que pueden ayudar a solución y corrección posibles problemas de rendimiento**. Puede identificar fácilmente las oportunidades para mejorar y optimizar el rendimiento de las consultas sin cambiar los recursos mediante la revisión [recomendaciones de optimización del rendimiento](sql-database-advisor.md). Los motivos comunes por los que se obtiene poco rendimiento de la base de datos son la falta de índices o la poca optimización de las consultas. Puede aplicar estas recomendaciones de optimización para mejorar el rendimiento de la carga de trabajo. También puede permitir que Azure SQL Database [optimice el rendimiento de las consultas automáticamente](sql-database-automatic-tuning.md) aplicando todas las recomendaciones identificadas y comprobando que mejoran el rendimiento de la base de datos.

Dispone de las siguientes opciones para supervisar el rendimiento de la base de datos y solucionar problemas relacionados con dicho rendimiento:

- En [Azure Portal](https://portal.azure.com), haga clic en **Bases de datos SQL**, seleccione la base de datos y luego use el gráfico de supervisión para buscar recursos que se acerquen a su máximo. El consumo de DTU se muestra de manera predeterminada. Haga clic en **Editar** para cambiar el intervalo de tiempo y los valores que se muestran.
- Herramientas como SQL Server Management Studio proporcionan varios informes muy útiles, como un [panel rendimiento](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard?view=sql-server-2017) donde puede supervisar la utilización de recursos e identificar las consultas, que consumen más recursos o [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed)donde puede identificar las consultas con peor rendimiento.
- Use [Query Performance Insight](sql-database-query-performance.md) el [portal Azure](https://portal.azure.com) para identificar las consultas que consumen la mayoría de los recursos. Esta característica solo está disponible en la base de datos única y los grupos elásticos.
- Usar [SQL Database Advisor](sql-database-advisor-portal.md) para ver recomendaciones para crear y quitar índices, parametrizar consultas y solucionar problemas de esquema. Esta característica solo está disponible en la base de datos única y los grupos elásticos.
- Use [Intelligent Insights de Azure SQL](sql-database-intelligent-insights.md) para supervisar automáticamente el rendimiento de su base de datos. Una vez que se detecta un problema de rendimiento, se genera un registro de diagnóstico con los detalles y el análisis de la causa principal (RCA) del problema. Cuando es posible, se proporciona una recomendación para la mejora del rendimiento.
- [Habilitar la optimización automática](sql-database-automatic-tuning-enable.md) y dejar que Azure SQL Database corrija los problemas de rendimiento automáticamente.
- Puede usar las [vistas de administración dinámica (DMV)](sql-database-monitoring-with-dmvs.md), los [eventos extendidos](sql-database-xevent-db-diff-from-svr.md) y el [almacén de datos de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para obtener soluciones más detalladas de los problemas de rendimiento.

> [!TIP]
> Consulte la [guía de rendimiento](sql-database-performance-guidance.md) para buscar las técnicas que puede usar para mejorar el rendimiento de Azure SQL Database si identifica algún problema de rendimiento al usar uno o más de los métodos detallados anteriormente.

## <a name="troubleshoot-performance-issues"></a>Solución de problemas de rendimiento

Para diagnosticar y resolver problemas de rendimiento, debe comprender el estado de cada consulta activa y las condiciones que causan esos problemas de rendimiento, ya que son relevantes para cada estado de la carga de trabajo. Para mejorar el rendimiento de Azure SQL Database, debe comprender que cada solicitud de consulta activa de la aplicación está en ejecución o en estado de espera. Al buscar una solución para un problema de rendimiento en Azure SQL Database, tenga en cuenta el siguiente gráfico cuando lea este artículo; le ayudará a diagnosticar y resolver los problemas de rendimiento.

![Estados de carga de trabajo](./media/sql-database-monitor-tune-overview/workload-states.png)

Para una carga de trabajo con problemas de rendimiento, dichos problemas pueden deberse a la contención de la CPU (una condición **relacionada con la ejecución**) o a la existencia de consultas individuales que esperan algo (una condición **relacionada con la espera**).

Las causas o **relacionadas con la ejecución** problemas podrían ser:
- **Problemas de compilación** -optimizador de consultas de SQL podría generar un plan poco óptimo debido a estadísticas obsoletas, la estimación del número de filas que se procesarán incorrecta o la estimación de memoria necesaria. Si sabe que se ha ejecutado con más rapidez en el pasado o en otra instancia (instancia de instancia administrada o SQL Server), desconecte los planes de ejecución real y comparar que vean son diferentes. Intente aplicar sugerencias de consulta o vuelve a generar estadísticas o índices que va a obtener el mejor plan. Habilitar la corrección automática de planes en Azure SQL Database para mitigar estos problemas de forma automática.
- **Problemas de ejecución** : si el plan de consulta es óptimo, a continuación, probablemente esté alcanzando algunos límites de recursos en la base de datos, como el rendimiento de escritura de registro o podría usar desfragmentados índices que se deben volver a generar. Un gran número de consultas simultáneas que se pasan los recursos también podría ser la causa de problemas de ejecución. **Relacionadas con la espera** son problemas en la mayoría de los casos relacionados con los problemas de ejecución, ya que las consultas que no se está ejecutando eficazmente probablemente están esperando para algunos recursos.

Las causas o **relacionadas con la espera** problemas podrían ser:
- **Bloqueo** -una consulta es posible que mantiene el bloqueo de algunos objetos de base de datos, mientras que otros usuarios están intentando obtener acceso a los mismos objetos. Puede identificar fácilmente las consultas de bloqueo mediante la DMV o las herramientas de supervisión.
- **Problemas de E/S** -consultas podrían estar esperando las páginas se escriban en los archivos de datos o de registro. En este caso verá `INSTANCE_LOG_RATE_GOVERNOR`, `WRITE_LOG`, o `PAGEIOLATCH_*` estadísticas en la DMV de espera.
- **Problemas de TempDB** : si está usando una gran cantidad de tablas temporales o consulte los derrames de un lote de TempDB en los planes de las consultas es posible que tenga un problema con el rendimiento de TempDB. 
- **Problemas relacionados con la memoria** -puede que no tenga suficiente memoria para la carga de trabajo por lo que podría colocar la esperanza de vida o las consultas obtienen menos concesión de memoria que necesitan. En algunos casos, la inteligencia integrada en el optimizador de consultas soluciona estos problemas.
 
En las secciones siguientes se explica cómo identificar y solucionar algunos de estos problemas.

## <a name="running-related-performance-issues"></a>Problemas de rendimiento relacionados con la ejecución

Como pauta general, si el uso de la CPU se mantiene en un 80 % o por encima de este porcentaje, tiene un problema de rendimiento relacionado con la ejecución. Si tiene un problema relacionado con la ejecución, puede deberse a que los recursos de la CPU son insuficientes o puede estar relacionado con una de las siguientes condiciones:

- Demasiadas consultas en ejecución
- Demasiadas consultas en compilación
- Una o varias consultas en ejecución usan un plan de consulta que no alcanza el nivel óptimo

Si resulta que tiene un problema de rendimiento relacionado con la ejecución, su objetivo es identificar el problema en cuestión mediante uno o más métodos. Los métodos más comunes para identificar problemas relacionados con la ejecución son los siguientes:

- Use [Azure Portal](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal) para supervisar el porcentaje de uso de la CPU.
- Use las [vistas de administración dinámicas](sql-database-monitoring-with-dmvs.md) siguientes:

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve el consumo de CPU, E/S y memoria para una base de datos de Azure SQL Database. Hay una fila para cada 15 segundos, incluso si no hay ninguna actividad en la base de datos. Los datos históricos se conservan durante una hora.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve datos de almacenamiento y uso de CPU para una base de datos de Azure SQL. Los datos se recopilan y agregan en intervalos de cinco minutos.

> [!IMPORTANT]
> Para obtener un conjunto de consultas de T-SQL que usan estas DMV para solucionar problemas de uso de la CPU, consulte [Identify CPU performance issues](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues) (Identificar problemas de rendimiento de la CPU).

### <a name="ParamSniffing"></a> Solucionar problemas de consultas con problemas de plan de ejecución de consulta sensibles a parámetros

El problema del plan que distingue entre parámetros (PSP) se refiere a un escenario donde el optimizador de consultas genera un plan de ejecución de consultas que es óptimo solo para un valor de parámetro específico (o un conjunto de valores) y el plan almacenado en caché no es óptimo para los valores de parámetros que se usan en ejecuciones consecutivas. Los planes no óptimos pueden crear problemas de rendimiento en las consultas y resultar en una degradación general del rendimiento de la carga de trabajo. Para obtener más información sobre el procesamiento de consultas y el examen de parámetros, vea el [Guía de arquitectura de procesamiento de consultas](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

Existen varias soluciones alternativas que se pueden usar para mitigar los problemas, aunque cada una cuenta con sus ventajas y desventajas correspondientes:

- Use la sugerencia de consulta [RECOMPILE](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) en cada ejecución de consulta. Esta solución alternativa tiene en cuenta el tiempo de compilación y el aumento del uso de la CPU para obtener mejor calidad en el plan. Si usa la opción `RECOMPILE`, verá que a menudo no es posible llevarla a cabo en cargas de trabajo que requieren un alto rendimiento.
- Use la sugerencia de consulta [OPTION (OPTIMIZE FOR…)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para anular el valor del parámetro real con un valor de parámetro típico que cree un plan lo suficientemente bueno para la mayoría de las posibilidades que ofrece el valor de parámetro.   Esta opción requiere tener una buena comprensión de los valores óptimos de los parámetros y de las características del plan asociado.
- Use la sugerencia de consulta [OPTION (OPTIMIZE FOR UNKNOWN)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para invalidar el valor del parámetro real y así poder usar la media del vector de densidad. Otra forma de hacerlo es capturar los valores de los parámetros entrantes en variables locales y luego usar esas variables locales en los predicados en lugar de usar los parámetros en sí. La densidad media debe ser lo *suficientemente buena* para que funcione esta corrección en particular.
- Desactive el examen de parámetros por completo mediante la sugerencia de consulta [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query).
- Use la sugerencia de consulta [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para evitar volver a compilar el contenido mientras esté en la caché. Esta solución asume que el plan común que ya está en la caché es lo *suficientemente bueno*. También puede deshabilitar las actualizaciones automáticas de las estadísticas para reducir la posibilidad de que el plan sea expulsado y se compile uno nuevo.
- Para forzar el plan, use de forma explícita la sugerencia de consulta [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query); para especificarlo explícitamente, configure un plan específico con el Almacén de consultas o habilitando el [ajuste automático](sql-database-automatic-tuning.md).
- Reemplace el procedimiento único con un conjunto anidado de procedimientos que se pueden usar según la lógica condicional y los valores de los parámetros asociados.
- Cree alternativas de ejecución de cadenas dinámicas en una definición de procedimiento estático.

Para obtener información adicional sobre cómo resolver este tipo de problemas, consulte:

- Esto [cerca un parámetro](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/) entrada de blog
- La entrada de blog [dynamic sql versus plan quality for parameterized queries](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/) (dynamic sql y la calidad del plan para consultas parametrizadas).
- Esto [técnicas de optimización de consulta SQL en SQL Server: Examen de parámetros](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/) entrada de blog

### <a name="troubleshooting-compile-activity-due-to-improper-parameterization"></a>Solución de problemas de la actividad de compilación debido a una parametrización incorrecta

Cuando una consulta tiene elementos literales, el motor de base de datos parametriza automáticamente la instrucción, aunque un usuario puede parametrizarla explícitamente para reducir el número de compilaciones. Un alto número de compilaciones de una consulta que usa el mismo patrón pero diferentes valores literales puede resultar en un uso elevado de la CPU. De manera similar, si solo parametriza parcialmente una consulta que sigue teniendo elementos literales, el motor de base de datos no la parametriza más.  A continuación se muestra un ejemplo de una consulta parcialmente parametrizada:

```sql
SELECT * FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

En el ejemplo anterior, `t1.c1` toma `@p1`, pero `t2.c2` continúa tomando GUID como elemento literal. En este caso, si cambia el valor de `c2`, la consulta se tratará como una consulta diferente y se producirá una nueva compilación. Para reducir las compilaciones del ejemplo anterior, la solución también pasa por parametrizar el GUID.

La siguiente consulta muestra el recuento de consultas en función del hash de consulta para determinar si una consulta está correctamente parametrizada o no:

```sql
SELECT  TOP 10  
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p 
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs 
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```
### <a name="factors-influencing-query-plan-changes"></a>Factores que influyen en los cambios del plan de consulta

Una recompilación de plan de ejecución de consultas puede producir un plan de consulta generado que difiere de lo que originalmente se almacenó en caché. Existen varias razones por qué podría compilarse automáticamente un plan existente original:
- Cambios en el esquema que se hace referencia la consulta
- Cambios de datos en las tablas que se hace referencia la consulta 
- Cambios realizados en las opciones de contexto de consulta 

Es posible que se va a expulsar un plan compilado desde la memoria caché para una variedad de motivos, incluidos se reinicia la instancia, base de datos con ámbito de los cambios de configuración, presión de memoria y las solicitudes explícitas para borrar la caché. Además, mediante una sugerencia RECOMPILE significa que no se almacena en caché un plan.

Todavía puede provocar una recompilación (o una compilación nueva después de la expulsión de caché) en la generación de un plan de ejecución de consulta idéntica de la que originalmente se observa.  Si, sin embargo, hay cambios en el plan en comparación con el plan original o anterior, estas son las explicaciones de por qué cambió un plan de ejecución de consulta más comunes:

- **Cambiar el diseño físico**. Por ejemplo, nuevos índices crean que más eficazmente cubren que los requisitos de una consulta se pueden utilizar en una nueva compilación si el optimizador de consultas decide que es más adecuada para aprovechar ese nuevo índice que usa la estructura de datos que se seleccionó originalmente para la primera versión de la ejecución de consultas.  Es posible que los cambios físicos a los objetos que se hace referencia en una nueva opción de plan en tiempo de compilación.

- **Diferencias de recursos de servidor**. En un escenario donde un plan difiere del "sistema A" frente a "el sistema B": la disponibilidad de recursos, como el número de procesadores disponibles, puede influir en el plan se genera.  Por ejemplo, si un sistema tiene un mayor número de procesadores, puede elegirse un plan paralelo. 

- **Estadísticas diferentes**. Las estadísticas asociadas con los objetos que se hace referencia cambiado o son significativamente diferentes de las estadísticas del sistema original.  Si cambia las estadísticas y se produce una recompilación, el optimizador de consultas usará las estadísticas a partir de ese momento concreto en el tiempo. Pueden tener las estadísticas revisadas significativamente distintas distribuciones de datos y las frecuencias que no eran el caso de la compilación original.  Estos cambios se utilizan para calcular las estimaciones de cardinalidad (número de filas que prevé que fluyen a través del árbol lógico de consulta).  Los cambios en las estimaciones de cardinalidad pueden dar lugar a nosotros para elegir diferentes operadores físicos y orden de operaciones asociadas.  Incluso los pequeños cambios en las estadísticas pueden dar lugar a un plan de ejecución de la consulta modificada.

- **Versión del estimador de cardinalidad o de nivel de compatibilidad base de datos modificados**.  Pueden permitir que los cambios realizados en el nivel de compatibilidad de base de datos nuevas estrategias y las características que pueden producir un plan de ejecución de consulta diferentes.  Más allá del nivel de compatibilidad de base de datos, cómo deshabilitar o habilitar la marca de seguimiento 4199 o cambiar el estado de la configuración con ámbito de base de datos QUERY_OPTIMIZER_HOTFIXES también puede influir en las opciones del plan de ejecución en tiempo de compilación de consulta.  Las marcas de seguimiento 9481 (forzar la CE heredada) y 2312 (forzar la CE predeterminada) también son planear que afectan a. 

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Resolver consultas de problemas o proporcionar más recursos

Una vez identificado el problema, puede ajustar las consultas del problema o actualizar el tamaño de proceso o el nivel de servicio para aumentar la capacidad de Azure SQL Database a fin de asimilar los requisitos de la CPU. Para obtener más información sobre el escalado de recursos de bases de datos únicas, consulte [Escalar recursos de base de datos única en Azure SQL Database](sql-database-single-database-scale.md) y, para el escalado de recursos de grupos elásticos, consulte [Escalar recursos de grupos elásticos en Azure SQL Database](sql-database-elastic-pool-scale.md). Para obtener información sobre cómo escalar una instancia administrada, consulte [Instance-level resource limits](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits) (Límites de recursos a nivel de instancia).

### <a name="determine-if-running-issues-due-to-increase-workload-volume"></a>Determinar si los problemas de ejecución se deben al aumento del volumen de la carga de trabajo

Un aumento en el tráfico de aplicaciones y la carga de trabajo puede explicar el aumento del uso de la CPU, pero debe tener cuidado a la hora de diagnosticar correctamente este problema. En un caso donde exista un uso elevado de la CPU, responda a estas preguntas para determinar si realmente ese aumento de uso de la CPU se debe a cambios en el volumen de trabajo:

1. ¿Son las consultas de la aplicación la causa del problema del uso elevado de la CPU?
2. Para las principales consultas que consumen CPU (que se puedan identificar):

   - Determine si había varios planes de ejecución asociados a la misma consulta. Si es así, determine por qué.
   - En cuanto a las consultas con el mismo plan de ejecución, determine si los tiempos de ejecución fueron consistentes y si el recuento de ejecución aumentó. Si es así, es probable que haya problemas de rendimiento debido al aumento de la carga de trabajo.

Para resumir, si el plan de ejecución de la consulta no se ejecutó de manera diferente pero el uso de la CPU aumentó junto con el recuento de ejecuciones, es probable que exista un problema de rendimiento relacionado con el aumento de la carga de trabajo.

No siempre es fácil concluir que hay un cambio en el volumen de trabajo que está provocando un problema en la CPU.   Factores que deben tenerse en cuenta: 

- **El uso de recursos ha cambiado**

  Por ejemplo, considere un escenario donde el uso de la CPU aumentó al 80 % por un período prolongado de tiempo.  El uso de la CPU por sí solo no significa que el volumen de la carga de trabajo haya cambiado.  Las regresiones del plan de ejecución de consultas y los cambios en la distribución de datos también pueden contribuir a un mayor uso de los recursos, incluso si la aplicación está ejecutando la misma carga de trabajo.

- **Apareció una nueva consulta**

   Una aplicación puede agregar un nuevo conjunto de consultas en diferentes momentos.

- **El número de solicitudes aumentó o disminuyó**

   Este escenario es la medida más obvia de la carga de trabajo. El número de consultas no siempre coincide con un mayor uso de los recursos. Sin embargo, esta métrica sigue siendo una señal significativa, suponiendo que otros factores no hayan cambiado.

## <a name="waiting-related-performance-issues"></a>Problemas de rendimiento relacionados con la espera

Una vez que esté seguro de que no se enfrenta a un problema de rendimiento relacionado con el uso elevado o la ejecución de una CPU, quedará claro que se enfrenta a un problema de rendimiento relacionado con la espera. Es decir, sus recursos de CPU no se están usando de manera eficiente porque la CPU está esperando a algún otro recurso. En este caso, el próximo paso es saber a qué están esperando los recursos de la CPU. Los métodos más comunes para mostrar las principales categorías de tipo de espera son los siguientes:

- El [Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) proporciona las estadísticas de espera por consulta con el tiempo. En el Almacén de consultas, los tipos de espera se combinan en categorías de espera. La asignación de categorías de espera a tipos de espera está disponible en [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) devuelve información acerca de todas las esperas encontradas por los subprocesos ejecutados durante la operación. Puede usar esta vista agregada para diagnosticar problemas de rendimiento con Azure SQL Database y también con consultas y lotes específicos.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) devuelve información acerca de la cola de espera de las tareas que están esperando en algún recurso.

En escenarios donde existe un uso elevado de la CPU, el Almacén de consultas y las estadísticas de espera no siempre reflejan el uso de la CPU por estos dos motivos:

- Es posible que las consultas que consumen mucha CPU aún se estén ejecutando y que las consultas no hayan finalizado.
- Las consultas que consumen mucha CPU se estaban ejecutando cuando se produjo una conmutación por error.

Las vistas de administración dinámica del Almacén de consultas y de seguimiento de estadísticas solo muestran los resultados de las consultas completadas con éxito y el tiempo de espera, pero no muestran los datos de las instrucciones que se están ejecutando actualmente (hasta que se completan). La vista de administración dinámica [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) le permite realizar un seguimiento de las consultas en ejecución y del tiempo de trabajo asociado.

Como se muestra en el gráfico anterior, las esperas más comunes son:

- Bloqueos
- E/S
- `tempdb`-contención relacionada
- Esperas de concesión de memoria

> [!IMPORTANT]
> Para obtener un conjunto de consultas de T-SQL que usan estas DMV para solucionar problemas relacionados con la espera, consulte lo siguiente:
>
> - [Identificar problemas de rendimiento de E/S](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar problemas de rendimiento de `tempdb` ](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar esperas de concesión de memoria](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)
> - [TigerToolbox - esperas y bloqueos temporales](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox - usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improving-database-performance-with-more-resources"></a>Mejora del rendimiento de la base de datos con más recursos

Por último, si no hay elementos para procesar que puedan mejorar el rendimiento de la base de datos, puede cambiar la cantidad de recursos disponibles en Azure SQL Database. Puede asignar más recursos cambiando el [nivel de servicio de DTU](sql-database-service-tiers-dtu.md) de una única base de datos o aumentar las eDTU de un grupo elástico en cualquier momento. O bien, si usa el [modelo de compra basado en núcleo virtual](sql-database-service-tiers-vcore.md), puede cambiar el nivel de servicio o aumentar los recursos asignados a la base de datos.

1. Para bases de datos individuales, puede [cambiar los niveles de servicio](sql-database-single-database-scale.md) o los [recursos de proceso](sql-database-single-database-scale.md) a petición para mejorar el rendimiento de la base de datos.
2. Para varias bases de datos, considere el uso de [grupos elásticos](sql-database-elastic-pool-guidance.md) para escalar recursos automáticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimización y refactorización del código de la aplicación o base de datos

Puede cambiar el código de la aplicación para usar la base de datos, cambiar índices, forzar planes o usar sugerencias de forma más óptima para adaptar manualmente la base de datos a la carga de trabajo. En el artículo sobre el [tema de la guía de rendimiento](sql-database-performance-guidance.md) puede buscar algunas pautas y sugerencias para la optimización manual y volver a escribir el código.

## <a name="next-steps"></a>Pasos siguientes

- Para habilitar la característica de ajuste automático en Azure SQL Database y permitir que administre totalmente la carga de trabajo, consulte [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md).
- Para utilizar la optimización manual, puede revisar [SQL Database Advisor mediante Azure Portal](sql-database-advisor-portal.md) y aplicar manualmente las recomendaciones que mejoran el rendimiento de las consultas.
- Cambiar los recursos que están disponibles en la base de datos cambiando los [niveles de servicio de Azure SQL Database](sql-database-performance-guidance.md).

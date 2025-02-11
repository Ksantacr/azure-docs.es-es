---
title: Modelo de datos para Azure Backup
description: En este artículo se explican los detalles del modelo de datos de Power BI para los informes de Azure Backup.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
origin.date: 06/26/2017
ms.date: 08/08/2018
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c6160570644da108ba713e8229b38f9587495c92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337585"
---
# <a name="data-model-for-azure-backup-reports"></a>Modelo de datos para informes de Azure Backup
En este artículo se describe el modelo de datos de Power BI utilizado para crear informes de Azure Backup. Con este modelo de datos, puede filtrar los informes existentes en función de los campos correspondientes, y lo que es más importante, crear sus propios informes mediante el uso de las tablas y campos del modelo. 

## <a name="creating-new-reports-in-power-bi"></a>Creación de informes nuevos en Power BI
Power BI proporciona características de personalización que se usan para [crear informes mediante el modelo de datos](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Uso del modelo de datos de Azure Backup
Los siguientes campos se pueden usar como parte del modelo de datos para crear informes y personalizar los informes existentes.

### <a name="alert"></a>Alerta
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados de la alerta.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Número entero |Número de alertas creadas en el período seleccionado |
| %ActiveAlertsCreatedInPeriod |Porcentaje |Porcentaje de alertas activas en el período seleccionado |
| %CriticalAlertsCreatedInPeriod |Porcentaje |Porcentaje de alertas críticas en el período seleccionado |
| AlertOccurrenceDate |Date |Fecha de creación de la alerta |
| AlertSeverity |Text |Gravedad de la alerta, por ejemplo, Crítica |
| AlertStatus |Text |Estado de la alerta, por ejemplo, Activa |
| AlertType |Text |Tipo de la alerta generada por ejemplo, Copia de seguridad |
| AlertUniqueId |Text |Identificador único de la alerta generada |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Número decimal |Tiempo medio (en minutos) para resolver la alerta en el período seleccionado |
| EntityState |Text |Estado actual del objeto de la alerta por ejemplo, Activo, Eliminado |

### <a name="backup-item"></a>Elemento de copia de seguridad
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el elemento de copia de seguridad.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #BackupItems |Número entero |Número de elementos de copia de seguridad |
| #UnprotectedBackupItems |Número entero |Número de elementos de copia de seguridad detenidos para su protección o configurados para que se realicen copias de seguridad, pero las copias de seguridad no se han iniciado|
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| BackupItemFriendlyName |Text |Nombre descriptivo del elemento de copia de seguridad |
| BackupItemId |Text |Identificador de elemento de copia de seguridad |
| BackupItemName |Text |Nombre de elemento de copia de seguridad |
| BackupItemType |Text |Tipo de elemento de copia de seguridad, por ejemplo, VM o FileFolder |
| EntityState |Text |Estado actual del objeto del elemento de copia de seguridad, por ejemplo, Active o Deleted |
| LastBackupDateTime |Fecha y hora |Hora de la última copia de seguridad del elemento de copia de seguridad seleccionado |
| LastBackupState |Text |Estado de la última copia de seguridad del elemento de copia de seguridad seleccionado, por ejemplo, Successful o Failed |
| LastSuccessfulBackupDateTime |Fecha y hora |Hora de última copia de seguridad correcta del elemento de copia de seguridad seleccionado |
| ProtectionState |Text |Estado actual de protección del elemento copia de seguridad, por ejemplo, Protected o ProtectionStopped |

### <a name="calendar"></a>Calendario
Esta tabla proporciona detalles acerca de los campos relacionados con el calendario.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| Date |Date |Fecha seleccionada para filtrar datos |
| DateKey |Text |Clave única para cada elemento de fecha |
| DayDiff |Número decimal |Diferencia en el día al filtrar datos, por ejemplo, 0 indican los datos del día actual, -1 indica los datos de un día anterior y 0 y -1 indican los datos del día actual y del anterior  |
| Mes |Text |Mes del año seleccionado para filtrar datos, el mes empieza el día 1 y termina el día 31 |
| MonthDate | Date |Fecha del mes en que finaliza el mes, se selecciona para filtrar datos |
| MonthDiff |Número decimal |Diferencia en el mes al filtrar datos, por ejemplo, 0 indican los datos del mes actual, -1 indica los datos de un mes anterior y 0 y -1 indican los datos del mes actual y del anterior |
| Semana |Text |Semana seleccionada para filtrar los datos; la semana comienza el domingo y termina el sábado |
| WeekDate |Date |Fecha de la semana en que finaliza la semana, se selecciona para filtrar datos |
| WeekDiff |Número decimal |Diferencia en la semana al filtrar datos, por ejemplo, 0 indican los datos de la semana actual, -1 indica los datos de una semana anterior y 0 y -1 indican los datos de la semana actual y de la anterior |
| Year |Text |Año natural para filtrar datos |
| YearDate |Fecha |Fecha del año en que finaliza el año, se selecciona para filtrar datos |

### <a name="job"></a>Trabajo
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el trabajo.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #JobsCreatedInPeriod |Número entero |Número de trabajos creados en el período seleccionado |
| %FailuresForJobsCreatedInPeriod |Porcentaje |Porcentaje de errores de los trabajos globales en el período seleccionado |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Número decimal |Valor del percentil 80 de los datos transferidos, en MB, para los trabajos de **copia de seguridad** creados en el período seleccionado |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Número decimal |Tiempo medio, en minutos, de los trabajos de **copia de seguridad completados** creados en el período seleccionado |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Número decimal |Tiempo medio, en minutos, de los trabajos de **restauración completados** creados en el período seleccionado |
| BackupStorageDestination |Text |Destino de almacenamiento de almacenamiento de copias de seguridad, por ejemplo, Cloud o Disk  |
| EntityState |Text |Estado actual del objeto del trabajo, por ejemplo, Active o Deleted |
| JobFailureCode |Text |Cadena del código de error por el que produjo el error del trabajo |
| JobOperation |Text |Operación para la que se ejecuta el trabajo, por ejemplo, Backup, Restore o Configure Backup |
| JobStartDate |Date |Fecha en que comenzó la ejecución del trabajo |
| JobStartTime |Hora |Hora en que comenzó la ejecución del trabajo |
| Estado del trabajo |Text |Estado del trabajo terminado, por ejemplo, Completed o Failed |
| JobUniqueId |Text |Identificador único que identifica el trabajo |

### <a name="policy"></a>Directiva
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con la directiva.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #Directivas |Número entero |Número de directivas de copia de seguridad que existen en el sistema |
| #PoliciesInUse |Número entero |Número de directivas que se usan actualmente para configurar copias de seguridad |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| BackupDaysOfTheWeek |Text |Días de la semana en los que se han programado copias de seguridad |
| BackupFrequency |Text |Frecuencia con la que se ejecutan las copias de seguridad, por ejemplo, a diario o semanalmente |
| BackupTimes |Text |Fecha y hora en que se programan las copias de seguridad |
| DailyRetentionDuration |Número entero |Duración de retención total de las copias de seguridad configuradas, en días |
| DailyRetentionTimes |Text |Fecha y hora en que se configuró la retención diaria |
| EntityState |Text |Estado actual del objeto de la directiva, por ejemplo, Active o Deleted |
| MonthlyRetentionDaysOfTheMonth |Text |Fechas del mes seleccionadas para la retención mensual |
| MonthlyRetentionDaysOfTheWeek |Text |Días de la semana seleccionados para la retención mensual |
| MonthlyRetentionDuration |Número decimal |Duración de retención total de las copias de seguridad configuradas, en meses |
| MonthlyRetentionFormat |Text |Tipo de configuración para la retención mensual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana |
| MonthlyRetentionTimes |Text |Fecha y hora en que se ha configurado la retención mensual |
| MonthlyRetentionWeeksOfTheMonth |Text |Semanas del mes en que se ha configurado la retención mensual, por ejemplo, la primera, la última, etc. |
| PolicyName |Text |Nombre de la directiva definida |
| PolicyUniqueId |Text |Identificador único que identifica la directiva |
| RetentionType |Text |Tipo de directiva de retención, por ejemplo, Daily, Weekly, Monthly, Yearly |
| WeeklyRetentionDaysOfTheWeek |Text |Días de la semana seleccionados para la retención semanal |
| WeeklyRetentionDuration |Número decimal |Duración total de la retención semanal de las copias de seguridad configuradas, en semanas |
| WeeklyRetentionTimes |Text |Fecha y hora en que se ha configurado la retención semanal |
| YearlyRetentionDaysOfTheMonth |Text |Fechas del mes seleccionadas para la retención anual |
| YearlyRetentionDaysOfTheWeek |Text |Días de la semana seleccionados para la retención anual |
| YearlyRetentionDuration |Número decimal |Duración total de la retención de las copias de seguridad configuradas, en años |
| YearlyRetentionFormat |Text |Tipo de configuración para la retención anual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana |
| YearlyRetentionMonthsOfTheYear |Text |Meses del año seleccionados para la retención anual |
| YearlyRetentionTimes |Text |Fecha y hora en que se ha configurado la retención anual |
| YearlyRetentionWeeksOfTheMonth |Text |Semanas del mes en que se ha configurado la retención anual, por ejemplo, la primera, la última, etc. |

### <a name="protected-server"></a>Servidor protegido
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el servidor protegido.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #ProtectedServers |Número entero |Número de servidores protegidos |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AzureBackupAgentOSType |Text |Tipo de sistema operativo de Azure Backup Agent |
| AzureBackupAgentOSVersion |Text |Versión del sistema operativo de Azure Backup Agent |
| AzureBackupAgentUpdateDate |Text |Fecha de actualización de Azure Backup Agent |
| AzureBackupAgentVersion |Text |Número de versión de Azure Backup Agent |
| BackupManagementType |Text |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder |
| EntityState |Text |Estado actual del objeto de servidor protegido, por ejemplo, Active o Deleted |
| ProtectedServerFriendlyName |Text |Nombre descriptivo del servidor protegido |
| ProtectedServerName |Text |Nombre del servidor protegido |
| ProtectedServerType |Text |Tipo de servidor protegido del que se realiza la copia de seguridad, por ejemplo, IaaSVMContainer |
| ProtectedServerName |Text |Nombre del servidor protegido al que pertenece el elemento de copia de seguridad |
| RegisteredContainerId |Text |Identificador de contenedor registrado para copia de seguridad |

### <a name="storage"></a>Almacenamiento
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el almacenamiento.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #ProtectedInstances |Número decimal |Número de instancias protegidas que se utilizan para calcular el almacenamiento de front-end en la facturación; se calcula en función del valor más reciente del tiempo seleccionado |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| CloudStorageInMB |Número decimal |Almacenamiento de copia de seguridad en la nube utilizado por las copias de seguridad, calculados basándose en el último valor de tiempo seleccionado |
| EntityState |Text |Estado actual del objeto, por ejemplo, Active o Deleted |
| LastUpdatedDate |Date |Fecha de última actualización de la fila seleccionada |

### <a name="time"></a>Hora
Esta tabla proporciona detalles acerca de los campos relacionados con el tiempo.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| Hora |Hora |Hora del día, por ejemplo, 1:00:00 p.m. |
| HourNumber |Número decimal |Número de hora en el día, por ejemplo, 13.00 |
| Minuto |Número decimal |Minuto de la hora |
| PeriodOfTheDay |Text |Intervalo de período en el día, por ejemplo, 12-3 a.m. |
| Hora |Hora |Hora del día, por ejemplo, 12:00:01 a.m. |
| TimeKey |Text |Valor de clave que representa el tiempo |

### <a name="vault"></a>Almacén
Esta tabla proporciona campos y agregaciones básicos en diversos campos relacionados con el almacén.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| #Vaults |Número entero |Número de almacenes |
| AsOnDateTime |Fecha y hora |Hora de la última actualización de la fila seleccionada |
| AzureDataCenter |Text |Centro de datos donde se encuentra el almacén |
| EntityState |Text |Estado actual del objeto del almacén por ejemplo, Activo, Eliminado |
| StorageReplicationType |Text |Tipo de replicación de almacenamiento para el almacén, por ejemplo, GeoRedundant |
| SubscriptionId |Text |Identificador de suscripción del cliente seleccionado para la generación de informes |
| VaultName |Text |Nombre del almacén |
| VaultTags |Text |Etiquetas asociadas al almacén |

## <a name="next-steps"></a>Pasos siguientes
Una vez que revise el modelo de datos para crear informes de Azure Backup, consulte los siguientes artículos para más información acerca de cómo crear y ver informes en Power BI.

- [Creación de un informe de Power BI nuevo mediante la importación de un conjunto de datos](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
- [Filtros y resaltado en informes de Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)


<!-- Update_Description: update metedata properties -->
---
title: Tipos de recursos que compatibles con Azure Resource Health | Microsoft Docs
description: Tipos de recursos que se admiten a través de Azure Resource Health
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 01/29/2019
ms.openlocfilehash: 0f79a1eed044814d6c2e27f4eadb5ba68a47303f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622290"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Tipos de recursos y comprobaciones de estado en Azure Resource Health
A continuación se muestra una lista completa de todas las comprobaciones que se ejecutan a través de Resource Health por tipos de recursos.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está el servidor en funcionamiento?</li><li>¿El servidor se ha quedado sin memoria?</li><li>¿Se está iniciando el servidor?</li><li>¿Se está recuperando el servidor?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿El servicio API Management está en funcionamiento?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Funcionan todos los nodos de la memoria caché?</li><li>¿Se puede acceder a la memoria caché desde el centro de datos?</li><li>¿Ha alcanzado la memoria caché el número máximo de conexiones?</li><li> ¿Ha agotado la caché su memoria disponible? </li><li>¿Sufre la memoria caché un gran número de errores de página?</li><li>¿Tiene mucha carga la memoria caché?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Comprobaciones ejecutadas|
|---|
|<ul> <li>¿Pueden acceder las operaciones de configuración de la red CDN al portal complementario?</li><li>¿Hay problemas de entrega en curso con los puntos de conexión de la red CDN?</li><li>¿Pueden los usuarios cambiar la configuración de sus recursos de la red CDN?</li><li>¿Se propagan los cambios en la configuración a la velocidad esperada?</li><li>¿Pueden los usuarios administrar la configuración de CDN mediante Azure Portal, Powershell o la API?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está el servidor en funcionamiento?</li><li>¿Se ha completado el arranque del sistema operativo host?</li><li>¿Está l contenedor de la máquina virtual aprovisionado y encendido?</li><li>¿Hay conectividad de red entre el host y la cuenta de almacenamiento?</li><li>¿Se ha completado el arranque del SO invitado?</li><li>¿Hay mantenimiento planeado en curso?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Se puede acceder a la cuenta desde el centro de datos?</li><li>¿Está disponible el proveedor de recursos de Cognitive Services?</li><li>¿Está disponible Cognitive Services en la región adecuada?</li><li>¿Se pueden realizar operaciones de lectura en la cuenta de almacenamiento que contiene los metadatos de recursos?</li><li>¿Se ha alcanzado la cuota de llamadas a la API?</li><li>¿Se ha alcanzado el límite de lectura de las llamadas a la API?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.compute/virtualmachines
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Funciona el servidor que hospeda esta máquina virtual?</li><li>¿Se ha completado el arranque del sistema operativo host?</li><li>¿Está l contenedor de la máquina virtual aprovisionado y encendido?</li><li>¿Hay conectividad de red entre el host y la cuenta de almacenamiento?</li><li>¿Se ha completado el arranque del SO invitado?</li><li>¿Hay mantenimiento planeado en curso?</li></ul>|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/Factories
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Ha habido errores de ejecución de canalización?</li><li>¿El clúster hospeda la factoría de datos en buen estado?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Los usuarios tienen problemas para enviar o mostrar sus trabajos de Data Lake Analytics?</li><li>¿Son los trabajos de Data Lake Analytics no se puede completar debido a errores del sistema?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Los usuarios tienen problemas para cargar datos a Data Lake Store?</li><li>¿Los usuarios tienen problemas para descargar datos de Data Lake Store?</li></ul>|

## <a name="microsoftdatamigrationservices"></a>Microsoft.datamigration/services
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿El servicio de migración de base de datos no pudo aprovisionar?</li><li>¿Se ha detenido el servicio de migración de base de datos debido a la solicitud de usuario o de inactividad?</li></ul>|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Es el servidor no está disponible debido a mantenimiento?</li><li>¿Es el servidor no está disponible debido a una reconfiguración?</li></ul>|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Es el servidor no está disponible debido a mantenimiento?</li><li>¿Es el servidor no está disponible debido a una reconfiguración?</li></ul>|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Es el servidor no está disponible debido a mantenimiento?</li><li>¿Es el servidor no está disponible debido a una reconfiguración?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Se está ejecutando el centro de IoT?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Ha habido solicitudes de bases de datos o recopilaciones que no se han atendido debido a una falta de disponibilidad del servicio Azure Cosmos DB?</li><li>¿Ha habido solicitudes de documentos que no se han atendido debido a una falta de disponibilidad del servicio Azure Cosmos DB?</li></ul>|

## <a name="microsofteventhubnamespaces"></a>Microsoft.eventhub/namespaces
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿El espacio de nombres de Event Hubs está experimentando errores generados por el usuario?</li><li>¿Es el espacio de nombres de Event Hubs se está actualizando?</li></ul>|

## <a name="microsofthdinsightclusters"></a>Microsoft.hdinsight/clusters
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Existen servicios principales en el clúster de HDInsight?</li><li>¿Puede acceder al clúster de HDInsight a la clave de cifrado de BYOK en reposo?</li></ul>|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Las solicitudes al almacén de claves producen errores debido a problemas de la plataforma de Azure KeyVault?</li><li>¿Se están limitando las solicitudes al almacén de claves porque el cliente está realizando demasiadas solicitudes?</li></ul>|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.network/applicationgateways
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Es el rendimiento de la puerta de enlace de aplicación degradada?</li><li>¿Es la puerta de enlace de aplicaciones disponible?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.network/connections
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está conectado el túnel VPN?</li><li>¿Existen conflictos de configuración en la conexión?</li><li>¿Están configuradas correctamente las claves precompartidas?</li><li>¿Se puede acceder al dispositivo local de VPN?</li><li>¿Hay discrepancias en la directiva de seguridad de IPSec/IKE?</li><li>¿Está la conexión VPN de sitio a sitio aprovisionada correctamente o está en estado de error?</li><li>¿Está la conexión entre redes virtuales aprovisionada correctamente o está en estado de error?</li></ul>|

## <a name="microsoftnetworkexpressreoutecircuits"></a>Microsoft.network/expressreoutecircuits
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Es correcto el circuito de ExpressRoute?</li></ul>|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.network/frontdoors
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿La puerta delantera back-ends responden con errores a los sondeos de estado?</li><li>¿Se retrasan los cambios de configuración?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Se puede acceder desde Internet a VPN Gateway?</li><li>¿Está VPN Gateway en modo de espera?</li><li>¿Se ejecuta el servicio VPN en la puerta de enlace?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Se pueden realizar operaciones de tiempo de ejecución, como el registro, la instalación o el envío, en el espacio de nombres?</li></ul>|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.operationalinsights/workspaces
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Hay retrasos del área de trabajo de indexación?</li></ul>|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/Capacities
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿El recurso de capacidad está en funcionamiento?</li><li>¿Todos las cargas de trabajo están en funcionamiento?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está el SO del host en funcionamiento?</li><li>¿Se puede acceder a workspaceCollection desde fuera del centro de datos?</li><li>¿Está disponible el proveedor de recursos de Power BI?</li><li>¿Es el servicio Power BI disponibles en la región adecuada?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Se pueden realizar operaciones de diagnóstico en el clúster?</li></ul>|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Los clientes experimentan errores de Service Bus generados por el usuario?</li><li>¿Los usuarios están experimentando un aumento de los errores transitorios debido a una actualización del espacio de nombres de Service Bus?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Comprobaciones ejecutadas|
|---|
|<ul><li> ¿Se han producido inicios de sesión en la base de datos?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Las solicitudes para leer datos de la cuenta de almacenamiento producen errores debido a problemas de la plataforma de Azure Storage?</li><li>¿Las solicitudes para escribir datos en la cuenta de almacenamiento producen errores debido a problemas de la plataforma de Azure Storage?</li><li>¿No está disponible el clúster de Storage en el que reside la cuenta de Storage?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Están en funcionamiento todos los hosts en los que se ejecuta el trabajo?</li><li>¿No pudo iniciarse el trabajo?</li><li>¿Hay actualizaciones del runtime en curso?</li><li>¿Está el trabajo en un estado esperado (por ejemplo, en ejecución o detenido por el cliente)?</li><li>¿Ha encontrado el trabajo excepciones de memoria insuficiente?</li><li>¿Hay actualizaciones del proceso programado en curso?</li><li>¿Está disponible Execution Manager (plan de control)?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está el servidor en funcionamiento?</li><li>¿Se ejecuta Internet Information Services (IIS)?</li><li>¿Funciona el equilibrador de carga?</li><li>¿Se puede acceder al plan de App Service desde el centro de datos?</li><li>¿Hospeda la cuenta de almacenamiento el contenido de los sitios de serverFarm disponible?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|Comprobaciones ejecutadas|
|---|
|<ul><li>¿Está el servidor en funcionamiento?</li><li>¿Se ejecuta Internet Information Server?</li><li>¿Funciona el equilibrador de carga?</li><li>¿Se puede acceder a la aplicación web desde el centro de datos?</li><li>¿Hospeda la cuenta de almacenamiento el contenido del sitio disponible?</li></ul>|

# <a name="next-steps"></a>Pasos siguientes
-  Consulte la [introducción al panel de Azure Service Health](service-health-overview.md) y la [introducción a Azure Resource Health](resource-health-overview.md) para más información sobre ellos. 
-  [Preguntas más frecuentes sobre Azure Resource Health](resource-health-faq.md)
- Configure alertas de forma que se le notifiquen los problemas de estado. Para más información, consulte el artículo de [configuración de alertas para eventos de Service Health](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

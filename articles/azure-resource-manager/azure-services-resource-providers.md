---
title: Proveedores de recursos de Azure Resource Manager mediante servicios de Azure
description: Enumera todos los espacios de nombres de proveedor de recursos de Azure Resource Manager y muestra el servicio de Azure para ese espacio de nombres.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/25/2019
ms.author: tomfitz
ms.openlocfilehash: 54493efdc0bffcbb4654b65676554f6707716968
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235582"
---
# <a name="resource-providers-for-azure-services"></a>Proveedores de recursos para servicios de Azure

En este artículo se muestra cómo los espacios de nombres del proveedor de recursos se asignan a servicios de Azure.

## <a name="match-resource-provider-to-service"></a>Proveedor de recursos de coincidencia para el servicio

| Espacio de nombres de proveedor de recursos | Servicio de Azure |
| --------------------------- | ------------- |
| Microsoft.AAD | [Azure Active Directory Domain Services](../active-directory-domain-services/index.yml) |
| microsoft.aadiam | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Addons | core |
| Microsoft.ADHybridHealthService | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Advisor | [Azure Advisor](../advisor/index.yml) |
| Microsoft.AlertsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/azure/analysis-services/) |
| Microsoft.ApiManagement | [Administración de API](../api-management/index.yml) |
| Microsoft.AppConfiguration | core |
| Microsoft.Authorization | [Administrador de recursos de Azure](index.yml) |
| Microsoft.Automation | [Automation](../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | [Azure Stack](/azure-stack/user/) |
| Microsoft.Batch | [Batch](../batch/index.yml) |
| Microsoft.Billing | [Facturación](/azure/billing/) |
| Microsoft.BingMaps | [Mapas de Bing](https://docs.microsoft.com/BingMaps/#pivot=main&panel=BingMapsAPI) |
| Microsoft.BizTalkServices | [BizTalk Services](../logic-apps/logic-apps-move-from-mabs.md) |
| Microsoft.Blockchain | [Servicio de Azure Blockchain](/azure/blockchain/workbench/) |
| Microsoft.Blueprint | [Planos técnicos de Azure](/azure/governance/blueprints/) |
| Microsoft.BotService | [Azure Bot Service](/azure/bot-service/) |
| Microsoft.Cache | [Azure Cache for Redis](/azure/azure-cache-for-redis/) |
| Microsoft.Capacity | core |
| Microsoft.Cdn | [Red de entrega de contenido](../cdn/index.yml) |
| Microsoft.CertificateRegistration | [Certificados de App Service](../app-service/web-sites-purchase-ssl-web-site.md) |
| Microsoft.ClassicCompute | Máquina virtual de modelo de implementación clásica |
| Microsoft.ClassicInfrastructureMigrate | Migración del modelo de implementación clásica |
| Microsoft.ClassicNetwork | Red virtual del modelo de implementación clásica |
| Microsoft.ClassicStorage | Almacenamiento del modelo de implementación clásica |
| Microsoft.ClassicSubscription | Modelo de implementación clásica |
| Microsoft.CognitiveServices | [Cognitivas Services](/azure/cognitive-services/) |
| Microsoft.Commerce | core |
| Microsoft.Compute | [Máquinas virtuales](/azure/virtual-machines/) |
| Microsoft.Consumption | [Administración de costos](/azure/cost-management/) |
| Microsoft.ContainerInstance | [Instancias de contenedor](/azure/container-instances/) |
| Microsoft.ContainerRegistry | [Registro de contenedor](/azure/container-registry/) |
| Microsoft.ContainerService | [Azure Kubernetes Service (AKS)](/azure/aks/) |
| Microsoft.ContentModerator | [Azure Content Moderator](../cognitive-services/content-moderator/index.yml) |
| Microsoft.CostManagement | [Administración de costos](/azure/cost-management/) |
| Microsoft.CustomerInsights | Customer Insights |
| Microsoft.CustomerLockbox | Caja de seguridad de cliente de Microsoft Azure |
| Microsoft.DataBox | [Azure Data Box](/azure/databox-family/) |
| Microsoft.DataBoxEdge | [Azure Data Box Edge](../databox-online/data-box-edge-overview.md) |
| Microsoft.Databricks | [Azure Databricks](/azure/azure-databricks/) |
| Microsoft.DataCatalog | [Catálogo de datos](/azure/data-catalog/) |
| Microsoft.DataFactory | [Factoría de datos](/azure/data-factory/) |
| Microsoft.DataLakeAnalytics | [Análisis de Data Lake](/azure/data-lake-analytics/) |
| Microsoft.DataLakeStore | [Azure Data Lake Store](../storage/blobs/data-lake-storage-introduction.md) |
| Microsoft.DataMigration | [Azure Database Migration Service](/azure/dms/) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](/azure/mariadb/) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](/azure/mysql/) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](/azure/postgresql/) |
| Microsoft.DeploymentManager | [Administrador de implementaciones de Azure](deployment-manager-overview.md) |
| Microsoft.Devices | [Centro de IoT](/azure/iot-hub/) |
| Microsoft.DevSpaces | [Espacios de desarrollo de Azure](/azure/dev-spaces/) |
| Microsoft.DevTestLab | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../cosmos-db/index.yml) |
| Microsoft.DomainRegistration | [Aplicación de servicio](/azure/app-service/) |
| Microsoft.EnterpriseKnowledgeGraph | Gráfico de conocimiento empresarial |
| Microsoft.EventGrid | [Cuadrícula de eventos](/azure/event-grid/) |
| Microsoft.EventHub | [Event Hubs](../event-hubs/index.yml) |
| Microsoft.Features | [Administrador de recursos de Azure](index.yml) |
| Microsoft.Genomics | [Microsoft Genomics](/azure/genomics/) |
| Microsoft.GuestConfiguration | [Directiva de Azure](../governance/policy/index.yml) |
| Microsoft.HanaOnAzure | [SAP HANA en Azure](../virtual-machines/workloads/sap/hana-overview-architecture.md) |
| Microsoft.HardwareSecurityModules | [Azure Dedicated HSM](../dedicated-hsm/index.yml) |
| Microsoft.HDInsight | [HDInsight](../hdinsight/index.yml) |
| Microsoft.HealthcareApis | [API de Azure para FHIR](../healthcare-apis/index.yml) |
| Microsoft.ImportExport | [Azure Import/Export](../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.Intune | [Intune](/intune/) |
| Microsoft.IoTCentral | [IoT Central](/azure/iot-central/) |
| Microsoft.IoTSpaces | [Azure gemelos Digital](../digital-twins/index.yml) |
| Microsoft.KeyVault | [Key Vault](../key-vault/index.yml) |
| Microsoft.Kusto | [Explorador de datos de Azure](../data-explorer/index.yml) |
| Microsoft.LabServices | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.LocationBasedServices | [Azure Maps](../azure-maps/index.yml) |
| Microsoft.LocationServices | core |
| Microsoft.LogAnalytics | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.Logic | [Logic Apps](../logic-apps/index.yml) |
| Microsoft.MachineLearning | [Machine Learning Studio](../machine-learning/studio/index.yml) |
| Microsoft.MachineLearningCompute | [Machine Learning Service](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningModelManagement | [Machine Learning Service](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningServices | [Machine Learning Service](../machine-learning/service/index.yml) |
| Microsoft.ManagedIdentity | [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.ManagedLab | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.Management | [Grupos de administración](/azure/governance/management-groups/) |
| Microsoft.Maps | [Azure Maps](../azure-maps/index.yml) |
| Microsoft.Marketplace | core |
| Microsoft.MarketplaceApps | core |
| Microsoft.MarketplaceOrdering | core |
| Microsoft.Media | [Media Services](../media-services/index.yml) |
| Microsoft.Migrate | [Azure Migrate](../migrate/migrate-overview.md) |
| Microsoft.MixedReality | [Azure delimitadores espaciales](/azure/spatial-anchors/) |
| Microsoft.NetApp | [Azure NetApp Files](../azure-netapp-files/index.yml) |
| Microsoft.Network | [Virtual Network](../virtual-network/index.yml)<br />[Equilibrador de carga](../load-balancer/index.yml)<br />[Application Gateway](../application-gateway/index.yml)<br />[Azure DNS](../dns/index.yml)<br />[ExpressRoute](../expressroute/index.yml)<br />[Puerta de enlace de VPN](../vpn-gateway/index.yml)<br />[Traffic Manager](../traffic-manager/index.yml)<br />[Network Watcher](../network-watcher/index.yml)<br />[Azure Firewall](../firewall/index.yml)<br />[Servicio de puerta de Azure](../frontdoor/index.yml) |
| Microsoft.NotificationHubs | [Centros de notificaciones](../notification-hubs/index.yml) |
| Microsoft.OffAzure | [Azure Migrate](../migrate/migrate-overview.md) |
| Microsoft.OperationalInsights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Directiva de Azure](../governance/policy/index.yml) |
| Microsoft.Portal | [Azure Portal](/azure/azure-portal/) |
| Microsoft.PowerBI | [Power BI](/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/azure/power-bi-embedded/) |
| Microsoft.RecoveryServices | [Recuperación de sitios](../site-recovery/index.yml) |
| Microsoft.Relay | [Relay de Azure](../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceHealth | core |
| Microsoft.Resources | [Azure Resource Manager](index.yml) |
| Microsoft.SaaS | core |
| Microsoft.Scheduler | [Scheduler](/azure/scheduler/) |
| Microsoft.Search | [Búsqueda de Azure](../search/index.yml) |
| Microsoft.Security | [Centro de seguridad](../security-center/index.yml) |
| Microsoft.ServiceBus | [Service Bus](/azure/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../service-fabric/index.yml) |
| Microsoft.ServiceFabricMesh | [Service Fabric Mesh](../service-fabric-mesh/index.yml) |
| Microsoft.SignalRService | [Azure SignalR Service](../azure-signalr/index.yml) |
| Microsoft.SiteRecovery | [Recuperación de sitios](../site-recovery/index.yml) |
| Microsoft.Solutions | [Aplicaciones administradas de Azure](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL Database](../sql-database/index.yml) |
| Microsoft.SqlVirtualMachine | [SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) |
| Microsoft.Storage | [Storage](../storage/index.yml) |
| Microsoft.StorageSync | [Storage](../storage/index.yml) |
| Microsoft.StorSimple | [StorSimple](/azure/storsimple/) |
| Microsoft.StreamAnalytics | [Stream Analytics](../stream-analytics/index.yml) |
| Microsoft.Subscription | core |
| microsoft.support | core |
| Microsoft.TimeSeriesInsights | [Time Series Insights](../time-series-insights/index.yml) |
| microsoft.visualstudio | [Azure DevOps](/azure/devops/?view=azure-devops) |
| Microsoft.Web | [App Service](../app-service/index.yml)<br />[Funciones](../azure-functions/index.yml) |
| Microsoft.WindowsDefenderATP | [Advanced Threat Protection de Windows Defender](/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection) |
| Microsoft.WindowsIoT | [Servicios de Windows 10 IoT Core](https://docs.microsoft.com/windows-hardware/manufacture/iot/iotcoreservicesoverview) |
| Microsoft.WorkloadMonitor | [Azure Monitor](../azure-monitor/index.yml) |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los proveedores de recursos, consulte [tipos y proveedores de recursos de Azure](resource-manager-supported-services.md)

---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ce64047fd7490106790ea8bb1ad7963d82a87c24
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238759"
---
| Recurso | Gratuito | Compartido | Básica | Estándar | Premium (v2) | Aislado </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Aplicaciones Web, móviles o de API](https://azure.microsoft.com/services/app-service/) por [plan de App Service](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup>|
| [plan de App Service](../articles/app-service/overview-hosting-plans.md) |10 por región |10 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos|
| Tipo de instancia de proceso |Compartido |Compartido |Dedicado<sup>3</sup> |Dedicado<sup>3</sup> |Dedicado<sup>3</sup></p> |Dedicado<sup>3</sup>|
| [Escalar horizontalmente](../articles/app-service/web-sites-scale.md) (número máximo de instancias) |1 compartido |1 compartido |3 dedicados<sup>3</sup> |10 dedicados<sup>3</sup> |20 dedicados<sup>3</sup>|100 dedicados<sup>4</sup>|
| Almacenamiento<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| Tiempo de CPU (5 minutos)<sup>6</sup> |3 minutos |3 minutos |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar.|
| Tiempo de CPU (día)<sup>6</sup> |60 minutos |240 minutos |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |
| Memoria (1 hora) |1.024 MB por plan de App Service |1.024 MB por aplicación |N/D |N/D |N/D |N/D |
| Ancho de banda |165 MB |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |
| Arquitectura de la aplicación |32 bits |32 bits |32 bits/64 bits |32 bits/64 bits |32 bits/64 bits |32 bits/64 bits |
| Web sockets por instancia<sup>7</sup> |5 |35 |350 |Ilimitado |Ilimitado |Ilimitado |
| Conexiones de depurador [simultáneas](../articles/app-service/troubleshoot-dotnet-visual-studio.md) por aplicación |1 |1 |1 |5 |5 |5 |
| Certificados de App Service por suscripción<sup>10</sup>| No compatible | No compatible |10 |10 |10 |10 |
| Dominios personalizados por aplicación</a> |0 (solo subdominio de azurewebsites.net)|500 |500 |500 |500 |500 |
| Compatibilidad con dominio [Compatibilidad con SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |No compatible, el certificado comodín para *. azurewebsites.net disponible de forma predeterminada|No compatible, el certificado comodín para *. azurewebsites.net disponible de forma predeterminada|Conexiones SSL SNI ilimitadas |Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP |Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP | Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP|
| Equilibrador de carga integrado | |X |X |X |X |X<sup>9</sup> |
| [Siempre activado](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Copias de seguridad programadas](../articles/app-service/manage-backup.md) | | | | Copias de seguridad programadas cada 2 horas, un máximo de 12 copias de seguridad por día (manual y programada) | Copias de seguridad programadas cada hora, un máximo de 50 copias de seguridad por día (manual y programada) | Copias de seguridad programadas cada hora, un máximo de 50 copias de seguridad por día (manual y programada) |
| [Autoscale](../articles/app-service/web-sites-scale.md) | | | |X |X |X |
| [WebJobs](../articles/app-service/webjobs-create.md)<sup>8</sup> |X |X |X |X |X |X |
| [Compatibilidad con Azure Scheduler](https://azure.microsoft.com/services/scheduler/) | |X |X |X |X |X |
| [Supervisión de extremos](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Ranuras de ensayo](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| Contrato de nivel de servicio | |  |99.9% |99,95 %|99,95 %|99,95 %|  

<sup>1</sup>Las aplicaciones y las cuotas de almacenamiento son por plan de App Service, a menos que se indique lo contrario.  
<sup>2</sup>El número real de aplicaciones que puedes hospedar en estas máquinas depende de la actividad de las aplicaciones, el tamaño de las instancias de máquina y el correspondiente uso de los recursos.  
<sup>3</sup>Las instancias dedicadas pueden tener diferentes tamaños. Para más información, consulte [Precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup>se permiten más a petición.  
<sup>5</sup>El límite de almacenamiento es el tamaño total del contenido en todas las aplicaciones en el mismo plan de App Service.  
<sup>6</sup>Estos recursos están limitados por los recursos físicos en las instancias dedicadas (el tamaño de la instancia y el número de instancias).  
<sup>7</sup>Si se escala una aplicación en el nivel Básico a dos instancias, dispones de 350 conexiones simultáneas para cada una de las dos.  
<sup>8</sup>Ejecute scripts o archivos ejecutables personalizados bajo demanda, según una programación o de manera continua como tarea en segundo plano dentro de su instancia de App Service. Siempre disponible se requiere para la ejecución continua de Trabajos web. Se requiere Azure Scheduler de nivel Gratis o Estándar para Trabajos web programados. No hay ningún límite predefinido en el número de trabajos Web que se puede ejecutar en una instancia de App Service. Hay límites prácticos que dependen de lo que el código de aplicación está intentando hacer.  
<sup>9</sup>SKU aisladas de app Service tienen la capacidad de carga interno (ILB) equilibrado con Azure Load Balancer, por lo que no hay ninguna conectividad pública desde internet. Como resultado, algunas características de un App Service aislado con ILB deben usarse desde máquinas que tienen acceso directo al punto de conexión de red del ILB.  
<sup>10</sup>se puede aumentar el límite de cuota del certificado de App Service por suscripción a través de una solicitud de soporte técnico para un límite máximo de 200.  
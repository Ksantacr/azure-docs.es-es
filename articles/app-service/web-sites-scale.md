---
title: 'Escalado vertical de características y capacidades: Azure App Service | Microsoft Docs'
description: Aprenda a escalar verticalmente una aplicación de Azure App Service para agregar capacidad y características.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 98d3d1f6fc0f2f30196f360811808579dfbab312
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832153"
---
# <a name="scale-up-an-app-in-azure"></a>Escalado vertical de aplicaciones en Azure

> [!NOTE]
> Con el nuevo nivel **PremiumV2** se obtienen unas CPU más rápidas, almacenamiento SSD y el doble de relación memoria-núcleo de los planes de tarifa actuales. Con la ventaja de rendimiento, puede ahorrar dinero mediante la ejecución de las aplicaciones en menos instancias. Para escalar al nivel **PremiumV2**, vea [Configure PremiumV2 tier for App Service](app-service-configure-premium-tier.md) (Configuración del nivel PremiumV2 para App Service).
>

En este artículo se muestra cómo escalar aplicaciones en Azure App Service. Existen dos flujos de trabajo para escalar aplicaciones, el escalado vertical y el escalado horizontal, y en este artículo se explica el flujo de trabajo para escalar verticalmente.

* [Escalado vertical](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): obtenga más CPU, memoria, espacio en disco y características adicionales como máquinas virtuales exclusivas, dominios y certificados personalizados, espacios de ensayo, autoescala y mucho más. Para escalar verticalmente, se cambia el plan de tarifa del plan de App Service al que pertenece la aplicación.
* [Escalado horizontal](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): se aumenta el número de instancias de máquina virtual que ejecutan la aplicación.
  Se puede escalar horizontalmente a un máximo de 20 instancias, según el plan de tarifa. [Entornos de App Service](environment/intro.md) en el nivel **Aislado** aumenta aún más el recuento de escalado horizontal a 100 instancias. Para más información sobre el escalado horizontal, consulte [Escalado del número de instancias de forma manual o automática](../monitoring-and-diagnostics/insights-how-to-scale.md). En ese artículo aprenderá a usar el escalado automático, que consiste en escalar el número de instancias automáticamente en función de las programaciones y reglas predefinidas.

La configuración de escalado tarda solo unos segundos en aplicarse y afecta a todas las aplicaciones del [plan de App Service](../app-service/overview-hosting-plans.md).
No hay que modificar el código ni volver a implementar la aplicación.

Para obtener información de los precios y características de planes de App Service individuales, consulte [Precios de App Service](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Antes de cambiar un plan de App Service con el nivel **Gratis** a uno superior, primero debe quitar los [límites de gasto](https://azure.microsoft.com/pricing/spending-limits/) vigentes de la suscripción de Azure. Para ver o cambiar opciones para la suscripción a Azure App Service, consulte [Suscripciones a Microsoft Azure][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Escalado vertical del plan de tarifa
1. En el explorador, abra [Azure Portal][portal].
2. En la hoja de la aplicación App Service, haga clic en **Toda la configuración** y en **Escalar verticalmente**.
   
    ![Desplácese para escalar verticalmente la aplicación de Azure.][ChooseWHP]
3. Elija el nivel y, después, haga clic en **Aplicar**.
   
    La pestaña **Notificaciones** emitirá el mensaje **CORRECTO** en color verde indicando que se ha completado con éxito una vez finalizada la operación.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Recursos relacionados con el escalado
Si su aplicación depende de otros servicios, como Azure SQL Database o Azure Storage, también puede escalar verticalmente estos recursos por separado. Estos recursos no se administran a través del plan de App Service.

1. En **Essentials**, haga clic en el vínculo **Grupo de recursos**.
   
    ![Escalado vertical de recursos relacionados con la aplicación de Azure](./media/web-sites-scale/RGEssentialsLink.png)
2. En la parte **Resumen** de la página **Grupo de recursos**, haga clic en un recurso que quiera escalar. En la captura de pantalla siguiente se muestra un recurso de SQL Database y uno de Azure Storage.
   
    ![Navegue a la página del grupo de recursos para escalar verticalmente la aplicación de Azure.](./media/web-sites-scale/ResourceGroup.png)
3. Para un recurso de SQL Database, haga clic en **Configuración** > **Nivel de precios** para escalar el plan de tarifa.
   
    ![Escalado vertical del back-end de SQL Database de la aplicación de Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    También puede activar la [replicación geográfica](../sql-database/sql-database-geo-replication-overview.md) de su instancia de SQL Database.
   
    Para un recurso de Azure Storage, haga clic en **Configuración** > **Configuración** para escalar verticalmente las opciones de almacenamiento.
   
    ![Escalado vertical de la cuenta de Azure Storage que utiliza la aplicación de Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Comparar planes de tarifa
Si necesita información detallada, como los tamaños de máquina virtual para cada plan de tarifa, vea [Precios de App Service](https://azure.microsoft.com/pricing/details/web-sites/).
Para ver una tabla de límites de servicio, presupuestos y restricciones, así como las características admitidas en cada nivel, vea [Límites de App Service](../azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="next-steps"></a>Pasos siguientes
* Para más información sobre precios, soporte técnico y contrato de nivel de servicio, vea estos vínculos:
  
    [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/)
  
    [Contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/)
  
    [Detalles de precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Tamaños de máquinas virtuales y servicios en la nube de Microsoft Azure][vmsizes]
  
* Para obtener información sobre las prácticas recomendadas de Azure App Service, incluida la creación de una arquitectura resistente y escalable, consulte [Prácticas recomendadas: Azure App Service Web Apps](https://azure.microsoft.com/blog/best-practices-windows-azure-websites-waws/).
* Para ver vídeos sobre cómo escalar aplicaciones de App Service, consulte los siguientes recursos:
  
  * [Cuándo escalar Azure Websites: con Stefan Schackow](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Escalación automática de Azure Websites mediante CPU o programación: con Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Escalación de Azure Websites: con Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png

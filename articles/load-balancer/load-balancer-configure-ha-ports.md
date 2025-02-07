---
title: Configuración de puertos de alta disponibilidad para Azure Load Balancer
titlesuffix: Azure Load Balancer
description: Aprenda a usar los puertos de alta disponibilidad para el tráfico interno de equilibrio de carga en todos los puertos.
services: load-balancer
documentationcenter: na
author: rdhillon
manager: narayan
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: ec43b79109181457f8ef8e214e296969db5dcb26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66122335"
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Configuración de los puertos de alta disponibilidad para un equilibrador de carga interno

En este artículo se proporciona un ejemplo de implementación de los puertos de alta disponibilidad en un equilibrador de carga interno. Para obtener más información sobre las configuraciones específicas de aplicaciones virtuales de red, consulte los sitios web del proveedor correspondiente.

>[!NOTE]
>Azure Load Balancer admite dos tipos diferentes: Básico y Estándar. En este artículo se describe Load Balancer Estándar. Para más información acerca de Load Balancer Básico, consulte [Introducción a Azure Load Balancer Básico (versión preliminar)](load-balancer-overview.md).

En la ilustración se muestra la configuración del ejemplo de implementación siguiente que se describe en este artículo:

- Los aplicaciones virtuales de red se implementan en el grupo back-end de un equilibrador de carga interno detrás de la configuración de los puertos de alta disponibilidad. 
- La ruta definida por el usuario aplicada en las rutas de la subred DMZ enruta todo el tráfico a aplicaciones virtuales de red al realizar el salto siguiente como IP virtual del equilibrador de carga interno. 
- El equilibrador de carga interno distribuye el tráfico a uno de los aplicaciones virtuales de red activos según el algoritmo del equilibrador de carga.
- El dispositivo virtual de red procesa el tráfico y lo reenvía al destino original en la subred de back-end.
- La ruta de devolución también puede ser la misma, si se configura la ruta definida por el usuario correspondiente en la subred de back-end. 

![Implementación del ejemplo de puertos de alta disponibilidad](./media/load-balancer-configure-ha-ports/haports.png)

## <a name="configure-high-availability-ports"></a>Configuración de los puertos de alta disponibilidad

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para configurar Puertos de alta disponibilidad, configure un equilibrador de carga interno con aplicaciones virtuales de red en el grupo back-end. Establezca una configuración de sondeo del estado del equilibrador de carga correspondiente para detectar el estado de la aplicación virtual de red y la regla del equilibrador de carga con los puertos de alta disponibilidad. La configuración general del equilibrador de carga se aborda en [Introducción](load-balancer-get-started-ilb-arm-portal.md). En este artículo se resalta la configuración de los puertos de alta disponibilidad.

Básicamente, la configuración implica establecer el valor del puerto de back-end y de front-end en **0**. Establezca el valor del protocolo en **Todos**. En este artículo se describe cómo configurar los puertos de alta disponibilidad mediante Azure Portal, PowerShell y la CLI de Azure.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Configuración de la regla del equilibrador de carga para los puertos de alta disponibilidad mediante Azure Portal

Para configurar los puertos de alta disponibilidad mediante Azure Portal, seleccione la casilla de verificación **HA Ports** (Puertos de alta disponibilidad). Al seleccionarla, la configuración del protocolo y el puerto relacionados se rellena automáticamente. 

![Configuración de los puertos de alta disponibilidad a través de Azure Portal](./media/load-balancer-configure-ha-ports/haports-portal.png)

### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Configuración de una regla de equilibrio de carga para los puertos de alta disponibilidad a través de la plantilla de Resource Manager

Puede configurar los puertos de alta disponibilidad con la versión de API 2017-08-01 para Microsoft.Network/loadBalancers en el recurso de Load Balancer. En el siguiente fragmento JSON se muestran los cambios en la configuración del equilibrador de carga para los puertos de alta disponibilidad a través de la API de REST:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>Configuración de la regla del equilibrador de carga para los puertos de alta disponibilidad mediante PowerShell

Use el comando siguiente para crear la regla del equilibrador de carga para los puertos de alta disponibilidad al crear el equilibrador de carga interno con PowerShell:

```powershell
lbrule = New-AzLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli"></a>Configuración de la regla del equilibrador de carga para los puertos de alta disponibilidad mediante la CLI de Azure

En el paso 4 del artículo sobre la [creación de un conjunto de equilibrador de carga interno](load-balancer-get-started-ilb-arm-cli.md), use el siguiente comando para crear la regla del equilibrador de carga para los puertos de alta disponibilidad:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los [puertos de alta disponibilidad](load-balancer-ha-ports-overview.md).
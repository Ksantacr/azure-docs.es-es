---
title: Características y plataformas compatibles con Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de características y plataformas compatibles con Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2019
ms.author: monhaber
ms.openlocfilehash: 807bde76bb6bb50490ee599768273a59c49d5e45
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258704"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Características y plataformas compatibles con Azure Security Center

La supervisión del estado de seguridad y las recomendaciones están disponibles para las máquinas virtuales, creadas con el modelo de implementación clásica o con el modelo de implementación de Resource Manager, y para los equipos.

> [!NOTE]
> Obtenga más información sobre los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) para los recursos de Azure.
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Plataformas compatibles con el agente de recopilación de datos 

En esta sección se enumeran las plataformas en las que puede ejecutarse el agente de Azure Security Center y desde las que puede recopilar datos.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Windows
Se admiten los siguientes sistemas operativos Windows:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

> [!NOTE]
> La integración con ATP de Windows Defender solo admite Windows Server 2012 R2 y Windows Server 2016.

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Linux

Se admiten los siguientes sistemas operativos Linux:

> [!NOTE]
> Puesto que la lista de sistemas operativos Linux compatibles cambia constantemente, si lo prefiere, haga clic en [aquí](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) para ver la lista más actualizada de las versiones compatibles, en caso de que ha habido cambios desde que este tema se publicó por última vez.

64 bits
* CentOS 6 y 7
* Amazon Linux 2017.09
* Oracle Linux 6 y 7
* Red Hat Enterprise Linux Server 6 y 7
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
* SUSE Linux Enterprise Server 12

32 bits
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS y 16.04 LTS

## <a name="vms-and-cloud-services"></a>Cloud Services y Virtual Machines
También se admiten máquinas virtuales que se ejecuten en un servicio en la nube. Se supervisan los roles de trabajo y web de servicios en la nube que se ejecutan en espacios de producción. Para más información sobre Cloud Services, consulte la [información general sobre Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Características admitidas de IaaS

> [!div class="mx-tableFixed"]
> 

|Server| Windows||Linux||||Precios|
|----|----|----|----|----|----|----|----|
|**Entorno**|**Las tablas de Azure**||**Non-Azure**|**Las tablas de Azure**||**Non-Azure**||
||**Máquina virtual**|**Conjunto de escalado de máquinas virtuales**||**Máquina virtual**|**Conjunto de escalado de máquinas virtuales**|
|Alertas de detección de amenazas de VMBA|✔|✔|✔|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|Detección de amenazas (gratis) de las recomendaciones (estándar)|
|Alertas de detección de amenazas basadas en red|✔|✔|X|✔|✔|X|Estándar|
|Integración con ATP de Windows Defender|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|X|X|X|Estándar|
|Revisiones que faltan|✔|✔|✔|✔|✔|✔|Gratuito|
|Configuraciones de seguridad|✔|✔|✔|✔|✔|✔|Gratuito|
|Evaluación de EndPoint protection|✔|✔|✔|X|X|X|Gratuito|
|Acceso a VM JIT|✔|X|X|✔|X|X|Estándar|
|Controles de aplicación adaptables|✔|X|✔|✔|X|✔|Estándar|
|FIM|✔|✔|✔|✔|✔|✔|Estándar|
|Evaluación del cifrado de disco|✔|✔|X|✔|✔|X|Gratuito|
|Implementación de aplicaciones de terceros|✔|X|X|✔|X|X|Gratuito|
|Evaluación de NSG|✔|✔|X|✔|✔|X|Gratuito|
|Detección de amenazas sin archivo|✔|✔|✔|X|X|X|Estándar|
|Mapa de red|✔|✔|X|✔|✔|X|Estándar|
|Controles de red adaptables|✔|✔|X|✔|✔|X|Estándar|
|Informes y panel de cumplimiento normativo|✔|✔|✔|✔|✔|✔|Estándar|
|Recomendaciones y detección de amenazas en los contenedores de Docker hospedado en IaaS|X|X|X|✔|✔|✔|Estándar|

### <a name="supported-endpoint-protection-solutions"></a>Soluciones de protección de punto de conexión compatibles

En la tabla siguiente se proporciona una matriz de:
 - Si puede usar Azure Security Center para instalar cada solución para usted.
 - Qué soluciones de protección de punto de conexión puede detectar Security Center. Si se detecta una de estas soluciones de protección de punto de conexión, Security Center no recomendará instalar ninguna.

Para obtener información acerca de cuándo se generaron las recomendaciones para cada una de estas protecciones, vea [Endpoint Protection evaluación y recomendaciones](security-center-endpoint-protection.md).

| Endpoint Protection| Plataformas | Instalación de Security Center | Detección de Security Center |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| No, se integra en el sistema operativo.| Sí |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (consulte la nota que hay a continuación) | Mediante extensión | Sí |
| Trend Micro: todas las versiones | Familia de Windows Server  | Sin  | Sí |
| Symantec v12.1.1100+| Familia de Windows Server  | Sin  | Sí |
| McAfee v10 o posterior | Familia de Windows Server  | Sin  | Sí |
| Kaspersky| Familia de Windows Server  | Sin  | Sin   |
| Sophos| Familia de Windows Server  | Sin  | No  |

> [!NOTE]
> - La detección de System Center Endpoint Protection (SCEP) en una máquina virtual de Windows Server 2008 R2 requiere que SCEP se instale después de PowerShell 3.0 (o una versión superior).

## <a name="supported-paas-features"></a>Características admitidas de PaaS


|Servicio|Recomendaciones (gratis)|Detección de amenazas (estándar)|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Cuentas de almacenamiento de Azure Blob*|✔| ✔|
|App Services|✔| ✔|
|Cloud Services|✔| X|
|Redes virtuales|✔| N/D|
|Subredes|✔| N/D|
|Tarjetas de red|✔| N/D|
|Grupos de seguridad de red|✔| N/D|
|Subscription|✔ **| ✔|
|App Service|✔| N/D|
|Batch|✔| N/D|
|Service Fabric|✔| N/D|
|Cuenta de Automation|✔| N/D|
|Equilibrador de carga|✔| N/D|
|Search|✔| N/D|
|Service Bus|✔| N/D|
|Stream Analytics|✔| N/D|
|Centro de eventos|✔| N/D|
|Aplicaciones lógicas|✔| N/D|
|Subred|✔| N/D|
|Vnet|✔| N/D|
|Cuenta de almacenamiento|✔| N/D|
|Redis|✔| N/D|
|SQL|✔| N/D|
|Data Lake Analytics|✔| N/D|
|Cuenta de almacenamiento|✔| N/D|
|Subscription|✔| N/D|
|Almacén de claves|✔| N/D|




\* Estas características se admiten actualmente en versión preliminar pública.

\*\* Recomendaciones de AAD solo están disponibles para las suscripciones estándar



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).
- Obtenga más información acerca del [análisis de comportamiento de máquinas virtuales y análisis de la memoria de volcado en Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Consulte las [preguntas más frecuentes sobre Azure Security Center](security-center-faq.md).
- Encuentre [artículos de blog sobre el cumplimiento y la seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/).

---
title: Introducción al TAP de Virtual Network | Microsoft Docs
description: Obtenga más información sobre Virtual Network TAP. Virtual Network TAP proporciona una copia en profundidad del tráfico de red de la máquina virtual que se puede transmitir a un recopilador de paquetes.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2019
ms.author: kaanan
ms.openlocfilehash: ff5c8c4d3f6a0c87afae67404a5a39d4fe3757d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60593187"
---
# <a name="virtual-network-tap"></a>Virtual Network TAP

Azure Virtual Network TAP (punto de acceso del terminal) permite transmitir en secuencias de forma continua el tráfico de red de la máquina virtual a un recopilador de paquetes de red o a una herramienta de análisis. Un asociado de la [aplicación virtual de red](https://azure.microsoft.com/solutions/network-appliances/) proporciona el recopilador o la herramienta de análisis de la herramienta. Para ver una lista de soluciones de asociados que estén validadas para funcionar con Virtual Network TAP, consulte las [soluciones de asociados](#virtual-network-tap-partner-solutions).

> [!IMPORTANT]
> Virtual network TAP es actualmente en versión preliminar en todas las regiones de Azure. Para usar los TAP de red virtual, debe inscribirse en la versión preliminar enviando un correo electrónico a <azurevnettap@microsoft.com> con el identificador de suscripción. Recibirá un correo electrónico una vez inscrita la suscripción. No puede usar la funcionalidad hasta que reciba un correo electrónico de confirmación. Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Consulte la [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obtener más información.

## <a name="virtual-network-tap-partner-solutions"></a>Soluciones de los partners de Virtual Network TAP

### <a name="network-packet-brokers"></a>Agentes de paquetes de red

- [Big Monitoring Fabric de Big Switch](https://www.bigswitch.com/products/big-monitoring-fabric/public-cloud/microsoft-azure)
- [GigaSECURE de Gigamon](https://blog.gigamon.com/2018/09/13/why-microsofts-new-vtap-service-works-even-better-with-gigasecure-for-azure)
- [CloudLens de Ixia](https://www.ixiacom.com/cloudlens/cloudlens-azure)
- [Prisms de Nubeva](https://www.nubeva.com/azurevtap)

### <a name="security-analytics-networkapplication-performance-management"></a>Análisis de seguridad, administración del rendimiento de red o de la aplicación

- [Seguridad activo](https://awakesecurity.com/technology-partners/microsoft-azure/)
- [En la nube de Cisco Stealthwatch](https://blogs.cisco.com/security/cisco-stealthwatch-cloud-and-microsoft-azure-reliable-cloud-infrastructure-meets-comprehensive-cloud-security)
- [Darktrace](https://www.darktrace.com/en/azure/)
- [Reveal(x) de ExtraHop](https://www.extrahop.com/company/tech-partners/microsoft/)
- [Cybersecurity de Fidelis](https://www.fidelissecurity.com/technology-partners/microsoft-azure )
- [Flowmon](https://www.flowmon.com/blog/azure-vtap)
- [NetFort LANGuardian](https://www.netfort.com/languardian/solutions/visibility-in-azure-network-tap/)
- [vSTREAM de Netscout]( https://www.netscout.com/technology-partners/microsoft/azure-vtap)
- [NetWitness® Platform de RSA](https://www.rsa.com/azure)
- [Cognito de Vectra](https://vectra.ai/microsoftazure)

En la siguiente imagen se muestra cómo funciona Virtual Network TAP. Puede agregar una configuración de TAP en una [interfaz de red](virtual-network-network-interface.md) adjunta a una máquina virtual implementada en la red virtual. El destino es una dirección IP de red virtual en la misma red virtual que la interfaz de red supervisada o una red [virtual emparejada](virtual-network-peering-overview.md). La solución del recopilador para Virtual Network TAP se puede implementar detrás de un [equilibrador de carga interno de Azure](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#concepts) para lograr alta disponibilidad. Para evaluar las opciones de implementación para una solución individual, consulte las [soluciones de partners](#virtual-network-tap-partner-solutions).

![Cómo funciona Virtual Network TAP](./media/virtual-network-tap/architecture.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de crear un TAP de red virtual, debe haber recibido un correo electrónico de confirmación que están inscritos en la vista previa y tiene uno o más máquinas virtuales creados mediante [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) modelo de implementación y un socio comercial solución para la agregación de la derivación de tráfico en la misma región de azure. Si no tiene ninguna solución de partner en la red virtual, consulte [soluciones de partners](#virtual-network-tap-partner-solutions) para implementar una. Puede usar el mismo recurso de Virtual Red TAP para agregar tráfico de diferentes interfaces de red en la misma suscripción o en suscripciones distintas. Si las interfaces de red supervisadas se encuentran en suscripciones distintas, ambas suscripciones deben estar asociadas al mismo inquilino de Azure Active Directory. Además, las interfaces de red supervisadas y el punto de conexión de destino para agregar tráfico del TAP pueden estar en redes virtuales emparejadas en la misma región. Si usa este modelo de implementación, asegúrese de que el [emparejamiento de redes virtuales](virtual-network-peering-overview.md) está habilitada antes de configurar Virtual Network TAP.

## <a name="permissions"></a>Permisos

Las cuentas que utiliza para aplicar la configuración del TAP en interfaces de red deben estar asignadas al rol de [colaborador de red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) o a un [rol personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que tenga asignadas las acciones necesarias de la tabla siguiente:

| . | NOMBRE |
|---|---|
| Microsoft.Network/virtualNetworkTaps/* | Necesaria para crear, actualizar, leer y eliminar un recurso de Virtual Network TAP |
| Microsoft.Network/networkInterfaces/read | Necesaria para leer el recurso de la interfaz de red en que el TAP se configurará |
| Microsoft.Network/tapConfigurations/* | Necesaria para crear, actualizar, leer y eliminar la configuración del TAP en una interfaz de red |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear un Virtual Network TAP](tutorial-tap-virtual-network-cli.md).

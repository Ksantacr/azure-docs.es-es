---
title: Azure Monitor para proveedores de servicios | Microsoft Docs
description: Monitor de Azure puede ayudar a los proveedores de servicios administrados (MSP), las grandes empresas, proveedores de Software independientes (ISV) y proveedores de servicios de hospedaje administración y supervisión los servidores de infraestructura de nube o local del cliente.
services: log-analytics
documentationcenter: ''
author: MeirMen
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: meirm
ms.openlocfilehash: 97d8d6fac93ebabac8fb319ce2f1ab8719f5f86b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60452660"
---
# <a name="azure-monitor-for-service-providers"></a>Azure Monitor para proveedores de servicios
Áreas de trabajo de log Analytics en Azure Monitor pueden ayudar a los proveedores de servicios administrados (MSP), las grandes empresas, proveedores de software independientes (ISV) y proveedores de servicio de hospedaje administrar y supervisar los servidores de infraestructura de nube o local del cliente. 

Las grandes empresas comparten muchas similitudes con los proveedores de servicios, especialmente cuando hay un equipo de TI centralizado que es responsable de la administración de TI de muchas unidades de negocio diferentes. Por motivos de simplicidad, este documento utiliza el término "*proveedor de servicios*", pero la misma funcionalidad también está disponible para empresas y otros clientes.

Para los asociados y proveedores de servicios que forman parte de la [proveedor de soluciones en la nube (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programa, Log Analytics en Azure Monitor es uno de los servicios de Azure disponibles en [suscripciones a Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Arquitecturas de proveedores de servicios

Las áreas de trabajo de Log Analytics proporcionan un método para que los administradores controlen el flujo y el aislamiento de los registros y creen una arquitectura de registro que aborde sus necesidades empresariales específicas. [En este artículo](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) se explican las consideraciones generales sobre la administración del área de trabajo. Los proveedores de servicios tienen consideraciones adicionales.

Hay tres arquitecturas posibles para proveedores de servicios con respecto a las áreas de trabajo de Log Analytics:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Distribuida: los registros se almacenan en áreas de trabajo que se encuentran en el inquilino del cliente 

En esta arquitectura, el área de trabajo se implementa en el inquilino del cliente que se usa para todos los registros de ese cliente. A los administradores de proveedores de servicios se les concede acceso a esta área de trabajo mediante [usuarios invitados de Azure Active Directory (B2B)](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b). Los administradores del proveedor de servicios tendrán que cambiar al directorio de su cliente en Azure Portal para acceder a estas áreas de trabajo.

Las ventajas de dicha arquitectura son las siguientes:
* El cliente puede administrar el acceso a los registros mediante su propio [acceso basado en roles](https://docs.microsoft.com/azure/role-based-access-control/overview).
* Cada cliente puede tener diferentes valores para su área de trabajo, como la retención y límite de datos.
* El aislamiento entre los clientes con respecto a las disposiciones legales y el cumplimiento.
* El cargo por cada área de trabajo se asignará a la suscripción del cliente.
* Los registros se pueden recopilar en todos los tipos de recursos, no solo según el agente. Por ejemplo, registros de auditoría de Azure.

Las desventajas de dicha arquitectura son las siguientes:
* Es más difícil para el proveedor de servicios administrar un gran número de inquilinos de cliente a la vez.
* Los administradores de proveedores de servicios se deben aprovisionar en el directorio de cliente.
* El proveedor de servicios no puede analizar los datos en sus clientes.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. Central: los registros se almacenan en el área de trabajo que se encuentra en el inquilino del proveedor de servicios

En esta arquitectura, los registros no se almacenan en los inquilinos del cliente, sino solo en una ubicación central dentro de una de las suscripciones del proveedor de servicios. Los agentes instalados en máquinas virtuales del cliente se configuran para enviar sus registros a esta área de trabajo con el identificador de área de trabajo y la clave secreta.

Las ventajas de dicha arquitectura son las siguientes:
* Es fácil administrar un gran número de clientes e integrarlos en distintos sistemas back-end.
* El proveedor de servicios tiene una propiedad completa a través de los registros y los distintos artefactos, como son las funciones y las consultas guardadas.
* El proveedor de servicios puede realizar análisis en todos los clientes.

Las desventajas de dicha arquitectura son las siguientes:
* Esta arquitectura es aplicable únicamente para los datos de máquina virtual basados en un agente, no abarcará orígenes de datos del tejido de Azure, PaaS ni SaaS.
* Puede resultar dificultoso separar los datos entre los clientes cuando se mezclan en una sola área de trabajo. El único método adecuado para ello consiste en usar el nombre del equipo de dominio completo (FQDN) o a través del identificador de la suscripción de Azure. 
* Todos los datos de todos los clientes se almacenarán en la misma región con una sola factura y las mismas opciones de configuración y de retención.
* Los servicios de PaaS y de tejido de Azure, como Azure Diagnostics y los registros de auditoría de Azure, requieren que el área de trabajo esté en el mismo inquilino que el recurso; por tanto, no pueden enviar los registros al área de trabajo central.
* Todos los agentes de VM desde todos los clientes se autenticarán en el área de trabajo central con el mismo identificador de área de trabajo y clave. No hay ningún método para bloquear los registros desde un cliente específico sin interrumpir a otros clientes.


### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Híbrido: los registros se almacenan en el área de trabajo que se encuentra en el inquilino del cliente y algunos se extraen a una ubicación central.

La tercera arquitectura es una mezcla de las dos opciones. Se basa en la primera arquitectura distribuida donde los registros son locales para cada cliente, pero con mecanismos para crear un repositorio central de registros. Una parte de los registros se extrae en una ubicación central para los informes y el análisis. Esta parte podría ser un número pequeño de tipos de datos o un resumen de la actividad, como la estadística diaria.

Hay dos opciones para implementar registros en una ubicación central:

1. Área de trabajo central: el proveedor de servicios puede crear un área de trabajo en su inquilino y usar un script que use [Query API](https://dev.loganalytics.io/) con [Data Collection API](../../azure-monitor/platform/data-collector-api.md) para traer los datos de las diversas áreas de trabajo a esta ubicación central. Otra opción diferente del script es usar [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).

2. Power BI como ubicación central: Power BI puede actuar como la ubicación central, cuando las diversas áreas de trabajo exportación datos a ella mediante la integración entre el área de trabajo de Log Analytics y [Power BI](../../azure-monitor/platform/powerbi.md). 


## <a name="next-steps"></a>Pasos siguientes
* Automatice la creación y configuración de áreas de trabajo con [plantillas de Resource Manager](template-workspace-configuration.md).
* Automatice la creación de áreas de trabajo con [PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md). 
* Use [alertas](../../azure-monitor/platform/alerts-overview.md) para integrarse con sistemas existentes.
* Genere informes de resumen con [Power BI](../../azure-monitor/platform/powerbi.md).
* Revise el proceso de [configurar Log Analytics y Power BI para supervisar varios clientes de CSP](https://docs.microsoft.com/azure/cloud-solution-provider/support/monitor-multiple-customers).

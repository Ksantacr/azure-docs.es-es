---
title: Uso de Azure DNS con otros servicios de Azure | Microsoft Docs
description: Descripción de cómo usar Azure DNS para resolver los nombres de otros servicios de Azure
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: victorh
ms.openlocfilehash: dcf209d2036d2686bea0b51380db3cd2473d04a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293202"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Funcionamiento de Azure DNS con otros servicios de Azure

Azure DNS es un servicio de resolución de nombres y administración de DNS hospedado. Puede usarlo para crear nombres DNS públicos para las otras aplicaciones y servicios que tiene implementados en Azure. Crear un nombre para un servicio de Azure en su dominio personalizado es sencillo. Solo tiene que agregar un registro del tipo correcto para el servicio.

* Para las direcciones IP asignadas dinámicamente, puede crear un registro CNAME de DNS que se asigna al nombre DNS que Azure creó para el servicio. Los estándares DNS evitan que use un registro CNAME para el vértice de la zona. Puede usar un registro de alias en su lugar. Para más información, consulte [Tutorial: Configurar un registro de alias para hacer referencia a una dirección IP pública de Azure](tutorial-alias-pip.md).
* Para las direcciones IP asignadas de forma estática, puede crear un registro D de DNS con cualquier nombre, incluido un nombre de *dominio simple* en el vértice de la zona.

En la tabla siguiente se describen los tipos de registro admitidos que se pueden usar para diversos servicios de Azure. Como muestra la tabla, Azure DNS solo admite registros DNS para recursos de red orientados a Internet. Azure DNS no se puede usar para la resolución de nombres de direcciones privadas internas.

| Servicio de Azure | interfaz de red | DESCRIPCIÓN |
| --- | --- | --- |
| Azure Application Gateway |[Dirección IP pública front-end](dns-custom-domain.md#public-ip-address) |Puede crear un registro D o CNAME de DNS. |
| Azure Load Balancer |[Dirección IP pública front-end](dns-custom-domain.md#public-ip-address) |Puede crear un registro D o CNAME de DNS. Load Balancer puede tener una dirección IP pública IPv6 que se asigna dinámicamente. Cree un registro CNAME para una dirección IPv6. |
| Administrador de tráfico de Azure |Nombre público |Puede crear un registro de alias que se asigne al nombre de trafficmanager.net que está asignado a su perfil de Traffic Manager. Para más información, consulte [Tutorial: Configurar un registro de alias para admitir nombres de dominio de vértice con Traffic Manager](tutorial-alias-tm.md). |
| Azure Cloud Services |[Dirección IP pública](dns-custom-domain.md#public-ip-address) |Para las direcciones IP asignadas de forma estática, puede crear un registro D de DNS. Para las direcciones IP asignadas dinámicamente, debe crear un registro CNAME que se asigne al nombre de *cloudapp.net* .|
| Azure App Service | [Dirección IP externa](dns-custom-domain.md#app-service-web-apps) |Para las direcciones IP externas, puede crear un registro D de DNS. De lo contrario, deberá crear un registro CNAME que se asigne al nombre de azurewebsites.net. Para más información, consulte [Asignación de un nombre de dominio personalizado a una aplicación de Azure](../app-service/app-service-web-tutorial-custom-domain.md). |
| Máquinas virtuales de Azure Resource Manager |[Dirección IP pública](dns-custom-domain.md#public-ip-address) |Las máquinas virtuales de Resource Manager pueden tener direcciones IP públicas. Una máquina virtual con una dirección IP pública también puede encontrarse detrás de un equilibrador de carga. Puede crear un registro D de DNS, CNAME o de alias para la dirección pública. Puede usar este nombre personalizado para omitir la dirección IP virtual en el equilibrador de carga. |
| Máquinas virtuales clásicas |[Dirección IP pública](dns-custom-domain.md#public-ip-address) |Las máquinas virtuales clásicas creadas con PowerShell o con la CLI se pueden configurar con una dirección virtual dinámica o estática (reservada). Puede crear un registro CNAME o D de DNS, respectivamente. |

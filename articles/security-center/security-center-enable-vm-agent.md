---
title: Habilitación del Agente de máquina virtual en Azure Security Center | Microsoft Docs
description: En este documento se muestra cómo implementar la recomendación de Azure Security Center **Habilitar el agente de máquina virtual**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 12934ad53050d16b89dd5b4175ca19a24d1ec4d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60916300"
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Habilitación del Agente de máquina virtual en Azure Security Center
El agente de máquina virtual debe instalarse en máquinas virtuales para [habilitar la recopilación de datos](security-center-enable-data-collection.md).  Azure Security Center permite ver qué máquinas virtuales requieren el Agente de máquina virtual y recomienda habilitarlo en esas máquinas virtuales.

De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. El artículo [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) (Agente de VM y extensiones, parte 2) proporciona información sobre cómo instalar el Agente de VM.

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo. No se trata de una guía paso a paso.
>
>

## <a name="implement-the-recommendation"></a>Implementación de la recomendación
1. En la hoja **Recomendaciones**, seleccione **Habilitar el agente de máquina virtual**.
   ![Habilitar el Agente de máquina virtual][1]
2. Se abrirá la hoja **VM Agent Is Missing Or Not Responding**(Agente de máquina virtual ausente o no responde). Esta hoja enumera las máquinas virtuales que requieren el Agente de máquina virtual. Siga las instrucciones de la hoja para instalar el Agente de máquina virtual.
   ![Agente de máquina virtual ausente][2]

## <a name="see-also"></a>Vea también
Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que lo ayudan a proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Centro de seguridad de Azure](security-center-monitoring.md): obtenga información sobre cómo supervisar el estado de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en el Centro de seguridad de Azure](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md) : aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes acerca del Centro de seguridad de Azure](security-center-faq.md): busque las preguntas más frecuentes sobre cómo usar el servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): obtenga las últimas noticias e información sobre la seguridad en Azure.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png

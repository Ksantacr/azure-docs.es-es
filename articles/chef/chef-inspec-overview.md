---
title: Uso de InSpec para automatizar el cumplimiento de normativas en una infraestructura de Azure
description: Información acerca de cómo usar InSpec para detectar problemas en las implementaciones de Azure
keywords: azure, chef, devops, máquinas virtuales, información general, automatizar, inspec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 03/19/2019
ms.topic: article
ms.openlocfilehash: bdfa30b48c79a8910d503bb9e54a42c30e5adba6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60629805"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Uso de InSpec para automatizar el cumplimiento de normativas en una infraestructura de Azure

[InSpec](https://www.chef.io/inspec/) es el lenguaje de código abierto de Chef para describir reglas de seguridad y cumplimiento que se pueden compartir entre los ingenieros de software, las operaciones y los ingenieros de seguridad. InSpec compara el estado real de la infraestructura con el estado deseado que usted expresa en código de InSpec que puede leer y escribir fácilmente. InSpec detecta infracciones y muestra los resultados en informes, pero le permite decidir cómo corregirlas.

Puede usar InSpec para validar el estado de los recursos y grupos de recursos de una suscripción, incluidas las máquinas virtuales, las configuraciones de red, configuración de Azure Active Directory y mucho más.

Este artículo describe las ventajas de usar InSpec para facilitar la seguridad y cumplimiento de normas en Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Cumplimiento fácil de entender y evaluar

Documentación de cumplimiento escrito en hojas de cálculo o documentos de Word deja requisitos abiertos a la interpretación. Con InSpec, transformará sus requisitos en código de lenguaje natural, ejecutable y con control de versiones. El código acaba con las especulaciones sobre qué debería evaluarse reemplazándolas por pruebas tangibles con una intención bien definida.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Detección de problemas globales y priorización de su corrección

El modo de detección sin agente de InSpec le permite evaluar rápidamente (a escala) el nivel de riesgo. Los metadatos integrados para la puntuación de la gravedad ayudan a determinar en qué áreas centrar las estrategias de corrección. También puede escribir reglas rápidamente en respuesta a las nuevas vulnerabilidades o reglamentos e implementarlas inmediatamente.

## <a name="audit-azure-virtual-machines-with-policy-guest-configuration"></a>Máquinas virtuales de Azure con configuración de invitado de directiva de auditoría

Azure admite directamente el uso de Chef InSpec definiciones para máquinas virtuales de Azure a través de auditoría [configuración de invitado de Azure directiva](/azure/governance/policy/concepts/guest-configuration). Configuración de invitado se evalúa como una máquina virtual de Linux para una definición de Chef InSpec proporcionado y el cumplimiento de los informes a través de la directiva de Azure. Los resultados de estas auditorías también se notifican a través de los registros de Azure Monitor habilitar las alertas y otros escenarios de automatización.

## <a name="satisfy-audits"></a>Cumplimiento de auditorías

Con InSpec, puede responder a las cuestiones que se formulan en una auditoría en cualquier momento, no solo en intervalos predeterminados, como trimestral o anualmente. Al ejecutar pruebas de InSpec de forma continua, podrá saber exactamente su historial y nivel de cumplimiento, con lo que las conclusiones de los auditores no le pillarán por sorpresa.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Pruebe InSpec en Azure Cloud Shell.](https://shell.azure.com)
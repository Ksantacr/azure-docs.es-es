---
title: Configuración de la solución Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Aprenda a configurar una solución de IoT de un extremo a otro mediante Azure Security Center for IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: c60b421e9b60c6a2191fe2be189d1abd1c328f24
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65200793"
---
# <a name="quickstart-configure-your-iot-solution"></a>Inicio rápido: Configuración de una solución de IoT

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo realizar la configuración inicial de una solución de seguridad de IoT mediante ASC for IoT. 

## <a name="azure-security-center-asc-for-iot"></a>Preguntas más frecuentes de Azure Security Center (ASC) for IoT

ASC for IoT proporciona una seguridad completa, de un extremo a otro, para las soluciones de IoT que usen Azure.

Con Azure Security Center for IoT se puede supervisar toda una solución de IoT desde un solo panel, así como presentar todos los dispositivos de IoT, las plataformas de IoT y los recursos de back-end de Azure.

Una vez que se habilita en IoT Hub, ASC for IoT identifica automáticamente otros servicios de Azure, que también estén conectados a IoT Hub y que estén relacionados con la solución de IoT.

Además de la detección automática de relaciones, se pueden elegir los otros recursos de Azure que se etiquetan como parte de la solución de IoT.
Las opciones que se seleccionen permiten agregar suscripciones completas, grupos de recursos o recursos individuales.

Después de definir todas las relaciones de los recursos, ASC for IoT aprovecha Azure Security Center para generar alertas y recomendaciones de seguridad relativas a estos recursos.

## <a name="add-azure-resources-to-your-iot-solution"></a>Adición de recursos de Azure a una solución de IoT

Para agregar un recurso nuevo a la solución de IoT, siga estos pasos: 

1. Abra **IoT Hub** en Azure Portal. 
2. Seleccione y abra **Resources** (Recursos) en **Security**  (Seguridad) en el menú izquierdo. 
3. Seleccione **Add resources** (Agregar recursos).
4. Elija los recursos que pertenezcan a su solución de IoT.
5. Haga clic en **Agregar**. 

Felicidades. Ha agregado un nuevo recurso a la solución de IoT.

ASC for IoT ahora supervisa los recursos que acaba de agregar y muestra alertas y recomendaciones de seguridad pertinentes como parte de la solución de IoT.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a crear módulos de seguridad...

> [!div class="nextstepaction"]
> [Creación de módulos de seguridad](quickstart-create-security-twin.md)
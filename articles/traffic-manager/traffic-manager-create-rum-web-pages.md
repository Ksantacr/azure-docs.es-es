---
title: Real User Measurements de Azure Traffic Manager con páginas web | Microsoft Docs
description: Configuración de páginas web para enviar medidas de usuarios reales a Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: d6b669f5baae13f9fb57c5c58060e70a8f871e17
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884111"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Envío de medidas de usuarios reales a Azure Traffic Manager mediante páginas web

Para configurar las páginas web para enviar medidas de usuarios reales a Traffic Manager, obtenga una clave de Real User Measurements (RUM) e inserte el código generado en la página web.

## <a name="obtain-a-real-user-measurements-key"></a>Obtención de una clave de Real User Measurements

El servicio identifica las medidas que realiza y envía a Traffic Manager desde su aplicación cliente mediante una cadena única, llamada **Real User Measurements (RUM)**. Puede obtener una clave RUM mediante Azure Portal, una API de REST o por medio de PowerShell o la CLI de Azure.

Para obtener la clave RUM con Azure Portal:
1. En un explorador, inicie sesión en Azure Portal. Si aún no tiene una cuenta, puede registrarse para disfrutar de una evaluación gratuita de un mes.
2. En la barra de búsqueda del portal, busque el nombre del perfil de Traffic Manager que desea modificar y haga clic en él en los resultados que se muestran.
3. En la hoja de perfil de Traffic Manager, haga clic en **Real User Measurements** en **Configuración**.
4. Haga clic en **Generar clave** para crear una nueva clave RUM.
 
   ![Generación de una clave de Real User Measurements](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Figura 1: Generación de la clave de Mediciones de usuario reales**

5. La hoja muestra ahora la clave RUM generada y un fragmento de código de JavaScript que debe insertarse en la página HTML.
 
    ![Código JavaScript para la clave de Real User Measurements](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Figura 2: Clave de Mediciones de usuario reales y JavaScript de medida**
 
6. Haga clic en el botón **Copiar** para copiar el código JavaScript. 

>[!IMPORTANT]
> Use el código JavaScript generado para que la característica Real User Measurements funcione correctamente. Los cambios realizados en este script o en los scripts usados por Real User Measurements pueden llevar a comportamientos impredecibles.

## <a name="embed-the-code-to-an-html-web-page"></a>Inserción del código en una página web HTML

Después de haber obtenido la clave RUM, el siguiente paso es insertar este código JavaScript copiado en una página HTML que visiten sus usuarios finales. La edición de HTML puede realizarse de muchas maneras y con distintas herramientas y flujos de trabajo. En este ejemplo se muestra cómo actualizar una página HTML para agregar este script. Puede usarlo como guía y adaptarlo a su flujo de trabajo de administración de código fuente HTML.

1.  Abra la página HTML en un editor de texto.
2.  Pegue el código de JavaScript que ha copiado en el paso anterior en la sección de cuerpo de HTML (el código copiado está en línea 8 y 9, consulte la figura 3).
 
    ![Inserción de código JavaScript en la página web para Real User Measurements](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Figura 3: HTML simple con JavaScript insertado de Mediciones de usuario reales**

3.  Guarde el archivo HTML y hospédelo en un servidor web conectado a Internet. 
4. La próxima vez que se represente la página en un explorador web, el código JavaScript al que se hace referencia se descarga y el script ejecuta las operaciones de medidas e informes.


## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [Real User Measurements](traffic-manager-rum-overview.md).
- Aprenda [cómo funciona el Administrador de tráfico](traffic-manager-overview.md)
- Aprenda más sobre los [métodos de enrutamiento de tráfico](traffic-manager-routing-methods.md) que admite el Administrador de tráfico.
- Aprenda a [crear un perfil del Administrador de tráfico](traffic-manager-create-profile.md)


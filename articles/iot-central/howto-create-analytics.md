---
title: Análisis de los datos del dispositivo en la aplicación de Azure IoT Central | Microsoft Docs
description: Analice los datos del dispositivo en la aplicación de Azure IoT Central.
author: lmasieri
ms.author: lmasieri
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 05915e2059e45c002a9dfa71a538ed6391caa157
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60886534"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cómo usar análisis para analizar los datos del dispositivo

*Este artículo se aplica a los administradores, operadores y compiladores.*

Azure IoT Central proporciona completas funcionalidades de análisis para dotar de sentido a las enormes cantidades de datos de sus dispositivos. Para comenzar, visite **Analytics** en el menú de navegación izquierdo.

## <a name="querying-your-data"></a>Consulta de los datos

Debe elegir un **conjunto de dispositivos**, agregue un **filtro** (opcional) y seleccione un **período de tiempo** para empezar a trabajar. Una vez que haya terminado, seleccione **mostrar resultados** para comenzar a visualizar los datos.

* **Conjuntos de dispositivos**: un [conjunto de dispositivos](howto-use-device-sets.md) es un grupo de dispositivos definido por el usuario. Por ejemplo, todos los refrigeradores en Oakland o todas las turbinas eólicas rev 2.0.

* **Filtros**: también puede agregar filtros a la búsqueda para centrarse en los datos. Puede agregar hasta 10 filtros a la vez. Por ejemplo, en todos los refrigeradores en Oakland, busque los que han tenido una temperatura superior a 60 grados.
* **Período de tiempo**: de forma predeterminada, se podrán recuperar datos de los últimos 10 minutos. Puede cambiar este valor a uno de los intervalos de tiempo predefinidos o seleccionar un período de tiempo personalizado.

  ![Consulta de Analytics](media/howto-create-analytics/analytics-query.png)

## <a name="visualizing-your-data"></a>Visualización de los datos

Cuando haya consultado los datos, podrá comenzar a visualizarlos. Se pueden mostrar u ocultar las medidas, cambiar la forma en que se agregan los datos y dividirlos por diferentes propiedades del dispositivo.  

* **Dividir por**: la división de datos por propiedades del dispositivo le permite explorar en profundidad los datos. Por ejemplo, puede dividir los resultados por identificador de dispositivo o por ubicación.

* **Medidas**: puede mostrar u ocultar hasta 10 elementos de telemetría distintos que notifiquen los dispositivos a la vez. Las medidas corresponden a elementos tales como la temperatura y la humedad.

* **Agregación**: de forma predeterminada, los datos se agregan por promedio, pero puede cambiar el criterio de agregación de datos por otro que se ajuste sus necesidades.

   ![Visualización de Analytics dividido por](media/howto-create-analytics/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Interacción con los datos

Hay varias maneras de cambiar los resultados de consulta para satisfacer las necesidades de visualización. Puede alternar entre una vista de gráfico y una vista de cuadrícula, acercar y alejar, actualice el conjunto de datos y modificar cómo se muestran las líneas.

* **Mostrar cuadrícula**: Los resultados están disponibles en un formato de tabla, lo que le permite ver el valor específico para cada punto de datos. Esta vista también cumple los estándares de seguridad.
* **Mostrar gráfico**: Los resultados se muestran en un formato de línea para ayudarle a identificar hacia arriba o hacia abajo tendencias y anomalías.

  ![Presentación de la vista de cuadrícula para los análisis](media/howto-create-analytics/analytics-showgrid.png)

Zoom le permite a la página principal de en los datos. Si encuentra un período de tiempo en el que quiere centrarse dentro del conjunto de resultados, use el cursor para seleccionar el área que desea ampliar y utilice los controles disponibles para realizar una de las siguientes acciones:

* **Acercar**: Una vez que haya seleccionado un período de tiempo, haga zoom está habilitada y le permite acercar a los datos.
* **Alejar**: este control le permite alejarse un nivel a partir del último zoom. Por ejemplo, si ha zoom a sus datos tres veces, zoom tarda cómo realizar una copia un paso a la vez.
* **Restablecer zoom**: cuando haya ejecutado varios niveles de zoom, puede usar el control de restablecimiento de zoom para volver al conjunto de resultados original.

  ![Ejecución del zoom en los datos](media/howto-create-analytics/analytics-zoom.png)

Puede cambiar el estilo de línea para ajustarse sus necesidades. Tiene cuatro opciones:

* **Línea**: Una línea plana entre cada uno de los puntos de datos.
* **Suave**: Una línea curva entre cada punto.
* **Paso**: Línea entre cada punto en el gráfico es un paso.
* **Dispersión**: Todos los puntos se trazan en el gráfico sin las líneas que conectan a ellos.

  ![Distintos tipos de línea disponibles en Analytics](media/howto-create-analytics/analytics-linetypes.png)

Por último, puede organizar los datos en el eje y eligiendo uno de tres modos:

* **Apilado**: se apila un gráfico de cada medida y cada gráfico tiene su propio eje Y. Los gráficos apilados son útiles cuando tiene varias medidas seleccionadas y desea tener una vista distinta de estas medidas.
* **No apilado**: se traza un gráfico de cada medida con respecto a un eje Y, pero los valores del eje Y se cambian en función de la medida resaltada. Los gráficos no apilados son útiles cuando se quieren superponer varias medidas y se quieren ver los patrones entre estas medidas para el mismo intervalo de tiempo.
* **Eje Y compartido**: todos los gráficos comparten el mismo eje Y y los valores del eje no cambian. Los gráficos de eje Y compartido son útiles cuando se quiere examinar una única medida mientras se dividen los datos con Split-by (Dividir por).

  ![Organización de datos en el eje Y con distintos modos de visualización](media/howto-create-analytics/analytics-yaxis.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear análisis personalizado para su aplicación de Azure IoT Central, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Preparación y conexión de una aplicación de Node.js](howto-connect-nodejs.md)
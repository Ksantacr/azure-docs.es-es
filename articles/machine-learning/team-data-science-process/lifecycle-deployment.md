---
title: Fase de implementación del ciclo de vida del proceso de ciencia de datos en equipos
description: Los objetivos, las tareas y los resultados de la fase de implementación de los proyectos de ciencia de datos.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 00710183828892c81d3ea887e4394237288eb6bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303543"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Fase de implementación del ciclo de vida del proceso de ciencia de datos en equipos

En este artículo se describen los objetivos, las tareas y los resultados asociados a la implementación del Proceso de ciencia de datos en equipo (TDSP). Este proceso proporciona un ciclo de vida recomendado que puede usar para estructurar los proyectos de ciencia de datos. El ciclo de vida describe las fases principales por las que pasan normalmente los proyectos, a menudo de forma iterativa:

   1. **Conocimiento del negocio**
   2. **Adquisición y comprensión de los datos**
   3. **Modelado**
   4. **Implementación**
   5. **Aceptación del cliente**

Esta es una representación visual del ciclo de vida de TDSP: 

![Ciclo de vida del TDSP](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Objetivo
Implemente modelos con canalización de datos en un entorno de producción o similar para que el usuario final los acepte. 

## <a name="how-to-do-it"></a>Modo de hacerlo
La tarea principal que se aborda en esta fase es la siguiente:

**Poner en marcha el modelo**: implemente el modelo y la canalización en un entorno de producción o semejante para el consumo de aplicaciones.

### <a name="operationalize-a-model"></a>Uso de modelos
Cuando ya disponga de un conjunto de modelos que funcionan bien, los puede hacer operativos para que los consuman otras aplicaciones. Dependiendo de los requisitos empresariales, se realizan predicciones en tiempo real o por lotes. Para implementar modelos, los expone con una interfaz de API abierta. La interfaz permite que el modelo se utilice fácilmente por diferentes aplicaciones, como las siguientes:

   * Sitios web en línea
   * Hojas de cálculo 
   * Paneles
   * Aplicaciones de línea de negocio 
   * Aplicaciones de back-end 

Para obtener información sobre cómo crear e implementar un servicio web de Azure Machine Learning, consulte [Implementación de un servicio web Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md). Es un procedimiento recomendado compilar la telemetría y la supervisión en el modelo de producción y la canalización de datos que se implementan. Este procedimiento ayuda con las posteriores tareas de solución de problemas e informes de estado del sistema.  

## <a name="artifacts"></a>Artefactos

* Un panel de estado que muestra el estado del sistema y métricas clave.
* Un informe de modelado final con detalles de implementación.
* Un documento de arquitectura de la solución final.


## <a name="next-steps"></a>Pasos siguientes

Estos son los vínculos a cada uno de los pasos del ciclo de vida del Proceso de ciencia de datos en equipo:

   1. [Conocimiento del negocio](lifecycle-business-understanding.md)
   2. [Adquisición y comprensión de los datos](lifecycle-data.md)
   3. [Modelado](lifecycle-modeling.md)
   4. [Implementación](lifecycle-deployment.md)
   5. [Aceptación del cliente](lifecycle-acceptance.md)

Proporcionamos tutoriales completos que muestran todos los pasos del proceso en escenarios concretos. El artículo con [tutoriales de ejemplo](walkthroughs.md) proporciona una lista de los escenarios con vínculos y descripciones de miniatura. En los tutoriales se muestra cómo combinar servicios y herramientas en la nube y locales en un flujo de trabajo o una canalización con el fin de crear una aplicación inteligente. 

Para obtener ejemplos de cómo ejecutar pasos en TDSP que usan Microsoft Azure Machine Learning Studio, consulte [Uso del Proceso de ciencia de los datos en equipos con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

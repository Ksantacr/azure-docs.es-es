---
title: Supervisión y administración de trabajos de Azure Stream Analytics con Visual Studio
description: En este artículo se describe cómo usar Visual Studio para supervisar y administrar trabajos de Azure Stream Analytics.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: e673d4f1c2d58a61c541cca4f8448dffc003f9fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479435"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-visual-studio"></a>Supervisión y administración de trabajos de Stream Analytics con Visual Studio

En este artículo se muestra cómo supervisar el trabajo de Stream Analytics en Visual Studio. Las herramientas de Azure Stream Analytics para Visual Studio proporcionan una experiencia de supervisión similar a Azure Portal sin tener que salir del IDE. Puede empezar a supervisar un trabajo tan pronto haga clic en **Enviar a Azure** desde su **Script.asaql**, o puede supervisar los trabajos existentes con independencia de cómo se hayan creado. 

## <a name="job-summary"></a>Resumen de trabajos

En **Resumen de trabajos** y **Job Metrics** (Métrica de trabajos) se proporciona una instantánea rápida de su trabajo. De un vistazo, puede determinar la información de estado y eventos de un trabajo.

<img src="./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-job-summary-metrics.png" alt="Stream Analytics job summary and job metrics" width="300px"/> 


## <a name="job-metrics"></a>Métricas de trabajo

Puede contraer la pestaña **Resumen de trabajos** y hacer clic en la pestaña **Job Metrics** (Métrica de trabajos) para ver un gráfico con métricas importantes. Marque o desmarque tipos de métricas para agregarlas o quitarlas del gráfico.

![Métricas de Stream Analytics en Visual Studio](./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-vs-metrics.png)


## <a name="error-monitoring"></a>Supervisión de errores

También puede hacer clic en la pestaña **Errores** para supervisar los errores.

![Errores de Stream Analytics en Visual Studio](./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-vs-errors.png)


## <a name="get-support"></a>Obtención de soporte técnico
Para obtener más ayuda, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Creación de un trabajo de Azure Stream Analytics con Visual Studio](stream-analytics-quick-create-vs.md)
* [Instalación de las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-tools-for-visual-studio-install.md) 



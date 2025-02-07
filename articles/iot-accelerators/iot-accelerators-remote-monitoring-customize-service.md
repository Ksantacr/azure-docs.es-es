---
title: Agregar un servicio a la solución de supervisión remota UI - Azure | Microsoft Docs
description: En este artículo se muestra cómo agregar un nuevo servicio a la interfaz de usuario web del acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: v-yiso
ms.service: iot-accelerators
services: iot-accelerators
origin.date: 10/02/2018
ms.date: 11/26/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447053"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Adición de un servicio personalizado a la interfaz de usuario web del acelerador de soluciones de supervisión remota

En este artículo se muestra cómo agregar un nuevo servicio a la interfaz de usuario web del acelerador de soluciones de supervisión remota. El artículo describe:

- Cómo preparar un entorno de desarrollo local.
- Cómo agregar un nuevo servicio a la interfaz de usuario web.

El servicio de ejemplo de este artículo proporciona datos para una cuadrícula que el artículo de procedimientos [Add a custom grid to the Remote Monitoring solution accelerator web UI](iot-accelerators-remote-monitoring-customize-grid.md) (Agregar una cuadrícula personalizada a la interfaz de usuario web del acelerador de soluciones de supervisión remota) muestra cómo agregar.

En una aplicación React, un servicio normalmente interactúa con un servicio back-end. Algunos ejemplos del acelerador de soluciones de supervisión remota incluyen servicios que interactúan con el Administrador de IoT Hub y los microservicios de configuración.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de esta guía, necesita que el software siguiente esté instalado en la máquina de desarrollo local:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Antes de comenzar

Debe completar los pasos descritos en el artículo de procedimientos [Adición de una página personalizada a la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-page.md) antes de continuar.

## <a name="add-a-service"></a>Agregar un servicio

Para agregar un servicio a la interfaz de usuario web, deberá agregar los archivos de origen que definen el servicio y modificar algunos archivos existentes para hacer que la interfaz de usuario web sea consciente del nuevo servicio.

### <a name="add-the-new-files-that-define-the-service"></a>Adición de nuevos archivos que definen el servicio

Para comenzar, la carpeta **src/walkthrough/services** contiene los archivos que definen un servicio sencillo:

**exampleService.js**



Para obtener más información acerca de cómo se implementan los servicios, vea [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) (La introducción a la programación reactiva que se ha perdido).

**model/exampleModels.js**



Copie **exampleService.js** a la carpeta **src/services** y copie **exampleModels.js** a la carpeta **src/services/models**.

Actualice el archivo **index.js** de la carpeta **src/services** para exportar el nuevo servicio:

```js
export * from './exampleService';
```

Actualice el archivo **index.js** de la carpeta **src/services/models** para exportar el nuevo modelo:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Configuración de las llamadas al servicio desde el almacén

Para comenzar, la carpeta **src/walkthrough/store/reducers** contiene un reductor de ejemplo:

**exampleReducer.js**



Copie **exampleReducer.js** a la carpeta **src/store/reducers**.

Para obtener más información acerca del reductor y **Epopeyas**, consulte [redux-observable](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>Configuración del middleware

Para configurar el middleware, agregue el reductor al archivo **rootReducer.js** en la carpeta **src/store**:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

Agregue las epopeyas al archivo **rootEpics.js** de la carpeta **src/store**:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió sobre los recursos que tiene a su disposición para ayudarlo a agregar o personalizar servicios en la interfaz de usuario web en el acelerador de la solución de supervisión remota.

Ya ha definido un servicio, el paso siguiente consiste en [agregar una cuadrícula personalizada a la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-grid.md) que muestra los datos que devuelve el servicio.

Para información más conceptual sobre el acelerador de la solución de supervisión remota, vea [Arquitectura de supervisión remota](iot-accelerators-remote-monitoring-sample-walkthrough.md).

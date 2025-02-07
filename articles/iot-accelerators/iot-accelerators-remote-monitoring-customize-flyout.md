---
title: Agregar un control flotante para la solución de supervisión remota UI - Azure | Microsoft Docs
description: En este artículo se muestra cómo agregar un nuevo control flotante en una pagina de la interfaz de usuario web del acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: v-yiso
ms.service: iot-accelerators
services: iot-accelerators
origin.date: 10/05/2018
ms.date: 11/26/2018
ms.topic: conceptual
ms.openlocfilehash: ccb1a7ff6abbc68f42c7632a8ba7a392b2c48794
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447121"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Adición de un control flotante personalizado a la interfaz de usuario web del acelerador de soluciones de supervisión remota

En este artículo se muestra cómo agregar un nuevo control flotante en una pagina de la interfaz de usuario web del acelerador de soluciones de supervisión remota. El artículo describe:

- Cómo preparar un entorno de desarrollo local.
- Cómo agregar un nuevo control flotante a una página de la interfaz de usuario web.

El control flotante de ejemplo de este artículo se muestra en la página con la cuadrícula que el artículo de procedimientos [Adición de una cuadrícula personalizada a la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-grid.md) muestra cómo agregar.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de esta guía, necesita que el software siguiente esté instalado en la máquina de desarrollo local:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Antes de comenzar

Debe completar los pasos descritos en los artículos siguientes antes de continuar:

- [Adición de una página personalizada a la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-page.md).
- [Adición de un servicio personalizado a la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-service.md)
- [Adición de una cuadrícula personalizada a la interfaz de usuario web del acelerador de soluciones de Supervisión remota](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Agregar un control flotante

Para agregar un control flotante a la interfaz de usuario web, deberá agregar los archivos de origen que definen el control flotante y modificar algunos archivos existentes para hacer que la interfaz de usuario web sea consciente del nuevo componente.

### <a name="add-the-new-files-that-define-the-flyout"></a>Adición de los nuevos archivos que definen el control flotante

Para comenzar, la carpeta **src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout** contiene los archivos que definen un control flotante:

**exampleFlyout.container.js**



**exampleFlyout.js**



Copie la carpeta **src/walkthrough/components/pages/pageWithFlyout/flyouts** a la carpeta **src/components/pages/example**.

### <a name="add-the-flyout-to-the-page"></a>Incorporación del control flotante a la página

Modifique el archivo **src/components/pages/example/basicPage.js** para agregar el control flotante.

Agregue **Btn** a las importaciones desde **components/shared** y agregue las importaciones para **svgs** y **ExampleFlyoutContainer**:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

Agregue una definición de **const** para **closedFlyoutState** y agréguelo al estado en el constructor:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

Agregue las siguientes funciones a la clase **BasicPage**:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

Agregue las siguientes definiciones **const** a la función **render**:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Agregue un botón para abrir el control flotante para el menú contextual:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Agregue texto y el contenedor del control flotante para el contenido de la página:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>Prueba del control flotante

Si la interfaz de usuario web no se está ejecutando localmente, ejecute el siguiente comando en la raíz de la copia local del repositorio:

```cmd/sh
npm start
```

El comando anterior ejecuta la interfaz de usuario localmente en [http://localhost:3000/dashboard](http://localhost:3000/dashboard). Vaya a la página **ejemplo** y haga clic en **Open Flyout** (Abrir control flotante).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió sobre los recursos que tiene a su disposición para ayudarle a agregar o personalizar páginas en la interfaz de usuario web del acelerador de la solución de supervisión remota.

Ya ha definido un control flotante en una página, el paso siguiente consiste en [agregar un panel al panel personalizado en la interfaz de usuario web del acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-customize-panel.md).

Para información más conceptual sobre el acelerador de soluciones de supervisión remota, vea [Arquitectura de supervisión remota](iot-accelerators-remote-monitoring-sample-walkthrough.md).

---
title: Ejecución de varios servicios dependientes con Node.js y VS Code
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: DrEsteban
ms.author: stevenry
ms.date: 11/21/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: d6c09b51a0b5200ed8b723042b7ee5629210c4d2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800895"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Desarrollo de varios servicios con Azure Dev Spaces

En este tutorial, aprenderá cómo puede desarrollar aplicaciones de varios servicios con Azure Dev Spaces, junto con algunas de las ventajas que proporciona Dev Spaces.

## <a name="call-a-service-running-in-a-separate-container"></a>Llamada a un servicio que se ejecuta en un contenedor diferente

En esta sección va a crear un segundo servicio `mywebapi` y a hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Abra el código de ejemplo de *mywebapi*.
Ya debería tener el código de ejemplo de `mywebapi` necesario en esta guía, en una carpeta llamada `samples` (si no es así, vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub). El código de esta sección se encuentra en `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra la carpeta `mywebapi` en una *ventana independiente de VS Code*.
1. Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Este comando no se debe confundir con el comando `azds prep`, que configura el proyecto para la implementación.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando aparezca el mensaje *Listening on port 80* (Escuchando en el puerto 80) en la consola de depuración.
1. Anote la dirección URL del punto de conexión; se parecerá a esta `http://localhost:<portnumber>`. **Sugerencia: La barra de estado de VS Code mostrará una dirección URL en la que se puede hacer clic.** Puede parecer que el contenedor se ejecuta localmente, pero en realidad se ejecuta en el entorno de desarrollo de Azure. La razón de la dirección de localhost es que `mywebapi` no ha definido ningún punto de conexión público y solo se puede acceder a él desde la instancia de Kubernetes. Para mayor comodidad, y para facilitar la interacción con el servicio privado desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.
1. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost. Verá una respuesta del servicio `mywebapi` ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`.
1. Cambie a la ventana de VS Code para `webfrontend`.
1. Agregue estas líneas de código en la parte superior de `server.js`:
    ```javascript
    var request = require('request');
    ```

3. *Reemplace* el código por el controlador GET `/api`. Al administrar una solicitud, se realiza a su vez una llamada a `mywebapi` y se devuelven los resultados de ambos servicios.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
   1. *Quite* la línea `server.close()` al final de `server.js`

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante verá cómo esta función ayuda a los equipos con el desarrollo en colaboración.

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el controlador GET `/` predeterminado.
1. En el proyecto `webfrontend`, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `http://mywebapi`.
1. Presione F5 en el proyecto `webfrontend`.
1. Abra la aplicación web y recorra el código de ambos servicios. La aplicación web debe mostrar un mensaje concatenado por los dos servicios: "Hello from webfrontend y Hello from mywebapi".

### <a name="automatic-tracing-for-http-messages"></a>Seguimiento automático de los mensajes HTTP
Puede que haya observado que, aunque *webfrontend* no contiene ningún código especial para imprimir la llamada HTTP que realiza a *mywebapi*, puede ver que HTTP realiza un seguimiento de mensajes en la ventana de salida:
```
// The request from your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io --hyh-> webfrontend:
   GET /api?_=1544485357627 HTTP/1.1

// *webfrontend* reaching out to *mywebapi*
webfrontend --1b1-> mywebapi:
   GET / HTTP/1.1

// Response from *mywebapi*
webfrontend <-1b1-- mywebapi:
   HTTP/1.1 200 OK
   Hello from mywebapi

// Response from *webfrontend* to your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io <-hyh-- webfrontend:
   HTTP/1.1 200 OK
   Hello from webfrontend and Hello from mywebapi
```
Esta es una de las ventajas "gratuitas" que obtendrá de la instrumentación de Dev Spaces. Insertamos los componentes que realizan los seguimientos de las solicitudes HTTP a medida que atraviesan el sistema para que le sea más fácil realizar un seguimiento de llamadas complejas a varios servicios durante el desarrollo.

### <a name="well-done"></a>¡Listo!
Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el desarrollo en Dev Spaces](team-development-nodejs.md)

---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: fe1b4699a300831294c26b103d322fb83ad87d3b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116756"
---
Si necesita configurar un proxy HTTP para realizar solicitudes de salida, use estos dos argumentos:

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|--|--|
|HTTP_PROXY|string|El proxy que se va a utilizar, por ejemplo, `http://proxy:8888`<br><proxy-url>|
|HTTP_PROXY_CREDS|string|Las credenciales necesarias para autenticarse en el proxy, por ejemplo, nombre de usuario:contraseña.|
|`<proxy-user>`|string|El usuario del proxy.|
|`proxy-password`|string|La contraseña asociada con `<proxy-user>` para el proxy.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
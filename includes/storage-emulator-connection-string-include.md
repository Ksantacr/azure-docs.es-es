---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114671"
---
El emulador de almacenamiento es compatible con una sola cuenta fija y una clave de autenticación ya conocida para autenticación de clave compartida. Esta cuenta y clave son las únicas credenciales de clave compartida que se admiten para su uso con el emulador de almacenamiento. Son las siguientes:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> La clave de autenticación admitida por el emulador de almacenamiento está pensada para comprobar únicamente la funcionalidad de su código de autenticación de cliente. No responde a ningún propósito de seguridad. A parte, no puede utilizar la cuenta de almacenamiento y la clave de producción con el emulador. Se debe tener en cuenta que no se puede utilizar la cuenta de desarrollo con datos de producción.
> 
> El emulador de almacenamiento admite solo la conexión a través de HTTP. Sin embargo, HTTPS es el protocolo recomendado para obtener acceso a recursos en una cuenta de producción de Azure Storage.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Conexión a la cuenta del emulador mediante un acceso directo
La manera más fácil de conectarse al emulador de almacenamiento desde su aplicación consiste en configurar, dentro del archivo de configuración de la aplicación, una cadena de conexión que haga referencia al método abreviado `UseDevelopmentStorage=true`. Este es un ejemplo de una cadena de conexión al emulador de almacenamiento en un archivo *app.config*: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Conexión a la cuenta del emulador con el nombre de cuenta y la clave conocidos
Para crear una cadena de conexión que hace referencia al nombre de la cuenta del emulador y la clave, debe especificar los puntos de conexión para cada uno de los servicios que desea usar desde el emulador en la cadena de conexión. Esto es necesario para que la cadena de conexión haga referencia a los extremos del emulador, que son diferentes de los de una cuenta de almacenamiento de producción. Por ejemplo, el valor de la cadena de conexión será similar al siguiente:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Este valor es idéntico al acceso directo mostrado anteriormente, `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Especificación de un servidor proxy HTTP
También puede especificar que se use un proxy HTTP cuando se está probando el servicio con el emulador de almacenamiento. Esto puede ser útil para observar solicitudes y respuestas HTTP mientras está depurando operaciones con los servicios de almacenamiento. Para especificar un proxy, agregue la opción `DevelopmentStorageProxyUri` a la cadena de conexión y establezca su valor en el URI del proxy. Por ejemplo, esta es una cadena de conexión que apunta al emulador de almacenamiento y configura un proxy HTTP:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```


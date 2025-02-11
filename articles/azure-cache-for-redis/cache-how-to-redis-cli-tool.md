---
title: Uso de redis-cli con Azure Cache for Redis | Microsoft Docs
description: Obtenga información para usar redis-cli con Azure Cache for Redis.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: yegu
ms.openlocfilehash: 318d02f5da816ae8fe2fe199b9c87b3748d5d1fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66132996"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Uso de la herramienta de línea de comandos de Redis con Azure Cache for Redis

*redis-cli.exe* es una herramienta de línea de comandos popular para interactuar con Azure Cache for Redis como cliente. Esta herramienta también está disponible para su uso con Azure Cache for Redis.

La herramienta está disponible para plataformas Windows al descargar las [herramientas de línea de comandos de Redis para Windows](https://github.com/MSOpenTech/redis/releases/). 

Si quiere ejecutar la herramienta de línea de comandos en otra plataforma, descargue Azure Cache for Redis desde [https://redis.io/download](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Recopilar información de acceso a la caché

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Puede recopilar la información necesaria para acceder a la memoria caché con tres métodos:

1. CLI de Azure mediante [az redis list-keys](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell mediante [Get AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey)
3. Mediante Azure Portal.

En esta sección se recuperan las claves desde Azure Portal.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Habilitar el acceso de redis-cli.exe

Con Azure Cache for Redis, solo está habilitado de forma predeterminada el puerto SSL (6380). La herramienta de línea de comandos `redis-cli.exe` no es compatible con SSL. Tiene dos opciones de configuración para usarla:

1. [Habilitar el puerto no SSL (6379)](cache-configure.md#access-ports) - **Esta configuración no se recomienda** porque en ella las claves de acceso se envían a través de TCP en texto no cifrado. Este cambio puede poner en peligro el acceso a la memoria caché. El único escenario donde se puede considerar esta configuración es cuando simplemente se va a acceder a una memoria caché de prueba.

2. Descargue e instale [stunnel](https://www.stunnel.org/downloads.html).

    Ejecute **stunnel GUI Start** (Inicio de GUI de stunnel) para iniciar el servidor.

    Haga clic con el botón derecho en el icono de la barra de tareas correspondiente al servidor de stunnel y haga clic en **Mostrar la ventana Registro**.

    En el menú Ventana Registro de stunnel, haga clic en **Configuración** > **Editar configuración** para abrir el archivo de configuración actual.

    Agregue la siguiente entrada de *redis-cli.exe* en la sección **Definiciones de servicio**. Inserte el nombre real de la caché en lugar de `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Guarde y cierre el archivo de configuración. 
  
    En el menú Ventana Registro de stunnel, haga clic en **Configuración** > **Volver a cargar configuración**.


## <a name="connect-using-the-redis-command-line-tool"></a>Conecte mediante la herramienta de línea de comandos de Redis.

Al usar stunnel, ejecute *redis-cli.exe* y pase solo el *puerto* y la *clave de acceso* (primaria o secundaria) para conectarse a la memoria caché.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![stunnel con redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Si usa una memoria caché de prueba con el puerto no SSL **no seguro**, ejecute `redis-cli.exe` y pase el *nombre de host*, el *puerto* y la *clave de acceso* (primaria o secundaria) para conectarse a la caché de prueba.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![stunnel con redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el uso de la [consola de Redis](cache-configure.md#redis-console) para emitir comandos.


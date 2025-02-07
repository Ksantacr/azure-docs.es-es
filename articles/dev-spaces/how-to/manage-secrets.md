---
title: Administración de secretos cuando se trabaja con un espacio de Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores
ms.openlocfilehash: 8ee50289083b12b7b2abd3b9ece2c8de345df9fe
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851427"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Administración de secretos cuando se trabaja con un espacio de Azure Dev Spaces

Los servicios pueden requerir algunas contraseñas, cadenas de conexión y otros secretos, como para las bases de datos u otros servicios de Azure seguros. Al establecer los valores de estos secretos en archivos de configuración, puede que estén disponibles en el código como variables de entorno.  Estos se deben administrar con cuidado para evitar poner en peligro la seguridad de los secretos.

Azure Dev Spaces proporciona dos opciones recomendadas para almacenar secretos: en el archivo values.dev.yaml y en línea directamente en azds.yaml. No se recomienda almacenar secretos en values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Método 1: values.dev.yaml
1. Abra VS Code con el proyecto que está habilitado para Azure Dev Spaces.
2. Agregue un archivo denominado _values.dev.yaml_ en la misma carpeta que existente _azds.yaml_ y definir la clave secreta y valores, como se muestra en el ejemplo siguiente:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds.yaml_ ya hace referencia a la _values.dev.yaml_ archivo si existe. Si lo prefiere, un nombre de archivo diferente, actualice la sección install.values:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Modifique el código de servicio para hacer referencia a estos secretos como variables de entorno, como en el ejemplo siguiente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Actualice los servicios que se ejecutan en el clúster con estos cambios. En la línea de comandos ejecute el comando:

    ```
    azds up
    ```
 
6. (Opcional) En la línea de comandos, compruebe que se han creado estos secretos:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Asegúrese de que agrega _values.dev.yaml_ al archivo _.gitignore_ para evitar la confirmación de los secretos en el control de código fuente.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Método 2: en línea directamente en azds.yaml
1.  En _azds.yaml_, establezca los secretos en la sección de yaml configurations/develop/install. Aunque puede especificar valores de secreto directamente, no se recomienda porque _azds.yaml_ está protegido en el control de código fuente. En su lugar, agregue los marcadores de posición utilizando la sintaxis de "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT"
                host: "$REDIS_HOST"
                key: "$REDIS_KEY"
    ```
     
2.  Cree un archivo _.env_ en la misma carpeta que _azds.yaml_. Escriba secretos con clave estándar= notación de valor. No confirme el archivo _.env_ en el control de código fuente. (Para omitir el control de código fuente en sistemas de control de versiones basados en git, agréguelo al archivo _.gitignore_). En el ejemplo siguiente se muestra un archivo _.env_:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Modifique el código fuente del servicio para hacer referencia a estos secretos en el código, como en el ejemplo siguiente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Actualice los servicios que se ejecutan en el clúster con estos cambios. En la línea de comandos, ejecute el comando:

    ```
    azds up
    ```

4.  (opcional) Ver secretos de kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Pasos siguientes

Con estos métodos, ahora puede conectarse de forma segura a una base de datos, a una instancia de Azure Cache for Redis o acceder a los servicios de Azure seguros.
 

---
title: Registrar las extensiones de enlace de Azure Functions
description: Aprenda a registrar una extensión de enlace de Azure Functions en función de su entorno.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 53eb5fc9389d913ecacec3729a06e47a1c2bf56b
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864544"
---
# <a name="register-azure-functions-binding-extensions"></a>Registrar las extensiones de enlace de Azure Functions

En la versión de Azure Functions 2.x, [enlaces](./functions-triggers-bindings.md) están disponibles como paquetes independientes del tiempo de ejecución de funciones. Mientras que las funciones de .NET tener acceso a los enlaces a través de paquetes de NuGet, paquetes de extensión permiten el acceso de otras funciones a todos los enlaces a través de un valor de configuración.

Tenga en cuenta los siguientes elementos relacionados con extensiones de enlace:

- Las extensiones de enlace explícitamente no están registradas en Functions 1.x, excepto cuando [creando un C# biblioteca de clases mediante Visual Studio de 2019](#local-csharp).

- Los desencadenadores HTTP y el temporizador se admiten de forma predeterminada y no requieren una extensión.

En la tabla siguiente indica cómo y cuándo registrar los enlaces.

| Entorno de desarrollo |Registro<br/> en Functions 1.x  |Registro<br/> en Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure Portal|Automático|Automático|
|Lenguajes que no sean de. NET o desarrollo de herramientas de Azure Core local|Automático|[Use Azure Functions Core Tools y agrupaciones de extensión](#local-development-with-azure-functions-core-tools-and-extension-bundles)|
|C#biblioteca de clases mediante Visual Studio de 2019|[Uso de herramientas NuGet](#c-class-library-with-visual-studio-2019)|[Uso de herramientas NuGet](#c-class-library-with-visual-studio-2019)|
|Biblioteca de clases de C# con Visual Studio Code|N/D|[Uso de la CLI de .NET Core](#c-class-library-with-visual-studio-code)|

## <a name="local-development-with-azure-functions-core-tools-and-extension-bundles"></a>Desarrollo local con Azure Functions Core Tools y agrupaciones de extensión

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2019"></a>C#biblioteca de clases con Visual Studio de 2019

En **2019 de Visual Studio**, puede instalar paquetes desde la consola de administrador de paquetes con el [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) de comandos, tal como se muestra en el ejemplo siguiente:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Se proporciona el nombre del paquete que se usa para un enlace determinado en el artículo de referencia para ese objeto binding. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#packages---functions-1x).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="c-class-library-with-visual-studio-code"></a>Biblioteca de clases de C# con Visual Studio Code

En **Visual Studio Code**, puede instalar paquetes desde el símbolo de sistema mediante el comando [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) de la CLI de .NET Core, tal como se muestra en el ejemplo siguiente:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

La CLI de .NET Core solo puede utilizarse para el desarrollo de Azure Functions 2.x.

El nombre del paquete que se usará para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#packages---functions-1x).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Ejemplo de desencadenador y enlace de Azure (función)](./functions-bindings-example.md)


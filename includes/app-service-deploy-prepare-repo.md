---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/05/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 7ec4028c319749b6a3da019e1d320d3937e9c4b2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133175"
---
## <a name="prepare-your-repository"></a>Preparación del repositorio

Para obtener las compilaciones automáticas desde el servidor de compilación de Kudu de App Service de Azure, asegúrese de que la raíz del repositorio tiene los archivos correctos en el proyecto.

| Tiempo de ejecución | Archivos del directorio raíz |
|-|-|
| ASP.NET (solo Windows) | _*.sln_, _*.csproj_ o _default.aspx_ |
| ASP.NET Core | _*.sln_ o _*.csproj_ |
| PHP | _index.php_ |
| Ruby (solo Linux) | _Gemfile_ |
| Node.js | _server.js_, _app.js_ o _package.json_ con un script de inicio |
| Python | _\*.py_, _requirements.txt_ o _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_ o _iisstart.htm_ |
| WebJobs | _\<nombre_de_trabajo>/run.\<extensión>_ en _App\_Data/jobs/continuous_ (para WebJobs continuos) o _App\_Data/jobs/triggered_ (para WebJobs desencadenados). Para obtener más información, consulte [documentación de WebJobs de Kudu](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| Funciones | Consulte [Implementación continua para Azure Functions](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Para personalizar la implementación puede incluir un archivo _.deployment_ en la raíz del repositorio. Para obtener más información, consulte [personalizar implementaciones](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) y [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Si desarrolla en Visual Studio, deje que [Visual Studio cree un repositorio en su lugar](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio). El proyecto está listo para implementarse mediante Git.
>
>


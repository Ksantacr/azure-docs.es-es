---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 79fb8517ec6880e8a3eae0e74275567a24644b87
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132554"
---
## <a name="deployment-customization"></a>Personalización de la implementación

En el proceso de implementación se da por supuesto que el archivo .zip que se inserta contiene una aplicación lista para ejecutarse. No se ejecuta ninguna personalización de forma predeterminada. Para habilitar los mismos procesos de compilación que se obtienen con la integración continua, agregue los siguientes elementos a la configuración de la aplicación:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Cuando se realiza la implementación de inserción del archivo .zip, el valor que se establece de forma predeterminada es **false**. El valor predeterminado es **true** para las implementaciones de integración continua. Cuando el valor se establece en **true**, la configuración relacionada con la implementación se usa durante la implementación. Puede configurar estas opciones a modo de configuración de la aplicación o en un archivo de configuración de implementación que se encuentra en la raíz del archivo .zip. Para obtener más información, consulte [Repository and deployment-related settings](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) (Repositorio y configuración relacionada con la implementación) en la referencia de la implementación.
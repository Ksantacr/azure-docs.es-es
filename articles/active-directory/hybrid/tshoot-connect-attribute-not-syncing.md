---
title: Solución de problemas de sincronización de atributos en Azure AD Connect | Microsoft Docs
description: En este tema se proporcionan los pasos para solucionar problemas relacionados con la sincronización de atributos mediante la tarea de solución de problemas.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a639b14c9313179816f6376aa0c5642a645ea344
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456001"
---
# <a name="troubleshoot-an-attribute-not-synchronizing-in-azure-ad-connect"></a>Solución de problemas de sincronización de atributos en Azure AD Connect

## <a name="recommended-steps"></a>**Pasos recomendados**

Antes de investigar los problemas de sincronización de un atributo, revisemos el proceso de sincronización de **Azure AD Connect**:

  ![Proceso de sincronización de Azure AD Connect](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminología**

* **CS:** espacio conector, una tabla de la base de datos.
* **MV:** metaverso, una tabla de la base de datos.
* **AD:** Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Pasos de sincronización**

* Importar desde AD: los objetos de Active Directory se incorporan al espacio conector de AD.

* Importar desde AAD: los objetos de Azure Active Directory se incorporan al espacio conector de AAD.

* Sincronización: **reglas de sincronización entrantes** y **reglas de sincronización salientes** se ejecutan de menor a mayor según el número de orden de prioridad. Para ver las reglas de sincronización, puede ir al **Editor de reglas de sincronización** desde las aplicaciones de escritorio. Las **reglas de sincronización entrantes** transfieren los datos del espacio conector al metaverso. Las **reglas de sincronización salientes** transfieren los datos del metaverso al espacio conector.

* Exportar a AD: después de ejecutar la sincronización, se exportan objetos del espacio conector de AD a **Active Directory**.

* Exportar a AAD: después de ejecutar la sincronización, se exportan objetos del espacio conector de AAD a **Azure Active Directory**.

### <a name="step-by-step-investigation"></a>**Investigación paso a paso**

* Comenzaremos a nuestra búsqueda desde el **metaverso** y examinaremos la asignación de atributos del origen al destino.

* Inicie **Synchronization Service Manager** desde las aplicaciones de escritorio como se muestra a continuación:

  ![Iniciar Synchronization Service Manager](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/startmenu.png)

* En **Synchronization Service Manager**, seleccione **Metaverse Search** (Búsqueda de metaverso), **Scope by Object Type** (Ámbito por tipo de objeto), seleccione el objeto con un atributo y haga clic en el botón de **Búsqueda**.

  ![Búsqueda de metaverso](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvsearch.png)

* Haga doble clic en el objeto que devuelve la búsqueda de **metaverso** para ver todos sus atributos. Puede hacer clic en la pestaña **Conectores** para ver el objeto correspondiente en todos los **espacios conector**.

  ![Conectores del objeto de metaverso](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvattributes.png)

* Haga doble clic en el **conector de Active Directory** para ver los atributos del **espacio conector**. Haga clic en el botón de **Vista previa**  y, en el cuadro de diálogo siguiente, haga clic en el botón **Generate Preview** (Generar vista previa).

  ![Atributos del espacio conector](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/csattributes.png)

* Ahora haga clic en el botón **Import Attribute Flow** (Importar flujo de atributos) para que se muestre el flujo de atributos del **espacio conector de Active Directory** al **metaverso**. La columna de **regla de sincronización** muestra qué **regla de sincronización** contribuye a ese atributo. La columna **origen de datos** muestra los atributos del **espacio conector**. La columna de **atributo de metaverso** muestra los atributos en el **metaverso**. Puede buscar el atributo que no se está sincronizando aquí. Si no encuentra el atributo, quiere decir que no está asignado y es necesario que cree una nueva **regla de sincronización** para asignarlo.

  ![Atributos del espacio conector](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/cstomvattributeflow.png)

* Haga clic en **Export Attribute Flow** (Exportar flujo de atributos) en el panel izquierdo para ver el flujo de atributos del **metaverso** al **espacio conector de Active Directory** mediante las **reglas de sincronización salientes**.

  ![Atributos del espacio conector](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvtocsattributeflow.png)

* De forma similar, puede ver el objeto del **espacio conector de Azure Active Directory** y puede generar la **vista previa** para visualizar el flujo de atributos del **metaverso** al **espacio conector** y viceversa. De este modo puede investigar por qué un atributo no se está sincronizando.

## <a name="recommended-documents"></a>**Documentos recomendados**
* [Sincronización de Azure AD Connect: conceptos técnicos](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Sincronización de Azure AD Connect: descripción de la arquitectura](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Sincronización de Azure AD Connect: Descripción del aprovisionamiento declarativo](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Sincronización de Azure AD Connect: descripción de las expresiones de aprovisionamiento declarativas](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Sincronización de Azure AD Connect: descripción de la configuración predeterminada](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Sincronización de Azure AD Connect: descripción de usuarios, grupos y contactos](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Sincronización de Azure AD Connect: Atributos paralelos](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Pasos siguientes

- [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md).
- [¿Qué es la identidad híbrida?](whatis-hybrid-identity.md)

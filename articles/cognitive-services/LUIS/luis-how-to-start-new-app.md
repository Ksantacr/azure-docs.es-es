---
title: Creación de una nueva aplicación
titleSuffix: Language Understanding - Azure Cognitive Services
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 9d650a17ddfac6461341e50c4693e4522d9628b3
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148195"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Creación de una aplicación de LUIS en el portal de LUIS
Hay un par de formas de crear aplicaciones de LUIS. Puede crear una aplicación de LUIS en el portal de [LUIS](https://www.luis.ai), o bien mediante las [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) de creación de LUIS.

## <a name="using-the-luis-portal"></a>Mediante el portal de LUIS
Puede crear una aplicación en el portal de LUIS de varias maneras:

* Comience con una aplicación vacía y cree intenciones, expresiones y entidades.
* Comience con una aplicación vacía y agregue un [dominio creado previamente](luis-how-to-use-prebuilt-domains.md).
* Importe una aplicación de LUIS desde un archivo JSON que ya contenga intenciones, expresiones y entidades.

## <a name="using-the-authoring-apis"></a>Mediante las API de creación
Puede crear una aplicación con las API de creación de dos maneras:

* [Comience](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) con una aplicación vacía y cree intenciones, expresiones y entidades.
* [Empiece](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) con un dominio creado previamente.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Creación de una aplicación en LUIS

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Create new app** (Crear aplicación).

    ![Lista de aplicaciones de LUIS](./media/luis-create-new-app/apps-list.png)


2. En el cuadro de diálogo asigne el nombre "TravelAgent" a la aplicación.

    ![Cuadro de diálogo para crear una aplicación](./media/luis-create-new-app/create-app.png)

3. Elija la referencia cultural de la aplicación (para la aplicación TravelAgent, seleccione Inglés) y, después, haga clic en **Done** (Listo). 

    > [!NOTE]
    > La referencia cultural no se puede cambiar una vez creada la aplicación. 

## <a name="import-an-app-from-file"></a>Importar una aplicación desde archivo

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Import new app** (Importar aplicación nueva).
1. En el cuadro de diálogo emergente, seleccione un archivo de aplicación válido de JSON y, a continuación, seleccione **realiza**.

### <a name="import-errors"></a>Errores de importación

Errores posibles son: 

* Una aplicación con ese nombre ya existe. Volver a importar la aplicación y establezca el **nombre opcional** a un nuevo nombre. 

## <a name="export-app-for-backup"></a>Exportar aplicación de copia de seguridad

1. En **mis aplicaciones** página, seleccione **exportar**.
1. Seleccione **exportar como JSON**. El explorador descarga la versión activa de la aplicación.
1. Agregue este archivo en el sistema de copia de seguridad para archivar el modelo.

## <a name="export-app-for-containers"></a>Exportar Apps for containers

1. En **mis aplicaciones** página, seleccione **exportar**.
1. Seleccione **exportar como contenedor** , a continuación, seleccione qué ranura publicada (producción o fase) que desea exportar.
1. Use este archivo con su [contenedor LUIS](luis-container-howto.md). 

    Si está interesado en exportar un modelo pero no aún modelo publicado para usar con el contenedor de LUIS, vaya a la **versiones** página y exportar desde allí. 

## <a name="delete-app"></a>Eliminar la aplicación

1. En la página **My Apps** (Mis aplicaciones), haga clic en los tres puntos (...) situados al final de la fila de la aplicación.
1. Seleccione **Delete** (Eliminar) en el menú.
1. En la ventana de confirmación, haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

La primera tarea en la aplicación es [agregar intenciones](luis-how-to-add-intents.md).

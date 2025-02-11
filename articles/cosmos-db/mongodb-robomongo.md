---
title: Uso de Robo 3T para conectarse a Azure Cosmos DB
description: Obtenga información sobre cómo conectarse a Azure Cosmos DB mediante Robo 3T y la API de Azure Cosmos DB para MongoDB
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
origin.date: 12/26/2018
ms.date: 03/18/2019
author: rockboyfor
ms.author: v-yeche
ms.openlocfilehash: ab066fef4bffe35408fdc5f3ad6d66796b4d0818
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372562"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>Uso de Robo 3T con la API de Azure Cosmos DB para MongoDB

Para conectar a una cuenta de Cosmos con Robo 3T, debe:

* Descargar e instalar [Robo 3T](https://robomongo.org/)
* Tener información de la [cadena de conexión](connect-mongodb-account.md) de Cosmos DB

## <a name="connect-using-robo-3t"></a>Conectar con Robo 3T
Para agregar la cuenta de Cosmos DB al administrador de conexiones de Robo 3T, siga estos pasos:

1. Recupere la información de conexión de su cuenta de Cosmos configurada con la API MongoDB de Azure Cosmos DB siguiendo las instrucciones indicadas [aquí](connect-mongodb-account.md).

    ![Captura de pantalla de la hoja de la cadena de conexión](./media/mongodb-robomongo/connectionstringblade.png)
2. Ejecute *Robomongo.exe*.

3. Haga clic en el botón de conexión, que está bajo **File** (Archivo), para administrar las conexiones. Después, haga clic en la opción **Create** (Create) de la ventana **MongoDB Connections**; se abrirá **Connection Settings** (Configuración de conexiones).

4. En la ventana **Connection Settings** (Configuración de conexiones), elija un nombre. Después, busque el **host** y **puerto** en la información de conexión del paso 1 y escríbalos en **Address** (Dirección) y **Port** (Puerto) respectivamente.

    ![Captura de pantalla de la Robomongo administrar conexiones](./media/mongodb-robomongo/manageconnections.png)
5. En la pestaña **Authentication** (Autenticación), haga clic en **Perform authentication** (Realizar autenticación). Después, escriba la base de datos (el valor predeterminado es *Admin*), el **nombre de usuario** y la **contraseña**.
Tanto el **nombre de usuario** como la **contraseña** están en la información de conexión del paso 1.

    ![Captura de pantalla de la ficha de autenticación de Robomongo](./media/mongodb-robomongo/authentication.png)
6. En la pestaña **SSL**, marque **Use SSL protocol** (Usar protocolo SSL) y, luego, cambie el **método de autenticación** a **Self-signed Certificate** (Certificado autofirmado).

    ![Captura de pantalla de la pestaña SSL de Robomongo](./media/mongodb-robomongo/SSL.png)
7. Por último, haga clic en **Test** (Probar) para comprobar que puede conectarse. Después, seleccione **Save** (Guardar).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [usar Studio 3T](mongodb-mongochef.md) con la API de Azure Cosmos DB para MongoDB.
- Explore [ejemplos](mongodb-samples.md) de MongoDB con la API de Azure Cosmos DB para MongoDB.

<!-- Update_Description: update meta properties, wording update -->
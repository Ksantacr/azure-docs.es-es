---
title: 'Implementación de una aplicación web con una plantilla: Azure Cosmos DB'
description: Aprenda a implementar una cuenta de Azure Cosmos DB, Azure App Service Web Apps y una aplicación web de ejemplo mediante una plantilla de Azure Resource Manager.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: 93cdea453050df8899abf9233991715ae237bcd4
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257238"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Implementar Azure Cosmos DB y Azure App Service Web Apps con una plantilla de Azure Resource Manager
En este tutorial se muestra cómo usar una plantilla de Azure Resource Manager para implementar e integrar [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), una aplicación web de [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) y una aplicación web de ejemplo.

Con las plantillas de Azure Resource Manager, puede automatizar fácilmente la implementación y la configuración de sus recursos de Azure.  En este tutorial se muestra cómo implementar una aplicación web y configurar automáticamente la información de conexión de la cuenta de Azure Cosmos DB.

Después de completar este tutorial, podrá responder a las siguientes preguntas:  

* ¿Cómo se puede usar una plantilla de Azure Resource Manager para implementar e integrar una cuenta de Azure Cosmos DB y una aplicación web en Azure App Service?
* ¿Cómo se puede usar una plantilla de Azure Resource Manager para implementar e integrar una cuenta de Azure Cosmos DB, una aplicación web de App Service Web Apps y una aplicación Webdeploy?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos
> [!TIP]
> Aunque en este tutorial no se exige experiencia con plantillas de Azure Resource Manager o JSON, si quiere modificar las plantillas o las opciones de implementación a las que se hace referencia, entonces sí es necesario tener conocimientos sobre cada una de estas áreas.
> 
> 

Antes de seguir las instrucciones de este tutorial, asegúrese de contar con una suscripción a Azure. Azure es una plataforma basada en suscripción.  Para más información sobre cómo obtener una suscripción, consulte [Opciones de compra](https://azure.microsoft.com/pricing/purchase-options/), [Ofertas para miembros](https://azure.microsoft.com/pricing/member-offers/) o [Evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Paso 1: Descarga de los archivos de plantilla
Para comenzar, vamos a descargar los archivos de plantilla que se necesitan en este tutorial.

1. Descargue la plantilla [Create a DocumentDB account, Web Apps, and deploy a demo application sample](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) (Crear una cuenta de Azure Cosmos DB, Web Apps e implementar un ejemplo de aplicación demo) en una carpeta local (por ejemplo, C:\Azure Cosmos DBTemplates). Con esta plantilla se implementa una cuenta de Azure Cosmos DB, una aplicación web de App Service y una aplicación web.  También se configura automáticamente la aplicación web para conectar con la cuenta de Azure Cosmos DB.
2. Descargue la plantilla [Create an Azure Cosmos DB account and Web Apps sample](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) (Crear una cuenta de Azure Cosmos DB y un ejemplo de Web Apps) en una carpeta local (por ejemplo, C:\Azure Cosmos DBTemplates). Con esta plantilla se implementa una cuenta de Azure Cosmos DB y una aplicación web de App Service y se modifica la configuración de la aplicación del sitio para exponer fácilmente información de conexión de Azure Cosmos DB. No se incluye ninguna aplicación web.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Paso 2: Implementación de la cuenta de Azure Cosmos DB, la aplicación web de App Service y el ejemplo de aplicación demo
Ahora vamos a implementar su primera plantilla.

> [!TIP]
> La plantilla no valida que el nombre de la aplicación web y el nombre de la cuenta de Azure Cosmos DB especificados en la plantilla siguiente a) sean válidos y b) estén disponibles.  Es muy recomendable comprobar la disponibilidad de los nombres que planea suministrar antes de enviar la implementación.
> 
> 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com), haga clic en Nuevo y busque "Implementación de plantillas".
    ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment1.png)
2. Seleccione el elemento de Template Deployment y haga clic en **Crear** ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment2.png)
3. Haga clic en **Editar plantilla**, pegue el contenido del archivo de plantilla DocDBWebsiteTodo.json y haga clic en **Guardar**.
   ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment3.png)
4. Haga clic en **Editar parámetros**, proporcione valores para cada uno de los parámetros obligatorios y haga clic en **Aceptar**.  Los parámetros son los siguientes:
   
   1. SITENAME: especifica el nombre de la aplicación web de App Service y se usa para crear la dirección URL que se emplea para el acceso a la aplicación web (por ejemplo, si especifica "mydemodocdbwebsite", la dirección URL mediante la que tiene acceso a la aplicación web es mydemodocdbwebsite.azurewebsites.net).
   2. HOSTINGPLANNAME: especifica el nombre del plan de hospedaje de App Service que se creará.
   3. LOCATION: especifica la ubicación de Azure en la que se crearán los recursos de Azure Cosmos DB y de la aplicación web.
   4. DATABASEACCOUNTNAME: especifica el nombre de la cuenta de Azure Cosmos DB que se creará.   
      
      ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment4.png)
5. Elija un grupo de recursos existente o proporcione un nombre para crear uno nuevo y elija una ubicación para él.

    ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment5.png)
6. Haga clic en **Revisar los términos legales**, **Compra** y luego haga clic en **Crear** para comenzar la implementación.  Seleccione **Anclar al panel** de modo que la implementación resultante se pueda ver fácilmente en la página principal del Portal de Azure.
   ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment6.png)
7. Cuando finalice la implementación, se abre el panel Grupo de recursos.
   ![Captura de pantalla del panel Grupo de recursos](./media/create-website/TemplateDeployment7.png)  
8. Para usar la aplicación, vaya a la dirección URL de la aplicación web (en el ejemplo anterior, la dirección URL sería http://mydemodocdbwebapp.azurewebsites.net).  Verá la siguiente aplicación web:
   
   ![Aplicación de tareas pendientes de ejemplo](./media/create-website/image2.png)
9. Siga adelante y cree un par de tareas en la aplicación web, luego vuelva al panel Grupo de recursos en Azure Portal. Haga clic en el recurso de la cuenta de Azure Cosmos DB en la lista Recursos y, luego, en **Explorador de datos**.
10. Ejecute la consulta predeterminada, "SELECT * FROM c" e inspeccione los resultados.  Observe que la consulta ha recuperado la representación JSON de los elementos todo creados en el paso 7 anterior.  No dude en experimentar con las consultas. Por ejemplo, pruebe a ejecutar SELECT * FROM c WHERE c.isComplete = true para devolver todos los elementos marcados como completos.
11. No dude en explorar la experiencia del portal de Azure Cosmos DB o modificar la aplicación Todo de ejemplo.  Cuando esté preparado, implementaremos otra plantilla.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>Paso 3: Implementación de la cuenta de documentos y del ejemplo de aplicación web
Ahora implementaremos su segunda plantilla.  Esta plantilla resulta útil para mostrar cómo insertar información de conexión de Azure Cosmos DB (por ejemplo, el punto de conexión y la clave maestra) en una aplicación web como configuración de la aplicación o como cadena de conexión personalizada. Por ejemplo, si tiene su propia aplicación web, quizás prefiera realizar la implementación con una cuenta de Azure Cosmos DB y que la información de conexión se rellene automáticamente durante dicha operación.

> [!TIP]
> La plantilla no valida si el nombre de la aplicación web y el nombre de la cuenta de Azure Cosmos DB especificados a continuación sean a) válidos y b) estén disponibles.  Es muy recomendable comprobar la disponibilidad de los nombres que planea suministrar antes de enviar la implementación.
> 
> 

1. En el [Azure Portal](https://portal.azure.com), haga clic en Nuevo y busque "Implementación de plantillas".
    ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment1.png)
2. Seleccione el elemento de Template Deployment y haga clic en **Crear** ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment2.png)
3. Haga clic en **Editar plantilla**, pegue el contenido del archivo de plantilla DocDBWebSite.json y haga clic en **Guardar**.
   ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment3.png)
4. Haga clic en **Editar parámetros**, proporcione valores para cada uno de los parámetros obligatorios y haga clic en **Aceptar**.  Los parámetros son los siguientes:
   
   1. SITENAME: especifica el nombre de la aplicación web de App Service y se usa para crear la dirección URL que se emplea para el acceso a la aplicación web (por ejemplo, si especifica "mydemodocdbwebsite", la dirección URL mediante la que tiene acceso a la aplicación web es mydemodocdbwebsite.azurewebsites.net).
   2. HOSTINGPLANNAME: especifica el nombre del plan de hospedaje de App Service que se creará.
   3. LOCATION: especifica la ubicación de Azure en la que se crearán los recursos de Azure Cosmos DB y de la aplicación web.
   4. DATABASEACCOUNTNAME: especifica el nombre de la cuenta de Azure Cosmos DB que se creará.   
      
      ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment4.png)
5. Elija un grupo de recursos existente o proporcione un nombre para crear uno nuevo y elija una ubicación para él.

    ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment5.png)
6. Haga clic en **Revisar los términos legales**, **Compra** y luego haga clic en **Crear** para comenzar la implementación.  Seleccione **Anclar al panel** de modo que la implementación resultante se pueda ver fácilmente en la página principal del Portal de Azure.
   ![Captura de pantalla de la interfaz de usuario de implementación de plantillas](./media/create-website/TemplateDeployment6.png)
7. Cuando finalice la implementación, se abre el panel Grupo de recursos.
   ![Captura de pantalla del panel Grupo de recursos](./media/create-website/TemplateDeployment7.png)  
8. Haga clic en el recurso Web App en la lista Recursos y luego haga clic en **Configuración de la aplicación**![Captura de pantalla del grupo de recursos](./media/create-website/TemplateDeployment9.png)  
9. Observe que existen valores de configuración de la aplicación para el punto de conexión de Azure Cosmos DB y para cada una de las claves maestras de Azure Cosmos DB.

    ![Captura de pantalla de configuración de la aplicación](./media/create-website/TemplateDeployment10.png)  
10. No dude en continuar explorando Azure Portal o siga uno de nuestros [ejemplos](https://go.microsoft.com/fwlink/?LinkID=402386) de Azure Cosmos DB para crear su propia aplicación de Azure Cosmos DB.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Pasos siguientes
Felicidades. Ha implementado Azure Cosmos DB, una aplicación web de App Service y una aplicación web de ejemplo mediante plantillas de Azure Resource Manager.

* Para más información sobre Azure Cosmos DB, haga clic [aquí](https://azure.microsoft.com/services/cosmos-db/).
* Para obtener más información acerca de aplicaciones de servicio web de la aplicación de Azure, haga clic en [aquí](https://go.microsoft.com/fwlink/?LinkId=325362).
* Para obtener más información sobre las plantillas del Administrador de recursos de Azure, haga clic [aquí](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Lo que ha cambiado
* Si desea ver una guía para el cambio de Websites a App Service, consulte: [Azure App Service y su impacto en los servicios de Azure existentes](https://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Si desea empezar a trabajar con Azure App Service antes de inscribirse para abrir una cuenta de Azure, vaya a [Prueba de App Service](https://go.microsoft.com/fwlink/?LinkId=523751), donde podrá crear inmediatamente una aplicación web de inicio de corta duración en App Service. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.
> 
> 


---
title: 'Ejemplos y escenarios comunes: Azure Logic Apps | Microsoft Docs'
description: Ejemplos, escenarios comunes, tutoriales y guías detalladas de Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.date: 01/31/2018
ms.openlocfilehash: 89e0294db3178cedd3b14aada0b505787b17c75e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303696"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Escenarios comunes, ejemplos, tutoriales y guías detalladas de Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) le ayuda a organizar e integrar servicios diferentes, ya que proporciona [más de 100 conectores que se pueden usar en cualquier momento](../connectors/apis-list.md), que van de SQL Server o SAP locales a Microsoft Cognitive Services. El servicio Logic Apps es "sin servidor", por lo que no tiene que preocuparse de escalados o instancias. Lo único que debe hacer es definir el flujo de trabajo con un desencadenador y las acciones que realiza el flujo de trabajo. La plataforma subyacente controla la escala, la disponibilidad y el rendimiento. Logic Apps es especialmente útil para aquellos casos de uso y escenarios en los que hay que coordinar varias acciones en varios sistemas.

En este artículo se muestran ejemplos y escenarios comunes para que obtenga más información sobre los distintos patrones y funcionalidades que [Azure Logic Apps](../logic-apps/logic-apps-overview.md) admite.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Puntos de partida más usados en flujos de trabajo de aplicaciones lógicas

Todas las aplicaciones lógica comienzan con un [*desencadenador*](../logic-apps/logic-apps-overview.md#logic-app-concepts), y solo un desencadenador, que inicia el flujo de trabajo de la aplicación lógica y pasa los datos como parte de dicho desencadenador. Algunos de los conectores proporcionan desencadenadores, que pueden ser de estos tipos:

* *Desencadenadores de sondeo*: Comprueba periódicamente un punto de conexión de servicio para los nuevos datos. Cuando los haya, el desencadenador crea y ejecuta una nueva instancia de flujo de trabajo con dichos datos como entrada.

* *Desencadenadores de inserción*: Realiza escuchas para los datos en un punto de conexión de servicio y espera hasta que se produce un evento específico. En ese momento, el desencadenador se activa inmediatamente y crear y ejecutar una nueva instancia de flujo de trabajo que utiliza como entrada todos los datos disponibles.

Estos son algunos ejemplos de los desencadenadores más usados:

* Sondeo: 

  * El [desencadenador **Programación - Periodicidad**](../connectors/connectors-native-recurrence.md) permite establecer no solo la fecha y hora de inicio sino también la periodicidad con que se activa la aplicación lógica. 
  Por ejemplo, puede seleccionar los días de la semana y las horas del día en que se activará la aplicación lógica.

  * El desencadenador "Cuando llega un correo electrónico nuevo" permite que la aplicación lógica compruebe si hay mensajes de correo electrónico nuevos en todos los proveedores de correo electrónico compatibles con Logic Apps, por ejemplo, [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/), etc.

  * El [desencadenador **HTTP**](../connectors/connectors-native-http.md) permite que la aplicación lógica compruebe un punto de conexión de servicio especificado, para lo que se comunica a través de HTTP.
  
* Inserción:

  * El [desencadenador **Solicitud/Respuesta - Solicitud**](../connectors/connectors-native-reqres.md) permite a la aplicación lógica recibir solicitudes HTTP y responder en tiempo real a los eventos de alguna manera.

  * El [desencadenador **Webhook de HTTP**](../connectors/connectors-native-webhook.md) realiza la suscripción a un punto de conexión de servicio mediante el registro de una *dirección URL de devolución de llamada* en dicho servicio. 
  De este modo, el servicio puede simplemente notificar el desencadenador cuando se produce el evento especificado, con el fin de que no sea preciso que este sondee el servicio.

Tras recibir una notificación acerca de datos nuevos o un evento, el desencadenador se activa, crea una nueva instancia de flujo de trabajo de aplicación lógica y ejecuta las acciones en el flujo de trabajo. Para acceder a los datos del desencadenador se usa el flujo de trabajo. Por ejemplo, el desencadenador "Cuando se publica un tweet nuevo" pasa el contenido del tweet a la aplicación lógica que se ejecute. 

## <a name="respond-to-triggers-and-extend-actions"></a>Respuesta a desencadenadores y extensión de acciones

En el caso de los sistemas y servicios que podrían no tener conectores publicados, también puede extender las aplicaciones lógicas.

* [Creación de desencadenadores o acciones personalizadas](../logic-apps/logic-apps-create-api-app.md)
* [Configuración de acciones de ejecución prolongada para ejecuciones de flujo de trabajo](../logic-apps/logic-apps-create-api-app.md)
* [Respuesta a eventos y acciones externos con webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Llamar, desencadenar o anidar flujos de trabajo con respuestas sincrónicas a las solicitudes HTTP](../logic-apps/logic-apps-http-endpoint.md)
* [Tutorial: Cree un panel social con tecnología de AI en minutos con Logic Apps y Power BI](https://aka.ms/logicappsdemo)
* [Vídeo: Responder a webhooks de SMS de Twilio y enviar una respuesta de texto](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Funcionalidades de flujo de control, registro y control de errores

Las aplicaciones lógicas incluyen amplias funciones de flujo de control avanzado, como condiciones, modificadores, bucles y ámbitos. Para garantizar que las soluciones sean resistentes, también puede implementar el control de errores y excepciones en los flujos de trabajo. Para los registros de notificación y diagnóstico del estado de ejecución del flujo de trabajo, Azure Logic Apps también ofrece supervisión y alertas.

* Realización de distintas acciones según las [instrucciones condicionales](../logic-apps/logic-apps-control-flow-conditional-statement.md) y las [instrucciones switch](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Repeat steps or process items in arrays and collections with loops](../logic-apps/logic-apps-control-flow-loops.md) (Repetición de pasos o procesamiento de elementos en matrices y colecciones con bucles)
* [Group actions together with scopes](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) (Agrupación de acciones con ámbitos)
* [Creación de control de errores y excepciones en un flujo de trabajo](../logic-apps/logic-apps-exception-handling.md)
* [Caso de uso: Cómo una compañía sanitaria usa lógica aplicación control de excepciones para los flujos de trabajo de HL7 FHIR](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Supervisar el estado, configurar el registro de diagnósticos y activar alertas para Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Turn on monitoring and diagnostics logging when creating logic apps](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md) (Activación de la supervisión y el registro de diagnóstico al crear aplicaciones lógicas)

## <a name="deploy-and-manage-logic-apps"></a>Implementación y administración de aplicaciones lógicas

Puede desarrollar e implementar aplicaciones lógicas completamente con Visual Studio, Azure DevOps o cualquier otra herramienta de compilación automatizada o de control de código fuente. Con el fin de admitir la implementación para flujos de trabajos y conexiones dependientes en una plantilla de recursos, las aplicaciones lógicas usan las plantillas de implementación de recursos de Azure. Visual Studio Tools genera automáticamente estas plantillas, lo que puede comprobar en el control de código fuente para el control de versiones.

* [Creación e implementación de aplicaciones lógicas con Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Supervisar el estado, configurar el registro de diagnósticos y activar alertas para Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Creación de una plantilla de implementación automatizada](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Tipos de contenido, conversiones y transformaciones dentro de una ejecución

Puede obtener acceso a distintos tipos de contenido, convertirlos y transformarlos mediante las diversas funciones que existen en el [lenguaje de definición de flujo de trabajo de](https://aka.ms/logicappsdocs) Azure Logic Apps. Por ejemplo, puede convertir entre una cadena, JSON y XML con las expresiones de flujo de trabajo `@json()` y `@xml()`. El motor de Logic Apps conserva los tipos de contenido para admitir la transferencia de contenido sin pérdida de información entre los servicios.

* [Funcionamiento de las expresiones de flujo de trabajo en las aplicaciones lógicas](../logic-apps/logic-apps-author-definitions.md)
* [Administración de los tipos de contenido no JSON](../logic-apps/logic-apps-content-type.md), como `application/xml`, `application/octet-stream` y `multipart/formdata`
* [Esquema del lenguaje de definición de flujo de trabajo - Azure Logic Apps](https://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Otras integraciones y funcionalidades

Las aplicaciones lógicas también ofrecen integración con muchos servicios, como Azure Functions, Azure API Management, Azure App Services y puntos de conexión HTTP personalizados, por ejemplo, REST y SOAP.

* [Creación de un panel de redes sociales en tiempo real y sin servidor con Azure](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Llamada a Azure Functions desde aplicaciones lógicas](../logic-apps/logic-apps-azure-functions.md)
* [Tutorial: Desencadenamiento de aplicaciones lógicas con Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Tutorial: Supervisar los cambios de la máquina virtual con Azure Event Grid y Logic Apps](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Tutorial: Crear una función que se integra con Azure Logic Apps y Microsoft Cognitive Services para analizar opiniones de Twitter post](../azure-functions/functions-twitter-email.md)
* [Tutorial: Supervisión remota de IoT y notificaciones con Azure Logic Apps conectando IoT hub y el buzón de correo](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Llamar a extremos SOAP desde aplicaciones lógicas](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Escenarios de un extremo a otro

* [Notas del producto: Integración de administración de casos-to-end con servicios de Azure, como Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Testimonios de clientes

Sepa cómo Azure Logic Apps, junto con otros servicios de Azure y productos de Microsoft, ayudó a que [estas empresas](https://aka.ms/logic-apps-customer-stories) mejoraran su agilidad y se centraran en sus negocios principales mediante la simplificación, organización, automatización y orquestación de procesos complejos.

## <a name="next-steps"></a>Pasos siguientes

* [Compilación de definiciones de aplicación lógica con JSON](../logic-apps/logic-apps-author-definitions.md)
* [Control de errores y excepciones en aplicaciones lógicas](../logic-apps/logic-apps-exception-handling.md)
* [Envío de comentarios, preguntas o sugerencias para mejorar Azure Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)

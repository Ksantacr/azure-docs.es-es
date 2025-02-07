---
title: Mensajería asincrónica de Service Bus | Microsoft Docs
description: Descripción de la mensajería asincrónica de Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 50778ae742c1ec66857a6c2fa6250dc3d67e5601
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531117"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Patrones de mensajería asincrónica y alta disponibilidad

La mensajería asincrónica se puede implementar de varias maneras. Con colas, temas y suscripciones, Azure Service Bus admite la asincronía a través de un mecanismo de almacenamiento y reenvío. En un funcionamiento normal (sincrónico), se envían mensajes a colas y temas, y se reciben mensajes de colas y suscripciones. Las aplicaciones que escriba dependen de que estas entidades estén siempre disponibles. Cuando cambia el estado de la entidad, debido a varias circunstancias, necesita una manera de proporcionar una entidad de capacidad reducida que pueda satisfacer la mayoría de las necesidades.

Las aplicaciones suelen usar patrones de mensajería asincrónica para habilitar varios escenarios de comunicación. Puede compilar aplicaciones en las que los clientes puedan enviar mensajes a servicios, incluso cuando el servicio no esté en ejecución. En las aplicaciones que sufren ráfagas de comunicaciones, una cola puede ayudar a nivelar la carga, ya que proporciona un lugar para almacenar las comunicaciones en el búfer. Por último, puede obtener un equilibrador de carga simple, pero eficaz, para distribuir los mensajes entre varias máquinas.

Para mantener la disponibilidad de todas estas entidades, considere las distintas maneras en que estas entidades pueden aparecer no disponibles para un sistema de mensajería duradero. Por lo general, vemos que la entidad deja de estar disponible para las aplicaciones que escribimos de las siguientes maneras:

* No es posible enviar mensajes.
* No es posible recibir mensajes.
* No es posible administrar entidades (crear, recuperar, actualizar o eliminar entidades).
* No es posible ponerse en contacto con el servicio.

Para cada uno de estos errores, existen modos de error diferentes que permiten que las aplicaciones sigan en funcionamiento con cierto nivel de capacidad reducida. Por ejemplo, un sistema que puede enviar mensajes, pero no recibirlos, puede recibir pedidos de clientes pero no procesarlos. En este tema se tratan los posibles problemas que se pueden producir y cómo se mitigan. Service Bus ha introducido varias mitigaciones en las que debe participar y en este tema también se describen las reglas que rigen el uso de ellas.

## <a name="reliability-in-service-bus"></a>Confiabilidad en Service Bus
Hay varias maneras de tratar los problemas de los mensajes y de las entidades, y hay directrices que rigen el uso adecuado de dichos procedimientos para mitigar problemas. Para comprender las directrices, primero es preciso saber qué es lo que puede generar errores en Service Bus. Dado el diseño de los sistemas de Azure, todos estos problemas tienden a durar poco tiempo. A alto nivel, las diferentes causas de la no disponibilidad aparecerán como sigue:

* Limitación desde un sistema externo del que depende Service Bus. Se produce una limitación de las interacciones con los recursos de proceso y almacenamiento.
* Problema en un sistema del que depende Service Bus. Por ejemplo, pueden aparecer problemas en una parte determinada del almacenamiento.
* Error de Service Bus en un subsistema individual. En esta situación, un nodo de proceso puede entrar en un estado incoherente y debe reiniciarse, provocando que todas las entidades a las que sirve equilibren su carga con otros nodos. A su vez, esto puede provocar que los mensajes se procesen más lentamente durante un breve periodo.
* Error de Service Bus en un centro de datos de Azure. Se trata de un error grave y es imposible acceder el sistema durante varios minutos, o incluso horas.

> [!NOTE]
> El término **almacenamiento** puede significar Azure Storage y SQL Azure.
> 
> 

Service Bus contiene varios procedimientos para mitigar estos problemas. En las secciones siguientes se tratan todos estos problema y los respectivos procedimientos para mitigarlos.

### <a name="throttling"></a>Limitaciones
Con Service Bus, la limitación permite la administración cooperativa de la velocidad de los mensajes. Cada nodo individual de Service Bus alberga muchas entidades. Cada una de esas entidades realiza peticiones al sistema en cuanto a CPU, memoria, almacenamiento, etc. Si cualquiera de estas facetas detecta un uso que supera los umbrales definidos, Service Bus puede denegar una solicitud determinada. El llamador recibe una excepción [ServerBusyException][ServerBusyException] y lo reintenta transcurridos 10 segundos.

Como mitigación, el código debe leer el error y detener todos los reintentos del mensaje durante al menos 10 segundos. Puesto que el error puede producirse en varias partes de la aplicación de cliente, se espera que cada una de ellas ejecute la lógica de reintento de forma independiente. El código puede reducir la posibilidad de que la habilitación de particiones en una cola o tema pueda suponer una limitación.

### <a name="issue-for-an-azure-dependency"></a>Problema en una dependencia de Azure
Otros componentes de Azure en ocasiones pueden tener problemas de servicio ocasionalmente. Por ejemplo, cuando se actualiza un sistema que usa Service Bus, dicho sistema puede experimentar temporalmente la reducción de sus funcionalidades. Para solucionar estos tipos de problemas, Service Bus investiga periódicamente e implementa las mitigaciones. Aparecen los efectos secundarios de estas mitigaciones. Por ejemplo, para controlar problemas transitorios de almacenamiento, Service Bus implementa un sistema que permite que las operaciones de envío funcionen de forma coherente. Dada la propia naturaleza de la mitigación, un mensaje enviado puede tardar hasta 15 minutos en aparecer en la cola o suscripción afectadas, y estar listo para una operación de recepción. Por lo general, la mayoría de las entidades no sufrirán este problema. Sin embargo, dado el número de entidades de Service Bus en Azure, a veces esta mitigación es necesaria para un pequeño subconjunto de los clientes de Service Bus.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Error de Service Bus en un subsistema individual
Con cualquier aplicación, las circunstancias pueden provocar que algún componente interno de Service Bus pueda volverse incoherente. Cuando Service Bus lo detecta, recopila datos de la aplicación para ayudar a diagnosticar lo que sucedió. Una vez que se hayan recopilado los datos, la aplicación se reinicia para intentar que vuelva a un estado coherente. Este proceso ocurre con bastante rapidez y su resultado es que una entidad parezca que no está disponible durante unos minutos, aunque los tiempos de inactividad son mucho menores.

En estos casos, la aplicación cliente genera una excepción [System.TimeoutException][System.TimeoutException] o [MessagingException][MessagingException]. Service Bus contiene una mitigación para este problema en forma de lógica de reintento de cliente automatizado. Si se agotó el período de reintento y no se entregó el mensaje, puede probar con otras de las funciones que se mencionan en el artículo sobre la [administración de desastres e interrupciones][handling outages and disasters].

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido los conceptos básicos de la mensajería asincrónica de Service Bus, puede consultar más información sobre la [administración de desastres e interrupciones][handling outages and disasters].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN
[handling outages and disasters]: service-bus-outages-disasters.md

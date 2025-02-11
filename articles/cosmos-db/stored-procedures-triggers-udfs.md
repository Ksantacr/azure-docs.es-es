---
title: Uso de procedimientos almacenados, desencadenadores y funciones definidas por el usuario en Azure Cosmos DB
description: En este artículo se introducen conceptos como procedimientos almacenados, desencadenadores y funciones definidas por el usuario en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 40d120fe5fcc79721923d3493e74b5195ecc129c
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65965701"
---
# <a name="stored-procedures-triggers-and-user-defined-functions"></a>Procedimientos almacenados, desencadenadores y funciones definidas por el usuario

Azure Cosmos DB proporciona la ejecución transaccional integrada en lenguaje JavaScript. Cuando se usa la API de SQL en Azure Cosmos DB, puede escribir los **procedimientos almacenados**, los **desencadenadores** y las **funciones definidas por el usuario (UDF)** en el lenguaje JavaScript. Puede escribir su lógica en JavaScript, que se ejecuta dentro del motor de base de datos. Puede crear y ejecutar desencadenadores, procedimientos almacenados y UDF mediante [Azure Portal](https://portal.azure.com/), la [API de consulta integrada del lenguaje JavaScript en Azure Cosmos DB](javascript-query-api.md) o los [SDK del cliente de la API de SQL de Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md).

## <a name="benefits-of-using-server-side-programming"></a>Ventajas del uso de la programación en el lado servidor

La escritura de procedimientos almacenados, desencadenadores y funciones definidas por el usuario (UDF) en JavaScript le permite crear aplicaciones completas y tienen las siguientes ventajas:

* **Lógica de procedimientos:** JavaScript como un lenguaje de programación de alto nivel que proporciona una interfaz completa y familiar para expresar la lógica empresarial. Puede realizar una secuencia de operaciones complejas en los datos.

* **Transacciones atómicas:** Cosmos DB garantiza que las operaciones de base de datos realizadas dentro de un único procedimiento almacenado o desencadenador sean atómicas. Esta funcionalidad atómica permite a una aplicación combinar operaciones relacionadas en un único lote para que todas se realicen correctamente o no lo haga ninguna.

* **Rendimiento:** Los datos de JSON se asignan intrínsecamente al sistema de tipos de lenguaje JavaScript. Esta asignación permite diversas optimizaciones, como la materialización diferida de documentos JSON en el grupo de búferes, y las pone a disposición a petición para el código en ejecución. Hay otras ventajas de rendimiento asociadas con el envío de la lógica empresarial a la base de datos, que incluyen:

   * *Procesamiento por lotes:* los desarrolladores pueden agrupar operaciones como inserciones y enviarlas en masa. Los costes de la latencia de tráfico de red y la sobrecarga de almacenamiento para crear transacciones independientes se reducen de forma significativa.

   * *Precompilación:* Los procedimientos almacenados, los desencadenadores y las UDF se precompilan implícitamente en formato de código byte para evitar los costos de compilación en el momento de cada invocación de script. Debido a la precompilación, la invocación de procedimientos almacenados es rápida y tiene una superficie baja.

   * *Secuenciación:* A veces, las operaciones necesitan un mecanismo de desencadenamiento que puede realizar una o varias actualizaciones en los datos. Además de la atomicidad, también existen ventajas de rendimiento cuando se ejecuta en el servidor.

* **Encapsulación:** los procedimientos almacenados se pueden utilizar para agrupar la lógica en un lugar. La encapsulación agrega una capa de abstracción en la parte superior de los datos sin procesar, lo cual le permite desarrollar sus aplicaciones de forma independiente de los datos. Esta capa de abstracción es útil cuando los datos no tienen esquema y no tiene que administrar la adición de lógica adicional directamente en la aplicación. Esta abstracción le permite mantener seguros sus datos simplificando el acceso desde los scripts.

> [!TIP]
> Los procedimientos almacenados son más adecuados para las operaciones con mucha actividad de escritura. Al decidir dónde se deben utilizar procedimientos almacenados, se optimiza mediante la encapsulación de la cantidad máxima de posibles operaciones de escritura. Por lo general, los procedimientos almacenados no son el medio más eficaz para realizar una gran cantidad de operaciones de lectura, por lo que su uso para procesar por lotes un gran número de lecturas para devolver al cliente no dará como resultado el beneficio deseado.

## <a name="transactions"></a>Transacciones

Una transacción en una base de datos típica se puede definir como una secuencia de operaciones realizadas como una única unidad lógica de trabajo. Cada transacción proporciona **garantías de propiedad ACID**. ACID es un acrónimo conocido que hacer referencia a: **A**tomicidad, **C**oherencia, a**I**slamiento y **D**urabilidad. 

* La atomicidad garantiza que todas las operaciones realizadas dentro de una transacción se lean como una única unidad donde se confirman todas o ninguna. 

* La coherencia asegura que los datos se encuentran siempre en un estado válido en todas las transacciones. 

* El aislamiento garantiza que dos transacciones no pueden interferir entre ellas; muchos sistemas comerciales proporcionan varios niveles de aislamiento que se pueden utilizar según las necesidades de aplicación. 

* La durabilidad asegura que cualquier cambio que esté confirmado en la base de datos estará siempre presente.

En Azure Cosmos DB, el entorno en tiempo de ejecución de JavaScript está hospedado dentro del motor de base de datos. Por lo tanto, las solicitudes realizadas dentro de los procedimientos almacenados y desencadenadores se ejecutan en el mismo ámbito que la sesión de base de datos. Esta característica permite a Azure Cosmos DB garantizar propiedades ACID para todas las operaciones que formen parte de un procedimiento almacenado o desencadenador. Para obtener ejemplos, vea el artículo [Cómo implementar transacciones](how-to-write-stored-procedures-triggers-udfs.md#transactions).

### <a name="scope-of-a-transaction"></a>Ámbito de una transacción

Si un procedimiento almacenado está asociado con un contenedor de Azure Cosmos, dicho procedimiento se ejecuta en el ámbito de transacción de una clave de partición lógica. Cada ejecución de procedimientos almacenados debe incluir un valor de clave de partición lógica que se corresponda con el ámbito de la transacción. Para más información, consulte el artículo [Creación de particiones de Azure Cosmos DB](partition-data.md).

### <a name="commit-and-rollback"></a>Confirmación y reversión

Las transacciones están integradas de forma nativa en el modelo de programación de JavaScript de Azure Cosmos DB. Dentro de una función de JavaScript, todas las operaciones se ajustan automáticamente en una única transacción. Si la lógica de JavaScript en un procedimiento almacenado se completa sin excepciones, todas las operaciones dentro de la transacción se confirman en la base de datos. Instrucciones como `BEGIN TRANSACTION` y `COMMIT TRANSACTION` (familiarizadas con bases de datos relacionales) están implícitas en Azure Cosmos DB. Si existe alguna excepción desde el script, el entorno en tiempo de ejecución de JavaScript de Azure Cosmos DB revertirá toda la transacción. Por lo tanto, el inicio de una excepción es equivalente a una instrucción `ROLLBACK TRANSACTION` en Azure Cosmos DB.

### <a name="data-consistency"></a>Coherencia de los datos

Los procedimientos almacenados y los desencadenadores se ejecutan siempre en la réplica principal del contenedor de Azure Cosmos. Esta característica garantiza que las lecturas desde los procedimientos almacenados ofrezcan una [fuerte coherencia](consistency-levels-tradeoffs.md). Las consultas que utilizan funciones definidas por el usuario se pueden ejecutar en el servidor principal o en cualquier réplica secundaria. Los procedimientos almacenados y los desencadenadores están diseñados para admitir escrituras de transacción; sin embargo, la lógica de solo lectura se implementa mejor como lógica del lado de la aplicación y las consultas con los [SDK de la API de SQL de Azure Cosmos DB](sql-api-dotnet-samples.md) le ayudarán a saturar el rendimiento de la base de datos. 

## <a name="bounded-execution"></a>Ejecución vinculada

Todas las operaciones de Azure Cosmos DB se deben completar dentro de la duración del tiempo de espera especificado. Esta restricción se aplica a las funciones de JavaScript: procedimientos almacenados, desencadenadores y funciones definidas por el usuario. Si una operación no se completa dentro de ese límite de tiempo, la transacción se revierte.

Puede asegurarse de que las funciones de JavaScript finalicen dentro del límite de tiempo o implementar un modelo basado en la continuación en el lote o reanudar la ejecución. Con el fin de simplificar el desarrollo de los procedimientos almacenados y los desencadenadores para controlar los límites de tiempo, todas las funciones del contenedor de Azure Cosmos (por ejemplo, la creación, lectura, actualización y eliminación de elementos) devuelven un valor booleano que representa si se completará la operación. Si este valor es false, es una indicación de que el procedimiento debe concluir la ejecución porque el script consume más tiempo o rendimiento aprovisionado que el valor configurado. Se garantiza la finalización de las operaciones en cola anteriores a la primera operación de almacenamiento no aceptada si el procedimiento almacenado se completa a tiempo y no pone en cola más solicitudes. Por lo tanto, las operaciones deben ponerse en cola una a una mediante la convención de devolución de llamada de JavaScript para administrar el flujo de control del script. Dado que los scripts se ejecutan en un entorno de servidor, se rigen estrictamente. Los scripts que infrinjan repetidamente los límites de ejecución puede que se marquen como inactivos y que no se puedan ejecutar, por lo que deben volver a crearse para respetar los dichos límites.

Las funciones de JavaScript también están sujetas a la [capacidad de rendimiento aprovisionado](request-units.md). Las funciones de JavaScript podrían acabar utilizando un gran número de unidades de solicitud en poco tiempo y pueden tener limitación de frecuencia si se alcanza el límite de capacidad de rendimiento aprovisionado. Es importante tener en cuenta que los scripts consumen un rendimiento adicional, además del rendimiento dedicado a ejecutar operaciones de bases de datos, aunque estas operaciones son ligeramente menos costosas que la ejecución de las misma desde el cliente.

## <a name="triggers"></a>Desencadenadores

En esta sección se describen los dos tipos de desencadenadores:

### <a name="pre-triggers"></a>Desencadenadores previos

Azure Cosmos DB proporciona desencadenadores que se pueden invocar mediante la realización de una operación en un elemento de Azure Cosmos DB. Por ejemplo, puede especificar un desencadenador previo al crear un elemento. En este caso, el desencadenador previo se ejecutará antes de la creación del elemento. Los desencadenadores previos no pueden tener parámetros de entrada. Si es necesario, el objeto de la solicitud se puede usar para actualizar el cuerpo del documento de la solicitud original. Cuando se registran los desencadenadores, los usuarios pueden especificar las operaciones que se pueden ejecutar con ellos. Si un desencadenador se creó con `TriggerOperation.Create`, significa que no se podrá usar en una operación de reemplazo. Para obtener ejemplos, vea el artículo [Escritura de desencadenadores](how-to-write-stored-procedures-triggers-udfs.md#triggers).

### <a name="post-triggers"></a>Desencadenadores posteriores

De forma similar a los desencadenadores previos, los desencadenadores posteriores también están asociados a una operación en un elemento de Azure Cosmos DB y no requieren ningún parámetro de entrada. Se ejecutan *después* de que se haya completado la operación y tienen acceso al mensaje de respuesta que se envía al cliente. Para obtener ejemplos, vea el artículo [Escritura de desencadenadores](how-to-write-stored-procedures-triggers-udfs.md#triggers).

## <a id="udfs"></a>Funciones definidas por el usuario

Las funciones definidas por el usuario (UDF) se utilizan para ampliar la sintaxis del lenguaje de consultas de la API de SQL e implementar fácilmente la lógica empresarial personalizada. Solo se pueden llamar dentro de las consultas. Las UDF no tienen acceso al objeto de contexto y se supone que se deben utilizar como un JavaScript únicamente de cálculo. Por lo tanto, se pueden ejecutar en réplicas secundarias. Para obtener ejemplos, vea el artículo [Escritura de funciones definidas por el usuario](how-to-write-stored-procedures-triggers-udfs.md#udfs).

## <a id="jsqueryapi"></a>Language-integrated query API de JavaScript

Además de emitir consultas mediante la sintaxis de consulta de la API de SQL, el [SDK del lado servidor](https://azure.github.io/azure-cosmosdb-js-server) permite realizar consultas a través de una interfaz de JavaScript sin necesitar conocimientos de SQL. La Query API de JavaScript le permite compilar consultas mediante programación pasando las funciones de predicado en la secuencia de las llamadas de función. Las consultas se analizan con el entorno en tiempo de ejecución de JavaScript y se ejecutan eficazmente dentro de Azure Cosmos DB. Para más información sobre la compatibilidad con la Query API de JavaScript, consulte el artículo [Working with JavaScript Language Integrated Query API in Azure Cosmos DB](javascript-query-api.md) (Trabajo con Language Integrated Query API de JavaScript). Para obtener ejemplos, consulte el artículo [Escritura de procedimientos almacenados y desencadenadores en Azure Cosmos DB con la API de consulta de JavaScript](how-to-write-javascript-query-api.md).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a escribir y usar procedimientos almacenados, desencadenadores y funciones definidas por el usuario en Azure Cosmos DB con los siguientes artículos:

* [Escritura de procedimientos almacenados, desencadenadores y funciones definidas por el usuario](how-to-write-stored-procedures-triggers-udfs.md)

* [Uso de procedimientos almacenados, desencadenadores y funciones definidas por el usuario](how-to-use-stored-procedures-triggers-udfs.md)

* [Working with JavaScript language integrated query API](javascript-query-api.md) (Trabajo con Language Integrated Query API de JavaScript)

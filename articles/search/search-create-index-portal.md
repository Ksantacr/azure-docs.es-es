---
title: 'Creación de un índice de Azure Search en Azure Portal: Azure Search'
description: Obtenga información sobre cómo crear un índice de Azure Search con diseñadores de índices integrados en el portal.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6a2bac71c37cc750eb24e3492ecdcdf0b2333cce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817324"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>Creación de un índice de Azure Search en el portal

Azure Search incluye un diseñador de índices integrado en el portal que resulta útil para prototipos o la creación de un [índice de búsqueda](search-what-is-an-index.md) hospedado en el servicio Azure Search. La herramienta se usa para la construcción del esquema. Cuando se guarda la definición, un índice vacío se expresa completamente en Azure Search. Cómo lo cargue con los datos para la búsqueda es decisión suya.

El diseñador de índices es solo un enfoque para la creación de un índice. Mediante programación, cree un índice usando las API [.NET](search-create-index-dotnet.md) o [REST](search-create-index-rest-api.md).

## <a name="start-index-designer"></a>Inicio del diseñador de índices

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra el panel de servicio. Puede hacer clic en **Todos los servicios** en la barra de acceso rápido para buscar los "servicios de búsqueda" existentes en la suscripción actual. 

2. Haga clic en el vínculo **Agregar índice** de la barra de comandos de la parte superior de la página.

   ![Vínculo Agregar índice en la barra de comandos](media/search-create-index-portal/add-index.png "Add index link in the command bar")

3. Asigne un nombre a su índice de Azure Search. Se hace referencia a los nombres de índice en las operaciones de indexación y consulta. El nombre del índice se convierte en parte de la dirección URL del punto de conexión que se usa en las conexiones al índice y para enviar solicitudes HTTP en la API de REST de Azure Search.

   * Empiece con una letra.
   * Use únicamente letras minúsculas, números o guiones ("-").
   * Limite el nombre a 60 caracteres.

## <a name="add-fields"></a>Adición de campos

La composición del índice incluye una *colección de campos* que define los datos del índice que se pueden buscar. En general, la colección de campos especifica la estructura de documentos que se carga por separado. La colección de campos incluye campos obligatorios y opcionales, con nombre y tipo, con atributos de índice para determinar cómo se puede usar el campo.

1. Agregue campos para especificar completamente los documentos que va a cargar, definiendo un [tipo de datos](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) para cada uno. Por ejemplo, si los documentos constan de *id. de hotel*, *nombre de hotel*, *dirección*, *ciudad* y *región*, cree un campo correspondiente para cada uno de ellos en el índice. Revise la [guía de diseño de la siguiente sección](#design) para obtener ayuda para configurar atributos.

2. Especifique un campo de *clave* de tipo Edm.String. Los valores de este campo deben identificar de forma exclusiva cada documento. De forma predeterminada, el campo se denomina *id*, pero se puede cambiar de nombre siempre que la cadena se ajuste a las [reglas de nomenclatura](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Por ejemplo, si la colección de campos incluye *id. de hotel*, elegiría ese valor para la clave. Hay un campo de clave obligatorio para cada índice de Azure Search y debe ser una cadena.

3. Defina atributos en cada campo. El diseñador de índices excluye todos los atributos que no son válidos para el tipo de datos, pero no sugiere qué incluir. Revise las instrucciones de la sección siguiente para comprender para qué son los atributos.

    La documentación de la API de Azure Search incluye ejemplos de código con un sencillo índice de *hoteles*. En la captura de pantalla siguiente, puede ver la definición del índice, incluido el analizador de idioma francés especificado durante la definición del índice, que puede volver a crear como ejercicio práctico en el portal.

    ![Índice de demostración de hoteles](media/search-create-index-portal/field-definitions.png "Hotels demo index")

4. Cuando termine, haga clic en **Crear** para guardar y crear el índice.

<a name="design"></a>

## <a name="set-attributes"></a>Definir atributos

Aunque puede agregar nuevos campos en cualquier momento, las definiciones de campo existentes se bloquean durante la vigencia del índice. Por este motivo, los desarrolladores suelen usar el portal para crear índices sencillos, probar ideas o emplear las páginas del portal para buscar una configuración. La iteración frecuente de un diseño de índice es más eficaz si se sigue un enfoque basado en código para poder volver a crear el índice fácilmente.

Los analizadores y los proveedores de sugerencias se asocian a los campos antes de guardar el índice. Asegúrese de agregar analizadores de lenguajes o proveedores de sugerencias a la definición del índice mientras lo crea.

Los campos de cadena se suelen marcar como **Buscable** y **Recuperable**. Los campos que se usan para restringir los resultados de búsqueda incluyen **Ordenable**, **Filtrable** y **Clasificable**.

Los atributos de campo determinan cómo se usa un campo, por ejemplo, si se usa en la búsqueda de texto completo, la navegación por facetas, las operaciones de ordenación, etc. En la tabla siguiente se describe cada atributo.

|Atributo|DESCRIPCIÓN|  
|---------------|-----------------|  
|**buscable**|Permite realizar búsquedas de texto completo, sujetas a análisis léxico, como la separación de palabras durante la indexación. Si establece un campo buscable en un valor como "día soleado", internamente se dividirá en los tokens individuales "soleado" y "día". Para obtener detalles, vea [Búsqueda de texto completo](search-lucene-query-architecture.md).|  
|**filtrable**|Se hace referencia en consultas **$filter**. Los campos filtrables de tipo `Edm.String` o `Collection(Edm.String)` no sufren separación de palabras, por lo que las comparaciones son solo de coincidencias exactas. Por ejemplo, si establece un campo de este tipo en "día soleado", `$filter=f eq 'sunny'` no encontrará ninguna coincidencia, pero `$filter=f eq 'sunny day'` sí. |  
|**ordenable**|De forma predeterminada, el sistema ordena los resultados por puntuación, pero puede configurar la ordenación en función de los campos de los documentos. Los campos de tipo `Collection(Edm.String)` no pueden ser **ordenables**. |  
|**clasificable**|Normalmente se usa en una presentación de resultados de búsqueda que incluye un recuento de visitas por categoría (por ejemplo, hoteles de una ciudad concreta). Esta opción no puede utilizarse con campos de tipo `Edm.GeographyPoint`. Los campos de tipo `Edm.String` que son **filtrables**, **ordenables** o **clasificables** pueden tener como máximo 32 kilobytes de longitud. Para obtener detalles, vea [Creación de un índice de Búsqueda de Azure con la API de REST](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**key**|Identificador único de los documentos del índice. Es necesario elegir exactamente un campo como campo de clave, y debe ser de tipo `Edm.String`.|  
|**recuperable**|Determina si el campo se puede devolver en un resultado de búsqueda. Esto resulta útil cuando se quiere usar un campo (por ejemplo, *margen de beneficio*) como mecanismo de filtrado, ordenación o puntuación, pero no se quiere que sea visible para el usuario final. Este atributo debe ser `true` for `key` .|  

## <a name="next-steps"></a>Pasos siguientes

Después de crear un índice de Azure Search, puede ir al siguiente paso: [cargar datos buscables en el índice](search-what-is-data-import.md).

También puede [profundizar en los índices](search-what-is-an-index.md). Además de la colección de campos, un índice también especifica analizadores, proveedores de sugerencias, perfiles de puntuación y configuración de CORS. El portal proporciona pestañas para definir los elementos más comunes: campos, analizadores y proveedores de sugerencias. Para crear o modificar otros elementos, puede usar la API de REST o el SDK de .NET.

## <a name="see-also"></a>Vea también

 [Cómo funciona la búsqueda de texto completo](search-lucene-query-architecture.md)  
 [API de REST del servicio Search](https://docs.microsoft.com/rest/api/searchservice/)[SDK de .NET](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)


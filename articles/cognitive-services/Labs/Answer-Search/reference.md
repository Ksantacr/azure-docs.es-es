---
title: Referencia de Project Answer Search
titlesuffix: Azure Cognitive Services
description: Referencia del punto de conexión de Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: reference
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 09fab691ea04ad98472abc4f4dee5ecb4d22e660
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60721018"
---
# <a name="project-answer-search-v7-reference"></a>Referencia de Project Answer Search versión 7

Bing Answer Search API toma un parámetro de consulta y devuelve `searchResponse` con `answerType`: `facts` o `entities`. 

Las aplicaciones que utilizan Answer Search API envían solicitudes al punto de conexión con una dirección URL para la versión preliminar en un parámetro de consulta.  La solicitud debe incluir el parámetro `q=searchTerm` y el encabezado *Ocp-Apim-Subscription-Key*.   

Se pueden analizar los datos y las entidades de la respuesta JSON que contienen detalles acerca del objeto de la búsqueda.

## <a name="endpoint"></a>Punto de conexión
Para solicitar los resultados de Answer Search, envíe una solicitud al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

Solicitud GET del punto de conexión: 
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

```

La solicitud debe utilizar el protocolo HTTPS e incluir el parámetro de consulta siguiente:
-  `q=<URL>` -La consulta que identifica el objeto de búsqueda

Para ver ejemplos en los que se muestra cómo realizar solicitudes, consulte [Inicio rápido de C#](c-sharp-quickstart.md) o [Inicio rápido de Java](java-quickstart.md). 

En las secciones siguientes se proporcionan detalles técnicos acerca de los objetos de respuesta, los parámetros de consulta y los encabezados que afectan a los resultados de la búsqueda. 
  
Para obtener información acerca de los encabezados que las solicitudes deben incluir, consulte [Encabezados](#headers).  
  
Para obtener información acerca de los parámetros de consulta que las solicitudes deben incluir, consulte [Parámetros de consulta](#query-parameters).  
  
Para obtener información acerca de los objetos JSON que incluye la respuesta, consulte [Objetos de respuesta](#response-objects).

La longitud máxima de la dirección URL de consulta es de 2048 caracteres. Para asegurarse de que la longitud de la dirección URL no supera el límite, la longitud máxima de los parámetros de consulta debe ser inferior a 1500 caracteres. Si la dirección URL supera los 2048 caracteres, el servidor devuelve 404 No encontrado.  

Para obtener información sobre el uso permitido y la visualización de los resultados, consulte [Requisitos de uso y visualización](use-display-requirements.md). 

> [!NOTE]
> Algunos encabezados de solicitud que son significativos para otras API de búsqueda no afectan a URL Preview.
> - Pragma: el autor de la llamada no controla si URL Preview utiliza la memoria caché.
> - Cache-Control: el autor de la llamada no controla si URL Preview utiliza la memoria caché.
> - User-Agent
> 
> Además, algunos parámetros no son actualmente significativos para URL Preview API, pero pueden utilizarse en el futuro para la globalización mejorada. 
 
## <a name="headers"></a>encabezados  
A continuación se indican los encabezados que una solicitud y una respuesta pueden incluir.  
  
|Encabezado|DESCRIPCIÓN|  
|------------|-----------------|  
|Accept|Encabezado de solicitud opcional.<br /><br /> El tipo de soporte predeterminado es application/json. Para especificar que la respuesta use [JSON-LD](https://json-ld.org/), establezca el encabezado Accept como application/ld + json.|  
|<a name="acceptlanguage" />Accept-Language|Encabezado de solicitud opcional.<br /><br /> Lista delimitada por comas de los idiomas que se usarán en las cadenas de la interfaz de usuario. La lista se muestra en orden de preferencia decreciente. Para más información, incluido el formato esperado, consulte [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este encabezado y el parámetro de consulta [setLang](#setlang) son mutuamente excluyentes, no especifique ambos.<br /><br /> Si establece este encabezado, debe especificar también el parámetro de consulta cc. Para determinar el mercado para el que hay que devolver resultados, Bing usa el primer idioma compatible que encuentra en la lista y lo combina con el valor del parámetro `cc`. Si la lista no incluye ningún idioma compatible, Bing busca el idioma y el mercado más cercanos que admiten la solicitud, o bien usa un mercado agregado o predeterminado para los resultados. Para determinar el mercado que usó Bing, consulte el encabezado BingAPIs-Market.<br /><br /> Use este encabezado y el parámetro de consulta `cc` solo si especifica varios idiomas. En caso contrario, use los parámetros de consulta [mkt](#mkt) y [setLang](#setlang).<br /><br /> Una cadena de interfaz de usuario es aquella que se usa como etiqueta en una interfaz de usuario. Hay pocas cadenas de interfaz de usuario en los objetos de respuesta JSON. Todos los vínculos a las propiedades de Bing.com en los objetos de respuesta aplican el idioma especificado.|  
|<a name="market" />BingAPIs-Market|Encabezado de respuesta.<br /><br /> Mercado que usa la solicitud. El formato es \<CódigoIdioma\>-\<CódigoPaís\>. Por ejemplo, en-US.|  
|<a name="traceid" />BingAPIs-TraceId|Encabezado de respuesta.<br /><br /> Identificador de la entrada de registro que contiene los detalles de la solicitud. Si se produce un error, capture este identificador. Si no puede determinar y resolver el problema, incluya este identificador junto con el resto de información que proporcione al equipo de soporte técnico.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio.<br /><br /> Clave de suscripción que recibió al suscribirse a este servicio en [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Encabezado de solicitud opcional<br /><br /> De forma predeterminada, Bing devuelve contenido almacenado en caché, si está disponible. Para que Bing no devuelva contenido almacenado en caché, establezca el encabezado Pragma en no-cache (por ejemplo, Pragma: no-cache).
|<a name="useragent" />User-Agent|Encabezado de solicitud opcional.<br /><br /> El agente de usuario que origina la solicitud. Bing usa el agente de usuario para proporcionar a los usuarios de dispositivos móviles una experiencia optimizada. Aunque es opcional, se recomienda especificar siempre este encabezado.<br /><br /> El agente de usuario debe ser la misma cadena que envían los exploradores que más se usan. Para obtener información acerca de los agentes de usuario, consulte [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> A continuación encontrará varios ejemplos de cadenas user-agent.<br /><ul><li>Windows Phone: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android: Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone: Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC: Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad: Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Encabezado de solicitud y respuesta opcional.<br /><br /> Bing usa este encabezado para proporcionar a los usuarios un comportamiento coherente en las llamadas API de Bing. Bing incorpora a menudo mejoras y características piloto nuevas, y usa el identificador de cliente como clave para asignar el tráfico en distintos paquetes piloto. Si no usa el mismo identificador de cliente para un usuario en varias solicitudes, Bing puede asignar el usuario a varios paquetes piloto en conflicto. La asignación a varios paquetes piloto en conflicto puede generar una experiencia de usuario incoherente. Por ejemplo, si la segunda solicitud tiene una asignación de paquete piloto distinta de la primera, puede que se produzca una experiencia inesperada. Además, Bing puede usar el identificador de cliente para personalizar los resultados web en el historial de búsqueda del identificador de cliente, lo que proporciona una experiencia enriquecida para el usuario.<br /><br /> Bing también utiliza este encabezado para ayudar a mejorar las clasificaciones de resultados mediante el análisis de la actividad que genera un identificador de cliente. Las mejoras de relevancia ayudan con una mayor calidad de los resultados que devuelven las API de Bing y, a su vez, permiten un mayor porcentaje de clics para el consumidor de la API.<br /><br /> **IMPORTANTE:** Aunque es opcional, considere este encabezado como obligatorio. Conservar el identificador de cliente en varias solicitudes para la misma combinación de usuario final y dispositivo permite 1) que el consumidor de API reciba una experiencia de usuario coherente y 2) un mayor porcentaje de clics a través de una mejor calidad de los resultados de las API de Bing.<br /><br /> Estas son las reglas de uso básico que se aplican a este encabezado.<br /><ul><li>Cada usuario que utiliza la aplicación en el dispositivo debe tener un identificador de cliente único generado por Bing.<br /><br/>Si no incluye este encabezado en la solicitud, Bing genera un identificador y lo devuelve en el encabezado de respuesta X-MSEdge-ClientID. La única ocasión en que NO debe incluir este encabezado en una solicitud es la primera vez que el usuario usa la aplicación en el dispositivo.<br /><br/></li><li>Use el identificador de cliente para cada solicitud de API de Bing que haga la aplicación para este usuario en el dispositivo.<br /><br/></li><li>**ATENCIÓN:** debe asegurarse de que este identificador de cliente no se pueda vincular a la información de cualquier cuenta de usuario autenticable.</li><br/><li>Conserve el identificador de cliente. Para conservar el identificador en una aplicación de explorador, use una cookie HTTP persistente para asegurarse de que se use ese identificador en todas las sesiones. No use una cookie de sesión. Para otras aplicaciones, como aplicaciones móviles, use el almacenamiento persistente del dispositivo para conservar el identificador.<br /><br/>La próxima vez que el usuario use la aplicación en ese dispositivo, obtenga el identificador de cliente que conservó.</li></ul><br /> **NOTA:** Las respuestas de Bing pueden incluir este encabezado o no incluirlo. Si la respuesta incluye este encabezado, capture el identificador de cliente y úselo para todas las solicitudes posteriores de Bing del usuario en dicho dispositivo.<br /><br /> **NOTA:** Si incluye X-MSEdge-ClientID, no debe incluir cookies en la solicitud.|  
|<a name="clientip" />X-MSEdge-ClientIP|Encabezado de solicitud opcional.<br /><br /> Dirección IPv4 o IPv6 del dispositivo cliente. La dirección IP se usa para detectar la ubicación del usuario. Bing usa la información de ubicación para determinar el comportamiento de la búsqueda segura.<br /><br /> **NOTA:** Aunque es opcional, se recomienda especificar siempre tanto este encabezado como el encabezado X-Search-Location.<br /><br /> No ofusque la dirección (por ejemplo, cambiando el último octeto a 0). Al ofuscar los resultados de dirección, la ubicación no se encuentra cerca de la ubicación real, lo que puede provocar que Bing devuelva resultados erróneos.|  
|<a name="location" />X-Search-Location|Encabezado de solicitud opcional.<br /><br /> Una lista delimitada por punto y coma de pares clave-valor que describen la ubicación geográfica del cliente. Bing usa la información de ubicación para determinar el comportamiento de la búsqueda segura y devolver contenido local apropiado. Especifique el par clave-valor como \<clave\>:\<valor\>. A continuación se incluyen las claves que usan para especificar la ubicación del usuario.<br /><br /><ul><li>lat: la latitud de la ubicación del cliente, en grados. La latitud debe ser mayor o igual que -90.0, y menor o igual que +90.0. Los valores negativos indican latitudes del sur, mientras que los valores positivos indican latitudes del norte.<br /><br /></li><li>long: la longitud de la ubicación del cliente, en grados. La longitud debe ser mayor o igual que -180.0, y menor o igual que +180.0. Los valores negativos indican longitudes del oeste, mientras que los valores positivos indican longitudes del este.<br /><br /></li><li>re: el radio, en metros, que especifica la precisión horizontal de las coordenadas. Pase el valor que devuelve el servicio de ubicación del dispositivo. Los valores típicos pueden ser 22 m para GPS/Wi-Fi, 380 m para la triangulación de antenas repetidoras de telefonía móvil y 18 000 m para la búsqueda inversa de IP.<br /><br /></li><li>ts: la marca de tiempo UTC UNIX del momento en que el cliente estaba en la ubicación (la marca de tiempo UNIX es el número de segundos transcurridos desde el 1 de enero de 1970).<br /><br /></li><li>head: opcional. El encabezado relativo del cliente o la dirección de desplazamiento. Especifique la dirección de desplazamiento como grados de 0 a 360, en la dirección de las agujas del reloj con respecto al norte verdadero. Especifique esta clave solo si la clave `sp` es distinta de cero.<br /><br /></li><li>sp: la velocidad horizontal, en metros por segundo, a la que se desplaza el dispositivo de cliente.<br /><br /></li><li>alt: la altitud del dispositivo de cliente, en metros.<br /><br /></li><li>are: opcional. El radio, en metros, que especifica la precisión vertical de las coordenadas. El radio predeterminado son 50 kilómetros. Especifique esta clave solo si especificó la clave `alt`.<br /><br /></li></ul> **NOTA:** Aunque muchas de las claves son opcionales, cuanta más información aporte, más precisos serán los resultados de la ubicación.<br /><br /> **NOTA:** Es aconsejable especificar siempre la ubicación geográfica del usuario. Proporcionar la ubicación es especialmente importante si la dirección IP del cliente no refleja con precisión la ubicación física del usuario (por ejemplo, si el cliente utiliza VPN). Para obtener resultados óptimos, debería incluir tanto este encabezado como X-MSEdge-ClientIP pero, como mínimo, debe incluir este.|

> [!NOTE] 
> Recuerde que los términos de uso exigen el cumplimiento de todas las leyes aplicables, incluido el uso de estos encabezados. Por ejemplo, en determinados lugares, como Europa, existen requisitos para obtener el consentimiento del usuario antes de colocar determinados dispositivos de seguimiento en los dispositivos de usuario.
  

## <a name="query-parameters"></a>Parámetros de consulta  
La solicitud puede incluir los siguientes parámetros de consulta. Consulte la columna Obligatorio para saber cuáles son los parámetros obligatorios. Debe codificar como dirección URL los parámetros de la consulta.  
  
  
|NOMBRE|Value|Type|Obligatorio|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|Mercado de dónde proceden los resultados. <br /><br />Para obtener una lista de los valores de mercado posibles, consulte Códigos de mercado.<br /><br /> **NOTA:** URL Preview API solo admite actualmente el mercado estadounidense y el idioma inglés.<br /><br />|string|Sí|  
|<a name="query" />q|Dirección URL para la vista previa.|string|Sí|  
|<a name="responseformat" />responseFormat|Tipo de medio que se usará para la respuesta. A continuación se indican los valores posibles que no distinguen entre mayúsculas y minúsculas.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> El valor predeterminado es JSON. Para obtener información acerca de los objetos JSON que contiene la respuesta, consulte [Objetos de respuesta](#response-objects).<br /><br />  Si especifica JsonLd, el cuerpo de respuesta incluye objetos JSON-LD que contienen los resultados de la búsqueda. Para obtener información acerca de JSON-LD, consulte [JSON-LD](https://json-ld.org/).|string|Sin |  
|<a name="safesearch" />safeSearch|Filtro que se usa para filtrar el contenido para adultos. Estos son los posibles valores posibles de filtro (no distingue entre mayúsculas y minúsculas).<br /><ul><li>Desactivado: devuelve páginas web con texto, imágenes o vídeos para adultos.<br /><br/></li><li>Moderado: devuelve páginas web con texto para adultos, pero no imágenes o vídeos para adultos.<br /><br/></li><li>Estricto: no devuelve páginas web con texto, imágenes o vídeos para adultos.</li></ul><br /> El valor predeterminado es Moderado.<br /><br /> **NOTA:** Si la solicitud proviene de un mercado para el que la directiva para adultos de Bing requiere que `safeSearch` se defina como Estricto, Bing ignora el valor `safeSearch` y usa Estricto.<br/><br/>**NOTA:** Si usa el operador de consulta `site:`, existe la posibilidad de que la respuesta contenga contenido para adultos, independientemente del valor asignado al parámetro de consulta `safeSearch`. Use `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos. |string|Sin |  
|<a name="setlang" />setLang|Idioma que se usará para las cadenas de la interfaz de usuario. Especifique el idioma con el código ISO 639-1 de 2 letras. Por ejemplo, el código de idioma de inglés es EN. El valor predeterminado es EN (inglés).<br /><br /> Aunque sea opcional, siempre debe especificar el idioma. Normalmente, se establece `setLang` como el mismo idioma especificado por `mkt`, salvo que el usuario quiera que las cadenas de la interfaz de usuario se muestren en otro idioma.<br /><br /> Este parámetro y el encabezado [Accept-Language](#acceptlanguage) son mutuamente excluyentes: no especifique los dos.<br /><br /> Una cadena de interfaz de usuario es aquella que se usa como etiqueta en una interfaz de usuario. Hay pocas cadenas de interfaz de usuario en los objetos de respuesta JSON. Además, los vínculos a las propiedades de Bing.com de los objetos de respuesta aplican el idioma especificado.|string|Sin | 


## <a name="response-objects"></a>Objetos de respuesta  
El esquema de respuesta es un objeto [WebPage] o ErrorResponse, como se muestra en Web Search API. Si se produce un error en la solicitud, el objeto de nivel superior es el objeto [ErrorResponse](#errorresponse).


|Object|DESCRIPCIÓN|  
|------------|-----------------|  
|[WebPage]|Objeto JSON de nivel superior que contiene los atributos de la vista previa.|  
|[Fact]|Objeto JSON de nivel superior que contiene los datos.| 
|[Entities]|Objeto JSON de nivel superior que contiene datos de la entidad.| 

  
### <a name="error"></a>Error  
Define el error que se produjo.  
  
|Elemento|DESCRIPCIÓN|Type|  
|-------------|-----------------|----------|  
|<a name="error-code" />code|Código de error que identifica la categoría de error. Para obtener una lista de los códigos posibles, consulte [Códigos de error](#error-codes).|string|  
|<a name="error-message" />message|Descripción del error.|string|  
|<a name="error-moredetails" />moreDetails|Descripción que proporciona información adicional sobre el error.|string|  
|<a name="error-parameter" />parameter|Parámetro de consulta de la solicitud que causó el error.|string|  
|<a name="error-subcode" />subCode|Código de error que identifica el error. Por ejemplo, si `code` es InvalidRequest, `subCode` puede ser ParameterInvalid o ParameterInvalidValue. |string|  
|<a name="error-value" />value|Valor del parámetro de consulta que no era válido.|string|  
  

### <a name="errorresponse"></a>ErrorResponse  
Objeto de nivel superior que la respuesta incluye cuando se produce un error en la solicitud.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Sugerencia de tipo.|string|  
|<a name="errors" />errors|Lista de errores que describen los motivos que causaron un error en la solicitud.|[Error](#error)|  

  
  
### <a name="license"></a>Licencia  
Define la licencia bajo la que se pueden usar el texto o la foto.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|Nombre|Nombre de la licencia.|string|  
|url|La dirección URL a un sitio web en el que el usuario puede obtener más información acerca de la licencia.<br /><br /> Utilice el nombre y la dirección URL para crear un hipervínculo.|string|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Define una regla contractual para la atribución de licencia.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Una sugerencia de tipo, que se establece en LicenseAttribution.|string|  
|license|Licencia con la que puede utilizarse el contenido.|[License](#license)|  
|licenseNotice|Licencia que se muestra al lado del campo de destino. Por ejemplo, "Texto bajo licencia CC-BY-SA".<br /><br /> Use el nombre y la dirección URL de la licencia en el campo `license` para crear un hipervínculo al sitio web que describe los detalles de la licencia. Luego, reemplace el nombre de la licencia en la cadena `licenseNotice` (por ejemplo, CC-BY-SA) por el hipervínculo que acaba de crear.|string|  
|mustBeCloseToContent|Booleano que determina si el contenido de la regla debe colocarse cerca del campo al que se aplica la regla. Si el valor es **true**, los contenidos deben colocarse cerca unos de otros. Si el valor es **false**, o este campo no existe, el contenido se pueden colocar a discreción del autor de la llamada.|Boolean|  
|targetPropertyName|El nombre del campo al que se aplica la regla.|string|  
  

### <a name="link"></a>Vínculo  
Define los componentes de un hipervínculo.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Sugerencia de tipo.|string|  
|text|Texto que se muestra.|string|  
|url|Una dirección URL. Use la dirección URL y el texto que se muestra para crear un hipervínculo.|string|  
  

### <a name="linkattribution"></a>LinkAttribution  
Define una regla contractual para la atribución del vínculo.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Una sugerencia de tipo, que se establece en LinkAttribution.|string|  
|mustBeCloseToContent|Booleano que determina si el contenido de la regla debe colocarse cerca del campo al que se aplica la regla. Si el valor es **true**, los contenidos deben colocarse cerca unos de otros. Si el valor es **false**, o este campo no existe, el contenido se pueden colocar a discreción del autor de la llamada.|Boolean|  
|targetPropertyName|El nombre del campo al que se aplica la regla.<br /><br /> Si no se especifica un destino, la atribución se aplica a la entidad en conjunto y se debe mostrar inmediatamente después de la presentación de la entidad. Si hay varias reglas de atribución de texto y vínculos que no especifiquen un destino, debe concatenarlas y mostrarlas con una etiqueta "Datos de:". Por ejemplo, "Datos de < nombre1 proveedor\> &#124; < nombre2 proveedor\>".|string|  
|text|El texto de la atribución.|string|  
|url|La dirección URL al sitio web del proveedor. Use `text` y la dirección URL para crear el hipervínculo.|string|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Define una regla contractual para la atribución del soporte físico.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Una sugerencia de tipo, que se establece en MediaAttribution.|string|  
|mustBeCloseToContent|Booleano que determina si el contenido de la regla debe colocarse cerca del campo al que se aplica la regla. Si el valor es **true**, los contenidos deben colocarse cerca unos de otros. Si el valor es **false**, o este campo no existe, el contenido se pueden colocar a discreción del autor de la llamada.|Boolean|  
|targetPropertyName|El nombre del campo al que se aplica la regla.|string|  
|url|La dirección URL que se usa para crear un hipervínculo del contenido multimedia. Por ejemplo, si el destino es una imagen, usaría la dirección URL para que se pueda hacer clic en la imagen.|string|  
  
  
  
### <a name="organization"></a>Organización  
Define un anunciante.  
  
Tenga en cuenta que un anunciante puede proporcionar su nombre, su sitio web o ambos.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|Nombre|Nombre del publicador.|string|  
|url|La dirección URL al sitio web del anunciante.<br /><br /> Tenga en cuenta que es posible que el publicador no publique un sitio web.|string|  
  
  

### <a name="webpage"></a>WebPage  
Define la información acerca de una página web en la vista previa.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|
|Nombre|Título de la página (no necesariamente el título HTML).|string|
|url|Dirección URL que se rastreó realmente (la solicitud puede tener redireccionamientos con seguimiento)|string|  
|description|Breve descripción de la página y el contenido|string|  
|isFamilyFriendly|Más preciso para elementos del índice web; las capturas en tiempo real realizan esta detección basada únicamente en la dirección URL y no en el contenido de la página.|boolean|
|primaryImageOfPage/contentUrl|Dirección URL de una imagen representativa que se incluirá en la vista previa.|string| 
  
  
### <a name="querycontext"></a>QueryContext  
Define el contexto de la consulta que Bing ha usado para la solicitud.  
  
|Elemento|DESCRIPCIÓN|Type|  
|-------------|-----------------|----------|  
|adultIntent|Un valor booleano que indica si la consulta especificada está prevista para adultos. El valor es **true** si la consulta está prevista para adultos; de lo contrario, es **false**.|Boolean|  
|alterationOverrideQuery|La cadena de consulta que se usa para forzar a Bing a usar la cadena original. Por ejemplo, si la cadena de consulta es *navegar a sotavento*, la cadena de consulta de invalidación será *+ navegar a sotavento*. No olvide codificar la cadena de consulta que da lugar a *%2Bnavegar+a sotavento*.<br /><br /> Este campo solo se incluye si la cadena de consulta original contiene un error de ortografía.|string|  
|alteredQuery|La cadena de consulta que utiliza Bing para realizar la consulta. Bing usa la cadena modificada si la cadena de consulta original contenía errores de ortografía. Por ejemplo, si la cadena de consulta es `saling downwind`, la cadena de consulta modificada será `sailing downwind`.<br /><br /> Este campo solo se incluye si la cadena de consulta original contiene un error de ortografía.|string|  
|askUserForLocation|Un valor booleano que indica si Bing requiere la ubicación del usuario para proporcionar resultados precisos. Si ha especificado la ubicación del usuario mediante los encabezados [X-MSEdge-ClientIP](#clientip) y [X-Search-Location](#location), puede omitir este campo.<br /><br /> En consultas relacionadas con la ubicación, como "clima de hoy" o "restaurantes cerca de mí", que necesitan la ubicación del usuario para proporcionar resultados precisos, este campo se establece en **true**.<br /><br /> En consultas relacionadas con la ubicación que incluyan la ubicación (por ejemplo, "clima en Seattle"), este campo se establece en **false**. Este campo también se establece en **false** en las consultas no relacionadas con la ubicación, como los "más vendidos".|Boolean|  
|originalQuery|La cadena de consulta como se especifica en la solicitud.|string|  

### <a name="identifiable"></a>Identifiable

|NOMBRE|Value|Type|  
|-------------|-----------------|----------|
|id|Identificador de recursos.|string|
 
### <a name="rankinggroup"></a>RankingGroup
Define un grupo de resultados de la búsqueda grupo, como línea principal.

|NOMBRE|Value|Type|  
|-------------|-----------------|----------|
|items|Una lista de resultados de la búsqueda que se muestran en el grupo.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Define un elemento del resultado de la búsqueda que se muestra.

|NOMBRE|Value|Type|  
|-------------|-----------------|----------|
|resultIndex|Índice basado en cero del elemento en la respuesta que se muestra. Si el elemento no incluye este campo, muestre todos los elementos en la respuesta. Por ejemplo, muestre todos los artículos de noticias en la respuesta de News.|Entero|
|answerType|La respuesta que contiene el elemento que se muestra. Por ejemplo, News.<br /><br />Use el tipo para encontrar la respuesta en el objeto SearchResponse. El tipo es el nombre de un campo de SearchResponse.<br /><br /> Sin embargo, ese el tipo de respuesta solo si el objeto incluye el campo de valor; de lo contario, omítalo.|string|
|textualIndex|El índice de la respuesta en textualAnswers que se muestra.| Entero sin signo|
|value|El identificador que identifica una respuesta que se muestra o un elemento de una respuesta que se muestra. Si el identificador identifica una respuesta, muestre todos los elementos de la respuesta.|Identifiable|

### <a name="rankingresponse"></a>RankingResponse  
Define que lugar de la página de resultados de la búsqueda debe colocarse el contenido y en qué orden.  
  
|NOMBRE|Value|  
|----------|-----------|  
|<a name="ranking-mainline" />línea principal|Los resultados de la búsqueda se muestran en la línea principal.|  
|<a name="ranking-pole" />polo|Los resultados de la búsqueda a los que se debe proporcionar el tratamiento más visible (por ejemplo, se muestran encima de la línea principal y la barra lateral).|  
|<a name="ranking-sidebar" />barra lateral|los resultados de la búsqueda que se muestran en la barra lateral.| 


### <a name="searchresponse"></a>SearchResponse  
Define el objeto de nivel superior que la respuesta incluye cuando la solicitud es correcta.  
  
Tenga en cuenta que si el servicio sospecha de un ataque de denegación de servicio, la solicitud se realizará correctamente (código de estado HTTP 200 OK); sin embargo, el cuerpo de la respuesta estará vacío.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Sugerencia de tipo, que se establece en SearchResponse.|string|  
|WebPage|Un objeto JSON que define la versión preliminar|string|  
  
  
### <a name="textattribution"></a>TextAttribution  
Define una regla contractual para la atribución de texto sin formato.  
  
|NOMBRE|Value|Type|  
|----------|-----------|----------|  
|_type|Una sugerencia de tipo, que se establece en TextAttribution.|string|  
|text|El texto de la atribución.<br /><br /> La atribución de texto se aplica a la entidad en conjunto y se debe mostrar inmediatamente después de la presentación de la entidad. Si hay varias reglas de atribución de texto o vínculos que no especifiquen un destino, debe concatenarlas y mostrarlas con una etiqueta "Datos de:".|string| 


## <a name="error-codes"></a>Códigos de error

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud.  
  
|Código de estado|DESCRIPCIÓN|  
|-----------------|-----------------|  
|200|Correcta.|  
|400|Uno de los parámetros de consulta falta o no es válido.|  
|401|Falta la clave de suscripción o no es válida.|  
|403|El usuario está autenticado (por ejemplo, usó una clave de suscripción válida), pero no tiene permiso para el recurso solicitado.<br /><br /> Bing también puede devolver este estado si el autor de la llamada supera la cuota de consultas por mes.|  
|410|La solicitud usó HTTP en lugar del protocolo HTTPS. HTTPS es el único protocolo admitido.|  
|429|El autor de la llamada superó la cuota de consultas por segundo.|  
|500|Evento de servidor inesperado.|

Si se produce un error en la solicitud, la respuesta contiene un objeto [ErrorResponse](#errorresponse), que contiene una lista de objetos [Error](#error) que describen la causa del error. Si el error está relacionado con un parámetro, el campo `parameter` identifica el parámetro del problema. Si el error está relacionado con un valor de parámetro, el campo `value` identifica el valor que no es válido.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

A continuación se indican los valores de código de error y subcódigo de error posibles.

|Código|Subcódigo|DESCRIPCIÓN
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|El código de estado de HTTP es 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devuelve InvalidRequest siempre que alguna parte de la solicitud no es válida. Por ejemplo, falta un parámetro necesario o un valor de parámetro no es válido.<br/><br/>Si el error es ParameterMissing o ParameterInvalidValue, el código de estado HTTP es 400.<br/><br/>Si usa el protocolo HTTP en lugar de HTTPS, Bing devuelve HttpNotAllowed y el código de estado HTTP es 410.
|RateLimitExceeded|No hay subcódigos|Bing devuelve RateLimitExceeded cada vez que se supera la cuota de consultas por segundo (QPS) o de consultas por mes (QPM).<br/><br/>Si se supera la cuota QPS, Bing devuelve el código de estado HTTP 429, mientras que si se supera la cuota QPM, devuelve 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devuelve InvalidAuthorization cuando no puede autenticar el autor de la llamada. Por ejemplo, falta el encabezado `Ocp-Apim-Subscription-Key` o la clave de suscripción no es válida.<br/><br/>Se produce redundancia si especifica más de un método de autenticación.<br/><br/>Si el error es InvalidAuthorization, el código de estado HTTP es 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devuelve InsufficientAuthorization cuando el autor de la llamada no tiene permisos para acceder al recurso. Esto puede ocurrir si la clave de suscripción se ha deshabilitado o ha expirado. <br/><br/>Si el error es InsufficientAuthorization, el código de estado HTTP es 403.

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)


---
title: 'Inicio rápido: Búsqueda de entidades con el SDK de Bing Entity Search para Java'
titlesuffix: Azure Cognitive Services
description: Use este inicio rápido para buscar entidades con el SDK de Bing Entity Search para Java
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 15958033ef305a44f9c5254409fa111acd9d1eb5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813716"
---
# <a name="quickstart-send-a-search-request-with-the-bing-entity-search-sdk-for-java"></a>Inicio rápido: Envío de una solicitud de búsqueda el SDK de Bing Entity Search para Java

Use este inicio rápido para empezar a buscar entidades con el SDK de Bing Entity Search para Java. Aunque Bing Entity Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch).

## <a name="prerequisites"></a>Requisitos previos

* [Kit de desarrollo de Java (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* SDK de Bing Entity Search para Java

Instale las dependencias del SDK de Bing Entity Search con Maven, Gradle u otro sistema de administración de dependencias. El archivo POM de Maven requiere la declaración:

```xml
<dependency>
  <groupId>com.microsoft.azure.cognitiveservices</groupId>
  <artifactId>azure-cognitiveservices-entitysearch</artifactId>
  <version>1.0.2</version>
</dependency>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Creación e inicialización de un proyecto

1. Cree un proyecto de Java en su IDE o editor favorito e importe las bibliotecas siguientes.

    ```java
    import com.microsoft.azure.cognitiveservices.entitysearch.*;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.EntitySearchAPIImpl;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.SearchResponseInner;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List;
    ```

2. Creación de una variable para la clave de suscripción

    ```java
    String subscriptionKey = "your-key-here"
    ```

## <a name="create-a-search-client"></a>Creación de un cliente de búsqueda

1. Implemente el cliente `dominantEntityLookup`, lo que requiere un punto de conexión de API y una instancia de la clase `ServiceClientCredentials`.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Para implementar `ServiceClientCredentials`, siga estos pasos:

   1. Reemplace la función `applyCredentialsFilter()` por un objeto `OkHttpClient.Builder` como parámetro. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. Dentro de `applyCredentialsFilter()`, llame a `builder.addNetworkInterceptor()`. Cree un nuevo objeto `Interceptor` e invalide su método `intercept()` para tomar un objeto interceptor `Chain`.

       ```java
       //...
       builder.addNetworkInterceptor(
           new Interceptor() {
               @Override
               public Response intercept(Chain chain) throws IOException {
               //...    
               }
           });
       ///...
       ```

   3. Dentro de la función `intercept`, cree variables para su solicitud. Use `Request.Builder()` para compilar su solicitud. Agregue su clave de suscripción al encabezado `Ocp-Apim-Subscription-Key` y devuelva `chain.proceed()` en el objeto de solicitud.
            
       ```java
       //...
       public Response intercept(Chain chain) throws IOException {
           Request request = null;
           Request original = chain.request();
           Request.Builder requestBuilder = original.newBuilder()
                   .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
           request = requestBuilder.build();
           return chain.proceed(request);
       }
       //...
       ```
      ## <a name="send-a-request-and-receive-a-response"></a>Envío de una solicitud y recepción de la respuesta

1. Cree una nueva instancia del cliente de búsqueda con la clave de suscripción. Use `client.entities().search()` para enviar una solicitud de búsqueda para la consulta de búsqueda `satya nadella` y obtener una respuesta. 
    
    ```java
    EntitySearchAPIImpl client = getClient(subscriptionKey);
    SearchResponseInner entityData = client.entities().search(
            "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
    ```

1. Si se ha devuelto alguna entidad, convertirla en una lista. Recórralas e imprima la entidad dominante.

    ```java
    if (entityData.entities().value().size() > 0){
        // Find the entity that represents the dominant entity
        List<Thing> entrys = entityData.entities().value();
        Thing dominateEntry = null;
        for(Thing thing : entrys) {
            if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                System.out.println("\r\nSearched for \"Satya Nadella\" and found a dominant entity with this description:");
                System.out.println(thing.description());
                break;
            }
        }
    }
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilar una aplicación web de una sola página](../tutorial-bing-entities-search-single-page-app.md)

* [¿Qué es Bing Entity Search API?](../overview.md )

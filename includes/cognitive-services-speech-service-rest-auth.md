---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 03/29/2019
ms.author: erhopf
ms.openlocfilehash: 9cad860b8808dd2682995768c282d8376ab5d9be
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145365"
---
## <a name="authentication"></a>Authentication

Cada solicitud requiere un encabezado de autorización. Esta tabla muestra qué encabezados son compatibles con cada servicio:

| Encabezados de autorización compatibles | Conversión de voz en texto | Texto a voz |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | Sí | No |
| Autorización: Portador | Sí | Sí |

Cuando se usa el encabezado `Ocp-Apim-Subscription-Key`, solo se le pide que proporcione la clave de suscripción. Por ejemplo:

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Cuando se usa el encabezado `Authorization: Bearer`, se le pide que haga una solicitud al punto de conexión `issueToken`. En esta solicitud, va a intercambiar la clave de suscripción de un token de acceso que es válido durante 10 minutos. En las secciones siguientes aprenderá a obtener, usar y actualizar un token.


### <a name="how-to-get-an-access-token"></a>Obtención de un token de acceso

Para obtener un token de acceso, tiene que realizar una solicitud al punto de conexión `issueToken` mediante `Ocp-Apim-Subscription-Key` y su clave de suscripción.

Se admiten estas regiones y puntos de conexión:

[!INCLUDE [](./cognitive-services-speech-service-endpoints-token-service.md)]

Use estos ejemplos para crear la solicitud de token de acceso.

#### <a name="http-sample"></a>Ejemplo de HTTP

Este ejemplo es una solicitud HTTP para obtener un token. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Si la suscripción no está en la región Oeste de EE. UU., reemplace el encabezado `Host` por el nombre de host de la región.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

El cuerpo de la respuesta contiene el token de acceso en formato JSON Web Token (JWT).

#### <a name="powershell-sample"></a>Ejemplo de PowerShell

En este ejemplo se muestra un script sencillo de PowerShell para obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Asegúrese de usar el punto de conexión correcto para la región que coincida con su suscripción. En este ejemplo la región es Oeste de EE. UU.

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

#### <a name="curl-sample"></a>Ejemplo de cURL

cURL es una herramienta de la línea de comandos disponible en Linux (y en el subsistema Windows para Linux). Este comando cURL muestra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Asegúrese de usar el punto de conexión correcto para la región que coincida con su suscripción. En este ejemplo la región es Oeste de EE. UU.

```cli
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>Ejemplo de C#

Esta clase de C# muestra cómo obtener un token de acceso. Pase la clave de suscripción del servicio Voz al crear una instancia de la clase. Si su suscripción no está en la región Oeste de EE. UU., cambie el valor de `FetchTokenUri` para que coincida con la región de su suscripción.

```cs
public class Authentication
{
    public static readonly string FetchTokenUri =
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

#### <a name="python-sample"></a>Ejemplo de Python

```python
# Request module must be installed.
# Run pip install requests if necessary.
import requests

subscription_key = 'REPLACE_WITH_YOUR_KEY'

def get_token(subscription_key):
    fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
    headers = {
        'Ocp-Apim-Subscription-Key': subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    access_token = str(response.text)
    print(access_token)
```

### <a name="how-to-use-an-access-token"></a>Uso de un token de acceso

Se debe enviar el token de acceso al servicio como encabezado `Authorization: Bearer <TOKEN>`. Cada token de acceso tiene una validez de 10 minutos. Puede obtener un nuevo token en cualquier momento. No obstante, para reducir el tráfico de red y la latencia, se recomienda usar el mismo token durante nueve minutos.

Este es un ejemplo de solicitud HTTP a la API REST de texto a voz:

```http
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.stt.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

// Message body here...
```

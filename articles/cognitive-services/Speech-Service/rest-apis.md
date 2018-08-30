---
title: REST-APIs des Spracherkennungsdiensts
description: Referenz für die REST-APIs des Spracherkennungsdiensts.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: 64dce26303c0e700da54d371af5cb275b1613d70
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122102"
---
# <a name="speech-service-rest-apis"></a>REST-APIs des Spracherkennungsdiensts

Die REST-APIs des vereinheitlichten Spracherkennungsdiensts ähneln den APIs, die von der [Sprach-API](https://docs.microsoft.com/azure/cognitive-services/Speech) (ehemals als Bing-Spracherkennungsdienst bekannt) zur Verfügung gestellt werden. Die Endpunkte unterscheiden sich von den Endpunkten, die vom vorhergehenden Spracherkennungsdienst verwendet wurden.

## <a name="speech-to-text"></a>Spracherkennung

In der Spracherkennungs-API unterscheiden sich nur die verwendeten Endpunkte von der Spracherkennungs-API des vorhergegangenen Sprachdiensts. Die neuen Endpunkte sind in der Tabelle unten dargestellt. Verwenden Sie den Endpunkt, der die beste Übereinstimmung mit Ihrer Abonnementregion aufweist.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

Die Spracherkennungs-API ist ansonsten der [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) für die vorhergegangene Sprach-API ähnlich.

Die Spracherkennungs-REST-API unterstützt nur kurze Äußerungen. Anforderungen dürfen bis zu 10 Sekunden Audio enthalten und insgesamt maximal 14 Sekunden dauern. Die REST-API gibt nur Endergebnisse zurück, keine Teil- oder Zwischenergebnisse.

> [!NOTE]
> Wenn Sie das akustische Modell, das Sprachmodell oder die Aussprache angepasst haben, verwenden Sie stattdessen Ihren eigenen Endpunkt.

## <a name="text-to-speech"></a>Text-zu-Sprache

Die neue Sprachsynthese-API unterstützt die Audioausgabe mit 24 KHz. Der `X-Microsoft-OutputFormat`-Header kann jetzt die folgenden Werte aufweisen.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

Der Spracherkennungsdienst bietet jetzt zwei Stimmen mit 24 KHz:

Gebietsschema | Sprache   | Geschlecht | Dienstnamenzuordnung
-------|------------|--------|------------
en-US  | Englisch (USA) | Female | Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS) 
en-US  | Englisch (USA) | Male   | Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)

Die folgende Aufstellung enthält die REST-Endpunkte für die Text-to-Speech-API des vereinheitlichten Spracherkennungsdiensts. Verwenden Sie den Endpunkt für Ihre Abonnementregion.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

Beachten Sie diese Unterschiede bei der Verwendung der [REST-API-Dokumentation](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) der vorhergegangenen Sprach-API.

## <a name="authentication"></a>Authentifizierung

Für das Senden einer Anforderung an die REST-API des Spracherkennungsdiensts ist ein Zugriffstoken erforderlich. Sie erhalten ein Token, indem Sie einem regionalen `issueToken`-Endpunkt des Spracherkennungsdiensts Ihren Abonnementschlüssel zur Verfügung stellen. Verwenden Sie den Endpunkt für Ihre Abonnementregion.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Jedes Zugriffstoken ist 10 Minuten lang gültig. Sie können jederzeit ein neues Token anfordern – wenn Sie möchten auch vor jeder Anforderung der Sprach-REST-API. Um Netzwerkverkehr und Latenz zu minimieren, empfiehlt es sich jedoch, neun Minuten lang das gleiche Token zu verwenden.

In den folgenden Abschnitten ist beschrieben, wie Sie ein Token abrufen und es in einer Anforderung verwenden.

### <a name="getting-a-token-http"></a>Abrufen eines Tokens: HTTP

Unten finden Sie eine HTTP-Beispielanforderung zum Abrufen eines Tokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Zone USA (Westen) befindet, ersetzen Sie den `Host`-Header durch den Hostnamen für Ihre Region.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Der Textkörper der Antwort auf diese Anforderung stellt das Zugriffstoken im JWT-Format (Java Web Token) dar.

### <a name="getting-a-token-powershell"></a>Abrufen eines Tokens: PowerShell

Das Windows PowerShell-Skript unten veranschaulicht das Abrufen eines Zugriffstokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Region USA (Westen) befindet, ändern Sie den Hostnamen des angegebenen URIs entsprechend.

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

### <a name="getting-a-token-curl"></a>Abrufen eines Tokens: cURL

cURL ist ein Befehlszeilentool, das in Linux (und im Windows-Subsystem für Linux) zur Verfügung steht. Der cURL-Befehl unten zeigt das Abrufen eines Zugriffstokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Region USA (Westen) befindet, ändern Sie den Hostnamen des angegebenen URIs entsprechend.

> [!NOTE]
> Der Befehl ist zur besseren Lesbarkeit mehrzeilig dargestellt, sollte aber an einem Shell-Prompt in einer einzelnen Zeile eingegeben werden.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Abrufen eines Tokens: C#

Die C#-Klasse unten veranschaulicht das Abrufen eines Zugriffstokens. Übergeben Sie Ihren Abonnementschlüssel für den Sprachdienst beim Instanziieren der Klasse. Wenn sich Ihr Abonnement nicht in der Region USA (Westen) befindet, ändern Sie den Hostnamen von `FetchTokenUri` entsprechend.

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
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

### <a name="using-a-token"></a>Verwenden eines Tokens

Um ein Token in einer REST-API-Anforderung zu verwenden, geben Sie es im `Authorization`-Header nach dem Wort `Bearer` an. Beispielsweise sehen Sie hier eine Sprachsynthese-REST-Beispielanforderung, die ein Token enthält. Ersetzen Sie `YOUR_ACCESS_TOKEN` durch Ihr tatsächliches Token, und verwenden Sie im `Host`-Header den richtigen Hostnamen.

```xml
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="renewing-authorization"></a>Erneuern der Autorisierung

Das Autorisierungstoken läuft nach 10 Minuten ab. Erneuern Sie Ihr Autorisierungstoken, indem Sie vor dem Ablauf ein neues Token abrufen – beispielsweise nach neun Minuten. 

Der folgende C#-Code stellt einen Drop-In-Ersatz für die weiter oben vorgestellte Klasse dar. Die `Authentication`-Klasse ruft mithilfe eines Timers automatisch alle 9 Minuten ein neues Zugriffstoken ab. Durch diesen Ansatz ist sichergestellt, dass während der Ausführung Ihres Programms immer ein gültiges Token zur Verfügung steht.

> [!NOTE]
> Statt einen Timer zu verwenden, können Sie auch einen Zeitstempel für den Abruf des aktuellen Tokens speichern und ein neues Token nur dann abrufen, wenn das aktuelle Token kurz vor dem Ablauf steht. Dieser Ansatz vermeidet das unnötige Abrufen neuer Token und kann die passendere Wahl für Programme sein, die nur selten Sprachanforderungen ausgeben.

Achten Sie wie auch in den anderen Beispielen darauf, dass der Wert von `FetchTokenUri` mit Ihrer Abonnementregion übereinstimmt. Übergeben Sie Ihren Abonnementschlüssel beim Instanziieren der Klasse.

```cs
    /*
     * This class demonstrates how to maintain a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = 
            "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
        private string subscriptionKey;
        private string token;
        private Timer accessTokenRenewer;

        //Access token expires every 10 minutes. Renew it every 9 minutes.
        private const int RefreshTokenDuration = 9;

        public Authentication(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
            this.token = FetchToken(FetchTokenUri, subscriptionKey).Result;

            // renew the token on set duration.
            accessTokenRenewer = new Timer(new TimerCallback(OnTokenExpiredCallback),
                                           this,
                                           TimeSpan.FromMinutes(RefreshTokenDuration),
                                           TimeSpan.FromMilliseconds(-1));
        }

        public string GetAccessToken()
        {
            return this.token;
        }

        private void RenewAccessToken()
        {
            this.token = FetchToken(FetchTokenUri, this.subscriptionKey).Result;
            Console.WriteLine("Renewed token.");
        }

        private void OnTokenExpiredCallback(object stateInfo)
        {
            try
            {
                RenewAccessToken();
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Failed renewing access token. Details: {0}", ex.Message));
            }
            finally
            {
                try
                {
                    accessTokenRenewer.Change(TimeSpan.FromMinutes(RefreshTokenDuration), TimeSpan.FromMilliseconds(-1));
                }
                catch (Exception ex)
                {
                    Console.WriteLine(string.Format("Failed to reschedule the timer to renew access token. Details: {0}", ex.Message));
                }
            }
        }

        private async Task<string> FetchToken(string fetchUri, string subscriptionKey)
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

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)


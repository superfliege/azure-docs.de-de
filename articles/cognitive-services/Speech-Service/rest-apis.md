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
ms.openlocfilehash: 6758cd658daf75beeea93bf9c719508cd271c8be
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032426"
---
# <a name="speech-service-rest-apis"></a>REST-APIs des Spracherkennungsdiensts

Die REST-APIs des vereinheitlichten Spracherkennungsdiensts Azure Cognitive Services ähneln den APIs, die von der [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/Speech) zur Verfügung gestellt werden. Die Endpunkte unterscheiden sich von den Endpunkten, die vom Bing-Spracheingabedienst verwendet werden. Regionale Endpunkte sind verfügbar, und Sie müssen einen Abonnementschlüssel verwenden, der dem von Ihnen verwendeten Endpunkt entspricht.

## <a name="speech-to-text"></a>Spracherkennung

Die Endpunkte für die Spracherkennungs-REST-API werden in der folgenden Tabelle aufgeführt. Verwenden Sie den Endpunkt, der die beste Übereinstimmung mit Ihrer Abonnementregion aufweist.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

> [!NOTE]
> Wenn Sie das akustische Modell, das Sprachmodell oder die Aussprache angepasst haben, verwenden Sie stattdessen Ihren eigenen Endpunkt.

Diese API unterstützt nur kurze Äußerungen. Anforderungen dürfen bis zu 10 Sekunden Audio enthalten und insgesamt maximal 14 Sekunden dauern. Die REST-API gibt nur Endergebnisse zurück, keine Teil- oder Zwischenergebnisse. Der Spracherkennungsdienst verfügt auch über eine [Batchtranskriptions](batch-transcription.md)-API, die längere Audioaufzeichnungen transkribieren kann.

### <a name="query-parameters"></a>Abfrageparameter

Die folgenden Parameter können in der Abfragezeichenfolge der REST-Anforderung enthalten sein.

|Parametername|Erforderlich/optional|Bedeutung|
|-|-|-|
|`language`|Erforderlich|Der Bezeichner der zu erkennenden Sprache. Siehe [Unterstützte Sprachen](supported-languages.md#speech-to-text).|
|`format`|Optional<br>Standardwert: `simple`|Ergebnisformat, `simple` oder `detailed`. Einfache Ergebnisse enthalten `RecognitionStatus`, `DisplayText`, `Offset` und die Dauer. Detaillierte Ergebnisse enthalten mehrere Kandidaten mit Zuverlässigkeitswerten und vier unterschiedliche Darstellungen.|
|`profanity`|Optional<br>Standardwert: `masked`|Umgang mit Obszönitäten in Erkennungsergebnissen. Kann `masked` (ersetzt Obszönitäten durch Sternchen), `removed` (entfernt alle Obszönitäten) oder `raw` (behält Obszönitäten bei) sein.

### <a name="request-headers"></a>Anforderungsheader

Die folgenden Felder werden im HTTP-Anforderungsheader gesendet.

|Header|Bedeutung|
|------|-------|
|`Ocp-Apim-Subscription-Key`|Ihr Abonnementschlüssel des Spracherkennungsdiensts. Dieser Header oder `Authorization` muss angegeben werden.|
|`Authorization`|Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Dieser Header oder `Ocp-Apim-Subscription-Key` muss angegeben werden. Siehe [Authentifizierung](#authentication).|
|`Content-type`|Beschreibt das Format und den Codec der Audiodaten. Zurzeit muss dieser Wert auf `audio/wav; codec=audio/pcm; samplerate=16000` festgelegt werden.|
|`Transfer-Encoding`|Optional. Wenn angegeben, muss der Wert `chunked` sein, damit Audiodaten in mehreren kleinen Blöcken anstelle einer einzelnen Datei gesendet werden können.|
|`Expect`|Wenn Sie segmentierte Übertragung verwenden, senden Sie `Expect: 100-continue`. Der Spracherkennungsdienst bestätigt die ursprüngliche Anforderung und wartet auf weitere Daten.|
|`Accept`|Optional. Wenn angegeben, muss `application/json` enthalten sein, da der Spracherkennungsdienst Ergebnisse im JSON-Format zurückgibt. (Einige Webanforderungsframeworks stellen einen inkompatiblen Standardwert bereit, wenn Sie keinen Wert angeben. Daher ist es empfehlenswert, immer `Accept` zu verwenden.)|

### <a name="audio-format"></a>Audioformat

Die Audiodaten werden im Textkörper der HTTP-`PUT`-Anforderung gesendet. Sie sollten im 16-Bit-WAV-Format mit einem PCM-Kanal (Mono) mit 16 KHz vorliegen.

### <a name="chunked-transfer"></a>Segmentierte Übertragung

Segmentierte Übertragung (`Transfer-Encoding: chunked`) kann dazu beitragen, die Erkennungslatenz zu verringern, da sie es dem Spracherkennungsdienst ermöglicht, mit der Verarbeitung der Audiodatei zu beginnen, während sie übertragen wird. Der REST-API bietet keine Teil- oder Zwischenergebnisse. Diese Option dient ausschließlich zur Verbesserung der Reaktionsfähigkeit.

Der folgende Code zeigt, wie Sie Audiodaten in Blöcken senden. `request` ist ein HTTPWebRequest-Objekt, das mit dem entsprechenden REST-Endpunkt verbunden ist. `audioFile` ist der Pfad zu einer Audiodatei auf dem Datenträger.

```csharp
using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

### <a name="example-request"></a>Beispielanforderung

Das folgende Beispiel zeigt eine typische Anforderung.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status"></a>HTTP-Status

Der HTTP-Status der Antwort zeigt den Erfolg oder allgemeine Fehlerbedingungen an.

HTTP-Code|Bedeutung|Mögliche Ursache
-|-|-|
100|Weiter|Die ursprüngliche Anforderung wurde akzeptiert. Mit dem Senden der restlichen Daten fortfahren. (Wird mit segmentierter Übertragung verwendet.)
200|OK|Die Anforderung war erfolgreich. Der Antworttext ist ein JSON-Objekt.
400|Ungültige Anforderung|Der Sprachcode wurde nicht bereitgestellt oder ist keine unterstützte Sprache, ungültige Audiodatei.
401|Nicht autorisiert|Der Abonnementschlüssel oder das Autorisierungstoken ist in der angegebenen Region ungültig oder ungültiger Endpunkt.
403|Verboten|Fehlender Abonnementschlüssel oder fehlendes Autorisierungstoken.

### <a name="json-response"></a>JSON-Antwort

Ergebnisse werden im JSON-Format zurückgegeben. Das `simple`-Format schließt nur die folgenden Felder auf oberster Ebene ein.

|Feldname|Inhalt|
|-|-|
|`RecognitionStatus`|Status, z.B. `Success` für erfolgreiche Erkennung. Siehe nächste Tabelle.|
|`DisplayText`|Der erkannte Text nach Großschreibung, Interpunktion, inverser Textnormalisierung (Umwandlung von gesprochenem Text in kürzere Formen, z.B. 200 für „zweihundert“ oder „Dr. Smith“ für „doctor smith“) und Obszönitätenmaskierung. Nur bei Erfolg vorhanden.|
|`Offset`|Die Zeit (in Einheiten von 100 Nanosekunden), zu der die erkannte Sprache im Audiostream beginnt.|
|`Duration`|Die Dauer (in Einheiten von 100 Nanosekunden) der erkannten Sprache im Audiostream.|

Das `RecognitionStatus`-Feld kann die folgenden Werte enthalten.

|Statuswert|BESCHREIBUNG
|-|-|
| `Success` | Die Erkennung war erfolgreich, und das DisplayText-Feld ist vorhanden. |
| `NoMatch` | Im Audiodatenstrom wurde Sprache erkannt, aber es wurde keine Übereinstimmung mit Wörtern aus der Zielsprache festgestellt. Normalerweise bedeutet dies, dass die Erkennungssprache eine andere Sprache ist als die, die der Benutzer spricht. |
| `InitialSilenceTimeout` | Der Anfang des Audiodatenstroms enthielt nur Stille, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `BabbleTimeout` | Der Anfang des Audiodatenstroms enthielt nur Rauschen, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `Error` | Der Erkennungsdienst hat einen internen Fehler erkannt und konnte nicht fortgesetzt werden. Versuchen Sie es noch mal, wenn möglich. |

> [!NOTE]
> Wenn die Audiodaten nur aus Obszönitäten bestehen und der `profanity`-Abfrageparameter auf `remove` festgelegt ist, gibt der Dienst kein Sprachergebnis zurück. 


Das `detailed`-Format enthält die gleichen Felder wie das `simple`-Format sowie ein `NBest`-Feld. Das `NBest`-Feld ist eine Liste von alternativen Interpretationen derselben Spracheingabe. Die Rangfolge beginnt mit der wahrscheinlichsten und endet mit der am wenigsten wahrscheinlichen Interpretation. Der erste Eintrag ist identisch mit den Haupterkennungsergebnis. Jeder Eintrag enthält die folgenden Felder:

|Feldname|Inhalt|
|-|-|
|`Confidence`|Die Zuverlässigkeitsbewertung des Eintrags von 0,0 (keine Zuverlässigkeit) bis 1,0 (volle Zuverlässigkeit)
|`Lexical`|Die lexikalische Form des erkannten Texts: die tatsächlich erkannten Wörter.
|`ITN`|Die inverse Textnormalisierung („kanonische Form“) des erkannten Texts mit Telefonnummern, Zahlen, Abkürzungen („doctor smith“ in „dr. smith“) und anderen angewendeten Transformationen.
|`MaskedITN`| Die ITN-Form mit angewendeter Obszönitätenmaskierung, wenn angefordert.
|`Display`| Die Anzeigeform des erkannten Texts mit hinzugefügten Satzzeichen und Großschreibung. Identisch mit `DisplayText` im Ergebnis der obersten Ebene.

### <a name="sample-responses"></a>Beispielantworten

Das folgende Beispiel ist eine typische Antwort für die `simple`-Erkennung.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Das folgende Beispiel ist eine typische Antwort für die `detailed`-Erkennung.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="text-to-speech"></a>Text-zu-Sprache

Die folgende Aufstellung enthält die REST-Endpunkte für die Text-to-Speech-API des Spracherkennungsdiensts. Verwenden Sie den Endpunkt für Ihre Abonnementregion.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Voicefont erstellt haben, verwenden Sie stattdessen den zugehörigen benutzerdefinierten Endpunkt.

Der Spracherkennungsdienst unterstützt 24-KHz-Audioausgabe zusätzlich zu der 16-KHz-Ausgabe, die von der Bing-Spracheingabe unterstützt wird. Vier 24-KHz-Ausgabeformate stehen für die Verwendung im `X-Microsoft-OutputFormat`-HTTP-Header zur Verfügung sowie zwei 24-KHz-Stimmen: `Jessa24kRUS` und `Guy24kRUS`.

Gebietsschema | Sprache   | Geschlecht | Dienstnamenzuordnung
-------|------------|--------|------------
en-US  | Englisch (USA) | Female | Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS) 
en-US  | Englisch (USA) | Male   | Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)

Eine vollständige Liste der verfügbaren Stimmen finden Sie unter [Unterstützte Sprachen](supported-languages.md#text-to-speech).

### <a name="request-headers"></a>Anforderungsheader

Die folgenden Felder werden im HTTP-Anforderungsheader gesendet.

|Header|Bedeutung|
|------|-------|
|`Authorization`|Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Erforderlich. Siehe [Authentifizierung](#authentication).|
|`Content-Type`|Der Inhaltstyp der Eingabe: `application/ssml+xml`.|
|`X-Microsoft-OutputFormat`|Das Audioformat der Ausgabe. Siehe nächste Tabelle.|
|`User-Agent`|Der Anwendungsname. Erforderlich, muss weniger als 255 Zeichen enthalten.|

Die verfügbaren Audioausgabeformate (`X-Microsoft-OutputFormat`) beinhalten eine Bitrate und eine Codierung.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `raw-8khz-8bit-mono-mulaw`
`riff-8khz-8bit-mono-mulaw`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

> [!NOTE]
> Wenn die ausgewählte Stimme und das ausgewählte Ausgabeformat unterschiedliche Bitraten aufweisen, wird das Audio nach Bedarf neu gesampelt. 24-KHz-Stimmen unterstützen keine `audio-16khz-16kbps-mono-siren`- und `riff-16khz-16kbps-mono-siren`-Ausgabeformate. 

### <a name="request-body"></a>Anforderungstext

Der in Sprache zu konvertierende Text wird als Textkörper einer HTTP-`POST`-Anforderung entweder als Nur-Text (ASCII oder UTF-8) oder im [Speech Synthesis Markup Language](speech-synthesis-markup.md)-Format (SSML) (UTF-8) gesendet. Nur-Text-Anforderungen verwenden die Standardstimme und -sprache des Diensts. Senden von SSML, um eine andere Stimme zu verwenden.

### <a name="sample-request"></a>Beispiel für eine Anforderung

Die folgende HTTP-Anforderung verwendet SSML-Text, um die Stimme auszuwählen. Der Text darf nicht länger als 1.000 Zeichen sein.

```xml
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' 
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

### <a name="http-response"></a>HTTP-Antwort

Der HTTP-Status der Antwort zeigt den Erfolg oder allgemeine Fehlerbedingungen an.

HTTP-Code|Bedeutung|Mögliche Ursache
-|-|-|
200|OK|Die Anforderung war erfolgreich. Der Antworttext ist eine Audiodatei.
400 |Ungültige Anforderung |Ein erforderlicher Parameter fehlt, ist leer oder Null. Oder der an einen erforderlichen oder optionalen Parameter übergebene Wert ist ungültig. Ein häufiges Problem sind zu lange Kopfzeilen.
401|Nicht autorisiert |Die Anforderung ist nicht autorisiert. Stellen Sie sicher, dass Ihr Abonnementschlüssel oder -token gültig ist und sich in der richtigen Region befindet.
413|Anforderungsentität zu groß|Die SSML-Eingabe umfasst mehr als 1024 Zeichen.
|502|Ungültiges Gateway    | Netzwerk- oder serverseitiges Problem. Kann auch auf ungültige Header hinweisen.

Wenn der HTTP-Status `200 OK` ist, enthält der Text der Antwort eine Audiodatei im angeforderten Format. Diese Datei kann während ihrer Übertragung wiedergegeben oder in einem Puffer oder einer Datei für die spätere Wiedergabe oder anderweitige Verwendung gespeichert werden.

## <a name="authentication"></a>Authentifizierung

Das Senden einer Anforderung an die REST-API des Spracherkennungsdiensts erfordert entweder einen Abonnementschlüssel oder ein Zugriffstoken. Im Allgemeinen ist es am einfachsten, den Abonnementschlüssel direkt zu senden. Der Spracherkennungsdienst ruft dann das Zugriffstoken für Sie ab. Um die Antwortzeit zu minimieren, empfiehlt es sich, stattdessen ein Zugriffstoken zu verwenden.

Um ein Token abzurufen, stellen Sie Ihren Abonnementschlüssel für einen `issueToken`-Endpunkt eines regionalen Spracherkennungsdiensts bereit, wie in der folgenden Tabelle dargestellt. Verwenden Sie den Endpunkt für Ihre Abonnementregion.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Jedes Zugriffstoken ist 10 Minuten lang gültig. Sie können jederzeit ein neues Token abrufen. Wenn Sie möchten, können Sie ein Token kurz vor jeder Anforderung der Spacherkennungs-REST-API abrufen. Um Netzwerkverkehr und Latenz zu minimieren, empfiehlt es sich, neun Minuten lang das gleiche Token zu verwenden.

In den folgenden Abschnitten ist beschrieben, wie Sie ein Token abrufen und es in einer Anforderung verwenden.

### <a name="get-a-token-http"></a>Abrufen eines Tokens: HTTP

Das folgende Beispiel zeigt eine HTTP-Beispielanforderung zum Abrufen eines Tokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Region „USA, Westen“ befindet, ersetzen Sie den `Host`-Header durch den Hostnamen für Ihre Region.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Der Textkörper der Antwort auf diese Anforderung stellt das Zugriffstoken im JWT-Format (Java Web Token) dar.

### <a name="get-a-token-powershell"></a>Abrufen eines Tokens: PowerShell

Das folgende Windows PowerShell-Skript veranschaulicht das Abrufen eines Zugriffstokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Region „USA, Westen“ befindet, ändern Sie den Hostnamen des angegebenen URIs entsprechend.

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

### <a name="get-a-token-curl"></a>Abrufen eines Tokens: cURL

cURL ist ein Befehlszeilentool, das in Linux (und im Windows-Subsystem für Linux) zur Verfügung steht. Der folgende cURL-Befehl zeigt das Abrufen eines Zugriffstokens. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Wenn sich Ihr Abonnement nicht in der Region „USA, Westen“ befindet, ändern Sie den Hostnamen des angegebenen URIs entsprechend.

> [!NOTE]
> Der Befehl wird zur besseren Lesbarkeit in mehreren Zeilen dargestellt, sollte aber an einer Shell-Eingabeaufforderung in einer Zeile eingegeben werden.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="get-a-token-c"></a>Abrufen eines Tokens: C#

Die folgende C#-Klasse zeigt das Abrufen eines Zugriffstokens. Übergeben Sie Ihren Abonnementschlüssel für den Sprachdienst beim Instanziieren der Klasse. Wenn sich Ihr Abonnement nicht in der Region „USA, Westen“ befindet, ändern Sie den Hostnamen von `FetchTokenUri` entsprechend.

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

### <a name="use-a-token"></a>Verwenden eines Tokens

Um ein Token in einer REST-API-Anforderung zu verwenden, geben Sie es im `Authorization`-Header nach dem Wort `Bearer` an. Dies ist ein Beispiel für eine Sprachsynthese-REST-Anforderung, die ein Token enthält. Ersetzen Sie `YOUR_ACCESS_TOKEN` durch Ihr tatsächliches Token. Verwenden Sie den richtigen Hostnamen im `Host`-Header.

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

### <a name="renew-authorization"></a>Erneuern der Autorisierung

Das Autorisierungstoken läuft nach 10 Minuten ab. Erneuern Sie Ihr Autorisierungstoken, indem Sie vor dem Ablauf ein neues Token abrufen. Beispielsweise können Sie ein neues Token nach neun Minuten abrufen.

Der folgende C#-Code stellt einen Drop-In-Ersatz für die weiter oben vorgestellte Klasse dar. Die `Authentication`-Klasse ruft mithilfe eines Timers automatisch alle 9 Minuten ein neues Zugriffstoken ab. Durch diesen Ansatz ist sichergestellt, dass während der Ausführung Ihres Programms immer ein gültiges Token zur Verfügung steht.

> [!NOTE]
> Anstatt einen Timer zu verwenden, können Sie einen Zeitstempel speichern, der angibt, wann das letzte Token abgerufen wurde. Dann können Sie nur dann ein neues Token anfordern, wenn es kurz vor dem Ablaufen ist. Dieser Ansatz vermeidet das unnötige Abrufen neuer Token und kann die passendere Wahl für Programme sein, die nur selten Sprachanforderungen ausgeben.

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


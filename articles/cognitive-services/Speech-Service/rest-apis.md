---
title: Speech Services-REST-APIs – Speech Services
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Spracherkennungs- und Text-to-Speech-REST-APIs verwenden. In diesem Artikel erfahren Sie mehr über Autorisierungs- und Abfrageoptionen sowie darüber, wie Sie eine Anforderung strukturieren und eine Antwort erhalten.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b7f5d4683f0042b95399b86cd4f53c93518c3c56
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330671"
---
# <a name="speech-service-rest-apis"></a>REST-APIs des Speech-Diensts

Als Alternative zum [Speech SDK](speech-sdk.md) ermöglicht Ihnen der Speech-Dienst die Konvertierung von Sprache in Text und Text in Sprache mit einer Reihe von REST-APIs. Jeder zugängliche Endpunkt ist einer Region zugeordnet. Ihre Anwendung benötigt einen Abonnementschlüssel für den Endpunkt, den Sie verwenden möchten.

Bevor Sie REST-APIs verwenden, müssen Sie Folgendes wissen:
* Spracherkennungsanforderungen über die REST-API dürfen nur 10 Sekunden aufgezeichnetes Audio enthalten.
* Die Spracherkennung-REST-API gibt nur Endergebnisse zurück. Teilergebnisse werden nicht bereitgestellt.
* Die Text-to-Speech-REST-API erfordert einen Autorisierungsheader. Das bedeutet, dass Sie einen Tokenaustausch ausführen müssen, um auf den Dienst zuzugreifen. Weitere Informationen finden Sie unter [Authentifizierung](#authentication).

## <a name="authentication"></a>Authentifizierung

Jede Anforderung an die Spracherkennungs- oder Text-to-Speech-REST-API erfordert einen Autorisierungsheader. Diese Tabelle zeigt, welche Header für welchen Dienst unterstützt werden:

| Unterstützte Autorisierungsheader | Spracherkennung | Text-zu-Sprache |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | JA | Nein  |
| Autorisierung: Bearer | JA | JA |

Wenn Sie den Header `Ocp-Apim-Subscription-Key` verwenden, müssen Sie nur Ihren Abonnementschlüssel angeben. Beispiel: 

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Wenn Sie den Header `Authorization: Bearer` verwenden, müssen Sie eine Anforderung an den Endpunkt `issueToken` stellen. In dieser Anforderung tauschen Sie Ihren Abonnementschlüssel gegen ein Zugriffstoken, der 10 Minuten lang gültig ist. In den nächsten Abschnitten erfahren Sie, wie Sie ein Token abrufen, verwenden und aktualisieren können.

### <a name="how-to-get-an-access-token"></a>Abrufen eines Zugriffstokens

Um ein Zugriffstoken abzurufen, müssen Sie eine Anforderung an den `issueToken`-Endpunkt mit dem Header `Ocp-Apim-Subscription-Key` und Ihrem Abonnementschlüssel senden.

Diese Regionen und Endpunkte werden unterstützt:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Verwenden Sie diese Beispiele, um Ihre Zugriffstokenanforderung zu erstellen.

#### <a name="http-sample"></a>HTTP-Beispiel

Dieses Beispiel stellt eine einfache HTTP-Anforderung zum Abrufen eines Tokens dar. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Speech-Dienst. Wenn sich Ihr Abonnement nicht in der Region „USA, Westen“ befindet, ersetzen Sie den `Host`-Header durch den Hostnamen für Ihre Region.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Der Antworttext enthält das Zugriffstoken im JWT-Format (Java Web Token).

#### <a name="powershell-sample"></a>PowerShell-Beispiel

Dieses Beispiel stellt ein einfaches PowerShell-Skript zum Abrufen eines Zugriffstokens dar. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Speech-Dienst. Achten Sie darauf, dass Sie den richtigen Endpunkt für die Region Ihres Abonnements verwenden. In diesem Beispiel ist das „USA, Westen“.

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

#### <a name="curl-sample"></a>cURL-Beispiel

cURL ist ein Befehlszeilentool, das in Linux (und im Windows-Subsystem für Linux) zur Verfügung steht. Dieser cURL-Befehl veranschaulicht, wie Sie ein Zugriffstoken abrufen. Ersetzen Sie `YOUR_SUBSCRIPTION_KEY` durch Ihren Abonnementschlüssel für den Speech-Dienst. Achten Sie darauf, dass Sie den richtigen Endpunkt für die Region Ihres Abonnements verwenden. In diesem Beispiel ist das „USA, Westen“.

```cli
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>C#-Beispiel

Diese C#-Klasse veranschaulicht, wie Sie ein Zugriffstoken abrufen. Übergeben Sie Ihren Abonnementschlüssel für den Speech-Dienst beim Instanziieren der Klasse. Wenn Ihr Abonnement sich nicht in der Region „USA, Westen“ befindet, ändern Sie den Wert von `FetchTokenUri` so, dass er der Region Ihres Abonnements entspricht.

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

### <a name="how-to-use-an-access-token"></a>Verwenden eines Zugriffstokens

Das Zugriffstoken sollte als `Authorization: Bearer <TOKEN>`-Header an den Dienst gesendet werden. Jedes Zugriffstoken ist 10 Minuten lang gültig. Sie können jederzeit ein neues Token abrufen, allerdings wird empfohlen, das gleiche Token 9 Minuten lang zu verwenden, um den Datenverkehr und die Wartezeit zu minimieren.

Hier ist eine Beispiel-HTTP-Anforderung an die Text-to-Speech-REST-API:

```http
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

### <a name="how-to-renew-an-access-token-using-c"></a>Erneuern eines Zugriffstokens mit C#

Dieser C#-Code stellt einen direkten Ersatz für die oben vorgestellte Klasse dar. Die `Authentication`-Klasse ruft mithilfe eines Timers alle 9 Minuten automatisch ein neues Zugriffstoken ab. Durch diesen Ansatz ist sichergestellt, dass während der Ausführung Ihres Programms immer ein gültiges Token zur Verfügung steht.

> [!NOTE]
> Anstatt einen Timer zu verwenden, können Sie einen Zeitstempel speichern, der angibt, wann das letzte Token abgerufen wurde. Dann können Sie nur dann ein neues Token anfordern, wenn es kurz vor dem Ablaufen ist. Dieser Ansatz vermeidet das unnötige Abrufen neuer Token und kann die passendere Wahl für Programme sein, die nur selten Sprachanforderungen ausgeben.

Achten Sie wie auch in den anderen Beispielen darauf, dass der Wert von `FetchTokenUri` mit Ihrer Abonnementregion übereinstimmt. Übergeben Sie Ihren Abonnementschlüssel beim Instanziieren der Klasse.

```cs
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

## <a name="speech-to-text-api"></a>Spracherkennungs-API

Die Spracherkennungs-REST-API unterstützt nur kurze Äußerungen. Anforderungen dürfen bis zu 10 Sekunden Audio enthalten und insgesamt maximal 14 Sekunden dauern. Die REST-API gibt nur Endergebnisse zurück, keine Teil- oder Zwischenergebnisse.

Wenn das Senden von längerem Audio eine Anforderung für Ihre Anwendung ist, verwenden Sie das [Speech SDK](speech-sdk.md) oder die [Batchtranskription](batch-transcription.md).

### <a name="regions-and-endpoints"></a>Regionen und Endpunkte

Diese Regionen werden für die Spracherkennungstranskription über die REST-API unterstützt. Achten Sie darauf, dass Sie den Endpunkt für Ihre Abonnementregion auswählen.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="query-parameters"></a>Abfrageparameter

Diese Parameter können in der Abfragezeichenfolge der REST-Anforderung enthalten sein.

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| `language` | Identifiziert die gesprochene Sprache, die erkannt wird. Siehe [Unterstützte Sprachen](language-support.md#speech-to-text). | Erforderlich |
| `format` | Gibt das Ergebnisformat an. Zulässige Werte sind `simple` und `detailed`. Einfache Ergebnisse enthalten `RecognitionStatus`, `DisplayText`, `Offset` und `Duration`. Detaillierte Antworten enthalten mehrere Ergebnisse mit Zuverlässigkeitswerten und vier unterschiedliche Darstellungen. Die Standardeinstellung ist `simple`. | Optional |
| `profanity` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `raw` (Obszönitäten sind im Ergebnis enthalten). Die Standardeinstellung ist `masked`. | Optional |

### <a name="request-headers"></a>Anforderungsheader

Diese Tabelle führt die erforderlichen und optionalen Header für Spracherkennungsanforderungen auf.

|Header| BESCHREIBUNG | Erforderlich/optional |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Ihr Abonnementschlüssel des Speech-Diensts. | Entweder dieser Header oder `Authorization` ist erforderlich. |
| `Authorization` | Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Weitere Informationen finden Sie unter [Authentifizierung](#authentication). | Entweder dieser Header oder `Ocp-Apim-Subscription-Key` ist erforderlich. |
| `Content-type` | Beschreibt das Format und den Codec der bereitgestellten Audiodaten. Zulässige Werte sind `audio/wav; codecs=audio/pcm; samplerate=16000` und `audio/ogg; codecs=opus`. | Erforderlich |
| `Transfer-Encoding` | Gibt an, dass segmentierte Audiodaten anstatt einer einzelnen Datei gesendet werden. Verwenden Sie diesen Header nur, wenn Sie Audiodaten segmentieren. | Optional |
| `Expect` | Wenn Sie segmentierte Übertragung verwenden, senden Sie `Expect: 100-continue`. Der Speech-Dienst bestätigt die ursprüngliche Anforderung und wartet auf weitere Daten.| Erforderlich, wenn segmentierte Audiodaten gesendet werden. |
| `Accept` | Wenn angegeben, muss der Wert `application/json` entsprechen. Der Speech-Dienst übermittelt Ergebnisse im JSON-Format. Einige Webanforderungsframeworks stellen einen inkompatiblen Standardwert bereit, wenn Sie keinen Wert angeben. Deswegen wird empfohlen, `Accept` immer zu verwenden. | Optional, wird jedoch empfohlen. |

### <a name="audio-formats"></a>Audioformate

Audiodaten werden im Text der HTTP-`POST`-Anforderung gesendet. Sie müssen in einem der in der folgenden Tabelle aufgeführten Formate vorliegen:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 16 kHz, mono |
| OGG | OPUS | 16 Bit | 16 kHz, mono |

>[!NOTE]
>Die oben genannten Formate werden durch die REST-API und WebSocket im Speech-Dienst unterstützt. Das [Speech-SDK](speech-sdk.md) unterstützt gegenwärtig nur das WAV-Format mit dem PCM-Codec.

### <a name="sample-request"></a>Beispiel für eine Anforderung

Hier sehen Sie eine übliche HTTP-Anforderung. Das folgende Beispiel enthält den Hostnamen und die erforderlichen Header. Beachten Sie, dass der Dienst auch Audiodaten erwartet, die in diesem Beispiel nicht enthalten sind. Wie bereits erwähnt, wird die Segmentierung empfohlen, ist aber nicht erforderlich.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status-codes"></a>HTTP-Statuscodes

Der HTTP-Statuscode jeder Antwort zeigt den Erfolg oder allgemeine Fehler an.

| HTTP-Statuscode | BESCHREIBUNG | Mögliche Ursache |
|------------------|-------------|-----------------|
| 100 | Weiter | Die ursprüngliche Anforderung wurde akzeptiert. Mit dem Senden der restlichen Daten fortfahren. (Wird mit segmentierter Übertragung verwendet.) |
| 200 | OK | Die Anforderung war erfolgreich. Der Antworttext ist ein JSON-Objekt. |
| 400 | Ungültige Anforderung | Der Sprachcode wurde nicht bereitgestellt oder ist keine unterstützte Sprache, ungültige Audiodatei. |
| 401 | Nicht autorisiert | Der Abonnementschlüssel oder das Autorisierungstoken ist in der angegebenen Region ungültig oder ungültiger Endpunkt. |
| 403 | Verboten | Fehlender Abonnementschlüssel oder fehlendes Autorisierungstoken. |

### <a name="chunked-transfer"></a>Segmentierte Übertragung

Die segmentierte Übertragung (`Transfer-Encoding: chunked`) kann die Erkennungslatenz verringern, da sie dem Speech-Dienst ermöglicht, mit der Verarbeitung der Audiodatei bei der Übertragung zu beginnen. Der REST-API bietet keine Teil- oder Zwischenergebnisse. Diese Option dient ausschließlich zur Verbesserung der Reaktionsfähigkeit.

Dieses Codebeispiel zeigt, wie Sie Audio in Blöcken senden. Nur der erste Block sollte den Header der Audiodatei enthalten. `request` ist ein HTTPWebRequest-Objekt, das mit dem entsprechenden REST-Endpunkt verbunden ist. `audioFile` ist der Pfad zu einer Audiodatei auf dem Datenträger.

```csharp

    HttpWebRequest request = null;
    request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
    request.SendChunked = true;
    request.Accept = @"application/json;text/xml";
    request.Method = "POST";
    request.ProtocolVersion = HttpVersion.Version11;
    request.Host = host;
    request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
    request.Headers["Ocp-Apim-Subscription-Key"] = args[1];
    request.AllowWriteStreamBuffering = false;

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

### <a name="response-parameters"></a>Antwortparameter

Ergebnisse werden im JSON-Format bereitgestellt. Das `simple`-Format schließt diese Felder auf oberster Ebene ein.

| Parameter | BESCHREIBUNG  |
|-----------|--------------|
|`RecognitionStatus`|Status, z.B. `Success` für erfolgreiche Erkennung. Siehe nächste Tabelle.|
|`DisplayText`|Der erkannte Text nach Großschreibung, Interpunktion, inverser Textnormalisierung (Umwandlung von gesprochenem Text in kürzere Formen, z.B. 200 für „zweihundert“ oder „Dr. Smith“ für „doctor smith“) und Obszönitätenmaskierung. Nur bei Erfolg vorhanden.|
|`Offset`|Die Zeit (in Einheiten von 100 Nanosekunden), zu der die erkannte Sprache im Audiostream beginnt.|
|`Duration`|Die Dauer (in Einheiten von 100 Nanosekunden) der erkannten Sprache im Audiostream.|

Das `RecognitionStatus`-Feld kann diese Werte enthalten:

| Status | BESCHREIBUNG |
|--------|-------------|
| `Success` | Die Erkennung war erfolgreich, und das `DisplayText`-Feld ist vorhanden. |
| `NoMatch` | Im Audiodatenstrom wurde Sprache erkannt, aber es wurde keine Übereinstimmung mit Wörtern aus der Zielsprache festgestellt. Normalerweise bedeutet dies, dass die Erkennungssprache eine andere Sprache ist als die, die der Benutzer spricht. |
| `InitialSilenceTimeout` | Der Anfang des Audiodatenstroms enthielt nur Stille, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `BabbleTimeout` | Der Anfang des Audiodatenstroms enthielt nur Rauschen, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `Error` | Der Erkennungsdienst hat einen internen Fehler erkannt und konnte nicht fortgesetzt werden. Versuchen Sie es noch mal, wenn möglich. |

> [!NOTE]
> Wenn die Audiodaten nur aus Obszönitäten bestehen und der `profanity`-Abfrageparameter auf `remove` festgelegt ist, gibt der Dienst kein Sprachergebnis zurück.

Das Format `detailed` enthält die gleichen Daten wie das Format `simple` sowie `NBest`, eine Liste alternativer Interpretationen desselben Spracherkennungsergebnisses. Diese Ergebnisse werden von sehr wahrscheinlich bis am wenigsten wahrscheinlich eingestuft. Der erste Eintrag entspricht dem Haupterkennungsergebnis.  Bei Verwendung des Formats `detailed` wird `DisplayText` als `Display` für jedes Ergebnis in der `NBest`-Liste angegeben.

Jedes Objekt in der `NBest`-Liste enthält:

| Parameter | BESCHREIBUNG |
|-----------|-------------|
| `Confidence` | Die Zuverlässigkeitsbewertung des Eintrags von 0,0 (keine Zuverlässigkeit) bis 1,0 (volle Zuverlässigkeit) |
| `Lexical` | Die lexikalische Form des erkannten Texts: die tatsächlich erkannten Wörter. |
| `ITN` | Die inverse Textnormalisierung („kanonische Form“) des erkannten Texts mit Telefonnummern, Zahlen, Abkürzungen („doctor smith“ in „dr. smith“) und anderen angewendeten Transformationen. |
| `MaskedITN` | Die ITN-Form mit angewendeter Obszönitätenmaskierung, wenn angefordert. |
| `Display` | Die Anzeigeform des erkannten Texts mit hinzugefügten Satzzeichen und Großschreibung. Dieser Parameter ist derselbe wie `DisplayText`, wenn das Format `simple` entspricht. |

### <a name="sample-responses"></a>Beispielantworten

Hier sehen Sie eine typische Antwort für die Erkennung `simple`.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Hier sehen Sie eine typische Antwort für die Erkennung `detailed`.

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

## <a name="text-to-speech-api"></a>Text-to-Speech-API

Die Text-to-Speech-REST-API unterstützt neuronale und Standardstimmen für die Sprachsynthese. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert.

* Eine vollständige Liste der Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).
* Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#text-to-speech).

### <a name="request-headers"></a>Anforderungsheader

Diese Tabelle führt die erforderlichen und optionalen Header für Spracherkennungsanforderungen auf.

| Header | BESCHREIBUNG | Erforderlich/optional |
|--------|-------------|---------------------|
| `Authorization` | Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Weitere Informationen finden Sie unter [Authentifizierung](#authentication). | Erforderlich |
| `Content-Type` | Gibt den Inhaltstyp des angegebenen Texts an. Zulässiger Wert: `application/ssml+xml`. | Erforderlich |
| `X-Microsoft-OutputFormat` | Gibt das Audioausgabeformat an. Eine vollständige Liste der zulässigen Werte finden Sie unter [Audioausgaben](#audio-outputs). | Erforderlich |
| `User-Agent` | Der Name der Anwendung. Er darf 255 Zeichen nicht überschreiten. | Erforderlich |

### <a name="audio-outputs"></a>Audioausgaben

Dies ist eine Liste der unterstützten Audioformate, die in jeder Anforderung als `X-Microsoft-OutputFormat`-Header gesendet werden. Es wird jeweils eine Bitrate und ein Codierungstyp angegeben. Der Speech-Dienst unterstützt Audioausgaben mit 24 kHz und 16 kHz.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-mulaw` | `riff-16khz-16bit-mono-pcm` |
| `audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3` |
| `audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm` |
| `riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3` |
| `audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3` |

> [!NOTE]
> Wenn die ausgewählte Stimme und das ausgewählte Ausgabeformat unterschiedliche Bitraten aufweisen, wird das Audio nach Bedarf neu gesampelt. 24-KHz-Stimmen unterstützen keine `audio-16khz-16kbps-mono-siren`- und `riff-16khz-16kbps-mono-siren`-Ausgabeformate.

### <a name="request-body"></a>Anforderungstext

Der Text jeder `POST`-Anforderung wird als [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) gesendet. SSML ermöglicht es Ihnen, die Stimme und Sprache der synthetisierten Sprachausgabe auszuwählen, die vom Text-zu-Sprache-Dienst zurückgegeben wird. Eine vollständige Liste der unterstützten Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

> [!NOTE]
> Wenn Sie eine benutzerdefinierte Stimme verwenden, kann der Text der Anforderung als Nur-Text (ASCII oder UTF-8) gesendet werden.

### <a name="sample-request"></a>Beispiel für eine Anforderung

Diese HTTP-Anforderung gibt mit SSML die Stimme und die Sprache an. Der Text darf 1.000 Zeichen nicht überschreiten.

```http
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

### <a name="http-status-codes"></a>HTTP-Statuscodes

Der HTTP-Statuscode jeder Antwort zeigt den Erfolg oder allgemeine Fehler an.

| HTTP-Statuscode | BESCHREIBUNG | Mögliche Ursache |
|------------------|-------------|-----------------|
| 200 | OK | Die Anforderung war erfolgreich. Der Antworttext ist eine Audiodatei. |
| 400 | Ungültige Anforderung | Ein erforderlicher Parameter fehlt, ist leer oder Null. Oder der an einen erforderlichen oder optionalen Parameter übergebene Wert ist ungültig. Ein häufiges Problem sind zu lange Kopfzeilen. |
| 401 | Nicht autorisiert | Die Anforderung ist nicht autorisiert. Stellen Sie sicher, dass Ihr Abonnementschlüssel oder -token gültig ist und sich in der richtigen Region befindet. |
| 413 | Anforderungsentität zu groß | Die SSML-Eingabe umfasst mehr als 1024 Zeichen. |
| 429 | Zu viele Anforderungen | Sie haben das Kontingent oder die Rate der Anforderungen überschritten, das bzw. die für Ihr Abonnement zulässig ist. |
| 502 | Ungültiges Gateway | Netzwerk- oder serverseitiges Problem. Kann auch auf ungültige Header hinweisen. |

Wenn der HTTP-Status `200 OK` ist, enthält der Text der Antwort eine Audiodatei im angeforderten Format. Diese Datei kann bei der Übertragung abgespielt sowie in einem Puffer oder in einer Datei gespeichert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)

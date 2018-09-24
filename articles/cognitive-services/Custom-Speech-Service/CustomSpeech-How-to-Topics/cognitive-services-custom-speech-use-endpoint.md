---
title: Verwenden eines benutzerdefinierten Spracherkennungsendpunkts mit Custom Speech Service in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen benutzerdefinierten Endpunkt für die Spracherkennung mit Custom Speech Service in Cognitive Services verwenden.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 55583952df3b83331f1f622a4fce269713ecf2a6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966519"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Verwenden eines benutzerdefinierten Spracherkennungsendpunkts
Sie können in ähnlicher Weise wie beim Standardendpunkt von Cognitive Services für die Spracherkennung Anforderungen an einen Azure Custom Speech Service-Endpunkt für die Spracherkennung senden. Diese Endpunkte sind funktionell mit den Standardendpunkten der Sprach-API identisch. Folglich steht die gleiche Funktionalität über die Clientbibliothek oder REST-API für die Sprach-API auch für Ihren benutzerdefinierten Endpunkt zur Verfügung.

Die Endpunkte, die Sie mit diesem Dienst erstellen, können unterschiedliche Mengen gleichzeitiger Anforderungen verarbeiten. Die Menge, hängt von dem Tarif ab, dem Ihr Abonnement zugeordnet ist. Wenn zu viele Anforderungen empfangen werden, tritt ein Fehler auf. Im Free-Tarif ist die Anzahl von Anforderungen in einem Monat begrenzt.

Bei der Verwendung des Diensts wird davon ausgegangen, dass Daten in Echtzeit übertragen werden. Wenn Daten noch schneller gesendet werden, wird die Anforderung so lange ausgeführt, bis deren Audiodauer in Echtzeit verstrichen ist.

> [!NOTE]
> Wir unterstützen auch schon die [neuen Websockets](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol). Wenn Sie bei Ihrem benutzerdefinierten Endpunkt für die Spracherkennung Websockets verwenden möchten, befolgen Sie die im Folgenden aufgeführten Anweisungen.
>
> Unterstützung für die neue [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) ist demnächst verfügbar. Wenn Sie Ihren benutzerdefinierten Endpunkt für die Spracherkennung über HTTP aufrufen möchten, befolgen Sie die im Folgenden aufgeführten Anweisungen.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Senden von Anforderungen mithilfe der Clientbibliothek für die Spracherkennung

Um mithilfe der Clientbibliothek für die Spracherkennung Anforderungen an den benutzerdefinierten Endpunkt zu senden, starten Sie den Client für die Spracherkennung. Verwenden Sie das Client-SDK für die Spracherkennung von [NuGet](http://nuget.org/). Suchen Sie nach *Spracherkennung*, und wählen Sie das Spracherkennungspaket von Microsoft für Ihre Plattform aus. Einige Beispielcodes finden Sie unter [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). Das Client-SDK für die Spracherkennung stellt eine **SpeechRecognitionServiceFactory** der Factoryklasse dar, die folgende Methoden bietet:

  *   ```CreateDataClient(...)```: Ein Client für die Datenerkennung.
  *   ```CreateDataClientWithIntent(...)```: Ein Client für die Datenerkennung mit Intents.
  *   ```CreateMicrophoneClient(...)```: Ein Client für die Mikrofonerkennung.
  *   ```CreateMicrophoneClientWithIntent(...)```: Ein Client für die Mikrofonerkennung mit Intents.

Eine ausführliche Dokumentation finden Sie unter [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/speech/home). Die Custom Speech Service-Endpunkte unterstützen dasselbe SDK.

Der Client für die Datenerkennung eignet sich für die Spracherkennung anhand von Daten, z.B. einer Datei oder einer anderen Audioquelle. Der Client für die Mikrofonerkennung eignet sich für die Spracherkennung von Mikrofoninhalten. Durch die Verwendung von Intents in einem der Clients können strukturierte Intentergebnisse vom [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS) zurückgegeben werden, wenn Sie eine LUIS-Anwendung für Ihr Szenario erstellt haben.

Alle vier Typen von Clients können auf zwei Arten instanziiert werden. Bei der ersten Methode wird die standardmäßige Sprach-API von Cognitive Services verwendet. Bei der zweiten Methode können Sie eine URL angeben, die Ihrem benutzerdefinierten Endpunkt entspricht, der mit Custom Speech Service erstellt wurde.

Sie können z.B. einen **DataRecognitionClient** erstellen, der mithilfe der folgenden Methode Anforderungen an einen benutzerdefinierten Endpunkt sendet:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

**Ihre_Abonnement-ID** und die **endpointURL** beziehen sich auf den Abonnementschlüssel bzw. die Websocket-URL auf der Seite **Bereitstellungsinformationen**.

Der **AuthenticationUri** dient zum Empfangen eines Tokens vom Authentifizierungsdienst. Dieser URI muss wie im folgenden Beispielcode gezeigt separat festgelegt werden.

In diesem Beispielcode wird gezeigt, wie das Client-SDK verwendet wird:

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> Bei Verwendung von **Create**-Methoden im SDK, müssen Sie die Abonnement-ID aufgrund der Überladung der **Create**-Methoden zweimal angeben.
>

Bei Custom Speech Service werden für die Kurzform- und die Langformerkennung zwei verschiedene URLs verwendet. Beide werden auf der Seite **Bereitstellungen** aufgeführt. Verwenden Sie die richtige Endpunkt-URL für die jeweilige Form, die Sie verwenden möchten.

Weitere Informationen zum Aufrufen der verschiedenen Clients für die Spracherkennung mit Ihrem benutzerdefinierten Endpunkt finden Sie in der Klasse [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop). Die Dokumentation auf dieser Seite bezieht sich auf die Akustikmodellanpassung, gilt jedoch für alle Endpunkte, die mit Custom Speech Service erstellt wurden.

## <a name="send-requests-by-using-the-speech-protocol"></a>Senden von Anforderungen mithilfe des Sprachprotokolls

Bei den Endpunkten, die für das [Sprachprotokoll](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) angezeigt werden, handelt es sich um Endpunkte für das Open Source-basierte Websocketprotokoll für die Spracherkennung.

Die einzige offizielle Clientimplementierung ist derzeit für [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) bestimmt. Wenn Sie mit dem dort bereitgestellten Beispiel starten möchten, nehmen Sie die folgenden Änderungen am Code vor, und erstellen Sie das Beispiel erneut:

1. Ersetzen Sie in _src\sdk\speech.browser\SpeechConnectionFactory.ts_ den Hostnamen „wss://speech.platform.bing.com“ durch den Hostnamen, der auf der Detailseite Ihrer Bereitstellung angezeigt wird. Fügen Sie an dieser Stelle nicht den vollständigen URI ein, sondern nur das *wss*-Protokollschema und den Hostnamen. Beispiel: 

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Legen Sie den Parameter _recognitionMode_ in der Datei _samples\browser\Samples.html_ gemäß Ihren Anforderungen fest:
    * _RecognitionMode.Interactive_ unterstützt Anforderungen von bis zu 15 Sekunden.
    * _RecognitionMode.Conversation_ und _RecognitionMode.Dictation_ (beide sind äquivalent in Custom Speech Service) unterstützen Anforderungen von bis zu 10 Minuten.

3. Bevor Sie ihn verwenden, erstellen Sie mit „gulp build“ das Beispiel.

> [!NOTE]
> Stellen Sie sicher, dass Sie den richtigen URI für dieses Protokoll verwenden. Das erforderliche Schema ist *wss* (nicht *http* wie im Clientprotokoll). 

Weitere Informationen finden Sie in der Dokumentation zur [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries).

## <a name="send-requests-by-using-http"></a>Senden von Anforderungen über HTTP

Das Senden einer Anforderung an Ihren benutzerdefinierten-Endpunkt mit einer POST-Methode (HTTP) ist mit dem Senden einer Anforderung über HTTP an die Bing-Spracheingabe-API von Cognitive Services vergleichbar. Ändern Sie die URL, um die Adresse Ihrer benutzerdefinierten Bereitstellung einzuschließen.

Es gibt einige Einschränkungen hinsichtlich der Anforderungen, die über HTTP für den Cognitive Services-Spracherkennungsendpunkt gesendet werden und die benutzerdefinierten Endpunkte, die mit diesem Dienst erstellt werden. Die HTTP-Anforderung kann während des Erkennungsvorgangs keine Teilergebnisse zurückgeben. Darüber hinaus ist die Dauer der Anforderungen auf 10 Sekunden bei Audioinhalten und auf 14 Sekunden insgesamt beschränkt.

Um eine POST-Anforderung zu erstellen, führen Sie den gleichen Prozess durch, den Sie für die Sprach-API von Cognitive Services durchführen.

1. Rufen Sie mithilfe Ihrer Abonnement-ID ein Zugriffstoken ab. Dieses Token ist erforderlich, um auf den Endpunkt für die Spracherkennung zuzugreifen. Dieser kann 10 Minuten lang wiederverwendet werden.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      Die **subscriptionId** sollte auf die Abonnement-ID festgelegt werden, die Sie für diese Bereitstellung verwenden. Die Antwort, ein einfaches Token, benötigten Sie für die nächste Anforderung.

2. Veröffentlichen Sie den Audioinhalt mithilfe der POST-Methode erneut im Endpunkt.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    Das **token** ist das Zugriffstoken, das Sie beim vorherigen Aufruf erhalten haben. Der **https_endpoint** ist die vollständige Adresse Ihres benutzerdefinierten Endpunkts für die Spracherkennung, der auf der Seite **Bereitstellungsinformationen** gezeigt wird.

Weitere Informationen zu POST-Parametern (HTTP) und zum Antwortformat finden Sie unter [Cognitive Services](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Senden von Anforderungen mithilfe der Dienstbibliothek
Mithilfe der Dienstbibliothek kann Ihr Dienst die Microsoft-Cloud für Sprachtranskriptionen verwenden, um gesprochene Sprache in Echtzeit in Text umzuwandeln, damit Ihre Client-App Audiodaten senden und gleichzeitig asynchron Teilergebnisse der Spracherkennung empfangen kann. Einzelheiten zum Dienst-SDK finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary).

> [!NOTE]
> Bei der Verwendung der Dienstbibliothek müssen Sie den URI des Autorisierungsanbieters in der Implementierung von **IAuthorizationProvider** in https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken ändern.

## <a name="next-steps"></a>Nächste Schritte
* Erhöhen Sie die Genauigkeit mit Ihrem [benutzerdefinierten Akustikmodell](cognitive-services-custom-speech-create-acoustic-model.md).
* Erhöhen Sie die Genauigkeit mit einem [benutzerdefinierten Sprachmodell](cognitive-services-custom-speech-create-language-model.md).

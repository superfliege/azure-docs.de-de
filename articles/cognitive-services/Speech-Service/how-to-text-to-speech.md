---
title: Verwenden von Sprachsynthese mit Spracherkennungsdiensten
description: Erfahren Sie, wie Sie Text-to-Speech im Spracherkennungsdienst verwenden.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d19b71542f032111bbd3d9c7f3fe246110377b5d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127585"
---
# <a name="use-text-to-speech-in-speech-service"></a>Verwenden von „Sprachsynthese“ im Spracherkennungsdienst

Der Spracherkennungsdienst bietet Sprachsynthese-Funktionalität über eine einfache HTTP-Anforderung. Sie senden den zu sprechenden Text per POST-Anweisung an den entsprechenden Endpunkt, und der Dienst gibt eine Audiodatei (`.wav`) mit synthetisierter Sprache zurück. Ihre Anwendung kann dieses Audio dann beliebig verwenden.

Der Textkörper der POST-Anforderung für Sprachsynthese kann einfacher Text (ASCII oder UTF8) oder ein [SSML](speech-synthesis-markup.md)-Dokument sein. Nur-Text-Anforderungen werden mit einer Standardstimme gesprochen. In den meisten Fällen ist es ratsam, einen SSML-Textkörper zu verwenden. Die HTTP-Anforderung muss ein [Autorisierungstoken](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#authentication) enthalten. 

Hier finden Sie die regionalen Endpunkte für Sprachsynthese. Verwenden Sie den entsprechenden Endpunkt für Ihr Abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Angeben einer Stimme

Verwenden Sie zum Angeben einer Stimme das `<voice>` [SSML](speech-synthesis-markup.md)-Tag. Beispiel: 

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Eine Liste der verfügbaren Stimmen und deren Namen finden Sie unter [Stimmen für Sprachsynthese](supported-languages.md#text-to-speech).

## <a name="make-a-request"></a>Erstellen einer Anforderung

Eine Sprachsynthese-HTTP-Anforderung erfolgt im POST-Modus mit dem zu sprechenden Text im Textkörper der Anforderung. Die maximale Länge des HTTP-Anforderungstexts beträgt 1024 Zeichen. Die Anforderung muss über die folgenden Header verfügen: 

Header|Werte|Kommentare
-|-|-
|`Content-Type` | `application/ssml+xml` | Textformat der Eingabe.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | Das Audioformat der Ausgabe.
|`User-Agent`   |Anwendungsname | Der Anwendungsname ist erforderlich und muss weniger als 255 Zeichen umfassen.
| `Authorization`   | Autorisierungstoken, das durch die Bereitstellung Ihres Abonnementschlüssels an den Tokendienst abgerufen wird. Jedes Token ist 10 Minuten gültig. Weitere Informationen finden Sie unter [REST-APIs: Authentifizierung](rest-apis.md#authentication).

> [!NOTE]
> Wenn die ausgewählte Stimme und das ausgewählte Ausgabeformat unterschiedliche Bitraten aufweisen, wird das Audio nach Bedarf neu gesampelt. 24khz-Stimmen unterstützen keine `audio-16khz-16kbps-mono-siren`- und `riff-16khz-16kbps-mono-siren`-Ausgabeformate. 

Nachfolgend sehen Sie eine Beispielanforderung.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

Der Antworttext mit dem Statuscode 200 enthält Audio im angegebenen Ausgabeformat.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Bei Fehlern werden die unten aufgeführten Statuscodes verwendet. Der Antworttext des Fehlers enthält außerdem eine Beschreibung des Problems.

|Code|BESCHREIBUNG|Problem|
|-|-|-|
400 |Ungültige Anforderung |Ein erforderlicher Parameter fehlt, ist leer oder Null. Oder der an einen erforderlichen oder optionalen Parameter übergebene Wert ist ungültig. Ein häufiges Problem sind zu lange Kopfzeilen.
401|Nicht autorisiert |Die Anforderung ist nicht autorisiert. Stellen Sie sicher, dass Ihr Abonnementschlüssel oder -token gültig ist.
413|Anforderungsentität zu groß|Die SSML-Eingabe umfasst mehr als 1024 Zeichen.
|502|Ungültiges Gateway    | Netzwerk- oder serverseitiges Problem. Kann auch auf ungültige Header hinweisen.

Weitere Informationen zur REST-API für die Sprachsynthese finden Sie unter [REST-APIs](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C++](quickstart-cpp-windows.md)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
- [Erkennen von Sprache in Java](quickstart-java-android.md)

---
title: Sprachsynthese-API-Referenz (REST) – Sprachdienste
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Sprachsynthese-REST-API verwenden. In diesem Artikel erfahren Sie mehr über Autorisierungs- und Abfrageoptionen sowie darüber, wie Sie eine Anforderung strukturieren und eine Antwort erhalten.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: d67d8462c177d19dfa3cebbd0b4b000fbe3f41b8
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2019
ms.locfileid: "57894955"
---
# <a name="text-to-speech-rest-api"></a>Text-to-Speech-REST-API

Mit den Sprachdiensten können Sie jetzt Sprachsynthese mit einer REST-API durchführen. Jeder zugängliche Endpunkt ist einer Region zugeordnet. Ihre Anwendung benötigt einen Abonnementschlüssel für den Endpunkt, den Sie verwenden möchten. Die Text-to-Speech-REST-API unterstützt neuronale und Standardstimmen für die Sprachsynthese. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert.

* Eine vollständige Liste der Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).
* Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#text-to-speech).

> [!IMPORTANT]
> Die Kosten sind für Standardstimmen, benutzerdefinierte und neuronale Stimmen unterschiedlich. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

Bevor Sie diese API verwenden, müssen Sie Folgendes wissen:

* Die Text-to-Speech-REST-API erfordert einen Autorisierungsheader. Das bedeutet, dass Sie einen Tokenaustausch ausführen müssen, um auf den Dienst zuzugreifen. Weitere Informationen finden Sie unter [Authentifizierung](#authentication).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="request-headers"></a>Anforderungsheader

Diese Tabelle führt die erforderlichen und optionalen Header für Spracherkennungsanforderungen auf.

| Header | BESCHREIBUNG | Erforderlich/optional |
|--------|-------------|---------------------|
| `Authorization` | Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Weitere Informationen finden Sie unter [Authentifizierung](#authentication). | Erforderlich |
| `Content-Type` | Gibt den Inhaltstyp des angegebenen Texts an. Zulässiger Wert: `application/ssml+xml`. | Erforderlich |
| `X-Microsoft-OutputFormat` | Gibt das Audioausgabeformat an. Eine vollständige Liste der zulässigen Werte finden Sie unter [Audioausgaben](#audio-outputs). | Erforderlich |
| `User-Agent` | Der Name der Anwendung. Der angegebene Wert muss kürzer als 255 Zeichen sein. | Erforderlich |

## <a name="audio-outputs"></a>Audioausgaben

Dies ist eine Liste der unterstützten Audioformate, die in jeder Anforderung als `X-Microsoft-OutputFormat`-Header gesendet werden. Es wird jeweils eine Bitrate und ein Codierungstyp angegeben. Der Speech-Dienst unterstützt Audioausgaben mit 24kHz, 16kHz und 8kHz.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-alaw` | `riff-8khz-8bit-mono-mulaw` |
| `riff-16khz-16bit-mono-pcm` | `audio-16khz-128kbitrate-mono-mp3` |
| `audio-16khz-64kbitrate-mono-mp3` | `audio-16khz-32kbitrate-mono-mp3` |
| `raw-24khz-16bit-mono-pcm` | `riff-24khz-16bit-mono-pcm` |
| `audio-24khz-160kbitrate-mono-mp3` | `audio-24khz-96kbitrate-mono-mp3` |
| `audio-24khz-48kbitrate-mono-mp3` | |

> [!NOTE]
> Wenn die ausgewählte Stimme und das ausgewählte Ausgabeformat unterschiedliche Bitraten aufweisen, wird das Audio nach Bedarf neu gesampelt. 24-KHz-Stimmen unterstützen keine `audio-16khz-16kbps-mono-siren`- und `riff-16khz-16kbps-mono-siren`-Ausgabeformate.

## <a name="request-body"></a>Anforderungstext

Der Text jeder `POST`-Anforderung wird als [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) gesendet. SSML ermöglicht es Ihnen, die Stimme und Sprache der synthetisierten Sprachausgabe auszuwählen, die vom Text-zu-Sprache-Dienst zurückgegeben wird. Eine vollständige Liste der unterstützten Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

> [!NOTE]
> Wenn Sie eine benutzerdefinierte Stimme verwenden, kann der Text der Anforderung als Nur-Text (ASCII oder UTF-8) gesendet werden.

## <a name="sample-request"></a>Beispiel für eine Anforderung

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

In unseren Schnellstarts finden Sie sprachspezifische Beispiele:

* [.NET Core, C#](quickstart-dotnet-text-to-speech.md)
* [Python](quickstart-python-text-to-speech.md)
* [Node.js](quickstart-nodejs-text-to-speech.md)

## <a name="http-status-codes"></a>HTTP-Statuscodes

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

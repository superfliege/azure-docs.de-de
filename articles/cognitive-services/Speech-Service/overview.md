---
title: Worum handelt es sich beim Speech-Dienst?
titleSuffix: Azure Cognitive Services
description: 'Der Speech-Dienst, ein Teil von Azure Cognitive Services, vereinigt in sich mehrere Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (umfasst die Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: e86adfd4e832e6b9514e4813ddd4a942b07ca624
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336593"
---
# <a name="what-is-speech-services"></a>Was ist der Speech-Dienst?

Der Speech-Dienst basiert genau wie die anderen Azure-Sprachdienste auf den Sprachtechnologien aus Produkten wie Cortana und Microsoft Office.

Der Speech-Dienst vereint in sich die Azure-Sprachfeatures, die zuvor über die Dienste [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Sprachübersetzung](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) und [Custom Voice](http://customvoice.ai/) verfügbar waren. Nun kann über ein einzelnes Abonnement auf alle diese Funktionen zugegriffen werden.

## <a name="main-speech-services-functions"></a>Hauptfunktionen des Speech-Diensts

Zu den Hauptfunktionen des Speech-Diensts zählen Sprache-zu-Text (auch Spracherkennung oder Transkription genannt), Text-zu-Sprache (Sprachsynthese) und Sprachübersetzung.

|Funktion|Features|
|-|-|
|[Spracherkennung](speech-to-text.md)| <li>Wandelt fortlaufende Sprache in Echtzeit in Text um.<li>Kann Sprache aus Audioaufnahmen als Batch transkribieren. <li>Unterstützt Zwischenergebnisse, die Erkennung des Endes der Spracheingabe, automatische Textformatierung sowie die Maskierung anstößiger Ausdrücke. <li>Kann mithilfe von [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) die Benutzerabsicht aus der transkribierten Sprache ableiten.\*|
|[Text-zu-Sprache](text-to-speech.md)| <li>**NEU**: Neuronale Text-zu-Sprache-Stimmen, die von menschlicher Sprache nahezu kaum zu unterscheiden sind (in Englisch). <li>Konvertiert Text in natürlich klingende Sprache. <li>Bietet mehrere Geschlechter und/oder Dialekte für viele unterstützte Sprachen. <li>Unterstützt Nur-Text-Eingabe und SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese). |
|[Sprachübersetzung](speech-translation.md)| <li>Übersetzt Audiostreams nahezu in Echtzeit.<li> Kann auch aufgezeichnete Sprache verarbeiten.<li>Liefert Ergebnisse als Text oder Sprachsynthese. |


## <a name="customize-speech-features"></a>Anpassen von Sprachfeatures

Sie können die Modelle, die den Texterkennungs- und Sprachsynthesefeatures des Speech-Diensts zugrunde liegen, mit Ihren eigenen Daten trainieren.

|Feature|Modell|Zweck|
|-|-|-|
|Spracherkennung|[Akustikmodell](how-to-customize-acoustic-models.md)|Hilfreich beim Transkribieren bestimmter Sprecher und Umgebungen (beispielsweise Autos oder Produktionsstätten).|
||[Sprachmodell](how-to-customize-language-model.md)|Hilfreich beim Transkribieren von spezifischem Vokabular und spezifischer Grammatik (beispielsweise aus dem Medizin- oder IT-Bereich).|
||[Aussprachemodell](how-to-customize-pronunciation.md)|Hilfreich beim Transkribieren von Abkürzungen und Akronymen (beispielsweise „IOU“ statt „i owe you“). |
|Text-zu-Sprache|[Voicefont](how-to-customize-voice-font.md)|Verleiht Ihrer App eine eigene Stimme, indem das Modell mithilfe von Beispielen für menschliche Sprache trainiert wird.|

Sie können Ihre benutzerdefinierten Modelle überall dort verwenden, wo sonst in den Spracherkennungs- oder Sprachsynthesefunktionen Ihrer App die Standardmodelle zur Anwendung kommen.

## <a name="use-the-speech-service"></a>Verwenden des Speech-Diensts

Um die Entwicklung sprachaktivierter Anwendungen zu vereinfachen, stellt Microsoft das [Speech SDK](speech-sdk.md) für den Speech-Dienst bereit. Das Speech SDK bietet konsistente native Spracherkennungs- und Sprachübersetzungs-APIs für C#, C++ und Java. Wenn Sie eine dieser Sprachen verwenden, kümmert sich das Speech SDK für Sie um die Netzwerkdetails und vereinfacht so die Entwicklungsarbeit.

Der Speech-Dienst verfügt auch über eine [REST-API](rest-apis.md), die mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen beherrscht. Die REST-Schnittstelle bietet nicht die Streaming-/Echtzeitfunktionen des SDK.

|<br>Methode|Spracheingabe<br>Spracherkennung|Sprachsynthese<br>Spracheingabe|Spracheingabe<br>Sprachübersetzung|<br>BESCHREIBUNG|
|-|-|-|-|-|
|[Speech SDK](speech-sdk.md)|JA|Nein|JA|Native APIs für C#, C++ und Java zur Vereinfachung der Entwicklung.|
|[REST-APIs](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, mit der Sie Ihren Anwendungen problemlos Sprachfunktionen hinzufügen können.|

### <a name="websockets"></a>WebSockets

Der Speech-Dienst unterstützt auch WebSocket-Protokolle für eine streamingbasierte Spracherkennung und Sprachübersetzung. Die Speech SDKs verwenden diese Protokolle, um mit dem Speech-Dienst zu kommunizieren. Verwenden Sie das Speech SDK, anstatt eine eigene WebSocket-Kommunikation mit dem Speech-Dienst zu implementieren.

Falls Sie jedoch bereits über Code verfügen, der die Bing-Spracheingabe oder die Sprachübersetzung über WebSockets verwendet, können Sie ihn für die Verwendung des Speech-Diensts aktualisieren. Die WebSocket-Protokolle sind kompatibel, aber die Endpunkte unterscheiden sich.

### <a name="speech-devices-sdk"></a>Speech-Geräte-SDK

Das [SDK für sprachaktivierte Geräte](speech-devices-sdk.md) ist eine integrierte Hardware- und Softwareplattform für Entwickler sprachaktivierter Geräte. Unsere Hardwarepartner stellen Referenzentwürfe und Entwicklungseinheiten bereit. Microsoft bietet ein geräteoptimiertes SDK, das die Funktionen der Hardware optimal ausnutzt.


## <a name="speech-scenarios"></a>Speech-Szenarien

Der Speech-Dienst eignet sich unter anderem für folgende Anwendungsfälle:

> [!div class="checklist"]
> * Erstellen von sprachgesteuerten Apps
> * Transkribieren von Callcenteraufzeichnungen
> * Implementieren von Sprachbots

### <a name="voice-user-interface"></a>Benutzeroberfläche mit Sprachsteuerung

Die Spracheingabe ist eine hervorragende Möglichkeit, um Ihre App flexibel, schnell und ohne Verwendung der Hände einsetzen zu können. In einer sprachgesteuerten App können Benutzer einfach nach den gewünschten Informationen fragen.

Wenn Ihre App für die Allgemeinheit vorgesehen ist, können Sie die standardmäßigen Spracherkennungsmodelle verwenden. Sie erkennen ein breites Spektrum an Sprechern in gängigen Umgebungen.

Wenn Ihre App in einem bestimmten Bereich verwendet wird (z.B. Medizin oder IT), können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen. Mit diesem Modell können Sie den Speech-Dienst bezüglich der speziellen Terminologie Ihrer App trainieren.

Wenn Ihre App in einer lauten Umgebung (beispielsweise einer Fabrik) verwendet wird, können Sie ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) erstellen. Dieses Modell unterstützt den Speech-Dienst bei der Unterscheidung zwischen Sprache und Störgeräuschen.

### <a name="call-center-transcription"></a>Callcentertranskription

Callcenteraufzeichnungen werden meist nur dann zu Rate gezogen, wenn ein Problem mit einem Anruf auftritt. Mit dem Speech-Dienst ist es einfach, jede Aufzeichnung in Text zu transkribieren. Sie können den Text ganz einfach für die [Volltextsuche](https://docs.microsoft.com/azure/search/search-what-is-azure-search) indizieren oder [Textanalysen](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) anwenden, um die Stimmung, die Sprache und Schlüsselausdrücke zu erkennen.

Wenn Ihre Callcenteraufzeichnungen spezielle Terminologie (beispielsweise Produktnamen oder IT-Jargon) enthalten, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst dieses Vokabular beizubringen. Ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) hilft dem Speech-Dienst dabei, schlechte Telefonverbindungen besser zu verstehen.

Für weitere Informationen zu diesem Szenario lesen Sie mehr über [Batchtranskripte](batch-transcription.md) im Speech-Dienst.

### <a name="voice-bots"></a>Sprachbots

[Bots](https://dev.botframework.com/) stellen eine beliebte Möglichkeit dar, Benutzern die gewünschten Informationen bereitzustellen und Kunden an Unternehmen zu binden. Wenn Sie Ihrer Website oder App eine dialogorientierte Benutzeroberfläche hinzufügen, sind die Funktionen einfacher zu finden und schneller zugänglich. Mit dem Speech-Dienst wird diese Konversation erheblich flüssiger, da auf gesprochene Anfragen eine gesprochene Antwort gegeben wird.

Damit Ihr Sprachbot eine einzigartige Persönlichkeit erhält, können Sie eine eigene Stimme verwenden. Das Erstellen einer benutzerdefinierte Sprache umfasst zwei Schritte. [Zeichnen Sie zuerst die Stimme auf](record-custom-voice-samples.md), die Sie verwenden möchten. [Übermitteln Sie diese Aufzeichnungen](how-to-customize-voice-font.md) anschließend (zusammen mit einem Transkript des Texts) an das [Sprachanpassungs-Portal](https://cris.ai/Home/CustomVoice) des Speech-Diensts, der den Rest übernimmt. Nachdem Sie Ihre benutzerdefinierte Stimme erstellt haben, sind die Schritte zur Verwendung in Ihrer App denkbar einfach.

## <a name="next-steps"></a>Nächste Schritte

Abrufen eines Abonnementschlüssels für den Speech-Dienst.

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

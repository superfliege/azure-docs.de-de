---
title: Worum handelt es sich beim Speech-Dienst (Vorschau)?
description: 'Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 922320bb0b880e933b27025257e6a533fe257680
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091471"
---
# <a name="what-is-the-speech-service"></a>Worum handelt es sich beim Speech-Dienst?

Der Speech-Dienst vereint die Azure-Sprachfeatures, die zuvor über die Dienste [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Sprachübersetzung](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) und [Custom Voice](http://customvoice.ai/) verfügbar waren. Nun kann über ein einzelnes Abonnement auf alle diese Funktionen zugegriffen werden.

Der Speech-Dienst basiert genau wie die anderen Azure-Sprachdienste auf den bewährten Sprachtechnologien aus Produkten wie Cortana und Microsoft Office. Sie können auf die Qualität der Ergebnisse und auf die Zuverlässigkeit der Azure-Cloud zählen.

> [!NOTE]
> Der Speech-Dienst ist derzeit als öffentliche Vorschauversion verfügbar. Hier finden Sie regelmäßig Aktualisierungen zur Dokumentation, neue Codebeispiele und vieles mehr.

## <a name="main-speech-service-functions"></a>Hauptfunktionen des Speech-Diensts

Zu den Hauptfunktionen des Speech-Diensts zählen Sprache-zu-Text (auch Spracherkennung oder Transkription genannt), Text-zu-Sprache (Sprachsynthese) und Sprachübersetzung.

|Funktion|Features|
|-|-|
|[Spracherkennung](speech-to-text.md)| <ul><li>Wandelt fortlaufende Sprache in Echtzeit in Text um.<li>Kann Sprache aus Audioaufnahmen als Batch transkribieren. <li>Bietet Erkennungsmodi für interaktive Anwendungsfälle sowie für Konversationen und Diktate.<li>Unterstützt Zwischenergebnisse, die Erkennung des Endes der Spracheingabe, automatische Textformatierung sowie die Maskierung anstößiger Ausdrücke. <li>Kann mithilfe von [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) die Benutzerabsicht aus der transkribierten Sprache ableiten.\*|
|[Sprachsynthese](text-to-speech.md)| <ul><li>Konvertiert Text in natürlich klingende Sprache. <li>Bietet mehrere Geschlechter und/oder Dialekte für viele unterstützte Sprachen. <li>Unterstützt Nur-Text-Eingabe und SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese). |
|[Sprachübersetzung](speech-translation.md)| <ul><li>Übersetzt Audiostreams nahezu in Echtzeit<li> Kann auch aufgezeichnete Sprache verarbeiten<li>Liefert Ergebnisse als Text oder Sprachsynthese. |

\* *Für die Absichtserkennung ist ein LUIS-Abonnement erforderlich.*


## <a name="customizing-speech-features"></a>Anpassen von Sprachfeatures

Der Speech-Dienst ermöglicht es Ihnen, die Modelle, die den Texterkennungs- und Sprachsynthesefeatures des Speech-Diensts zugrunde liegen, mit Ihren eigenen Daten zu trainieren. 

|Feature|Modell|Zweck|
|-|-|-|
|Spracherkennung|[Akustikmodell](how-to-customize-acoustic-models.md)|Hilfreich beim Transkribieren bestimmter Sprecher und Umgebungen (beispielsweise Autos oder Produktionsstätten)|
||[Sprachmodell](how-to-customize-language-model.md)|Hilfreich beim Transkribieren von spezifischem Vokabular und spezifischer Grammatik (beispielsweise aus dem Medizin- oder IT-Bereich)|
||[Aussprachemodell](how-to-customize-pronunciation.md)|Hilfreich beim Transkribieren von Abkürzungen und Akronymen (beispielsweise „IOU“ statt „i oh you“) |
|Sprachsynthese|[Voicefont](how-to-customize-voice-font.md)|Verleiht Ihrer App eine eigene Stimme, indem das Modell mithilfe von Beispielen für menschliche Sprache trainiert wird.|

Nach der Erstellung Ihrer benutzerdefinierten Modelle können Sie diese überall dort verwenden, wo sonst in den Spracherkennungs- oder Sprachsynthesefunktionen Ihrer App die Standardmodelle zur Anwendung kommen.


## <a name="using-the-speech-service"></a>Verwenden des Speech-Diensts

Für den neuen Speech-Dienst stellt Microsoft das [Speech SDK](speech-sdk.md) bereit, um die Entwicklung sprachaktivierter Anwendungen zu vereinfachen. Das Speech SDK bietet konsistente native Spracherkennungs- und Sprachübersetzungs-APIs für C#, C++ und Java. Wenn Sie eine dieser Sprachen verwenden, kümmert sich das Speech SDK für Sie um die Netzwerkdetails und vereinfacht so die Entwicklungsarbeit.

Der Speech-Dienst verfügt auch über eine [REST-API](rest-apis.md), die mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen beherrscht. Die REST-Schnittstelle bietet jedoch nicht die Streaming-/Echtzeitfunktionen des SDK.

|<br>Methode|Spracheingabe<br>Spracherkennung|Sprachsynthese<br>Spracheingabe|Spracheingabe<br>Sprachübersetzung|<br>Beschreibung|
|-|-|-|-|-|
|[Speech SDK](speech-sdk.md)|JA|Nein|JA|Native APIs für C#, C++ und Java zur Vereinfachung der Entwicklung.|
|[REST](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, mit der Sie Ihren Anwendungen problemlos Sprachfunktionen hinzufügen können.|

### <a name="websockets"></a>WebSockets

Der Speech-Dienst verfügt auch über WebSockets-Protokolle für eine streamingbasierte Spracherkennung und Sprachübersetzung. Die Speech SDKs verwenden diese Protokolle, um mit dem Speech-Dienst zu kommunizieren. Es empfiehlt sich, das Speech SDK zu verwenden, anstatt eine eigene WebSockets-Kommunikation mit dem Speech-Dienst zu implementieren.

Falls Sie jedoch bereits über Code verfügen, der die Bing-Spracheingabe oder die Sprachübersetzung über WebSockets verwendet, können Sie ihn ganz einfach für die Verwendung des Speech-Diensts aktualisieren. Die WebSockets-Protokolle sind kompatibel. Nur die Endpunkte unterscheiden sich.

### <a name="speech-devices-sdk"></a>Speech-Geräte-SDK

Das [SDK für sprachaktivierte Geräte](speech-devices-sdk.md) ist eine integrierte Hardware- und Softwareplattform für Entwickler sprachaktivierter Geräte. Unsere Hardwarepartner stellen Referenzentwürfe und Entwicklungseinheiten bereit. Microsoft bietet ein geräteoptimiertes SDK, das die Funktionen der Hardware optimal ausnutzt.


## <a name="speech-scenarios"></a>Speech-Szenarien

Der Speech-Dienst eignet sich unter anderem für folgende Anwendungsfälle:

> [!div class="checklist"]
> * Erstellen von sprachgesteuerten Apps
> * Transkribieren von Callcenteraufzeichnungen
> * Implementieren von Sprachbots

### <a name="voice-user-interface"></a>Benutzeroberfläche mit Sprachsteuerung

Die Spracheingabe ist eine hervorragende Möglichkeit, um Ihre App flexibel, schnell und ohne Verwendung der Hände einsetzen zu können. In einer sprachgesteuerten App können Benutzer einfach nach den gewünschten Informationen fragen, anstatt dorthin zu navigieren.

Wenn Ihre App für die Allgemeinheit vorgesehen ist, können Sie die standardmäßigen Spracherkennungsmodelle verwenden. Sie erkennen zuverlässig ein breites Spektrum an Sprechern in gängigen Umgebungen.

Wenn Ihre App in einer bestimmten Domäne (z.B. Medizin oder IT) verwendet wird, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst besondere Terminologie beizubringen, die von Ihrer App verwendet wird.

Wenn Ihre App in einer lauten Umgebung, wie z.B. einer Fabrik, verwendet werden soll, können Sie ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) erstellen, damit der Speech-Dienst Sprache besser von Hintergrundgeräuschen unterscheiden kann.

Für den Einstieg laden Sie einfach das [Speech SDK](speech-sdk.md) herunter und lesen den entsprechenden [Schnellstart](quickstart-csharp-dotnet-windows.md)-Artikel.

### <a name="call-center-transcription"></a>Callcentertranskription

Callcenteraufzeichnungen werden meist nur dann verwendet, wenn ein Problem mit einem Anruf auftritt. Mit dem Speech-Dienst ist es einfach, jede Aufzeichnung in Text zu transkribieren. Wenn der Text verfügbar ist, können Sie ihn ganz einfach für die [Volltextsuche](https://docs.microsoft.com/azure/search/search-what-is-azure-search) indizieren oder [Textanalysen](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) anwenden, um die Stimmung, die Sprache und Schlüsselausdrücke zu erkennen.

Wenn Ihre Callcenteraufzeichnungen spezielle Terminologie (beispielsweise Produktnamen oder IT-Jargon) enthalten, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst dieses Vokabular beizubringen. Ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) hilft dem Speech-Dienst dabei, schlechte Telefonverbindungen besser zu verstehen.

Für weitere Informationen zu diesem Szenario lesen Sie mehr über [Batchtranskripte](batch-transcription.md) im Speech-Dienst.

### <a name="voice-bots"></a>Sprachbots

[Bots](https://dev.botframework.com/) stellen eine zunehmend beliebtere Möglichkeit dar, Benutzern die gewünschten Informationen bereitzustellen und Kunden an das Unternehmen zu binden. Das Hinzufügen einer Benutzeroberfläche für die Konversation auf Ihrer Website oder in Ihrer App vereinfacht und beschleunigt das Auffinden der Funktionen. Mit dem Speech-Dienst wird diese Konversation erheblich flüssiger, da auf gesprochene Anfragen eine gesprochene Antwort gegeben wird.

Damit Ihr Sprachbot eine einzigartige Persönlichkeit erhält (und Ihr Branding stärkt), können Sie eine eigene Stimme verwenden. Das Erstellen einer benutzerdefinierte Sprache umfasst zwei Schritte. Zuerst [zeichnen Sie die Stimme auf](record-custom-voice-samples.md), die Sie verwenden möchten. Anschließend [übermitteln Sie diese Aufzeichnungen](how-to-customize-voice-font.md) (zusammen mit einem Transkript des Texts) an das [Sprachanpassungs-Portal](https://cris.ai/Home/CustomVoice) des Speech-Diensts, der den Rest übernimmt. Nachdem Sie Ihre benutzerdefinierte Sprache erstellt haben, ist deren Anwendung in Ihrer App ein Kinderspiel.

## <a name="next-steps"></a>Nächste Schritte

Abrufen eines Abonnementschlüssels für den Speech-Dienst

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

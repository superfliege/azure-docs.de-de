---
title: Worum handelt es sich beim Speech-Dienst?
description: 'Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: b014144dd819aa354596175068bfb2c5905a4377
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857991"
---
# <a name="what-is-the-speech-service"></a>Worum handelt es sich beim Speech-Dienst?


Der Speech-Dienst basiert genau wie die anderen Azure-Sprachdienste auf den Sprachtechnologien aus Produkten wie Cortana und Microsoft Office.

Der Speech-Dienst vereint die Azure-Sprachfeatures, die zuvor über die Dienste [Bing-Spracheingabe-API](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Sprachübersetzung](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Custom Speech](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) und [Custom Voice](http://customvoice.ai/) verfügbar waren. Nun kann über ein einzelnes Abonnement auf alle diese Funktionen zugegriffen werden.

## <a name="main-speech-service-functions"></a>Hauptfunktionen des Speech-Diensts

Zu den Hauptfunktionen des Speech-Diensts zählen Sprache-zu-Text (auch Spracherkennung oder Transkription genannt), Text-zu-Sprache (Sprachsynthese) und Sprachübersetzung.

|Funktion|Features|
|-|-|
|[Spracherkennung](speech-to-text.md)| <ul><li>Wandelt fortlaufende Sprache in Echtzeit in Text um.<li>Kann Sprache aus Audioaufnahmen als Batch transkribieren. <li>Unterstützt Zwischenergebnisse, die Erkennung des Endes der Spracheingabe, automatische Textformatierung sowie die Maskierung anstößiger Ausdrücke. <li>Kann mithilfe von [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) die Benutzerabsicht aus der transkribierten Sprache ableiten.\*|
|[Sprachsynthese](text-to-speech.md)| <ul><li>Konvertiert Text in natürlich klingende Sprache. <li>Bietet mehrere Geschlechter und/oder Dialekte für viele unterstützte Sprachen. <li>Unterstützt Nur-Text-Eingabe und SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese). |
|[Sprachübersetzung](speech-translation.md)| <ul><li>Übersetzt Audiostreams nahezu in Echtzeit.<li> Kann auch aufgezeichnete Sprache verarbeiten.<li>Liefert Ergebnisse als Text oder Sprachsynthese. |

\* *Für die Absichtserkennung ist ein LUIS-Abonnement erforderlich.*

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
|[REST](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, mit der Sie Ihren Anwendungen problemlos Sprachfunktionen hinzufügen können.|

### <a name="websockets"></a>WebSockets

Der Speech-Dienst verfügt auch über WebSocket-Protokolle für eine streamingbasierte Spracherkennung und Sprachübersetzung. Die Speech SDKs verwenden diese Protokolle, um mit dem Speech-Dienst zu kommunizieren. Verwenden Sie das Speech SDK, anstatt eine eigene WebSocket-Kommunikation mit dem Speech-Dienst zu implementieren.

Falls Sie jedoch bereits über Code verfügen, der die Bing-Spracheingabe oder die Sprachübersetzung über WebSockets verwendet, können Sie ihn für die Verwendung des Speech-Diensts aktualisieren. Die WebSocket-Protokolle sind kompatibel. Nur die Endpunkte unterscheiden sich.

### <a name="speech-devices-sdk"></a>Speech-Geräte-SDK

Das [SDK für sprachaktivierte Geräte](speech-devices-sdk.md) ist eine integrierte Hardware- und Softwareplattform für Entwickler sprachaktivierter Geräte. Unsere Hardwarepartner stellen Referenzentwürfe und Entwicklungseinheiten bereit. Microsoft bietet ein geräteoptimiertes SDK, das die Funktionen der Hardware optimal ausnutzt.

## <a name="why-move-to-the-speech-service"></a>Gründe für die Umstellung auf den Speech-Dienst

Mit dem Speech-Dienst wird nicht nur die gesamte Funktionalität der Bing-Spracheingabe-API bereitgestellt, sondern darüber hinaus drei weitere Azure-Sprachdienste: Custom Speech, Custom Voice und Sprachübersetzung. Wir raten Benutzern dieser Dienste, die Migration zum Speech-Dienst durchzuführen.

Mit dem Speech-Dienst werden viele Upgrades in diese anderen Dienste eingebunden, z.B.:

* Höhere Genauigkeit bei der Spracherkennung. Wir verbessern regelmäßig die Modelle, die im Dienst verwendet werden.

* Höhere Skalierbarkeit. Der Dienst kann mehrere gleichzeitige Anforderungen besser verarbeiten, sodass die Latenz verringert wird.

* Für den Speech-Dienst wird ein zeitbasiertes Preismodell verwendet. Ausführlichere Informationen finden Sie unter [Cognitive Services-Preise – Sprachdienste](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

* Der Speech-Dienst ist in [mehreren Regionen](regions.md) verfügbar, um die Anforderungen der Kunden weltweit zu erfüllen. Sie benötigen ein [Azure-Abonnement](https://docs.microsoft.com/azure/cognitive-services/welcome) für jede Region, die von Ihrer Anwendung verwendet wird.

* Mit einem einzelnen Abonnementschlüssel für den Speech-Dienst wird Zugriff auf die folgenden Features gewährt. Da die Abrechnung jeweils separat erfolgt, werden Ihnen nur die genutzten Features berechnet.

    * [Spracherkennung](speech-to-text.md)
    * [Benutzerdefinierte Spracherkennung](https://cris.ai/CustomSpeech)
    * [Sprachsynthese](text-to-speech.md)
    * [Benutzerdefinierte Stimmen für Sprachsynthese](https://cris.ai/CustomVoice)
    * [Sprachübersetzung](speech-translation.md) (ohne [Textübersetzung](https://docs.microsoft.com/azure/cognitive-services/translator/translator-info-overview))

* Die Spracherkennungsfunktion des Speech-Diensts wird in den [Language Understanding Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) integriert, um die Absicht des Sprechers zu erkennen. Auch ein LUIS-Endpunktschlüssel kann mit dem Speech-Dienst verwendet werden. Ausführlichere Informationen finden Sie unter [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md).

* Für die Spracherkennung ist es nicht mehr erforderlich, einen Erkennungsmodus anzugeben.

* Der Speech-Dienst unterstützt 24-KHz-Stimmen für die Sprachsynthese, um die Audioqualität zu verbessern. Zum Zeitpunkt der Erstellung dieses Texts sind zwei Stimmen verfügbar (nur „Englisch USA“): `Jessa24kRUS` und `Guy24kRUS`.

* Die [Batch-Transkription](batch-transcription.md) des Speech-Diensts ermöglicht es, dass große Mengen von aufgezeichneter Sprache, z.B. Callcenter-Aufzeichnungen, effizient in Text transkribiert werden, damit dieser leicht analysiert und durchsucht werden kann.

* Beim Verwenden des Speech SDK gilt kein Zeitlimit für das Streamen der Transkription für die Spracherkennung.

* Das [Speech SDK](speech-sdk.md) stellt für den Speech-Dienst über mehrere Programmiersprachen und Ausführungsumgebungen (z.B. Windows 10, UWP und .NET Core) hinweg eine einheitliche API bereit, um die Entwicklung zu vereinfachen. Dies gilt vor allem bei Verwendung von mehreren Plattformen.

* Der Speech-Dienst ist mit den REST-APIs und dem WebSockets-Protokoll kompatibel, die von anderen Azure-Sprachdiensten genutzt werden, sodass es einfach ist, vorhandene Clientanwendungen zum Speech-Dienst zu migrieren.

## <a name="speech-scenarios"></a>Speech-Szenarien

Der Speech-Dienst eignet sich unter anderem für folgende Anwendungsfälle:

> [!div class="checklist"]
> * Erstellen von sprachgesteuerten Apps
> * Transkribieren von Callcenteraufzeichnungen
> * Implementieren von Sprachbots

### <a name="voice-user-interface"></a>Benutzeroberfläche mit Sprachsteuerung

Die Spracheingabe ist eine hervorragende Möglichkeit, um Ihre App flexibel, schnell und ohne Verwendung der Hände einsetzen zu können. In einer sprachgesteuerten App können Benutzer einfach nach den gewünschten Informationen fragen.

Wenn Ihre App für die Allgemeinheit vorgesehen ist, können Sie die standardmäßigen Spracherkennungsmodelle verwenden. Sie erkennen ein breites Spektrum an Sprechern in gängigen Umgebungen.

Wenn Ihre App in einem bestimmten Bereich verwendet wird (z.B. Medizin oder IT), können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen. Mit diesem Modell können Sie den Speech Dienst bezüglich der speziellen Terminologie Ihrer App trainieren.

Wenn Ihre App in einer lauten Umgebung (beispielsweise einer Fabrik) verwendet wird, können Sie ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) erstellen. Dieses Modell unterstützt den Speech-Dienst bei der Unterscheidung von Sprache von Störgeräuschen.

Der Anfang ist denkbar einfach. Laden Sie das [Speech SDK](speech-sdk.md) einfach herunter, und befolgen Sie die Anweisungen im entsprechenden [Schnellstart](quickstart-csharp-dotnet-windows.md)-Artikel.

### <a name="call-center-transcription"></a>Callcentertranskription

Callcenteraufzeichnungen werden meist nur dann zu Rate gezogen, wenn ein Problem mit einem Anruf auftritt. Mit dem Speech-Dienst ist es einfach, jede Aufzeichnung in Text zu transkribieren. Sie können den Text ganz einfach für die [Volltextsuche](https://docs.microsoft.com/azure/search/search-what-is-azure-search) indizieren oder [Textanalysen](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) anwenden, um die Stimmung, die Sprache und Schlüsselausdrücke zu erkennen.

Wenn Ihre Callcenteraufzeichnungen spezielle Terminologie (beispielsweise Produktnamen oder IT-Jargon) enthalten, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst dieses Vokabular beizubringen. Ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) hilft dem Speech-Dienst dabei, schlechte Telefonverbindungen besser zu verstehen.

Für weitere Informationen zu diesem Szenario lesen Sie mehr über [Batchtranskripte](batch-transcription.md) im Speech-Dienst.

### <a name="voice-bots"></a>Sprachbots

[Bots](https://dev.botframework.com/) stellen eine beliebte Möglichkeit dar, Benutzern die gewünschten Informationen bereitzustellen und Kunden an Unternehmen zu binden. Wenn Sie Ihrer Website oder App eine dialogorientierte Benutzeroberfläche hinzufügen, sind die Funktionen einfacher zu finden und schneller zugänglich. Mit dem Speech-Dienst wird diese Konversation erheblich flüssiger, da auf gesprochene Anfragen eine gesprochene Antwort gegeben wird.

Damit Ihr Sprachbot eine einzigartige Persönlichkeit erhält, können Sie eine eigene Stimme verwenden. Das Erstellen einer benutzerdefinierte Sprache umfasst zwei Schritte. [Zeichnen Sie zuerst die Stimme auf](record-custom-voice-samples.md), die Sie verwenden möchten. [Übermitteln Sie diese Aufzeichnungen](how-to-customize-voice-font.md) anschließend (zusammen mit einem Transkript des Texts) an das [Sprachanpassungs-Portal](https://cris.ai/Home/CustomVoice) des Speech-Diensts, der den Rest übernimmt. Nachdem Sie Ihre benutzerdefinierte Stimme erstellt haben, sind die Schritte zur Verwendung in Ihrer App denkbar einfach.

## <a name="next-steps"></a>Nächste Schritte

Abrufen eines Abonnementschlüssels für den Speech-Dienst

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

---
title: Worum handelt es sich beim Speech-Dienst (Vorschau)?
description: 'Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 60ff2f71766a14af17ebb1cb9d20825976471296
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "41929754"
---
# <a name="what-is-the-speech-service-preview"></a>Worum handelt es sich beim Speech-Dienst (Vorschau)?

Mit nur einem Abonnement erhalten Entwickler mit dem Speech-Dienst die Möglichkeit, ihre Anwendungen auf einfache Weise um leistungsfähige Sprachfunktionen zu erweitern. Ihre Apps können jetzt Sprachbefehle, Transkription, Diktatfunktion, Sprachsynthese und Sprachübersetzung umfassen.

Der Speech-Dienst basiert auf den in anderen Microsoft-Produkten, z.B. Cortana und Microsoft Office, verwendeten Technologien.

> [!NOTE]
> Der Speech-Dienst ist derzeit als öffentliche Vorschauversion verfügbar. Auf dieser Seite finden Sie regelmäßig Aktualisierungen zur Dokumentation, zusätzliche Codebeispiele und weitere Informationen.

## <a name="speech-service-features"></a>Features des Speech-Diensts

|Funktion|BESCHREIBUNG|
|-|-|
|[Spracherkennung](speech-to-text.md)| Transkribiert Audiodatenströme in Text, der von Ihrer Anwendung als Eingabe akzeptiert werden kann. Zudem ist eine Integration in den [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) möglich, um die Benutzerabsicht aus Äußerungen abzuleiten.|
|[Sprachsynthese](text-to-speech.md)| Konvertiert einfachen Text in natürlich klingende Sprache, die für Ihre Anwendung als Audiodatei bereitgestellt wird. Es sind mehrere Stimmen, die in Bezug auf das Geschlecht oder den Akzent variieren, für viele unterstützte Sprachen verfügbar. |
|[Sprachübersetzung](speech-translation.md)| Kann verwendet werden, um Audiodatenströme nahezu in Echtzeit zu übersetzen oder um aufgezeichnete Spracheingaben zu verarbeiten. |
|Benutzerdefinierte Spracherkennung|Sie können die Spracherkennung anpassen, indem Sie eigene [akustische](how-to-customize-acoustic-models.md) und [sprachliche](how-to-customize-language-model.md) Modelle erstellen und benutzerdefinierte [Ausspracheregeln](how-to-customize-pronunciation.md) angeben. |
|[Benutzerdefinierte Sprachsynthese](how-to-customize-voice-font.md)|Sie können eigene Stimmen für die Sprachsynthese erstellen.|
|[Speech-Geräte-SDK](speech-devices-sdk.md)| Mit der Einführung des vereinheitlichten Speech-Diensts bieten Microsoft und seine Partner eine integrierte Hardware/Software-Plattform an, die für die Entwicklung von sprachaktivierten Geräten optimiert ist. |

## <a name="access-to-the-speech-service"></a>Zugreifen auf den Speech-Dienst

Der Speech-Dienst wird auf zwei Arten zur Verfügung gestellt. [Das SDK](speech-sdk.md) abstrahiert die Details der Netzwerkprotokolle, um die Entwicklung auf den unterstützten Plattformen zu vereinfachen. Die [REST-API](rest-apis.md) kann mit jeder Programmiersprache verwendet werden, bietet jedoch nicht alle mit dem SDK verfügbaren Funktionen.

|<br>Methode|Spracheingabe<br>Spracherkennung|Sprachsynthese<br>Spracheingabe|Spracheingabe<br>Sprachübersetzung|<br>BESCHREIBUNG|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|JA|Nein|JA|Bibliotheken für bestimmte Programmiersprachen nutzen ein auf Websockets basierendes Protokoll, das die Entwicklung vereinfacht.|
|[REST](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, über die sich Sprache zu Ihrer Anwendung hinzufügen lässt.|

## <a name="speech-scenarios"></a>Speech-Szenarien

Einige häufige Anwendungsmöglichkeiten der Speech-Technologie werden nachfolgend kurz beschrieben. Das [Speech SDK](speech-sdk.md) ist bei den meisten Szenarien zentraler Bestandteil.

> [!div class="checklist"]
> * Erstellen von sprachgesteuerten Apps
> * Transkribieren von Callcenteraufzeichnungen
> * Implementieren von Sprachbots

### <a name="voice-triggered-apps"></a>Sprachgesteuerte Apps

Die Spracheingabe ist eine hervorragende Möglichkeit, um Ihre App flexibel, schnell und ohne Verwendung der Hände einsetzen zu können. In einer sprachgesteuerten App können Benutzer einfach nach den gewünschten Informationen fragen, anstatt durch Klicken oder Tippen navigieren zu müssen.

Wenn Ihre App für die allgemeine Öffentlichkeit vorgesehen ist, können Sie ein grundlegendes Spracherkennungsmodell vom Speech-Dienst verwenden. Es erkennt zuverlässig eine Vielzahl von Sprechern in typischen Umgebungen.

Wenn Ihre App in einer bestimmten Domäne (z.B. Medizin oder IT) verwendet wird, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst besondere Terminologie beizubringen, die von Ihrer App verwendet wird.

Wenn Ihre App in einer lauten Umgebung, wie z.B. einer Fabrik, verwendet werden soll, können Sie ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) erstellen, damit der Speech-Dienst Sprache besser von Hintergrundgeräuschen unterscheiden kann.

Für den Einstieg laden Sie einfach das [Speech SDK](speech-sdk.md) herunter und lesen den entsprechenden [Schnellstart](quickstart-csharp-dotnet-windows.md)-Artikel.

### <a name="transcribe-call-center-recordings"></a>Transkribieren von Callcenteraufzeichnungen

Callcenteraufzeichnungen werden meist nur dann verwendet, wenn ein Problem mit einem Anruf auftritt. Mit dem Speech-Dienst ist es einfach, jede Aufzeichnung in Text zu transkribieren. Wenn der Text verfügbar ist, können Sie ihn ganz einfach für die [Volltextsuche](https://docs.microsoft.com/azure/search/search-what-is-azure-search) indizieren oder [Textanalysen](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) anwenden, um die Stimmung, die Sprache und Schlüsselausdrücke zu erkennen.

Wenn Ihre Callcenteraufzeichnungen häufig spezielle Terminologie (z.B. Produktnamen oder IT-Jargon) enthalten, können Sie ein [Sprachmodell](how-to-customize-language-model.md) erstellen, um dem Speech-Dienst dieses Vokabular beizubringen. Ein benutzerdefiniertes [Akustikmodell](how-to-customize-acoustic-models.md) hilft dem Speech-Dienst dabei, schlechte Telefonverbindungen besser zu verstehen.

Für weitere Informationen zu diesem Szenario lesen Sie mehr über [Batchtranskripte](batch-transcription.md) im Speech-Dienst.

### <a name="voice-bots"></a>Sprachbots

[Bots](https://dev.botframework.com/) stellen eine zunehmend beliebtere Möglichkeit dar, Benutzern die gewünschten Informationen bereitzustellen und Kunden an das Unternehmen zu binden. Das Hinzufügen einer Benutzeroberfläche für die Konversation auf Ihrer Website oder in Ihrer App vereinfacht und beschleunigt das Auffinden der Funktionen. Mit dem Speech-Dienst erreicht diese Konversation eine neue Dimension im Hinblick auf Sprachkompetenz, da auf gesprochene Anfragen tatsächlich mit Sprachausgabe geantwortet wird.

Damit Ihr Sprachbot eine einzigartige Persönlichkeit erhält (und Ihr Branding stärkt), können Sie eine eigene Stimme verwenden. Das Erstellen einer benutzerdefinierte Sprache umfasst zwei Schritte. Zuerst [zeichnen Sie die Stimme auf](record-custom-voice-samples.md), die Sie verwenden möchten. Anschließend [übermitteln Sie diese Aufzeichnungen](how-to-customize-voice-font.md) (zusammen mit einem Transkript des Texts) an das [Sprachanpassungs-Portal](https://cris.ai/Home/CustomVoice) des Speech-Diensts, der den Rest übernimmt. Nachdem Sie Ihre benutzerdefinierte Sprache erstellt haben, ist deren Anwendung in Ihrer App ein Kinderspiel.

## <a name="next-steps"></a>Nächste Schritte

Abrufen eines Abonnementschlüssels für den Speech-Dienst

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

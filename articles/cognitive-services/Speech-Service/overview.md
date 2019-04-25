---
title: Was sind die Azure Speech-Dienste?
titleSuffix: Azure Cognitive Services
description: Die Azure Speech-Dienste dienen zur Vereinheitlichung von Spracherkennung, Sprachsynthese und Sprachübersetzung in einem einzelnen Azure-Abonnement. Über das Speech SDK, das Speech Devices SDK oder REST-APIs können Sie Ihren Anwendungen, Tools und Geräten komfortabel Sprachfunktionen hinzufügen. Fügen Sie einem vorhandenen Chatbot Sprachfunktionen hinzu, nutzen Sie die Sprachsynthese in einer Übersetzungsanwendung, oder transkribieren Sie umfangreiche Daten aus einem Callcenter.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 04/03/2019
ms.author: erhopf
ms.openlocfilehash: 61f22568aa6e6cf04963b40ad7c47163e87b9800
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59010191"
---
# <a name="what-are-the-speech-services"></a>Was sind Speech-Dienste?

Azure Speech-Dienste dienen zur Vereinheitlichung von Spracherkennung, Sprachsynthese und Sprachübersetzung in einem einzelnen Azure-Abonnement. Über das [Speech SDK](speech-sdk-reference.md), das [Speech Devices SDK](speech-devices-sdk-qsg.md) oder [REST-APIs](rest-apis.md) können Sie Ihren Anwendungen, Tools und Geräten komfortabel mit Sprachfunktionen ausstatten.

> [!IMPORTANT]
> Speech-Dienste haben die Bing-Spracheingabe-API sowie Sprachübersetzung und Custom Speech abgelöst. Eine Migrationsanleitung finden Sie unter *Anleitungen > Migration*.

Die Azure Speech-Dienste umfassen folgende Features. Verwenden Sie die Links in der Tabelle, um weitere Informationen zu gängigen Anwendungsfällen für das jeweilige Feature zu erhalten oder sich die API-Referenz anzusehen.

| Dienst | Feature | BESCHREIBUNG | SDK | REST |
|---------|---------|-------------|-----|------|
| [Spracherkennung](speech-to-text.md) | Spracherkennung | Die Spracherkennung wandelt Audiodatenströme in Echtzeit in Text um, der von Ihren Anwendungen, Tools oder Geräten genutzt oder angezeigt werden kann. In Kombination mit [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) können Sie Benutzerabsichten aus transkribierter Sprache ableiten und auf Sprachbefehle reagieren. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Batch-Transkription](batch-transcription.md) | Die Batch-Transkription ermöglicht eine asynchrone Spracherkennung/Transkription großer Datenmengen. Dieser REST-basierte Dienst verwendet den gleichen Endpunkt wie die Anpassung und Modellverwaltung. | Nein  | [Ja](https://westus.cris.ai/swagger/ui/index) |
| | [Anpassung](#customize-your-speech-experience) | Wenn Sie die Spracherkennung für die Erkennung und Transkription in einer individuellen Umgebung verwenden, können Sie benutzerdefinierte Akustik-, Sprach- und Aussprachemodelle erstellen, um Umgebungsgeräusche zu kompensieren oder branchenspezifisches Vokabular zu berücksichtigen. | Nein  | [Ja](https://westus.cris.ai/swagger/ui/index) |
| [Text-zu-Sprache](text-to-speech.md) | Text-zu-Sprache | Die Sprachsynthese konvertiert Eingabetext in menschenähnliche synthetische Sprache. Sie haben die Wahl zwischen Standardstimmen und neuronalen Stimmen. (Weitere Informationen finden Sie unter [Sprach- und Regionsunterstützung für die API für den Spracherkennungsdienst](language-support.md).) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Anpassung](#customize-your-speech-experience) | Erstellen Sie spezielle benutzerdefinierte Voicefonts für Ihre Marke oder Ihr Produkt. | Nein  | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Sprachübersetzung](speech-translation.md) | Sprachübersetzung | Die Sprachübersetzung ermöglicht Echtzeitübersetzungen in mehreren Sprachen für Ihre Anwendungen, Tools und Geräte. Verwenden Sie diesen Dienst für die Übersetzung von Sprache in Sprache und Sprache in Text. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Nein  |

## <a name="news-and-updates"></a>Neuigkeiten und Aktualisierungen

Hier finden Sie Neuigkeiten im Zusammenhang mit den Azure Speech-Diensten.

* April 2019: Das Speech SDK 1.4.0 mit Unterstützung von Sprachsynthese (Beta) für C++, C# und Java unter Windows und Linux wurde veröffentlicht. Darüber hinaus unterstützt das SDK nun die Audioformate MP3 und Opus/Ogg für C++ und C# unter Linux. Eine vollständige Liste der Aktualisierungen, Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md). 
* März 2019: Ein neuer Endpunkt für die Sprachsynthese (Text-to-Speech, TTS) steht nun zur Verfügung, der eine vollständige Liste mit den in einer bestimmten Region verfügbaren Stimmen zurückgibt. Darüber hinaus werden jetzt neue Regionen für TTS unterstützt. Weitere Informationen finden Sie unter [Text-to-speech REST API](rest-text-to-speech.md) (Text-to-Speech-REST-API).
* Februar 2019: Das Speech SDK 1.3.0 mit Unterstützung von [Unity (Beta)](quickstart-csharp-unity.md) wurde veröffentlicht. Unterstützung für die Klasse `AudioInput` wurde hinzugefügt, um die Wahl der Audiostreamingquelle zu ermöglichen. Eine vollständige Liste der Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md).
* Dezember 2018: Das Speech SDK 1.2.0 mit Unterstützung von [Python](quickstart-python.md), [Node.js](quickstart-js-node.md) und Ubuntu 18.04 LTS wurde veröffentlicht. Weitere Informationen finden Sie in den [Versionshinweisen](releasenotes.md).

## <a name="try-speech-services"></a>Ausprobieren der Speech-Dienste

Wir stellen Schnellstartanleitungen für die gängigsten Programmiersprachen bereit. Diese sind jeweils so konzipiert, dass Sie in weniger als zehn Minuten über ausführbaren Code verfügen. Die folgende Tabelle enthält die beliebtesten Schnellstartanleitungen für die einzelnen Features. Über die Navigationsleiste auf der linken Seite gelangen Sie auf Wunsch zu weiteren Programmiersprachen und Plattformen.

| Spracherkennung (SDK) | Übersetzung (SDK) | Sprachsynthese (REST) | Sprachsynthese (SDK) |
|-------------------|-------------------|-----------------------|-----------------------|
| [C#, .NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) | [Python (Windows, Linux, macOS)](quickstart-python-text-to-speech.md) | [C#, .NET Framework (Windows)](quickstart-text-to-speech-dotnet-windows.md) |
| [JavaScript (Browser)](quickstart-js-browser.md) | [C#, .NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) | [C#, .NET Core (Windows, Linux, macOS)](quickstart-dotnet-text-to-speech.md) | [C++ (Windows)](quickstart-text-to-speech-cpp-windows.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C#, .NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) | [Node.js (Windows, Linux, macOS)](quickstart-nodejs-text-to-speech.md) | [C++ (Linux)](quickstart-text-to-speech-cpp-linux.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) | |

Nachdem Sie Gelegenheit hatten, die Speech-Dienste zu verwenden, erfahren Sie in unserem Tutorial, wie Sie mithilfe von Speech SDK und LUIS Absichten in gesprochener Sprache erkennen.

* [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="get-sample-code"></a>Abrufen des Beispielcodes

Auf GitHub steht Beispielcode für jeden der Azure Speech-Dienste zur Verfügung. In den Beispielen werden gängige Szenarien wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung oder die Verwendung benutzerdefinierter Modelle behandelt. Über die folgenden Links gelangen Sie zu den SDK- und REST-Beispielen:

* [Beispiele für Spracherkennung, Sprachsynthese und Sprachübersetzung (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Batch transcription samples (REST) (Batchtranskriptionsbeispiele (REST))](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Beispiele für die Sprachsynthese (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>Anpassen der Speech-Funktionen

Die Azure Speech-Dienste funktionieren bestens mit integrierten Modellen, können auf Wunsch aber auch speziell für Ihr Produkt oder Ihre Umgebung angepasst und optimiert werden. Die Anpassungsoptionen reichen von der Feinabstimmung des Akustikmodells bis hin zu individuellen Voicefonts für Ihre Marke. Nachdem Sie ein benutzerdefiniertes Modell erstellt haben, können Sie es mit allen Azure Speech-Diensten verwenden.

| Spracherkennungsdienst | Modell | BESCHREIBUNG |
|----------------|-------|-------------|
| Spracherkennung | [Akustikmodell](how-to-customize-acoustic-models.md) | Erstellen Sie ein benutzerdefiniertes Akustikmodell für Anwendungen, Tools oder Geräte, die in speziellen Umgebungen (etwa in einem Auto oder in einer Produktionshalle) verwendet werden, in denen jeweils besondere Aufzeichnungsverhältnisse herrschen. Beispiele wären etwa Sprecher mit Akzent, bestimmte Hintergrundgeräusche oder die Verwendung eines bestimmten Mikrofons für die Aufzeichnung. |
| | [Sprachmodell](how-to-customize-language-model.md) | Erstellen Sie ein benutzerdefiniertes Sprachmodell, um die Transkription von spezifischem Vokabular und spezifischer Grammatik (beispielsweise aus dem Medizin- oder IT-Bereich) zu verbessern. |
| | [Aussprachemodell](how-to-customize-pronunciation.md) | Durch ein benutzerdefiniertes Aussprachemodell können Sie die phonetische Form und Darstellung eines Worts oder Begriffs definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist zu Beginn lediglich eine Aussprachedatei – eine einfache TXT-Datei. |
| Sprachsynthese | [Voicefont](how-to-customize-voice-font.md) | Mithilfe benutzerdefinierter Voicefonts können Sie eine wiedererkennbare, einzigartige Stimme für Ihre Marke erstellen. Für den Einstieg reicht bereits eine kleine Datenmenge aus. Je mehr Daten Sie bereitstellen, desto natürlicher und menschenähnlicher klingt Ihr Voicefont. |

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Beziehen eines kostenlosen Abonnementschlüssels für die Speech-Dienste](get-started.md)

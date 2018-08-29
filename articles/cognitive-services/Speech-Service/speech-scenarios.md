---
title: Speech-Szenarien – Azure Cognitive Services
description: Szenarien
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 07/02/2018
ms.author: v-jerkin
ms.openlocfilehash: 98e79112ba70ff578975cea9d872b4fcc41512bc
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41929958"
---
# <a name="speech-scenarios"></a>Speech-Szenarien

Es gibt viele Möglichkeiten, die Speech-Technologie bereichernd einzusetzen. Wir analysieren einige der gängigsten Szenarien und heben wichtige Funktionen in der Dokumentation hervor. In den meisten Szenarien ist das [SDK](speech-sdk.md) der Ausgangspunkt.

Dieser Artikel behandelt die folgenden Themen:
> [!div class="checklist"]
> * Erstellen von sprachgesteuerten Apps
> * Transkribieren von Callcenter-Audioanrufen
> * Stimmbots

## <a name="voice-triggered-apps"></a>Sprachgesteuerte Apps

Viele Anwender wollen die Spracheingabe in ihren Anwendungen aktivieren. Die Spracheingabe ist eine großartige Möglichkeit, Ihre Anwendung flexibel zu gestalten, sei es zum Verwenden der Freisprechfunktion (z. B. im Auto) oder zum Beschleunigen verschiedener Aufgaben, z. B. um den Weg, Nachrichten oder Wetterinformationen abzufragen. 

### <a name="voice-triggered-apps-with-baseline-models"></a>Sprachgesteuerte Apps mit Grundmodellen

Wenn Sie Ihre Anwendung für die allgemeine Öffentlichkeit zur Verwendung in Umgebungen mit gemäßigten Hintergrundgeräuschen bereitstellen möchten, ist der schnellste und einfachste Weg, unser [Speech SDK](speech-sdk.md) herunterzuladen und den entsprechenden [Beispielen](quickstart-csharp-dotnet-windows.md) zu folgen. Mit Ihrem [Azure-Abonnementschlüssel](https://azure.microsoft.com/try/cognitive-services/) SDK können Entwickler Audio in grundlegende Spracherkennungsmodelle hochladen, die Cortana und Skype unterstützen. Die Modelle sind auf dem neuesten technischen Stand und werden von den oben genannten Produkten genutzt. Sie sind innerhalb von Minuten einsatzbereit.

### <a name="voice-triggered-apps-with-custom-models"></a>Sprachgesteuerte Apps mit benutzerdefinierten Modellen

Wenn Ihre App für einen bestimmten Bereich gedacht ist (z. B. Chemie, Biologie oder spezielle Ernährungsbedürfnisse), sollten Sie ggf. ein [Sprachmodell](how-to-customize-language-model.md) erstellen. Durch die Anpassung eines Sprachmodells lernt der Decoder die gängigsten Ausdrücke und Wörter Ihrer App. Der Decoder kann mit einem benutzerdefinierten Sprachmodell eine Spracheingabe für einen bestimmten Bereich genauer transkribieren als mit dem Grundmodell. So ist es ggf. auch hilfreich, ein Akustikmodell zu erstellen, wenn Ihre App in einer Umgebung mit auffälligeren Hintergrundgeräuschen verwendet werden soll. Lesen Sie die Dokumentation für andere Fälle, in denen die [Anpassung des Sprachmodells](how-to-customize-language-model.md) und die [Anpassung des Akustikmodels](how-to-customize-acoustic-models.md) sinnvoll sind, und besuchen Sie unser [Anpassungsportal](https://customspeech.ai), um ein Modell zu erstellen. Ähnlich wie bei Grundmodellen werden benutzerdefinierte Modelle über das [Speech SDK](speech-sdk.md) aufgerufen, und befolgen die entsprechenden [Beispiele](quickstart-csharp-dotnet-windows.md).

## <a name="transcribe-call-center-audio-calls"></a>Transkribieren von Callcenter-Audioanrufen

In Callcentern werden große Mengen an Audiodateien gesammelt. Diese Audiodateien enthalten wertvolle Daten, die per Transkription extrahiert werden können. Die Dauer eines Anrufs, die Stimmung, die Kundenzufriedenheit und die allgemeine Relevanz des Anrufs für den Anrufer können mithilfe von Anrufprotokollen ermittelt werden.

Der beste Ausgangspunkt ist die [Batchtranskriptions-API](batch-transcription.md) samt den entsprechenden [Beispielen](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

Zunächst benötigen Sie einen [Azure-Abonnementschlüssel](https://azure.microsoft.com/try/cognitive-services/). Sehen Sie sich dann die Dokumentation zur [Batchtranskriptions-API](batch-transcription.md) an.

### <a name="transcribe-call-center-audio-calls-with-baseline-models"></a>Transkribieren von Callcenter-Audioaufrufen mit Grundmodellen

Legen Sie fest, ob Sie für die Transkription oder die Anpassung eines Sprach- bzw Akustikmodells (oder beider Modelle) interne Grundmodelle verwenden möchten. Um Grundmodelle zu verwenden, benötigt die API nur den API-Schlüssel. Die API wird intern das beste Modell für Ihre Daten aufrufen und anpassen.

### <a name="transcribe-call-center-audio-calls-with-custom-models"></a>Transkribieren von Callcenter-Audioanrufen mit benutzerdefinierten Modellen

Wenn Sie ein benutzerdefiniertes Modell verwenden möchten, benötigen Sie die ID dieses Modells und den entsprechenden API-Schlüssel. Die Modell-ID erhalten Sie im [Anpassungsportal](https://customspeech.ai). Es handelt sich hierbei nicht um die Endpunkt-ID, die in der Ansicht „Endpunktdetails“ angegeben ist, sondern um die Modell-ID. Diese können Sie abrufen, indem Sie bei entsprechenden Modell auf „Details“ klicken.

## <a name="voice-bots"></a>Stimmbots

Entwickler können ihre Anwendungen mit Sprachausgabe ausstatten. Der Dienst Speech kann Sprache für mehrere [Sprachen](supported-languages.md) synthetisieren und die erforderlichen [Endpunkte](rest-apis.md) bereitstellen, um diese Funktion Ihrer App hinzufügen und darauf zuzugreifen.

Außerdem können Benutzer, die ihren Bots mehr Persönlichkeit und Einzigartigkeit verleihen möchten, mit Speech eine einzigartige Voicefont erstellen. Ähnlich wie bei der Anpassung von Spracherkennungsmodellen benötigen auch Voicefonts Benutzerdaten. Entwickler laden diese Daten im [Sprachadaptionsportal](https://customspeech.ai) hoch, damit Sie Ihrem Bot eine einzigartige Stimme verleihen können. Weitere Informationen finden Sie [hier](how-to-text-to-speech.md) und in den [häufig gestellten Fragen](faq-text-to-speech.md). 

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Einführung in das Speech SDK](speech-sdk.md)

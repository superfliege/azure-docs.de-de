---
title: Beispiel für Übersetzung | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Hier ist ein Beispiel für die Sprachübersetzung.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 66d26181334a71578f1a94000cb942a6a87398bc
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070790"
---
# <a name="sample-for-translation"></a>Beispiel für Übersetzung

[!include[Get a Subscription Key](../../../includes/cognitive-services-speech-service-get-subscription-key.md)]

## <a name="top-level-declarations"></a>Deklarationen der obersten Ebene

Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/translation_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#toplevel)]

## <a name="translation-using-the-microphone"></a>Übersetzung über Mikrofon

Der folgende Codeausschnitt zeigt, wie die Spracheingabe aus dem Englischen ins Deutsche übersetzt wird. Außerdem wird die Stimmausgabe des übersetzten Texts abgerufen. Hierbei wird das Mikrofon verwendet.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithMicrophoneAsync)]

[!code-cpp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/translation_samples.cpp#TranslationWithMicrophone)]

[!code-java [Translation Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#TranslationWithMicrophoneAsync)]

## <a name="translation-using-file-input"></a>Übersetzung über Dateieingabe

Der folgende Codeausschnitt zeigt, wie die Spracheingabe aus dem Englischen ins Deutsche und Französische übersetzt wird.
Hierbei wird eine Datei als Eingabe verwendet.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithFileAsync)]

[!code-java [Translation Using File Input](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/TranslationSamples.java#TranslationWithFileAsync)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Spracherkennung](./speech-to-text-sample.md)

- [Absichtserkennung](./intent.md)

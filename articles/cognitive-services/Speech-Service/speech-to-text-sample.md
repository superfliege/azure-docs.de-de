---
title: Beispiel für die Spracherkennung | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Hier finden Sie ein Beispiel für die Spracherkennung.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: b08b461ceccbdf5e79d7bb4320bdc15d09c4b859
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114241"
---
# <a name="sample-for-speech-to-text"></a>Beispiel für die Spracherkennung

[!include[Get a Subscription Key](../../../includes/cognitive-services-speech-service-get-subscription-key.md)]

## <a name="top-level-declarations"></a>Deklarationen der obersten Ebene

Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

## <a name="speech-recognition-using-the-microphone"></a>Spracherkennung unter Verwendung des Mikrofons

Der unten stehende Codeausschnitt zeigt, wie Spracheingaben in der Standardsprache (`en-US`) vom Mikrofon erkannt werden.

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

[!code-java[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

## <a name="speech-recognition-using-a-customized-model"></a>Spracherkennung unter Verwendung eines benutzerdefinierten Modells

Der [benutzerdefinierte Spracherkennungsdienst (Custom Speech Service, CRIS)](https://www.cris.ai/) ermöglicht Ihnen, das Spracherkennungsmoduls von Microsoft für Ihre Anwendung anzupassen. Der folgende Codeausschnitt zeigt, wie die Spracherkennung über ein Mikrofon unter Verwendung Ihres CRIS-Modells funktioniert. Geben Sie vor der Ausführung Ihren CRIS-Abonnementschlüssel und Ihre eigene Bereitstellungs-ID ein.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

[!code-java[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

## <a name="continuous-speech-recognition-from-a-file"></a>Kontinuierliche Spracherkennung aus einer Datei

Der folgende Codeausschnitt erkennt kontinuierlich Spracheingaben aus einer Audiodatei in der Standardsprache (`en-US`). Das unterstützte Format ist einkanalig (mono) WAV/PCM mit einer Samplingrate von 16 kHz.

[!include[Sample Audio](../../../includes/cognitive-services-speech-service-sample-audio.md)]

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionContinuousWithFile)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionWithFile)]

[!code-java[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Absichtserkennung](./intent.md)

- [Übersetzung](./translation.md)

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
ms.openlocfilehash: 2a1850e6a4f3c8eebd1b947aabe1374bdaab3fc8
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030249"
---
# <a name="sample-for-speech-to-text"></a>Beispiel für die Spracherkennung

> [!NOTE]
> Anweisungen zum Herunterladen dieses und anderer Beispiele finden Sie unter [Beispiele für Speech SDK](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:
>
> [!code-csharp[](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#toplevel)]
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="speech-recognition-using-the-microphone"></a>Spracherkennung unter Verwendung des Mikrofons

Der unten stehende Codeausschnitt zeigt, wie Spracheingaben in der Standardsprache (`en-US`) vom Mikrofon erkannt werden.

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

- - -

## <a name="speech-recognition-from-a-file"></a>Spracherkennung aus einer Datei

Der folgende Codeausschnitt erkennt Spracheingaben aus einer Audiodatei in der Standardsprache (`en-US`). Das unterstützte Format ist einkanalig (mono) WAV/PCM mit einer Samplingrate von 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs?name=recognitionFromFile)]

[!code-cpp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp?name=SpeechRecognitionWithFile)]

- - -

## <a name="speech-recognition-using-a-customized-model"></a>Spracherkennung unter Verwendung eines benutzerdefinierten Modells

Der [benutzerdefinierte Spracherkennungsdienst (Custom Speech Service, CRIS)](https://www.cris.ai/) ermöglicht Ihnen, das Spracherkennungsmoduls von Microsoft für Ihre Anwendung anzupassen. Der folgende Codeausschnitt zeigt, wie die Spracherkennung über ein Mikrofon unter Verwendung Ihres CRIS-Modells funktioniert. Geben Sie vor der Ausführung Ihren CRIS-Abonnementschlüssel und Ihre eigene Bereitstellungs-ID ein.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

- - -

## <a name="continuous-speech-recognition"></a>Kontinuierliche Spracherkennung

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionContinuous)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Beispielquellcode

Die neueste Version der Beispiele sowie erweiterte Beispiele befinden sich in einem dedizierten [GitHub-Repository](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Nächste Schritte

- [Absichtserkennung](./intent.md)

- [Übersetzung](./translation.md)

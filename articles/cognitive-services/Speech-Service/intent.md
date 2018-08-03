---
title: Beispiel für Absichtserkennung | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Hier finden Sie ein Beispiel für die Absichtserkennung.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 3e9afc990d6bfa73eb045e7ed76dfd194df309c6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213197"
---
# <a name="sample-for-intent-recognition"></a>Beispiel für Absichtserkennung

Rufen Sie zunächst einen Abonnementschlüssel ab. Im Gegensatz zu anderen Diensten, die vom Sprach-SDK der Cognitive Services unterstützt werden, erfordert die Absichtserkennung einen bestimmten Abonnementschlüssel. [Hier](https://www.luis.ai) finden Sie zusätzliche Informationen zur Technologie der Absichtserkennung sowie Informationen zum Abrufen eines Abonnementschlüssels. Ersetzen Sie den eigenen Language Understanding-Abonnementschlüssel, die [Region Ihres Abonnements](regions.md) und die AppId Ihres Absichtsmodells an der entsprechenden Stelle in den Beispielen.

## <a name="top-level-declarations"></a>Deklarationen der obersten Ebene

Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

## <a name="intent-recognition-using-microphone"></a>Absichtserkennung über Mikrofon

Der unten stehende Codeausschnitt zeigt, wie eine Absicht über Mikrofoneingabe in der Standardsprache (`en-US`) erkannt wird.

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

[!code-java[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Absichtserkennung über Mikrofon in einer angegebenen Sprache

Der unten stehende Codeausschnitt zeigt, wie eine Absicht über Mikrofoneingabe in einer angegebenen Sprache, in diesem Fall Deutsch (`de-de`), erkannt wird.

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

[!code-java[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

## <a name="intent-recognition-from-a-file-using-events"></a>Absichtserkennung aus einer Datei mithilfe von Ereignissen

Der Codeausschnitt zeigt, wie eine Absicht in der Standardsprache (`en-US`) auf kontinuierliche Weise erkannt wird. Mit diesem Code kann auf zusätzliche Informationen, z.B. Zwischenergebnisse, zugegriffen werden. Die Eingabe erfolgt aus einer Audiodatei, das unterstützte Format ist einkanalig (mono) WAV/PCM mit einer Samplingrate von 16 KHz.

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!code-cpp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentContinuousRecognitionWithFile)]

[!code-java[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Spracherkennung](./speech-to-text-sample.md)

- [Übersetzung](./translation.md)

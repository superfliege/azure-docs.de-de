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
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 38f7f038a803546adb83245519efc5de0c0d1599
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045009"
---
# <a name="sample-for-intent-recognition"></a>Beispiel für Absichtserkennung

> [!NOTE]
> Anweisungen zum Herunterladen dieses und anderer Beispiele finden Sie unter [Beispiele für Sprach-SDK](samples.md).

> [!NOTE]
> Rufen Sie zunächst einen Abonnementschlüssel ab. Im Gegensatz zu anderen Diensten, die vom Sprach-SDK der Cognitive Services unterstützt werden, erfordert die Absichtserkennung einen bestimmten Abonnementschlüssel. [Hier](https://www.luis.ai) finden Sie zusätzliche Informationen zur Technologie der Absichtserkennung sowie Informationen zum Abrufen eines Abonnementschlüssels. Ersetzen Sie den eigenen Abonnementschlüssel, die Region des Diensts und die AppId Ihres Absichtsmodells an der entsprechenden Stelle in den Beispielen.

> [!NOTE]
> Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="intent-recognition-using-microphone"></a>Absichtserkennung über Mikrofon

Der unten stehende Codeausschnitt zeigt, wie eine Absicht über Mikrofoneingabe in der Standardsprache (`en-US`) erkannt wird.

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

- - -

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Absichtserkennung über Mikrofon in einer angegebenen Sprache

Der unten stehende Codeausschnitt zeigt, wie eine Absicht über Mikrofoneingabe in einer angegebenen Sprache, in diesem Fall Deutsch (`de-de`), erkannt wird.

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

- - -

## <a name="intent-recognition-from-a-file"></a>Absichtserkennung aus einer Datei

Der folgende Codeausschnitt erkennt eine Absicht aus einer Audiodatei in der Standardsprache (`en-US`). Das unterstützte Format ist einkanalig (mono) WAV/PCM mit einer Samplingrate von 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-cpp[Intent Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithFile)]

- - -

## <a name="intent-recognition-using-events"></a>Absichtserkennung über Ereignisse

Der Codeausschnitt zeigt, wie eine Absicht auf kontinuierliche Weise erkannt wird. Mit diesem Code kann auf zusätzliche Informationen, z.B. Zwischenergebnisse, zugegriffen werden. 

[!code-cpp[Intent Recognition Using Events](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Beispielquellcode

Die neueste Reihe von Beispielen finden Sie im [GitHub-Beispielrepository für das Sprach-SDK der Cognitive Services](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Nächste Schritte

- [Spracherkennung](./speech-to-text-sample.md)

- [Übersetzung](./translation.md)

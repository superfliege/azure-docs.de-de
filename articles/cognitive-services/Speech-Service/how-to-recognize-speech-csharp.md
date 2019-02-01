---
title: Erkennen von Sprache mit dem Speech SDK für C#
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Sprache (aus einer Datei, von einem Mikrofon, mit einem benutzerdefinierten Modell, kontinuierlich oder einmalig) mit dem Speech SDK für C# erkennen.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: c2ea06fd578078489d2d13d4d067280d459e8416
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217933"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-c"></a>Erkennen von Sprache mit dem Speech SDK für C#

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-csharp[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-csharp[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionCustomized)]

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-csharp[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionContinuousWithFile)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Den Code aus diesem Artikel finden Sie im Ordner „samples/csharp/sharedcontent/console“.

## <a name="next-steps"></a>Nächste Schritte

- [Erkennen von Absichten anhand von gesprochener Sprache](how-to-recognize-intents-from-speech-csharp.md)
- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)


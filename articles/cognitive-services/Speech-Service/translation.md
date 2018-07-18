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
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 1151628ed695e74e2196c20b08e33fa5eaf33282
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37028386"
---
# <a name="sample-for-translation"></a>Beispiel für Übersetzung

> [!NOTE]
> Anweisungen zum Herunterladen dieses und anderer Beispiele finden Sie unter [Beispiele für Sprach-SDK](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Für alle unten aufgeführten Beispiele sollten die folgenden Deklarationen der obersten Ebene vorhanden sein:
>
> [!code-csharp [Using Statements](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#toplevel)]
>
> - - -

## <a name="translation-using-the-microphone"></a>Übersetzung über Mikrofon

Der folgende Codeausschnitt zeigt, wie die Spracheingabe aus dem Englischen ins Deutsche übersetzt wird. Außerdem wird die Stimmausgabe des übersetzten Texts abgerufen. Hierbei wird das Mikrofon verwendet.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithMicrophoneAsync)]

- - -

## <a name="translation-using-file-input"></a>Übersetzung über Dateieingabe

Der folgende Codeausschnitt zeigt, wie die Spracheingabe aus dem Englischen ins Deutsche und Französische übersetzt wird.
Hierbei wird eine Datei als Eingabe verwendet.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithFileAsync)]

- - -

## <a name="sample-source-code"></a>Beispielquellcode

Die neueste Version der Beispiele sowie noch komplexere Beispiele sind in einem dedizierten [GitHub-Repository](https://github.com/Azure-Samples/cognitive-services-speech-sdk) zu finden.

## <a name="next-steps"></a>Nächste Schritte

- [Spracherkennung](./speech-to-text-sample.md)

- [Absichtserkennung](./intent.md)

---
title: Verwenden der Spracherkennung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Spracherkennung im Speech-Dienst verwenden.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 26cecedfc3ad2d472b9686e25054fe08253cee77
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068521"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Verwenden der Spracherkennung im Speech-Dienst

Sie können die **Spracherkennung** in Ihren Anwendungen auf zwei unterschiedliche Arten verwenden.

| Methode | BESCHREIBUNG |
|-|-|
| [SDK](speech-sdk.md) | Einfachste Methode für C/C++-, C#- und Java*-Entwickler |
| [REST](rest-apis.md) | Erkennen von kurzen Äußerungen mithilfe einer HTTP POST-Anforderung | 

## <a name="using-the-sdk"></a>Verwenden des SDK

Das [Speech SDK](speech-sdk.md) ist die einfachste Methode zum Verwenden von **Spracherkennung** mit voller Funktionalität in Ihrer Anwendung.

1. Erstellen Sie eine Sprachfactory, und geben Sie einen Abonnementschlüssel und eine [Region](regions.md) oder ein Autorisierungstoken für den Speech-Dienst an. Sie können auch Optionen konfigurieren, z.B. die Erkennungssprache oder einen benutzerdefinierten Endpunkt für Ihre eigenen Spracherkennungsmodelle.

2. Rufen Sie eine Erkennung aus der Factory ab. Drei verschiedene Typen von Erkennungen stehen zur Verfügung. Für jeden Erkennungstyp können Sie das Standardmikrofon Ihres Geräts, einen Audiodatenstrom oder Audioinhalt aus einer Datei verwenden.

    Erkennung | Funktion
    -|-
    Spracherkennung|Ermöglicht die Texttranskription von Sprache.
    Absichtserkennung|Leitet die Absicht von Sprechern über [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) nach der Erkennung ab.\*
    Übersetzungserkennung|Übersetzt den transkribierten Text in eine andere Sprache (siehe [Sprachübersetzung](how-to-translate-speech.md)).

    \* *Im Hinblick auf die Absichtserkennung müssen Sie beim Erstellen einer Sprachfactory für die Absichtserkennung einen separaten Abonnementschlüssel für LUIS verwenden.*
    
4. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung ein Transkriptionsendergebnis.

5. Starten Sie die Erkennung.
   Verwenden Sie für eine Einzelerkennung, wie eine Befehls- oder Abfrageerkennung, `RecognizeAsync()`. Damit wird die erste Äußerung erkannt.
   Verwenden Sie für eine lang andauernde Erkennung, wie eine Transkription, `StartContinuousRecognitionAsync()`, und verknüpfen Sie die Ereignisse für asynchrone Erkennungsergebnisse.

### <a name="sdk-samples"></a>SDK-Beispiele

Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Verwenden der REST-API

Die REST-API ist die einfachste Möglichkeit zur Spracherkennung, wenn Sie eine Sprache verwenden, die vom SDK nicht unterstützt wird. Sie senden eine HTTP POST-Anforderung an den Dienstendpunkt und übergeben die gesamte Äußerung in den Anforderungstext. Sie erhalten eine Antwort mit dem erkannten Text.

> [!NOTE]
> Bei Verwendung der REST-API sind Äußerungen auf maximal 15 Sekunden beschränkt.

Weitere Informationen zur REST-API für die **Spracherkennung** finden Sie unter [REST-APIs](rest-apis.md#speech-to-text). Um sie zu testen, laden Sie die [REST-API-Beispiele](https://github.com/Azure-Samples/SpeechToText-REST) von GitHub herunter.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C++](quickstart-cpp-windows.md)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
- [Erkennen von Sprache in Java](quickstart-java-android.md)

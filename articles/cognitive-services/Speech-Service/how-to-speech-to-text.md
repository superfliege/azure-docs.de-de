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
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 878a31992415b1f8688afcfb186fcd94ce2567b4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378923"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Verwenden der Spracherkennung im Speech-Dienst

Sie können die **Spracherkennung** in Ihren Anwendungen auf zwei unterschiedliche Arten verwenden.

| Methode | BESCHREIBUNG |
|-|-|
| [SDK](speech-sdk.md) | Einfachste Methode für C/C++-, C#- und Java*-Entwickler |
| [REST](rest-apis.md) | Erkennen von kurzen Äußerungen mithilfe einer HTTP POST-Anforderung | 

\* *Das Java SDK ist Teil des [Speech-Geräte-SDK](speech-devices-sdk.md).*

## <a name="using-the-sdk"></a>Verwenden des SDK

Das [Speech SDK](speech-sdk.md) ist die einfachste Methode zum Verwenden von **Spracherkennung** mit voller Funktionalität in Ihrer Anwendung.

1. Erstellen Sie eine Sprachfactory, und geben Sie einen Abonnementschlüssel für den Speech-Dienst oder ein Autorisierungstoken an. Sie können auch Optionen konfigurieren, z.B. die Erkennungssprache oder einen benutzerdefinierten Endpunkt für Ihre eigenen Spracherkennungsmodelle.

2. Rufen Sie eine Erkennung aus der Factory ab. Drei verschiedene Typen von Erkennungen stehen zur Verfügung. Für jeden Erkennungstyp können Sie das Standardmikrofon Ihres Geräts, einen Audiodatenstrom oder Audioinhalt aus einer Datei verwenden.

    Erkennung | Funktion
    -|-
    Spracherkennung|Ermöglicht die Texttranskription von Sprache.
    Absichtserkennung|Leitet die Absicht von Sprechern über [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) nach der Erkennung ab.\*
    Übersetzungserkennung|Übersetzt den transkribierten Text in eine andere Sprache (siehe [Sprachübersetzung](how-to-translate-speech.md)).

    \* *Im Hinblick auf die Absichtserkennung müssen Sie beim Erstellen einer Sprachfactory für die Absichtserkennung einen separaten Abonnementschlüssel für LUIS verwenden.*
    
4. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung ein Transkriptionsendergebnis.

5. Starten Sie die Erkennung.

### <a name="sdk-samples"></a>SDK-Beispiele

Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Verwenden der REST-API

Die REST-API ist die einfachste Möglichkeit zur Spracherkennung, wenn Sie eine Sprache verwenden, die vom SDK nicht unterstützt wird. Sie senden eine HTTP POST-Anforderung an den Dienstendpunkt und übergeben die gesamte Äußerung in den Anforderungstext. Sie erhalten eine Antwort mit dem erkannten Text.

> [!NOTE]
> Bei Verwendung der REST-API sind Äußerungen auf maximal 15 Sekunden beschränkt.


Weitere Informationen zur REST-API für die **Spracherkennung** finden Sie unter [REST-APIs](rest-apis.md#speech-to-text). Um sie zu testen, laden Sie die [REST-API-Beispiele](https://github.com/Azure-Samples/SpeechToText-REST) von GitHub herunter.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-windows.md)

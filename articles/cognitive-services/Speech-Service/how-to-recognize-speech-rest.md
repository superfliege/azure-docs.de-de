---
title: Erkennen von Sprache mit der REST-API
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Spracherkennungs-API des Speech-Diensts von Cognitive Services verwenden.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 1bcc290d987e8294f9a73cf8490f6cdd0251b440
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467275"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Erkennen von Sprache mit der REST-API

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

Die REST-API kann zum Erkennen kurzer Äußerungen mithilfe einer HTTP POST-Anforderung verwendet werden.

Die REST-API ist die einfachste Möglichkeit zur Spracherkennung, wenn Sie keine vom [SDK](speech-sdk.md) unterstützte Sprache verwenden. Sie senden eine HTTP POST-Anforderung an den Dienstendpunkt und übergeben die gesamte Äußerung im Anforderungstext. Sie erhalten eine Antwort mit dem erkannten Text.

> [!NOTE]
> Bei Verwendung der REST-API sind Äußerungen auf maximal 15 Sekunden begrenzt.
> Im [Speech SDK](how-to-recognize-speech-csharp.md) finden Sie Möglichkeiten zur Erkennung von längeren Äußerungen.

Weitere Informationen zur REST-API für die **Spracherkennung** finden Sie im [Artikel zu REST-APIs](rest-apis.md#speech-to-text). Um die API in Aktion zu erleben, laden Sie die [REST-API-Beispiele](https://github.com/Azure-Samples/SpeechToText-REST) von GitHub herunter.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [REST-API-Übersicht](rest-apis.md) an.

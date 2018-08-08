---
title: Erkennen von Sprache mit der REST-API
description: Erfahren Sie, wie Sie Spracherkennung im Speech-Dienst verwenden.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 15db2d9b872c76d70fd531af07fb55c701e86494
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331278"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Erkennen von Sprache mit der REST-API

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

Die REST-API kann zum Erkennen von kurzen Äußerungen mithilfe einer HTTP POST-Anforderung verwendet werden.

Die REST-API ist die einfachste Möglichkeit zur Spracherkennung, wenn Sie eine Sprache verwenden, die vom SDK nicht unterstützt wird.
Sie senden eine HTTP POST-Anforderung an den Dienstendpunkt und übergeben die gesamte Äußerung in den Anforderungstext.
Sie erhalten eine Antwort mit dem erkannten Text.

> [!NOTE]
> Bei Verwendung der REST-API sind Äußerungen auf maximal 15 Sekunden beschränkt.
> Im [Speech SDK](how-to-recognize-speech-csharp.md) finden Sie Möglichkeiten zur Erkennung von längeren Äußerungen.

Weitere Informationen zur REST-API für die **Spracherkennung** finden Sie unter [REST-APIs](rest-apis.md#speech-to-text). Um sie zu testen, laden Sie die [REST-API-Beispiele](https://github.com/Azure-Samples/SpeechToText-REST) von GitHub herunter.

## <a name="next-steps"></a>Nächste Schritte

- [Lesen Sie die REST-API-Übersicht](rest-apis.md)

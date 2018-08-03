---
title: Sprachübersetzung mit Speech-Diensten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sprachübersetzung im Speech-Dienst verwenden.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 6acfcf0ae8ab4c63e6cc943f93da6b947f3d118c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071095"
---
# <a name="translate-speech-using-speech-service"></a>Sprachübersetzung mit dem Speech-Dienst

Das [Speech SDK](speech-sdk.md) ist die einfachste Möglichkeit zur Verwendung der Sprachübersetzung in Ihrer Anwendung. Das SDK bietet die volle Funktionalität des Diensts. Die Sprachübersetzung wird mit folgenden grundlegenden Schritten durchgeführt:

1. Erstellen Sie eine Sprachfactory, und geben Sie einen Abonnementschlüssel und eine [Region](regions.md) für den Speech-Dienst oder ein Autorisierungstoken an. Sie konfigurieren auch die Ausgangs- und Zielsprachen für die Übersetzung. Zudem geben Sie an, ob die Übersetzung als Text- oder als Sprachausgabe erfolgt.

2. Rufen Sie eine Erkennung aus der Factory ab. Wählen Sie für die Übersetzung eine Übersetzungserkennung aus. (Die anderen Erkennungen werden für die *Spracherkennung* verwendet.) Je nach der verwendeten Audioquelle stehen verschiedene Arten von Übersetzungserkennungen zur Verfügung.

4. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung ein Übersetzungsendergebnis.

5. Starten Sie die Erkennung und Übersetzung.

# <a name="sdk-samples"></a>SDK-Beispiele

Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)

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
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7f39f284998489574049d82c44b3d3a0a3797adb
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378979"
---
# <a name="translate-speech-using-speech-service"></a>Sprachübersetzung mit dem Speech-Dienst

Das [Speech SDK](speech-sdk.md) ist die einfachste Möglichkeit zur Verwendung der Sprachübersetzung in Ihrer Anwendung. Das SDK bietet die volle Funktionalität des Diensts. Die Sprachübersetzung wird mit folgenden grundlegenden Schritten durchgeführt:

1. Erstellen Sie eine Sprachfactory, und geben Sie einen Abonnementschlüssel für den Speech-Dienst oder ein Autorisierungstoken an. Sie konfigurieren auch die Ausgangs- und Zielsprachen für die Übersetzung. Zudem geben Sie an, ob die Übersetzung als Text- oder als Sprachausgabe erfolgt.

2. Rufen Sie eine Erkennung aus der Factory ab. Wählen Sie für die Übersetzung eine Übersetzungserkennung aus. (Die anderen Erkennungen werden für die *Spracherkennung* verwendet.) Je nach der verwendeten Audioquelle stehen verschiedene Arten von Übersetzungserkennungen zur Verfügung.

4. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung ein Übersetzungsendergebnis.

5. Starten Sie die Erkennung und Übersetzung.

# <a name="sdk-samples"></a>SDK-Beispiele

Die Sammlung der aktuellsten Beispiele finden Sie im [GitHub-Repository mit Beispielen für das Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-windows.md)

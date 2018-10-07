---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: ab4795325f733d15403b53f027daa4fb8c824cc6
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47185268"
---
<!-- N.B. no header, language-agnostic -->

Das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) von Microsoft Cognitive Services ist die einfachste Methode zum Verwenden von **Sprachübersetzung** in Ihrer Anwendung.
Das SDK bietet die volle Funktionalität des Diensts. Die Sprachübersetzung wird mit folgenden grundlegenden Schritten durchgeführt:

1. Erstellen Sie eine Sprachübersetzungskonfiguration, und geben Sie als Parameter einen Abonnementschlüssel für den Spracherkennungsdienst (oder ein Autorisierungstoken) und eine [Region](~/articles/cognitive-services/speech-service/regions.md) an. Ändern Sie die Konfiguration nach Bedarf. Sie können z.B. auch die Ausgangs- und Zielsprachen für die Übersetzung konfigurieren. Zudem können Sie angeben, ob die Übersetzung als Text- oder als Sprachausgabe erfolgen soll.

1. Erstellen Sie eine Übersetzungserkennung aus der Sprachübersetzungskonfiguration. Stellen Sie eine Audiokonfiguration bereit, wenn Sie eine andere Quelle als Ihr Standardmikrofon für die Erkennung verwenden möchten (z.B. einen Audiodatenstrom oder eine Audiodatei).

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann Ihre Ereignishandler auf, wenn Zwischen- und Endergebnisse und ein Sprachsyntheseereignis für die optionale Audioausgabe vorliegen. Andernfalls erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Erkennung. Verwenden Sie für Einzelübersetzungen die `RecognizeOnceAsync()`-Methode, die die erste erkannte Äußerung zurückgibt. Verwenden Sie für eine lang andauernde Übersetzung die `StartContinuousRecognitionAsync()`-Methode, und binden Sie die Ereignisse für asynchrone Erkennungsergebnisse.

Sehen Sie sich die folgenden Codeausschnitte für Sprachübersetzungsszenarien an, in denen das Speech SDK verwendet wird.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]

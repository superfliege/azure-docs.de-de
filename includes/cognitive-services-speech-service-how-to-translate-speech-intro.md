---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ee50a104a75d3cd5ff958bd49a1ff7010c5d5083
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144405"
---
<!-- N.B. no header, language-agnostic -->

Das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) von Microsoft Cognitive Services ist die einfachste Methode zum Verwenden von **Sprachübersetzung** in Ihrer Anwendung.
Das SDK bietet die volle Funktionalität des Diensts. Die Sprachübersetzung wird mit folgenden grundlegenden Schritten durchgeführt:

1. Erstellen Sie eine Sprachfactory, und geben Sie als Parameter einen Abonnementschlüssel oder ein Autorisierungstoken für den Speech-Dienst und eine [Region](~/articles/cognitive-services/speech-service/regions.md) an.
   
1. Erstellen Sie eine Übersetzungserkennung aus der Sprachfactory. Sie können auch die Ausgangs- und Zielsprachen für die Übersetzung konfigurieren. Zudem können Sie angeben, ob die Übersetzung als Text- oder als Sprachausgabe erfolgen soll. Je nach der verwendeten Audioquelle stehen verschiedene Arten von Übersetzungserkennungen zur Verfügung.

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann Ihre Ereignishandler auf, wenn Zwischen- und Endergebnisse und ein Sprachsyntheseereignis für die optionale Audioausgabe vorliegen. Andernfalls erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Erkennung. Verwenden Sie für Einzelübersetzungen die `RecognizeAsync()`-Methode, die die erste erkannte Äußerung zurückgibt. Verwenden Sie für eine lang andauernde Übersetzung die `StartContinuousRecognitionAsync()`-Methode, und binden Sie die Ereignisse für asynchrone Erkennungsergebnisse.

Sehen Sie sich die folgenden Codeausschnitte für Sprachübersetzungsszenarien an, in denen das Speech SDK verwendet wird.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]

---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1103e5a217ca4972cc1c983a7ad0d07b0797e9e9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43129322"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) von Cognitive Services ist die einfachste Methode zum Verwenden von **Spracherkennung** mit voller Funktionalität in Ihrer Anwendung.

1. Erstellen Sie eine Sprachfactory, und geben Sie als Parameter einen Abonnementschlüssel für den Speech-Dienst (oder ein Autorisierungstoken) und eine [Region](~/articles/cognitive-services/speech-service/regions.md) an. Sie können auch einen benutzerdefinierten Endpunkt angeben, wenn Sie keinen Standard-Dienstendpunkt verwenden möchten.

1. Rufen Sie eine Spracherkennung aus der Sprachfactory ab. Sie können die Eingabesprache und das Ausgabeformat konfigurieren. Die Spracherkennung kann das Standardmikrofon Ihres Geräts, einen Audiostream oder Audioinhalt aus einer Datei verwenden.

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Erkennung. Verwenden Sie für Einzelerkennungen, wie eine Befehls- oder Abfrageerkennungen, die `RecognizeAsync()`-Methode. Diese Methode gibt die erste erkannte Äußerung zurück. Verwenden Sie für länger andauernde Erkennungen (z.B. bei Transkriptionen) die `StartContinuousRecognitionAsync()`-Methode. Sie können die Ereignisse für asynchrone Erkennungsergebnisse binden.

Sehen Sie sich die folgenden Codeausschnitte für Spracherkennungsszenarien an, in denen das Speech SDK verwendet wird.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]

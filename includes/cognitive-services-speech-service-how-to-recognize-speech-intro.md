---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 31482cc7850c574b952c454021af729da324ba15
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978859"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) von Cognitive Services ist die einfachste Methode zum Verwenden von **Spracherkennung** mit voller Funktionalität in Ihrer Anwendung.

1. Erstellen Sie eine Sprachkonfiguration, und geben Sie als Parameter einen Abonnementschlüssel für den Speech-Dienst (oder ein Autorisierungstoken) und eine [Region](~/articles/cognitive-services/speech-service/regions.md) an. Ändern Sie die Konfiguration nach Bedarf. Geben Sie beispielsweise einen benutzerdefinierten Endpunkt an, um einen nicht standardmäßigen Dienstendpunkt anzugeben, oder wählen Sie die gesprochene Eingabesprache oder das Ausgabeformat aus.

1. Erstellen Sie eine Spracherkennung aus der Sprachkonfiguration. Stellen Sie eine Audiokonfiguration bereit, wenn Sie eine andere Quelle als Ihr Standardmikrofon für die Erkennung verwenden möchten (z.B. einen Audiodatenstrom oder eine Audiodatei).

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen die Ereignishandler auf. Andernfalls erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Erkennung. Verwenden Sie für Einzelerkennungen wie Befehls- oder Abfrageerkennungen die `RecognizeOnceAsync()`-Methode (oder die Entsprechung in der jeweiligen Sprache). Diese Methode gibt die erste erkannte Äußerung zurück. Verwenden Sie für länger andauernde Erkennungen (z.B. bei Transkriptionen) die `StartContinuousRecognitionAsync()`-Methode (oder die Entsprechung in der jeweiligen Sprache). Sie können die Ereignisse für asynchrone Erkennungsergebnisse binden.

Sehen Sie sich die folgenden Codeausschnitte für Spracherkennungsszenarien an, in denen das Speech SDK verwendet wird.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]

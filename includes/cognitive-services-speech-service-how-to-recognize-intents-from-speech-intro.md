---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: a4accb0846ec9cf5efc5ca5aa00c731edd41d4d4
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143057"
---
<!-- N.B. no header, language-agnostic -->

Das Microsoft Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) bietet eine Möglichkeit zur Erkennung von **Absichten aus Sprachinhalten**. Es wird von Cognitive Services [LUIS (Language Understanding Intelligent Service)](https://www.luis.ai/home) unterstützt.

1. Erstellen Sie eine Sprachfactory mit einem LUIS-Abonnementschlüssel und der [Region](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition) als Parameter. Der LUIS-Abonnementschlüssel wird in der Dienstdokumentation als **Endpunktschlüssel** bezeichnet. Sie können nicht den LUIS-Erstellungsschlüssel verwenden. (Lesen Sie dazu den Hinweis weiter unten in diesem Abschnitt.)

1. Rufen Sie eine Absichtserkennung aus der Sprachfactory ab. Die Spracherkennung kann das Standardmikrofon Ihres Geräts, einen Audiostream oder Audioinhalt aus einer Datei verwenden.

1. Rufen Sie das LUIS-Modell basierend auf Ihrer **AppID** ab. Fügen Sie die gewünschten Absichten hinzu. 

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen (mit den Absichten) die Ereignishandler auf. Wenn Sie die Ereignisse nicht binden, erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Absichtserkennung. Verwenden Sie für Einzelerkennungen, wie eine Befehls- oder Abfrageerkennungen, die `RecognizeAsync()`-Methode. Diese Methode gibt die erste erkannte Äußerung zurück. Verwenden Sie für länger andauernde Erkennungen die `StartContinuousRecognitionAsync()`-Methode. Sie können die Ereignisse für asynchrone Erkennungsergebnisse binden.

Sehen Sie sich die folgenden Codeausschnitte für Absichtserkennungsszenarien an, in denen das Speech SDK verwendet wird. Ersetzen Sie die Werte im Beispiel durch Ihren eigenen LUIS-Abonnementschlüssel (Endpunktschlüssel), die [Region Ihres Abonnements](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition) und die **AppID** Ihres Absichtsmodells.

> [!NOTE]
> Im Gegensatz zu anderen Diensten, die vom Speech SDK unterstützt werden, erfordert die Absichtserkennung einen bestimmten Abonnementschlüssel (LUIS-Endpunktschlüssel). Weitere Informationen zur Technologie hinter der Absichtserkennung finden Sie auf der [LUIS-Website](https://www.luis.ai). Weitere Informationen zum Erhalt des **Endpunktschlüssels** finden Sie unter [Erstellen eines LUIS-Endpunktschlüssels](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).

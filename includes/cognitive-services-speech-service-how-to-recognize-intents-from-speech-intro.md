---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 80bf9247bbb07fa61b7153e321b1991b82a9d616
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246894"
---
<!-- N.B. no header, language-agnostic -->

Das Microsoft Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) bietet eine Möglichkeit zur Erkennung von **Absichten aus Sprachinhalten**. Es wird von Cognitive Services [LUIS (Language Understanding Intelligent Service)](https://www.luis.ai/home) unterstützt.

1. Erstellen Sie eine Sprachkonfiguration mit einem LUIS-Abonnementschlüssel und einer [Region](~/articles/cognitive-services/speech-service/regions.md#intent-recognition) als Parameter. Der LUIS-Abonnementschlüssel wird in der Dienstdokumentation als **Endpunktschlüssel** bezeichnet. Sie können nicht den LUIS-Erstellungsschlüssel verwenden. (Lesen Sie dazu den Hinweis weiter unten in diesem Abschnitt.)

1. Erstellen Sie eine Absichtserkennung aus der Sprachkonfiguration. Stellen Sie eine Audiokonfiguration bereit, wenn Sie eine andere Quelle als Ihr Standardmikrofon für die Erkennung verwenden möchten (z.B. einen Audiodatenstrom oder eine Audiodatei).

1. Rufen Sie das LUIS-Modell basierend auf Ihrer **AppID** ab. Fügen Sie die gewünschten Absichten hinzu.

1. Falls gewünscht, können Sie die Ereignisse für asynchrone Vorgänge binden. Die Erkennung ruft dann nach erhaltener Rückgabe von Zwischen- und Endergebnissen (mit den Absichten) die Ereignishandler auf. Wenn Sie die Ereignisse nicht binden, erhält die Anwendung nur ein Transkriptionsendergebnis.

1. Starten Sie die Absichtserkennung. Verwenden Sie für Einzelerkennungen, wie eine Befehls- oder Abfrageerkennungen, die `RecognizeOnceAsync()`-Methode. Diese Methode gibt die erste erkannte Äußerung zurück. Verwenden Sie für länger andauernde Erkennungen die `StartContinuousRecognitionAsync()`-Methode. Sie können die Ereignisse für asynchrone Erkennungsergebnisse binden.

Sehen Sie sich die folgenden Codeausschnitte für Absichtserkennungsszenarien an, in denen das Speech SDK verwendet wird. Ersetzen Sie die Werte im Beispiel durch Ihren eigenen LUIS-Abonnementschlüssel (Endpunktschlüssel), die [Region Ihres Abonnements](~/articles/cognitive-services/speech-service/regions.md#intent-recognition) und die **AppID** Ihres Absichtsmodells.

> [!NOTE]
> Im Gegensatz zu anderen Diensten, die vom Speech SDK unterstützt werden, erfordert die Absichtserkennung einen bestimmten Abonnementschlüssel (LUIS-Endpunktschlüssel). Weitere Informationen zur Technologie hinter der Absichtserkennung finden Sie auf der [LUIS-Website](https://www.luis.ai). Weitere Informationen zum Erhalt des **Endpunktschlüssels** finden Sie unter [Erstellen eines LUIS-Endpunktschlüssels](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription).

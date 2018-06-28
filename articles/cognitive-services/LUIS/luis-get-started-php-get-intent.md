---
title: Aufrufen einer LUIS-App (Language Understanding Intelligent Service) mithilfe von PHP | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie eine LUIS-App mithilfe von PHP aufrufen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 90aa1fc9d458484bbbaf54d4d838064ced03b257
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265125"
---
# <a name="tutorial-call-a-luis-endpoint-using-php"></a>Tutorial: Aufrufen eines LUIS-Endpunkts mithilfe von PHP
Übergeben Sie Äußerungen an einen LUIS-Endpunkt, um Absichten und Entitäten zurückzugeben.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines LUIS-Abonnements und Kopieren des Schlüsselwerts zur späteren Verwendung
> * Anzeigen von LUIS-Endpunktergebnissen aus dem Browser in der öffentlichen IoT-Beispiel-App
> * Erstellen einer Visual Studio-C#-Konsolen-App zum Ausführen von HTTPS-Aufrufen für den LUIS-Endpunkt

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="create-luis-subscription-key"></a>Erstellen eines LUIS-Abonnementschlüssels
Sie benötigen einen Cognitive Services-API-Schlüssel, um die in dieser exemplarischen Vorgehensweise verwendete LUIS-Beispiel-App aufzurufen. 

Führen Sie zum Abrufen eines API-Schlüssels die folgenden Schritte aus: 

1. Zuerst müssen Sie ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) im Azure-Portal erstellen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

2. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an. 

3. Führen Sie zum Abrufen eines Schlüssels die Schritte unter [Manage Azure endpoint subscription keys](./luis-how-to-azure-subscription.md) (Verwalten von Azure-Endpunktabonnementschlüsseln) aus.

4. Kehren Sie zurück zur [LUIS](luis-reference-regions.md)-Website, und melden Sie sich mit Ihrem Azure-Konto an. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Screenshot der App-Liste")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Grundlegendes zu LUIS-Rückgaben

Wenn Sie nachvollziehen möchten, was von einer LUIS-App zurückgegeben wird, fügen Sie die URL einer LUIS-Beispiel-App in ein Browserfenster ein. Die Beispiel-App ist eine IoT-App, die ermittelt, ob der Benutzer Lichter ein- oder ausschalten möchte.

1. Der Endpunkt der Beispiel-App hat dieses Format: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Kopieren Sie die URL, und ersetzen Sie den Wert im Feld `subscription-key` durch Ihren Abonnementschlüssel.
2. Fügen Sie die URL in ein Browserfenster ein, und drücken Sie die EINGABETASTE. Das im Browser angezeigte JSON-Ergebnis gibt an, dass LUIS die Absicht `HomeAutomation.TurnOn` und die Entität `HomeAutomation.Room` mit dem Wert `bedroom` erkennt.

    ![JSON-Ergebnis, das die Absicht „TurnOn“ erkennt](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. Ändern Sie den Wert des Parameters `q=` in der URL in `turn off the living room light`, und drücken Sie die EINGABETASTE. Das Ergebnis gibt jetzt an, dass der LUIS die Absicht `HomeAutomation.TurnOff` und die Entität `HomeAutomation.Room` mit dem Wert `living room` erkannt hat. 

    ![JSON-Ergebnis, das die Absicht „TurnOff“ erkennt](./media/luis-get-started-node-get-intent/turn-off-living-room.png)

## <a name="consume-a-luis-result-using-the-endpoint-api-with-php"></a>Nutzen eines LUIS-Ergebnisses mithilfe der Endpunkt-API und PHP 

Sie können mit PHP auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. 
1. Kopieren Sie den folgenden Code, und speichern Sie ihn in einer HTML-Datei:

   [!code-php[PHP code that calls a LUIS endpoint](~/samples-luis/documentation-samples/endpoint-api-samples/php/endpoint-call.php)]
2. Ersetzen Sie in der folgenden Codezeile `"YOUR-SUBSCRIPTION-KEY"` durch Ihren Abonnementschlüssel: `$subscriptionKey = "YOUR-SUBSCRIPTION-KEY";`.

3. Führen Sie die PHP-Anwendung aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
In diesem Tutorial werden zwei Ressourcen erstellt: der LUIS-Abonnementschlüssel und das C#-Projekt. Löschen Sie den LUIS-Abonnementschlüssel im Azure-Portal. Schließen Sie das Visual Studio-Projekt, und entfernen Sie das Verzeichnis vom Dateisystem. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-php-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
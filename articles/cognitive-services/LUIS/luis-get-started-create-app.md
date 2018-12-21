---
title: 'Schnellstart: Erstellen einer App'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 71f3084be697dd84f3f262d2a79cd04a0ba76d8e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086809"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Schnellstart: Verwenden der vordefinierten Home Automation-App

In dieser Schnellstartanleitung erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel benötigen Sie ein kostenloses LUIS-Konto, das Sie im LUIS-Portal unter [https://www.luis.ai](https://www.luis.ai) erstellt haben. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
Sie können Ihre Anwendungen auf der Seite **Meine Apps** erstellen und verwalten. 

1. Melden Sie sich beim LUIS-Portal an.

2. Klicken Sie auf **Create new app** (Neue App erstellen).

    [![Screenshot der App-Liste](media/luis-quickstart-new-app/app-list.png "Screenshot der App-Liste")](media/luis-quickstart-new-app/app-list.png)

3. Geben Sie im Dialogfeld den Namen „Home Automation“ für Ihre Anwendung ein.

    [![Screenshot des Popupdialogfelds zum Erstellen einer neuen App](media/luis-quickstart-new-app/create-new-app-dialog.png "Screenshot des Popupdialogfelds zum Erstellen einer neuen App")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Wählen Sie Ihre Anwendungskultur aus. Wählen Sie für die Home Automation-App die Kultur „English“ (Englisch) aus. Wählen Sie dann **Fertig** aus. LUIS erstellt die Home Automation-App. 

    >[!NOTE]
    >Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

## <a name="add-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

Klicken Sie im linken Navigationsbereich auf **Prebuilt domains** (Vordefinierte Domänen). Suchen Sie dann nach „Home“. Wählen Sie **Add domain** (Domäne hinzufügen) aus.

[![Screenshot der Domäne „HomeAutomation“ im Menü „Prebuilt domains“ (Vordefinierte Domänen)](media/luis-quickstart-new-app/home-automation.png "Screenshot der Domäne „HomeAutomation“ im Menü „Prebuilt domains“ (Vordefinierte Domänen)")](media/luis-quickstart-new-app/home-automation.png)

Wenn die Domäne erfolgreich hinzugefügt wurde, enthält das Feld für die vordefinierte Domäne eine Schaltfläche **Remove domain** (Domäne entfernen).

[![Screenshot der Domäne „HomeAutomation“ mit Schaltfläche zum Entfernen](media/luis-quickstart-new-app/remove-domain.png "Screenshot der Domäne „HomeAutomation“ mit Schaltfläche zum Entfernen")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Absichten und Entitäten

Klicken Sie im linken Navigationsbereich auf **Intents** (Absichten), um die Absichten der Domäne „HomeAutomation“ zu überprüfen. Für jede Absicht sind Beispieläußerungen verfügbar.

> [!NOTE]
> **None** (Keine) ist eine Absicht, die von allen LUIS-Apps bereitgestellt wird. Diese Absicht verwenden Sie zum Behandeln von Äußerungen, die keiner Funktionalität Ihrer App entsprechen. 

Wählen Sie die Absicht **HomeAutomation.TurnOff** aus. Wie Sie sehen, enthält die Absicht eine Liste von Äußerungen, die mit Entitäten bezeichnet sind.

[![Screenshot der Absicht „HomeAutomation.TurnOff“](media/luis-quickstart-new-app/home-automation-turnon.png "Screenshot der Absicht „HomeAutomation.TurnOff“")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Testen Ihrer App
Nachdem Sie Ihre App trainiert haben, können Sie sie testen. Klicken Sie auf der oberen Navigationsleiste auf **Test** (Testen). Geben Sie eine Testäußerung wie „Turn off the lights“ (Mach das Licht aus) in den Bereich für interaktive Tests ein, und drücken Sie die EINGABETASTE. 

```
Turn off the lights
```

Überprüfen Sie, ob die Absicht mit der höchsten Bewertung der Absicht entspricht, die Sie für die einzelnen Testäußerungen erwartet haben.

In diesem Beispiel wurde „Turn off the lights“ korrekt als Absicht mit der höchsten Bewertung von „HomeAutomation.TurnOff“ identifiziert.

[![Screenshot des Testbereichs mit hervorgehobener Äußerung](media/luis-quickstart-new-app/test.png "Screenshot des Testbereichs mit hervorgehobener Äußerung")](media/luis-quickstart-new-app/test.png)


Klicken Sie erneut auf **Test** (Testen), um den Testbereich zuzuklappen. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Gehen Sie in der Adresse an das Ende der URL, geben Sie `turn off the living room light` ein, und drücken Sie dann die EINGABETASTE. Der Browser zeigt die JSON-Antwort Ihres HTTP-Endpunkts an.

    [![Screenshot des Browsers mit dem JSON-Ergebnis für die erkannte Absicht „TurnOff“](media/luis-quickstart-new-app/turn-off-living-room.png "Screenshot des Browsers mit dem JSON-Ergebnis für die erkannte Absicht „TurnOff“")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können den Endpunkt im Code aufrufen:

> [!div class="nextstepaction"]
> [Aufrufen eines LUIS-Endpunkts mithilfe von Code](luis-get-started-cs-get-intent.md)

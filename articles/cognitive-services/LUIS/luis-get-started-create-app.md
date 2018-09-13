---
title: Erstellen Ihrer ersten LUIS-App (Language Understanding) in zehn Minuten– Cognitive Services LUIS | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: diberry
ms.openlocfilehash: 457f23936dec0cf85e9aebbf3e54bba37c2f3ca3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2018
ms.locfileid: "43769942"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Schnellstart: Verwenden der vordefinierten Home Automation-App

In dieser Schnellstartanleitung erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel benötigen Sie ein kostenloses LUIS-Konto, das Sie im LUIS-Portal unter [http://www.luis.ai](http://www.luis.ai) erstellt haben. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
Sie können Ihre Anwendungen auf der Seite **Meine Apps** erstellen und verwalten. 

1. Melden Sie sich beim LUIS-Portal an.

2. Klicken Sie auf **Create new app** (Neue App erstellen).

    [![](media/luis-quickstart-new-app/app-list.png "Screenshot der App-Liste")](media/luis-quickstart-new-app/app-list.png)

3. Geben Sie im Dialogfeld den Namen „Home Automation“ für Ihre Anwendung ein.

    [![](media/luis-quickstart-new-app/create-new-app-dialog.png "Screenshot: Popupdialogfenster zum Erstellen einer neuen App")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Wählen Sie Ihre Anwendungskultur aus. Wählen Sie für die Home Automation-App die Kultur „English“ (Englisch) aus. Wählen Sie dann **Fertig** aus. LUIS erstellt die Home Automation-App. 

    >[!NOTE]
    >Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

## <a name="add-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

Klicken Sie im linken Navigationsbereich auf **Prebuilt domains** (Vordefinierte Domänen). Suchen Sie dann nach „Home“. Wählen Sie **Add domain** (Domäne hinzufügen) aus.

[![](media/luis-quickstart-new-app/home-automation.png "Screenshot der Domäne „HomeAutomation“ im Menü „Prebuilt domains“ (Vordefinierte Domänen)")](media/luis-quickstart-new-app/home-automation.png)

Wenn die Domäne erfolgreich hinzugefügt wurde, enthält das Feld für die vordefinierte Domäne eine Schaltfläche **Remove domain** (Domäne entfernen).

[![](media/luis-quickstart-new-app/remove-domain.png "Screenshot der Domäne „HomeAutomation“ mit Schaltfläche zum Entfernen")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Absichten und Entitäten

Klicken Sie im linken Navigationsbereich auf **Intents** (Absichten), um die Absichten der Domäne „HomeAutomation“ zu überprüfen. 

[![](media/luis-quickstart-new-app/home-automation-intents.png "Screenshot der Liste „Intents“ (Absichten) mit hervorgehobenen Namen der Absichten in der Tabelle")](media/luis-quickstart-new-app/home-automation-intents.png)

Für jede Absicht sind Beispieläußerungen verfügbar.

> [!NOTE]
> **None** (Keine) ist eine Absicht, die von allen LUIS-Apps bereitgestellt wird. Diese Absicht verwenden Sie zum Behandeln von Äußerungen, die keiner Funktionalität Ihrer App entsprechen. 

Wählen Sie die Absicht **HomeAutomation.TurnOff** aus. Wie Sie sehen, enthält die Absicht eine Liste von Äußerungen, die mit Entitäten bezeichnet sind.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Screenshot der Absicht „HomeAutomation.TurnOff“")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-your-app"></a>Trainieren Ihrer App

Klicken Sie auf der oberen Navigationsleiste auf **Train** (Trainieren).

[![](media/luis-quickstart-new-app/trained.png "Screenshot der Absicht „HomeAutomation.TurnOff“ mit grüner Erfolgsmeldung")](media/luis-quickstart-new-app/trained.png)

## <a name="test-your-app"></a>Testen Ihrer App
Nachdem Sie Ihre App trainiert haben, können Sie sie testen. Klicken Sie auf der oberen Navigationsleiste auf **Test** (Testen). Geben Sie eine Testäußerung wie „Turn off the lights“ (Mach das Licht aus) in den Bereich für interaktive Tests ein, und drücken Sie die EINGABETASTE. 

```
Turn off the lights
```

Überprüfen Sie, ob die Absicht mit der höchsten Bewertung der Absicht entspricht, die Sie für die einzelnen Testäußerungen erwartet haben.

In diesem Beispiel wurde „Turn off the lights“ korrekt als Absicht mit der höchsten Bewertung von „HomeAutomation.TurnOff“ identifiziert.

[![](media/luis-quickstart-new-app/test.png "Screenshot des Testbereichs mit hervorgehobener Äußerung")](media/luis-quickstart-new-app/test.png)


Klicken Sie erneut auf **Test** (Testen), um den Testbereich zuzuklappen. 

## <a name="publish-your-app"></a>Veröffentlichen der App
Klicken Sie auf der oberen Navigationsleiste auf **Publish** (Veröffentlichen). 

[![](media/luis-quickstart-new-app/publish.png "Screenshot der App mit hervorgehobener Schaltfläche „Publish“ (Veröffentlichen)")](media/luis-quickstart-new-app/publish.png)

Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus.

Die grüne Benachrichtigungsleiste im oberen Bereich zeigt an, dass die App erfolgreich veröffentlicht wurde.

[![](media/luis-quickstart-new-app/published.png "Screenshot der App nach erfolgreicher Veröffentlichung")](media/luis-quickstart-new-app/published.png)

Nach der erfolgreichen Veröffentlichung können Sie die auf der Seite **Publish app** (App veröffentlichen) angezeigte Endpunkt-URL verwenden.

[![](media/luis-quickstart-new-app/endpoint.png "Screenshot der Seite „Publish app“ (App veröffentlichen) mit hervorgehobener Endpunkt-URL")](media/luis-quickstart-new-app/endpoint.png)

## <a name="use-your-app"></a>Verwenden der App
Sie können Ihren veröffentlichten Endpunkt mithilfe der generierten URL in einem Browser testen. Öffnen Sie diese URL in Ihrem Browser, und legen Sie den URL-Parameter für Ihre Testabfrage auf „&1q“ fest. Fügen Sie beispielsweise `turn off the living room light` am Ende Ihrer URL hinzu, und drücken Sie dann die EINGABETASTE. Der Browser zeigt die JSON-Antwort Ihres HTTP-Endpunkts an.


[![](media/luis-quickstart-new-app/turn-off-living-room.png "Screenshot des Browsers mit dem JSON-Ergebnis für die erkannte Absicht „TurnOff“")](media/luis-quickstart-new-app/turn-off-living-room.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie dazu in der Liste rechts vom App-Namen die Auslassungspunkte (***...***) und dann **Delete** (Löschen) aus. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

Sie können den Endpunkt im Code aufrufen:

> [!div class="nextstepaction"]
> [Aufrufen eines LUIS-Endpunkts mithilfe von Code](luis-get-started-cs-get-intent.md)

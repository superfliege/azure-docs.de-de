---
title: Demo einer Unterhaltungslernanwendung, Pizzabestellung – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie eine Demo einer Unterhaltungslernanwendung erstellen.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99cd89c4f4430f2d65ed0963e3092d51a83842d7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376394"
---
# <a name="demo-pizza-order"></a>Demo: Pizzabestellung
In dieser Demo wird ein Pizzabestellungsbot gezeigt. Er ermöglicht das Bestellen einer einzelnen Pizza mithilfe der folgenden Funktionalität:

- Erkennen von Pizzabelägen in Benutzeräußerungen
- Prüfen, ob Pizzabeläge vorrätig sind oder nicht, und entsprechendes Antworten
- Speichern von Pizzabelägen aus einer vorherigen Bestellung und Anbieten einer neuen Bestellung mit den gleichen Belägen

## <a name="requirements"></a>Anforderungen
Für dieses Tutorial muss der Pizzabestellungsbot ausgeführt werden.

    npm run demo-pizza

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste der Webbenutzeroberfläche auf „TutorialDemo Pizza Order“. 

## <a name="entities"></a>Entitäten

Wir haben drei Entitäten erstellt.

- Toppings: Sammelt die Beläge, um die der Benutzer gebeten hat. Die gültigen Beläge, die vorrätig sind, werden aufgenommen. Es wird geprüft, ob ein Belag vorrätig ist oder nicht.
- OutofStock: Hiermit wird dem Benutzer zurückgemeldet, dass der ausgewählte Belag nicht vorrätig ist.
- LastToppings: Nachdem eine Bestellung aufgegeben wurde, wird mithilfe dieser Entität dem Benutzer die Liste der in seiner Bestellung genannten Beläge angeboten.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Aktionen

Wir haben eine Reihe von Aktionen erstellt. Dazu gehört die Frage, welchen Belag der Benutzer auf seiner Pizza wünscht, die Angabe, welche Beläge bereits hinzugefügt sind, usw.

Es finden auch zwei API-Aufrufe statt:

- FinalizeOrder: Hiermit wird die Pizzabestellung aufgegeben.
- UseLastToppings: Hiermit werden die Beläge aus der vorherigen Bestellung übernommen. 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Trainingsdialoge
Wir haben einige Trainingsdialoge definiert. 

![](../media/tutorial_pizza_dialogs.PNG)

Probieren Sie als Beispiel eine Trainingssitzung aus.

1. Klicken Sie erst auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „order a pizza“ (Pizza bestellen) ein.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Wählen Sie „what would you like on your pizza?“ (Welchen Belag wünschen Sie auf Ihrer Pizza) durch Klicken aus.
4. Geben Sie „mushrooms and cheese“ (Pilze und Käse) ein.
    - Beachten Sie, dass LUIS beides als Beläge gekennzeichnet hat. Wenn das nicht richtig wäre, könnten Sie darauf klicken, um die Einträge hervorzuheben, und sie dann ändern.
    - Das Pluszeichen neben der Entität bedeutet, dass sie zur Gruppe der Beläge hinzugefügt wird.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass Pilze und Käse nicht im Speicher für Beläge enthalten sind.
3. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) durch Klicken aus.
    - Beachten Sie, dass dies eine Aktion ohne Wartezeit ist und der Bot daher zur nächsten Aktion auffordert.
6. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
7. Geben Sie „remove mushrooms and add peppers“ (Pilze entfernen und Paprika hinzufügen) ein.
    - Beachten Sie, dass neben **mushrooms** (Pilze) ein Minuszeichen angezeigt wird, damit diese Auswahl entfernt wird. Neben „peppers“ (Paprika) hingegen wird ein Pluszeichen zum Hinzufügen zu den Belägen angezeigt.
2. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass **peppers** (Paprika) jetzt fett formatiert ist, da dieser Eintrag neu ist. **Mushrooms** (Pilze) hingegen ist durchgestrichen.
8. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) durch Klicken aus.
6. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
7. Geben Sie „add peas“ (Erbsen hinzufügen) ein.
    - Erbsen sind ein Beispiel für einen Belag, der nicht vorrätig ist. Beachten Sie, dass dieser dennoch als Belag gekennzeichnet ist.
2. Klicken Sie auf „Bewertungsaktionen“.
    - Erbsen werden als nicht vorrätig (OutOfStock) angezeigt.
    - Um zu sehen, wie das funktioniert, öffnen Sie den Code unter „C:\<\installedpath>\src\demos\demoPizzaOrder.ts“. Beachten Sie die „EntityDetectionCallback“-Methode. Diese Methode wird nach jeder Auswahl eines Belags aufgerufen, um festzustellen, ob er vorrätig ist. Ist das nicht der Fall, wird er aus der Gruppe der Beläge gelöscht und zur Entität „OutOfStock“ hinzugefügt. Die „inStock“-Variable ist oberhalb dieser Methode definiert und enthält die Liste der vorrätigen Beläge.
6. Wählen Sie „We don't have $OutOfStock“ ($OutOfStock ist nicht vorrätig) aus.
7. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
8. Geben Sie „no“ (Nein) ein.
9. Klicken Sie auf „Bewertungsaktionen“.
10. Wählen Sie den API-Aufruf „FinalizeOrder“ aus. 
    - Dadurch wird die im Code definierte Funktion „FinalizeOrder“ aufgerufen. Die Anzeige der Beläge wird gelöscht, und „your order is on its way“ (Ihre Bestellung wird bearbeitet) wird zurückgegeben. 
2. Geben Sie „order another“ (Weitere Bestellung) ein. Es beginnt nun eine neue Bestellung.
9. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, das Käse und Paprika als Beläge aus der letzten Bestellung im Speicher enthalten sind.
1. Wählen Sie „Would you like $LastToppings“ (Wünschen Sie $LastToppings) aus.
2. Geben Sie „yes“ (Ja) ein.
3. Klicken Sie auf „Bewertungsaktionen“.
    - Der Bot möchte die Aktion „UseLastToppings“ ausführen. Dies ist die zweite der beiden Rückrufmethoden. Sie kopiert die Beläge der letzten Bestellung und beendet die Anzeige dieser Beläge. Auf diese Weise wird die letzte Bestellung gespeichert, und wenn der Benutzer eine weitere Pizza wünscht, werden diese Beläge als Optionen angezeigt.
2. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) durch Klicken aus.
3. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
8. Geben Sie „no“ (Nein) ein.
4. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Demo – VR-App-Starter](./demo-vr-app-launcher.md)

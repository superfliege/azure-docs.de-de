---
title: Verwenden von Branching- und Rückgängig-Aktionen in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Branching- und Rückgängig-Aktionen in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 05140693026e21a73b756ed0ea7bc9936bef067e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173297"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Verwenden von Branching- und Rückgängig-Vorgängen
In diesem Tutorial befassen wir uns mit Rückgängig- und Branching-Vorgängen.


## <a name="details"></a>Details
- Rückgängig: ermöglicht es dem Entwickler, eine Benutzereingabe oder eine Aktionsauswahl „rückgängig“ zu machen. Hinter den Kulissen erstellt „rückgängig“ tatsächlich einen neuen Dialog und spielt ihn bis zum vorhergehenden Schritt ab.  Dies bedeutet, dass der Rückruf zur Entitätserkennung und die API-Aufrufe im Dialog erneut aufgerufen werden.

- Branch: erstellt einen neuen Trainingsdialog, der genau so beginnt wie der vorhandene Trainingsdialog – dadurch entfällt die Mühe, Dialogwendungen manuell erneut einzugeben. Hinter den Kulissen erstellt „branch“ einen neuen Dialog und spielt den vorhandenen Trainingsdialog noch einmal bis zum ausgewählten Schritt ab.  Dies bedeutet, dass der Rückruf zur Entitätserkennung und die API-Aufrufe im Dialog erneut aufgerufen werden.


## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der Pizzabestellungsbot ausgeführt werden:

    npm run demo-pizza

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „TutorialDemo Pizza Order“. 

Details zur Pizzabestellungsdemo finden Sie im Pizzabestellungstutorial.

## <a name="undo"></a>Rückgängig

Wir werden einen Teil des Dialogs rückgängig machen und ihn von diesem Schritt an neu erstellen.

### <a name="training-dialogs"></a>Trainingsdialoge
Beginnen wir eine Trainingssitzung. 

1. Klicken Sie erst auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „order a pizza“ (Pizza bestellen) ein.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Wählen Sie „what would you like on your pizza?“ (Welchen Belag wünschen Sie auf Ihrer Pizza) durch Klicken aus.
4. Geben Sie „mushrooms and cheese“ (Pilze und Käse) ein.
5. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
3. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) durch Klicken aus.
6. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
7. Geben Sie „remove mushrooms and add peppers“ (Pilze entfernen und Paprika hinzufügen) ein.
    - Wählen Sie „mushrooms“ (Pilze) aus, und deaktivieren Sie die Entität „Toppings“. Wir erstellen eine Aktion, die wir rückgängig machen werden.
2. Klicken Sie auf „Undo Step“ (Schritt rückgängig machen).
    - Der letzte Eintrag wird entfernt und wir sind wieder bei „Wünschen Sie sonst noch etwas?“.  Siehe den Screenshot unten.
2. Geben Sie „remove mushrooms and add peppers“ (Pilze entfernen und Paprika hinzufügen) ein.
8. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) durch Klicken aus.
    - Achten Sie darauf, dass beide Einträge ordnungsgemäß ausgewählt sind.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion). Sie können jetzt mit dem korrigierten Dialogfeld fortfahren.
4. Klicken Sie auf „Training abgeschlossen“.

Sie haben jetzt gesehen, wie „rückgängig“ zum Entfernen einer Benutzereingabe und einer Aktion verwendet wird.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Verzweigung

Lassen Sie uns als Beispiel einen vorhandenen Trainingsdialog öffnen und durch Verzweigen (Branchen) einen weiteren Trainingsdialog erstellen.

1. Klicken Sie auf „Train Dialogs“ (Trainingsdialoge) und dann auf „new order“ (neue Bestellung), um den vorhandenen Dialog zu öffnen. 
2. Klicken Sie auf das letzte „No“ (Nein) im Dialog (siehe den Screenshot unten).
3. Klicken Sie auf „Branch“.
    - „Nein“ wird entfernt und der gesamte Dialog bis zu diesem Punkt wird in einen neuen kopiert. 
    - Dies erspart Ihnen die erneute Eingabe der vorhergehenden Wendungen, um eine neue Verzweigung ab diesem Punkt zu untersuchen.
1. Geben Sie „yes“ (Ja) ein.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Wählen Sie „you have $Toppings on your pizza“ (Sie haben $Toppings auf Ihrer Pizza) aus.
6. Wählen Sie „Would you like anything else?“ (Wünschen Sie sonst noch etwas?) aus.
7. Geben Sie „No“ (Nein) ein.
4. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Versionsverwaltung und Tagging](./16-versioning-and-tagging.md)

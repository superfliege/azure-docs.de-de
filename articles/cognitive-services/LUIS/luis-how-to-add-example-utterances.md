---
title: Hinzufügen von Beispieläußerungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Beispieläußerungen sind Textbeispiele für Benutzerfragen oder -befehle. Fügen Sie einer Absicht Beispieläußerungen hinzu, um Language Understanding-Apps zu trainieren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 33c941f84952faca1961bb65687b4098b837a2fd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139167"
---
# <a name="add-an-entity-to-example-utterances"></a>Hinzufügen einer Entität zu Beispieläußerungen 

Beispieläußerungen sind Textbeispiele für Benutzerfragen oder -befehle. Fügen Sie [Beispieläußerungen](luis-concept-utterance.md) zu einer [Absicht](luis-concept-intent.md) hinzu, um Language Understanding-Apps zu trainieren.

Im Allgemeinen fügen Sie einer Absicht zunächst eine Beispieläußerung hinzu und erstellen dann Entitäten und bezeichnen Äußerungen auf der Seite der Absicht. Wenn Sie zuerst Entitäten erstellen möchten, finden Sie weitere Informationen unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).

## <a name="marking-entities-in-example-utterances"></a>Markieren von Entitäten in Beispieläußerungen

Wenn Sie Text in der Beispieläußerung für eine Entität markieren, wird ein direktes Popupmenü angezeigt. Erstellen in diesem Menü eine Entität, oder wählen Sie sie aus. 

Bestimmte Entitätstypen, z.B. vordefinierte Entitäten und RegEx-Entitäten, können in der Beispieläußerung nicht markiert werden, da sie automatisch markiert werden. 

## <a name="add-a-simple-entity"></a>Hinzufügen einer einfachen Entität

Mit dem folgenden Verfahren erstellen und markieren Sie eine benutzerdefinierte Entität innerhalb der folgenden Äußerung auf der Seite der Absicht:

```text
Does John Smith work in Seattle?
```

1. Wählen Sie `Seattle` in der Äußerung aus, um den Begriff als einfache Entität zu bezeichnen.

    [![Screenshot: Auswählen von Text in der Äußerung für eine einfache Entität](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)

    > [!NOTE]
    > Wenn Sie Wörter auswählen, um sie als Entitäten zu markieren, gilt Folgendes:
    > * Wählen Sie bei einem einzelnen Wort nur dieses Wort aus. 
    > * Wählen Sie bei mindestens zwei Wörtern den Anfang und das Ende der Wortgruppe aus.

1. Im Dropdownfeld der Entität, das angezeigt wird, können Sie eine vorhandene Entität auszuwählen oder eine neue Entität hinzufügen. Geben Sie den Namen der neuen Entität, die Sie hinzufügen möchten, in das Textfeld ein, und wählen Sie anschließend **Neue Entität erstellen** aus. 

    ![Screenshot: Eingeben des Entitätsnamens](./media/luis-how-to-add-example-utterances/hr-create-simple-2.png)

1. Überprüfen Sie im Popupfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?) den Entitätsnamen, und wählen Sie den Entitätstyp **Einfach** und anschließend **Fertig** aus.

    Normalerweise wird eine [Ausdrucksliste](luis-concept-feature.md) verwendet, um das Signal einer einfachen Entität zu verstärken.

## <a name="add-a-list-entity"></a>Hinzufügen einer Listenentität

Listenentitäten stellen eine feste, abgeschlossene Gruppe verwandter Wörter mit genauer Textübereinstimmung in Ihrem System dar. 

Für die Abteilungsliste eines Unternehmens können Sie normalisierte Werte verwenden: `Accounting` und `Human Resources`. Jeder normalisierte Name verfügt über Synonyme. Für eine Abteilung können diese Synonyme Abteilungskürzel, Nummern oder Jargon umfassen. Sie müssen nicht alle Werte kennen, wenn Sie die Entität erstellen. Sie können weitere Werte hinzufügen, nachdem Sie wirkliche Benutzeräußerungen mit Synonymen überprüft haben.

1. Wählen Sie in der Liste der Beispieläußerungen für eine bestimmte Äußerung das Wort oder den Ausdruck aus, die in der neuen Liste enthalten sein sollen. Geben Sie dann den Namen der Liste im oberen Textfeld ein, und wählen Sie **Neue Entität erstellen** aus.   

    ![Screenshot: Eingeben des Namens der Listenentität](./media/luis-how-to-add-example-utterances/hr-create-list-1.png)


1. Benennen Sie die Entität im Popupfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?), und wählen Sie **Liste** als Typ aus. Fügen Sie Synonyme für dieses Listenelement hinzu, und wählen Sie dann **Fertig** aus. 

    ![Screenshot: Eingeben von Synonymen für Listenentität](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    Sie können weitere Listenelemente oder Synonyme hinzufügen, indem Sie Bezeichnungen für andere Äußerungen vergeben oder indem Sie die Entität im linken Bereich unter **Entitäten** bearbeiten. Durch die [Bearbeitung](luis-how-to-add-entities.md#add-list-entities) von Entitäten können Sie zusätzliche Elemente mit entsprechenden Synonymen eingeben oder eine Liste importieren. 

## <a name="add-composite-entity"></a>Zusammengesetzte Entität hinzufügen

Zusammengesetzte Entitäten werden aus vorhandenen **Entitäten** in einer übergeordneten Entität erstellt. 

Bei der Äußerung `Does John Smith work in Seattle?` kann eine zusammengesetzte Entität Entitätsinformationen zum Mitarbeiternamen und dem Ort in einem einzigen übergeordneten Objekt zurückgeben. 

Der Mitarbeitername „John Smith“ ist eine vordefinierte Entität vom Typ [personName](luis-reference-prebuilt-person.md). Der Ort „Seattle“ ist eine benutzerdefinierte einfache Entität. Nachdem diese beiden Entitäten erstellt und in einer Beispieläußerung markiert wurden, können sie in einer zusammengesetzten Entität umschlossen werden. 

1. Um die einzelnen Entitäten in einer zusammengesetzten Entität zu umschließen, wählen Sie die **erste** bezeichnete Entität (ganz links) in der Äußerung für die zusammengesetzte Entität aus. In einer Dropdownliste werden die Optionen für diese Auswahl angezeigt.

1. Wählen Sie **Wrap composite entity** (Zusammengesetzte Entität umschließen) aus der Dropdownliste aus. 

    ![Screenshot: Auswählen von „Wrap in composite entity“ (Zusammengesetzte Entität umschließen)](./media/luis-how-to-add-example-utterances/hr-create-composite-1.png)

1. Wählen Sie das letzte Wort der zusammengesetzten Entität aus (ganz rechts). Die zusammengesetzte Entität ist mit einer grünen Linie unterstrichen.

1. Geben Sie den Namen der zusammengesetzten Entität in der Dropdownliste ein.

    ![Screenshot der Eingabe des Namens der zusammengesetzten Entität in der Dropdownliste ein.](./media/luis-how-to-add-example-utterances/hr-create-composite-2.png)

    Wenn Sie die Entitäten ordnungsgemäß umschließen, wird eine grüne Linie unter dem gesamten Ausdruck angezeigt.

1. Überprüfen Sie die Details der zusammengesetzten Entität im Popupfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?), und wählen Sie dann **Fertig** aus.

    ![Screenshot des Popupelements mit den Details der Entität](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. Die zusammengesetzte Entität wird mit blauer Hervorhebung einzelner Entitäten und einer grünen Unterstreichung der gesamten zusammengesetzten Entität angezeigt. 

    ![Screenshot der Seite „Intents details“ (Details zu Absichten) mit zusammengesetzter Entität](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-hierarchical-entity"></a>Hinzufügen einer Entität vom Typ „Hierarchisch“

Eine hierarchische Entität ist eine Kategorie von im Kontext erlernten und konzeptionell verwandten Entitäten. Die Entität im folgenden Beispiel enthält Abflug- und Zielorte. 

In der Äußerung `Move John Smith from Seattle to Cairo` ist Seattle der Abflug- und Kairo der Zielort. Die Orte unterscheiden sich durch den Kontext und werden aufgrund der Wortstellung und Wortwahl in der Äußerung erlernt.

1. Wählen Sie auf der Seite der Absicht in der Äußerung den Ort `Seattle` aus, geben Sie als Namen der Entität `Location` an, und drücken Sie dann auf der Tastatur die EINGABETASTE.

    ![Screenshot des Dialogfelds zum Erstellen der Beschriftung einer hierarchischen Entität](./media/luis-how-to-add-example-utterances/hr-hier-1.png)

1. Wählen Sie im Popupfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?) _Hierarchisch_ für den **Entitätstyp** aus, fügen Sie anschließend `Origin` und `Destination` als untergeordnete Elemente hinzu, und wählen Sie dann **Fertig** aus.

    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit der hervorgehobenen Option „ToLocation“ (zum Zielort)](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

1. Das Wort in der Äußerung wurde mit der hierarchischen übergeordneten Entität beschriftet. Sie müssen das Wort einer untergeordneten Entität zuweisen. Kehren Sie zur Äußerung auf der Seite der Absichtsdetails zurück. Wählen Sie das Wort aus. Wählen Sie anschließend aus der Dropdownliste den von Ihnen erstellten Entitätsnamen aus, und folgen Sie dann der Menüstruktur nach rechts, um die richtige untergeordnete Entität auszuwählen.

    ![Screenshot der Detailseite „Absichten“, auf der Sie das Wort einer untergeordneten Entität zuweisen](./media/luis-how-to-add-example-utterances/hr-hier-3.png)

    >[!CAUTION]
    >Die Namen untergeordneter Entitäten müssen für alle Entitäten einer einzelnen App eindeutig sein. Zwei unterschiedliche hierarchische Entitäten dürfen keine untergeordneten Elemente mit dem gleichen Namen enthalten. 

## <a name="entity-status-predictions"></a>Vorhersagen zum Entitätsstatus

Wenn Sie im LUIS-Portal eine neue Äußerung eingeben, enthält diese Äußerung möglicherweise Vorhersagefehler für die Entität. Der Vorhersagefehler ist der Unterschied zwischen der Bezeichnung einer Entität und der Vorhersage zur Entität durch LUIS. 

Dieser Unterschied wird im LUIS-Portal in der Äußerung rot unterstrichen. Die rote Unterstreichung kann in den Klammern der Entität oder außerhalb der Klammern angezeigt werden. 

![Screenshot der Vorhersageabweichung für den Entitätsstatus](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Wählen Sie die rot unterstrichenen Wörter in der Äußerung aus. 

Im Entitätsfeld wird der **Entitätsstatus** mit einem roten Ausrufezeichen angezeigt, wenn eine Abweichung zur Vorhersage vorliegt. Wählen Sie zum Anzeigen des Entitätsstatus mit Informationen zu den Unterschieden zwischen den bezeichneten und vorhergesagten Entitäten die Option **Entitätsstatus** und dann das Element auf der rechten Seite aus.

![Screenshot der Auswahl des richtigen Elements zum Beheben der Abweichung bei der Vorhersage](./media/luis-how-to-add-example-utterances/entity-status.png)

Die rote Linie kann in folgenden Situationen angezeigt werden:

   * Wenn eine Äußerung eingegeben wird, bevor die Entität bezeichnet wurde
   * Wenn die Entitätsbezeichnung angewandt wird
   * Wenn die Entitätsbezeichnung entfernt wird
   * Wenn für den Text mehrere Entitätsbezeichnungen vorhergesagt werden 

Die folgenden Lösungen beheben Abweichungen bei Entitätsvorhersagen:

|Entität|Visueller Indikator|Vorhersage|Lösung|
|--|--|--|--|
|Die Äußerung wurde eingegeben, aber die Entität wurde noch nicht bezeichnet.|rot unterstrichen|Die Vorhersage ist falsch.|Bezeichnen Sie die Entität mit dem vorhergesagten Wert.|
|Nicht bezeichneter Text|rot unterstrichen|Falsche Vorhersage|Die aktuellen Äußerungen, die diese falsche Entität verwenden, müssen für alle Absichten überprüft werden. Die aktuellen Äußerungen haben LUIS fälschlicherweise trainiert, diesen Text als die vorhergesagte Entität anzusehen.
|Richtig bezeichneter Text|blaue Entitätsmarkierung, rot unterstrichen|Falsche Vorhersage|Stellen Sie weitere Äußerungen mit der richtig bezeichneten Entität an verschiedenen Stellen und in unterschiedlichen Verwendungen bereit. Die aktuellen Äußerungen sind entweder nicht ausreichend, um LUIS für diese Entität zu trainieren, oder ähnliche Entitäten kommen im selben Kontext vor. Ähnliche Entitäten sollten in einer einzigen Entität zusammengefasst werden, damit LUIS nicht verwirrt wird. Eine weitere Lösung besteht darin, eine Ausdrucksliste hinzuzufügen, um die Wichtigkeit der Wörter zu steigern. |
|Falsch bezeichneter Text|blaue Entitätsmarkierung, rot unterstrichen|Richtige Vorhersage| Stellen Sie weitere Äußerungen mit der richtig bezeichneten Entität an verschiedenen Stellen und in unterschiedlichen Verwendungen bereit. 

## <a name="other-actions"></a>Andere Aktionen

Sie können Aktionen für Beispieläußerungen als ausgewählte Gruppe oder als einzelnes Element ausführen. Bei Gruppen von ausgewählten Beispieläußerungen ändert sich das Kontextmenü oberhalb der Liste. Für einzelne Elemente können das Kontextmenü oberhalb der Liste sowie die einzelnen kontextbezogenen Auslassungspunkte am Ende jeder Zeile einer Äußerung verwendet werden. 

### <a name="remove-entity-labels-from-utterances"></a>Entfernen von Entitätsbeschriftungen aus Äußerungen

Sie können durch maschinelles Lernen erworbene Entitätsbeschriftungen aus einer Äußerung auf der Seite der Absicht entfernen. Wenn die Entität nicht durch maschinelles Lernen erworben wurde, kann sie nicht aus der Äußerung entfernt werden. Wenn Sie eine nicht durch maschinelles Lernen erworbene Entität aus der Äußerung entfernen möchten, müssen Sie die Entität aus der gesamten App löschen. 

Um eine durch maschinelles Lernen erworbene Entität aus einer Äußerung zu entfernen, wählen Sie die Entität in der Äußerung aus. Wählen Sie anschließend im angezeigten Dropdownfeld der Entität die Option **Beschriftung entfernen** aus.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Option „Beschriftung entfernen“](./media/luis-how-to-add-example-utterances/remove-label.png) 

### <a name="add-prebuilt-entity-label"></a>Hinzufügen von vordefinierten Entitätsbeschriftungen

Wenn Sie Ihrer LUIS-App die vordefinierten Entitäten hinzufügen, müssen Sie die Äußerungen nicht mit diesen Entitäten markieren. Weitere Informationen zu vordefinierten Entitäten und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-prebuilt-entity).

### <a name="add-regular-expression-entity-label"></a>Hinzufügen einer Entitätsbeschriftung vom Typ „Regulärer Ausdruck“

Wenn Sie Ihrer LUIS-App die Entitäten vom Typ „Regulärer Ausdruck“ hinzufügen, müssen Sie die Äußerungen nicht mit diesen Entitäten markieren. Weitere Informationen zu Entitäten vom Typ „Regulärer Ausdruck“ und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-regular-expression-entities).


### <a name="create-a-pattern-from-an-utterance"></a>Erstellen eines Musters aus einer Äußerung

Weitere Informationen finden Sie unter [Hinzufügen eines Musters aus einer vorhandenen Äußerung auf der Seite der Absicht oder der Entität](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).


### <a name="add-patternany-entity"></a>Hinzufügen einer pattern.any-Entität

Wenn Sie die „Pattern.any“-Entitäten zu Ihrer LUIS-App hinzufügen, können Sie die Äußerungen nicht mit diesen Entitäten beschriften. Sie sind nur in Mustern gültig. Weitere Informationen zu „Pattern.any“-Entitäten und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-patternany-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Trainieren Ihrer App nach dem Ändern des Modells mit Äußerungen

Nachdem Sie Äußerungen hinzugefügt, bearbeitet oder entfernt haben, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen Auswirkungen auf Endpunktabfragen zeigen. 

## <a name="next-steps"></a>Nächste Schritte

Nach dem Beschriften von Äußerungen in Ihren Absichten können Sie jetzt eine [zusammengesetzte Entität](luis-how-to-add-entities.md) erstellen.

---
title: Hinzufügen von Beispieläußerungen in LUIS-Apps
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Äußerungen in Language Understanding-Anwendungen (LUIS) hinzufügen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/10/2018
ms.author: diberry
ms.openlocfilehash: 6a3edfd426fcdce83bd60332ba2b1ff6224dae1a
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645558"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Hinzufügen von Beispieläußerungen und -beschriftungen bei Entitäten

Beispieläußerungen sind Textbeispiele für Benutzerfragen oder -befehle. Fügen Sie [Beispieläußerungen](luis-concept-utterance.md) zu einer [Absicht](luis-concept-intent.md) hinzu, um Language Understanding-Apps zu trainieren.

Fügen Sie einer Absicht zunächst eine Beispieläußerung hinzu, und erstellen Sie dann Entitäten und beschriften Äußerungen auf der Seite der Absicht. Wenn Sie zuerst Entitäten erstellen möchten, finden Sie weitere Informationen unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Hinzufügen einer Äußerung
Geben Sie auf der Seite einer Absicht im Textfeld unter ihrem Namen eine relevante Beispieläußerung ein, die Sie von Ihren Benutzern erwarten, z.B. `book 2 adult business tickets to Paris tomorrow on Air France`, und drücken Sie dann die EINGABETASTE. 
 
>[!NOTE]
>LUIS konvertiert alle Äußerungen in Kleinbuchstaben.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Äußerung](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Die Äußerungen werden der Liste der Äußerungen für die aktuelle Absicht hinzugefügt. 

## <a name="ignoring-words-and-punctuation"></a>Ignorieren von Wörtern und Interpunktion
Wenn Sie bestimmte Wörter oder Satzzeichen in der Beispieläußerung ignorieren möchten, verwenden Sie ein [Muster](luis-concept-patterns.md#pattern-syntax) mit der _zu ignorierenden_ Syntax. 

## <a name="add-simple-entity-label"></a>Hinzufügen einer Beschriftung zu Entitäten vom Typ „Einfach“
Mit dem folgenden Verfahren erstellen Sie benutzerdefinierte Entitäten innerhalb der folgenden Äußerung auf der Seite der Absicht und beschriften sie:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Wählen Sie „Air France“ in der Äußerung aus, um den Begriff als einfache Entität zu beschriften.

    > [!NOTE]
    > Wenn Sie Wörter auswählen, um sie als Entitäten zu beschriften, gelten folgende Regeln:
    > * Wählen Sie bei einem einzelnen Wort nur dieses Wort aus. 
    > * Wählen Sie bei mindestens zwei Wörtern den Anfang und das Ende der Wortgruppe aus.

2. Im Dropdownfeld der Entität, das angezeigt wird, können Sie eine vorhandene Entität auszuwählen oder eine neue Entität hinzufügen. Geben Sie den Namen der neuen Entität, die Sie hinzufügen möchten, in das Textfeld ein, und wählen Sie anschließend **Neue Entität erstellen** aus. 
 
    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Option zur Beschriftung einer Entität vom Typ „Einfach“](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. Überprüfen Sie im Popupdialogfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?) den Entitätsnamen, und wählen Sie den einfachen Entitätstyp und anschließend **Fertig** aus.

    ![Bilder des Bestätigungsdialogfelds](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Weitere Informationen dazu, wie Sie die einfache Entität aus der Antwort der JSON-Endpunktabfrage extrahieren, finden Sie unter [Datenextrahierung](luis-concept-data-extraction.md#simple-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-primary-and-secondary-data.md) für einfache Entitäten aus, um mehr über das Verwenden einer einfachen Entität zu erfahren.


## <a name="add-list-entity-and-label"></a>Hinzufügen einer Entität vom Typ „Liste“ und Beschriftung
Listenentitäten stellen eine feste, abgeschlossene Gruppe verwandter Wörter mit genauer Textübereinstimmung in Ihrem System dar. 

Die Entität „drinks list“ (Getränkeliste) beispielsweise kann zwei normalisierte Werte aufweisen: Water und Soda Pop. Jeder normalisierte Name verfügt über Synonyme. Die Synonyme für Water lauten H20, gas, flat (H2O, sprudelig, still). Die Synonyme für Soda Pop lauten fruit, cola, ginger (Frucht, Cola, Ingwer). Sie müssen nicht alle Werte kennen, wenn Sie die Entität erstellen. Sie können weitere Werte hinzufügen, nachdem Sie wirkliche Benutzeräußerungen mit Synonymen überprüft haben.

|Normalisierter Name|Synonyme|
|--|--|
|Water|H20, gas, flat|
|Soda pop|Fruit, cola, ginger|

Beim Erstellen einer neuen Listenentität von der Seite der Absicht führen Sie zwei Schritte durch, die möglicherweise nicht offensichtlich sind. Erstellen Sie zunächst eine neue Liste, indem Sie das erste Listenelement hinzufügen. Benennen Sie dann das erste Listenelement mit dem Wort oder dem Ausdruck, das bzw. den Sie in der Äußerung ausgewählt haben. Sie können den Namen zwar noch zu einem späteren Zeitpunkt auf der Seite der Absicht ändern. Dennoch beschleunigen Sie den Vorgang, indem Sie eine Äußerung auswählen, die den gewünschten Namen des Listenelements bereits enthält.

Ein Beispiel: Sie möchten eine Liste von Getränkearten erstellen und haben das Wort `h2o` aus der Äußerung ausgewählt, aus der Sie die Entität erstellen. In diesem Fall besteht Ihre Liste aus einem Element mit dem Namen H2O. Wenn Sie einen allgemeineren Namen bevorzugen, sollten Sie eine Äußerung auswählen, die einen allgemeineren Namen verwendet. 

1. Wählen Sie in der Äußerung das Wort aus, das an erster Stelle in der Liste steht, geben Sie anschließend in das Textfeld den Namen der Liste ein, und wählen Sie dann **Neue Entität erstellen** aus.   

    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit der hervorgehobenen Option „Neue Entität erstellen“](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. Fügen Sie im Dialogfeld **What type of entity do you want to create?** (Welchen Entitätstyp möchten Sie erstellen?) Synonyme für dieses Listenelement hinzu. Fügen Sie für das Wasserelement in der Getränkeliste `h20`, `perrier` und `waters` hinzu, und wählen Sie **Fertig** aus. Beachten Sie, dass „waters“ (Wasserarten) hinzugefügt wird, da die Listensynonyme auf der Tokenebene übereinstimmen. Es geht um die Wortebene, d.h. „Wasserarten“ wird nicht mit „Wasser“ abgeglichen, wenn dieser Begriff nicht in der Liste enthalten ist. 

    ![Screenshot des Dialogfelds „What type of entity do you want to create“ (Welchen Entitätstyp möchten Sie erstellen?)](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Die Getränkeliste enthält jetzt nur ein Getränk, nämlich Wasser. Sie können jedoch weitere Getränke hinzufügen, indem Sie Beschriftungen für andere Äußerungen vergeben oder indem Sie die Entität unter **Entitäten** auf der linken Seite bearbeiten. Durch die [Bearbeitung](luis-how-to-add-entities.md#add-list-entities) von Entitäten können Sie zusätzliche Elemente mit entsprechenden Synonymen eingeben oder eine Liste [importieren](luis-how-to-add-entities.md#import-list-entity-values). 

    Weitere Informationen dazu, wie Sie Listenentitäten aus der Antwort der JSON-Endpunktabfrage extrahieren, finden Sie unter [Datenextrahierung](luis-concept-data-extraction.md#list-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-list-entity.md) aus, um mehr über das Verwenden einer Listenentität zu erfahren.

## <a name="add-synonyms-to-the-list-entity"></a>Hinzufügen von Synonymen zur Listenentität 
Fügen Sie der Listenentität ein Synonym hinzu, indem Sie das Wort bzw. den Ausdruck in der Äußerung auswählen. Wenn Sie einer Listenentität für Getränke das Wort `agua` als Synonym für Wasser hinzufügen möchten, führen Sie die folgenden Schritte durch:

Wählen Sie in der Äußerung das Synonym, z.B. `aqua` für Wasser, und anschließend den Namen der Entität in der Dropdownliste aus, z.B. **Drink**. Wählen Sie dann **Set as synonym** (Als Synonym festlegen) und dann das Listenelement mit der synonymen Bedeutung aus, wie etwa **Wasser**.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit der hervorgehoben Option „Create a new normalized view“ (Neue normalisierte Ansicht erstellen)](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Erstellen eines neuen Elements für die Listenentität
Fügen Sie der Listenentität ein neues Element hinzu, indem Sie das Wort bzw. den Ausdruck in der Äußerung auswählen. Wenn Sie der Mitarbeiterliste das Wort `Bob Smith` als neues Element hinzufügen möchten, führen Sie die folgenden Schritte durch:

Wählen Sie in der Äußerung das Wort oder den Ausdruck für das neue Listenelement aus, z. B. `Bob Smith`, wählen Sie anschließend den Namen der Listenentität in der Dropdownliste aus, z. B. **Employee**, und wählen Sie dann **Create a new normalized view** (Neue normalisierte Ansicht erstellen) aus. 

![Screenshot: Hinzufügen eines neuen Listenelements](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Das Wort ist jetzt blau hervorgehoben. Wenn Sie auf das Wort zeigen, wird ein Tag mit dem Namen des Listenelements angezeigt, z.B. Tee.

![Screenshot eines Tags für ein neues Listenelement](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Umschließen von Entitäten in zusammengesetzten Beschriftungen
Zusammengesetzte Entitäten werden auf der Basis von **Entitäten** erstellt. Eine zusammengesetzte Entität kann nicht auf der Seite der Absicht erstellt werden. Sobald die zusammengesetzte Entität erstellt wurde, können Sie die Entitäten in einer Äußerung auf der Seite der Absicht umschließen. 

Bei einer Äußerung wie `book 2 tickets from Seattle to Cairo` kann eine zusammengesetzte Äußerung folgende Entitätsinformationen in einer einzigen übergeordneten Entität zurückgeben: Anzahl der Tickets (2), Abflugort (Seattle) und Zielort (Kairo). 

Führen Sie die folgenden [Schritte](luis-how-to-add-entities.md#add-prebuilt-entity) durch, um die vordefinierte Entität **Zahl** hinzuzufügen. Nach der Erstellung der Entität wird `2` in der Äußerung blau angezeigt und damit als bezeichnete Entität ausgewiesen. Vordefinierte Entitäten werden von LUIS beschriftet. Eine vordefinierte Entitätsbeschriftung lässt sich aus einer einzelnen Äußerung weder hinzufügen noch entfernen. Sie können nur alle vordefinierten Beschriftungen zusammen hinzufügen oder entfernen, indem Sie die vordefinierte Entität der Anwendung hinzufügen bzw. sie von dort entfernen.

Führen Sie die folgenden [Schritte](#add-hierarchical-entity-and-label) durch, um eine hierarchische Entität für den **Ort** zu erstellen. Beschriften Sie den Ausgangs- und Zielort in der Beispieläußerung. 

Bevor Sie die Entitäten in einer zusammengesetzten Entität umschließen, stellen Sie sicher, dass alle untergeordneten Elemente in blau hervorgehoben sind. Dies bedeutet, dass sie in der Äußerung beschriftet wurden.

1. Umschließen Sie die einzelnen Entitäten in einem Verbund, und wählen Sie die erste beschriftete Entität in der Äußerung für die zusammengesetzte Entität aus. In der Beispieläußerung `book 2 tickets from Seattle to Cairo` ist die erste Entität die Zahl 2. In einer Dropdownliste werden die Optionen für diese Auswahl angezeigt.

    ![Screenshot der ausgewählten Zahl und der hervorgehobenen Dropdownoptionen](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Wählen Sie **Wrap composite entity** (Zusammengesetzte Entität umschließen) aus der Dropdownliste aus. 

    ![Screenshot der Dropdownoptionen für das Umschließen von zusammengesetzten Entitäten mit hervorgehobener Option „Wrap composite entity“ (Zusammengesetzte Entität umschließen)](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Wählen Sie das letzte Wort der zusammengesetzten Entität aus. Wählen Sie in der Äußerung dieses Beispiels „Location::Destination“ (Ort::Ziel), in diesem Fall Kairo, aus. Alle Wörter sind jetzt mit einer grünen Linie unterstrichen, einschließlich der zusammengesetzten Nicht-Entitätswörter in der Äußerung, die den Verbund darstellen.

    ![Screenshot der Seite mit der Absicht „BookFlight“ (Flug buchen) mit hervorgehobener Zahl](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Wählen Sie den zusammengesetzten Entitätsnamen aus der Dropdownliste aus. In diesem Beispiel ist es **TicketOrder** (Flugticketbestellung).

    ![Screenshot der umschließenden Wörter mit zusammengesetzter Entität, wobei der Name der zusammengesetzten Entität in der Dropdownliste hervorgehoben ist](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Wenn Sie die Entitäten ordnungsgemäß umschließen, wird eine grüne Linie unter dem gesamten Ausdruck angezeigt.

    ![Screenshot der Äußerung mit hervorgehobener zusammengesetzter Entität](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Weitere Informationen dazu, wie Sie die zusammengesetzte Entität aus der Antwort der JSON-Endpunktabfrage extrahieren, finden Sie unter [Datenextrahierung](luis-concept-data-extraction.md#composite-entity-data). Probieren Sie das [Tutorial](luis-tutorial-composite-entity.md) für zusammengesetzte Entitäten aus, um mehr über das Verwenden einer zusammengesetzten Entität zu erfahren.

## <a name="add-hierarchical-entity-and-label"></a>Hinzufügen einer Entität und Beschriftung vom Typ „Hierarchisch“
Eine hierarchische Entität ist eine Kategorie von im Kontext erlernten und konzeptionell verwandten Entitäten. Die Entität im folgenden Beispiel enthält Abflug- und Zielorte. 

In der Äußerung `Book 2 tickets from Seattle to Cairo` ist Seattle der Abflug- und Kairo der Zielort. Die Orte unterscheiden sich durch den Kontext und werden aufgrund der Wortstellung und Wortwahl in der Äußerung erlernt.

1. Wählen Sie auf Seite der Absicht in der Äußerung den Ort „Seattle“ aus, geben Sie anschließend den Namen der Entität als „Location“ (Ort) an, und wählen Sie dann **Neue Entität erstellen** aus.

    ![Screenshot des Dialogfelds zum Erstellen der Beschriftung einer hierarchischen Entität](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Wählen Sie im Popupdialogfeld den hierarchischen **Entitätstyp**, fügen Sie anschließend `Origin` und `Destination` als untergeordnete Elemente hinzu, und wählen Sie dann **Fertig** aus.

    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit der hervorgehobenen Option „ToLocation“ (zum Zielort)](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Das Wort in der Äußerung wurde mit der hierarchischen übergeordneten Entität beschriftet. Sie müssen das Wort einer untergeordneten Entität zuweisen. Kehren Sie zur Äußerung auf der Seite der Absicht zurück. Wählen Sie das Wort aus. Wählen Sie anschließend aus der Dropdownliste den von Ihnen erstellten Entitätsnamen aus, und folgen Sie dann der Menüstruktur nach rechts, um die richtige untergeordnete Entität auszuwählen.

    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit der hervorgehobenen Option „ToLocation“ (zum Zielort)](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Die Namen untergeordneter Entitäten müssen für alle Entitäten einer einzelnen App eindeutig sein. Zwei unterschiedliche hierarchische Entitäten dürfen keine untergeordneten Elemente mit dem gleichen Namen enthalten. 

    Weitere Informationen dazu, wie Sie die hierarchische Entität aus der Antwort der JSON-Endpunktabfrage extrahieren, finden Sie unter [Datenextrahierung](luis-concept-data-extraction.md#hierarchical-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-hier-entity.md) für hierarchische Entitäten aus, um mehr über das Verwenden einer hierarchischen Entität zu erfahren.

## <a name="entity-status-predictions"></a>Vorhersagen zum Entitätsstatus

Wenn Sie im LUIS-Portal eine neue Äußerung eingeben, enthält diese Äußerung möglicherweise Vorhersagefehler für die Entität. Der Vorhersagefehler ist der Unterschied zwischen der Bezeichnung einer Entität und der Vorhersage zur Entität durch LUIS. 

Dieser Unterschied wird im LUIS-Portal in der Äußerung rot unterstrichen. Die rote Unterstreichung kann in den Klammern der Entität oder außerhalb der Klammern angezeigt werden. 

![Screenshot der Vorhersageabweichung für den Entitätsstatus](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Wählen Sie die rot unterstrichenen Wörter in der Äußerung aus. 

Im Entitätsfeld wird der **Entitätsstatus** mit einem roten Ausrufezeichen angezeigt, wenn eine Abweichung zur Vorhersage vorliegt. Wählen Sie zum Anzeigen des Entitätsstatus mit Informationen zu den Unterschieden zwischen den bezeichneten und vorhergesagten Entitäten die Option **Entitätsstatus** und dann das Element auf der rechten Seite aus.

![Screenshot der Vorhersageabweichung für den Entitätsstatus](./media/luis-how-to-add-example-utterances/entity-status.png)

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


## <a name="remove-entity-labels-from-utterances"></a>Entfernen von Entitätsbeschriftungen aus Äußerungen
Sie können durch maschinelles Lernen erworbene Entitätsbeschriftungen aus einer Äußerung auf der Seite der Absicht entfernen. Wenn die Entität nicht durch maschinelles Lernen erworben wurde, kann sie nicht aus der Äußerung entfernt werden. Wenn Sie eine nicht durch maschinelles Lernen erworbene Entität aus der Äußerung entfernen möchten, müssen Sie die Entität aus der gesamten App löschen. 

Um eine durch maschinelles Lernen erworbene Entität aus einer Äußerung zu entfernen, wählen Sie die Entität in der Äußerung aus. Wählen Sie anschließend im angezeigten Dropdownfeld der Entität die Option **Beschriftung entfernen** aus.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Option „Beschriftung entfernen“](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Hinzufügen von vordefinierten Entitätsbeschriftungen
Wenn Sie die vordefinierten Entitäten zu Ihrer LUIS-App hinzufügen, müssen Sie die Äußerungen nicht mit diesen Entitäten beschriften. Weitere Informationen zu vordefinierten Entitäten und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Hinzufügen einer Entitätsbeschriftung vom Typ „Regulärer Ausdruck“
Wenn Sie die Entitäten vom Typ „Regulärer Ausdruck“ zu Ihrer LUIS-App hinzufügen, müssen Sie die Äußerungen nicht mit diesen Entitäten beschriften. Weitere Informationen zu Entitäten vom Typ „Regulärer Ausdruck“ und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Erstellen eines Musters aus einer Äußerung
Weitere Informationen finden Sie unter [Hinzufügen eines Musters aus einer vorhandenen Äußerung auf der Seite der Absicht oder der Entität](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Hinzufügen der Entitätsbeschriftung „Pattern.any“
Wenn Sie die „Pattern.any“-Entitäten zu Ihrer LUIS-App hinzufügen, können Sie die Äußerungen nicht mit diesen Entitäten beschriften. Sie sind nur in Mustern gültig. Weitere Informationen zu „Pattern.any“-Entitäten und wie Sie sie hinzufügen finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Trainieren Ihrer App nach dem Ändern des Modells mit Äußerungen
Nachdem Sie Äußerungen hinzugefügt, bearbeitet oder entfernt haben, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen Auswirkungen auf Endpunktabfragen zeigen. 

## <a name="next-steps"></a>Nächste Schritte

Nach dem Beschriften von Äußerungen in Ihren Absichten können Sie jetzt eine [zusammengesetzte Entität](luis-how-to-add-entities.md) erstellen.

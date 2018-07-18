---
title: Tutorial zur Verwendung von Mustern zur Verbesserung der LUIS Vorhersagen – Azure | Microsoft-Dokumentation
titleSuffix: Azure
description: In diesem Tutorial verwenden Sie Muster für Absichten, um die Vorhersagen von LUIS zu Absichten und Entitäten zu verbessern.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: ff5572366be548132b28e5ce03b9595e7f98128c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265315"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Tutorial: Verwenden von Mustern zur Verbesserung von Vorhersagen

In diesem Tutorial verwenden Sie Muster, um die Vorhersagen von LUIS für Absichten und Entitäten zu verbessern.  

> [!div class="checklist"]
* Ermitteln, ob ein Muster für eine App hilfreich wäre
* Erstellen eines Musters 
* Verwenden vordefinierter und benutzerdefinierter Entitäten in einem Muster 
* Überprüfen der Verbesserungen bei Vorhersagen durch Muster
* Hinzufügen einer Rolle zu einer Entität, um kontextbezogene Entitäten zu finden
* Hinzufügen einer Pattern.any-Entität, um Freiformentitäten zu finden

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="import-humanresources-app"></a>Importieren der HumanResources-App
In diesem Tutorial wird die HumanResources-App importiert. Die App enthält drei Absichten: None, GetEmployeeOrgChart, GetEmployeeBenefits. Die App enthält zwei Entitäten: number (vordefiniert) und Employee. Die Employee-Entität ist eine einfache Entität zum Extrahieren des Namens eines Mitarbeiters. 

1. Erstellen Sie eine neue LUIS-App-Datei, und benennen Sie sie mit `HumanResources.json`. 

2. Kopieren Sie die folgende App-Definition in die Datei:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. Wählen Sie auf der Seite der LUIS-**Apps** die Option **Import new app** (Neue App importieren) aus. 

4. Wählen Sie im Dialogfeld **Import new App** (Neue App importieren) die Datei `HumanResources.json` aus, die Sie in Schritt 1 erstellt haben.

5. Wählen Sie die Absicht **GetEmployeeOrgChart** aus, und wechseln Sie dann von der **Entitätenansicht** zur **Tokenansicht**. Es werden mehrere Beispieläußerungen aufgeführt. Jede Äußerung enthält einen Namen, der eine Employee-Entität ist. Beachten Sie, dass jeder Name anders ist und dass die Anordnung der Wörter bei jeder Äußerung anders ist. Diese Vielfalt hilft LUIS dabei, eine Vielzahl von Äußerungen zu erlernen.

    ![Screenshot der Seite „Absicht“ mit der Entitätenansicht](media/luis-tutorial-pattern/utterances-token-view.png)

6. Wählen Sie auf der oberen Navigationsleiste **Train** (Trainieren) aus, um die App zu trainieren. Warten Sie, bis die grüne Bestätigungsleiste angezeigt wird.

7. Wählen Sie im oberen Bereich **Test** aus. Geben Sie `Who does Patti Owens report to?` ein, und drücken Sie dann die EINGABETASTE. Wählen Sie unter der Äußerung **Überprüfen** aus, um weitere Informationen zum Test anzuzeigen.
    
    Der Mitarbeitername „Patti Owens“ wurde bisher in keiner Beispieläußerung verwendet. Dies ist ein Test, um festzustellen, wie gut LUIS gelernt hat, dass sich diese Äußerung auf die Absicht `GetEmployeeOrgChart` bezieht und dass die Employee-Entität `Patti Owens` sein sollte. Das Ergebnis sollte kleiner als 50 % (0,50) für die Absicht `GetEmployeeOrgChart` sein. Obwohl die Absicht richtig ist, ist die Bewertung niedrig. Die Employee-Entität wurde auch richtig als `Patti Owens` identifiziert. Muster erhöhen dieses anfängliche Vorhersageergebnis. 

    ![Screenshot des Testbereichs](media/luis-tutorial-pattern/original-test.png)

8. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Über Muster erlernt LUIS allgemeine Äußerungen mit weniger Beispielen
Im Themenbereich Personalverwaltung gibt es einige wenige häufig verwendete Methoden, um Mitarbeiterbeziehungen in Organisationen zu erfragen. Beispiel: 

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Diese Äußerungen sind zu ähnlich, um jeweils ihre kontextbezogene Eindeutigkeit zu ermitteln, ohne zahlreiche Beispieläußerungen anzugeben. Durch Hinzufügen eines Musters für eine Absicht lernt LUIS häufige Muster von Äußerungen für eine Absicht, ohne dass dafür viele Beispieläußerungen angegeben werden müssen. 

Beispiele für Vorlagenäußerungen zu dieser Absicht sind:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Das Muster ist eine Kombination aus der Suche nach Übereinstimmungen mit regulären Ausdrücken und maschinellem Lernen. Stellen Sie als Nächstes einige Beispiele für Vorlagenäußerungen bereit, damit LUIS das Muster erlernen kann. Diese Beispiele vermitteln LUIS zusammen mit den Äußerungen zur Absicht selbst ein besseres Verständnis dafür, welche Äußerungen zur Absicht passen und wo sich die Entität innerhalb der Äußerung befindet. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Hinzufügen der Vorlagenäußerungen

1. Wählen Sie im linken Navigationsbereich unter **Improve app performance** (App-Leistung verbessern) die Option **Muster** aus.

2. Wählen Sie die Absicht **GetEmployeeOrgChart** aus, geben Sie dann die folgenden Vorlagenäußerungen jeweils einzeln ein, und drücken Sie nach jeder Vorlagenäußerung die EINGABETASTE:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    Die Syntax `{Employee}` markiert die Position der Entität innerhalb der Vorlagenäußerung sowie die Entität selbst. 

    ![Screenshot der Eingabe von Vorlagenäußerungen für eine Absicht](./media/luis-tutorial-pattern/enter-pattern.png)

3. Wählen Sie auf der oberen Navigationsleiste **Train** (Trainieren) aus. Warten Sie, bis die grüne Bestätigungsleiste angezeigt wird.

4. Wählen Sie im oberen Bereich **Test** aus. Geben Sie `Who does Patti Owens report to?` in das Textfeld ein. Drücken Sie die EINGABETASTE. Dies ist die gleiche Äußerung, die im vorherigen Abschnitt getestet wurde. Das Ergebnis sollte für die Absicht `GetEmployeeOrgChart` höher sein. 

    Die Bewertung ist nun viel besser. LUIS hat das für die Absicht relevante Muster gelernt, ohne dass Sie viele Beispiele angeben mussten.

    ![Screenshot des Testbereichs mit hohem Bewertungsergebnis](./media/luis-tutorial-pattern/high-score.png)

    Zuerst wird die Entität gefunden und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entität nicht erkannt und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen. 

5. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Verwenden einer Entität mit einer Rolle in einem Muster
Die LUIS-App soll dabei helfen, Mitarbeiter von einem Standort zu einem anderen zu versetzen. Eine Beispieläußerung ist `Move Bob Jones from Seattle to Los Colinas`. Jeder Standort in der Äußerung hat eine andere Bedeutung. Seattle ist der ursprüngliche Standort, und Los Colinas der Zielstandort für die Versetzung. Um diese Standorte im Muster zu unterscheiden, erstellen Sie in den folgenden Abschnitten eine einfache Entität für den Standort mit zwei Rollen: „Origin“ und „Destination“. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Erstellen einer neuen Absicht zum Versetzen von Personen und Ressourcen
Erstellen Sie eine neue Absicht für alle Äußerungen zum Versetzen von Personen oder Verlegen von Ressourcen.

1. Wählen Sie im linken Navigationsbereich **Absichten** aus.
2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus.
3. Benennen Sie die neue Absicht mit `MoveAssetsOrPeople`.
4. Fügen Sie Beispieläußerungen hinzu:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Screenshot der Beispieläußerung für die Absicht „MoveAssetsOrPeople“](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    Zweck der Beispieläußerungen ist die Bereitstellung ausreichend vieler Beispiele. Wenn später im Test die Entität „Location“ und damit das Muster nicht erkannt wird, kehren Sie zu diesem Schritt zurück, und fügen Sie weitere Beispiele hinzu. Wiederholen Sie dann das Training und die Tests. 

5. Markieren Sie die Employee-Entitäten in den Beispieläußerungen, indem Sie den Vor- und den Nachnamen in einer Äußerung auswählen und dann in der Liste die Employee-Entität auswählen.

    ![Screenshot von Äußerungen in „MoveAssetsOrPeople“ mit markierter Employee-Entität](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Wählen Sie den Text `portland` in der Äußerung `move travis hinton from portland to orlando` aus. Geben Sie im Popupdialogfeld den neuen Entitätsnamen `Location` ein, und wählen Sie **Create new entity** (Neue Entität erstellen) aus. Wählen Sie den Entitätstyp **Simple** und dann **Fertig** aus.

    ![Screenshot der Erstellung der neuen Location-Entität](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Markieren Sie die restlichen Standortnamen in den Äußerungen. 

    ![Screenshot aller markierten Entitäten](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    Das Muster aus Wortwahl und -anordnung in der vorherigen Abbildung ist offensichtlich. Wenn Sie noch **kein** Muster verwenden und die Äußerungen der Absicht ein so offensichtliches Muster aufweisen, sollten Sie dringend damit beginnen, Muster zu verwenden. 

    Wenn Sie anstelle eines Musters von sehr unterschiedlichen Äußerungen ausgehen, wären dies die falschen Beispieläußerungen. In diesem Fall würden Sie in Bezug auf Begriffs- oder Wortwahl, Länge und Position der Entitäten sehr unterschiedliche Äußerungen verwenden. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Hinzufügen von Rollen zur Location-Entität 
Rollen können nur für Muster verwendet werden. Fügen Sie der Location-Entität die Rollen „Origin“ und „Destination“ hinzu. 

1. Wählen Sie im linken Navigationsbereich **Entitäten** und dann in der Liste der Entitäten **Location** aus.

2. Fügen Sie der Entität die Rollen `Origin` und `Destination` hinzu.

    ![Screenshot der neuen Entität mit Rollen](./media/luis-tutorial-pattern/location-entity.png)

    Die Rollen werden auf der Seite der Absicht „MoveAssetsOrPeople“ nicht markiert, da es bei Äußerungen für Absichten keine Rollen gibt. Sie stehen nur für Vorlagenäußerungen in Mustern zur Verfügung. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Hinzufügen von Vorlagenäußerungen, in denen die Location-Rollen „Origin“ und „Destination“ verwendet werden
Fügen Sie Vorlagenäußerungen hinzu, in denen die neue Entität verwendet wird.

1. Wählen Sie im linken Navigationsbereich **Muster** aus.

2. Wählen Sie die Absicht **MoveAssetsOrPeople** aus.

3. Geben Sie eine neue Vorlagenäußerung unter Verwendung der neuen Entität `Move {Employee} from {Location:Origin} to {Location:Destination}` ein. Die Syntax für eine Entität und eine Rolle in einer Vorlagenäußerung ist `{entity:role}`.

    ![Screenshot der neuen Entität mit Rollen](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Trainieren Sie die App für die neue Absicht, die Entität und das Muster.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Testen des neuen Musters für das Extrahieren von Daten der Rolle
Überprüfen Sie das neue Muster mit einem Test.

1. Wählen Sie im oberen Bereich **Test** aus. 
2. Geben Sie die Äußerung `Move Tammi Carlson from Bellingham to Winthrop` ein.
3. Wählen Sie unter dem Ergebnis **Überprüfen** aus, um die Testergebnisse für die Entität und die Absicht anzuzeigen.

    ![Screenshot der neuen Entität mit Rollen](./media/luis-tutorial-pattern/test-with-roles.png)

    Zuerst werden die Entitäten gefunden und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entitäten nicht erkannt wurden und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen. 

4. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Verwenden einer Pattern.any-Entität zum Ermitteln von Freiformentitäten in einem Muster
Diese HumanResources-App hilft Mitarbeitern auch dabei, Unternehmensformulare zu finden. Viele der Formulare haben Titel mit unterschiedlicher Länge. Die unterschiedlichen Längen enthalten Ausdrücke, die es für LUIS schwierig machen, das Ende von Formularnamen zu erkennen. Mit einer **Pattern.any**-Entität in einem Muster können Sie den Anfang und das Ende des Formularnamens angeben, damit LUIS diesen ordnungsgemäß extrahiert. 

### <a name="create-a-new-intent-for-the-form"></a>Erstellen einer neuen Absicht für das Formular
Erstellen Sie eine neue Absicht für Äußerungen, mit denen nach Formularen gesucht wird.

1. Wählen Sie im linken Navigationsbereich **Absichten** aus.

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus.

3. Benennen Sie die neue Absicht mit `FindForm`.

4. Fügen Sie eine Beispieläußerung hinzu.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Screenshot der neuen Entität mit Rollen](./media/luis-tutorial-pattern/intent-findform.png)

    Der Titel des Formulars lautet `What to do when a fire breaks out in the Lab`. In der Äußerung wird nach dem Speicherort des Formulars gefragt und danach, wer es unterzeichnen muss, um das Lesen durch den Mitarbeiter zu verifizieren. Ohne eine Pattern.any-Entität wäre es schwierig, zu verstehen, wo der Titel des Formulars endet, und den Titel des Formulars als eine Entität der Äußerung zu extrahieren.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Erstellen einer Pattern.any-Entität für den Formulartitel
Die Pattern.any-Entität erlaubt Entitäten unterschiedlicher Länge. Sie funktioniert nur in einem Muster, da das Muster Anfang und Ende der Entität kennzeichnet. Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](luis-concept-patterns.md#explicit-lists), um dieses Problem zu beheben. 

1. Wählen Sie im linken Navigationsbereich **Entitäten** aus.

2. Wählen Sie **Neue Entität erstellen** aus. 

3. Benennen Sie die Entität vom Typ **Pattern.any** mit `FormName`. Für dieses Tutorial müssen Sie der Entität keine Rollen hinzufügen.

    ![Abbildung des Dialogfelds für Entitätsname und Entitätstyp](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Hinzufügen eines Muster, das Pattern.any verwendet

1. Wählen Sie im linken Navigationsbereich **Muster** aus.

2. Wählen Sie die Absicht **FindForm** aus.

3. Geben Sie eine Vorlagenäußerung unter Verwendung der neuen Entität `Where is the form {FormName} and who needs to sign it after I read it?` ein.

    ![Screenshot einer Vorlagenäußerung mit Pattern.any-Entität](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Trainieren Sie die App für die neue Absicht, die Entität und das Muster.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testen des neuen Musters für das Extrahieren von Freiformdaten
1. Wählen Sie auf der oberen Leiste **Test** aus, um den Testbereich zu öffnen. 

2. Geben Sie die Äußerung `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?` ein.

3. Wählen Sie unter dem Ergebnis **Überprüfen** aus, um die Testergebnisse für die Entität und die Absicht anzuzeigen.

    ![Screenshot einer Vorlagenäußerung mit Pattern.any-Entität](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    Zuerst wird die Entität gefunden und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entitäten nicht erkannt wurden und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen.

4. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Klicken Sie dazu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer Ausdrucksliste zum Verbessern der Vorhersagen](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
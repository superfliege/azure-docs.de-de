---
title: Verwenden von Batchtests zum Verbessern der LUIS-Vorhersagen | Microsoft-Dokumentation
titleSuffix: Azure
description: Laden Sie Batchtests, überprüfen Sie die Ergebnisse, und verbessern Sie die Vorhersagen von LUIS durch Änderungen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 5788f17f2724a0354a1db506971c2343c1800f01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266395"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Verwenden von Batchtests zum Ermitteln von Problemen mit der Vorhersagegenauigkeit

Dieses Tutorial veranschaulicht die Verwendung von Batchtests zum Ermitteln von Problemen bei der Vorhersage für Äußerungen.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
* Erstellen einer Batchtestdatei 
* Ausführen eines Batchtests
* Überprüfen der Testergebnisse
* Beheben von Fehlern bei Absichten
* Erneutes Testen des Batchs

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

> [!Tip]
> Wenn Sie noch kein Abonnement besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.

## <a name="create-new-app"></a>Erstellen einer neuen App
In diesem Artikel wird die vordefinierte Domäne HomeAutomation verwendet. Diese vordefinierte Domäne enthält Absichten, Entitäten und Äußerungen zum Steuern von HomeAutomation-Geräten wie Lampen. Erstellen Sie die App, fügen Sie die Domäne hinzu, und trainieren und veröffentlichen Sie sie.

1. Erstellen Sie auf der [LUIS]-Website eine neue App, indem Sie **Neue App erstellen** auf der Seite **MyApps** auswählen. 

    ![Erstellen einer neuen App](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Geben Sie den Namen `Batchtest-HomeAutomation` im Dialogfeld ein.

    ![Eingeben des App-Namens](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Wählen Sie in der linken unteren Ecke **Prebuilt Domains** (Vordefinierte Domänen) aus. 

    ![Auswählen einer vordefinierten Domäne](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Wählen Sie **Domäne hinzufügen** für HomeAutomation aus.

    ![Hinzufügen der Domäne HomeAutomation](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Wählen Sie auf der rechten oberen Navigationsleiste **Train** (Trainieren) aus.

    ![Auswählen der Schaltfläche „Train“ (Trainieren)](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Kriterien für Batchtests
Sie können beim Testen in Batches bis zu 1.000 Äußerungen auf einmal testen. Der Batch sollte keine Duplikate enthalten. [Exportieren](create-new-app.md#export-app) Sie die App, um die Liste der aktuellen Äußerungen anzuzeigen.  

Die Teststrategie für LUIS nutzt drei separate Datensätze: Modelläußerungen, Äußerungen für den Batchtest und Endpunktäußerungen. Stellen Sie in diesem Tutorial sicher, dass Sie weder Modelläußerungen (einer Absicht hinzugefügt) noch Endpunktäußerungen verwenden. 

Verwenden Sie bei Batchtests keine der Äußerungen, die bereits in der App enthalten sind:

```
'breezeway on please',
'change temperature to seventy two degrees',
'coffee bar on please',
'decrease temperature for me please',
'dim kitchen lights to 25 .',
'fish pond off please',
'fish pond on please',
'illuminate please',
'living room lamp on please',
'living room lamps off please',
'lock the doors for me please',
'lower your volume',
'make camera 1 off please',
'make some coffee',
'play dvd',
'set lights bright',
'set lights concentrate',
'set lights out bedroom',
'shut down my work computer',
'silence the phone',
'snap switch fan fifty percent',
'start master bedroom light .',
'theater on please',
'turn dimmer off',
'turn off ac please',
'turn off foyer lights',
'turn off living room light',
'turn off staircase',
'turn off venice lamp',
'turn on bathroom heater',
'turn on external speaker',
'turn on my bedroom lights .',
'turn on the furnace room lights',
'turn on the internet in my bedroom please',
'turn on thermostat please',
'turn the fan to high',
'turn thermostat on 70 .' 
```

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Erstellen eines Batchs zum Testen der Vorhersagegenauigkeit von Absichten
1. Erstellen Sie `homeauto-batch-1.json` in einem Text-Editor wie [VS Code](https://code.visualstudio.com/). 

2. Fügen Sie Äußerungen mit der **Absicht**, für die Sie im Test eine Vorhersage erhalten möchten, hinzu. Zur Vereinfachung verwenden Sie in diesem Tutorial Äußerungen in `HomeAutomation.TurnOn` und `HomeAutomation.TurnOff` und vertauschen `on` und `off` in den Äußerungen. Fügen Sie für die Absicht `None` eine Reihe von Äußerungen hinzu, die nicht Teil der [Domäne](luis-glossary.md#domain) (des Themenbereichs) sind. 

    Um zu verstehen, in welchem Verhältnis die Ergebnisse von Batchtests zum JSON-Code im Batch stehen, fügen Sie nur sechs Absichten hinzu.

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Ausführen des Batchs
1. Wählen Sie auf der oberen Navigationsleiste **Test** aus. 

    ![Auswählen von „Test“ auf der Navigationsleiste](./media/luis-tutorial-batch-testing/test-1.png)

2. Wählen Sie im rechten Bereich **Batch testing panel** (Batchtestbereich) aus. 

    ![Auswählen des Batchtestbereichs](./media/luis-tutorial-batch-testing/test-2.png)

3. Wählen Sie **Dataset importieren** aus.

    ![Auswählen von „Dataset importieren“](./media/luis-tutorial-batch-testing/test-3.png)

4. Wählen Sie den Speicherort im Dateisystem für die Datei `homeauto-batch-1.json` aus.

5. Benennen Sie das Dataset mit `set 1`.

    ![Datei auswählen](./media/luis-tutorial-batch-testing/test-4.png)

6. Wählen Sie die Schaltfläche **Ausführen**. Warten Sie, bis der Test abgeschlossen ist.

    ![Auswählen von „Ausführen“](./media/luis-tutorial-batch-testing/test-5.png)

7. Wählen Sie **See results** (Ergebnisse anzeigen) aus.

    ![Anzeigen der Ergebnisse](./media/luis-tutorial-batch-testing/test-6.png)

8. Überprüfen Sie die Ergebnisse im Diagramm und in der Legende.

    ![Batchergebnisse](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Überprüfen der Batchergebnisse
Die Batchergebnisse werden in zwei Abschnitten dargestellt. Der obere Abschnitt enthält das Diagramm und die Legende. Im unteren Abschnitt werden Äußerungen angezeigt, wenn Sie einen Bereichsnamen des Diagramms auswählen.

Fehler werden rot angezeigt. Das Diagramm ist in vier Abschnitte unterteilt, von denen zwei in Rot angezeigt werden. **Auf diese Abschnitte sollten Sie besonders achten**. 

Im Abschnitt rechts oben hat der Test falsch vorhergesagt, dass eine Absicht oder Entität vorhanden ist. Im Abschnitt links unten hat der Test falsch vorhergesagt, dass eine Absicht oder Entität fehlt.

### <a name="homeautomationturnoff-test-results"></a>Testergebnisse von HomeAutomation.TurnOff
Wählen Sie in der Legende die Absicht `HomeAutomation.TurnOff` aus. Links neben ihrem Namen steht in der Legende ein grünes Erfolgssymbol. Für diese Absicht liegen keine Fehler vor. 

![Batchergebnisse](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>Absichten „HomeAutomation.TurnOn“ und „None“ mit Fehlern
Die anderen beiden Absichten weisen Fehler auf. Dies bedeutet, dass die Testvorhersagen nicht den Erwartungen der Batchdatei entsprechen. Wählen Sie die Absicht `None` in der Legende aus, um den ersten Fehler zu überprüfen. 

![Absicht „None“](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Fehler werden im Diagramm in den roten Abschnitten angezeigt: **Falsch positives Ergebnis** und **Falsch negatives Ergebnis**. Wählen Sie den Abschnittsnamen **Falsch negatives Ergebnis** im Diagramm aus, um die fehlerhaften Äußerungen unterhalb des Diagramms anzuzeigen. 

![Falsch negative Ergebnisse](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

Die fehlerhafte Äußerung `help` wurde als Absicht `None` erwartet, aber der Test hat die Absicht `HomeAutomation.TurnOn` vorhergesagt.  

Es gibt zwei Fehler, einen in „HomeAutomation.TurnOn“ und einen in „None“. Beide wurden durch die Äußerung `help` verursacht, da diese nicht die Erwartung in „None“ erfüllt hat, sondern stattdessen eine unerwartete Übereinstimmung mit der Absicht „HomeAutomation.TurnOn“ auftrat. 

Um zu bestimmen, warum die Äußerungen in `None` zu Fehlern geführt haben, überprüfen Sie die Äußerungen, die sich derzeit in `None` befinden. 

## <a name="review-none-intents-utterances"></a>Überprüfen der Äußerungen der Absicht „None“

1. Schließen Sie den Bereich **Test** durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste. 

2. Wählen Sie im oberen Navigationsbereich **Build** aus. 

3. Wählen Sie in der Liste der Absichten die Absicht **None** aus.

4. Drücken von STRG+E zum Anzeigen der Tokenansicht der Äußerungen 
    
    |Äußerungen der Absicht „None“|Vorhersageergebnis|
    |--|--|
    |"decrease temperature for me please"|0.44|
    |"dim kitchen lights to 25."|0.43|
    |"lower your volume"|0,46|
    |"turn on the internet in my bedroom please"|0.28|

## <a name="fix-none-intents-utterances"></a>Beheben von Problemen mit Äußerungen der Absicht „None“
    
Alle Äußerungen in `None` sollten außerhalb der App-Domäne liegen. Diese Äußerungen haben Bezug zu HomeAutomation, sodass sie der falschen Absicht angehören. 

LUIS bewertet die Äußerungen ebenfalls mit einem Vorhersageergebnis von weniger als 50 % (<.50). Bei den Äußerungen in den anderen beiden Absichten finden Sie deutlich höhere Vorhersagebewertungen. Wenn LUIS niedrige Bewertungen für Beispieläußerungen zurückgibt, ist dies ein deutlicher Hinweis, dass LUIS die Äußerungen nicht sicher der aktuellen Absicht oder anderen Absichten zuordnen kann. 

Zum Korrigieren der App müssen die Äußerungen, die sich derzeit in der Absicht `None` befinden, der richtigen Absicht zugeordnet werden. Gleichzeitig benötigt die Absicht `None` neue, angemessene Absichten. 

Drei der Äußerungen in der Absicht `None` sollen die Einstellungen des Automatisierungsgeräts verringern. Sie verwenden Wörter wie `dim`, `lower` oder `decrease`. Die vierte Äußerung fordert das Aktivieren des Internets an. Da alle vier Äußerungen dem Einschalten oder Ändern der Leistung eines Geräts dienen, sollten sie in die Absicht `HomeAutomation.TurnOn` verschoben werden. 

Dies ist nur eine Lösung. Sie können auch eine neue Absicht `ChangeSetting` erstellen und die Äußerungen mit „dim“, „lower“ und „decrease“ in diese neue Absicht verschieben. 

## <a name="fix-the-app-based-on-batch-results"></a>Korrigieren der App anhand der Batchergebnisse
Verschieben Sie die vier Äußerungen in die Absicht `HomeAutomation.TurnOn`. 

1. Aktivieren Sie das Kontrollkästchen oberhalb der Äußerungsliste, damit alle Äußerungen ausgewählt werden. 

2. Wählen Sie in der Dropdownliste **Reassign intent** (Absicht neu zuweisen) die Option `HomeAutomation.TurnOn` aus. 

    ![Verschieben von Äußerungen](./media/luis-tutorial-batch-testing/move-utterances.png)

    Nachdem die vier Äußerungen neu zugewiesen wurden, ist die Liste der Äußerung für die Absicht `None` leer.

3. Fügen Sie der Absicht „None“ vier neue Absichten hinzu:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Diese Äußerungen liegen definitiv außerhalb der Domäne von HomeAutomation. Beachten Sie, während Sie die einzelnen Äußerungen eingeben, das zugehörige Ergebnis. Die Bewertung kann niedrig oder sogar sehr niedrig sein (mit einem roten Rahmen). Nachdem Sie die App in Schritt 8 trainiert haben, wird das Ergebnis deutlich höher sein. 

7. Entfernen Sie alle Bezeichnungen, indem Sie die blaue Beschriftung in der Äußerung markieren und dann **Beschriftung entfernen** auswählen.

8. Wählen Sie auf der rechten oberen Navigationsleiste **Train** (Trainieren) aus. Die Bewertung der einzelnen Äußerungen ist deutlich höher. Alle Bewertungen für die Absicht `None` sollten nun über 0,80 liegen. 

## <a name="verify-the-fix-worked"></a>Überprüfen der Korrektur
Um sicherzustellen, dass die Äußerungen im Batchtest für die Absicht **None** richtig vorhergesagt werden, führen Sie den Batchtest erneut aus.

1. Wählen Sie auf der oberen Navigationsleiste **Test** aus. 

2. Wählen Sie im rechten Bereich **Batch testing panel** (Batchtestbereich) aus. 

3. Wählen Sie die drei Punkte (...) rechts neben dem Batchnamen und dann **Dataset ausführen** aus. Warten Sie, bis der Batchtest abgeschlossen ist.

    ![Dataset ausführen](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Wählen Sie **See results** (Ergebnisse anzeigen) aus. Links neben dem Namen aller Absichten sollten grüne Symbole angezeigt werden. Legen Sie den Filter auf die Absicht `HomeAutomation.Turnoff` fest, und wählen Sie den grünen Punkt im rechten oberen Bereich in der Mitte des Diagramms aus. Der Name der Äußerung wird in der Tabelle unterhalb des Diagramms angezeigt. Die Bewertung von `breezeway off please` ist sehr niedrig. Optional können Sie der Absicht auch weitere Äußerungen hinzufügen, um ihre Bewertung zu erhöhen. 

    ![Dataset ausführen](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

<!--
    The Entities section of the legend may have errors. That is the next thing to fix.

## Create a batch to test entity detection
1. Create `homeauto-batch-2.json` in a text editor such as [VSCode](https://code.visualstudio.com/). 

2. Utterances have entities identified with `startPos` and `endPost`. These two elements identify the entity before [tokenization](luis-glossary.md#token), which happens in some [cultures](luis-supported-languages.md#tokenization) in LUIS. If you plan to batch test in a tokenized culture, learn how to [extract](luis-concept-data-extraction.md#tokenized-entity-returned) the non-tokenized entities.

    Copy the following JSON into the file:

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 4
            }
          ]
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Operation",
              "startPos": 7,
              "endPos": 17
            }
          ]
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 9
            }
          ]
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Device",
              "startPos": 0,
              "endPos": 10
            }
          ]
        }
      ]
    ```

3. Import the batch file, following the [same instructions](#run-the-batch) as the first import, and name the dataset `set 2`. Run the test.

## Possible entity errors
Since the intents in the right-side filter of the test panel still pass the test, this section focuses on correct entity identification. 

Entity testing is diferrent than intents. An utterance will have only one top scoring intent, but it may have several entities. An utterance's entity may be correctly identified, may be incorrectly identified as an entity other than the one in the batch test, may overlap with other entities, or not identified at all. 

## Review entity errors
1. Select `HomeAutomation.Device` in the filter panel. The chart changes to show a single false positive and several true negatives. 

2. Select the False positive section name. The utterance for this chart point is displayed below the chart. The labeled intent and the predicted intent are the same, which is consistent with the test -- the intent prediction is correct. 

    The issue is that the HomeAutomation.Device was detected but the batch expected HomeAutomation.Room for the utterance "coffee bar off please". `Coffee bar` could be a room or a device, depending on the environment and context. As the model designer, you can either enforce the selection as `HomeAutomation.Room` or change the batch file to use `HomeAutomation.Device`. 

    If you want to reinforce that coffee bar is a room, you nee to add an utterances to LUIS that help LUIS decide a coffee bar is a room. 

    The most direct route is to add the utterance to the intent but that to add the utterance for every entity detection error is not the machine-learned solution. Another fix would be to add an utterance with `coffee bar`.

## Add utterance to help extract entity
1. Select the **Test** button on the top navigation to close the batch test panel.

2. On the `HomeAutomation.TurnOn` intent, add the utterance, `turn coffee bar on please`. The uttterance should have all three entities detected after you select enter. 

3. Select **Train** on the top navigation panel. Wait until training completes successfully.

3. Select **Test** on the top navigation panel to open the Batch testing pane again. 

4. If the list of datasets is not visible, select **Back to list**. Select the three dots (...) at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
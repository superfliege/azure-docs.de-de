---
title: 'Tutorial: Überprüfen von Endpunktäußerungen für Language Understanding (LUIS) – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Endpunktäußerungen im Personalbereich (Human Resources, HR) für LUIS überprüfen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: diberry
ms.openlocfilehash: a04c8987bc2e16a41196286e3260b4b7ec11f3c4
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39357840"
---
# <a name="tutorial-review-endpoint-utterances"></a>Tutorial: Überprüfen von Endpunktäußerungen
In diesem Tutorial erfahren Sie, wie Sie App-Vorhersagen verbessern können, indem Sie die über den LUIS-HTTP-Endpunkt erhaltenen Äußerungen überprüfen bzw. korrigieren. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zur Überprüfung von Endpunktäußerungen 
> * Verwenden der LUIS-App für den Personalbereich 
> * Überprüfen von Endpunktäußerungen
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem [Tutorial zur Standpunktanalyse](luis-quickstart-intent-and-sentiment-analysis.md) verfügen, können Sie sie aus dem GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-sentiment-HumanResources.json) importieren. Wenn Sie dieses Tutorial mit einer neuen, importierten App verwenden, müssen Sie auch das Trainieren und Veröffentlichen durchführen und die Äußerungen dem Endpunkt mit einem [Skript](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) oder in einem Browser vom Endpunkt hinzufügen. Fügen Sie die folgenden Äußerungen hinzu:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `review`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

Falls Sie über alle Versionen der App für die gesamte Tutorialreihe verfügen, sind Sie vielleicht überrascht, dass sich die Liste **Review endpoint utterances** (Endpunktäußerungen überprüfen) nicht je nach Version ändert. Es muss nur ein Pool mit Äußerungen überprüft werden. Dies gilt unabhängig davon, welche Version der Äußerung Sie aktiv bearbeiten oder welche Version der App auf dem Endpunkt veröffentlicht wurde. 

## <a name="purpose-of-reviewing-endpoint-utterances"></a>Zweck der Überprüfung von Endpunktäußerungen
Dieser Überprüfungsprozess ist eine der Möglichkeiten, die für LUIS zum Erlernen Ihrer App-Domäne zur Verfügung stehen. LUIS wählt die Äußerungen in der Überprüfungsliste aus. Für diese Liste gilt Folgendes:

* Sie gilt spezifisch für die App.
* Sie soll die Vorhersagegenauigkeit der App verbessern. 
* Sie sollte in regelmäßigen Abständen überprüft werden. 

Indem Sie die Endpunktäußerungen überprüfen, verifizieren bzw. korrigieren Sie die vorhergesagte Absicht der Äußerung. Außerdem bezeichnen Sie benutzerdefinierte Entitäten, die nicht vorhergesagt wurden. 

## <a name="review-endpoint-utterances"></a>Überprüfen von Endpunktäußerungen

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste **Build** auswählen. 

2. Wählen Sie im linken Navigationsbereich die Option **Review endpoint utterances** (Endpunktäußerungen überprüfen). Die Liste wird für die Absicht **ApplyForJob** gefiltert. 

    [ ![Screenshot: Schaltfläche „Review endpoint utterances“ (Endpunktäußerungen überprüfen) im linken Navigationsbereich](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png)](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png#lightbox)

3. Schalten Sie **Entities view** (Entitätsansicht) um, um die bezeichneten Entitäten anzuzeigen. 
    
    [ ![Screenshot: „Review endpoint utterances“ (Endpunktäußerungen überprüfen) mit hervorgehobenem Umschalter „Entities view“ (Entitätsansicht)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png#lightbox)

    |Äußerung|Richtige Absicht|Fehlende Entitäten|
    |:--|:--|:--|
    |I'm looking for a job with Natural Language Processing (Ich suche nach einem Job im Bereich Verarbeitung natürlicher Sprache)|GetJobInfo|Job – „Natural Language Process“|

    Diese Äußerung weist nicht die richtige Absicht auf und hat eine Bewertung von weniger als 50%. Die Absicht **ApplyForJob** verfügt über 21 Äußerungen, während für **GetJobInformation** nur sieben Äußerungen vorhanden sind. Zusätzlich zum richtigen Zuordnen der Endpunktäußerung sollten der Absicht **GetJobInformation** weitere Äußerungen hinzugefügt werden. Diese Übung können Sie selbstständig durchführen. Alle Absichten, mit Ausnahme von **None** (Keine), sollten ungefähr über die gleiche Anzahl von Beispieläußerungen verfügen. Die Absicht **None** (Keine) sollte etwa 10% der gesamten Äußerungen in der App aufweisen. 

    Wenn Sie sich in **Tokens View** (Tokenansicht) befinden, können Sie den Mauszeiger auf beliebigen blauen Text bewegen, um den vorhergesagten Entitätsnamen anzuzeigen. 

4. Wählen Sie für die Absicht `I'm looking for a job with Natual Language Processing` die richtige Absicht **GetJobInformation** in der Spalte **Aligned intent** (Zugeordnete Absicht) aus. 

    [ ![Screenshot: „Review endpoint utterances“ (Endpunktäußerungen überprüfen) mit Zuordnung der Äußerung zur Absicht](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

5. In derselben Äußerung lautet die Entität für `Natural Language Processing` „keyPhrase“. Dies sollte stattdessen eine **Job**-Entität sein. Wählen Sie `Natural Language Processing` und dann in der Liste die **Job**-Entität aus.

    [ ![Screenshot: „Review endpoint utterances“ (Endpunktäußerungen überprüfen) – Bezeichnen der Entität in der Äußerung](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

6. Wählen Sie in derselben Zeile den Kreis mit dem Häkchen in der Spalte **Add to aligned intent** (Zugeordneter Absicht hinzufügen). 

    [ ![Screenshot: Abschließen der Äußerungszuordnung in der Absicht](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Mit dieser Aktion wird die Äußerung aus **Review endpoint utterances** (Endpunktäußerungen überprüfen) in die Absicht **GetJobInformation** verschoben. Die Endpunktäußerung ist jetzt eine Beispieläußerung für diese Absicht. 

7. Überprüfen Sie die restlichen Äußerungen dieser Absicht, bezeichnen Sie die Äußerungen, und korrigieren Sie die zugeordnete Absicht (**Aligned intent**), falls dies erforderlich ist.

8. Wenn alle Äußerungen korrekt sind, können Sie das Kontrollkästchen in den einzelnen Zeilen aktivieren und dann die Option **Add selected** (Ausgewählte hinzufügen) wählen, um die Äußerungen richtig auszurichten. 

    [ ![Screenshot: Abschließen der restlichen Äußerungen in Bezug auf die zugeordnete Absicht](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

9. Die Liste sollte diese Äußerungen nicht mehr enthalten. Falls weitere Äußerungen angezeigt werden, können Sie die Liste weiter durcharbeiten, Absichten korrigieren und alle fehlenden Entitäten bezeichnen, bis die Liste leer ist. Wählen Sie in der Liste „Filter“ die nächste Absicht aus, und fahren Sie dann mit dem Korrigieren von Äußerungen und dem Bezeichnen von Entitäten fort. Beachten Sie, dass der letzte Schritt für eine Absicht jeweils entweder das Auswählen von **Add to aligned intent** (Zugeordneter Absicht hinzufügen) in der Zeile mit der Äußerung oder das Aktivieren des Kontrollkästchens für jede Absicht und Auswählen von **Add selected** (Ausgewählte auswählen) oberhalb der Tabelle ist. 

    Dies ist eine sehr kleine App. Der Überprüfungsprozess dauert nur wenige Minuten.

## <a name="add-new-job-name-to-phrase-list"></a>Hinzufügen eines neuen Jobnamens zur Ausdrucksliste
Halten Sie die Ausdrucksliste mit allen neu ermittelten Jobnamen auf dem aktuellen Stand. 

1. Wählen Sie im linken Navigationsbereich die Option **Phrase lists** (Ausdruckslisten).

2. Wählen Sie die Ausdrucksliste **Jobs** aus.

3. Fügen Sie `Natural Language Processing` als Wert hinzu, und wählen Sie anschließend **Save** (Speichern). 

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

LUIS ist erst über die Änderungen informiert, nachdem das Trainieren durchgeführt wurde. 

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

Nach dem Importieren dieser App müssen Sie die Option **Sentiment analysis** (Standpunktanalyse) wählen.

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

Probieren Sie es mit einer Äußerung, die nicht weit von der korrigierten Äußerung abweicht. 

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Are there any natural language processing jobs in my department right now?` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

  ```JSON
  {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
  }
  }
  ```

  Die richtige Absicht wurde mit einer hohen Bewertung vorhergesagt, und die **Job**-Entität wurde als `natural language processing` ermittelt. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Kann die Überprüfung durch das Hinzufügen von weiteren Äußerungen ersetzt werden? 
Sie fragen sich vielleicht, warum nicht einfach weitere Beispieläußerungen hinzugefügt werden. Was ist der Zweck der Überprüfung von Endpunktäußerungen? Bei einer LUIS-App in der Praxis stammen die Endpunktäußerungen von Benutzern mit einer Wortwahl und einer Zusammensetzung, die Sie noch nicht verwendet haben. Wenn Sie die gleiche Wortwahl und Zusammensetzung verwendet hätten, würde die ursprüngliche Vorhersage einen höheren Prozentsatz aufweisen. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>Warum ist die oberste Absicht in der Liste mit den Äußerungen enthalten? 
Einige Endpunktäußerungen weisen in der Überprüfungsliste einen hohen Prozentsatz auf. Sie müssen diese Äußerungen trotzdem überprüfen und verifizieren. Sie sind in der Liste enthalten, weil die nächsthöhere Absicht über eine Bewertung verfügt hat, die zu nah an der Bewertung der obersten Absicht gelegen hat. 

## <a name="what-has-this-tutorial-accomplished"></a>Was wurde mit diesem Tutorial erreicht?
Die Vorhersagegenauigkeit der App wurde erhöht, indem die Äußerungen vom Endpunkt überprüft wurden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden von Mustern](luis-tutorial-pattern.md)

---
title: Tutorial zur Verwendung von Mustern zur Verbesserung der LUIS Vorhersagen – Azure | Microsoft-Dokumentation
titleSuffix: Cognitive Services
description: In diesem Tutorial verwenden Sie Muster für Absichten, um die Vorhersagen von LUIS zu Absichten und Entitäten zu verbessern.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: diberry
ms.openlocfilehash: 9ad1d9e1543c3d9a74025fb23bd1767478b53b4b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238453"
---
# <a name="tutorial-improve-app-with-patterns"></a>Tutorial: Verbessern der App mit Mustern

In diesem Tutorial verwenden Sie Muster, um die Vorhersagen von LUIS für Absichten und Entitäten zu verbessern.  

> [!div class="checklist"]
* Ermitteln, ob ein Muster für eine App hilfreich wäre
* Erstellen eines Musters 
* Überprüfen der Verbesserungen bei Vorhersagen durch Muster

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](luis-reference-regions.md)-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie die Personal-App aus dem Tutorial zu [Batchtests](luis-tutorial-batch-testing.md) nicht haben, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App auf der [LUIS-Website](luis-reference-regions.md#luis-website). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `patterns`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Über Muster erlernt LUIS allgemeine Äußerungen mit weniger Beispielen
Im Themenbereich Personalwesen gibt es einige häufig verwendete Methoden, um Mitarbeiterbeziehungen in Organisationen zu erfragen. Beispiel: 

```
Who does Jill Jones report to?
Who reports to Jill Jones? 
```

Diese Äußerungen sind zu ähnlich, um jeweils ihre kontextbezogene Eindeutigkeit zu ermitteln, ohne zahlreiche Beispieläußerungen anzugeben. Durch Hinzufügen eines Musters für eine Absicht lernt LUIS häufige Muster von Äußerungen für eine Absicht, ohne dass dafür viele Beispieläußerungen angegeben werden müssen. 

Beispiele für Vorlagenäußerungen zu dieser Absicht sind:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Das Muster wird als Beispiel für eine Vorlagenäußerung bereitgestellt, die die Syntax zum Identifizieren von Entitäten und ignorierbarem Text enthält. Ein Muster ist eine Kombination aus der Suche nach Übereinstimmungen mit regulären Ausdrücken und maschinellem Lernen.  Das Beispiel für Vorlagenäußerungen und die Äußerungen der Absicht vermitteln LUIS ein besseres Verständnis dafür, welche Äußerungen zur Absicht passen.

Damit ein Muster mit einer Äußerung übereinstimmt, müssen die Entitäten innerhalb der Äußerung zuerst mit den Entitäten in der Vorlagenäußerung übereinstimmen. Allerdings hilft die Vorlage nicht dabei, Entitäten vorherzusagen, sondern nur Absichten. 

**Muster ermöglichen Ihnen, weniger Beispieläußerungen bereitzustellen. Doch wenn die Entitäten nicht erkannt werden, stimmt das Muster nicht überein.**

Das [Tutorial zum Entitätstyp „Liste“](luis-quickstart-intent-and-list-entity.md) zeigt, wie Mitarbeiter erstellt werden.

## <a name="create-new-intents-and-their-utterances"></a>Erstellen neuer Absichten samt Äußerungen
Hinzufügen Sie die beiden neuen Absichten `OrgChart-Manager` und `OrgChart-Reports` hinzu. Sobald LUIS eine Vorhersage an die Clientanwendung zurückgibt, kann der Name der Absicht als Funktionsname in der Clientanwendung und die Entität „Employee“ als Parameter für diese Funktion verwendet werden.

```
OrgChart-Manager(employee){
    ///
}
```

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste **Build** auswählen. 

2. Wählen Sie auf der Seite **Absichten** die Option **Create new intent** (Neue Absicht erstellen). 

3. Geben Sie im Popupdialogfeld die Zeichenfolge `OrgChart-Manager` ein, und klicken Sie anschließend auf **Fertig**.

    ![Popupfenster zum Erstellen einer neuen Absicht](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |Who is John W. Smith the subordinate of?|
    |Who does John W. Smith report to?|
    |Who is John W. Smith's manager?|
    |Who does Jill Jones directly report to?|
    |Who is Jill Jones supervisor?|

    [![](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Screenshot von LUIS – Hinzufügen von neuen Äußerungen zu einer Absicht")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    Möglicherweise wird auch die keyPhrase-Entität in den Äußerungen der Absicht bezeichnet, anstelle der Employee-Entität. Beide werden im Testbereich und am Endpunkt korrekt vorhergesagt. 

5. Klicken Sie im linken Navigationsbereich auf **Absichten**.

6. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

7. Geben Sie im Popupdialogfeld die Zeichenfolge `OrgChart-Reports` ein, und klicken Sie anschließend auf **Fertig**.

8. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |Who are John W. Smith's subordinates?|
    |Who reports to John W. Smith?|
    |Who does John W. Smith manage?|
    |Who are Jill Jones direct reports?|
    |Who does Jill Jones supervise?|

## <a name="caution-about-example-utterance-quantity"></a>Wichtiger Hinweis zur Menge der Beispieläußerungen
Die Menge der Beispieläußerungen in diesen Absichten reicht nicht aus, um LUIS richtig zu trainieren. In einer realen App sollte jede Absicht mindestens 15 Äußerungen mit einer vielfältigen Wortauswahl und unterschiedlicher Äußerungslänge beinhalten. Diese wenigen Äußerungen wurden speziell ausgewählt, um Muster hervorzuheben. 

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
Die neue Absicht und die neuen Äußerungen müssen trainiert werden. 

1. Wählen Sie oben rechts auf der LUIS-Website die Schaltfläche **Train** (Trainieren) aus.

    ![Abbildung der Schaltfläche „Trainieren“](./media/luis-tutorial-pattern/hr-train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website eine grüne Statusleiste angezeigt wird.

    ![Abbildung der Benachrichtigungsleiste nach erfolgreicher Ausführung](./media/luis-tutorial-pattern/hr-trained-inline.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Veröffentlichen**. 

2. Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus.

    [ ![Screenshot der Seite „Publish“ (Veröffentlichen) mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot](./media/luis-tutorial-pattern/hr-publish-to-production.png)](./media/luis-tutorial-pattern/hr-publish-to-production.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    [ ![Screenshot der Seite „Publish“ (Veröffentlichen) mit hervorgehobener Endpunkt-URL](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png)](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png#lightbox)


2. Geben Sie in der Adressleiste am Ende der URL `Who is the boss of Jill Jones?` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

War diese Abfrage erfolgreich? Für diesen Trainingszyklus war sie erfolgreich. Die Bewertungen der beiden oberen Absichten liegen nahe beieinander. Da das Trainieren von LUIS nicht jedes Mal identisch abläuft, gibt es ein wenig Abwechslung. Beim nächsten Trainingszyklus können sich diese beiden Werte umkehren. Das Ergebnis ist, dass die falsche Absicht zurückgegeben werden konnte. 

Verwenden Sie Muster, um die Bewertung der richtigen Absicht deutlich zu erhöhen (in Prozent) und den Abstand zur nächst höheren Bewertung zu vergrößern. 

## <a name="add-the-template-utterances"></a>Hinzufügen der Vorlagenäußerungen

1. Wählen Sie oben im Menü **Build** (Erstellen) aus.

2. Wählen Sie im linken Navigationsbereich unter **Improve app performance** (App-Leistung verbessern) die Option **Muster** aus.

3. Wählen Sie die Absicht **OrgChart-Manager** aus, geben Sie dann die folgenden Vorlagenäußerungen einzeln ein, und drücken Sie nach jeder Vorlagenäußerung die Eingabetaste:

    |Vorlagenäußerungen|
    |:--|
    |Who is {Employee} the subordinate of[?]|
    |Who does {Employee} report to[?]|
    |Who is {Employee}['s] manager[?]|
    |Who does {Employee} directly report to[?]|
    |Who is {Employee}['s] supervisor[?]|
    |Who is the boss of {Employee}[?]|

    Die Syntax `{Employee}` markiert die Position der Entität innerhalb der Vorlagenäußerung sowie die Entität selbst. 
    
    Entitäten mit Rollen verwenden eine Syntax, die den Rollennamen enthält. Sie werden in einem separaten [Tutorial für Rollen](luis-tutorial-pattern-roles.md) behandelt. 

    Die optionale Syntax (`[]`) markiert Wörter oder Satzzeichen, die optional sind. LUIS gleicht die Äußerung ab und ignoriert dabei den optionalen Text in den Klammern.

    Bei der Eingabe der Vorlagenäußerung hilft LUIS Ihnen, die Entität auszufüllen, wenn Sie die linke geschweifte Klammer `{` verwenden.

    [ ![Screenshot der Eingabe von Vorlagenäußerungen für eine Absicht](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)



4. Wählen Sie die Absicht **OrgChart-Reports** aus, geben Sie dann die folgenden Vorlagenäußerungen einzeln ein, und drücken Sie nach jeder Vorlagenäußerung die Eingabetaste:

    |Vorlagenäußerungen|
    |:--|
    |Who are {Employee}['s] subordinates[?]|
    |Who reports to {Employee}[?]|
    |Who does {Employee} manage[?]|
    |Who are {Employee} direct reports[?]|
    |Who does {Employee} supervise[?]|
    |Who does {Employee} boss[?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Abfragen des Endpunkts mit Mustern

1. Trainieren Sie die App und veröffentlichen Sie sie erneut.

2. Wählen Sie unten auf der Seite **Publish** (Veröffentlichen) den Link **endpoint** (Endpunkt) aus. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

3. Geben Sie in der Adressleiste am Ende der URL `Who is the boss of Jill Jones?` als Äußerung ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

Die Vorhersage der Absicht hat nun einen deutlich höheren Wert. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie dazu in der Liste rechts vom App-Namen die Auslassungspunkte (***...***) und dann **Delete** (Löschen) aus. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden von Rollen mit einem Muster](luis-tutorial-pattern-roles.md)
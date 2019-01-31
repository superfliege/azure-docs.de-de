---
title: Muster
titleSuffix: Azure Cognitive Services
description: Verwenden Sie Muster, um die Vorhersage von Absichten und Entitäten zu verbessern und zugleich weniger Beispieläußerungen anzugeben. Das Muster wird als Beispiel für eine Vorlagenäußerung bereitgestellt, die die Syntax zum Identifizieren von Entitäten und ignorierbarem Text enthält.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cgronlun
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: e5aadfc8319ff67e471123d457fddf468af4842a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225770"
---
# <a name="tutorial-add-common-pattern-template-utterance-formats"></a>Tutorial: Hinzufügen von Formaten für allgemeine Mustervorlagen

In diesem Tutorial verwenden Sie Muster, um die Vorhersage von Absichten und Entitäten zu verbessern und zugleich weniger Beispieläußerungen anzugeben. Das Muster wird als Beispiel für eine Vorlagenäußerung bereitgestellt, die die Syntax zum Identifizieren von Entitäten und ignorierbarem Text enthält. Ein Muster ist eine Kombination aus der Suche nach Übereinstimmungen mit Ausdrücken und maschinellem Lernen.  Das Beispiel für Vorlagenäußerungen und die Äußerungen der Absicht vermitteln LUIS ein besseres Verständnis dafür, welche Äußerungen zur Absicht passen. 

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Importieren der Beispiel-App 
> * Erstellen einer Absicht
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt
> * Erstellen eines Musters
> * Überprüfen der Verbesserungen bei Vorhersagen durch Muster
> * Kennzeichnen von Text als ignorierbar und Verschachteln innerhalb von Mustern
> * Verwenden des Testpanels zum Überprüfen des Erfolgs der Muster

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importieren der Beispiel-App

Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Führen Sie die folgenden Schritte aus:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `patterns`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="create-new-intents-and-their-utterances"></a>Erstellen neuer Absichten samt Äußerungen

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

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

    [![Screenshot von LUIS beim Hinzufügen neuer Äußerungen für eine Absicht](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Screenshot von LUIS beim Hinzufügen neuer Äußerungen für eine Absicht")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

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

[!INCLUDE [Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Who is the boss of Jill Jones?` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```json
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

Lassen Sie das zweite Browserfenster geöffnet. Sie verwenden es später im Tutorial. 

## <a name="template-utterances"></a>Vorlagenäußerungen
Im Themenbereich Personalwesen gibt es einige häufig verwendete Methoden, um Mitarbeiterbeziehungen in Organisationen zu erfragen. Beispiel: 

|Äußerungen|
|--|
|Who does Jill Jones report to?|
|Who reports to Jill Jones?|

Diese Äußerungen sind zu ähnlich, um jeweils ihre kontextbezogene Eindeutigkeit zu ermitteln, ohne zahlreiche Beispieläußerungen anzugeben. Durch Hinzufügen eines Musters für eine Absicht lernt LUIS häufige Muster von Äußerungen für eine Absicht, ohne dass dafür viele Beispieläußerungen angegeben werden müssen. 

Zu den Beispielen für Vorlagenäußerungen zu dieser Absicht gehören:

|Beispiele für Vorlagenäußerungen|Bedeutung der Syntax|
|--|--|
|Who does {Employee} report to[?]|austauschbar {Employee}, ignorieren [?]}|
|Who reports to {Employee}[?]|austauschbar {Employee}, ignorieren [?]}|

Die Syntax `{Employee}` markiert die Position der Entität innerhalb der Vorlagenäußerung sowie die Entität selbst. Die optionale Syntax (`[?]`) markiert Wörter oder Satzzeichen, die optional sind. LUIS gleicht die Äußerung ab und ignoriert dabei den optionalen Text in den Klammern.

Zwar sieht die Syntax nach regulären Ausdrücken aus, es handelt sich aber nicht um reguläre Ausdrücke. Es wird nur die Syntax mit geschweiften `{}` und eckigen `[]` Klammern unterstützt. Sie können bis zu zwei Ebenen tief geschachtelt werden.

Damit ein Muster mit einer Äußerung übereinstimmt, müssen die Entitäten innerhalb der Äußerung zuerst mit den Entitäten in der Vorlagenäußerung übereinstimmen. Allerdings hilft die Vorlage nicht dabei, Entitäten vorherzusagen, sondern nur Absichten. 

**Muster ermöglichen Ihnen, weniger Beispieläußerungen bereitzustellen. Doch wenn die Entitäten nicht erkannt werden, stimmt das Muster nicht überein.**

In diesem Tutorial fügen Sie zwei neue Absichten hinzu: `OrgChart-Manager` und `OrgChart-Reports`. 

|Absicht|Äußerung|
|--|--|
|OrgChart-Manager|Who does Jill Jones report to?|
|OrgChart-Reports|Who reports to Jill Jones?|

Sobald LUIS eine Vorhersage an die Clientanwendung zurückgibt, kann der Name der Absicht als Funktionsname in der Clientanwendung und die Entität „Employee“ als Parameter für diese Funktion verwendet werden.

```nodejs
OrgChartManager(employee){
    ///
}
```

Das [Tutorial zum Entitätstyp „Liste“](luis-quickstart-intent-and-list-entity.md) zeigt, wie Mitarbeiter erstellt werden.

1. Wählen Sie oben im Menü **Erstellen** aus.

2. Wählen Sie im linken Navigationsbereich unter **Improve app performance** (App-Leistung verbessern) die Option **Muster** aus.

3. Wählen Sie die Absicht **OrgChart-Manager** aus, und geben Sie dann die folgende Vorlagenäußerung ein:

    |Vorlagenäußerungen|
    |:--|
    |Who is {Employee} the subordinate of[?]|
    |Who does {Employee} report to[?]|
    |Who is {Employee}['s] manager[?]|
    |Who does {Employee} directly report to[?]|
    |Who is {Employee}['s] supervisor[?]|
    |Who is the boss of {Employee}[?]|

    Entitäten mit Rollen verwenden eine Syntax, die den Rollennamen enthält. Sie werden in einem [separaten Tutorial für Rollen](luis-tutorial-pattern-roles.md) behandelt. 

    Bei der Eingabe der Vorlagenäußerung hilft LUIS Ihnen, die Entität auszufüllen, wenn Sie die linke geschweifte Klammer `{` verwenden.

    [![Screenshot der Eingabe von Vorlagenäußerungen für eine Absicht](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Wählen Sie die Absicht **OrgChart-Reports** aus, und geben Sie dann die folgende Vorlagenäußerung ein:

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

2. Wechseln Sie in den Browserregisterkarten wieder zur Endpunkt-URL-Registerkarte.

3. Geben Sie in der Adressleiste am Ende der URL `Who is the boss of Jill Jones?` als Äußerung ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```json
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

## <a name="working-with-optional-text-and-prebuilt-entities"></a>Arbeiten mit optionalem Text und vordefinierten Entitäten

Die vorherigen Mustervorlagenäußerungen in diesem Tutorial enthielten einige Beispiele für optionalen Text wie die possesive Verwendung des Buchstabens s, `'s`, und die Verwendung des Fragezeichens, `?`. Angenommen, die Endpunktäußerungen zeigen, dass Manager und Personalverantwortliche nach historischen Daten sowie geplanten Mitarbeiterumzügen innerhalb des Unternehmens zu einem späteren Zeitpunkt suchen.

Beispieläußerungen sind:

|Absicht|Beispieläußerungen mit optionalem Text und vordefinierten Entitäten|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

Jedes dieser Beispiele verwendet ein Verb, `was`, `is`, `will be`, sowie ein Datum, `March 3`, `now` und `in a month`, das LUIS korrekt voraussagen muss. Beachten Sie, dass die letzten beiden Beispiele fast den gleichen Text verwenden, mit Ausnahme von `in` und `on`.

Beispielhafte Vorlagenäußerungen:
|Absicht|Beispieläußerungen mit optionalem Text und vordefinierten Entitäten|
|:--|:--|
|OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

Durch die Verwendung der optionalen Syntax von eckigen Klammern, `[]`, kann dieser optionale Text zur Vorlagenäußerung hinzugefügt und bis zu einer zweiten Ebene, `[[]]`, verschachtelt werden und Entitäten oder Text enthalten.

**Frage: Warum konnten die letzten beiden Beispieläußerungen nicht zu einer Vorlagenäußerung zusammengefasst werden?** Diese Mustervorlage unterstützt nicht die ODER-Syntax. Um sowohl die `in`- als auch die `on`-Version zu erfassen, müssen diese jeweils separate Vorlagenäußerungen sein.

**Frage: Warum ist der Buchstabe `w` am Anfang jeder Vorlagenäußerung jeweils ein Kleinbuchstabe? Sollten dies nicht optional Groß- oder Kleinbuchstaben sein?** Die von der Clientanwendung an den Abfrageendpunkt übermittelte Äußerung wird in Kleinbuchstaben konvertiert. Die Vorlagenäußerung kann in Groß- oder Kleinbuchstaben geschrieben sein, bei der Endpunktäußerung besteht diese Möglichkeit ebenfalls. Der Vergleich erfolgt immer nach der Konvertierung in Kleinbuchstaben.

**Frage: Warum ist die vordefinierte Zahl nicht Teil der Vorlagenäußerung, wenn der 3. März sowohl als Zahl `3` als auch als Datum `March 3` vorhergesagt wird?** Die Vorlagenäußerung verwendet kontextuell ein Datum, entweder wörtlich wie in `March 3` oder abstrahiert wie `in a month`. Ein Datum kann eine Zahl enthalten, aber eine Zahl muss nicht unbedingt als Datum angesehen werden. Verwenden Sie immer die Entität, die den Typ am besten repräsentiert, den Sie in den JSON-Vorhersageergebnissen zurückgeben möchten.  

**Frage: Wie ist es bei schlecht formulierten Äußerungen, z.B. `Who will {Employee}['s] manager be on March 3?`** Grammatisch unterschiedliche Verbformen wie diese, bei denen `will` und `be` getrennt sind, müssen eine neue Vorlagenäußerung sein. Die vorhandene Vorlagenäußerung wird nicht übereinstimmen. Die Absicht der Äußerung nicht sich zwar nicht geändert, die Wortstellung aber schon. Diese Änderung wirkt sich auf die Vorhersage in LUIS aus.

**Beachten Sie: Entitäten werden zuerst gefunden, dann wird das Muster verglichen.**

## <a name="edit-the-existing-pattern-template-utterance"></a>Bearbeiten der vorhandenen Mustervorlagenäußerung

1. Wählen Sie auf der LUIS-Website die Option **Erstellen** im oberen Menü und anschließend **Muster** im linken Menü. 

2. Suchen Sie die vorhandene Vorlagenäußerung `Who is {Employee}['s] manager[?]`, und wählen Sie die Auslassungspunkte (***...*** ) auf der rechten Seite. 

3. Wählen Sie im Popupmenü **Bearbeiten** aus. 

4. Ändern Sie die Vorlagenäußerung in: `who is {Employee}['s] manager [[on]{datetimeV2}?]]`

## <a name="add-new-pattern-template-utterances"></a>Hinzufügen von neuen Mustervorlagenäußerungen

1. Fügen Sie im Abschnitt **Muster** unter **Erstellen** verschiedene neue Mustervorlagenäußerungen hinzu. Wählen Sie **OrgChart-Manager** im Dropdownmenü „Absicht“, und geben Sie jede der folgenden Vorlagenäußerungen ein:

    |Absicht|Beispieläußerungen mit optionalem Text und vordefinierten Entitäten|
    |--|--|
    |OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Trainieren Sie die App.

3. Wählen Sie im oberen Bereich **Test**, um den Testbereich zu öffnen. 

4. Geben Sie mehrere Testäußerungen ein, um zu überprüfen, ob dieses Muster abgeglichen wird und die Absichtsbewertung hoch genug ist. 

    Nachdem Sie die erste Äußerung eingegeben haben, wählen Sie in den Ergebnissen **Untersuchen**, damit alle Vorhersageergebnisse angezeigt werden.

    |Äußerung|
    |--|
    |Who will be Jill Jones manager|
    |who will be jill jones's manager|
    |Who will be Jill Jones's manager?|
    |who will be Jill jones manager on March 3|
    |Who will be Jill Jones manager next Month|
    |Who will be Jill Jones manager in a month?|

Alle diese Äußerungen beinhalten Entitäten, daher entsprechen sie dem gleichen Muster und haben einen hohen Vorhersagewert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial werden zwei Absichten für Äußerungen hinzugefügt, die ohne Vorliegen vieler Beispieläußerungen schwer mit hoher Genauigkeit vorherzusagen waren. Das Hinzufügen von Mustern zu diesen ermöglichte es LUIS, die Absicht mit einer erheblich höheren Punktzahl besser vorherzusagen. Das Kennzeichnen von Entitäten und ignorierbarem Text ermöglichte LUIS die Anwendung des Musters auf eine größere Vielfalt von Äußerungen.

> [!div class="nextstepaction"]
> [Verwenden von Rollen mit einem Muster](luis-tutorial-pattern-roles.md)

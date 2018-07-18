---
title: 'Tutorial: Erstellen einer LUIS-App zum Abrufen von Daten, die mit einem regulären Ausdruck abgeglichen wurden – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache LUIS-App erstellen, die Absichten und eine Entität vom Typ „Regulärer Ausdruck“ verwenden, um Daten zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: 522d24c1c03a338633c340502087300c890d1771
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128444"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Tutorial 3: Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie mithilfe der Entität **Regulärer Ausdruck** einheitlich formatierte Daten aus einer Äußerung extrahieren.


<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu Entitäten vom Typ „Regulärer Ausdruck“ 
> * Verwenden einer LUIS-App für eine Personaldomäne mit FindForm-Absicht
> * Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“ zum Extrahieren der Formularnummer aus einer Äußerung
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](luis-reference-regions.md#luis-website)-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem Tutorial zu [vordefinierten Entitäten](luis-tutorial-prebuilt-intents-entities.md) verfügen, [importieren](create-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS](luis-reference-regions.md#luis-website)-Website über das GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json)).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `regex`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 


## <a name="purpose-of-the-regular-expression-entity"></a>Zweck der Entität vom Typ „Regulärer Ausdruck“
Eine Entität dient dazu, wichtige Daten aus der Äußerung zu extrahieren. In der App wird die Entität vom Typ „Regulärer Ausdruck“ verwendet, um formatierte Personalformularnummern aus einer Äußerung zu extrahieren. Dabei wird kein Machine Learning verwendet. 

Hier sehen Sie einige einfache Beispieläußerungen:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Hier sehen Sie einige abgekürzte oder umgangssprachliche Versionen von Äußerungen:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Die Entität vom Typ „Regulärer Ausdruck“ für den Abgleich der Formularnummer lautet `hrf-[0-9]{6}`. Dieser reguläre Ausdruck gleicht die Literalzeichen `hrf -` ab, ignoriert aber die Groß-/Kleinschreibung sowie kulturelle Varianten. Außerdem werden genau sechs Ziffern (jeweils von 0 bis 9) abgeglichen.

„HRF“ steht für „Human Resources Form“ (Personalformular).

### <a name="tokenization-with-hyphens"></a>Tokenisierung mit Bindestrichen
LUIS tokenisiert die Äußerung, wenn diese einer Absicht hinzugefügt wird. Bei der Tokenisierung für diese Äußerungen werden vor und nach dem Bindestrich Leerzeichen eingefügt: `Where is HRF - 123456?`. Der reguläre Ausdruck wird auf die Rohform der Äußerung (vor der Tokenisierung) angewendet. Aufgrund der Anwendung auf die _Rohform_ muss der reguläre Ausdruck keine Wortgrenzen berücksichtigen. 


## <a name="add-findform-intent"></a>Hinzufügen der Absicht „FindForm“

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste auf **Build** klicken. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intents-regex-entity/first-image.png)](./media/luis-quickstart-intents-regex-entity/first-image.png#lightbox)

2. Klicken Sie auf **Create new intent** (Neue Absicht erstellen). 

    [ ![Screenshot: Seite „Intents“ (Absichten) mit hervorgehobener Schaltfläche „Create new intent“ (Neue Absicht erstellen)](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png) ](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png#lightbox)

3. Geben Sie im Popupdialogfeld die Zeichenfolge `FindForm` ein, und klicken Sie anschließend auf **Fertig**. 

    ![Screenshot: Dialogfeld zum Erstellen einer neuen Absicht mit eingegebenem Suchbegriff](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispieläußerungen|
    |--|
    |What is the URL for hrf-123456? (Wie lautet die URL für hrf-123456?)|
    |Where is hrf-345678? (Wo ist hrf-345678?)|
    |When was hrf-456098 updated? (Wann wurde hrf-456098 aktualisiert?)|
    |Did John Smith update hrf-234639 last week? (Hat John Smith hrf-234639 letzte Woche aktualisiert?)|
    |How many version of hrf-345123 are there? (Wie viele-Versionen von hrf-345123 gibt es?)|
    |Who needs to authorize form hrf-123456? (Wer muss das Formular hrf-123456 autorisieren?)|
    |How many people need to sign off on hrf-345678? (Wie viele Personen müssen hrf-345678 genehmigen?)|
    |hrf-234123 date? (hrf-234123 Datum?)|
    |author of hrf-546234? (Ersteller von hrf-546234?)|
    |title of hrf-456234? (Titel von hrf-456234?)|

    [ ![Screenshot: Seite „Intents“ (Absichten) mit hervorgehobenen neuen Äußerungen](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    Die Anwendung verfügt über die vorgefertigte Zahlenentität aus dem vorherigen Tutorial, sodass jede Formularnummer über ein Tag verfügt. Unter Umständen reicht das für Ihre Clientanwendung bereits aus, die Nummer ist jedoch nicht mit der Art der Nummer gekennzeichnet. Durch die Erstellung einer neuen Entität mit einem geeigneten Namen kann die Clientanwendung die Entität entsprechend verarbeiten, wenn sie von LUIS zurückgegeben wird.

## <a name="create-a-hrf-number-regular-expression-entity"></a>Erstellen einer Entität vom Typ „Regulärer Ausdruck“ namens „HRF-number“ 
In den folgenden Schritten erstellen Sie eine Entität vom Typ „Regulärer Ausdruck“, um LUIS mit dem Format von „HRF-number“ vertraut zu machen:

1. Klicken Sie im linken Bereich auf **Entitäten**.

2. Klicken Sie auf der Seite „Entitäten“ auf die Schaltfläche **Create new entity** (Neue Entität erstellen). 

    [![Screenshot: Seite „Entitäten“ mit hervorgehobener Schaltfläche „Create new entity“ (Neue Entität erstellen)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png#lightbox)

3. Geben Sie im daraufhin angezeigten Popupdialogfeld `HRF-number` als Name der neuen Entität, **RegEx** als Entitätstyp und `hrf-[0-9]{6}` als regulären Ausdruck ein, und klicken Sie anschließend auf **Fertig**.

    ![Screenshot: Popupdialogfeld zum Festlegen der Eigenschaften für die neue Entität](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Klicken Sie auf **Intents** (Absichten) und dann auf die Absicht **FindForm**, um den gekennzeichneten regulären Ausdruck in den Äußerungen zu sehen. 

    [![Screenshot: Gekennzeichnete Äußerungen mit vorhandener Entität und RegEx-Muster](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Da die Entität keine ML-Entität ist, wird die Bezeichnung sofort nach der Erstellung auf die Äußerungen angewendet und auf der LUIS-Website angezeigt.

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
Für eine Entität vom Typ „Regulärer Ausdruck“ ist zwar kein Training erforderlich, für die neue Absicht und die neuen Äußerungen dagegen schon. 

1. Klicken Sie rechts oben auf der LUIS-Website auf die Schaltfläche **Trainieren**.

    ![Abbildung der Schaltfläche „Trainieren“](./media/luis-quickstart-intents-regex-entity/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website eine grüne Statusleiste angezeigt wird.

    ![Abbildung der Benachrichtigungsleiste nach erfolgreicher Ausführung](./media/luis-quickstart-intents-regex-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie rechts oben auf der LUIS-Website auf die Schaltfläche **Veröffentlichen**. 

    ![Screenshot: „FindForm“ mit hervorgehobener Veröffentlichungsschaltfläche im oberen Navigationsbereich](./media/luis-quickstart-intents-regex-entity/publish-button.png)

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Veröffentlichen**.

    ![Screenshot: Veröffentlichungsseite mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot](./media/luis-quickstart-intents-regex-entity/publish-to-production.png)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website eine grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Dadurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    ![Screenshot: Veröffentlichungsseite mit hervorgehobener Endpunkt-URL](./media/luis-quickstart-intents-regex-entity/publish-select-endpoint.png)

2. Geben Sie auf der Adressleiste am Ende der URL Folgendes ein: `When were HRF-123456 and hrf-234567 published in the last year?`. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der gekennzeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `FindForm` mit den beiden Formularnummern `HRF-123456` und `hrf-234567` zurückgeben.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Die Nummern in der Äußerung werden zweimal zurückgegeben: einmal als die neue Entität `hrf-number` und einmal als vorgefertigte Entität `number`. Eine Äußerung kann mehrere Entitäten gleichen Typs besitzen, wie in diesem Beispiel zu sehen. Durch die Verwendung einer Entität vom Typ „Regulärer Ausdruck“ extrahiert LUIS benannte Daten. Dies ist programmatisch hilfreicher für die Clientanwendung, die die JSON-Antwort erhält.

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App hat die Absicht ermittelt und die extrahierten Daten zurückgegeben. 

Ihr Chatbot verfügt nun über genügend Informationen, um die primäre Aktion (`FindForm`) und die Formularnummern in der Suche zu bestimmen. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das topScoringIntent-Ergebnis und die Formularnummern für eine Suche über eine Drittanbieter-API verwenden. Diese Schritte werden nicht von LUIS ausgeführt. LUIS bestimmt lediglich die Absicht des Benutzers und extrahiert entsprechende Daten. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Klicken Sie im Menü oben links auf **Meine Apps**. Klicken Sie in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Listenentität](luis-quickstart-intent-and-list-entity.md)


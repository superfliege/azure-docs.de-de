---
title: 'Tutorial 3: Mit regulären Ausdrücken abgeglichene Daten – Extrahieren korrekt formatierter Daten'
titleSuffix: Azure Cognitive Services
description: Extrahieren Sie mithilfe der Entität vom Typ „Regulärer Ausdruck“ einheitlich formatierte Daten aus einer Äußerung.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 82d7e5ab57d9cf12c6917386282182faacb07725
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282389"
---
# <a name="tutorial-3-extract-well-formatted-data"></a>Tutorial 3: Extrahieren korrekt formatierter Daten
In diesem Tutorial ändern Sie die Personal-App so, dass mithilfe der Entität **Regulärer Ausdruck** einheitlich formatierte Daten aus einer Äußerung extrahiert werden.

Eine Entität dient dazu, wichtige Daten aus der Äußerung zu extrahieren. In dieser App wird die Entität vom Typ „Regulärer Ausdruck“ verwendet, um formatierte Personalformularnummern aus einer Äußerung zu extrahieren. Zwar wird die Absicht der Äußerung immer durch Machine Learning bestimmt, doch wird bei diesem spezifischen Entitätstyp kein Machine Learning verwendet. 

**Hier sehen Sie einige einfache Beispieläußerungen:**

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Ein regulärer Ausdruck ist eine gute Wahl für diese Art von Daten, wenn Folgendes gilt:

* Die Daten sind korrekt formatiert.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen der Absicht „FindForm“
> * Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“ 
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1. Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-prebuilts-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `regex`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind. 

## <a name="findform-intent"></a>Absicht „FindForm“

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

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

    [!INCLUDE[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“ 
Die Entität vom Typ „Regulärer Ausdruck“ für den Abgleich der Formularnummer lautet `hrf-[0-9]{6}`. Dieser reguläre Ausdruck gleicht die Literalzeichen `hrf-` ab, ignoriert aber die Groß-/Kleinschreibung sowie kulturelle Varianten. Außerdem werden genau sechs Ziffern (jeweils von 0 bis 9) abgeglichen.

HRF steht für `human resources form`.

LUIS tokenisiert die Äußerung, wenn diese einer Absicht hinzugefügt wird. Bei der Tokenisierung für diese Äußerungen werden vor und nach dem Bindestrich Leerzeichen eingefügt: `Where is HRF - 123456?`. Der reguläre Ausdruck wird auf die Rohform der Äußerung (vor der Tokenisierung) angewendet. Aufgrund der Anwendung auf die _Rohform_ muss der reguläre Ausdruck keine Wortgrenzen berücksichtigen. 

In den folgenden Schritten erstellen Sie eine Entität vom Typ „Regulärer Ausdruck“, um LUIS mit dem Format von „HRF-number“ vertraut zu machen:

1. Klicken Sie im linken Bereich auf **Entitäten**.

2. Klicken Sie auf der Seite „Entitäten“ auf die Schaltfläche **Create new entity** (Neue Entität erstellen). 

3. Geben Sie im daraufhin angezeigten Popupdialogfeld `HRF-number` als Name der neuen Entität, **RegEx** als Entitätstyp und `hrf-[0-9]{6}` als Wert für **RegEx** ein, und klicken Sie anschließend auf **Fertig**.

    ![Screenshot: Popupdialogfeld zum Festlegen der Eigenschaften für die neue Entität](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Wählen Sie im linken Menü die Option **Intents** (Absichten) und dann die Absicht **FindForm** aus, um den gekennzeichneten regulären Ausdruck in den Äußerungen zu sehen. 

    [![Screenshot: Gekennzeichnete Äußerungen mit vorhandener Entität und RegEx-Muster](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Da die Entität keine ML-Entität ist, wird die Bezeichnung sofort nach der Erstellung auf die Äußerungen angewendet und auf der LUIS-Website angezeigt.

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `When were HRF-123456 and hrf-234567 published in the last year?` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der gekennzeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `FindForm` mit den beiden Formularnummern `HRF-123456` und `hrf-234567` zurückgeben.

    ```JSON
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


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden eine neue Absicht erstellt, Beispieläußerungen hinzugefügt und dann eine Entität vom Typ „Regulärer Ausdruck“ erstellt, um korrekt formatierte Daten aus den Äußerungen zu extrahieren. Nach dem Trainieren und Veröffentlichen der App hat eine Abfrage des Endpunkts die Absicht identifiziert und die extrahierten Daten zurückgegeben.

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Listenentität](luis-quickstart-intent-and-list-entity.md)


---
title: 'Tutorial 9: Standpunktanalyse, einschließlich positiver, negativer und neutraler Stimmung in LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie positive, negative und neutrale Emotionen aus Äußerungen extrahieren. Die Stimmung wird anhand der gesamten Äußerung bestimmt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 95d1c4ffe76cf4c652f347014a838f1250c0ca15
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277476"
---
# <a name="tutorial-9--extract-sentiment-of-overall-utterance"></a>Tutorial 9: Extrahieren der Stimmung aus einer gesamten Äußerung
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie positive, negative und neutrale Emotionen aus Äußerungen extrahieren. Die Stimmung wird anhand der gesamten Äußerung bestimmt.

Mithilfe der Stimmungsanalyse können Sie ermitteln, ob die Äußerung eines Benutzers positiv, negativ oder neutral ist. 

Die folgenden Äußerungen sind Beispiele für die Stimmung:

|Stimmung|Punkte|Äußerung|
|:--|:--|:--|
|Positiv|0,91 |John W. Smith did a great job on the presentation in Paris. (John W. Smith hat bei der Präsentation in Paris gute Arbeit geleistet.)|
|Positiv|0,84 |jill-jones@mycompany.com did fabulous work on the Parker sales pitch. (jill-jones@mycompany.com hat bei der Parker-Verkaufspräsentation erstklassige Arbeit abgeliefert.)|

Die Standpunktanalyse ist eine Veröffentlichungseinstellung, die auf jede Äußerung angewendet wird. Sie müssen in der Äußerung nicht nach den Wörtern für die Standpunktangabe suchen und diese Wörter kennzeichnen, da für die Standpunktanalyse die gesamte Äußerung herangezogen wird. 

Da es sich um eine Veröffentlichungseinstellung handelt, wird diese nicht auf den Seiten mit Absichten oder Entitäten angezeigt. Sie wird aber im Bereich für den [interaktiven Test](luis-interactive-test.md#view-sentiment-results) oder beim Testen an der Endpunkt-URL angezeigt. 

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App 
> * Hinzufügen der Standpunktanalyse als Veröffentlichungseinstellung
> * Trainieren
> * Veröffentlichen
> * Abrufen der Stimmung einer Äußerung von einem Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App

Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-keyphrase-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `sentiment`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="employeefeedback-intent"></a>Absicht „EmployeeFeedback“ 
Fügen Sie eine neue Absicht für die Erfassung von internem Mitarbeiterfeedback hinzu. 

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus.

3. Nennen Sie die neue Absicht `EmployeeFeedback`.

    ![Dialogfeld zum Erstellen einer neuen Absicht namens „EmployeeFeedback“](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Fügen Sie mehrere Äußerungen hinzu, in denen ein Mitarbeiter für etwas gelobt wird oder die einen Bereich mit Verbesserungsbedarf angeben:

    Zur Erinnerung: In dieser Personal-App werden Mitarbeiter in der Listenentität (`Employee`) mit Name, E-Mail-Adresse, Durchwahl, Mobiltelefonnummer und US-Sozialversicherungsnummer definiert. 

    |Äußerungen|
    |--|
    |425-555-1212 did a nice job of welcoming back a co-worker from maternity leave (425-555-1212 hat einer Kollegin den Wiedereinstieg nach ihrem Mutterschaftsurlaub erleichtert.)|
    |234-56-7891 did a great job of comforting a co-worker in their time of grief. (234-56-7891 hat sich mitfühlend um einen trauernden Kollegen gekümmert.)|
    |jill-jones@mycompany.com didn't have all the required invoices for the paperwork. (jill-jones@mycompany.com hatte nicht alle erforderlichen Rechnungen für die Unterlagen beisammen.)|
    |john.w.smith@mycompany.com turned in the required forms a month late with no signatures (john.w.smith@mycompany.com hat die erforderlichen Formulare einen Monat zu spät und ohne Unterschrift eingereicht.)|
    |x23456 didn't make it to the important marketing off-site meeting. (x23456 war bei der wichtigen externen Marketingbesprechung nicht anwesend.)|
    |x12345 missed the meeting for June reviews. (x12345 war bei der Besprechung für die Juni-Prüfung nicht dabei.)|
    |Jill Jones rocked the sales pitch at Harvard (Jill Jones hat die Verkaufspräsentation für Harvard exzellent gemeistert.)|
    |John W. Smith did a great job on the presentation at Stanford. (John W. Smith hat bei der Präsentation für Stanford gute Arbeit geleistet.)|

    [ ![Screenshot: LUIS-App mit Beispieläußerungen in der Absicht „EmployeeFeedback“](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Konfigurieren der App zur Aufnahme der Stimmungsanalyse
1. Klicken Sie im Navigationsbereich oben rechts auf **Verwalten**, und wählen Sie dann im linken Menü die Option **Veröffentlichungseinstellungen** aus.

2. Schalten Sie zum Aktivieren dieser Einstellung auf **Standpunktanalyse** um. 

    ![](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-sentiment-of-utterance-from-endpoint"></a>Abrufen der Stimmung einer Äußerung von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Jill Jones work with the media team on the public portal was amazing` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `EmployeeFeedback` mit der extrahierten Stimmungsanalyse zurückgeben.
    
    ```JSON
    {
      "query": "Jill Jones work with the media team on the public portal was amazing",
      "topScoringIntent": {
        "intent": "EmployeeFeedback",
        "score": 0.4983256
      },
      "intents": [
        {
          "intent": "EmployeeFeedback",
          "score": 0.4983256
        },
        {
          "intent": "MoveEmployee",
          "score": 0.06617523
        },
        {
          "intent": "GetJobInformation",
          "score": 0.04631853
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0103248553
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.007531875
        },
        {
          "intent": "FindForm",
          "score": 0.00344597152
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00337914471
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0026357458
        },
        {
          "intent": "None",
          "score": 0.00214573368
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00157622492
        },
        {
          "intent": "Utilities.Confirm",
          "score": 7.379545E-05
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 0,
          "endIndex": 9,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "media team",
          "type": "builtin.keyPhrase",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "public portal",
          "type": "builtin.keyPhrase",
          "startIndex": 43,
          "endIndex": 55
        },
        {
          "entity": "jill jones",
          "type": "builtin.keyPhrase",
          "startIndex": 0,
          "endIndex": 9
        }
      ],
      "sentimentAnalysis": {
        "label": "positive",
        "score": 0.8694164
      }
    }
    ```

    Die Standpunktanalyse ist positiv (Wert: 0,86). 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wird die Standpunktanalyse als Veröffentlichungseinstellung zum Extrahieren von Stimmungswerten aus der gesamten Äußerung hinzugefügt.

> [!div class="nextstepaction"] 
> [Überprüfen von Endpunktäußerungen in der Personal-App](luis-tutorial-review-endpoint-utterances.md) 


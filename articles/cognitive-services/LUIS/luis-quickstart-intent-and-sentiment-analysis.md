---
title: 'Tutorial: Erstellen einer LUIS-App zum Zurückgeben einer Stimmungsanalyse: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine Stimmungsanalyse zu Ihrer LUIS-App hinzufügen, um Äußerungen auf positive, negative und neutrale Emotionen zu analysieren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: a89755bcc0ed5ef8bee4ed00b99c73993a57bcb9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163021"
---
# <a name="tutorial-9--add-sentiment-analysis"></a>Tutorial: 9.  Hinzufügen einer Standpunktanalyse
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie positive, negative und neutrale Emotionen aus Äußerungen extrahieren.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zur Standpunktanalyse
> * Verwenden der LUIS-App im Personalbereich 
> * Hinzufügen einer Standpunktanalyse
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort 

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem Tutorial zur [vordefinierten keyPhrase-Entität](luis-quickstart-intent-and-key-phrase.md) verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `sentiment`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="sentiment-analysis"></a>Stimmungsanalyse
Mithilfe der Stimmungsanalyse können Sie ermitteln, ob die Äußerung eines Benutzers positiv, negativ oder neutral ist. 

Die folgenden Äußerungen sind Beispiele für die Stimmung:

|Stimmung|Punkte|Äußerung|
|:--|:--|:--|
|Positiv|0,91 |John W. Smith did a great job on the presentation in Paris. (John W. Smith hat bei der Präsentation in Paris gute Arbeit geleistet.)|
|Positiv|0,84 |jill-jones@mycompany.com did fabulous work on the Parker sales pitch. (jill-jones@mycompany.com hat bei der Parker-Verkaufspräsentation erstklassige Arbeit abgeliefert.)|

Die Standpunktanalyse ist eine App-Einstellung, die auf jede Äußerung angewendet wird. Sie müssen in der Äußerung nicht nach den Wörtern für die Standpunktangabe suchen und diese Wörter kennzeichnen, da für die Standpunktanalyse die gesamte Äußerung herangezogen wird. 

## <a name="add-employeefeedback-intent"></a>Hinzufügen der Absicht „EmployeeFeedback“ 
Fügen Sie eine neue Absicht für die Erfassung von internem Mitarbeiterfeedback hinzu. 

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste auf **Build** klicken. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus.

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

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

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Konfigurieren der App zur Aufnahme der Stimmungsanalyse
Konfigurieren Sie die Standpunktanalyse auf der Seite **Veröffentlichen**. 

1. Klicken Sie im Navigationsbereich oben rechts auf **Veröffentlichen**.

    ![Screenshot der Seite „Absicht“ mit erweiterter Schaltfläche „Veröffentlichen“ ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. Aktivieren Sie **Enable Sentiment Analysis** (Stimmungsanalyse aktivieren). 

## <a name="publish-app-to-endpoint"></a>Veröffentlichen der App im Endpunkt

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Jill Jones work with the media team on the public portal was amazing` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `EmployeeFeedback` mit der extrahierten Stimmungsanalyse zurückgeben.

```
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

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit aktivierter Stimmungsanalyse hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten sowie die allgemeine Stimmung als Punktzahl zurückgegeben. 

Ihr Chatbot verfügt nun über ausreichend Informationen, um den nächsten Schritt in der Unterhaltung zu bestimmen. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Stimmungsdaten aus der Äußerung verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Überprüfen von Endpunktäußerungen in der Personal-App](luis-tutorial-review-endpoint-utterances.md) 


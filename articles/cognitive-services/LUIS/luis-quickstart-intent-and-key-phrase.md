---
title: 'Tutorial: Erstellen einer LUIS-App zum Zurückgeben von Schlüsselbegriffen: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Entität „keyPhrase“ zur LUIS-App hinzufügen, um Äußerungen auf Schlüsselinhalte zu analysieren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: d5b92681b9677db085b3c271c37490154d781d84
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494309"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Tutorial: 8. Hinzufügen der keyPhrase-Entität 
In diesem Tutorial verwenden Sie eine App, die veranschaulicht, wie Sie Schlüsselinhalte aus Äußerungen extrahieren.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu keyPhrase-Entitäten 
> * Verwenden der LUIS-App im Personalbereich 
> * Hinzufügen der keyPhrase-Entität zum Extrahieren von Schlüsselinhalten aus einer Äußerung
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort (einschließlich Schlüsselbegriffen)

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem [Tutorial zur einfachen Entität](luis-quickstart-primary-and-secondary-data.md) verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `keyphrase`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="keyphrase-entity-extraction"></a>keyPhrase-Entitätsextraktion
Schlüsselinhalte werden von der vordefinierten Entität **keyPhrase** bereitgestellt. Diese Entität gibt Schlüsselinhalte aus der Äußerung zurück.

Die folgenden Äußerungen sind Beispiele für Schlüsselbegriffe:

|Äußerung|keyPhrase-Entitätswerte|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Gibt es nächstes Jahr eine neue Krankenversicherung mit einem niedrigeren Selbstbehalt?)|„lower deductible“ (niedrigeren Selbstbehalt)<br>„new medical plan“ (neue Krankenversicherung)<br>„year“ (Jahr)|
|Is vision therapy covered in the high deductible medical plan? (Ist Visionstherapie in der Krankenversicherung mit hohem Selbstbehalt abgedeckt?)|„high deductible medical plan“ (Krankenversicherung mit hohem Selbstbehalt)<br>„vision therapy“ (Visionstherapie)|

Ihre Clientanwendung kann diese Werte zusammen mit anderen extrahierten Entitäten verwenden, um zu entscheiden, wie es in dem Gespräch weitergehen soll.

## <a name="add-keyphrase-entity"></a>Hinzufügen der keyPhrase-Entität 
Fügen Sie die vordefinierte keyPhrase-Entität hinzu, um Schlüsselinhalte aus Äußerungen zu extrahieren.

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste **Build** auswählen. 

2. Wählen Sie im linken Menü die Option **Entitäten**.

    [ ![Screenshot mit hervorgehobener Option „Entitäten“ im linken Navigationsbereich des Abschnitts „Erstellen“](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. Klicken Sie auf **Manage prebuilt entities** (Vordefinierte Entitäten verwalten).

    [ ![Screenshot des Popupdialogfensters mit der Liste der Entitäten](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. Klicken Sie im Popupdialogfenster auf **keyPhrase** und anschließend auf **Fertig**. 

    [ ![Screenshot des Popupdialogfensters mit der Liste der Entitäten](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. Klicken Sie im linken Menü auf **Absichten**, und wählen Sie die Absicht **Utilities.Confirm** aus. Die keyPhrase-Entität ist in mehreren Äußerungen gekennzeichnet. 

    [ ![Screenshot: Absicht „Utilities.Confirm“ mit keyPhrases-Kennzeichnung in Äußerungen](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Veröffentlichen der App im Endpunkt

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]


## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `does form hrf-123456 cover the new dental benefits and medical plan` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

```
{
  "query": "does form hrf-123456 cover the new dental benefits and medical plan",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9300641
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.9300641
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0359598845
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0141798034
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0112197418
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00507669244
    },
    {
      "intent": "None",
      "score": 0.00238501839
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00202810857
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00102957746
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0008688423
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.557994E-05
    }
  ],
  "entities": [
    {
      "entity": "hrf-123456",
      "type": "HRF-number",git 
      "startIndex": 10,
      "endIndex": 19
    },
    {
      "entity": "new dental benefits",
      "type": "builtin.keyPhrase",
      "startIndex": 31,
      "endIndex": 49
    },
    {
      "entity": "medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 55,
      "endIndex": 66
    },
    {
      "entity": "hrf",
      "type": "builtin.keyPhrase",
      "startIndex": 10,
      "endIndex": 12
    },
    {
      "entity": "-123456",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 19,
      "resolution": {
        "value": "-123456"
      }
    }
  ]
}
```

Bei der Suche nach einem Formular hat der Benutzer mehr Informationen angegeben als erforderlich. Die zusätzlichen Informationen werden als **builtin.keyPhrase** zurückgegeben. Die Clientanwendung kann diese zusätzlichen Informationen für Folgefragen nutzen (Beispiel: Möchten Sie mit einem Mitarbeiter der Personalabteilung über Zahnarztleistungen unterhalten?) oder ein Menü mit weiteren Optionen anzeigen (einschließlich einer Option für weitere Informationen zu Zahnarztleistungen oder zur medizinischen Versorgung).

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit keyPhrase-Entitätserkennung hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten sowie den Hauptschlüsselinhalt zurückgegeben. 

Ihr Chatbot verfügt nun über ausreichend Informationen, um den nächsten Schritt in der Unterhaltung zu bestimmen. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die keyPhrase-Daten aus der Äußerung verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer App, die neben der Absichtsvorhersage die Stimmung zurückgibt](luis-quickstart-intent-and-sentiment-analysis.md)
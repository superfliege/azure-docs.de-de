---
title: 'Tutorial: Erstellen einer LUIS-App zum Zurückgeben von Schlüsselbegriffen: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Entität „keyPhrase“ zur LUIS-App hinzufügen, um Äußerungen auf Schlüsselinhalte zu analysieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12c306b5199da5862302c28d1690b81c6e1edb0e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264614"
---
# <a name="tutorial-create-app-that-returns-keyphrases-entity-data-found-in-utterances"></a>Tutorial: Erstellen einer App, die in Äußerungen ermittelte keyPhrases-Entitätsdaten zurückgibt
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie Schlüsselinhalte aus Äußerungen extrahieren.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu keyPhrase-Entitäten 
> * Erstellen einer neuen LUIS-App für den Personalbereich
> * Hinzufügen der Absicht vom Typ _None_ (Keine) und Hinzufügen von Beispieläußerungen
> * Hinzufügen der keyPhrase-Entität zum Extrahieren von Schlüsselinhalten aus einer Äußerung
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

Für diesen Artikel können Sie das kostenlose [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung verwenden.

## <a name="keyphrase-entity-extraction"></a>keyPhrase-Entitätsextraktion
Schlüsselinhalte werden von der vordefinierten Entität **keyPhrase** bereitgestellt. Diese Entität gibt Schlüsselinhalte in der Äußerung zurück.

Die folgenden Äußerungen sind Beispiele für Schlüsselbegriffe:

|Äußerung|keyPhrase-Entitätswerte|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Gibt es nächstes Jahr eine neue Krankenversicherung mit einem niedrigeren Selbstbehalt?)|„lower deductible“ (niedrigeren Selbstbehalt)<br>„new medical plan“ (neue Krankenversicherung)<br>„year“ (Jahr)|
|Is vision therapy covered in the high deductible medical plan? (Ist Visionstherapie in der Krankenversicherung mit hohem Selbstbehalt abgedeckt?)|„high deductible medical plan“ (Krankenversicherung mit hohem Selbstbehalt)<br>„vision therapy“ (Visionstherapie)|

Ihr Chatbot kann zusätzlich zu allen anderen extrahierten Entitäten diese Werte für den nächsten Schritt in der Unterhaltung berücksichtigen.

## <a name="download-sample-app"></a>Herunterladen der Beispiel-App
Laden Sie die [Personal](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json)-App herunter, und speichern Sie sie in einer Datei mit der Erweiterung „*.json“. Diese Beispiel-App erkennt Äußerungen, die relevant sind für Sozialleistungen, Organigramme und Sachwerte.

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich bei der [LUIS][LUIS]-Website an. Achten Sie darauf, sich unter der [Region][LUIS-regions] anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS][LUIS]-Website auf **Import new app** (Neue App importieren), um die im vorherigen Abschnitt heruntergeladene Personal-App zu importieren. 

    [![](media/luis-quickstart-intent-and-key-phrase/app-list.png "Screenshot der Seite mit der App-Liste")](media/luis-quickstart-intent-and-key-phrase/app-list.png#lightbox)

3. Geben Sie im Dialogfeld **Import new app** (Neue App importieren) den Namen `Human Resources with Key Phrase entity` für die App ein. 

    ![Bild des Dialogfelds „Neue App erstellen“](./media/luis-quickstart-intent-and-key-phrase/import-new-app-inline.png)

    Nach der Erstellung der App wird von LUIS die Absichtsliste angezeigt.

    [![](media/luis-quickstart-intent-and-key-phrase/intents-list.png "Screenshot der Seite mit der Liste der Absichten")](media/luis-quickstart-intent-and-key-phrase/intents-list.png#lightbox)

## <a name="add-keyphrase-entity"></a>Hinzufügen der keyPhrase-Entität 
Fügen Sie die vordefinierte keyPhrase-Entität hinzu, um Schlüsselinhalte aus Äußerungen zu extrahieren.

1. Wählen Sie im linken Menü die Option **Entitäten**.

    [ ![Screenshot mit hervorgehobener Option „Entitäten“ im linken Navigationsbereich des Abschnitts „Erstellen“](./media/luis-quickstart-intent-and-key-phrase/select-entities.png)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png#lightbox)

2. Klicken Sie auf **Manage prebuilt entities** (Vordefinierte Entitäten verwalten).

    [ ![Screenshot des Popupdialogfensters mit der Liste der Entitäten](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png#lightbox)

3. Klicken Sie im Popupdialogfenster auf **keyPhrase** und anschließend auf **Fertig**. 

    [ ![Screenshot des Popupdialogfensters mit der Liste der Entitäten](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS weiß erst über diese Änderung am Modell Bescheid, wenn der Dienst trainiert wurde. 

1. Klicken Sie rechts oben auf der LUIS-Website auf die Schaltfläche **Trainieren**.

    ![Screenshot der hervorgehobenen Schaltfläche „Trainieren“](./media/luis-quickstart-intent-and-key-phrase/train-button-expanded.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Screenshot der Benachrichtigungsleiste zum Trainingserfolg ](./media/luis-quickstart-intent-and-key-phrase/trained-inline.png)

## <a name="publish-app-to-endpoint"></a>Veröffentlichen der App im Endpunkt

1. Klicken Sie im Navigationsbereich oben rechts auf **Veröffentlichen**.

    ![Screenshot der Seite „Entität“ mit erweiterter Schaltfläche „Veröffentlichen“ ](./media/luis-quickstart-intent-and-key-phrase/publish-expanded.png)

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Veröffentlichen**.

    [![](media/luis-quickstart-intent-and-key-phrase/publish-to-production-inline.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-key-phrase/publish-to-production-expanded.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Dadurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    ![Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Endpunkt-URL](media/luis-quickstart-intent-and-key-phrase/endpoint-url-inline.png )

2. Geben Sie in der Adressleiste am Ende der URL `Is there a new medical plan with a lower deductible offered next year?` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

```
{
  "query": "Is there a new medical plan with a lower deductible offered next year?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.216838628
  },
  "entities": [
    {
      "entity": "lower deductible",
      "type": "builtin.keyPhrase",
      "startIndex": 35,
      "endIndex": 50
    },
    {
      "entity": "new medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 26
    },
    {
      "entity": "year",
      "type": "builtin.keyPhrase",
      "startIndex": 65,
      "endIndex": 68
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit keyPhrase-Entitätserkennung hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten sowie den Hauptschlüsselinhalt zurückgegeben. 

Ihr Chatbot verfügt nun über ausreichend Informationen, um den nächsten Schritt in der Unterhaltung zu bestimmen. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die keyPhrase-Daten aus der Äußerung verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Klicken Sie dazu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer App, die neben der Absichtsvorhersage die Stimmung zurückgibt](luis-quickstart-intent-and-sentiment-analysis.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions

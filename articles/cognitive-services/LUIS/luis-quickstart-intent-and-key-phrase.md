---
title: 'Tutorial: Erstellen einer LUIS-App zum Zurückgeben von Schlüsselbegriffen: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Entität „keyPhrase“ zur LUIS-App hinzufügen, um Äußerungen auf Schlüsselinhalte zu analysieren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: 1cafca2433cd96c0595a6124df82856d0c491a49
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224261"
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

Für diesen Artikel können Sie das kostenlose [LUIS](luis-reference-regions.md#publishing-regions)-Konto für die Erstellung Ihrer LUIS-Anwendung verwenden.

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

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste auf **Build** klicken. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png)](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png#lightbox)

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
Die neue `keyphrase`-Version der App muss trainiert werden.  

1. Wählen Sie oben rechts auf der LUIS-Website die Schaltfläche **Train** (Trainieren) aus.

    ![Trainieren der App](./media/luis-quickstart-intent-and-key-phrase/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Training erfolgreich](./media/luis-quickstart-intent-and-key-phrase/trained.png)

## <a name="publish-app-to-endpoint"></a>Veröffentlichen der App im Endpunkt

1. Klicken Sie im Navigationsbereich oben rechts auf **Veröffentlichen**.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png#lightbox)

2. Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Dadurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    ![Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Endpunkt-URL](media/luis-quickstart-intent-and-key-phrase/hr-endpoint-url-inline.png )

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
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie im Menü oben links die Option **Meine Apps**. Wählen Sie in der App-Liste rechts vom App-Namen die Auslassungspunkte (***...***) und anschließend die Option **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer App, die neben der Absichtsvorhersage die Stimmung zurückgibt](luis-quickstart-intent-and-sentiment-analysis.md)
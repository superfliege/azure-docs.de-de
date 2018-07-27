---
title: Hinzufügen von vordefinierten Absichten und Entitäten zum Extrahieren von allgemeinen Daten in LUIS – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie vordefinierte Absichten und Entitäten verwenden, um verschiedene Typen von Entitätsdaten zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: 387f20d2080a67041c90ec1af93e791716839dd9
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929048"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Tutorial: 2. Hinzufügen von vordefinierten Absichten und Entitäten
Fügen Sie der Personal-App aus dem Tutorial vordefinierte Absichten und Entitäten hinzu, um schnell Absichtsvorhersagen und extrahierte Daten zu erhalten. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
* Hinzufügen vordefinierter Absichten 
* Hinzufügen der vordefinierten Entitäten datetimeV2 und number
* Trainieren und Veröffentlichen
* Abfragen von LUIS und Empfangen von Vorhersageantworten

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die [Personal](luis-quickstart-intents-only.md)-App aus dem vorherigen Tutorial verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS](luis-reference-regions.md#luis-website)-Website über das GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json)).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `prebuilts`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="add-prebuilt-intents"></a>Hinzufügen vordefinierter Absichten
LUIS enthält mehrere vordefinierte Absichten für allgemeine Benutzerabsichten.  

1. Vergewissern Sie sich, dass sich Ihre App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste **Build** auswählen. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

2. Wählen Sie **Add prebuilt domain intent** (Vordefinierte Domänenabsicht hinzufügen) aus. 

    [ ![Screenshot der Seite „Absichten“ mit hervorgehobener Schaltfläche zum Hinzufügen vordefinierter Domänenabsichten](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

3. Suchen Sie nach `Utilities`. 

    [ ![Screenshot des Dialogfelds zum Hinzufügen vordefinierter Absichten mit eingegebenem Suchbegriff „Utilities“](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Wählen Sie die folgenden Absichten und dann **Fertig** aus: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Stop


## <a name="add-prebuilt-entities"></a>Hinzufügen vordefinierter Entitäten
LUIS enthält mehrere vordefinierte Entitäten für das Extrahieren allgemeiner Daten. 

1. Wählen Sie im linken Navigationsmenü die Option **Entities** (Entitäten) aus.

    [ ![Screenshot der Absichtenliste mit hervorgehobener Option „Entities“ (Entitäten) im linken Navigationsbereich](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Wählen Sie die Schaltfläche **Manage prebuilt entities** (Vordefinierte Entitäten verwalten) aus.

    [![Screenshot: Liste mit Entitäten mit hervorgehobener Option „Manage prebuilt entities“ (Vordefinierte Entitäten verwalten)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Wählen Sie in der Liste mit den vordefinierten Entitäten **number** und **datetimeV2** und danach **Fertig** aus.

    ![Screenshot: Auswahl von „number“ im Dialogfeld mit den vordefinierten Entitäten](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App
1. Wählen Sie oben rechts auf der LUIS-Website die Schaltfläche **Train** (Trainieren) aus. 

    ![Schaltfläche „Train“ (Trainieren)](./media/luis-quickstart-intents-only/train-button.png)

    Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Statusleiste bei abgeschlossenem Training](./media/luis-quickstart-intents-only/trained.png)

2. Wählen Sie rechts oben auf der LUIS-Website die Schaltfläche **Publish** (Veröffentlichen) aus, um die Seite „Publish“ (Veröffentlichen) zu öffnen. 

3. Der Produktionsslot ist standardmäßig aktiviert. Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    Vor dem Veröffentlichen oder dem Testen der Endpunkt-URL müssen Sie keinen LUIS-Endpunktschlüssel im Azure-Portal erstellen. Jede LUIS-App verfügt über einen kostenlosen Startschlüssel für die Erstellung. Dieser ermöglicht das unbegrenzte Erstellen und eine [geringe Anzahl an Endpunkttreffern](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung
Wählen Sie unten auf der Seite **Publish** (Veröffentlichen) den Link **endpoint** (Endpunkt) aus. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. Geben Sie in der Adressleiste am Ende der URL `I want to cancel on March 3` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

Das Ergebnis hat die Absicht „Utilities.Cancel“ vorhergesagt und als Datum den 3. März und die Zahl 3 extrahiert. 

    ```
    {
      "query": "I want to cancel on March 3",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.7818295
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.7818295
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0237864349
        },
        {
          "intent": "GetJobInformation",
          "score": 0.017576348
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0130122062
        },
        {
          "intent": "Utilities.Help",
          "score": 0.006731322
        },
        {
          "intent": "None",
          "score": 0.00524190161
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.004912514
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00092950504
        }
      ],
      "entities": [
        {
          "entity": "march 3",
          "type": "builtin.datetimeV2.date",
          "startIndex": 20,
          "endIndex": 26,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2018-03-03"
              },
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2019-03-03"
              }
            ]
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 26,
          "endIndex": 26,
          "resolution": {
            "value": "3"
          }
        }
      ]
    }
    ```

Es sind zwei Werte für den 3. März vorhanden, da die Äußerung keine Angaben dazu enthielt, ob der 3. März in der Vergangenheit oder in der Zukunft liegt. Es ist Aufgabe der Anwendung, mit der LUIS aufgerufen wird, eine Annahme zu treffen oder nachzufragen, falls dies erforderlich ist. 

Durch das einfache und schnelle Hinzufügen von vordefinierten Absichten und Entitäten kann die Clientanwendung eine Unterhaltungsverwaltung hinzufügen und allgemeine Datentypen extrahieren. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie hierzu im Menü oben links die Option **Meine Apps**. Klicken Sie in der Liste rechts vom App-Namen auf die Auslassungspunkte (***...***) und anschließend auf **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“ zur App](luis-quickstart-intents-regex-entity.md)


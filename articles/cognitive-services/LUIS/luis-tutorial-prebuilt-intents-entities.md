---
title: 'Tutorial 2: Vordefinierte Absichten und Entitäten – Verwenden vordefinierter allgemeiner Äußerungen – Extrahieren allgemeiner Daten in LUIS'
titleSuffix: Azure Cognitive Services
description: Fügen Sie der Personal-App aus dem Tutorial vordefinierte Absichten und Entitäten hinzu, um schnell Absichtsvorhersagen und extrahierte Daten zu erhalten. Sie müssen Äußerungen nicht mit vordefinierten Entitäten bezeichnen. Die Entität wird automatisch erkannt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 3bad68d1a388a5bc8780df633313206afaadcef9
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422421"
---
# <a name="tutorial-2-identify-common-intents-and-entities"></a>Tutorial 2: Identifizieren häufiger Absichten und Entitäten
In diesem Tutorial ändern Sie die Personal-App. Fügen Sie der Personal-App aus dem Tutorial vordefinierte Absichten und Entitäten hinzu, um schnell Absichtsvorhersagen und extrahierte Daten zu erhalten. Sie müssen Äußerungen nicht mit vordefinierten Entitäten bezeichnen, da die Entität automatisch erkannt wird.

Vordefinierte Modelle allgemeiner Themendomänen und Datentypen unterstützen Sie beim schnellen Erstellen Ihres Modells und dienen als Beispiele für das Aussehen eines Modells. 

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen vordefinierter Absichten 
> * Hinzufügen vordefinierter Entitäten 
> * Trainieren 
> * Veröffentlichen 
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-intent-only-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `prebuilts`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind. 

## <a name="add-prebuilt-intents"></a>Hinzufügen vordefinierter Absichten
LUIS enthält mehrere vordefinierte Absichten für allgemeine Benutzerabsichten.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie **Add prebuilt intent** (Vordefinierte Absicht hinzufügen) aus. 

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

2. Wählen Sie die Schaltfläche **Manage prebuilt entity** (Vordefinierte Entität verwalten) aus.

3. Wählen Sie in der Liste mit den vordefinierten Entitäten **number** und **datetimeV2** und danach **Fertig** aus.

    ![Screenshot: Auswahl von „number“ im Dialogfeld mit den vordefinierten Entitäten](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Wechseln Sie an das Ende der URL in der Adressleiste des Browsers, und geben Sie `I want to cancel on March 3` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```JSON
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

    Das Ergebnis hat die Absicht „Utilities.Cancel“ vorhergesagt und als Datum den 3. März und die Zahl 3 extrahiert. 

    Es sind zwei Werte für den 3. März vorhanden, da die Äußerung keine Angaben dazu enthielt, ob der 3. März in der Vergangenheit oder in der Zukunft liegt. Es ist Aufgabe der Clientanwendung, eine Annahme zu treffen oder nachzufragen, falls dies erforderlich ist. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Durch das Hinzufügen vordefinierter Absichten und Entitäten kann die Clientanwendung allgemeine Benutzerabsichten bestimmen und allgemeine Datentypen extrahieren. 

> [!div class="nextstepaction"]
> [Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“ zur App](luis-quickstart-intents-regex-entity.md)


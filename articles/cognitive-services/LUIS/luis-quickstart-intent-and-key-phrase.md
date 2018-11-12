---
title: 'Tutorial 8: Schlüsselbegriffserkennung in LUIS'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die vordefinierte keyPhrase-Entität, um Schlüsselinhalte aus Äußerungen zu extrahieren. Sie müssen Äußerungen nicht mit vordefinierten Entitäten bezeichnen. Die Entität wird automatisch erkannt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d6feb24b1e59aee70204d8438f1a4c51f71d1835
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281811"
---
# <a name="tutorial-8-extract-key-phrases-of-utterance"></a>Tutorial 8: Extrahieren von Schlüsselbegriffen einer Äußerung
In diesem Tutorial verwenden Sie die vordefinierte keyPhrase-Entität, um Schlüsselinhalte aus Äußerungen zu extrahieren. Sie müssen Äußerungen nicht mit vordefinierten Entitäten bezeichnen. Die Entität wird automatisch erkannt.

Die folgenden Äußerungen sind Beispiele für Schlüsselbegriffe:

|Äußerung|keyPhrase-Entitätswerte|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Gibt es nächstes Jahr eine neue Krankenversicherung mit einem niedrigeren Selbstbehalt?)|„lower deductible“ (niedrigeren Selbstbehalt)<br>„new medical plan“ (neue Krankenversicherung)<br>„year“ (Jahr)|
|Is vision therapy covered in the high deductible medical plan? (Ist Visionstherapie in der Krankenversicherung mit hohem Selbstbehalt abgedeckt?)|„high deductible medical plan“ (Krankenversicherung mit hohem Selbstbehalt)<br>„vision therapy“ (Visionstherapie)|

Ihre Clientanwendung kann diese Werte zusammen mit anderen extrahierten Entitäten verwenden, um zu entscheiden, wie es in dem Gespräch weitergehen soll.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen der keyPhrase-Entität 
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App

Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-simple-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `keyphrase`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="add-keyphrase-entity"></a>Hinzufügen der keyPhrase-Entität 
Fügen Sie die vordefinierte keyPhrase-Entität hinzu, um Schlüsselinhalte aus Äußerungen zu extrahieren.

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie im linken Menü die Option **Entitäten**.

3. Wählen Sie **Add prebuilt entity** (Vordefinierte Entität hinzufügen) aus.

4. Klicken Sie im Popupdialogfenster auf **keyPhrase** und anschließend auf **Fertig**. 

    [ ![Screenshot des Popupdialogfensters mit der Liste der Entitäten](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

5. Klicken Sie im linken Menü auf **Absichten**, und wählen Sie die Absicht **Utilities.Confirm** aus. Die keyPhrase-Entität ist in mehreren Äußerungen gekennzeichnet. 

    [ ![Screenshot: Absicht „Utilities.Confirm“ mit keyPhrases-Kennzeichnung in Äußerungen](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `does form hrf-123456 cover the new dental benefits and medical plan` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 
    
    ```JSON
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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde die vordefinierte keyPhrase-Entität hinzugefügt, durch die Schlüsselbegriffe in Äußerungen schnell bereitgestellt werden, ohne die Äußerungen kennzeichnen zu müssen. 

> [!div class="nextstepaction"]
> [Erstellen einer App, die neben der Absichtsvorhersage die Stimmung zurückgibt](luis-quickstart-intent-and-sentiment-analysis.md)
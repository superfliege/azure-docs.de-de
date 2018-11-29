---
title: 'Tutorial 4: Genaue Textübereinstimmungen – LUIS-Listenentität'
titleSuffix: Azure Cognitive Services
description: Abrufen von Daten, die mit einer vordefinierten Liste von Elementen übereinstimmen. Jedes Element auf der Liste kann Synonyme aufweisen, die ebenfalls exakt übereinstimmen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: a4e294687b6c3ea2ba6ff8003e7a8f1ac69ea639
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425070"
---
# <a name="tutorial-4-extract-exact-text-matches"></a>Tutorial 4: Extrahieren genauer Textübereinstimmungen
In diesem Tutorial lernen Sie, wie Daten abgerufen werden, die mit einer vordefinierten Liste von Elementen übereinstimmen. Jedes Element auf der Liste kann eine Liste mit Synonymen enthalten. Für die Personalwesen-App kann ein Mitarbeiter anhand verschiedener Schlüsselinformationen, wie Name, E-Mail, Telefonnummer und Steuernummer identifiziert werden. 

Die Personalwesen-App muss bestimmen, welcher Mitarbeiter von einem Gebäude in ein anderes Gebäude verlegt wird. Für eine Äußerung über eine Mitarbeiterverlegung bestimmt LUIS die Absicht und extrahiert den Mitarbeiter, damit von der Clientanwendung ein Standardauftrag zum Verlegen des Mitarbeiters erstellt werden kann.

Diese App verwendet eine Listenentität, um den Mitarbeiter zu extrahieren. Auf den Mitarbeiter kann über den Namen, die Telefondurchwahl, die Mobiltelefonnummer, die E-Mail-Adresse oder die Steuernummer Bezug genommen werden. 

Eine Listenentität ist eine gute Wahl für diese Art von Daten, wenn Folgendes gilt:

* Bei den Datenwerten handelt es sich um einen bekannten Satz.
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine exakte Übereinstimmung mit einem Synonym oder dem kanonischen Namen. 

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen einer MoveEmployee-Absicht
> * Hinzufügen einer Entität vom Typ „Liste“ 
> * Trainieren 
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-regex-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `list`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind. 


## <a name="moveemployee-intent"></a>MoveEmployee-Absicht

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

3. Geben Sie im Popupdialogfeld die Zeichenfolge `MoveEmployee` ein, und klicken Sie anschließend auf **Fertig**. 

    ![Screenshot: Dialogfenster „Create new intent“ (Neue Absicht erstellen)](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |move John W. Smith from B-1234 to H-4452 (John W. Smith von B-1234 nach H-4452 verschieben)|
    |mv john.w.smith@mycompany.com from office b-1234 to office h-4452 (Verschieben von „john.w.smith@mycompany.com“ von Büro b-1234 in Büro h-4452)|
    |shift x12345 to h-1234 tomorrow (x12345 morgen nach h-1234 verlagern)|
    |place 425-555-1212 in HH-2345 (425-555-1212 in HH-2345 anordnen)|
    |move 123-45-6789 from A-4321 to J-23456 (123-45-6789 von A-4321 nach J-23456 verschieben)|
    |mv Jill Jones from D-2345 to J-23456 (Verschieben von Jill Jones von D-2345 nach J-23456)|
    |shift jill-jones@mycompany.com to M-12345 („jill-jones@mycompany.com“ nach M-12345 verlagern)|
    |x23456 to M-12345 (x23456 nach M-12345)|
    |425-555-0000 to h-4452 (425-555-0000 nach h-4452)|
    |234-56-7891 to hh-2345 (234-56-7891 nach hh-2345)|

    [ ![Screenshot: Seite „Intents“ (Absichten) mit hervorgehobenen neuen Äußerungen](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

    Beachten Sie, dass „Zahl“ und „datetimeV2“ in einem vorhergehenden Tutorial hinzugefügt wurden und automatisch bezeichnet werden, wenn sie in Beispieläußerungen gefunden werden.

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="employee-list-entity"></a>Mitarbeiter-Listenentität
Nachdem die Absicht **MoveEmployee** jetzt über Beispieläußerungen verfügt, muss LUIS verstehen, was ein Mitarbeiter ist. 

Der primäre, _kanonische_ Name für die einzelnen Elemente ist die Mitarbeiternummer. Für diese Domäne lauten Beispiele für die Synonyme der einzelnen kanonischen Namen wie folgt: 

|Zweck des Synonyms|Wert des Synonyms|
|--|--|
|NAME|John W. Smith|
|E-Mail-Adresse|john.w.smith@mycompany.com|
|Durchwahl|x12345|
|Persönliche Mobiltelefonnummer|425-555-1212|
|US-Sozialversicherungsnummer|123-45-6789|


1. Klicken Sie im linken Bereich auf **Entitäten**.

2. Wählen Sie **Neue Entität erstellen** aus.

3. Geben Sie im Popupdialogfeld `Employee` als Entitätsname und **List** (Liste) als Entitätstyp ein. Wählen Sie **Fertig**aus.  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Screenshot: Popupdialogfeld zur Erstellung einer neuen Entität")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Geben Sie auf der Seite mit der Mitarbeiterentität `Employee-24612` als neuen Wert ein.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Screenshot: Eingeben des Werts")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Fügen Sie unter „Synonyme“ die folgenden Werte hinzu:

    |Zweck des Synonyms|Wert des Synonyms|
    |--|--|
    |NAME|John W. Smith|
    |E-Mail-Adresse|john.w.smith@mycompany.com|
    |Durchwahl|x12345|
    |Persönliche Mobiltelefonnummer|425-555-1212|
    |US-Sozialversicherungsnummer|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Screenshot: Eingeben von Synonymen")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Geben Sie `Employee-45612` als neuen Wert ein.

7. Fügen Sie unter „Synonyme“ die folgenden Werte hinzu:

    |Zweck des Synonyms|Wert des Synonyms|
    |--|--|
    |NAME|Jill Jones|
    |E-Mail-Adresse|jill-jones@mycompany.com|
    |Durchwahl|x23456|
    |Persönliche Mobiltelefonnummer|425-555-0000|
    |US-Sozialversicherungsnummer|234-56-7891|

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Geben Sie in der Adressleiste am Ende der URL `shift 123-45-6789 from Z-1242 to T-54672` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage („**q**uery“) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test, und es sollte die Absicht `MoveEmployee` mit Extrahierung von `Employee` zurückgegeben werden.

  ```JSON
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  Der Mitarbeiter wurde gefunden und mit dem Typ `Employee` mit dem Auflösungswert `Employee-24612` zurückgegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden eine neue Absicht erstellt, Beispieläußerungen hinzugefügt und dann eine Listenentität erstellt, um exakte Textübereinstimmungen aus Äußerungen zu extrahieren. Nach dem Trainieren und Veröffentlichen der App hat eine Abfrage des Endpunkts die Absicht identifiziert und die extrahierten Daten zurückgegeben.

> [!div class="nextstepaction"]
> [Hinzufügen einer hierarchischen Entität](luis-quickstart-intent-and-hier-entity.md)


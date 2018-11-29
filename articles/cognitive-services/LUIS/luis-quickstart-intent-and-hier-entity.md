---
title: 'Tutorial 5: Überordnungs-/Unterordnungsbeziehungen – Hierarchische LUIS-Entität für im Kontextbezug gelernte Daten'
titleSuffix: Azure Cognitive Services
description: Kontextbasiertes Finden aufeinander bezogener Teildaten. Beispielsweise sind der Ausgangsort und der Zielort für eine physische Verlegung aus einem Gebäude und Büro in ein anderes Gebäude und Büro aufeinander bezogen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d3b8d0597f0732a4a3cfab79125a885b2d141c9f
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424699"
---
# <a name="tutorial-5-extract-contextually-related-data"></a>Tutorial 5: Extrahieren kontextbezogener Daten
In diesem Tutorial wird das kontextbasierte Finden aufeinander bezogener Teildaten behandelt. Beispielsweise sind der Ausgangsort und der Zielort für eine physische Verlegung aus einem Gebäude und Büro in ein anderes Gebäude und Büro aufeinander bezogen. Zum Erstellen eines Arbeitsauftrags sind möglicherweise beide Teilinformationen erforderlich, und sie sind aufeinander bezogen.  

Mit dieser App wird ermittelt, wie ein Mitarbeiter vom Ursprungsort (Gebäude und Büro) an den Zielort (Gebäude und Büro) verschoben wird. Hierbei wird die hierarchische Entität verwendet, um die Orte in der Äußerung zu ermitteln. Der Zweck der Entität **hierarchical** (Hierarchisch) besteht darin, in der Äußerung anhand des Kontexts nach zusammengehörenden Daten zu suchen. 

Die hierarchische Entität ist eine gute Wahl für diese Art von Daten, da für die beiden Datentypen Folgendes gilt:

* Es sind einfache Entitäten.
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter gewählt, um die einzelnen Orte zu nennen. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Beide Standorte sind häufig Teil derselben Äußerung. 
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen einer Absicht 
> * Hinzufügen einer hierarchischen Standortentität mit Unterelementen zu Abflug- und Zielort
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-list-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `hier`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Entfernen der vordefinierten Zahlenentität aus der App
Entfernen Sie die vordefinierte Zahlenentität vorübergehend, um die gesamte Äußerung anzuzeigen und die untergeordneten Elemente der Hierarchie zu kennzeichnen.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie im linken Menü die Option **Entitäten**.

3. Aktivieren Sie das Kontrollkästchen links neben der Zahlenentität in der Liste. Klicken Sie auf **Löschen**. 

## <a name="add-utterances-to-moveemployee-intent"></a>Hinzufügen von Äußerungen zur MoveEmployee-Absicht

1. Wählen Sie im linken Menü die Option **Intents** (Absichten) aus.

2. Wählen Sie in der Liste mit den Absichten die Option **MoveEmployee**.

3. Fügen Sie die folgenden Beispieläußerungen hinzu:

    |Beispiele für Äußerungen|
    |--|
    |Move John W. Smith **to** a-2345 (John W. Smith „nach“ a-2345 verschieben)|
    |Direct Jill Jones **to** b-3499 (Jill Jones „nach“ b-3499 leiten)|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234 (Verschiebung von x23456 „von“ hh-2345 „nach“ e-0234 organisieren)|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567 (Mit Bearbeitung der Unterlagen für x12345 zum „Verlassen“ von a-3459 mit dem „Ziel“ f-34567 beginnen)|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345 (Verschieben von 425-555-0000 „von“ g-2323 „nach“ hh-2345)|

    [![Screenshot: LUIS mit neuen Äußerungen in der MoveEmployee-Absicht](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    Im Tutorial zur [Listenentität](luis-quickstart-intent-and-list-entity.md) wird ein Mitarbeiter anhand von Name, E-Mail-Adresse, Durchwahl, Mobiltelefonnummer oder US-Sozialversicherungsnummer bezeichnet. Diese Mitarbeiternummern werden in den Äußerungen verwendet. Die obigen Beispieläußerungen enthalten unterschiedliche Möglichkeiten zum Notieren der Ursprungs- und Zielorte (fett markiert). Zwei Äußerungen verfügen absichtlich nur über ein Ziel. So kann LUIS besser verstehen, wo diese Orte in der Äußerung angeordnet sind, wenn der Ursprungsort nicht enthalten ist.     

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="create-a-location-entity"></a>Erstellen einer Entität für den Ort
LUIS muss verstehen, was ein Ort ist, indem der Ursprung und das Ziel in den Äußerungen gekennzeichnet werden. Wenn Sie die Äußerung in der Tokenansicht (unformatiert) anzeigen möchten, können Sie den Umschalter in der Liste oberhalb der Äußerungen mit der Bezeichnung **Entities View** (Entitätsansicht) wählen. Nach dem Umschalten hat das Steuerelement die Bezeichnung **Tokens View** (Tokenansicht).

Wir verwenden als Beispiel die folgende Äußerung:

```JSON
mv Jill Jones from a-2349 to b-1298
```

In der Äußerung werden zwei Orte angegeben: `a-2349` und `b-1298`. Gehen Sie davon aus, dass der Buchstabe für einen Gebäudenamen und die Zahl für ein Büro in diesem Gebäude steht. Es ist sinnvoll, dass beide Elemente als untergeordnete Elemente einer hierarchischen Entität (`Locations`) gruppiert sind, da beide Datenelemente aus der Äußerung extrahiert werden müssen, um die Anforderung in der Clientanwendung abzuschließen, und miteinander verwandt sind. 
 
Wenn nur ein untergeordnetes Element (Abflug oder Ziel) einer hierarchischen Entität vorhanden ist, wird es trotzdem extrahiert. Es müssen nicht alle untergeordneten Elemente gefunden werden, um nur ein bzw. einige Elemente extrahieren zu können. 

1. Wählen Sie in der Äußerung `Displace 425-555-0000 away from g-2323 toward hh-2345` das Wort `g-2323` aus. Ein Dropdownmenü mit einem darüber angeordneten Textfeld wird angezeigt. Geben Sie den Entitätsnamen `Locations` in das Textfeld ein, und wählen Sie dann im Dropdownmenü die Option **Create new entity** (Neue Entität erstellen). 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png "Screenshot: Erstellung einer neuen Entität auf der Seite mit der Absicht")](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png#lightbox)

2. Wählen Sie im Popupfenster den Entitätstyp **Hierarchical** (Hierarchisch) mit `Origin` und `Destination` als untergeordnete Elemente aus. Wählen Sie **Fertig**aus.

    ![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Screenshot: Popupdialogfeld zur Entitätserstellung für die neue Entität „Location“ (Standort)")

3. Die Bezeichnung für `g-2323` ist als `Locations` gekennzeichnet, da LUIS nicht weiß, ob es sich hierbei um den Abflugort, Zielort oder einen anderen Ort handelt. Wählen Sie `g-2323` und dann **Locations**. Navigieren Sie im Menü nach rechts, und wählen Sie `Origin`.

    [![](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png "Screenshot: Popupdialogfeld zur Entitätskennzeichnung, um das untergeordnete Element der Entität „Locations“ zu ändern")](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png#lightbox)

5. Kennzeichnen Sie die anderen Orte in allen anderen Äußerungen, indem Sie das Gebäude und das Büro in der Äußerung auswählen, „Locations“ wählen und dann im Menü nach rechts navigieren, um `Origin` oder `Destination` zu wählen. Nachdem alle Orte gekennzeichnet wurden, wird für die Äußerungen unter **Tokens View** (Tokenansicht) ein Muster deutlich. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png "Screenshot: In Äußerungen gekennzeichnete Entität „Locations“")](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png#lightbox)

## <a name="add-prebuilt-number-entity-to-app"></a>Hinzufügen einer vordefinierten Zahlenentität zu einer App
Fügen Sie die vordefinierte Zahlenentität wieder der Anwendung hinzu.

1. Wählen Sie im linken Navigationsmenü die Option **Entities** (Entitäten) aus.

2. Wählen Sie die Schaltfläche **Add prebuilt entity** (Vordefinierte Entität hinzufügen).

3. Wählen Sie in der Liste mit den vordefinierten Entitäten die Option **number** und dann **Fertig**.

    ![Screenshot: Auswahl von „number“ im Dialogfeld mit den vordefinierten Entitäten](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


2. Geben Sie in der Adressleiste am Ende der URL `Please relocation jill-jones@mycompany.com from x-2345 to g-23456` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `MoveEmployee` mit der extrahierten hierarchischen Entität zurückgeben.

    ```JSON
    {
      "query": "Please relocation jill-jones@mycompany.com from x-2345 to g-23456",
      "topScoringIntent": {
        "intent": "MoveEmployee",
        "score": 0.9966052
      },
      "intents": [
        {
          "intent": "MoveEmployee",
          "score": 0.9966052
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.0325253047
        },
        {
          "intent": "FindForm",
          "score": 0.006137873
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00462633232
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00415637763
        },
        {
          "intent": "ApplyForJob",
          "score": 0.00382325822
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00249120337
        },
        {
          "intent": "None",
          "score": 0.00130756292
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00119622645
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.26910036E-05
        }
      ],
      "entities": [
        {
          "entity": "jill - jones @ mycompany . com",
          "type": "Employee",
          "startIndex": 18,
          "endIndex": 41,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "x - 2345",
          "type": "Locations::Origin",
          "startIndex": 48,
          "endIndex": 53,
          "score": 0.8520272
        },
        {
          "entity": "g - 23456",
          "type": "Locations::Destination",
          "startIndex": 58,
          "endIndex": 64,
          "score": 0.974032
        },
        {
          "entity": "-2345",
          "type": "builtin.number",
          "startIndex": 49,
          "endIndex": 53,
          "resolution": {
            "value": "-2345"
          }
        },
        {
          "entity": "-23456",
          "type": "builtin.number",
          "startIndex": 59,
          "endIndex": 64,
          "resolution": {
            "value": "-23456"
          }
        }
      ]
    }
    ```
    
    Die richtige Absicht wird vorhergesagt, und das Entitätenarray weist sowohl den Ursprungs- als auch den Zielwert in der entsprechenden **Entitätseigenschaft** auf.
    

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Kann auch ein regulärer Ausdruck für jeden Ort verwendet werden?
Ja. Erstellen Sie die reguläre Ausdrucksentität mit Ursprungs- und Zielrolle, und verwenden Sie sie in einem Muster.

Für die Orte in diesem Beispiel, z.B. `a-1234`, wird ein bestimmtes Format mit einem oder mehreren Buchstaben, einem Bindestrich und dann vier oder fünf Zahlen verwendet. Diese Daten können als Entität vom Typ „Regulärer Ausdruck“ mit einer Rolle für jeden Ort beschrieben werden. Rollen sind nur für Muster verfügbar. Sie können basierend auf diesen Äußerungen Muster erstellen und dann einen regulären Ausdruck für das Ortsformat erstellen und ihn den Mustern hinzufügen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="hierarchical-entities-versus-roles"></a>Hierarchische Entitäten im Vergleich zu Rollen

Weitere Informationen finden Sie unter [Rollen und hierarchische Entitäten](luis-concept-roles.md#roles-versus-hierarchical-entities).

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden eine neue Absicht erstellt und Beispieläußerungen für die im Kontext gelernten Daten von Ursprungs- und Zielort hinzugefügt. Nachdem die App trainiert und veröffentlicht ist, kann eine Clientanwendung diese Informationen für einen Verlegungsauftrag mit den relevanten Informationen verwenden.

> [!div class="nextstepaction"] 
> [Hinzufügen einer zusammengesetzten Entität](luis-tutorial-composite-entity.md) 

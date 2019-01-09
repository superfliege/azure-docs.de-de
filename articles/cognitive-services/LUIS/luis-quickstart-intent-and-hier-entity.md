---
title: Entität vom Typ „Hierarchical“
titleSuffix: Azure Cognitive Services
description: Kontextbasiertes Finden aufeinander bezogener Teildaten. Beispielsweise sind der Ausgangsort und der Zielort für eine physische Verlegung aus einem Gebäude und Büro in ein anderes Gebäude und Büro aufeinander bezogen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 91a9646e88adbfaf6d3c3fc0b06b341c647e773f
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753692"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extrahieren von Daten mit ähnlichem Kontext aus einer Äußerung

In diesem Tutorial wird das kontextbasierte Finden aufeinander bezogener Teildaten behandelt. Ein Beispiel wären etwa Ursprungs- und Zielorte bei der Versetzung in eine andere Stadt. Beide Teilinformationen können erforderlich sein und stehen miteinander in Verbindung.  

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen einer neuen App
> * Hinzufügen einer Absicht 
> * Hinzufügen einer hierarchischen Standortentität mit Unterelementen zu Abflug- und Zielort
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="hierarchical-data"></a>Hierarchische Daten

Diese App erkennt, ob ein Mitarbeiter vom Ursprungsort an den Zielort versetzt werden soll. Hierbei wird die hierarchische Entität verwendet, um die Orte in der Äußerung zu ermitteln. 

Die hierarchische Entität ist eine gute Wahl für diese Art von Daten, da für die beiden Datentypen (also die untergeordneten Orte) Folgendes gilt:

* Es sind einfache Entitäten.
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter verwendet, um die einzelnen Entitäten anzugeben. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Beide untergeordnete Elemente sind häufig Teil der gleichen Äußerung. 
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

## <a name="create-a-new-app"></a>Erstellen einer neuen App

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Erstellen einer Absicht zur Versetzung von Mitarbeitern an einen anderen Ort

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

1. Geben Sie im Popupdialogfeld die Zeichenfolge `MoveEmployeeToCity` ein, und klicken Sie anschließend auf **Fertig**. 

    ![Screenshot: Dialogfenster „Create new intent“ (Neue Absicht erstellen)](./media/luis-quickstart-intent-and-hier-entity/create-new-intent-move-employee-to-city.png)

1. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |„move John W. Smith leaving Seattle headed to Dallas“ (John W. Smith von Seattle nach Dallas versetzen)|
    |„transfer Jill Jones from Seattle to Cairo“ (Jill Jones von Seattle nach Kairo transferieren)|
    |„Place John Jackson away from Tampa, coming to Atlanta“ (John Jackson von Tampa abziehen und nach Atlanta holen) |
    |„move Debra Doughtery to Tulsa from Dallas“ (Debra Doughtery von Dallas nach Tulsa versetzen)|
    |„mv Jill Jones leaving Cairo headed to Tampa“ (Jill Jones von Kairo nach Tampa holen)|
    |„Shift Alice Anderson to Oakland from Redmond“ (Alice Anderson von Redmond nach Oakland versetzen)|
    |„Carl Chamerlin from San Francisco to Redmond“ (Carl Chamerlin von San Francisco nach Redmond)|
    |„Transfer Steve Standish from San Diego toward Bellevue“ (Steve Standish von San Diego abziehen und nach Bellevue versetzen) |
    |„lift Tanner Thompson from Kansas city and shift to Chicago“ (Tanner Thompson von Kansas City nach Chicago versetzen)|

    [![Screenshot: LUIS mit neuen Äußerungen in der MoveEmployee-Absicht](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

## <a name="create-a-location-entity"></a>Erstellen einer Entität für den Ort
LUIS muss verstehen, was ein Ort ist, indem der Ursprung und das Ziel in den Äußerungen gekennzeichnet werden. Wenn Sie die Äußerung in der Tokenansicht (unformatiert) anzeigen möchten, können Sie den Umschalter in der Liste oberhalb der Äußerungen mit der Bezeichnung **Entities View** (Entitätsansicht) wählen. Nach dem Umschalten hat das Steuerelement die Bezeichnung **Tokens View** (Tokenansicht).

Wir verwenden als Beispiel die folgende Äußerung:

```json
move John W. Smith leaving Seattle headed to Dallas
```

In der Äußerung werden zwei Orte angegeben: `Seattle` und `Dallas`. Beide Elemente werden als untergeordnete Elemente einer hierarchischen Entität (`Location`) gruppiert, da beide Datenelemente aus der Äußerung extrahiert werden müssen, um die Anforderung in der Clientanwendung abzuschließen, und miteinander in Verbindung stehen. 
 
Wenn nur ein untergeordnetes Element (Abflug oder Ziel) einer hierarchischen Entität vorhanden ist, wird es trotzdem extrahiert. Es müssen nicht alle untergeordneten Elemente gefunden werden, um nur ein bzw. einige Elemente extrahieren zu können. 

1. Wählen Sie in der Äußerung `move John W. Smith leaving Seattle headed to Dallas` das Wort `Seattle` aus. Ein Dropdownmenü mit einem darüber angeordneten Textfeld wird angezeigt. Geben Sie den Entitätsnamen `Location` in das Textfeld ein, und wählen Sie dann im Dropdownmenü die Option **Create new entity** (Neue Entität erstellen). 

    [![Screenshot: Erstellen einer neuen Entität auf der Absichtsseite](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png "Screenshot: Erstellen einer neuen Entität auf der Absichtsseite")](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png#lightbox)

1. Wählen Sie im Popupfenster den Entitätstyp **Hierarchical** (Hierarchisch) mit `Origin` und `Destination` als untergeordnete Elemente aus. Wählen Sie **Fertig**aus.

    ![Screenshot: Popupdialogfeld zur Entitätserstellung für die neue Entität „Location“ (Standort)](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Screenshot: Popupdialogfeld zur Entitätserstellung für die neue Entität „Location“ (Standort)")

1. Die Bezeichnung für `Seattle` ist als `Location` gekennzeichnet, da LUIS nicht weiß, ob es sich hierbei um den Abflugort, Zielort oder einen anderen Ort handelt. Wählen Sie `Seattle` und dann **Location** (Standort) aus. Navigieren Sie im Menü nach rechts, und wählen Sie anschließend `Origin` aus.

    [![Screenshot: Popupdialogfeld zur Entitätskennzeichnung, um das untergeordnete Element der Entität „Locations“ (Standorte) zu ändern](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png "Screenshot: Popupdialogfeld zur Entitätskennzeichnung, um das untergeordnete Element der Entität „Locations“ (Standorte) zu ändern")](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png#lightbox)

1. Versehen Sie die anderen Orte in allen anderen Äußerungen jeweils mit einer Bezeichnung. Nachdem alle Orte markiert wurden, wird für die Äußerungen ein Muster erkennbar. 

    [![Screenshot: In Äußerungen gekennzeichnete Entität „Locations“ (Standorte)](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png "Screenshot: In Äußerungen gekennzeichnete Entität „Locations“ (Standorte)")](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png#lightbox)

    Die rote Unterstreichung gibt an, dass LUIS bei der Entität nicht sicher ist. Dieses Problem lässt sich durch Trainieren lösen. 

## <a name="add-example-utterances-to-the-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“ 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Trainieren der App, um die Absichtsänderungen testen zu können 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Veröffentlichen der App, damit das trainierte Modell über den Endpunkt abgefragt werden kann

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Geben Sie in der Adressleiste am Ende der URL `Please move Carl Chamerlin from Tampa to Portland` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `MoveEmployee` mit der extrahierten hierarchischen Entität zurückgeben.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "portland",
          "type": "Location::Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "tampa",
          "type": "Location::Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    Die richtige Absicht wird vorhergesagt, und das Entitätenarray weist sowohl den Ursprungs- als auch den Zielwert in der entsprechenden Eigenschaft vom Typ **entities** auf.
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Informationen zum Konzept der hierarchischen Entität](luis-concept-entity-types.md)
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)
* [Vergleich von Rollen und hierarchischen Entitäten](luis-concept-roles.md#roles-versus-hierarchical-entities)
* [Informationen zum Verbessern von Vorhersagen mit Mustern](luis-concept-patterns.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden eine neue Absicht erstellt und Beispieläußerungen für die im Kontext gelernten Daten von Ursprungs- und Zielort hinzugefügt. Nachdem die App trainiert und veröffentlicht ist, kann eine Clientanwendung diese Informationen für einen Verlegungsauftrag mit den relevanten Informationen verwenden.

> [!div class="nextstepaction"] 
> [Hinzufügen einer zusammengesetzten Entität](luis-tutorial-composite-entity.md) 

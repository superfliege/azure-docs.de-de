---
title: 'Tutorial: Erstellen einer LUIS-App zum Abrufen von Standortdaten – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache LUIS-App erstellen, in der Absichten und eine hierarchische Entität zum Extrahieren von Daten verwendet werden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/04/2018
ms.author: v-geberr
ms.openlocfilehash: babfc2f82e17f3745af1d940df89763170a002bd
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929585"
---
# <a name="tutorial-5-add-hierarchical-entity"></a>Tutorial: 5. Hinzufügen einer Entität vom Typ „Hierarchisch“
In diesem Tutorial erstellen Sie eine App, mit der veranschaulicht wird, wie Sie anhand des Kontexts nach zusammengehörenden Datenelementen suchen. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu hierarchischen Entitäten und aus dem Kontext erschlossenen Unterelementen 
> * Verwenden der LUIS-App im Personalbereich 
> * Hinzufügen einer hierarchischen Standortentität mit Unterelementen zu Abflug- und Zielort
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort, einschließlich hierarchischer untergeordneter Elemente 

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](luis-reference-regions.md#luis-website)-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem [Tutorial zu Listenentitäten](luis-quickstart-intent-and-list-entity.md) verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-list-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `hier`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="purpose-of-the-app-with-this-entity"></a>Zweck der App mit dieser Entität
Mit dieser App wird ermittelt, wie ein Mitarbeiter vom Ursprungsort (Gebäude und Büro) an den Zielort (Gebäude und Büro) verschoben wird. Hierbei wird die hierarchische Entität verwendet, um die Orte in der Äußerung zu ermitteln. 

Die hierarchische Entität ist eine gute Wahl für diese Art von Daten, da für die beiden Datentypen Folgendes gilt:

* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter gewählt, um die einzelnen Orte zu nennen. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Beide Standorte sind häufig Teil derselben Äußerung. 

Der Zweck der Entität **hierarchical** (Hierarchisch) besteht darin, in der Äußerung anhand des Kontexts nach zusammengehörenden Daten zu suchen. Wir verwenden als Beispiel die folgende Äußerung:

```JSON
mv Jill Jones from a-2349 to b-1298
```
In der Äußerung werden zwei Orte angegeben: `a-2349` und `b-1298`. Gehen Sie davon aus, dass der Buchstabe für einen Gebäudenamen und die Zahl für ein Büro in diesem Gebäude steht. Es ist sinnvoll, dass beide Elemente als untergeordnete Elemente einer hierarchischen Entität (`Locations`) gruppiert sind, da beide Datenelemente aus der Äußerung extrahiert werden müssen und miteinander verwandt sind. 
 
Wenn nur ein untergeordnetes Element (Abflug oder Ziel) einer hierarchischen Entität vorhanden ist, wird es trotzdem extrahiert. Es müssen nicht alle untergeordneten Elemente gefunden werden, um nur ein bzw. einige Elemente extrahieren zu können. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Entfernen der vordefinierten Zahlenentität aus der App
Entfernen Sie die vordefinierte Zahlenentität vorübergehend, um die gesamte Äußerung anzuzeigen und die untergeordneten Elemente der Hierarchie zu kennzeichnen.

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste auf **Build** klicken. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png#lightbox)

2. Wählen Sie im linken Menü die Option **Entitäten**.

    [![Screenshot: LUIS-App mit Hervorhebung der Schaltfläche „Entities“ (Entitäten) im linken Menü](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png#lightbox)


3. Wählen Sie die Auslassungspunkte (***...***) rechts von der Zahlenentität in der Liste aus. Klicken Sie auf **Löschen**. 

    [ ![Screenshot: LUIS-App auf der Seite mit den Entitäten mit Hervorhebung der Löschschaltfläche für die vordefinierte Zahlenentität](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png)](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png#lightbox)


## <a name="add-utterances-to-moveemployee-intent"></a>Hinzufügen von Äußerungen zur MoveEmployee-Absicht

1. Wählen Sie im linken Menü die Option **Intents** (Absichten) aus.

    [ ![Screenshot: LUIS-App mit Hervorhebung von „Intents“ (Absichten) im linken Menü](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png#lightbox)

2. Wählen Sie in der Liste mit den Absichten die Option **MoveEmployee**.

    [![Screenshot: LUIS-App mit Hervorhebung der MoveEmployee-Absicht im linken Menü](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png)](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png#lightbox)

3. Fügen Sie die folgenden Beispieläußerungen hinzu:

    |Beispiele für Äußerungen|
    |--|
    |Move John W. Smith **to** a-2345 (John W. Smith „nach“ a-2345 verschieben)|
    |Direct Jill Jones **to** b-3499 (Jill Jones „nach“ b-3499 leiten)|
    |Organize the move of x23456 **from** hh-2345 **to** e-0234 (Verschiebung von x23456 „von“ hh-2345 „nach“ e-0234 organisieren)|
    |Begin paperwork to set x12345 **leaving** a-3459 **headed to** f-34567 (Mit Bearbeitung der Unterlagen für x12345 zum „Verlassen“ von a-3459 mit dem „Ziel“ f-34567 beginnen)|
    |Displace 425-555-0000 **away from** g-2323 **toward** hh-2345 (Verschieben von 425-555-0000 „von“ g-2323 „nach“ hh-2345)|

    Im Tutorial zur [Listenentität](luis-quickstart-intent-and-list-entity.md) kann ein Mitarbeiter anhand von Name, E-Mail-Adresse, Durchwahl, Mobiltelefonnummer oder US-Sozialversicherungsnummer bezeichnet werden. Diese Mitarbeiternummern werden in den Äußerungen verwendet. Die obigen Beispieläußerungen enthalten unterschiedliche Möglichkeiten zum Notieren der Ursprungs- und Zielorte (fett markiert). Zwei Äußerungen verfügen absichtlich nur über ein Ziel. So kann LUIS besser verstehen, wo diese Orte in der Äußerung angeordnet sind, wenn der Ursprungsort nicht enthalten ist.

    [![Screenshot: LUIS mit neuen Äußerungen in der MoveEmployee-Absicht](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)
     

## <a name="create-a-location-entity"></a>Erstellen einer Entität für den Ort
LUIS muss verstehen, was ein Ort ist, indem der Ursprung und das Ziel in den Äußerungen gekennzeichnet werden. Wenn Sie die Äußerung in der Tokenansicht (unformatiert) anzeigen möchten, können Sie den Umschalter in der Liste oberhalb der Äußerungen mit der Bezeichnung **Entities View** (Entitätsansicht) wählen. Nach dem Umschalten hat das Steuerelement die Bezeichnung **Tokens View** (Tokenansicht).

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

1. Wählen Sie im linken Navigationsmenü die Option **Entities** (Entitäten).

    [![Screenshot: Hervorhebung der Schaltfläche „Entitäten“ im linken Navigationsmenü](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png#lightbox)

2. Klicken Sie auf die Schaltfläche **Manage prebuilt entities** (Vordefinierte Entitäten verwalten).

    [![Screenshot: Liste mit Entitäten mit hervorgehobener Option „Manage prebuilt entities“ (Vordefinierte Entitäten verwalten)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png#lightbox)

3. Wählen Sie in der Liste mit den vordefinierten Entitäten die Option **number** und dann **Fertig**.

    ![Screenshot: Auswahl von „number“ im Dialogfeld mit den vordefinierten Entitäten](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS ist erst dann über die Änderungen an den Absichten und Entitäten (Modell) informiert, nachdem der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Train** (Trainieren).

    ![Trainieren der App](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Training erfolgreich](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Veröffentlichen**. 

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Veröffentlichen**.

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Screenshot: Seite „Publish“ (Veröffentlichen) mit hervorgehobener Endpunkt-URL")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

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

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Kann auch ein regulärer Ausdruck für jeden Ort verwendet werden?
Ja. Erstellen Sie den regulären Ausdruck mit Ursprungs- und Zielrolle, und verwenden Sie ihn in einem Muster.

Für die Orte in diesem Beispiel, z.B. `a-1234`, wird ein bestimmtes Format mit einem oder mehreren Buchstaben, einem Bindestrich und dann vier oder fünf Zahlen verwendet. Diese Daten können als Entität vom Typ „Regulärer Ausdruck“ mit einer Rolle für jeden Ort beschrieben werden. Rollen sind für Muster verfügbar. Sie können basierend auf diesen Äußerungen Muster erstellen und dann einen regulären Ausdruck für das Ortsformat erstellen und ihn den Mustern hinzufügen. <!-- Go to this tutorial to see how that is done -->

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit einigen Absichten und einer hierarchischen Entität hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten zurückgegeben. 

Ihr Chatbot verfügt jetzt über genügend Informationen, um die Hauptaktion `MoveEmployee` und die in der Äußerung enthaltenen Informationen zum Standort zu ermitteln. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (z.B. ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie hierzu in der App-Liste rechts vom App-Namen die Auslassungspunkte (***...***) und anschließend die Option **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Hinzufügen einer zusammengesetzten Entität](luis-tutorial-composite-entity.md) 
---
title: 'Tutorial: Erstellen einer LUIS-App zum Abrufen von Textdaten, die genau mit Daten in einer Liste übereinstimmen – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird im Rahmen einer Schnellstartanleitung beschrieben, wie Sie eine einfache LUIS-App mit Absichten und Listenentitäten erstellen, um Daten zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: c5408d20a736f262e95ce7014c385b50521967ad
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127854"
---
# <a name="tutorial-4-add-list-entity"></a>Tutorial: 4. Hinzufügen einer Entität vom Typ „Liste“
In diesem Tutorial erstellen Sie eine App, mit der veranschaulicht wird, wie Sie Daten abrufen, die mit Angaben in einer vordefinierten Liste übereinstimmen. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Verstehen von Listenentitäten 
> * Erstellen einer neuen LUIS-App für den Personalbereich mit MoveEmployee-Absicht
> * Hinzufügen einer Listenentität zum Extrahieren des Mitarbeiters aus einer Äußerung
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

Für diesen Artikel benötigen Sie ein kostenloses [LUIS](luis-reference-regions.md#luis-website)-Konto, um Ihre LUIS-Anwendung erstellen zu können.

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem Tutorial zur [RegEx](luis-quickstart-intents-regex-entity.md)-Entität verfügen, [importieren](create-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-regex-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `list`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="purpose-of-the-list-entity"></a>Zweck der Listenentität
Mit dieser App werden Äußerungen zur Verschiebung eines Mitarbeiters von einem Gebäude in ein anderes vorhergesagt. Diese App nutzt eine Listenentität zum Extrahieren eines Mitarbeiters. Auf den Mitarbeiter kann per Name, Telefonnummer, E-Mail-Adresse oder US-Sozialversicherungsnummer verwiesen werden. 

Eine Listenentität kann viele Elemente mit Synonymen für die einzelnen Elemente enthalten. Für kleine oder mittelgroße Unternehmen wird die Listenentität zum Extrahieren der Mitarbeiterinformationen genutzt. 

Der kanonische Name für die einzelnen Elemente ist die Mitarbeiternummer. Für diese Domäne lauten Beispiele für die Synonyme wie folgt: 

|Zweck des Synonyms|Wert des Synonyms|
|--|--|
|NAME|John W. Smith|
|E-Mail-Adresse|john.w.smith@mycompany.com|
|Durchwahl|x12345|
|Persönliche Mobiltelefonnummer|425-555-1212|
|US-Sozialversicherungsnummer|123-45-6789|

Eine Listenentität ist eine gute Wahl für diese Art von Daten, wenn Folgendes gilt:

* Bei den Datenwerten handelt es sich um einen bekannten Satz.
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine genaue Übereinstimmung mit einem Synonym. 

Bei LUIS wird der Mitarbeiter so extrahiert, dass von der Clientanwendung eine Standardreihenfolge zum Verschieben des Mitarbeiters erstellt werden kann.
<!--
## Example utterances
Simple example utterances for a `MoveEmployee` inent:

```
move John W. Smith from B-1234 to H-4452
mv john.w.smith@mycompany from office b-1234 to office h-4452

```
-->

## <a name="add-moveemployee-intent"></a>Hinzufügen einer MoveEmployee-Absicht

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste auf **Build** klicken. 

    [ ![Screenshot: LUIS-App mit hervorgehobener Build-Option (rechts oben auf der Navigationsleiste)](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png#lightbox)

2. Klicken Sie auf **Create new intent** (Neue Absicht erstellen). 

    [ ![Screenshot: Seite „Intents“ (Absichten) mit hervorgehobener Schaltfläche „Create new intent“ (Neue Absicht erstellen)](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png#lightbox)

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

## <a name="create-an-employee-list-entity"></a>Erstellen einer Mitarbeiterlistenentität
Nachdem die Absicht **MoveEmployee** jetzt über Äußerungen verfügt, muss LUIS verstehen, was ein Mitarbeiter ist. 

1. Klicken Sie im linken Bereich auf **Entitäten**.

    [ ![Screenshot: Seite „Intent“ (Absicht) mit Hervorhebung der Schaltfläche „Entities“ (Entitäten) im linken Navigationsbereich](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png#lightbox)

2. Klicken Sie auf **Neue Entität erstellen**.

    [![Screenshot: Seite „Entities“ (Entitäten) mit Hervorhebung von „Neue Entität erstellen“](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png#lightbox)

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

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS ist erst dann über die Änderungen an den Absichten und Entitäten (Modell) informiert, nachdem der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Train** (Trainieren).

    ![Trainieren der App](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Training erfolgreich](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Publish** (Veröffentlichen). 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Screenshot: Auswahl der Schaltfläche „Publish“ (Veröffentlichen)")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Publish** (Veröffentlichen). 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Screenshot: Auswahl von „Publish“ (Veröffentlichen) für den Produktionsslot")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Screenshot: Endpunkt-URL auf der Seite „Publish“ (Veröffentlichen)")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

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

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Wo wird für die Listenentität die Verarbeitung natürlicher Sprache durchgeführt? 
Da für die Listenentität eine genaue Textübereinstimmung erforderlich ist, wird keine Verarbeitung von natürlicher Sprache durchgeführt (und auch kein maschinelles Lernen). Die Verarbeitung natürlicher Sprache (bzw. maschinelles Lernen) wird für LUIS verwendet, um die richtige Absicht mit der höchsten Bewertung auszuwählen. Darüber hinaus kann eine Äußerung eine Mischung von mehr als einer Entität oder sogar mehr als einem Entitätstyp sein. Jede Äußerung wird für alle Entitäten der App verarbeitet, einschließlich der Entitäten, die auf der Verarbeitung natürlicher Sprache (oder maschinellem Lernen) basieren.

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Mit dieser App wurde basierend auf einer Listenentität der richtige Mitarbeiter extrahiert. 

Ihr Chatbot verfügt nun über genügend Informationen, um die primäre Aktion (`MoveEmployee`) und den Mitarbeiter für den Verschiebevorgang zu ermitteln. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (z.B. ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Wählen Sie im Menü oben links die Option **Meine Apps**. Klicken Sie in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer hierarchischen Entität](luis-quickstart-intent-and-hier-entity.md)


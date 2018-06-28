---
title: 'Tutorial: Erstellen einer LUIS-App zum Abrufen von Textdaten, die genau mit Daten in einer Liste übereinstimmen – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird im Rahmen einer Schnellstartanleitung beschrieben, wie Sie eine einfache LUIS-App mit Absichten und Listenentitäten erstellen, um Daten zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 33394dff1091f27c79c74d8648a90724ba8d6698
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264826"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Tutorial: Erstellen einer App mit einer Listenentität
In diesem Tutorial erstellen Sie eine App, mit der veranschaulicht wird, wie Sie Daten abrufen, die mit Angaben in einer vordefinierten Liste übereinstimmen. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Verstehen von Listenentitäten 
> * Erstellen einer neuen LUIS-App für den Getränkebereich mit OrderDrinks-Absicht
> * Hinzufügen der Absicht _None_ (Keine) und Hinzufügen von Beispielen für Äußerungen
> * Hinzufügen einer Listenentität zum Extrahieren von Getränkeelementen aus einer Äußerung
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="purpose-of-the-list-entity"></a>Zweck der Listenentität
Mit dieser App können Bestellungen für Getränke aufgenommen, z.B. `1 coke and 1 milk please`, und die entsprechenden Daten zurückgegeben werden, z.B. die Art des Getränks. Eine Entität vom Typ **list** (Liste) für Getränke sucht nach genauen Textübereinstimmungen und gibt diese zurück. 

Eine Listenentität ist eine gute Wahl für diese Art von Daten, wenn die Datenwerte bekannt sind. Die Namen von Getränken können variieren und auch umgangssprachliche Ausdrücke oder Abkürzungen sein, aber die Namen ändern sich nicht häufig. 

## <a name="app-intents"></a>App-Absichten
Die Absichten sind Kategorien für die Wünsche des Benutzers. Die App verfügt über zwei Absichten: „OrderDrink“ (Getränk bestellen) und „None“ (Keine). Die Absicht [None](luis-concept-intent.md#none-intent-is-fallback-for-app) (Keine) dient für alle Zwecke außerhalb der App.  

## <a name="list-entity-is-an-exact-text-match"></a>Listenentität ist eine genaue Textübereinstimmung
Der Zweck der Entität besteht darin, Teile des Texts einer Äußerung zu finden und zu kategorisieren. Mit der Entität [list](luis-concept-entity-types.md) (Liste) können Wörter oder Wortgruppen abgeglichen werden.  

Für diese Getränke-App wird die Getränkebestellung per LUIS so extrahiert, dass eine Standardbestellung erstellt und ausgefüllt werden kann. Bei LUIS können Äußerungen Varianten, Abkürzungen und umgangssprachliche Ausdrücke enthalten. 

Einfache Beispiele für Äußerungen von Benutzern sind:

```
2 glasses of milk
3 bottles of water
2 cokes
```

Hier sind einige abgekürzte oder umgangssprachliche Versionen von Äußerungen aufgeführt:

```
5 milk
3 h2o
1 pop
```
 
Die Listenentität ordnet `h2o` der Kategorie „Wasser“ und `pop` der Kategorie „Softdrink“ zu.  

## <a name="what-luis-does"></a>Vorgehensweise von LUIS
Nachdem die Absicht und die Entitäten der Äußerung identifiziert, [extrahiert](luis-concept-data-extraction.md#list-entity-data) und im JSON-Format vom [Endpunkt](https://aka.ms/luis-endpoint-apis) zurückgegeben wurden, ist der LUIS-Vorgang abgeschlossen. Die aufrufende Anwendung bzw. der Chatbot verwendet diese JSON-Antwort, um die Anforderung zu erfüllen – jeweils gemäß der Auslegung der App bzw. des Chatbots. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich an der [LUIS][LUIS]-Website an. Achten Sie darauf, sich unter der [Region][LUIS-regions] anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS][LUIS]-Website auf **Create new app** (Neue App erstellen).  

    ![Erstellen einer neuen App](./media/luis-quickstart-intent-and-list-entity/app-list.png)

3. Geben Sie im Popupdialogfenster den Namen `MyDrinklist` ein. 

    ![Name der MyDrinkList-App](./media/luis-quickstart-intent-and-list-entity/create-app-dialog.png)

4. Nach Abschluss dieses Vorgangs wird in der App die Seite **Intents** (Absichten) mit der Absicht **None** (Keine) angezeigt. 

    [![](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png "Screenshot: Seite mit den Absichten")](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Erstellen einer neuen Absicht

1. Wählen Sie auf der Seite **Absichten** die Option **Create new intent** (Neue Absicht erstellen). 

    [![](media/luis-quickstart-intent-and-list-entity/create-new-intent.png "Screenshot: Seite „Intents“ (Absichten) mit hervorgehobener Schaltfläche „Create new intent“ (Neue Absicht erstellen)")](media/luis-quickstart-intent-and-list-entity/create-new-intent.png#lightbox)

2. Geben Sie den Namen für die neue Absicht ein: `OrderDrinks`. Diese Absicht muss jedes Mal ausgewählt werden, wenn ein Benutzer ein Getränk bestellen möchte.

    Indem Sie eine Absicht erstellen, erstellen Sie auch die primäre Kategorie der zu ermittelnden Informationen. Durch die Benennung der Kategorie können andere Anwendungen, die die Ergebnisse der LUIS-Abfrage nutzen, anhand des Kategorienamens eine passende Antwort finden oder geeignete Aktionen ausführen. LUIS liefert keine Antwort auf die eigentliche Frage, sondern gibt lediglich an, welche Art von Information in natürlicher Sprache erfragt wird. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png "Screenshot: Erstellung der neuen Absicht „OrderDrinks“ (Getränke bestellen)")](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png#lightbox)

3. Fügen Sie der Absicht `OrderDrinks` mehrere voraussichtliche Äußerungen von Benutzern hinzu, z.B.:

    | Beispiele für Äußerungen|
    |--|
    |Please send 2 cokes and a bottle of water to my room (Bitte bringen Sie mir 2 Colas und eine Flasche Wasser auf mein Zimmer)|
    |2 perriers with a twist of lime (Zweimal Perrier mit Limone)|
    |h2o (H2O)|

    [![](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png "Screenshot: Eingabe der Äußerung auf der Seite für die Absicht „OrderDrinks“ (Getränke bestellen)")](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Hinzufügen von Äußerungen zur Absicht „None“ (Keine)

Die LUIS-App enthält derzeit keine Äußerungen für die Absicht **None** (Keine). Es sind Äußerungen erforderlich, die die App nicht beantworten soll. Daher müssen Äußerungen für die Absicht **None** (Keine) enthalten sein. Lassen Sie sie nicht leer. 

1. Wählen Sie im linken Bereich **Intents** (Absichten). 

    [![](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png "Screenshot: Auswahl des Links „Intents“ (Absichten) im linken Bereich")](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png#lightbox)

2. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, aber die für Ihre App nicht relevant sind:

    | Beispiele für Äußerungen|
    |--|
    |Cancel! (Abbrechen!)|
    |Good bye (Auf Wiedersehen)|
    |What is going on? (Was ist los?)|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Vorhersage der Äußerung für die Absicht „None“ (Keine)
In der Anwendung, die den LUIS-Dienst aufruft (z.B. ein Chatbot), kann der Bot den Benutzer fragen, ob er die Unterhaltung beenden möchte, wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt. Der Bot kann außerdem weitere Anweisungen zum Fortsetzen der Unterhaltung geben, falls der Benutzer sie nicht beenden möchte. 

Entitäten werden in der Absicht **None** (Keine) verwendet. Wenn die Absicht **None** die höchste Bewertung hat, aber eine Entität extrahiert wird, die für Ihren Chatbot aussagekräftig ist, kann Ihr Chatbot eine Frage zur Absicht des Kunden stellen. 

## <a name="create-a-menu-entity-from-the-intent-page"></a>Erstellen einer Menüentität über die Seite „Intent“ (Absicht)
Nachdem die beiden Absichten jetzt über Äußerungen verfügen, muss LUIS wissen, was ein Getränk ist. Navigieren Sie zurück zur Absicht `OrderDrinks`, und kennzeichnen Sie die in einer Äußerung enthaltenen Getränke mit den folgenden Schritten:

1. Kehren Sie zur Absicht `OrderDrinks` zurück, indem Sie im linken Bereich **Intents** (Absichten) wählen.

2. Wählen Sie `OrderDrinks` in der Liste mit den Absichten aus.

3. Wählen Sie in der Äußerung `Please send 2 cokes and a bottle of water to my room` das Wort `water` aus. Ein Dropdownmenü mit einem darüber angeordneten Textfeld zum Erstellen einer neuen Entität wird angezeigt. Geben Sie den Entitätsnamen `Drink` in das Textfeld ein, und wählen Sie dann im Dropdownmenü die Option **Create new entity** (Neue Entität erstellen). 

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png "Screenshot: Erstellung einer neuen Entität durch Auswahl des in der Äußerung enthaltenen Worts")](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png#lightbox)

4. Wählen Sie im Popupfenster den Entitätstyp **List** (Liste) aus. Fügen Sie das Synonym `h20` hinzu. Drücken Sie nach jedem Synonym die EINGABETASTE. Fügen Sie der Synonymliste nicht das Wort `perrier` hinzu. Es wird im nächsten Schritt als Beispiel hinzugefügt. Wählen Sie **Fertig**aus.

    [![](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png "Screenshot: Konfiguration der neuen Entität")](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png#lightbox)

5. Nachdem die Entität nun erstellt wurde, können Sie die anderen Synonyme für Wasser angeben, indem Sie sie jeweils auswählen und in der Dropdownliste dann `Drink` wählen. Navigieren Sie im Menü nach rechts, und wählen Sie dann `Set as synonym` und `water` aus.

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png "Screenshot: Kennzeichnen der Äußerung mit einer vorhandenen Entität")](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png#lightbox)

## <a name="modify-the-list-entity-from-the-entity-page"></a>Ändern der Listenentität über die Seite „Entity“ (Entität)
Die Entität mit der Getränkeliste wird erstellt, aber sie enthält nicht viele Elemente und Synonyme. Wenn Sie einige Begriffe, Abkürzungen und umgangssprachliche Ausdrücke kennen, geht es schneller, die Liste auf der Seite **Entity** (Entität) auszufüllen. 

1. Wählen Sie im linken Bereich die Option **Entities** (Entitäten).

    [![](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png "Screenshot: Auswahl von „Entities“ (Entitäten) im linken Bereich")](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png#lightbox)

2. Wählen Sie in der Liste mit den Entitäten die Option `Drink`.

    [![](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png "Screenshot: Auswahl der Entität „Drink“ (Getränk) in der Liste mit den Entitäten")](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png#lightbox)

3. Geben Sie im Textfeld `Soda pop` ein, und drücken Sie anschließend die EINGABETASTE. Dies ist eine Bezeichnung, die im Englischen für kohlensäurehaltige Getränke verwendet wird. In allen Kulturen gibt es eine besondere umgangssprachliche Bezeichnung für diese Art von Getränk.

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png "Screenshot: Eingabe des kanonischen Namens")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png#lightbox)

4. Geben Sie in derselben Zeile wie `Soda pop` Synonyme ein, z.B.: 

    ```
    coke
    cokes
    coca-cola
    coca-colas
    ```

    Die Synonyme können Wortgruppen, Satzzeichen, Possessivformen und Pluralbildung umfassen. Da für die Listenentität eine genaue Textübereinstimmung erforderlich ist (mit Ausnahme der Groß-/Kleinschreibung), müssen für die Synonyme alle Varianten angegeben werden. Sie können die Liste erweitern, wenn Sie über die Abfrageprotokolle oder nach einer Prüfung der Endpunktprotokolle weitere Varianten ermitteln. 

    Dieser Artikel enthält nur einige Synonyme, um das Beispiel kurz zu halten. Eine LUIS-App für die Produktionsebene sollte über viele Synonyme verfügen und regelmäßig überprüft und erweitert werden. 

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png "Screenshot: Hinzufügen von Synonymen")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png#lightbox)

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
1. Klicken Sie unten auf der Seite **Publish** (Veröffentlichen) auf den Link **Endpoint** (Endpunkt). Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Screenshot: Endpunkt-URL auf der Seite „Publish“ (Veröffentlichen)")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Geben Sie in der Adressleiste am Ende der URL `2 cokes and 3 waters` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage („**q**uery“) der Äußerung). Diese Äußerung entspricht keiner der gekennzeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `OrderDrinks` mit den beiden Getränkearten `cokes` und `waters` zurückgeben.

```
{
  "query": "2 cokes and 3 waters",
  "topScoringIntent": {
    "intent": "OrderDrinks",
    "score": 0.999998569
  },
  "intents": [
    {
      "intent": "OrderDrinks",
      "score": 0.999998569
    },
    {
      "intent": "None",
      "score": 0.23884207
    }
  ],
  "entities": [
    {
      "entity": "cokes",
      "type": "Drink",
      "startIndex": 2,
      "endIndex": 6,
      "resolution": {
        "values": [
          "Soda pop"
        ]
      }
    },
    {
      "entity": "waters",
      "type": "Drink",
      "startIndex": 14,
      "endIndex": 19,
      "resolution": {
        "values": [
          "h20"
        ]
      }
    }
  ]
}
```

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Wo wird für die Listenentität die Verarbeitung natürlicher Sprache durchgeführt? 
Da für die Listenentität eine genaue Textübereinstimmung erforderlich ist, wird keine Verarbeitung von natürlicher Sprache durchgeführt (und auch kein maschinelles Lernen). Die Verarbeitung natürlicher Sprache (bzw. maschinelles Lernen) wird für LUIS verwendet, um die richtige Absicht mit der höchsten Bewertung auszuwählen. Darüber hinaus kann eine Äußerung eine Mischung von mehr als einer Entität oder sogar mehr als einem Entitätstyp sein. Jede Äußerung wird für alle Entitäten der App verarbeitet, einschließlich der Entitäten, die auf der Verarbeitung natürlicher Sprache (oder maschinellem Lernen) basieren, z.B. der Entität **Simple** (Einfach).

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit nur zwei Absichten und einer Listenentität hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten zurückgegeben. 

Ihr Chatbot verfügt jetzt über genügend Informationen, um ermitteln zu können, welche Hauptaktion (`OrderDrinks`) durchgeführt werden soll und welche Arten von Getränken über die Listenentität „Drink“ (Getränk) bestellt wurden. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (z.B. ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls sie nicht mehr benötigt wird. Klicken Sie hierzu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Delete** (Löschen). Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Sie eine Entität vom Typ „Regulärer Ausdruck“ hinzufügen](luis-quickstart-intents-regex-entity.md)

Fügen Sie die [vordefinierte Entität](luis-how-to-add-entities.md#add-prebuilt-entity) **number** (Anzahl) zum Extrahieren der Anzahl hinzu. 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions

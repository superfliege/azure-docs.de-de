---
title: 'Tutorial: Erstellen einer LUIS-App zum Abrufen von Standortdaten – Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache LUIS-App erstellen, in der Absichten und eine hierarchische Entität zum Extrahieren von Daten verwendet werden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: v-geberr
ms.openlocfilehash: 2547407126943161ba604fa2f5e80b9186cae57e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266497"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Tutorial: Erstellen einer App mit einer Entität vom Typ „Hierarchisch“
In diesem Tutorial erstellen Sie eine App, mit der veranschaulicht wird, wie Sie anhand des Kontexts nach zusammengehörenden Datenelementen suchen. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu hierarchischen Entitäten und aus dem Kontext erschlossenen Unterelementen 
> * Erstellen einer neuen LUIS-App für den Reisebereich mit der Absicht „BookFlight“ (Flug buchen)
> * Hinzufügen der Absicht _None_ (Keine) und Hinzufügen von Beispielen für Äußerungen
> * Hinzufügen einer hierarchischen Standortentität mit Unterelementen zu Abflug- und Zielort
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort, einschließlich hierarchischer untergeordneter Elemente 

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="purpose-of-the-app-with-this-entity"></a>Zweck der App mit dieser Entität
Mit dieser App wird ermittelt, ob ein Benutzer einen Flug buchen möchte. Sie nutzt die hierarchische Entität, um die Standorte (den Abflugort und den Zielort) im Text des Benutzers zu ermitteln. 

Die hierarchische Entität ist eine gute Wahl für diese Art von Daten, da für die beiden Datentypen Folgendes gilt:

* Beides sind Standorte, die normalerweise als Städte oder Flughafencodes angegeben werden.
* Normalerweise werden eindeutige Wörter gewählt, sodass ermittelt werden kann, was der Abflug- und was der Zielort ist. Beispiele für diese Wörter sind: nach, Ziel, von, Abflug.
* Beide Standorte sind häufig Teil derselben Äußerung. 

Der Zweck der Entität **hierarchical** (Hierarchisch) besteht darin, in der Äußerung anhand des Kontexts nach zusammengehörenden Daten zu suchen. Wir verwenden als Beispiel die folgende Äußerung:

```JSON
1 ticket from Seattle to Cairo`
```

In der Äußerung werden zwei Orte angegeben. Dies sind der Abflugort „Seattle“ und der Zielort „Kairo“. Beide Städtenamen sind wichtig, um einen Flug buchen zu können. Sie können zwar auch mit einfachen Entitäten gefunden werden, aber sie gehören zusammen und sind häufig in derselben Äußerung enthalten. Daher ist es sinnvoll, beide als untergeordnete Elemente der hierarchischen Entität **Location** (Standort) zu gruppieren. 

Da auf maschinellem Lernen basierende Entitäten verwendet werden, benötigt die App Beispieläußerungen, für die jeweils der Abflug- und Zielort angegeben ist. So kann LUIS lernen, wo sich die Entitäten in den Äußerungen befinden, wie lang sie sind und welche Wörter in der Nähe vorkommen. 

## <a name="app-intents"></a>App-Absichten
Die Absichten sind Kategorien für die Wünsche des Benutzers. Diese App verfügt über zwei Absichten: „BookFlight“ (Flug buchen) und „None“ (Keine). Die Absicht [None](luis-concept-intent.md#none-intent-is-fallback-for-app) (Keine) dient für alle Zwecke außerhalb der App.  

## <a name="hierarchical-entity-is-contextually-learned"></a>Aus dem Kontext erschlossene hierarchische Entität 
Der Zweck der Entität besteht darin, Teile des Texts einer Äußerung zu finden und zu kategorisieren. Eine Entität vom Typ [hierarchical](luis-concept-entity-types.md) (Hierarchisch) basiert auf dem Kontext der Wortverwendung. Eine Person kann den Abflug- und Zielort anhand der Verwendung von `to` (nach) und `from` (von) ermitteln. Dies ist ein Beispiel für die kontextabhängige Verwendung.  

Für diese Reise-App wird der Abflug- und Zielort von LUIS so extrahiert, dass eine Standardreservierung erstellt und ausgefüllt werden kann. Bei LUIS können Äußerungen Varianten, Abkürzungen und umgangssprachliche Ausdrücke enthalten. 

Einfache Beispiele für Äußerungen von Benutzern sind:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Researve a seat from New York to Paris on the first of April
```

Hier sind einige abgekürzte oder umgangssprachliche Versionen von Äußerungen aufgeführt:

```
LHR tomorrow
SEA to NYC next Monday
LA to MCO spring break
```
 
Die hierarchische Entität gleicht den Abflug- und Zielort ab. Wenn nur ein untergeordnetes Element (Abflug oder Ziel) einer hierarchischen Entität vorhanden ist, wird es trotzdem extrahiert. Es müssen nicht alle untergeordneten Elemente gefunden werden, um nur ein bzw. einige Elemente extrahieren zu können. 

## <a name="what-luis-does"></a>Vorgehensweise von LUIS
Nachdem die Absicht und die Entitäten der Äußerung identifiziert, [extrahiert](luis-concept-data-extraction.md#list-entity-data) und im JSON-Format vom [Endpunkt](https://aka.ms/luis-endpoint-apis) zurückgegeben wurden, ist der LUIS-Vorgang abgeschlossen. Die aufrufende Anwendung bzw. der Chatbot verwendet diese JSON-Antwort, um die Anforderung zu erfüllen – jeweils gemäß der Auslegung der App bzw. des Chatbots. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich an der [LUIS][LUIS]-Website an. Achten Sie darauf, sich unter der [Region][LUIS-regions] anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS][LUIS]-Website auf **Create new app** (Neue App erstellen).  

    [![](media/luis-quickstart-intent-and-hier-entity/app-list.png "Screenshot: Seite mit App-Listen")](media/luis-quickstart-intent-and-hier-entity/app-list.png#lightbox)

3. Geben Sie im Popupdialogfenster den Namen `MyTravelApp` ein. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-app.png "Screenshot: Popupdialogfenster zum Erstellen einer neuen App")](media/luis-quickstart-intent-and-hier-entity/create-new-app.png#lightbox)

4. Nach Abschluss dieses Vorgangs wird in der App die Seite **Intents** (Absichten) mit der Absicht **None** (Keine) angezeigt. 

    [![](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png "Screenshot: Liste „Intents“ (Absichten), die nur die Absicht „None“ (Keine) enthält")](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Erstellen einer neuen Absicht

1. Wählen Sie auf der Seite **Absichten** die Option **Create new intent** (Neue Absicht erstellen). 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png "Screenshot: Liste „Intents“ (Absichten) mit Hervorhebung der Schaltfläche „Create new intent“ (Neue Absicht erstellen)")](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png#lightbox)

2. Geben Sie den neuen Namen für die Absicht ein: `BookFlight`. Diese Absicht sollte immer ausgewählt werden, wenn ein Benutzer Flugreservierungen durchführen möchte.

    Indem Sie eine Absicht erstellen, erstellen Sie auch die primäre Kategorie der zu ermittelnden Informationen. Durch die Benennung der Kategorie können andere Anwendungen, die die Ergebnisse der LUIS-Abfrage nutzen, anhand des Kategorienamens eine passende Antwort finden oder geeignete Aktionen ausführen. LUIS liefert keine Antwort auf die eigentliche Frage, sondern gibt lediglich an, welche Art von Information in natürlicher Sprache erfragt wird. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png "Screenshot: Popupdialogfenster „Create new intent“ (Neue Absicht erstellen)")](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png#lightbox)

3. Fügen Sie der Absicht `BookFlight` mehrere voraussichtliche Äußerungen von Benutzern hinzu, z.B.:

    | Beispiele für Äußerungen|
    |--|
    |Book 2 flights from Seattle to Cairo next Monday (Buchung von 2 Flügen von Seattle nach Kairo am nächsten Montag)|
    |Reserve a ticket to London tomorrow (Reservierung eines Flugtickets nach London für morgen)|
    |Schedule 4 seats from Paris to London for April 1 (4 Plätze von Paris nach London für den 1. April reservieren)|

    [![](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png "Screenshot: Eingabe von Äußerungen auf der Seite mit der Absicht „BookFlight“ (Flug buchen)")](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Hinzufügen von Äußerungen zur Absicht „None“ (Keine)

Die LUIS-App enthält derzeit keine Äußerungen für die Absicht **None** (Keine). Es sind Äußerungen erforderlich, die die App nicht beantworten soll. Daher müssen Äußerungen für die Absicht **None** (Keine) enthalten sein. Lassen Sie sie nicht leer. 

1. Wählen Sie im linken Bereich **Intents** (Absichten). 

    [![](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png "Screenshot: Seite mit Absicht „BookFlight“ (Flug buchen) mit Hervorhebung der Schaltfläche „Intents“ (Absichten)")](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png#lightbox)

2. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, aber die für Ihre App nicht relevant sind:

    | Beispiele für Äußerungen|
    |--|
    |Cancel! (Abbrechen!)|
    |Good bye (Auf Wiedersehen)|
    |What is going on? (Was ist los?)|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Vorhersage der Äußerung für die Absicht „None“ (Keine)
In der Anwendung, die den LUIS-Dienst aufruft (z.B. ein Chatbot), kann der Bot den Benutzer fragen, ob er die Unterhaltung beenden möchte, wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt. Der Bot kann außerdem weitere Anweisungen zum Fortsetzen der Unterhaltung geben, falls der Benutzer sie nicht beenden möchte. 

Entitäten werden in der Absicht **None** (Keine) verwendet. Wenn die Absicht **None** die höchste Bewertung hat, aber eine Entität extrahiert wird, die für Ihren Chatbot aussagekräftig ist, kann Ihr Chatbot eine Frage zur Absicht des Kunden stellen. 

## <a name="create-a-location-entity-from-the-intent-page"></a>Erstellen einer Standortentität auf der Seite „Intent“ (Absicht)
Nachdem die beiden Absichten jetzt über Äußerungen verfügen, muss LUIS wissen, was ein Standort ist. Navigieren Sie zurück zur Absicht `BookFlight`, und kennzeichnen Sie die in einer Äußerung enthaltenen Städtenamen mit den folgenden Schritten:

1. Kehren Sie zur Absicht `BookFlight` zurück, indem Sie im linken Bereich **Intents** (Absichten) wählen.

2. Wählen Sie `BookFlight` in der Liste mit den Absichten aus.

3. Wählen Sie in der Äußerung `Book 2 flights from Seattle to Cairo next Monday` das Wort `Seattle` aus. Ein Dropdownmenü mit einem darüber angeordneten Textfeld zum Erstellen einer neuen Entität wird angezeigt. Geben Sie den Entitätsnamen `Location` in das Textfeld ein, und wählen Sie dann im Dropdownmenü die Option **Create new entity** (Neue Entität erstellen). 

    [![](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png "Screenshot: Seite mit der Absicht „BookFlight“ (Flug buchen), Erstellung einer neuen Entität aus ausgewähltem Text")](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png#lightbox)

4. Wählen Sie im Popupfenster den Entitätstyp **Hierarchical** (Hierarchisch) mit `Origin` und `Destination` als untergeordnete Elemente aus. Wählen Sie **Fertig**aus.

    [![](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png "Screenshot: Popupdialogfeld zur Entitätserstellung für die neue Entität „Location“ (Standort)")](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png#lightbox)

    Die Bezeichnung für `Seattle` ist als `Location` gekennzeichnet, da LUIS nicht weiß, ob es sich hierbei um den Abflugort, Zielort oder einen anderen Ort handelt. Wählen Sie `Seattle` und dann „Location“ (Standort). Navigieren Sie im Menü nach rechts, und wählen Sie anschließend `Origin` aus.

5. Nachdem die Entität nun erstellt und eine Äußerung gekennzeichnet wurde, können Sie die anderen Städte wie folgt kennzeichnen: Wählen Sie den Namen des Orts und dann „Location“ (Standort) aus, navigieren Sie im Menü nach rechts, und wählen Sie die Option `Origin` oder `Destination`.

    [![](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png "Screenshot: Entität „BookFlight“ (Flug buchen) mit ausgewähltem Text der Äußerung für die Wahl der Entität")](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png#lightbox)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS ist erst dann über die Änderungen an den Absichten und Entitäten (Modell) informiert, nachdem der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Train** (Trainieren).

    ![Trainieren der App](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Training erfolgreich](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Publish** (Veröffentlichen). 

    [![](media/luis-quickstart-intent-and-hier-entity/publish.png "Screenshot: Absicht „BookFlight“ (Flug buchen) mit Hervorhebung der Schaltfläche „Publish“ (Veröffentlichen)")](media/luis-quickstart-intent-and-hier-entity/publish.png#lightbox)

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Publish** (Veröffentlichen).

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Screenshot der Seite „Publish“ (Veröffentlichen) mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Publish** (Veröffentlichen) auf den Link **Endpoint** (Endpunkt). Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Screenshot: Seite „Publish“ (Veröffentlichen) mit hervorgehobener Endpunkt-URL")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Geben Sie in der Adressleiste am Ende der URL `1 ticket to Portland on Friday` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage („**q**uery“) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `BookFlight` mit der extrahierten hierarchischen Entität zurückgeben.

```
{
  "query": "1 ticket to Portland on Friday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9998226
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.9998226
    },
    {
      "intent": "None",
      "score": 0.221926212
    }
  ],
  "entities": [
    {
      "entity": "portland",
      "type": "Location::Destination",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.564448953
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit nur zwei Absichten und einer hierarchischen Entität hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten zurückgegeben. 

Ihr Chatbot verfügt jetzt über genügend Informationen, um die Hauptaktion `BookFlight` und die in der Äußerung enthaltenen Informationen zum Standort zu ermitteln. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (z.B. ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls sie nicht mehr benötigt wird. Klicken Sie hierzu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Delete** (Löschen). Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **OK**.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Erfahren Sie, wie Sie eine Listenentität hinzufügen](luis-quickstart-intent-and-list-entity.md) 

Fügen Sie die [vordefinierte Entität](luis-how-to-add-entities.md#add-prebuilt-entity) **number** (Anzahl) zum Extrahieren der Anzahl hinzu. 

Fügen Sie die [vordefinierte Entität](luis-how-to-add-entities.md#add-prebuilt-entity) **datetimeV2** zum Extrahieren der Datumsinformationen hinzu.


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions

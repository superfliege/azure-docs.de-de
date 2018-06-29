---
title: 'Tutorial: Erstellen einer LUIS-App zum Extrahieren von Daten: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache LUIS-App erstellen, die Absichten und eine Entität vom Typ „Einfach“ verwendet, um Machine Learning-Daten (ML) zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: v-geberr
ms.openlocfilehash: 1e8647e34da3d34946a4f6ac298017f6d4c99de6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265359"
---
# <a name="tutorial-create-app-that-uses-simple-entity"></a>Tutorial: Erstellen einer App, die eine Entität vom Typ „Einfach“ verwendet
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie mithilfe der Entität vom Typ **Einfach** ML-Daten aus einer Äußerung extrahiert werden.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu einfachen Entitäten 
> * Erstellen einer neuen LUIS-App für den Kommunikationsbereich mit der Absicht „SendMessage“
> * Hinzufügen der Absicht vom Typ _None_ (Keine) und Hinzufügen von Beispieläußerungen
> * Hinzufügen der Entität vom Typ „Einfach“, um Nachrichteninhalte aus einer Äußerung zu extrahieren
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="purpose-of-the-app"></a>Zweck der App
Diese App veranschaulicht, wie Sie Daten aus einer Äußerung extrahieren. Sehen Sie sich die folgende Äußerung eines Chatbots an:

```JSON
Send a message telling them to stop
```

Die Absicht ist das Senden einer Nachricht. Die wichtigen Daten der Äußerung stellen die Nachricht selbst dar: `telling them to stop`.  

## <a name="purpose-of-the-simple-entity"></a>Zweck der Entität vom Typ „Einfach“
Die Entität vom Typ „Einfach“ dient dazu, LUIS zu lehren, was eine Nachricht ist und wie sie in einer Äußerung ermittelt werden kann. Der Teil der Äußerung, der die Nachricht darstellt, kann sich basierend auf der Wortwahl und der Länge der Äußerung bei den einzelnen Äußerungen unterscheiden. LUIS benötigt Beispiele für Nachrichten in jeder Äußerung und für alle Absichten.  

Bei dieser einfachen App befindet sich die Nachricht am Ende der Äußerung. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich bei der [LUIS][LUIS]-Website an. Achten Sie darauf, sich unter der Region anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS][LUIS]-Website auf **Neue App erstellen**.  

    ![Liste der LUIS-Apps](./media/luis-quickstart-primary-and-secondary-data/app-list.png)

3. Geben Sie im Popupdialogfenster den Namen `MyCommunicator` ein. 

    ![Liste der LUIS-Apps](./media/luis-quickstart-primary-and-secondary-data/create-new-app-dialog.png)

4. Nach Abschluss dieses Vorgangs wird in der App die Seite **Absichten** mit der Absicht vom Typ **None** (Keine) angezeigt. 

    [![](media/luis-quickstart-primary-and-secondary-data/intents-list.png "Screenshot der LUIS-Seite „Absichten“ mit der Absicht vom Typ „None“ (Keine)")](media/luis-quickstart-primary-and-secondary-data/intents-list.png#lightbox)

## <a name="create-a-new-intent"></a>Erstellen einer neuen Absicht

1. Klicken Sie auf der Seite **Absichten** auf **Create new intent** (Neue Absicht erstellen). 

    [![](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png "Screenshot von LUIS mit hervorgehobener Schaltfläche „Create new intent“ (Neue Absicht erstellen)")](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png#lightbox)

2. Geben Sie den neuen Namen für die Absicht ein: `SendMessage`. Diese Absicht muss jedes Mal ausgewählt werden, wenn ein Benutzer eine Nachricht senden möchte.

    Indem Sie eine Absicht erstellen, erstellen Sie auch die primäre Kategorie der zu bestimmenden Informationen. Durch die Benennung der Kategorie können andere Anwendungen, die die Ergebnisse der LUIS-Abfrage nutzen, anhand des Kategorienamens eine passende Antwort finden oder geeignete Aktionen ausführen. LUIS liefert keine Antwort auf die eigentliche Frage, sondern gibt lediglich an, welche Art von Information in natürlicher Sprache erfragt wird. 

    ![Eingeben des Absichtsnamens „SendMessage“](./media/luis-quickstart-primary-and-secondary-data/create-new-intent-popup-dialog.png)

3. Fügen Sie der Absicht `SendMessage`, nach der Benutzer voraussichtlich fragen, sieben Äußerungen hinzu. Beispiele:

    | Beispieläußerungen|
    |--|
    |Reply with I got your message, I will have the answer tomorrow (Antworten mit: Ich habe Ihre Nachricht erhalten und werde morgen antworten.)|
    |Send message of When will you be home? (Nachricht senden: Wann wirst du zu Hause sein?)|
    |Text that I am busy (SMS senden: Ich bin beschäftigt.)|
    |Tell them that it needs to be done today (Mitteilen, dass es heute erledigt werden muss)|
    |IM that I am driving and will respond later (Sofortnachricht senden, dass ich im Auto unterwegs bin und später antworte)|
    |Compose message to David that says When was that? (Nachricht an David erstellen: Wann war das?)|
    |say greg hello (Grüße an Greg)|

    [![](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png "Screenshot von LUIS mit eingegeben Äußerungen")](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Hinzufügen von Äußerungen zur Absicht vom Typ „None“ (Keine)

Die LUIS-App enthält derzeit keine Äußerungen für die Absicht vom Typ **None** (Keine). Es sind Äußerungen erforderlich, die die App nicht beantworten soll, daher muss sie Äußerungen in der Absicht vom Typ **None** (Keine) enthalten. Lassen Sie sie nicht leer. 
    
1. Wählen Sie im linken Bereich **Absichten**. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png "Screenshot von LUIS mit hervorgehobener Schaltfläche „Absichten“")](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png#lightbox)

2. Wählen Sie die Absicht vom Typ **None** (Keine) aus. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png "Screenshot zum Auswählen der Absicht vom Typ „None“ (Keine)")](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png#lightbox)

3. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre App jedoch nicht relevant sind. Einige geeignete Äußerungen für **None** (Keine):

    | Beispieläußerungen|
    |--|
    |Cancel! (Abbrechen!)|
    |Good bye (Auf Wiedersehen)|
    |What is going on? (Was ist los?)|
    
    In der Anwendung, die den LUIS-Dienst aufruft (beispielsweise ein Chatbot), kann der Bot den Benutzer fragen, ob er die Unterhaltung beenden möchte, wenn LUIS für eine Äußerung die Absicht vom Typ **None** (Keine) zurückgibt. Der Bot kann außerdem weitere Anweisungen zum Fortsetzen der Unterhaltung geben, falls der Benutzer sie nicht beenden möchte. 

    [![](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png "Screenshot von LUIS mit Äußerungen für die Absicht vom Typ „None“ (Keine)")](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png#lightbox)

## <a name="create-a-simple-entity-to-extract-message"></a>Erstellen einer Entität vom Typ „Einfach“ zum Extrahieren von Nachrichten 
1. Wählen Sie im linken Menü die Option **Absichten**.

    ![Klicken auf den Link „Absichten“](./media/luis-quickstart-primary-and-secondary-data/select-intents-from-none-intent.png)

2. Wählen Sie `SendMessage` in der Liste der Absichten aus.

    ![Auswählen der Absicht „SendMessage“](./media/luis-quickstart-primary-and-secondary-data/select-sendmessage-intent.png)

3. Wählen Sie in der Äußerung `Reply with I got your message, I will have the answer tomorrow` das erste Wort (`I`) und das letzte Wort (`tomorrow`) des Nachrichtentexts aus. Alle diese Wörter werden für die Nachricht ausgewählt, und es wird ein Dropdownmenü mit einem Textfeld am oberen Rand angezeigt.

    [![](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png "Screenshot: Auswählen von Wörtern in einer Äußerung für die Nachricht")](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png#lightbox)

4. Geben Sie den Entitätsnamen `Message` in das Textfeld ein.

    [![](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png "Screenshot: Eingeben des Entitätsnamens in das Feld")](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png#lightbox)

5. Klicken Sie im Dropdownmenü auf **Create new entity** (Neue Entität erstellen). Der Zweck der Entität besteht im Extrahieren des Texts, der den Nachrichtentext darstellt. In dieser LUIS-App befindet sich die Textnachricht am Ende der Äußerung. Die Äußerung und die Nachricht können jedoch jeweils eine beliebige Länge haben. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png "Screenshot: Erstellen einer neuer Entität auf der Grundlage einer Äußerung")](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png#lightbox)

6. Im Popupfenster lautet der Standardentitätstyp **Einfach** und der Entitätsname `Message`. Übernehmen Sie diese Einstellungen, und wählen Sie **Fertig**.

    ![Überprüfen des Entitätstyps](./media/luis-quickstart-primary-and-secondary-data/entity-type.png)

7. Die Entität wurde erstellt und eine Äußerung mit einer Bezeichnung versehen. Versehen Sie nun die übrigen Äußerungen mit der Bezeichnung dieser Entität. Wählen Sie eine Äußerung und anschließend das erste und letzte Wort einer Nachricht aus. Wählen Sie im Dropdownmenü die Entität `Message` aus. Die Nachricht wird in der Entität mit einer Bezeichnung versehen. Versehen Sie alle Nachrichtenausdrücke in den übrigen Äußerungen mit einer Bezeichnung.

    [![](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png "Screenshot aller Nachrichtenäußerungen mit Bezeichnung")](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png#lightbox)

    Die Standardansicht der Äußerungen heißt **Entities view** (Entitätsansicht). Klicken Sie oberhalb der Äußerungen auf das Steuerelement **Entities view** (Entitätsansicht). In der **Tokens view** (Tokenansicht) wird der Text der Äußerungen angezeigt. 

    [![](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png "Screenshot der Äußerungen in der Tokenansicht")](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png#lightbox)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS weiß erst über die Änderungen an den Absichten und Entitäten (Modell) Bescheid, wenn der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Trainieren**.

    ![Klicken auf die Schaltfläche „Trainieren“](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Trainingserfolgsmeldung](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Veröffentlichen**. 

2. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Veröffentlichen**.

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
Klicken Sie auf der Seite **Veröffentlichen** unten auf den Link **Endpunkt**. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobenem Endpunkt")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Dadurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. Geben Sie in der Adressleiste am Ende der URL `text I'm driving and will be 30 minutes late to the meeting` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Äußerungen vom Typ `SendMessage` zurückgeben.

```
{
  "query": "text I'm driving and will be 30 minutes late to the meeting",
  "topScoringIntent": {
    "intent": "SendMessage",
    "score": 0.987501
  },
  "intents": [
    {
      "intent": "SendMessage",
      "score": 0.987501
    },
    {
      "intent": "None",
      "score": 0.111048922
    }
  ],
  "entities": [
    {
      "entity": "i ' m driving and will be 30 minutes late to the meeting",
      "type": "Message",
      "startIndex": 5,
      "endIndex": 58,
      "score": 0.162995353
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit nur zwei Absichten und einer Entität hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die Nachrichtendaten zurückgegeben. 

Laut JSON-Ergebnis hat die Absicht `SendMessage` die höchste Punktzahl (0,987501). Alle Punktzahlen liegen zwischen 1 und 0. Die bessere Punktzahl liegt näher bei 1. Die Punktzahl der Absicht `None` lautet 0,111048922, was näher bei 0 liegt. 

Für die Nachrichtendaten ist sowohl ein Typ (`Message`) als auch ein Wert (`i ' m driving and will be 30 minutes late to the meeting`) angegeben. 

Ihr Chatbot verfügt nun über ausreichend Informationen, um die primäre Aktion (`SendMessage`) zu ermitteln, sowie über einen Parameter für diese Aktion, nämlich den Text der Nachricht. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um die Nachricht über eine Drittanbieter-API zu senden. Sind andere programmgesteuerte Optionen für den Bot oder die aufrufende Anwendung vorhanden, werden sie nicht von LUIS verwendet. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls sie nicht mehr benötigt wird. Klicken Sie dazu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **Ok**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer hierarchischen Entität](luis-quickstart-intent-and-hier-entity.md)


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website

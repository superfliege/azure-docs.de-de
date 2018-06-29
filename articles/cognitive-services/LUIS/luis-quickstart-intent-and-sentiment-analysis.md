---
title: 'Tutorial: Erstellen einer LUIS-App zum Zurückgeben einer Stimmungsanalyse: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine Stimmungsanalyse zu Ihrer LUIS-App hinzufügen, um Äußerungen auf positive, negative und neutrale Emotionen zu analysieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d000637312619fc493e2f7bad8e8edf0d8d0d94b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265333"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Tutorial: Erstellen einer App, die neben der Absichtsvorhersage die Stimmung zurückgibt
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie Sie positive, negative und neutrale Emotionen aus Äußerungen extrahieren.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu hierarchischen Entitäten und aus dem Kontext erschlossenen Unterelementen 
> * Erstellen einer neuen LUIS-App für den Reisebereich mit der Absicht „Bookflight“ (Flug buchen)
> * Hinzufügen der Absicht vom Typ _None_ (Keine) und Hinzufügen von Beispieläußerungen
> * Hinzufügen einer hierarchischen Standortentität mit Unterelementen zu Ursprung und Ziel
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort, einschließlich hierarchischer untergeordneter Elemente 

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="sentiment-analysis"></a>Stimmungsanalyse
Mithilfe der Stimmungsanalyse können Sie ermitteln, ob die Äußerung eines Benutzers positiv, negativ oder neutral ist. 

Die folgenden Äußerungen sind Beispiele für die Stimmung:

|Stimmung und Punktzahl|Äußerung|
|:--|--|
|positiv – 0,89 |The soup and salad combo was great. (Die Suppe und der Salat waren hervorragend.)|
|negativ – 0,07 |I didn't like the appetizer during the dinner service. (Mir hat die Vorspeise beim Abendessen nicht geschmeckt.)|

Die Stimmungsanalyse ist eine App-Einstellung, die auf jede Äußerung angewendet wird. Sie müssen die Wörter, die auf die Stimmung in einer Äußerung hinweisen, nicht suchen und bezeichnen. LUIS übernimmt das für Sie.

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich bei der [LUIS][LUIS]-Website an. Achten Sie darauf, sich unter der [Region][LUIS-regions] anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS][LUIS]-Website auf **Neue App erstellen**. 

    [![](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png "Screenshot der Seite mit der App-Liste")](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png#lightbox)

3. Geben Sie im Dialogfeld **Neue App erstellen** den Namen `Restaurant Reservations With Sentiment` für die App ein, und klicken Sie auf **Fertig**. 

    ![Bild des Dialogfelds „Neue App erstellen“](./media/luis-quickstart-intent-and-sentiment-analysis/create-app-ddl.png)

    Nach der Erstellung der App zeigt LUIS die Absichtsliste mit der Absicht vom Typ „None“ (Keine) an.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png "Screenshot der Seite mit der Liste der Absichten")](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png#lightbox)

## <a name="add-a-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne
Fügen Sie eine vordefinierte Domäne hinzu, um schnell Absichten, Entitäten und bezeichnete Äußerungen hinzuzufügen.

1. Klicken Sie im linken Menü auf **Prebuilt Domains** (Vordefinierte Domänen).

    [ ![Screenshot der Schaltfläche „Prebuilt Domains“ (Vordefinierte Domänen)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-expanded.png#lightbox)

2. Wählen Sie für die vordefinierte Domäne **RestaurantReservation** die Option **Domäne hinzufügen**. Warten Sie, bis die Domäne hinzugefügt wurde.

    [ ![Screenshot der Schaltfläche „Prebuilt Domains“ (Vordefinierte Domänen)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-expanded.png#lightbox)

3. Klicken Sie im linken Navigationsbereich auf **Absichten**. Diese vordefinierte Domäne enthält eine Absicht.

    [ ![Screenshot der Liste mit vordefinierten Domänen und der hervorgehobenen Option „Absichten“ im linken Navigationsbereich](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png#lightbox)

4.  Klicken Sie auf die Absicht **RestaurantReservation.Reserve**. 

    [ ![Screenshot der Absichtsliste mit hervorgehobener Absicht „RestaurantReservation.Reserve“](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png#lightbox)

5. Schalten Sie **Entities View** (Entitätsansicht) um, um die zahlreichen für domänenspezifische bezeichnete Entitäten angegebenen Äußerungen anzuzeigen.

    [ ![Screenshot der Absicht „RestaurantReservation.Reserve“, auf dem die Entitätsansicht auf die hervorgehobene Tokenansicht umgestellt wurde](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-expanded.png#lightbox)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS weiß erst über die Änderungen an den Absichten und Entitäten (Modell) Bescheid, wenn der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Trainieren**.

    ![Screenshot der hervorgehobenen Schaltfläche „Trainieren“](./media/luis-quickstart-intent-and-sentiment-analysis/train-button-expanded.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Screenshot der Benachrichtigungsleiste zum Trainingserfolg ](./media/luis-quickstart-intent-and-sentiment-analysis/trained-expanded.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Konfigurieren der App zur Aufnahme der Stimmungsanalyse
Die Stimmungsanalyse wird auf der Seite **Veröffentlichen** aktiviert. 

1. Klicken Sie im Navigationsbereich oben rechts auf **Veröffentlichen**.

    ![Screenshot der Seite „Absicht“ mit erweiterter Schaltfläche „Veröffentlichen“ ](./media/luis-quickstart-intent-and-sentiment-analysis/publish-expanded.png)

2. Aktivieren Sie **Enable Sentiment Analysis** (Stimmungsanalyse aktivieren).

    ![Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Option „Enable Sentiment Analysis“ (Stimmungsanalyse aktivieren) ](./media/luis-quickstart-intent-and-sentiment-analysis/enable-sentiment-expanded.png)

3. Wählen Sie den Produktionsslot aus, und klicken Sie auf die Schaltfläche **Veröffentlichen**.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-inline.png "Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Schaltfläche zum Veröffentlichen im Produktionsslot")](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-expanded.png#lightbox)

4. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-an-utterance"></a>Abfragen des Endpunkts mit einer Äußerung

1. Klicken Sie auf der Seite **Veröffentlichen** unten auf den Link **Endpunkt**. Dadurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    ![Screenshot der Seite „Veröffentlichen“ mit hervorgehobener Endpunkt-URL](media/luis-quickstart-intent-and-sentiment-analysis/endpoint-url-inline.png)

2. Geben Sie in der Adressleiste am Ende der URL `Reserve table for  10 on upper level away from kitchen` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `RestaurantReservation.Reserve` mit der extrahierten Stimmungsanalyse zurückgeben.

```
{
  "query": "Reserve table for 10 on upper level away from kitchen",
  "topScoringIntent": {
    "intent": "RestaurantReservation.Reserve",
    "score": 0.9926384
  },
  "intents": [
    {
      "intent": "RestaurantReservation.Reserve",
      "score": 0.9926384
    },
    {
      "intent": "None",
      "score": 0.00961109251
    }
  ],
  "entities": [],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit aktivierter Stimmungsanalyse hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten sowie die allgemeine Stimmung als Punktzahl zurückgegeben. 

Ihr Chatbot verfügt nun über ausreichend Informationen, um den nächsten Schritt in der Unterhaltung zu bestimmen. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
Für LUIS ist diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Stimmungsdaten aus der Äußerung verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls sie nicht mehr benötigt wird. Klicken Sie dazu in der Liste rechts vom App-Namen auf die drei Punkte (...) und anschließend auf **Löschen**. Klicken Sie im Popupdialogfenster **Delete app?** (App löschen?) auf **Ok**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Aufrufen der LUIS-Endpunkt-API mit C#](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions

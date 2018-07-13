---
title: Erstellen einer zusammengesetzten Entität zum Extrahieren komplexer Daten – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine zusammengesetzte Entität in Ihrer LUIS-App erstellen, um unterschiedliche Typen von Entitätsdaten zu extrahieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: cb581ee60dea2b0810332933455a03a8b68e16ea
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264384"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Verwenden einer zusammengesetzten Entität zum Extrahieren von komplexen Daten
Diese einfache App verfügt über zwei [Absichten](luis-concept-intent.md) und mehrere Entitäten. Ihr Zweck besteht im Buchen von Flügen (z.B. „1 Ticket von Seattle nach Kairo am Freitag“) und dem Zurückgegeben aller Einzelheiten der Reservierung als ein einzelnes Datenelement. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
* Hinzufügen der vordefinierten Entitäten datetimeV2 und number
* Erstellen einer zusammengesetzten Entität
* Abfragen von LUIS und Empfangen zusammengesetzter Entitätsdaten

## <a name="before-you-begin"></a>Voraussetzungen
* Ihre LUIS-App aus dem **[Schnellstart zu hierarchischen Entitäten](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Wenn Sie noch kein Abonnement besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.

## <a name="composite-entity-is-a-logical-grouping"></a>Eine zusammengesetzte Entität ist eine logische Gruppierung 
Der Zweck der Entität besteht darin, Teile des Texts einer Äußerung zu finden und zu kategorisieren. Eine zusammengesetzten Entität (Entitätstyp [Composite](luis-concept-entity-types.md)) besteht aus anderen Entitätstypen, die im Kontext gelernt wurden. Da diese Reise-App Flugreservierungen akzeptiert, gibt es mehrere Arten von Informationen, z.B. Datumsangaben, Orte und Anzahl der Sitzplätze. 

Die Informationen sind vor dem Erstellen einer zusammengesetzten Entität jeweils separate Entitäten. Erstellen Sie eine zusammengesetzte Entität, wenn die einzelnen Entitäten logisch gruppiert werden können und diese logische Gruppierung hilfreich für den Chatbot oder eine anderen Anwendung ist, die LUIS nutzt. 

Einfache Beispiele für Äußerungen von Benutzern sind:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
Die zusammengesetzte Entität umfasst die Anzahl der Sitzplätze, den Abflugort, den Zielort und das Datum. 

## <a name="what-luis-does"></a>Vorgehensweise von LUIS
Nachdem die Absicht und die Entitäten der Äußerung identifiziert, [extrahiert](luis-concept-data-extraction.md#list-entity-data) und im JSON-Format vom [Endpunkt](https://aka.ms/luis-endpoint-apis) zurückgegeben wurden, ist der LUIS-Vorgang abgeschlossen. Die aufrufende Anwendung bzw. der Chatbot verwendet diese JSON-Antwort, um die Anforderung zu erfüllen – jeweils gemäß der Auslegung der App bzw. des Chatbots. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Hinzufügen der vordefinierten Entitäten number und datetimeV2
1. Wählen Sie die App `MyTravelApp` in der Liste der Apps auf der [LUIS][LUIS]-Website aus.

2. Wenn die App geöffnet wird, wählen Sie links den Navigationslink **Entitäten** aus.

    ![Auswählen der Schaltfläche „Entitäten“](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Wählen Sie **Manage prebuilt entities** (Vordefinierte Entitäten verwalten) aus.

    ![Auswählen der Schaltfläche „Entitäten“](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Wählen Sie im Popupfeld **number** und **datetimeV2** aus.

    ![Auswählen der Schaltfläche „Entitäten“](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Damit die neuen Entitäten extrahiert werden, wählen Sie in der oberen Navigationsleiste **Train** (Trainieren) aus.

    ![Auswählen der Schaltfläche „Train“ (Trainieren)](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Verwenden vorhandener Absichten beim Erstellen zusammengesetzter Entitäten
1. Wählen Sie im linken Navigationsbereich **Absichten** aus. 

    ![Auswählen der Seite „Absichten“](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Wählen Sie `BookFlight` in der Liste mit den **Absichten** aus.  

    ![Auswählen der Absicht „BookFlight“ in der Liste](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Die vordefinierten Entitäten number und datetimeV2 werden in den Äußerungen bezeichnet.

3. Wählen Sie für die Äußerung `book 2 flights from seattle to cairo next monday` die blaue `number`-Entität und dann in der Liste **Wrap in composite entity** (In Composite-Entität einschließen) aus. Eine grüne Linie unter den Wörtern folgt dem Cursor bei der Bewegung nach rechts. Sie zeigt eine zusammengesetzte Entität an. Wählen Sie dann rechts die letzte vordefinierte Entität `datetimeV2` aus, geben Sie anschließend `FlightReservation` in das Textfeld des Popupfensters ein, und wählen Sie dann **Create new composite** (Composite-Entität erstellen) aus. 

    ![Erstellen zusammengesetzter Entitäten auf der Seite „Absichten“](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Ein Popupdialogfeld wird angezeigt, in dem Sie die untergeordneten Elemente der zusammengesetzten Entität überprüfen können. Wählen Sie **Fertig**aus.

    ![Erstellen zusammengesetzter Entitäten auf der Seite „Absichten“](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Einschließen von Entitäten in der zusammengesetzten Entität
Nachdem die zusammengesetzte Entität erstellt wurde, bezeichnen Sie die verbleibenden Äußerungen in der zusammengesetzten Entität. Um einen Ausdruck in einer zusammengesetzten Entität einzuschließen, müssen Sie das am weitesten links stehende Wort auswählen, anschließend in der angezeigten Liste **Wrap in composite entity** (In Composite-Entität einschließen) auswählen, danach das Wort ganz rechts auswählen und schließlich die benannte zusammengesetzte Entität `FlightReservation` auswählen. Diese schnelle und einfache Auswahl teilt sich in die folgenden Schritte auf:

1. Wählen Sie in der Äußerung `schedule 4 seats from paris to london for april 1` die 4 als die vordefinierte Entität number aus.

    ![Auswählen des am weitesten links stehenden Worts](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Wählen Sie in der angezeigten Liste **Wrap in composite entity** (In Composite-Entität einschließen) aus.

    ![Auswählen der Einschlussoption in der Liste](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Wählen Sie das Wort ganz rechts aus. Eine grüne Linie wird unter dem Ausdruck angezeigt, die eine zusammengesetzte Entität kennzeichnet.

    ![Auswählen des am weitesten rechts stehenden Worts](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Wählen Sie in der angezeigten Liste den Namen der zusammengesetzten Entität `FlightReservation` aus.

    ![Auswählen der benannten zusammengesetzten Entität](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Schließen Sie für die letzte Äußerung `London` und `tomorrow` mithilfe der gleichen Anweisungen in der zusammengesetzten Entität ein. 

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App
LUIS ist erst dann über die Änderungen an den Absichten und Entitäten (Modell) informiert, nachdem der Dienst trainiert wurde. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Train** (Trainieren).

    ![Trainieren der App](./media/luis-tutorial-composite-entity/train-button.png)

2. Das Training ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

    ![Training erfolgreich](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL
Damit Sie eine LUIS-Vorhersage in einem Chatbot oder einer anderen Anwendung abrufen können, muss die App veröffentlicht werden. 

1. Klicken Sie oben rechts auf der LUIS-Website auf die Schaltfläche **Veröffentlichen**. 

2. Wählen Sie den Produktionsslot und dann die Schaltfläche **Publish** (Veröffentlichen) aus.

    ![Veröffentlichen der App](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. Die Veröffentlichung ist abgeschlossen, wenn oben auf der Website die grüne Statusleiste angezeigt wird.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung
1. Klicken Sie unten auf der Seite **Veröffentlichen** auf den Link **Endpunkt**. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

    ![Auswählen der Endpunkt-URL](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Geben Sie in der Adressleiste am Ende der URL `reserve 3 seats from London to Cairo on Sunday` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (query) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `BookFlight` mit der extrahierten hierarchischen Entität zurückgeben.

```
{
  "query": "reserve 3 seats from London to Cairo on Sunday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.227036044
    }
  ],
  "entities": [
    {
      "entity": "sunday",
      "type": "builtin.datetimeV2.date",
      "startIndex": 40,
      "endIndex": 45,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-03-25"
          },
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-04-01"
          }
        ]
      }
    },
    {
      "entity": "3 seats from london to cairo on sunday",
      "type": "flightreservation",
      "startIndex": 8,
      "endIndex": 45,
      "score": 0.6892485
    },
    {
      "entity": "cairo",
      "type": "Location::Destination",
      "startIndex": 31,
      "endIndex": 35,
      "score": 0.557570755
    },
    {
      "entity": "london",
      "type": "Location::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.8933808
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "3"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "flightreservation",
      "value": "3 seats from london to cairo on sunday",
      "children": [
        {
          "type": "builtin.datetimeV2.date",
          "value": "sunday"
        },
        {
          "type": "Location::Destination",
          "value": "cairo"
        },
        {
          "type": "builtin.number",
          "value": "3"
        },
        {
          "type": "Location::Origin",
          "value": "london"
        }
      ]
    }
  ]
}
```

Diese Äußerung gibt ein Array einer zusammengesetzten Entitäten einschließlich des **flightreservation**-Objekts mit den extrahierten Daten zurück.  

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit nur zwei Absichten und einer zusammengesetzten Entität hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die extrahierten Daten zurückgegeben. 

Ihr Chatbot verfügt jetzt über genügend Informationen, um die Hauptaktion `BookFlight` und die in der Äußerung enthaltenen Informationen zur Reservierung zu ermitteln. 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (z.B. ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um den nächsten Schritt auszuführen. LUIS führt diese programmgesteuerte Aufgabe nicht für den Bot oder die aufrufende Anwendung aus. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Entitäten.](luis-concept-entity-types.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions

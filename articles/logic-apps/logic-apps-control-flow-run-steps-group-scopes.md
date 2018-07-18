---
title: Hinzufügen von Bereichen für die Ausführung von Aktionen auf der Grundlage des Gruppenstatus – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie in Azure Logic Apps Bereiche erstellen, die Workflowaktionen auf der Grundlage des Gruppenstatus ausführen.
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 1258175eb3d28d39be8be08498ba8d2e0998aa43
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298813"
---
# <a name="create-scopes-that-run-workflow-actions-based-on-group-status-in-azure-logic-apps"></a>Erstellen von Bereichen in Azure Logic Apps, die Workflowaktionen auf der Grundlage des Gruppenstatus ausführen

Wenn Aktionen nur ausgeführt werden sollen, nachdem eine andere Gruppe von Aktionen erfolgreich oder nicht erfolgreich war, gruppieren Sie diese Aktionen in einem *Bereich*. Diese Struktur ist hilfreich, wenn Sie Aktionen als logische Gruppe organisieren, den Status dieser Gruppe auswerten und Aktionen durchführen möchten, die auf dem Status des Bereichs basieren. Nachdem die Ausführung aller Aktionen in einem Bereich beendet ist, erhält der Bereich auch einen eigenen Status. Beispielsweise können Sie Bereiche verwenden, wenn Sie die [Ausnahme- und Fehlerbehandlung](../logic-apps/logic-apps-exception-handling.md#scopes) implementieren möchten. 

Zum Überprüfen des Status eines Bereichs können Sie die gleichen Kriterien wie zum Bestimmen des Ausführungsstatus einer Logik-App verwenden, z.B. „Erfolgreich“, „Fehlerhaft“, „Abgebrochen“ usw. Bei erfolgreicher Ausführung aller Aktionen des Bereichs wird der Status des Bereichs als „Erfolgreich“ festgelegt. Wenn jedoch eine Aktion im Bereich fehlschlägt oder abgebrochen wird, erhält der Bereich den Status „Fehlerhaft“. Grenzwerte für Bereiche finden Sie unter [Grenzwerte und Konfiguration](../logic-apps/logic-apps-limits-and-config.md). 

Hier sehen Sie beispielsweise eine allgemeine Logik-App, die einen Bereich zum Ausführen bestimmter Aktionen und eine Bedingung zum Überprüfen des Bereichsstatus verwendet. Wenn Aktionen im Bereich fehlschlagen oder unerwartet beendet werden, wird der Bereich mit „Fehlerhaft“ bzw. „Abgebrochen“ gekennzeichnet, und die Logik-App sendet eine Nachricht, dass der Bereich fehlerhaft ist. Wenn alle Aktionen im Bereich erfolgreich sind, sendet die Logik-App ebenfalls eine entsprechende Nachricht.

![Einrichten des Auslösers „Zeitplan: Wiederholung“](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Voraussetzungen

Um dem Beispiel in diesem Artikel zu folgen, benötigen Sie diese Elemente:

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Ein E-Mail-Konto bei einem von Logic Apps unterstützten E-Mail-Anbieter. In diesem Beispiel wird Outlook.com verwendet. Wenn Sie einen anderen Anbieter verwenden, bleibt der allgemeine Ablauf gleich, das Erscheinungsbild der Benutzeroberfläche weicht jedoch ab.

* Ein Bing Karten-Schlüssel. Informationen zum Abrufen eines Bing Karten-Schlüssels finden Sie <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">hier</a>.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Erstellen einer Beispiel-Logik-App

Erstellen Sie zunächst diese Beispiel-Logik-App, damit Sie später einen Bereich hinzufügen können:

![Erstellen einer Beispiel-Logik-App](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* Ein Auslöser vom Typ **„Zeitplan: Wiederholung“** der den Bing Karten-Dienst in einem von Ihnen festgelegten Intervall überprüft
* Die Aktion **Bing Maps - Get-Route** (Bing Karten – Route ermitteln), die die Reisezeit zwischen zwei Standorten überprüft
* Eine Bedingungsanweisung, die überprüft, ob die Reisezeit Ihre angegebene Reisezeit überschreitet
* Eine Aktion, die Ihnen eine E-Mail sendet, wenn die aktuelle Reisezeit Ihre angegebene Zeit überschreitet

Sie können Ihre Logik-App jederzeit speichern, machen Sie also häufig Gebrauch davon.

1. Melden Sie sich beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an, falls Sie dies noch nicht getan haben. Erstellen einer leeren Logik-App

2. Fügen Sie den Auslöser **Zeitplan: Wiederholung** mit diesen Einstellungen hinzu: **Intervall** = „1“ und **Häufigkeit** = „Minute“

   ![Einrichten des Auslösers „Zeitplan: Wiederholung“](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Zum visuellen Vereinfachen der Ansicht und zum Ausblenden von Details der einzelnen Aktionen im Designer reduzieren Sie die Form jeder Aktion, während Sie diese Schritte nacheinander ausführen.

3. Fügen Sie die Aktion **Bing Maps - Get route** (Bing Karten– Route ermitteln) hinzu. 

   1. Falls noch keine Verbindung mit Bing Karten besteht, werden Sie aufgefordert, eine Verbindung herzustellen.

      | Einstellung | Wert | BESCHREIBUNG |
      | ------- | ----- | ----------- |
      | **Verbindungsname** | BingMapsConnection | Geben Sie einen Namen für die Verbindung an. | 
      | **API-Schlüssel** | <*Ihr Bing Maps-Schlüssel*> | Geben Sie den Bing Maps-Schlüssel ein, den Sie zuvor abgerufen haben. | 
      ||||  

   2. Richten Sie die Aktion **Route ermitteln** wie in der Tabelle unter dieser Grafik gezeigt ein:

      ![Einrichten der Aktion „Bing Karten – Route ermitteln“](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Weitere Informationen zu diesen Parametern finden Sie unter [Calculate a route](https://msdn.microsoft.com/library/ff701717.aspx) (Berechnen einer Route).

      | Einstellung | Wert | BESCHREIBUNG |
      | ------- | ----- | ----------- |
      | **Wegpunkt 1** | <*Start*> | Geben Sie den Ausgangspunkt Ihrer Route ein. | 
      | **Wegpunkt 2** | <*Ende*> | Geben Sie das Ziel Ihrer Route ein. | 
      | **Vermeiden** | Keine | Geben Sie Elemente ein, die auf der Route vermieden werden sollen, etwa Autobahnen, Mautgebühren usw. Mögliche Werte finden Sie im Artikel zum [Berechnen einer Route](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **Optimieren** | timeWithTraffic | Wählen Sie einen Parameter zur Optimierung der Route aus, z.B. Entfernung, Reisezeit basierend auf der aktuellen Verkehrslage, usw. In diesem Beispiel wird der „timeWithTraffic“ verwendet. | 
      | **Distance unit** (Einheit für Entfernung) | <*Ihre Präferenz*> | Geben Sie die Einheit der Entfernung ein, um die Route zu berechnen. In diesem Beispiel werden Meilen verwendet. | 
      | **Travel mode** (Reisemodus) | Driving (Auto) | Geben Sie das Fortbewegungsmittel für die Route ein. In diesem Beispiel wird „Driving“ (Auto) verwendet. | 
      | **Transit Date-Time** (Datum und Uhrzeit für Transit) | Keine | Gilt für nur für den Transitmodus. | 
      | **Transit Date-Time Type** (Typ für Datum und Uhrzeit für Transit) | Keine | Gilt für nur für den Transitmodus. | 
      ||||  

4. Fügen Sie eine Bedingung hinzu, um zu prüfen, ob die aktuelle Reisezeit mit Verkehr eine angegebene Zeit überschreitet. Führen Sie für dieses Beispiel die Schritte unter dieser Grafik aus:

   ![Erstellen der Bedingung](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Benennen Sie die Bedingung mit dieser Beschreibung um: **Wenn Verkehrszeit die angegebene Zeit überschreitet**

   2. Wählen Sie in der Parameterliste das Feld **Travel Duration Traffic** (Reisedauer (Verkehr)) aus, das in Sekunden angegeben wird. 

   3. Wählen Sie **ist größer als** als Vergleichsoperator aus.

   4. Geben Sie als Vergleichswert **600**; die Einheit sind Sekunden, und der Wert entspricht 10 Minuten.

5. In der Verzweigung **If true** (Bei „true“) der Bedingung fügen Sie eine Aktion zum Senden einer E-Mail für Ihren E-Mail-Anbieter hinzu. Richten Sie diese Aktion mit den Details wie in der Tabelle unter dieser Grafik gezeigt ein:

   ![Hinzufügen der Aktion „Senden einer E-Mail“ zur Verzweigung „If true“ (Bei „true“)](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. Geben Sie im Feld **An** Ihre E-Mail-Adresse für Testzwecke an.

   2. Geben Sie im Feld **Betreff** diesen Text ein:

      ```Time to leave: Traffic more than 10 minutes```

   3. Geben Sie im Feld **Text** diesen Text mit einem nachgestellten Leerzeichen ein: 

      ```Travel time: ```

      Während der Cursor im Feld **Text** angezeigt wird, bleibt die Liste mit den dynamischen Inhalten geöffnet, damit Sie alle Parameter auswählen können, die an diesem Punkt verfügbar sind.

   4. Wenn die Liste mit den dynamischen Inhalten erscheint, wählen Sie **Ausdruck** aus.

   5. Suchen Sie die Funktion **div( )**, und wählen Sie sie aus.

   6. Während sich der Cursor innerhalb der Klammern der befindet, wählen Sie **Dynamischer Inhalt**, damit Sie als nächstes den Parameter **Travel Duration Traffic** (Reisedauer (Verkehr)) hinzufügen können.

   7. Wählen Sie unter **Get Route** (Route ermitteln) in der Liste mit dynamischen Parametern das Feld **Travel Duration Traffic** (Reisedauer (Verkehr)) aus.

      ![Auswählen von „Travel Duration Traffic“ (Reisedauer (Verkehr))](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   8. Nachdem das Feld in das JSON-Format aufgelöst wurde, fügen Sie ein **Komma** (```,```) gefolgt von der Zahl ```60``` hinzu, damit Sie den Wert **Traffic Duration Traffic** (Reisedauer (Verkehr)) von Sekunden in Minuten umwandeln können. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      Nun sieht Ihr Ausdruck wie in diesem Beispiel aus:

      ![Fertigstellen des Ausdrucks](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   9. Achten Sie darauf, **OK** zu wählen, wenn Sie fertig sind.

  10. Nachdem der Ausdruck aufgelöst wurde, fügen Sie diesen Text mit einem vorangestellten Leerzeichen hinzu: ``` minutes```
  
      Nun sieht Ihr **Textfeld** wie in diesem Beispiel aus:

      ![Fertiggestelltes Feld „Text“](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)

6. Speichern Sie Ihre Logik-App.

Fügen Sie als nächstes einen Bereich hinzu, damit Sie bestimmte Aktionen gruppieren und ihren Status auswerten können.

## <a name="add-a-scope"></a>Hinzufügen eines Bereichs

1. Öffnen Sie Ihre Logik-App im Azure-Portal im Logik-App-Designer, sofern Sie dies noch nicht getan haben. 

2. Fügen im gewünschten Workflow-Speicherort einen Bereich hinzu. Beispiel: 

   * Um einen Bereich zwischen vorhandenen Schritten im Logik-App-Workflow hinzuzufügen, bewegen Sie den Zeiger auf den Pfeil an der Position, an der Sie den Bereich hinzufügen möchten. 
   Wählen Sie das **Pluszeichen** (**+**) > **Hinzufügen eines Bereichs**.

     ![Hinzufügen eines Bereichs](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

     Wenn Sie einen Bereich am Ende Ihres Workflows hinzufügen möchten, wählen Sie in Ihrer Logik-App unten die Option **+ Neuer Schritt** > **... Mehr** > **Hinzufügen eines Bereichs**.

3. Fügen Sie jetzt die Schritte hinzu, oder ziehen Sie vorhandene Schritte, die Sie ausführen möchten, in den Bereich. Ziehen Sie für dieses Beispiel diese Aktionen in den Bereich:
      
   * **Get route** (Route ermitteln)
   * **If traffic time more than specified time** (Wenn Verkehrszeit die angegebene Zeit überschreitet); enthält die beiden Verzweigungen **true** und **false**

   Nun sieht Ihre Logik-App wie dieses Beispiel aus:

   ![Bereich hinzugefügt](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

4. Fügen Sie unter dem Bereich eine Bedingung hinzu, die den Status des Bereichs prüft. Benennen Sie die Bedingung mit dieser Beschreibung um: **If scope failed** (Bei fehlerhaftem Bereich)

   ![Hinzufügen einer Bedingung zum Überprüfen des Bereichsstatus](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
5. Erstellen Sie diesen Ausdruck, der überprüft, ob der Bereichsstatus `Failed` oder `Aborted` lautet.

   ![Hinzufügen eines Ausdrucks, der den Bereichsstatus prüft](./media/logic-apps-control-flow-run-steps-group-scopes/build-expression-check-scope-status.png)

   Oder wählen Sie **Im erweiterten Modus bearbeiten**, um diesen Ausdruck als Text hinzuzufügen.

   ```@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')```

6. Fügen Sie in den Verzweigungen **If true** (Bei „true“) und **If false** (Bei „false“) die Aktionen hinzu, die Sie ausführen möchten, beispielsweise das Senden einer E-Mail-Adresse oder einer Nachricht.

   ![Hinzufügen eines Ausdrucks, der den Bereichsstatus prüft](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

7. Speichern Sie Ihre Logik-App.

Ihre fertiggestellte Logik-App sieht nun wie in diesem Beispiel aus, in dem alle Formen erweitert sind:

![Fertiggestellte Logik-App mit Bereich](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Testen Ihrer Arbeit

Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. Wenn alle Aktionen im Bereich erfolgreich sind, erhalten Sie eine entsprechende Nachricht. Wenn Aktionen im Bereich nicht erfolgreich sind, erhalten Sie ebenfalls entsprechende Nachricht. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON-Definition

Wenn Sie in der Codeansicht arbeiten, können Sie stattdessen in der JSON-Definition Ihrer Logik-App eine Bereichsstruktur definieren. Hier sehen Sie z.B. die JSON-Definition für Auslöser und Aktionen in der vorherigen Logik-App:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    },
  }
}
```

```json
"actions": {
  "If_scope_failed": {
    "type": "If",
    "actions": {
      "Scope_failed": {
        "type": "ApiConnection",
        "inputs": {
          "body": {
            "Body": "Scope failed",
            "Subject": "Scope failed",
            "To": "<your-email@domain.com>"
          },
          "host": {
            "connection": {
              "name": "@parameters('$connections')['outlook']['connectionId']"
            }
          },
          "method": "post",
          "path": "/Mail"
        },
        "runAfter": {}
      }
    },
    "else": {
      "actions": {
        "Scope_succeded": {
          "type": "ApiConnection",
          "inputs": {
            "body": {
              "Body": "None",
              "Subject": "Scope succeeded",
              "To": "<your-email@domain.com>"
            },
            "host": {
              "connection": {
               "name": "@parameters('$connections')['outlook']['connectionId']"
              }
            },
            "method": "post",
            "path": "/Mail"
          },
          "runAfter": {}
        }
      }
    },
    "expression": "@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')",
    "runAfter": {
      "Scope": [
        "Succeeded"
      ]
    }
  },
  "Scope": {
    "type": "Scope",
    "actions": {
      "Get_route": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['bingmaps']['connectionId']"
            }
          },
          "method": "get",
          "path": "/REST/V1/Routes/Driving",
          "queries": {
            "distanceUnit": "Mile",
            "optimize": "timeWithTraffic",
            "travelMode": "Driving",
            "wp.0": "<start>",
            "wp.1": "<end>"
          }
        },
        "runAfter": {}
      },
      "If_traffic_time_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'], 60)} minutes",
                 "Subject": "Time to leave: Traffic more than 10 minutes",
                 "To": "<your-email@domain.com>"
              },
              "host": {
                "connection": {
                   "name": "@parameters('$connections')['outlook']['connectionId']"
                }
              },
              "method": "post",
              "path": "/Mail"
            },
            "runAfter": {}
          }
        },
        "expression": "@greater(body('Get_route')?['travelDurationTraffic'], 600)",
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
}
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)

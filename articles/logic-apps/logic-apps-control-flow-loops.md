---
title: Hinzufügen von Schleifen zum Wiederholen von Aktionen oder Verarbeiten von Arrays – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie in Azure Logic Apps Schleifen erstellen, die Workflowaktionen wiederholen oder Arrays verarbeiten.
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 87595eeb0330a2d8210258c097c29b205b628cf4
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298184"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Erstellen von Schleifen in Azure Logic Apps, die Workflowaktionen wiederholen oder Arrays verarbeiten

Arrays in einer Logik-App können mithilfe einer [ForEach-Schleife](#foreach-loop) oder mithilfe einer [sequenziellen ForEach-Schleife](#sequential-foreach-loop) durchlaufen werden. Die Iterationen für eine standardmäßige ForEach-Schleife werden parallel ausgeführt. Bei einer sequenziellen ForEach-Schleife werden die Iterationen dagegen einzeln ausgeführt. Informationen über die maximale Anzahl der Arrayelemente, die „ForEach“-Schleifen in einer einzelnen Logik-App-Ausführung verarbeiten können, finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Wenn Sie einen Auslöser verwenden, der ein Array empfängt, und für jedes Arrayelement einen Workflow ausführen möchten, können Sie dieses Array mit der [**Auslösereigenschaft** SplitOn](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) *aus dem Batch lösen*. 
  
Um Aktionen zu wiederholen, bis eine Bedingung erfüllt ist oder ein Status sich geändert hat, verwenden Sie eine [„Until“-Schleife](#until-loop). Ihre Logik-App führt zunächst alle Aktionen innerhalb der Schleife aus und überprüft anschließend als letzten Schritt die Bedingung. Wenn die Bedingung erfüllt ist, wird die Schleife beendet. Andernfalls wird die Schleife wiederholt. Informationen über die maximale Anzahl der „Until“-Schleifen in einer einzelnen Logik-App-Ausführung finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>„ForEach“-Schleife

Um Aktionen für jedes Element in einem Array zu wiederholen, verwenden Sie eine „ForEach“-Schleife in Ihrem Logik-App-Workflow. Sie können mehrere Aktionen in eine „ForEach“-Schleife einbeziehen und „ForEach“-Schleifen ineinander schachteln. Standardmäßig werden Zyklen in einer standardmäßigen „ForEach“-Schleife parallel ausgeführt. Welche maximale Anzahl von parallelen Zyklen „ForEach“-Schleifen ausführen können, erfahren Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Eine „ForEach“-Schleife funktioniert nur mit einem Array, und Aktionen in der Schleife verwenden den `@item()`-Verweis, um jedes Element im Array zu verarbeiten. Wenn Sie Daten angeben, die sich nicht in einem Array befinden, tritt im Logik-App-Workflow ein Fehler auf. 

Beispielsweise sendet diese Logik-App Ihnen eine tägliche Zusammenfassung des RSS-Feeds einer Website. Die App verwendet eine „ForEach“-Schleife, die für jedes neu gefundene Element eine E-Mail sendet.

1. [Erstellen Sie diese Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) mit einem Outlook.com- oder Office 365 Outlook-Konto.

2. Fügen Sie zwischen dem RSS-Trigger und der E-Mail-Sendeaktion eine „ForEach“-Schleife hinzu. 

   Um eine Schleife zwischen Schritten hinzuzufügen, bewegen Sie den Zeiger auf den Pfeil an der Position, an der Sie die Schleife hinzufügen möchten. 
   Wählen Sie das angezeigte **Pluszeichen** (**+**) und dann **„ForEach“-Schleife hinzufügen** aus.

   ![„ForEach“-Schleife zwischen den Schritten hinzufügen](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Erstellen Sie jetzt die Schleife. Wählen Sie unter **Ausgabe von vorherigen Schritten auswählen**, nachdem die Liste **Dynamischen Inhalt hinzufügen** angezeigt wird, das **Feed links**-Array aus, das vom RSS-Trigger ausgegeben wird. 

   ![Aus Liste mit dynamischen Inhalten auswählen](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Sie können *nur* Arrayausgaben aus dem vorherigen Schritt auswählen.

   Das ausgewählte Array wird nun hier angezeigt:

   ![Array auswählen](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Um für jedes Arrayelement eine Aktion auszuführen, ziehen Sie die **E-Mail senden**-Aktion in die **„ForEach“-Schleife**. 

   Ihre Logik-App sieht in etwa wie im folgenden Beispiel aus:

   ![Schritte für „ForEach“-Schleife hinzufügen](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie in der Symbolleiste des Designers die Option **Ausführen**, um Ihre Logik-App manuell zu testen.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definition der „ForEach“-Schleife (JSON)

Wenn Sie in der Codeansicht für Ihre Logik-App arbeiten, können Sie die `Foreach`-Schleife stattdessen in der JSON-Definition der Logik-App definieren, z.B.:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {
                "type": "ApiConnection",
                "inputs": {
                    "body": {
                        "Body": "@{item()}",
                        "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                        "To": "me@contoso.com"
                    },
                    "host": {
                        "api": {
                            "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                        },
                        "connection": {
                            "name": "@parameters('$connections')['office365']['connectionId']"
                        }
                    },
                    "method": "post",
                    "path": "/Mail"
                },
                "runAfter": {}
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
    }
},
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>„ForEach“-Schleife: sequenziell

Standardmäßig wird jeder Zyklus in einer „ForEach“-Schleife für jedes Arrayelement parallel ausgeführt. Um jeden Zyklus sequenziell auszuführen, legen Sie die Option **Sequenziell** in Ihrer „ForEach“-Schleife fest.

1. Wählen Sie in der oberen rechten Ecke der Schleife **Auslassungspunkte** (**...** ) > **Einstellungen**.

   ![In „ForEach“-Schleife „...“ > „Einstellungen“ auswählen](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Aktivieren Sie die Einstellung **Sequenziell**, und wählen Sie dann **Fertig** aus.

   ![Einstellung „Sequenziell“ der „ForEach“-Schleife aktivieren](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Sie können auch den **operationOptions**-Parameter in der JSON-Definition Ihrer Logik-App auf `Sequential` festlegen. Beispiel: 

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {               
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
        "operationOptions": "Sequential"
    }
},
```

<a name="until-loop"></a>

## <a name="until-loop"></a>„Until“-Schleife
  
Um Aktionen zu wiederholen, bis eine Bedingung erfüllt ist oder ein Status sich geändert hat, verwenden Sie eine „Until“-Schleife in Ihrem Logik-App-Workflow. Hier sind einige gängige Verwendungsbeispiele für eine „Until“-Schleife:

* Aufrufen eines Endpunkts, bis Sie die gewünschte Antwort erhalten.
* Erstellen eines Datensatzes in einer Datenbank, Warten, bis ein bestimmtes Feld in diesem Datensatz genehmigt ist, und Fortsetzen der Verarbeitung. 

Diese Logik-App inkrementiert z.B. täglich um 8:00 Uhr eine Variable, bis diese den Wert 10 hat. Dann sendet die Logik-App eine E-Mail, die den aktuellen Wert bestätigt. Obwohl in diesem Beispiel Outlook für Office 365 verwendet wird, können Sie alle von Logic Apps unterstützten E-Mail-Anbieter verwenden ([lesen Sie diese Connectorsliste](https://docs.microsoft.com/connectors/)). Bei Verwendung eines anderen E-Mail-Kontos sind die Schritte im Großen und Ganzen identisch, aber die Benutzeroberfläche weicht ggf. etwas ab. 

1. Erstellen einer leeren Logik-App Suchen Sie im Logik-App-Designer nach „Wiederholung“, und wählen Sie diesen Trigger aus: **Zeitplan: Wiederholung**. 

   ![Trigger „Zeitplan: Wiederholung“ hinzufügen](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Geben Sie an, wann der Trigger ausgelöst wird, indem Sie das Intervall, die Häufigkeit und die Stunde des Tages festlegen. Um die Stunde festzulegen, wählen Sie **Erweiterte Optionen anzeigen** aus.

   ![Trigger „Zeitplan: Wiederholung“ hinzufügen](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Eigenschaft | Wert |
   | -------- | ----- |
   | **Intervall** | 1 | 
   | **Frequency** | Day (Tag) |
   | **Zu diesen Stunden** | 8 |
   ||| 

3. Wählen Sie unter dem Trigger **Neuer Schritt** > **Aktion hinzufügen** aus. Suchen Sie nach „Variablen“, und wählen Sie dann die folgende Aktion aus: **Variablen – Variable initialisieren**.

   ![Aktion „Variablen – Variable initialisieren“ hinzufügen](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Richten Sie die Variable mit den folgenden Werten ein:

   ![Variableneigenschaften festlegen](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   | -------- | ----- | ----------- |
   | **Name** | Begrenzung | Name Ihrer Variablen | 
   | **Typ** | Ganze Zahl  | Datentyp Ihrer Variablen | 
   | **Wert** | 0 | Startwert Ihrer Variablen | 
   |||| 

5. Wählen Sie unter der Aktion **Variable initialisieren** die Option **Neuer Schritt** > **Weitere** aus. Wählen Sie diese Schleife aus: **Wiederholen bis „“ hinzufügen**.

   ![„Wiederholen bis“-Schleife hinzufügen](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Erstellen Sie die Beendigungsbedingung der Schleife durch Auswahl der **Limit**-Variablen und des **ist gleich**-Operators. Geben Sie **10** als Vergleichswert ein.

   ![Beendigungsbedingung zum Anhalten der Schleife erstellen](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Wählen Sie in der Schleife **Aktion hinzufügen**. Suchen Sie nach „Variablen“, und fügen Sie dann die folgende Aktion hinzu: **Variablen – Variable inkrementieren**.

   ![Aktion zum Inkrementieren der Variablen hinzufügen](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Wählen Sie für **Name** die **Limit**-Variable. Geben Sie für **Wert** „1“ ein. 

   ![„Limit“ um 1 inkrementieren](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Fügen Sie unter, aber außerhalb der Schleife eine Aktion hinzu, die E-Mail sendet. Melden Sie sich nach Aufforderung bei Ihrem E-Mail-Konto an.

   ![Aktion hinzufügen, die E-Mail sendet](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Legen Sie die E-Mail-Eigenschaften fest. Fügen Sie die **Limit**-Variable dem Betreff hinzu. Auf diese Weise können Sie sicherstellen, dass der aktuelle Wert der Variablen die angegebene Bedingung erfüllt, beispielsweise:

    ![E-Mail-Eigenschaften einrichten](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Eigenschaft | Wert | BESCHREIBUNG |
    | -------- | ----- | ----------- | 
    | **An** | *<email-address@domain>* | Die E-Mail-Adresse des Empfängers. Geben Sie zum Testen Ihre eigene E-Mail-Adresse an. | 
    | **Betreff** | Aktueller Wert für „Limit“ ist **Limit** | Legen Sie den E-Mail-Betreff fest. Stellen Sie in diesem Beispiel sicher, dass Sie die **Limit**-Variable einbeziehen. | 
    | **Text** | <*E-Mail-Inhalt*> | Geben Sie den Inhalt der E-Mail-Nachricht an, die Sie senden möchten. In diesem Beispiel können Sie beliebigen Text eingeben. | 
    |||| 

11. Speichern Sie Ihre Logik-App. Wählen Sie in der Symbolleiste des Designers die Option **Ausführen**, um Ihre Logik-App manuell zu testen.

    Sobald Ihre Logik ausgeführt wird, erhalten Sie eine E-Mail mit dem Inhalt, den Sie angegeben:

    ![Empfangene E-Mail](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Verhindern von Endlosschleifen

Standardeinschränkungen beenden die Ausführung einer „Until“-Schleife, wenn eine dieser Bedingungen vorliegt:

| Eigenschaft | Standardwert | BESCHREIBUNG | 
| -------- | ------------- | ----------- | 
| **Count** | 60 | Die maximale Anzahl von Schleifen, die ausgeführt werden, bevor die Schleife beendet wird. Der Standardwert sind 60 Zyklen. | 
| **Timeout** | PT1H | Der maximale Zeitraum, für den eine Schleife ausgeführt wird, bevor sie beendet wird. Der Standardwert beträgt eine Stunde und wird im ISO 8601-Format angegeben. <p>Der Timeoutwert wird für jeden Schleifendurchlauf ausgewertet. Wenn die Ausführungsdauer einer Aktion in der Schleife das Timeoutlimit überschreitet, wird der aktuelle Zyklus nicht beendet, doch der nächste Zyklus beginnt nicht, da die Limitbedingung nicht erfüllt ist. | 
|||| 

Um diese Standardlimits zu ändern, wählen Sie **Erweiterte Optionen anzeigen** in der Schleifenaktionsform.

<a name="until-json"></a>

## <a name="until-definition-json"></a>„Until“-Definition (JSON)

Wenn Sie in der Codeansicht für Ihre Logik-App arbeiten, können Sie eine `Until`-Schleife stattdessen in der JSON-Definition der Logik-App definieren, z.B.:

``` json
"actions": {
    "Initialize_variable": {
        // Definition for initialize variable action
    },
    "Send_an_email": {
        // Definition for send email action
    },
    "Until": {
        "type": "Until",
        "actions": {
            "Increment_variable": {
                "type": "IncrementVariable",
                "inputs": {
                    "name": "Limit",
                    "value": 1
                },
                "runAfter": {}
            }
        },
        "expression": "@equals(variables('Limit'), 10)",
        // To prevent endless loops, an "Until" loop 
        // includes these default limits that stop the loop. 
        "limit": { 
            "count": 60,
            "timeout": "PT1H"
        },
        "runAfter": {
            "Initialize_variable": [
                "Succeeded"
            ]
        },
    }
},
```

In einem anderen Beispiel ruft diese „Until“-Schleife einen HTTP-Endpunkt auf, der eine Ressource erstellt und beendet, wenn der HTTP-Antworttext den Status „Abgeschlossen“ zurückgibt. Um Endlosschleifen zu verhindern, wird die Schleife auch beendet, wenn eine dieser Bedingungen vorliegt:

* Die Schleife wurde wie im `count`-Attribut angegeben 10-mal ausgeführt. Der Standardwert ist 60-mal. 
* Es wurde gemäß `timeout`-Attribut im ISO 8601-Format zwei Stunden lang versucht, die Schleife auszuführen. Der Standardwert ist eine Stunde.
  
``` json
"actions": {
    "myUntilLoopName": {
        "type": "Until",
        "actions": {
            "Create_new_resource": {
                "type": "Http",
                "inputs": {
                    "body": {
                        "resourceId": "@triggerBody()"
                    },
                    "url": "https://domain.com/provisionResource/create-resource",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                },
                "runAfter": {},
                "type": "ApiConnection"
            }
        },
        "expression": "@equals(triggerBody(), 'Completed')",
        "limit": {
            "count": 10,
            "timeout": "PT2H"
        },
        "runAfter": {}
    }
},
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)

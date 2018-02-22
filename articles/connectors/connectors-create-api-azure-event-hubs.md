---
title: "Einrichten der Ereignisüberwachung mit Azure Event Hubs für Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen von Datenströmen zum Empfangen und Senden von Ereignissen mit Ihren Logik-Apps mit Azure Event Hubs"
services: logic-apps
keywords: "Datenstrom, Ereignisüberwachung, Event Hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 076f7dd11ca8c153046727861ecb755e88f32b01
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Überwachen, Empfangen und Senden von Ereignissen mit dem Event Hub-Connector

Um eine Ereignisüberwachung so einzurichten, dass Ihre Logik-App Ereignisse erkennen, empfangen und senden kann, stellen Sie über Ihre Logik-App eine Verbindung zu einem [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) her. Erfahren Sie mehr über [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) und die [Preisgestaltung für Logic Apps-Connectors](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Event Hubs-Connector verwenden können, müssen Sie über diese Elemente verfügen:

* Einen [Azure Event Hubs-Namespace und Event Hub](../event-hubs/event-hubs-create.md)
* Eine [Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-event-hubs"></a>Herstellen einer Verbindung mit Azure Event Hubs

Damit Ihre Logik-App auf einen Dienst zugreifen kann, müssen Sie eine [*Verbindung*](./connectors-overview.md) zwischen Ihrer Logik-App und dem Dienst erstellen, sofern dies noch nicht geschehen ist. Diese Verbindung erlaubt Ihrer Logik-App den Zugriff auf Daten. Damit Ihre Logik-App auf Ihren Event Hub zugreifen kann, überprüfen Sie Ihre Berechtigungen, und rufen Sie die Verbindungszeichenfolge für Ihren Event Hubs-Namespace ab.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an. 

2.  Navigieren Sie zu Ihrem Event Hubs-*Namespace*, jedoch nicht zum eigentlichen Event Hub. Wählen Sie auf der Seite „Namespace“ unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien**. Stellen Sie im Bereich **Ansprüche** sicher, dass Sie die **Verwaltungsberechtigungen** für diesen Namespace besitzen.

    ![Verwalten von Berechtigungen für Ihren Event Hub-Namespace](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Wenn Sie die Verbindungsinformationen später manuell eingeben möchten, rufen Sie die Verbindungszeichenfolge für Ihren Event Hubs-Namespace ab. Wählen Sie **RootManageSharedAccessKey**. Wählen Sie neben der Verbindungszeichenfolge Ihres Primärschlüssels die Schaltfläche „Kopieren“. Speichern Sie die Verbindungszeichenfolge für die spätere Verwendung.

    ![Kopieren der Verbindungszeichenfolge für Event Hub-Namespaces](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Um zu überprüfen, ob Ihre Verbindungszeichenfolge mit Ihrem Event Hubs-Namespace oder einem bestimmten Event Hub verknüpft ist, überprüfen Sie die Verbindungszeichenfolge für den Parameter `EntityPath`. Wenn Sie diesen Parameter gefunden haben, gilt die Verbindungszeichenfolge für eine bestimmte Event Hub-Entität und stellt nicht die richtige Zeichenfolge für die Verwendung mit Ihrer Logik-App dar.

## <a name="trigger-workflow-when-your-event-hub-gets-new-events"></a>Auslösen des Workflows, wenn Ihr Event Hub neue Ereignisse abruft

Ein [*Trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Ereignis, das einen Workflow in Ihrer Logik-App startet. Um einen Workflow zu starten, wenn neue Ereignisse an Ihren Event Hub gesendet werden, führen Sie diese Schritte zum Hinzufügen des Triggers durch, der dieses Ereignis erkennt.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com "Azure-Portal") zu Ihrer vorhandenen Logik-App oder erstellen Sie eine leere Logik-App.

2. Geben Sie im Designer für Logik-Apps „Event Hubs“ als Filter in das Suchfeld ein. Wählen Sie folgenden Trigger aus: **Wenn Ereignisse im Event Hub verfügbar sind**

   ![Auswählen des Triggers beim Empfangen neuer Ereignisse durch Ihren Event Hub](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

   1. Wenn noch keine Verbindung zu Ihrem Event Hub-Namespace besteht, werden Sie aufgefordert, diese Verbindung nun zu erstellen. Benennen Sie Ihre Verbindung, und wählen Sie den Event Hubs-Namespace, den Sie verwenden möchten.

      ![Herstellen einer Event Hub-Verbindung](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

      Wenn Sie die Verbindungszeichenfolge manuell eingeben möchten, wählen Sie **Verbindungsinformationen manuell eingeben**. 
      Erfahren Sie, [wie Sie Ihre Verbindungszeichenfolge suchen](#permissions-connection-string).

   2. Wählen Sie jetzt die zu verwendende Event Hubs-Richtlinie aus, und wählen Sie **Erstellen**.

      ![Herstellen der Event Hub-Verbindung, Teil 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Wählen Sie den zu überwachenden Event Hub aus, und richten Sie Intervall und Häufigkeit der Event Hub-Überprüfung ein.

    ![Festlegen von Event Hubs oder Consumergruppen](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

    > [!TIP]
    > Um optional eine Consumergruppe zum Lesen von Ereignissen auszuwählen, wählen Sie **Erweiterte Optionen anzeigen**.

4. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Wenn Ihre Logik-App jetzt den ausgewählten Event Hub überprüft und ein neues Ereignis findet, führt der Trigger die Aktionen für das gefundene Ereignis in der Logik-App aus.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Senden von Ereignissen über Ihre Logik-App an Ihren Event Hub

Eine [*Aktion*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist eine Aufgabe, die von Ihrem Logik-App-Workflow ausgeführt wird. Nachdem Sie Ihrer Logik-App einen Trigger hinzugefügt haben, können Sie eine Aktion zum Ausführen von Vorgängen mit Daten hinzufügen, die von diesem Trigger generiert wurden. Um über Ihre Logik-App ein Ereignis an Ihren Event Hub zu senden, führen Sie folgende Schritte durch.

1. Wählen Sie im Designer für Logik-Apps unter dem Trigger **Neuer Schritt** > **Aktion hinzufügen**.

2. Geben Sie im Suchfeld „Event Hubs“ als Filter ein.
Wählen Sie diese Aktion aus: **Event Hubs – Ereignis senden**.

   ![„Event Hubs – Ereignis senden“ auswählen](./media/connectors-create-api-azure-event-hubs/select-event-hubs-send-event-action.png)

3. Wählen Sie den Event Hub aus, für den das Ereignis gesendet werden soll. Geben Sie dann den Ereignisinhalt und andere Details ein.

   ![Event Hub-Namen auswählen und Ereignisinhalt bereitstellen](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

4. Speichern Sie Ihre Logik-App.

Sie haben nun eine Aktion zum Senden von Ereignissen über Ihre Logik-App eingerichtet. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Weitere Informationen zu Triggern und Aktionen gemäß Definition in der Swagger-Datei und etwaige Grenzwerte finden Sie in den [Connectordetails](/connectors/eventhubs/).

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [andere Connectors für Azure Logic Apps](../connectors/apis-list.md).
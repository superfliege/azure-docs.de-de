---
title: "Einrichten des Messaging mit Azure Service Bus für Azure Logic Apps | Microsoft-Dokumentation"
description: Senden und Empfangen von Nachrichten mit Ihren Logik-Apps mithilfe von Azure Service Bus
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 02/06/2018
ms.author: ladocs
ms.openlocfilehash: e81580db17610adc6be534c9801881f9b68b14fd
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="send-and-receive-messages-with-the-azure-service-bus-connector"></a>Senden und Empfangen von Nachrichten mit dem Azure Service Bus-Connector

Um Nachrichten mit Ihrer Logik-App zu senden und zu empfangen, stellen Sie eine Verbindung mit [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) her. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus einer Warteschlange und Empfangen aus Abonnements usw. Erfahren Sie mehr über [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) und die [Preisgestaltung für Logic Apps-Trigger](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Service Bus-Connector verwenden können, müssen Sie über diese Elemente verfügen, die im gleichen Azure-Abonnement vorhanden sein müssen, damit sie füreinander sichtbar sind:

* Ein [Service Bus-Namespace und eine Messagingentität, z.B. eine Warteschlange](../service-bus-messaging/service-bus-create-namespace-portal.md)
* Eine [Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-service-bus"></a>Herstellen der Verbindung mit Azure Service Bus

Damit Ihre Logik-App auf einen Dienst zugreifen kann, müssen Sie eine [*Verbindung*](./connectors-overview.md) zwischen Ihrer Logik-App und dem Dienst erstellen, sofern dies noch nicht geschehen ist. Diese Verbindung erlaubt Ihrer Logik-App den Zugriff auf Daten. Überprüfen Sie, ob Ihre Berechtigungen den Zugriff Ihrer Logik-App auf Ihr Service Bus-Konto erlauben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an. 

2. Wechseln Sie zu Ihrem Service Bus-*Namespace*, nicht zu einer bestimmten „Messagingentität“. Wählen Sie auf der Seite „Namespace“ unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien**. Stellen Sie im Bereich **Ansprüche** sicher, dass Sie die **Verwaltungsberechtigungen** für diesen Namespace besitzen.

   ![Verwalten von Berechtigungen für Ihren Service Bus-Namespace](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Wenn Sie die Verbindungsinformationen später manuell eingeben möchten, rufen Sie die Verbindungszeichenfolge für Ihren Service Bus-Namespace ab. Wählen Sie **RootManageSharedAccessKey**. Wählen Sie neben der Verbindungszeichenfolge Ihres Primärschlüssels die Schaltfläche „Kopieren“. Speichern Sie die Verbindungszeichenfolge für die spätere Verwendung.

   ![Kopieren der Verbindungszeichenfolge für Service Bus-Namespaces](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Um zu überprüfen, ob Ihre Verbindungszeichenfolge mit Ihrem Service Bus-Namespace oder einer bestimmten Entität verknüpft ist, überprüfen Sie die Verbindungszeichenfolge für den Parameter `EntityPath`. Wenn Sie diesen Parameter gefunden haben, gilt die Verbindungszeichenfolge für eine bestimmte Entität und stellt nicht die richtige Zeichenfolge für die Verwendung mit Ihrer Logik-App dar.

## <a name="trigger-workflow-when-your-service-bus-gets-new-messages"></a>Auslösen des Workflows, wenn Ihr Service Bus neue Nachrichten erhält

Ein [*Trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Ereignis, das einen Workflow in Ihrer Logik-App startet. Um einen Workflow zu starten, wenn neue Nachrichten an Ihren Service Bus gesendet werden, führen Sie diese Schritte zum Hinzufügen des Triggers durch, der diese Nachrichten erkennt.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com "Azure-Portal") zu Ihrer vorhandenen Logik-App oder erstellen Sie eine leere Logik-App.

2. Geben Sie im Designer für Logik-Apps „Service Bus“ als Filter in das Suchfeld ein. Wählen Sie den **Service Bus**-Connector aus. 

   ![Service Bus-Connector auswählen](./media/connectors-create-api-azure-service-bus/select-service-bus-connector.png) 

3. Wählen Sie den zu verwendenden Trigger aus. Um z.B. eine Logik-App auszuführen, wenn ein neues Element an eine Service Bus-Warteschlange gesendet wird, wählen Sie diesen Trigger: **Service Bus – Wenn eine Nachricht in einer Warteschlange empfangen wird (automatisch abschließen)**.

   ![Service Bus-Trigger auswählen](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   1. Wenn noch keine Verbindung zu Ihrem Service Bus-Namespace besteht, werden Sie aufgefordert, diese Verbindung nun zu erstellen. Benennen Sie Ihre Verbindung, und wählen Sie den Service Bus-Namespace, den Sie verwenden möchten.

      ![Service Bus-Verbindung herstellen](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Wenn Sie die Verbindungszeichenfolge manuell eingeben möchten, wählen Sie **Verbindungsinformationen manuell eingeben**. 
      Erfahren Sie, [wie Sie Ihre Verbindungszeichenfolge suchen](#permissions-connection-string).

   2. Wählen Sie jetzt die zu verwendende Service Bus-Richtlinie aus, und wählen Sie **Erstellen**.

      ![Service Bus-Verbindung herstellen, Teil 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

4. Wählen Sie die zu verwendende Service Bus-Warteschlange aus, und richten Sie Intervall und Häufigkeit der Warteschlangenüberprüfung ein.

   ![Service Bus-Warteschlange auswählen, Abrufintervall einrichten](./media/connectors-create-api-azure-service-bus/select-service-bus-queue.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Wenn Ihre Logik-App jetzt die ausgewählte Warteschlange überprüft und eine neue Nachricht findet, führt der Trigger die Aktionen für die gefundene Nachricht in der Logik-App aus.

## <a name="send-messages-from-your-logic-app-to-your-service-bus"></a>Senden von Nachrichten von der Logik-App an Ihren Service Bus

Eine [*Aktion*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist eine Aufgabe, die von Ihrem Logik-App-Workflow ausgeführt wird. Nachdem Sie Ihrer Logik-App einen Trigger hinzugefügt haben, können Sie eine Aktion zum Ausführen von Vorgängen mit Daten hinzufügen, die von diesem Trigger generiert wurden. Um eine Nachricht aus Ihrer Logik-App an Ihre Service Bus-Messagingentität zu senden, befolgen Sie diese Schritte.

1. Wählen Sie im Designer für Logik-Apps unter dem Trigger **Neuer Schritt** > **Aktion hinzufügen**.

2. Geben Sie im Suchfeld den Begriff „Service Bus“ als Filter ein. Wählen Sie diesen Connector aus: **Service Bus**.

   ![Connector „Service Bus“ auswählen](./media/connectors-create-api-azure-service-bus/select-service-bus-connector-for-action.png) 

3. Wählen Sie diese Aktion aus: **Service Bus – Nachricht senden**.

   ![„Service Bus – Nachricht senden“ auswählen](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png)

4. Wählen Sie die Messagingentität aus, die die Warteschlange oder ein Themenname ist, wohin die Nachricht gesendet werden soll. Geben Sie dann den Nachrichteninhalt und andere Details ein.

   ![Messagingentität auswählen und Nachrichtendetails angeben](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)    

5. Speichern Sie Ihre Logik-App. 

Sie haben nun eine Aktion zum Senden von Nachrichten über Ihre Logik-App eingerichtet. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Weitere Informationen zu Triggern und Aktionen gemäß Definition in der Swagger-Datei und etwaige Grenzwerte finden Sie in den [Connectordetails](/connectors/servicebus/).

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [andere Connectors für Azure Logic Apps](../connectors/apis-list.md).
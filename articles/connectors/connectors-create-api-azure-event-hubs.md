---
title: Herstellen einer Verbindung mit Azure Event Hubs – Azure Logic Apps | Microsoft-Dokumentation
description: Verwalten und Überwachen von Ereignissen mit Azure Event Hubs und Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 86fc1c3542bea1be840041bb73df15631c066c7e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294971"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Überwachen, Erhalten und Senden von Ereignissen mit Azure Event Hubs und Azure Logic Apps 

In diesem Artikel wird gezeigt, wie Sie Ereignisse überwachen und verwalten können, die mit dem Azure Event Hubs-Connector aus einer Logik-App heraus an [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) gesendet werden. Auf diese Weise können Sie Logik-Apps erstellen, die Aufgaben und Workflows zum Überprüfen, Senden und Empfangen von Ereignissen aus Ihrem Event Hub automatisieren. 

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. Wenn Sie noch nicht mit Logik-Apps vertraut sind, lesen Sie [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten Logik-App-Workflows](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Connectorspezifische technische Informationen finden Sie in der <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">Referenz zum Azure Event Hubs-Connector</a>.

## <a name="prerequisites"></a>Voraussetzungen

* Einen [Azure Event Hubs-Namespace und Event Hub](../event-hubs/event-hubs-create.md)

* Die Logik-App, in der Sie auf Ihren Event Hub zugreifen möchten. Um Ihre Logik-App mit einem Azure Event Hubs-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>Überprüfen der Berechtigungen und Abrufen der Verbindungszeichenfolge

Damit Ihre Logik-App auf Ihren Event Hub zugreifen kann, überprüfen Sie Ihre Berechtigungen, und rufen Sie die Verbindungszeichenfolge für Ihren Event Hubs-Namespace ab.

1. Melden Sie sich beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an. 

2. Navigieren Sie zu Ihrem Event Hubs-*Namespace*, jedoch nicht zum eigentlichen Event Hub. Wählen Sie auf der Seite „Namespace“ unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien**. Stellen Sie im Bereich **Ansprüche** sicher, dass Sie die **Verwaltungsberechtigungen** für diesen Namespace besitzen.

   ![Verwalten von Berechtigungen für Ihren Event Hub-Namespace](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Wenn Sie die Verbindungsinformationen später manuell eingeben möchten, rufen Sie die Verbindungszeichenfolge für Ihren Event Hubs-Namespace ab. 

   1. Wählen Sie unter **Richtlinie** die Option **RootManageSharedAccessKey**. 

   2. Suchen Sie die Verbindungszeichenfolge des Primärschlüssels. Wählen Sie die Schaltfläche „Kopieren“, und speichern Sie die Verbindungszeichenfolge zur späteren Verwendung.

      ![Kopieren der Verbindungszeichenfolge für Event Hub-Namespaces](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Um zu überprüfen, ob Ihre Verbindungszeichenfolge Ihrem Event Hubs-Namespace oder einem bestimmten Event Hub zugeordnet ist, stellen Sie sicher, dass die Verbindungszeichenfolge nicht den Parameter `EntityPath` aufweist. Wenn Sie diesen Parameter gefunden haben, gilt die Verbindungszeichenfolge für eine bestimmte Event Hub-Entität und stellt nicht die richtige Zeichenfolge für die Verwendung mit Ihrer Logik-App dar.

4. Fahren Sie jetzt mit [Hinzufügen eines Event Hubs-Triggers](#add-trigger) oder [Hinzufügen einer Event Hubs-Aktion](#add-action) fort.

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Hinzufügen eines Event Hubs-Triggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer App.

Dieses Beispiel zeigt, wie Sie einen Logik-App-Workflow starten können, wenn neue Ereignisse an Ihren Event Hub gesendet werden. 

1. Erstellen Sie im Azure-Portal oder in Visual Studio eine leere Logik-App, die den Logic Apps-Designer öffnet. In diesem Beispiel wird das Azure-Portal verwendet.

2. Geben Sie im Suchfeld „Event Hubs“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten Trigger aus. 

   Dieses Beispiel verwendet diesen Trigger: **Event Hubs – Wenn Ereignisse im Event Hub verfügbar sind**.

   ![Trigger auswählen](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, [erstellen Sie jetzt Ihre Event Hubs-Verbindung](#create-connection). Falls die Verbindung bereits besteht, können Sie die erforderlichen Informationen für den Trigger angeben.

   1. Wählen Sie in der Liste **Event Hub-Name** den Event Hub aus, den Sie überwachen möchten.

      ![Festlegen von Event Hubs oder Consumergruppen](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Wählen Sie das Intervall und die Häufigkeit aus, um anzugeben, wie oft der Trigger den Event Hub überprüfen soll.
 
   3. Wenn Sie optional einige erweiterte Triggeroptionen auswählen möchten, wählen Sie **Erweiterte Optionen anzeigen** aus.
   
      ![Erweiterte Triggeroptionen](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Eigenschaft | Details | 
      |----------|---------| 
      | Content-Typ  | Wählen Sie den Inhaltstyp des Ereignisses aus. Der Standardwert lautet „application/octet-stream“. |
      | Inhaltsschema | Geben Sie das Inhaltsschema im JSON-Format für die Ereignisse an, die vom Event Hub gelesen werden. |
      | Name der Consumergruppe | Geben Sie den [Namen der Consumergruppe](../event-hubs/event-hubs-features.md#consumer-groups) des Event Hubs zum Lesen von Ereignissen ein. Wenn Sie hier nichts angeben, wird die Standardconsumergruppe verwendet. |
      | Partitionsschlüssel (niedrigster Wert) | Geben Sie die niedrigste [Partitions](../event-hubs/event-hubs-features.md#partitions)-ID ein, die gelesen werden soll. Standardmäßig werden alle Partitionen gelesen. |
      | Partitionsschlüssel (höchster Wert) | Geben Sie die höchste [Partitions](../event-hubs/event-hubs-features.md#partitions)-ID ein, die gelesen werden soll. Standardmäßig werden alle Partitionen gelesen. |
      | Höchstanzahl von Ereignissen | Geben Sie einen Wert für die Höchstanzahl von Ereignissen ein. Der Trigger gibt einen Wert zwischen 1 und der durch diese Eigenschaft definierten Anzahl von Ereignissen zurück. |
      |||

4. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

5. Fahren Sie nun damit fort, der Logik-App weitere Aktionen für die Aufgaben hinzuzufügen, die anhand der Triggerergebnisse durchgeführt werden sollen.

> [!NOTE]
> Alle Event Hub-Trigger sind Trigger mit *langem Abruf*. Das bedeutet, dass ein Trigger beim Auslösen alle Ereignisse verarbeitet und dann 30 Sekunden lang auf weitere im Event Hub eingehende Ereignisse wartet.
> Werden innerhalb von 30 Sekunden keine Ereignisse empfangen, wird die Triggerausführung übersprungen. Andernfalls setzt der Trigger das Lesen von Ereignissen fort, bis Ihr Event Hub leer ist.
> Der nächste Triggerabruf erfolgt basierend auf dem in den Triggereigenschaften angegebenen Wiederholungsintervall.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Hinzufügen einer Event Hubs-Aktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. In diesem Beispiel startet die Logik-App mit einem Event Hubs-Trigger, der Ihren Event Hub auf neue Ereignisse überprüft. 

1. Öffnen Sie im Azure-Portal oder in Visual Studio Ihre Logik-App im Logic Apps-Designer. In diesem Beispiel wird das Azure-Portal verwendet.

2. Wählen Sie unter dem Trigger oder der Aktion **Neuer Schritt** > **Aktion hinzufügen** aus.

   Um eine Aktion zwischen vorhandenen Schritten hinzuzufügen, bewegen Sie den Mauszeiger über den Verbindungspfeil. 
   Wählen Sie das daraufhin angezeigte Pluszeichen (**+**) aus, und wählen Sie dann **Aktion hinzufügen** aus.

3. Geben Sie im Suchfeld „Event Hubs“ als Filter ein.
Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus. 

   Wählen Sie für dieses Beispiel diese Aktion aus: **Event Hubs – Ereignis senden**.

   ![„Event Hubs – Ereignis senden“ auswählen](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, [erstellen Sie jetzt Ihre Event Hubs-Verbindung](#create-connection). Falls Ihre Verbindung bereits besteht, können Sie die erforderlichen Informationen für die Aktion angeben.

   | Eigenschaft | Erforderlich | BESCHREIBUNG | 
   |----------|----------|-------------|
   | Event Hub-Name | Ja | Wählen Sie den Event Hub aus, an den das Ereignis gesendet werden soll. | 
   | Ereignisinhalt | Nein  | Der Inhalt für das Ereignis, das Sie senden möchten | 
   | Eigenschaften | Nein  | Die App-Eigenschaften und Werte, die gesendet werden sollen | 
   |||| 

   Beispiel:  

   ![Event Hub-Namen auswählen und Ereignisinhalt bereitstellen](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Herstellen einer Event Hub-Verbindung

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Wenn Sie zur Eingabe von Verbindungsinformationen aufgefordert werden, geben Sie diese Details an:

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG | 
   |----------|----------|-------|-------------|
   | Verbindungsname | Ja | <*verbindungsname*> | Der Name, der für Ihre Verbindung erstellt werden soll |
   | Event Hubs-Namespace | Ja | <*event-hubs-namespace*> | Wählen Sie den Event Hubs-Namespace, den Sie verwenden möchten. | 
   |||||  

   Beispiel: 

   ![Herstellen einer Event Hub-Verbindung](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Wenn Sie die Verbindungszeichenfolge manuell eingeben möchten, wählen Sie **Verbindungsinformationen manuell eingeben**. 
   Erfahren Sie, [wie Sie Ihre Verbindungszeichenfolge suchen](#permissions-connection-string).

2. Wählen Sie ggf. die zu verwendende Event Hubs-Richtlinie aus. Wählen Sie **Erstellen**.

   ![Herstellen der Event Hub-Verbindung, Teil 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Nachdem Sie die Verbindung erstellt haben, fahren Sie mit [Hinzufügen eines Event Hubs-Triggers](#add-trigger) oder [Hinzufügen einer Event Hubs-Aktion](#add-action) fort.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details, z.B. Trigger, Aktionen und Grenzwerte, wie sie in der Swagger-Datei des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/eventhubs/). 

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
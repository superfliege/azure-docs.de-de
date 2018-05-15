---
title: Integration von Azure Event Grid und Event Hubs
description: Hier wird erläutert, wie Daten mithilfe von Azure Event Grid und Event Hubs zu einem SQL Data Warehouse migriert werden.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 60857327685fca9a5f97588ab51909ce2537d68f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Streamen von Big Data in ein Data Warehouse

Bei Azure [Event Grid](overview.md) handelt es sich um einen intelligenten Ereignisroutingdienst, der es Ihnen ermöglicht, auf Benachrichtigungen von Apps und Diensten zu reagieren. Das [Beispiel für Event Hubs Capture und Event Grid](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) veranschaulicht, wie Sie Azure Event Hubs Capture mit Azure Event Grid für die nahtlose Migration von Daten von einem Event Hub zu einem SQL Data Warehouse verwenden.

![Anwendungsübersicht](media/event-grid-event-hubs-integration/overview.png)

Wenn Daten an den Event Hub gesendet werden, ruft Capture per Pull Daten aus dem Stream ab und generiert Speicherblobs mit den Daten im Avro-Format. Beim Erstellen des Blobs durch Capture wird ein Ereignis ausgelöst. Event Grid verteilt Daten zum Ereignis an Abonnenten. In diesem Fall werden die Ereignisdaten an den Azure Functions-Endpunkt gesendet. Die Ereignisdaten enthalten den Pfad des erstellten Blobs. Die Funktion ruft die Datei mithilfe dieser URL ab und sendet sie an das Data Warehouse.

In diesem Artikel führen Sie folgende Schritte aus:

* Bereitstellen der folgenden Infrastruktur:
  * Event Hub, für den Capture aktiviert ist
  * Speicherkonto für die Dateien aus Capture
  * Azure App Service-Tarif zum Hosten der Funktions-App
  * Funktions-App zum Verarbeiten des Ereignisses
  * SQL Server zum Hosten des Data Warehouse
  * SQL Data Warehouse zum Speichern der migrierten Daten
* Erstellen einer Tabelle im Data Warehouse
* Hinzufügen von Code zur Funktions-App
* Abonnieren des Ereignisses
* Ausführen der App, die Daten an den Event Hub sendet
* Anzeigen von migrierten Daten im Data Warehouse

## <a name="about-the-event-data"></a>Informationen zu den Ereignisdaten

Event Grid verteilt Ereignisdaten an die Abonnenten. Das folgende Beispiel zeigt Ereignisdaten zum Erstellen einer Capture-Datei. Beachten Sie insbesondere die `fileUrl`-Eigenschaft im `data`-Objekt. Die Funktions-App ruft diesen Wert ab und verwendet ihn zum Abrufen der Capture-Datei.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* [Visual Studio 2017 Version 15.3.2 oder höher](https://www.visualstudio.com/vs/) mit Workloads für .NET-Desktopentwicklung, Azure-Entwicklung, ASP.NET- und Webentwicklung, Node.js-Entwicklung und Python-Entwicklung
* Das auf Ihren Computer heruntergeladene [EventHubsCaptureEventGridDemo-Beispielprojekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo)

## <a name="deploy-the-infrastructure"></a>Bereitstellen der Infrastruktur

Zur Vereinfachung dieses Artikels stellen Sie die erforderliche Infrastruktur mit einer Resource Manager-Vorlage bereit. Sehen Sie sich die [Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json) an, um zu ermitteln, welche Ressourcen bereitgestellt werden. Verwenden Sie eine der [unterstützten Regionen](overview.md) als Standort für die Ressourcengruppe.

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

Geben Sie bei entsprechender Aufforderung einen Kennwortwert ein.

## <a name="create-a-table-in-sql-data-warehouse"></a>Erstellen einer Tabelle in SQL Data Warehouse

Fügen Sie dem Data Warehouse durch Ausführen des Skripts [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) eine Tabelle hinzu. Verwenden Sie zum Ausführen des Skripts Visual Studio oder den Abfrage-Editor im Portal.

Das auszuführende Skript lautet wie folgt:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Veröffentlichen der Azure Functions-App

1. Öffnen Sie das [EventHubsCaptureEventGridDemo-Beispielprojekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) in Visual Studio 2017 (15.3.2 oder höher).

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **FunctionEGDWDumper**, und wählen Sie **Veröffentlichen** aus.

   ![Veröffentlichen einer Funktions-App](media/event-grid-event-hubs-integration/publish-function-app.png)

1. Wählen Sie **Azure-Funktionen-App** und **Vorhandenes Element auswählen**. Wählen Sie **Veröffentlichen**.

   ![Ziel-Funktions-App](media/event-grid-event-hubs-integration/pick-target.png)

1. Wählen Sie die Funktions-App aus, die Sie über die Vorlage bereitgestellt haben. Klicken Sie auf **OK**.

   ![Auswählen der Funktions-App](media/event-grid-event-hubs-integration/select-function-app.png)

1. Wenn Visual Studio das Profil konfiguriert hat, wählen Sie **Veröffentlichen**.

   ![Wählen Sie "Veröffentlichen"](media/event-grid-event-hubs-integration/select-publish.png)

Nach dem Veröffentlichen der Funktion können Sie das Ereignis abonnieren.

## <a name="subscribe-to-the-event"></a>Abonnieren des Ereignisses

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie die Ressourcengruppe und die Funktions-App aus.

   ![Anzeigen der Funktions-App](media/event-grid-event-hubs-integration/view-function-app.png)

1. Wählen Sie die Funktion aus.

   ![Auswählen der Funktion](media/event-grid-event-hubs-integration/select-function.png)

1. Wählen Sie **Event Grid-Abonnement hinzufügen** aus.

   ![Abonnement hinzufügen](media/event-grid-event-hubs-integration/add-event-grid-subscription.png)

9. Benennen Sie das Event Grid-Abonnement. Verwenden Sie **Event Hubs-Namespaces** als Ereignistyp. Geben Sie Werte zum Auswählen der Instanz des Event Hubs-Namespace an. Übernehmen Sie als Abonnenten-Endpunkt den angegebenen Wert. Klicken Sie auf **Erstellen**.

   ![Erstellen des Abonnements](media/event-grid-event-hubs-integration/set-subscription-values.png)

## <a name="run-the-app-to-generate-data"></a>Ausführen der App zum Generieren von Daten

Sie haben nun Event Hub, SQL Data Warehouse, die Azure-Funktions-App und das Ereignisabonnement fertig eingerichtet. Die Projektmappe kann nun Daten vom Event Hub zum Data Warehouse migrieren. Vor dem Ausführen einer Anwendung, die Daten für Event Hubs generiert, müssen Sie einige Werte konfigurieren.

1. Wählen Sie im Portal den Event Hub-Namespace aus. Wählen Sie **Verbindungszeichenfolgen**.

   ![Auswählen von „Verbindungszeichenfolgen“](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Wählen Sie **RootManageSharedAccessKey**.

   ![Auswählen des Schlüssels](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopieren Sie den Wert für **Verbindungszeichenfolge – Primärschlüssel**.

   ![Kopieren des Schlüssels](media/event-grid-event-hubs-integration/copy-key.png)

4. Kehren Sie zum Visual Studio-Projekt zurück. Öffnen Sie **program.cs** im Projekt „WindTurbineDataGenerator“.

5. Ersetzen Sie die zwei konstanten Werte. Verwenden Sie für **EventHubConnectionString** den kopierten Wert. Verwenden Sie **hubdatamigration** als Event Hub-Name.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Erstellen Sie die Projektmappe. Führen Sie die Anwendung „WindTurbineGenerator.exe“ aus. Fragen Sie nach ein paar Minuten in der Tabelle im Data Warehouse die migrierten Daten ab.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Eine Einführung zu Event Hubs Capture finden Sie unter [Aktivieren von Event Hubs Capture über das Azure-Portal](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Weitere Informationen zum Einrichten und Ausführen des Beispiels finden Sie unter [Event Hubs Capture and Event Grid sample](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) (Beispiel für Events Hubs Capture und Event Grid).
---
title: "Diagnoseprotokollierung für Azure Cosmos DB | Microsoft-Dokumentation"
description: Dieses Tutorial dient als Hilfe bei den ersten Schritten mit der Azure Cosmos DB-Protokollierung.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: mimig
ms.openlocfilehash: 407a9a3be4ae8a9b00a953914e6b4414d8dac8b6
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Diagnoseprotokollierung für Azure Cosmos DB

Sobald Sie damit begonnen haben, eine oder mehrere Azure Cosmos DB-Datenbanken zu verwenden, möchten Sie wahrscheinlich überwachen, wie und wann auf Ihre Datenbanken zugegriffen wird. Mit der Diagnoseprotokollierung in Azure Cosmos DB können Sie diese Überwachung durchführen. Durch Aktivieren der Diagnoseprotokollierung können Sie Protokolle an [Azure Storage](https://azure.microsoft.com/services/storage/) senden, diese in [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) streamen und/oder nach [Log Analytics](https://azure.microsoft.com/services/log-analytics/) exportieren, einem Teil der [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite).

![Diagnoseprotokollierung in Storage, Event Hubs oder Operations Management Suite mithilfe von Log Analytics](./media/logging/azure-cosmos-db-logging-overview.png)

Verwenden Sie dieses Tutorial für den Einstieg in die Arbeit mit der Azure Cosmos DB-Protokollierung über das Azure-Portal, die CLI oder PowerShell.

## <a name="what-is-logged"></a>Was wird protokolliert?

* Alle authentifizierten REST DocumentDB (SQL)-API-Anforderungen werden protokolliert, z.B. auch Anforderungen, die aufgrund von Zugriffsberechtigungen, Systemfehlern oder fehlerhaften Anforderungen nicht erfolgreich sind. Unterstützung für MongoDB, Graph und Tabellen-APIs ist derzeit nicht verfügbar.
* Vorgänge für die Datenbank selbst; hierzu zählen CRUD-Vorgänge für alle Dokumente, Container und Datenbanken.
* Vorgänge für Kontoschlüssel; dies umfasst das Erstellen, Ändern oder Löschen dieser Schlüssel.
* Bei nicht authentifizierten Anforderungen wird eine 401-Antwort zurückgegeben. Wenn Anforderungen beispielsweise über kein Bearertoken verfügen oder falsch formatiert oder abgelaufen sind, ist deren Token ungültig.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie die folgenden Ressourcen:

* Azure Cosmos DB-Konto, -Datenbank und -Container müssen vorhanden sein. Anweisungen zum Erstellen dieser Ressourcen finden Sie unter [Erstellen eines Datenbankkontos mit dem Azure-Portal](create-documentdb-dotnet.md#create-a-database-account), [CLI-Beispiele](cli-samples.md) und [PowerShell-Beispiele](powershell-samples.md).

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Aktivieren der Protokollierung im Azure-Portal

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Diagnoseprotokolle**, und klicken Sie dann auf **Diagnose aktivieren**.

    ![Aktivieren der Diagnoseprotokollierung für Azure Cosmos DB im Azure-Portal](./media/logging/turn-on-portal-logging.png)

2. Gehen Sie auf der Seite **Diagnoseeinstellungen** wie folgt vor: 

    * **Name**. Geben Sie einen Namen für die zu erstellenden Protokolle ein.

    * **In einem Speicherkonto archivieren**. Sie benötigen ein vorhandenes Speicherkonto, mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Informationen zum Erstellen eines neuen Speicherkontos im Portal finden Sie unter [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md); befolgen Sie die Anweisungen zum Erstellen eines Ressourcen-Manager-Kontos für allgemeine Zwecke. Kehren Sie anschließend auf diese Seite im Portal zurück, und wählen Sie Ihr Speicherkonto aus. Es dauert möglicherweise einige Minuten, bis neu erstellte Speicherkonten im Dropdownmenü angezeigt werden.
    * **An einen Event Hub streamen**. Sie benötigen einen vorhandenen Event Hub-Namespace und einen Event Hub. mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Weitere Informationen zum Erstellen eines Event Hubs-Namespace finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md). Kehren Sie anschließend auf diese Seite im Portal zurück, um den Event Hub-Namespace und den Richtliniennamen auszuwählen.
    * **An Log Analytics senden**.     Zur Verwendung dieser Option benötigen Sie entweder einen vorhandenen Arbeitsbereich, oder erstellen Sie einen neuen Log Analytics-Arbeitsbereich mithilfe der Schritte zum [Erstellen eines neuen Arbeitsbereichs](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) im Portal. Weitere Informationen zum Anzeigen Ihrer Protokolle in Log Analytics finden Sie unter [Anzeigen von Protokollen in Log Analytics](#view-in-loganalytics).
    * **Log DataPlaneRequests** (DataPlaneRequests protokollieren). Wählen Sie diese Option, um Diagnosen für DocumentDB-, Graph- und Tabellen-API-Konten zu protokollieren. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Protokollieren von MongoRequests**. Wählen Sie diese Option, um die Diagnosen für MongoDB-API-Konten zu protokollieren. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Metrikanforderungen**. Wählen Sie diese Option zum Speichern von ausführlichen Daten in [Azure-Metriken](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftdocumentdbdatabaseaccounts-cosmosdb). Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.

3. Klicken Sie auf **Speichern**.

    Sie erhalten die Fehlermeldung: „Fehler beim Aktualisieren der Diagnose für \<Arbeitsbereichsname>. Das Abonnement \<Abonnement-ID> ist nicht für die Verwendung von microsoft.insights registriert.“ Befolgen Sie die Anweisungen zur [Problembehandlung bei der Azure-Diagnose](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage), um das Konto zu registrieren, und wiederholen Sie dann dieses Verfahren.

    Wenn Sie ändern möchten, wie die Diagnoseprotokolle zu einem beliebigen zukünftigen Zeitpunkt gespeichert werden, können Sie zum Ändern der Einstellungen des Diagnoseprotokolls für Ihr Konto jederzeit zu dieser Seite zurückkehren.

## <a name="turn-on-logging-using-cli"></a>Aktivieren der Protokollierung mit der CLI

Verwenden Sie die folgenden Befehle, um die Metrik- und Diagnoseprotokollierung mit der Azure-CLI zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   `resourceId` ist der Name des Azure Cosmos DB-Kontos. `storageId` ist der Name des Speicherkontos, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   `resourceId` ist der Name des Azure Cosmos DB-Kontos. `serviceBusRuleId` ist eine Zeichenfolge in folgendem Format:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

## <a name="turn-on-logging-using-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Zum Aktivieren der Protokollierung mit PowerShell benötigen Sie mindestens Azure Powershell, Version 1.0.1.

Um Azure PowerShell zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

Wenn Sie Azure PowerShell bereits installiert haben und Ihnen die Version nicht bekannt ist, geben Sie an der Azure PowerShell-Konsole `(Get-Module azure -ListAvailable).Version` ein.  

### <a id="connect"></a>Verbindungsherstellung mit Ihren Abonnements
Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:  

```powershell
Login-AzureRmAccount
```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Azure PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind, und verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen, müssen Sie unter Umständen ein bestimmtes Abonnement angeben, das zum Erstellen der Azure Key Vault-Instanz verwendet wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzureRmSubscription
```

Geben Sie anschließend Folgendes ein, um das Abonnement anzugeben, das dem protokollierten Azure Cosmos DB-Konto zugeordnet ist:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Wenn Ihrem Konto mehrere Abonnements zugeordnet sind, müssen Sie unbedingt das Konto angeben.
>
>

Weitere Informationen zum Konfigurieren von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

### <a id="storage"></a>Erstellen eines neuen Speicherkontos für Ihre Protokolle
Sie können zwar ein vorhandenes Speicherkonto für Ihre Protokolle verwenden, in diesem Tutorial wird jedoch ein neues Speicherkonto erstellt, das für Azure Cosmos DB-Protokolle bestimmt ist. Der Einfachheit halber speichern wir die Details zum Speicherkonto in einer Variablen mit dem Namen **sa**.

Um die Verwaltung noch weiter zu vereinfachen, verwenden wir in diesem Tutorial dieselbe Ressourcengruppe, die auch unsere Azure Cosmos DB-Datenbank enthält. Ersetzen Sie die Werte für ContosoResourceGroup, contosocosmosdblogs und „USA, Norden-Mitte“ durch Ihre eigenen Werte:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, muss dafür das Abonnement verwendet werden, das Ihrem Azure Cosmos DB-Abonnement entspricht. Außerdem muss das Ressourcen-Manager-Bereitstellungsmodell genutzt werden, nicht das klassische Bereitstellungsmodell.
>
>

### <a id="identify"></a>Bestimmen des Azure Cosmos DB-Kontos für Ihre Protokolle
Legen Sie den Azure Cosmos DB-Kontonamen auf eine Variable namens **account** fest, wobei ResourceName der Name des Azure Cosmos DB-Kontos ist.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Aktivieren der Protokollierung
Verwenden Sie zum Aktivieren der Protokollierung für Azure Cosmos DB das Cmdlet Set-AzureRmDiagnosticSetting zusammen mit den Variablen für das neue Speicherkonto, das Azure Cosmos DB-Konto und die Kategorie, für welche Protokolle aktiviert werden sollen. Führen Sie den folgenden Befehl aus, und legen Sie das Flag **-Enabled** auf **$true** fest:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Die Ausgabe des Befehls sollte der Folgenden ähneln:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

So wird bestätigt, dass die Protokollierung für Ihre Datenbank jetzt aktiviert ist und Informationen in Ihrem Speicherkonto gespeichert werden.

Optional können Sie eine Aufbewahrungsrichtlinie für Ihre Protokolle festlegen, mit der ältere Protokolle automatisch gelöscht werden. Richten Sie die Aufbewahrungsrichtlinie beispielsweise wie folgt ein: Legen Sie das Flag **-RetentionEnabled** auf **$true** und den Parameter **-RetentionInDays** auf **90** fest, sodass Protokolle, die älter sind als 90 Tage, automatisch gelöscht werden.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Zugreifen auf Ihre Protokolle
Azure Cosmos DB-Protokolle für die Kategorie **DataPlaneRequests** werden im Container **insights-logs-data-plane-requests** im angegebenen Speicherkonto gespeichert. 

Erstellen Sie zunächst eine Variable für den Containernamen. Diese wird im gesamten weiteren Verlauf der exemplarischen Vorgehensweise verwendet.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Geben Sie Folgendes ein, um alle Blobs in diesem Container aufzulisten:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Die Ausgabe sieht etwa wie folgt aus:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Wie Sie in dieser Ausgabe sehen, wird für die Blobs eine Benennungskonvention genutzt: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Für die Werte für Datum und Uhrzeit wird UTC verwendet.

Da dasselbe Speicherkonto zum Erfassen von Protokollen für mehrere Ressourcen verwendet werden kann, ist die vollqualifizierte Ressourcen-ID im Blobnamen sehr hilfreich, um nur auf die benötigten Blobs zuzugreifen bzw. diese herunterzuladen. Zuerst wird aber beschrieben, wie Sie alle Blobs herunterladen.

Erstellen Sie zunächst einen Ordner zum Herunterladen der Blobs. Beispiel:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Rufen Sie anschließend eine Liste mit allen Blobs ab:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Leiten Sie diese Liste an Get-AzureStorageBlobContent um, um die Blobs in Ihren Zielordner herunterzuladen:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Beim Ausführen dieses zweiten Befehls wird mit dem Trennzeichen **/** in den Blobnamen eine vollständige Ordnerstruktur unter dem Zielordner erstellt. Diese Struktur wird zum Herunterladen und Speichern der Blobs als Dateien verwendet.

Verwenden Sie Platzhalter, um Blobs selektiv herunterzuladen. Beispiel:

* Sie verfügen über mehrere Datenbanken und möchten Protokolle für nur eine Datenbank mit dem Namen CONTOSOCOSMOSDB3 herunterladen:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Wenn Sie über mehrere Ressourcengruppen verfügen und nur Protokolle für eine Ressourcengruppe herunterladen möchten, verwenden Sie `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Wenn Sie alle Protokolle für den Monat Juli 2017 herunterladen möchten, verwenden Sie `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Außerdem haben Sie folgende Möglichkeiten:

* Zum Abfragen des Status von Diagnoseeinstellungen für Ihre Datenbankressource: `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
* Zum Deaktivieren der Protokollierung der Kategorie **DataPlaneRequests** für Ihre Datenbankkontoressource: `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`


Die in jeder dieser Abfragen zurückgegebenen Blobs werden, wie im folgenden Code gezeigt, als Text im JSON-Blob-Format gespeichert. 

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Weitere Informationen zu den Daten in jedem einzelnen JSON-Blob finden Sie unter [Interpretieren der Azure Cosmos DB-Protokolle](#interpret).

## <a name="managing-your-logs"></a>Verwalten der Protokolle

Protokolle werden zwei Stunden nach Ausführung des Azure Cosmos DB-Vorgangs in Ihrem Konto verfügbar gemacht. Die Verwaltung der Protokolle im Speicherkonto ist Ihre Aufgabe:

* Verwenden Sie zum Schützen der Protokolle standardmäßige Azure-Zugriffssteuerungsmethoden, indem Sie einschränken, wer darauf zugreifen kann.
* Löschen Sie Protokolle, die im Speicherkonto nicht mehr aufbewahrt werden sollen.
* Der Aufbewahrungszeitraum für in einem Speicherkonto archivierte Anforderungen auf der Datenebene wird im Portal konfiguriert, wenn **Log DataPlaneRequests** ausgewählt wird. Informationen zum Ändern dieser Einstellung finden Sie unter [Aktivieren der Protokollierung im Azure-Portal](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Anzeigen von Protokollen in Log Analytics

Wenn Sie beim Aktivieren der Protokollierung die Option **An Log Analytics senden** ausgewählt haben, werden Diagnosedaten aus Ihrer Sammlung innerhalb von zwei Stunden an Log Analytics weitergeleitet. Dies bedeutet: Wenn Sie sofort nach dem Aktivieren der Protokollierung Log Analytics anzeigen, sehen Sie keine Daten. Warten Sie zwei Stunden, und wiederholen Sie den Vorgang. 

Bevor Sie Ihre Protokolle anzeigen, sollten Sie überprüfen, ob Ihr Log Analytics-Arbeitsbereich zur Verwendung der neuen Log Analytics-Abfragesprache aktualisiert wurde. Um dies zu überprüfen, öffnen Sie das [Azure-Portal](https://portal.azure.com), klicken Sie ganz links auf **Log Analytics**, und wählen Sie dann den Arbeitsbereichsnamen wie in der folgenden Abbildung dargestellt. Die Seite **OMS-Arbeitsbereich** wird angezeigt, wie in der folgenden Abbildung dargestellt.

![Log Analytics im Azure-Portal](./media/logging/azure-portal.png)

Wenn die folgende Meldung auf der Seite **OMS-Arbeitsbereich** angezeigt wird, wurde Ihr Arbeitsbereich nicht zur Verwendung der neuen Sprache aktualisiert. Weitere Informationen zum Upgrade auf die neue Abfragesprache finden Sie unter [Upgrade des Azure Log Analytics-Arbeitsbereichs auf die neue Protokollsuche](../log-analytics/log-analytics-log-search-upgrade.md). 

![Log Analytics-Upgradebenachrichtigung](./media/logging/upgrade-notification.png)

Zum Anzeigen der Diagnosedaten in Log Analytics öffnen Sie die Seite „Protokollsuche“ im linken Menü oder den Bereich „Verwaltung“ der Seite, wie in der folgenden Abbildung dargestellt.

![Protokollsuchoptionen im Azure-Portal](./media/logging/log-analytics-open-log-search.png)

Jetzt haben Sie die Datensammlung aktiviert und können das folgende Protokollsuchbeispiel mit der neuen Abfragesprache ausführen, um die letzten zehn Protokolle anzuzeigen: `AzureDiagnostics | take 10`.

![Beispiel der letzten 10 Protokollsuchen](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Abfragen

Hier sind einige zusätzliche Abfragen, die Sie in das Feld **Protokollsuche** eingeben können, um die Überwachung Ihrer Azure Cosmos DB-Container zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](../log-analytics/log-analytics-log-search-upgrade.md). 

Informationen über die Bedeutung der von jeder Protokollsuche zurückgegebenen Daten finden Sie unter [Interpretieren der Azure Cosmos DB-Protokolle](#interpret).

* Alle Diagnoseprotokolle von Azure Cosmos DB für den angegebenen Zeitraum.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Die zuletzt protokollierten zehn Ereignisse.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Alle Vorgänge gruppiert nach Vorgangstyp.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Alle Vorgänge gruppiert nach Ressource.

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Gesamte Benutzeraktivität gruppiert nach Ressource. Beachten Sie, dass dies ein Aktivitätsprotokoll und kein Diagnoseprotokoll ist.

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Welche Vorgänge länger als 3 Millisekunden dauern.

    ```
    AzureDiagnostics | where toint(duration_s) > 3000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Welcher Agent die Vorgänge ausführt.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Wann Vorgänge mit langer Laufzeit ausgeführt wurden.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Weitere Informationen zur Verwendung der neuen Protokollsuchesprache finden Sie unter [Grundlegendes zu Protokollsuchvorgängen in Log Analytics](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Interpretieren Ihrer Protokolle

Das Schema in Azure Storage und Log Analytics gespeicherter Diagnosedaten ist sehr ähnlich. 

Die folgende Tabelle beschreibt die Inhalte der einzelnen Protokolleinträge.

| Azure Storage-Feld oder -Eigenschaft | Log Analytics-Eigenschaft | Beschreibung |
| --- | --- | --- |
| in | TimeGenerated | Datum und Uhrzeit (UTC), zu denen der Vorgang aufgetreten ist. |
| resourceId | Ressource | Das Azure Cosmos DB-Konto, für das Protokolle aktiviert sind.|
| category | Category (Kategorie) | Für Azure Cosmos DB-Protokolle ist DataPlaneRequests der einzige verfügbare Wert. |
| operationName | NameVorgang | Name des Vorgangs. Bei diesem Wert kann es sich um einen der folgenden Vorgänge handeln: Create, Update, Read, ReadFeed, Delete, Replace, Execute, SqlQuery, Query, JSQuery, Head, HeadFeed und Upsert.   |
| properties | – | Die Inhalte dieser Felder werden in den folgenden Zeilen beschrieben. |
| activityId | activityId_g | Die eindeutige GUID für den protokollierten Vorgang. |
| userAgent | userAgent_s | Eine Zeichenfolge, die den Client-Benutzer-Agent angibt, der die Anforderung ausführt. Das Format lautet {Name des Benutzer-Agents}/{version}.|
| resourceType | ResourceType | Der Typ der Ressource, auf die zugegriffen wird. Bei diesem Wert kann es sich um einen der folgenden Ressourcentypen handeln: Database, Collection, Document, Attachment, User, Permission, StoredProcedure, Trigger, UserDefinedFunction oder Offer. |
| statusCode |statusCode_s | Der Antwortstatus des Vorgangs. |
| requestResourceId | ResourceId | Die resourceId für die Anforderung; diese kann je nach ausgeführtem Vorgang auf databaseRid, collectionRid oder documentRid zeigen.|
| clientIpAddress | clientIpAddress_s | Die IP-Adresse des Clients. |
| requestCharge | requestCharge_s | Die Anzahl der vom Vorgang verwendeten RUs. |
| collectionRid | collectionId_s | Die eindeutige ID für die Sammlung.|
| duration | duration_s | Die Dauer des Vorgangs in Ticks. |
| requestLength | requestLength_s | Die Länge der Anforderung in Bytes. |
| responseLength | responseLength_s | Die Länge der Antwort in Bytes.|
| resourceTokenUserRid | resourceTokenUserRid_s | Dieser Wert ist nicht leer, wenn [Ressourcentoken](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) für die Authentifizierung verwendet werden; er zeigt auf die Ressourcen-ID des Benutzers. |

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie sich mit dem Aktivieren der Protokollierung vertraut machen und darüber hinaus ein Verständnis der von den verschiedenen Azure-Diensten unterstützten Metriken und Protokollkategorien erlangen möchten, lesen Sie sowohl den Artikel [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) als auch den Artikel [Übersicht über Azure-Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) durch.
- Lesen Sie diese Artikel durch, um sich über Event Hubs zu informieren:
   - [Was sind Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lesen Sie [Herunterladen von Metrik- und Diagnoseprotokollen aus Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs).
- Lesen Sie [Grundlegendes zu Protokollsuchvorgängen in Log Analytics](../log-analytics/log-analytics-log-search-new.md).

---
title: Azure SQL-Datenbankmetriken und Diagnoseprotokollierung | Microsoft Docs
description: "Hier erhalten Sie Informationen zum Konfigurieren von Azure SQL-Datenbank zum Speichern von Statistiken für Ressourcenverbrauch, Konnektivität und Abfrageausführung."
services: sql-database
documentationcenter: 
author: veljko-msft
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: vvasic
ms.openlocfilehash: 6d5fc10b5186f2830f724325846a485e4064d12b
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Protokollierung von Metriken und Diagnosen für Azure SQL-Datenbank 
Azure SQL-Datenbank kann Metrik- und Diagnoseprotokolle ausgeben, um die Überwachung zu erleichtern. Sie können SQL-Datenbank zum Speichern von Ressourcenverbrauch, Workern und Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:

* **Azure Storage**: Wird für die Archivierung großer Mengen von Telemetriedaten zu einem kleinen Preis verwendet.
* **Azure Event Hubs**: Wird für die Integration von Telemetriedaten von SQL-Datenbank in Ihrer benutzerdefinierte Überwachungslösung oder Hotpipelines verwendet.
* **Azure Log Analytics**: Wird für eine einsatzbereite Überwachungslösung mit Funktionen für Berichterstellung, Warnungen und Problemlösung verwendet.

    ![Architektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Die Protokollierung von Metriken und Diagnosen ist standardmäßig nicht aktiviert. Sie können die Protokollierung von Metriken und Diagnosen mit einer der folgenden Methoden aktivieren und verwalten:

- Azure-Portal
- PowerShell
- Azure-Befehlszeilenschnittstelle
- Azure Monitor-REST-API 
- Azure Resource Manager-Vorlage

Wenn Sie die Protokollierung von Metriken und Diagnosen aktivieren, müssen Sie die Azure-Ressource angeben, in der die ausgewählten Daten erfasst werden. Verfügbare Optionen:

- Log Analytics
- Event Hubs
- Speicher 

Sie können eine neue Azure-Ressource bereitstellen oder eine vorhandene Ressource auswählen. Nachdem Sie die Speicherressource ausgewählt haben, müssen Sie angeben, welche Daten erfasst werden sollen. Verfügbare Optionen:

- [1-minute metrics (Minutenmetriken)](sql-database-metrics-diag-logging.md#1-minute-metrics): Enthält DTU-Prozentsatz, DTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesenen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status Erfolgreich/Fehlgeschlagen/Durch Firewall blockiert, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent und XTP-Speicher in Prozent.
- [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): Enthält Informationen zu den Laufzeitstatistiken der Abfrage, z.B. CPU-Nutzung und Abfragedauer.
- [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): Enthält Informationen zu den Wartestatistiken der Abfrage, die angibt, worauf Ihre Abfragen warten, z.B. CPU, Protokolle und Sperrungen.
- [Errors](sql-database-metrics-diag-logging.md#errors-dataset): Enthält Informationen zu den SQL-Fehlern, die für diese Datenbank aufgetreten sind.
- [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-waits-dataset): Enthält Informationen dazu, wie lange eine Datenbank jeweils auf die verschiedenen Wartezeittypen gewartet hat.
- [Timeouts](sql-database-metrics-diag-logging.md#timeouts-dataset): Enthält Informationen zu Timeouts, die für eine Datenbank aufgetreten sind.
- [Blockings](sql-database-metrics-diag-logging.md#blockings-dataset): Enthält Informationen zu blockierenden Ereignissen, die für eine Datenbank aufgetreten sind.
- [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): Enthält Intelligent Insights. [Weitere Informationen zu Intelligent Insights](sql-database-intelligent-insights.md).

Wenn Sie Event Hubs oder ein Speicherkonto auswählen, können Sie eine Aufbewahrungsrichtlinie angeben. Diese Richtlinie löscht Daten, die älter als ein ausgewählter Zeitraums sind. Wenn Sie Log Analytics angeben, hängt die Aufbewahrungsrichtlinie vom ausgewählten Tarif ab. Weitere Informationen finden Sie unter [Log Analytics-Preise](https://azure.microsoft.com/pricing/details/log-analytics/). 

Informationen zum Aktivieren der Protokollierung und zum Verständnis der Metriken und Protokollkategorien, die von den verschiedenen Azure-Diensten unterstützt werden, finden Sie in den folgenden empfohlenen Themen: 

* [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Übersicht über Azure-Diagnose](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

### <a name="azure-portal"></a>Azure-Portal

1. Um die Erfassung von Metriken und Diagnoseprotokollen im Portal zu aktivieren, navigieren Sie zur Seite der SQL-Datenbank oder des Pools für elastische Datenbanken, und wählen Sie dann **Diagnoseeinstellungen** aus.

   ![Aktivieren im Azure-Portal](./media/sql-database-metrics-diag-logging/enable-portal.png)

2. Erstellen Sie Diagnoseeinstellungen, oder bearbeiten Sie vorhandene, indem Sie das Ziel und die Telemetrie auswählen.

   ![Diagnoseeinstellungen](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

### <a name="powershell"></a>PowerShell

Verwenden Sie die folgenden Befehle, um die Metrik- und Diagnoseprotokollierung mit PowerShell zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Die Azure Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Sie können die Ressourcen-ID mit dem folgenden Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="to-configure-multiple-azure-resources"></a>So konfigurieren Sie mehrere Azure-Ressourcen

Wenn mehrere Abonnements unterstützt werden, verwenden Sie das PowerShell-Skript aus [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/) (Aktivieren der Protokollierung der Azure-Ressourcenmetriken mit PowerShell).

Geben Sie die Arbeitsbereichressourcen-ID &lt;$WSID&gt; als Parameter an, wenn Sie das Skript („Enable-AzureRMDiagnostics.ps1“) ausführen, um Diagnosedaten von mehreren Ressourcen an den Arbeitsbereich zu senden. Um die Arbeitsbereich-ID &lt;$WSID&gt; abzurufen, an die Sie Diagnosedaten senden möchten, ersetzen Sie &lt;$WSID&gt; durch die Abonnement-ID, ersetzen Sie &lt;RG_NAME&gt; durch den Namen der Ressourcengruppe, und ersetzen Sie &lt;WS_NAME&gt; durch den Namen des Arbeitsbereichs im folgenden Skript.

- Um mehrere Azure-Ressourcen zu konfigurieren, verwenden Sie die folgenden Befehle:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie die folgenden Befehle, um die Metrik- und Diagnoseprotokollierung mit der Azure CLI zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Die Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="rest-api"></a>REST-API

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API ändern](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen beim Erstellen von Ressourcen mithilfe der Resource Manager-Vorlage aktivieren](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Streamen nach Log Analytics 
SQL-Datenbankmetriken und -Diagnoseprotokolle können in Log Analytics gestreamt werden, indem die integrierte Option **An Log Analytics senden** im Portal verwendet wird. Sie können Log Analytics auch aktivieren, indem Sie eine Diagnoseeinstellung über PowerShell-Cmdlets, die Azure CLI oder die Azure Monitor-REST-API verwenden.

### <a name="installation-overview"></a>Übersicht über die Installation

Das Überwachen einer SQL-Datenbankflotte mit Log Analytics ist einfach. Es sind drei Schritte erforderlich:

1. Erstellen einer Log Analytics-Ressource.

2. Konfigurieren von Datenbanken zum Aufzeichnen von Metrik- und Diagnoseprotokollen in der erstellten Log Analytics-Ressource.

3. Installieren der **Azure SQL-Analyse**-Lösung aus dem Katalog in Log Analytics.

### <a name="create-a-log-analytics-resource"></a>Erstellen einer Log Analytics-Ressource

1. Wählen Sie im Menü auf der linken Seite **Neu** aus.

2. Wählen Sie **Überwachung und Verwaltung** aus.

3. Wählen Sie **Log Analytics**.

4. Tragen Sie in das Log Analytics-Formular die erforderlichen zusätzlichen Informationen ein: Name des Arbeitsbereichs, Abonnement, Ressourcengruppe, Ort und Tarif.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Konfigurieren von Datenbanken zum Aufzeichnen von Metrik- und Diagnoseprotokollen

Das Azure-Portal stellt die einfachste Möglichkeit dar, das Verzeichnis zu konfigurieren, in dem Datenbanken ihre Metriken aufzeichnen. Navigieren Sie im Portal zu Ihrer SQL-Datenbankressource, und wählen Sie **Diagnoseeinstellungen** aus. 

### <a name="install-the-sql-analytics-solution-from-the-gallery"></a>Installieren der Azure SQL-Analyselösung aus dem Katalog

1. Installieren Sie die Lösung „Azure SQL-Analyse“, nachdem Sie die Log Analytics-Ressource erstellt haben und Daten in diese fließen. Wählen Sie auf der Startseite der Operations Management Suite im seitlichen Menü die Option **Lösungskatalog** aus. Wählen Sie im Katalog die Lösung **Azure SQL-Analyse** aus, und wählen Sie dann **Hinzufügen** aus.

   ![Überwachungslösung](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Auf Ihrer Startseite der Operations Management Suite wird die Kachel **Azure SQL-Analyse** angezeigt. Wählen Sie diese Kachel aus, um das SQL-Analysedashboard zu öffnen.

### <a name="use-the-sql-analytics-solution"></a>Verwenden der Lösung „SQL-Analyse“

Die SQL-Analyse stellt ein hierarchisches Dashboard dar, in dem Sie die Hierarchie der Azure SQL-Datenbankressourcen durchsuchen können. Informationen zum Verwenden der Lösung „SQL-Analyse“ finden Sie unter [Überwachen der SQL-Datenbank mithilfe der Lösung „SQL-Analyse“](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Streamen an Event Hubs

SQL-Datenbankmetriken und -Diagnoseprotokolle können in Event Hubs gestreamt werden, indem die integrierte Option **An einen Event Hub streamen** im Portal verwendet wird. Sie können die Service Bus-Regel-ID auch aktivieren, indem Sie eine Diagnoseeinstellung über PowerShell-Cmdlets, die Azure CLI oder die Azure Monitor-REST-API verwenden. 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Welche Vorgänge können mit Metrik- und Diagnoseprotokollen in Event Hub ausgeführt werden?
Nachdem die ausgewählten Daten an Event Hubs gestreamt wurden, sind Sie der Einrichtung fortgeschrittener Überwachungsszenarien ein ganzes Stück näher gekommen. Event Hubs fungiert als Eingangsportal für eine Ereignispipeline. Nachdem Daten in einem Event Hub erfasst wurden, können sie transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradaptern gespeichert werden. Event Hubs entkoppelt die Produktion eines Ereignisdatenstroms von der Nutzung dieser Ereignisse. Auf diese Weise können Ereignisconsumer auf die Ereignisse gemäß ihrem eigenen Zeitplan zugreifen. Weitere Informationen zu Event Hubs finden Sie hier:

- [Was sind Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen:

* **Anzeigen der Dienstintegrität durch Streamen von Daten zum langsamsten Pfad an PowerBI**. Mithilfe von Event Hubs, Stream Analytics und PowerBI können Sie sich anhand Ihrer Metrik- und Diagnosedaten problemlos und nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. [Stream Analytics und Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md) bietet eine gute Übersicht über die Einrichtung von Event Hubs, die Verarbeitung von Daten mit Stream Analytics und die Verwendung von PowerBI als Ausgabe.

* **Streamen von Protokollen in Protokollierungs- und Telemetriedatenströme von Drittanbietern**. Mithilfe des Event Hubs-Streamings können Sie Ihre Metrik- und Diagnoseprotokolle in verschiedene Überwachungs- und Protokollanalyselösungen von Drittanbietern übertragen. 

* **Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung von Diagnoseprotokollen. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der [Anleitung von Dan Rosanova](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Streamen in den Speicher

SQL-Datenbankmetriken und -Diagnoseprotokolle können in Storage mithilfe der integrierten Option **In ein Speicherkonto archivieren** im Portal gespeichert werden. Sie können Storage auch aktivieren, indem Sie eine Diagnoseeinstellung über PowerShell-Cmdlets, die Azure CLI oder die Azure Monitor-REST-API verwenden.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Schema der Metrik- und Diagnoseprotokolle im Speicherkonto

Nachdem Sie die Sammlung von Metrik- und Diagnoseprotokollen eingerichtet haben, wird ein Speichercontainer im ausgewählten Speicherkonto erstellt, wenn die ersten Datenzeilen verfügbar sind. Die Struktur dieser Blobs sieht wie folgt aus:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Einfacher ausgedrückt:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Ein Blob-Name für Minutenmetriken kann beispielsweise wie folgt lauten:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Wenn Sie die Daten aus dem Pool für elastische Datenbanken aufzeichnen möchten, weicht der Blobname etwas ab:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>Herunterladen von Metriken und Protokollen aus Storage

Weitere Informationen zum [Herunterladen von Metrik- und Diagnoseprotokollen aus Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs).

## <a name="metrics-and-logs-available"></a>Verfügbare Metriken und Protokolle

### <a name="1-minute-metrics"></a>Minutenmetriken

|**Ressource**|**Metriken**|
|---|---|
|Datenbank|DTU-Prozentsatz, DTU-Verwendung, DTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status Erfolgreich/Fehlgeschlagen/Durch Firewall blockiert, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, XTP-Speicher in Prozent und Deadlocks |
|Elastischer Pool|eDTU-Prozentsatz, eDTU-Verwendung, eDTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, Speicherbegrenzung, XTP-Speicher in Prozent |
|||

### <a name="query-store-runtime-statistics"></a>Laufzeitstatistiken für den Abfragespeicher

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: QueryStoreRuntimeStatistics|
|NameVorgang|Name des Vorgangs. Immer: QueryStoreRuntimeStatisticsEvent|
|Ressource|Der Name der Ressource.|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|query_hash_s|Der Abfragehash.|
|query_plan_hash_s|Der Hash des Abfrageplans.|
|statement_sql_handle_s|Das SQL-Anweisungshandle.|
|interval_start_time_d|Beginn des DateTimeOffsets des Intervalls in Anzahl von Takten von 1900-1-1.|
|interval_end_time_d|Ende des DateTimeOffsets des Intervalls in Anzahl von Takten von 1900-1-1.|
|logical_io_writes_d|Gesamtzahl von logischen E/A-Schreibvorgängen|
|max_logical_io_writes_d|Maximale Anzahl von logischen E/A-Schreibvorgängen pro Ausführung|
|physical_io_reads_d|Gesamtzahl von physischen E/A-Lesevorgängen|
|max_physical_io_reads_d|Maximale Anzahl von logischen E/A-Lesevorgängen pro Ausführung|
|logical_io_reads_d|Gesamtzahl von logischen E/A-Lesevorgängen|
|max_logical_io_reads_d|Maximale Anzahl von logischen E/A-Lesevorgängen pro Ausführung|
|execution_type_d|Der Ausführungstyp.|
|count_executions_d|Anzahl von Ausführungen der Abfrage|
|cpu_time_d|Verwendete CPU-Gesamtzeit der Abfrage in Mikrosekunden|
|max_cpu_time_d|Maximal verwendete CPU-Zeit durch eine einzelne Ausführung in Mikrosekunden|
|dop_d|Summe des Grads an Parallelität|
|max_dop_d|Maximal verwendeter Grad an Parallelität für eine einzelne Ausführung|
|rowcount_d|Gesamtzahl von zurückgegebenen Zeilen|
|max_rowcount_d|Maximale Anzahl von Zeilen, die in einer einzelnen Ausführung zurückgegeben werden|
|query_max_used_memory_d|Gesamtmenge des verwendeten Arbeitsspeichers in KB|
|max_query_max_used_memory_d|Maximal verwendete Menge an Arbeitsspeicher durch eine einzelne Ausführung in KB|
|duration_d|Gesamtausführungszeit in Mikrosekunden|
|max_duration_d|Maximale Ausführungszeit einer einzelnen Ausführung|
|num_physical_io_reads_d|Gesamtzahl von physischen Lesevorgängen|
|max_num_physical_io_reads_d|Maximale Anzahl von physischen Lesevorgängen pro Ausführung|
|log_bytes_used_d|Gesamtmenge der verwendeten Protokollbytes|
|max_log_bytes_used_d|Maximale Menge der verwendeten Protokollbytes pro Ausführung|
|query_id_d|Die ID der Abfrage im Abfragespeicher.|
|plan_id_d|Die ID des Plans im Abfragespeicher.|

Weitere Informationen zu [Laufzeitstatistikdaten des Abfragespeichers](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Wartestatistiken des Abfragespeichers

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: QueryStoreWaitStatistics|
|NameVorgang|Name des Vorgangs. Immer: QueryStoreWaitStatistics|
|Ressource|Name der Ressource|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|wait_category_s|Kategorie des Wartevorgangs|
|is_parameterizable_s|Parametrisierbarkeit der Abfrage|
|statement_type_s|Anweisungstyp|
|statement_key_hash_s|Schlüsselhash der Anweisung|
|exec_type_d|Der Ausführungstyp.|
|total_query_wait_time_ms_d|Gesamtwartezeit der Abfrage für die bestimmte Wartekategorie|
|max_query_wait_time_ms_d|Die maximale Wartezeit der Abfrage in einer einzelnen Ausführung der bestimmten Wartekategorie.|
|query_param_type_d|0|
|query_hash_s|Abfragehash im Abfragespeicher|
|query_plan_hash_s|Der Hash des Abfrageplans im Abfragespeicher.|
|statement_sql_handle_s|Das Anweisungshandle im Abfragespeicher.|
|interval_start_time_d|Beginn des DateTimeOffsets des Intervalls in Anzahl von Takten von 1900-1-1.|
|interval_end_time_d|Ende des DateTimeOffsets des Intervalls in Anzahl von Takten von 1900-1-1.|
|count_executions_d|Die Anzahl von Ausführungen der Abfrage.|
|query_id_d|Die ID der Abfrage im Abfragespeicher.|
|plan_id_d|Die ID des Plans im Abfragespeicher.|

Weitere Informationen zu [Wartestatistikdaten des Abfragespeichers](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Fehlerdataset

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: Errors|
|NameVorgang|Name des Vorgangs. Immer: ErrorEvent|
|Ressource|Name der Ressource|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|Nachricht|Die Fehlermeldung im Nur-Text-Format.|
|user_defined_b|Das benutzerdefiniertes Fehlerbit.|
|error_number_d|Der Fehlercode.|
|Severity|Der Schweregrad des Fehlers.|
|state_d|Der Status des Fehlers.|
|query_hash_s|Der Abfragehash der fehlerhaften Abfrage (falls vorhanden).|
|query_plan_hash_s|Der Hash des Abfrageplans der fehlerhaften Abfrage (falls vorhanden).|

Weitere Informationen zu [SQL Server-Fehlermeldungen](https://msdn.microsoft.com/en-us/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Dataset zur Wartestatistik der Datenbank

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: DatabaseWaitStatistics|
|NameVorgang|Name des Vorgangs. Immer: DatabaseWaitStatisticsEvent|
|Ressource|Name der Ressource|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|wait_type_s|Der Name des Wartetyps.|
|start_utc_date_t [UTC]|Gemessener Zeitraum der Startzeit|
|end_utc_date_t [UTC]|Gemessener Zeitraum der Endzeit|
|delta_max_wait_time_ms_d|Maximale Wartezeit pro Ausführung|
|delta_signal_wait_time_ms_d|Die gesamte Signalwartezeit.|
|delta_wait_time_ms_d|Die Gesamtwartezeit im Zeitraum.|
|delta_waiting_tasks_count_d|Die Anzahl der wartenden Aufgaben.|

Weitere Informationen zu [Datenbankwartestatistiken](https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Dataset zu Timeouts

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: Timeouts|
|NameVorgang|Name des Vorgangs. Immer: TimeoutEvent|
|Ressource|Name der Ressource|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|error_state_d|Der Fehlerstatuscode.|
|query_hash_s|Der Abfragehash (falls verfügbar).|
|query_plan_hash_s|Der Hash des Abfrageplans (falls verfügbar).|

### <a name="blockings-dataset"></a>Dataset der Blockierungen

|Eigenschaft|Beschreibung|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: Blocks|
|NameVorgang|Name des Vorgangs. Immer: BlockEvent|
|Ressource|Name der Ressource|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des elastischen Pools, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|lock_mode_s|Der Von der Abfrage verwendeter Sperrmodus.|
|resource_owner_type_s|Besitzer der Sperre|
|blocked_process_filtered_s|Blockierter XML-Prozessbericht|
|duration_d|Die Dauer der Sperre in Millisekunden.|

### <a name="intelligent-insights-dataset"></a>Dataset von Intelligent Insights
Weitere Informationen zum [Protokollformat von Intelligent Insights](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Aktivieren der Protokollierung und zum Verständnis der Metriken und Protokollkategorien, die von den verschiedenen Azure-Diensten unterstützt werden, finden Sie in den folgenden Themen:

 * [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Übersicht über Azure-Diagnose](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Weitere Informationen zu Event Hubs finden Sie hier:

* [Was ist Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Weitere Informationen zu Storage finden Sie unter [Herunterladen von Metrik- und Diagnoseprotokollen aus Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs).

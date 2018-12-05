---
title: Azure SQL-Datenbankmetriken und Diagnoseprotokollierung | Microsoft Docs
description: Hier erhalten Sie Informationen zum Konfigurieren von Azure SQL-Datenbank zum Speichern von Statistiken für Ressourcenverbrauch, Konnektivität und Abfrageausführung.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: b903d0ddbccac8fe4fa8b251d409bd8addebb435
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425998"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Protokollierung von Metriken und Diagnosen für Azure SQL-Datenbank 

Azure SQL-Datenbank, Pools für elastische Datenbanken, verwaltete Instanzen und Datenbanken in der verwalteten Instanz können Metriken und Diagnoseprotokolle zur einfacheren Leistungsüberwachung ausgeben. Sie können eine Datenbank zum Streamen von Ressourcenverbrauch, Workern, Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:

* **Azure SQL-Analyse**: Wird als integrierte Azure-Datenbanklösung zur intelligenten Leistungsüberwachung mit Berichts-, Warnungs- und Risikominimierungsfunktionen verwendet.
* **Azure Event Hubs**: Wird für die Integration von Telemetriedaten von SQL-Datenbank in Ihrer benutzerdefinierte Überwachungslösung oder Hotpipelines verwendet.
* **Azure Storage**: Wird für die Archivierung großer Mengen von Telemetriedaten zu einem Bruchteil des Preises verwendet.

    ![Architecture](./media/sql-database-metrics-diag-logging/architecture.png)

Um die Metriken und Protokollkategorien zu verstehen, die von den verschiedenen Azure-Diensten unterstützt werden, sollten Sie folgende Artikel lesen:

* [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Übersicht über Azure-Diagnose](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

## <a name="enable-logging-of-diagnostics-telemetry"></a>Aktivieren der Protokollierung von Diagnosetelemetriedaten

Im ersten Abschnitt dieses Dokuments wird das Aktivieren von Diagnosetelemetriedaten für Datenbanken beschrieben und im zweiten Teil das Aktivieren von Diagnosetelemetriedaten für Pools für elastische Datenbanken oder verwaltete Instanzen. Die späteren Abschnitte dieses Dokuments enthalten Informationen zum Konfigurieren von Azure SQL-Analyse als Überwachungstool für das Anzeigen der Diagnosetelemetriedaten einer gestreamten Datenbank.

> [!NOTE]
> Für den Fall, dass Sie Pools für elastische Datenbanken oder verwaltete Instanzen verwenden, müssen Sie neben dem Aktivieren der Diagnosetelemetriedaten für Datenbanken auch die Diagnosetelemetriedaten für diese Ressourcen aktivieren. Der Grund hierfür ist, dass Pools für elastische Datenbanken und verwaltete Instanzen in der Rolle von Datenbankcontainern über eigene Diagnosetelemetriedaten verfügen, die unabhängig von den Diagnosetelemetriedaten einer einzelnen Datenbank sind. 
>

Sie können die Protokollierung von Metriken und Diagnosetelemetriedaten mit einer der folgenden Methoden aktivieren und verwalten:

- Azure-Portal
- PowerShell
- Azure-Befehlszeilenschnittstelle
- Azure Monitor-REST-API 
- Azure Resource Manager-Vorlage

Wenn Sie die Protokollierung von Metriken und Diagnosen aktivieren, müssen Sie das Azure-Ressourcenziel angeben, in dem die ausgewählten Daten gesammelt werden. Verfügbare Optionen:

- Azure SQL-Analysen
- Azure Event Hubs
- Azure Storage

Sie können eine neue Azure-Ressource bereitstellen oder eine vorhandene Ressource auswählen. Nachdem Sie eine Ressource ausgewählt haben, müssen Sie mithilfe einer Diagnoseeinstellungsoption angeben, welche Daten gesammelt werden.

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>Aktivieren der Protokollierung für Azure SQL-Datenbank bzw. Datenbanken in einer verwalteten Instanz

Die Protokollierung von Metriken und Diagnosen in SQL-Datenbank und für Datenbanken in verwalteten Instanzen ist standardmäßig nicht aktiviert.

Die folgenden Diagnosetelemetriedaten sind für die Sammlung für Azure SQL-Datenbank-Instanzen und Datenbanken in verwalteten Instanzen verfügbar:

| Überwachen von Telemetriedaten für Datenbanken | Unterstützung für Azure-SQL-Datenbank | Unterstützung für Datenbanken der verwalteten Instanz |
| :------------------- | ------------------- | ------------------- |
| [Alle Metriken](sql-database-metrics-diag-logging.md#all-metrics): Enthält DTU-/CPU-Prozentsatz, DTU/CPU-Limit, Prozentsatz der gelesenen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status „Erfolgreich“/„Fehlgeschlagen“/„Durch Firewall blockiert“, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent und XTP-Speicher in Prozent. | JA | Nein  |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): Enthält Informationen zu den Laufzeitstatistiken der Abfrage, z.B. CPU-Nutzung und Abfragedauerstatistiken. | JA | JA |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): Enthält Informationen zu den Wartestatistiken der Abfrage, die angibt, worauf Ihre Abfragen warten, z.B. CPU, Protokolle und Sperrungen. | JA | JA |
| [Errors](sql-database-metrics-diag-logging.md#errors-dataset): Enthält Informationen zu den SQL-Fehlern, die für diese Datenbank aufgetreten sind. | JA | Nein  |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): Enthält Informationen dazu, wie lange eine Datenbank jeweils auf die verschiedenen Wartezeittypen gewartet hat. | JA | Nein  |
| [Timeouts](sql-database-metrics-diag-logging.md#time-outs-dataset): Enthält Informationen zu Timeouts, die für eine Datenbank aufgetreten sind. | JA | Nein  |
| [Blocks](sql-database-metrics-diag-logging.md#blockings-dataset): Enthält Informationen zu blockierenden Ereignissen, die für eine Datenbank aufgetreten sind. | JA | Nein  |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): Enthält Intelligent Insights-Informationen zur Leistung. [Weitere Informationen zu Intelligent Insights](sql-database-intelligent-insights.md). | JA | JA |

### <a name="azure-portal"></a>Azure-Portal

Das Streaming von Diagnosetelemetriedaten für Azure SQL-Datenbank und Datenbanken in einer verwalteten Instanz für Ziele von Azure Storage, Event Hubs oder Log Analytics wird für die einzelnen Datenbanken im Azure-Portal über das Menü „Diagnoseeinstellungen“ konfiguriert.

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>Konfigurieren des Streamings von Diagnosetelemetriedaten für Azure SQL-Datenbank

   ![Symbol der SQL-Datenbank](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

Um das Streaming von Diagnosetelemetriedaten für **Azure SQL-Datenbank** zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu Ihrer Azure SQL-Datenbank-Ressource.
2. Klicken Sie auf **Diagnoseeinstellungen**.
3. Klicken Sie auf **Diagnose aktivieren**, wenn noch keine Einstellungen vorgenommen wurden, oder auf **Einstellung bearbeiten**, um eine vorhandene Einstellung zu bearbeiten.
- Es können bis zu drei (3) parallele Verbindungen zum Streamen von Diagnosetelemetriedaten erstellt werden. Wählen Sie zum Konfigurieren von mehreren parallelen Streamingvorgängen für Diagnosedaten an mehrere Ressourcen **+ Diagnoseeinstellung hinzufügen** aus, um eine weitere Einstellung zu erstellen.

   ![Aktivieren der Diagnose für SQL-Datenbank](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

4. Geben Sie den Namen für die Einstellung für die eigene Verwendung ein.
5. Wählen Sie aus, von welcher Ressource Diagnosedaten aus der Datenbank gestreamt werden sollen: **In Speicherkonto archivieren**, **An einen Event Hub streamen** oder **An Log Analytics senden**.
6. Aktivieren Sie für die standardmäßige Überwachungsoberfläche Kontrollkästchen für die Datenbankdiagnoseprotokolltelemetriedaten: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics**, **Errors**, **DatabaseWaitStatistics**, **Timeouts**, **Blocks**, **Deadlocks**. Diese Telemetriedaten sind ereignisbasiert und stellen die standardmäßige Überwachungsumgebung bereit.
7. Aktivieren Sie für eine erweiterte Überwachungsoberfläche das Kontrollkästchen für **AllMetrics**. Hierbei handelt es sich um Telemetriedaten basierend auf 1 Minute für die Datenbankdiagnosetelemetriedaten, wie oben beschrieben wird. 
8. Klicken Sie im Menü „Einstellungen“ auf **Speichern**

   ![Konfigurieren der Diagnose für SQL-Datenbank](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)

> [!NOTE]
> Sicherheitsüberwachungsprotokolle können nicht über die Einstellungen für die Datenbankdiagnose aktiviert werden. Informationen zum Aktivieren des Streamings von Überwachungsprotokollen finden Sie unter [Einrichten der Überwachung für Ihre Datenbank](sql-database-auditing.md#subheading-2) sowie unter [SQL-Überwachungsprotokolle in Azure Log Analytics und Azure Event Hubs](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/13/sql-audit-logs-in-azure-log-analytics-and-azure-event-hubs/).
>

> [!TIP]
> Wiederholen Sie die oben genannten Schritte für jede Azure SQL-Datenbank, die überwacht werden soll. 
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>Konfigurieren des Streamings von Diagnosetelemetriedaten für Datenbanken in verwalteten Instanzen

   ![Symbol der Datenbank in der verwalteten Instanz](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

Um das Streaming von Diagnosetelemetriedaten für **Datenbanken in verwalteten Instanzen** zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu Ihrer Datenbank in der verwalteten Instanz.
2. Klicken Sie auf **Diagnoseeinstellungen**.
3. Klicken Sie auf **Diagnose aktivieren**, wenn noch keine Einstellungen vorgenommen wurden, oder auf **Einstellung bearbeiten**, um eine vorhandene Einstellung zu bearbeiten.
- Es können bis zu drei (3) parallele Verbindungen zum Streamen von Diagnosetelemetriedaten erstellt werden. Wählen Sie zum Konfigurieren von mehreren parallelen Streamingvorgängen für Diagnosedaten an mehrere Ressourcen **+ Diagnoseeinstellung hinzufügen** aus, um eine weitere Einstellung zu erstellen.

   ![Aktivieren der Diagnose für die Datenbank in einer verwalteten Instanz](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. Geben Sie den Namen für die Einstellung für die eigene Verwendung ein.
5. Wählen Sie aus, von welcher Ressource Diagnosedaten aus der Datenbank gestreamt werden sollen: **In Speicherkonto archivieren**, **An einen Event Hub streamen** oder **An Log Analytics senden**.
6. Aktivieren Sie die Kontrollkästchen für die Datenbankdiagnosetelemetriedaten: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** und **Errors**.
7. Klicken Sie im Menü „Einstellungen“ auf **Speichern**

   ![Konfigurieren der Diagnose für die Datenbank in einer verwalteten Instanz](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)

> [!TIP]
> Wiederholen Sie die oben genannten Schritte für jede Datenbank in der verwalteten Instanz, die überwacht werden soll.
>

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>Aktivieren der Protokollierung für Pools für elastische Datenbanken oder die verwaltete SQL-Datenbank-Instanz

Pools für elastische Datenbanken und verwaltete Instanzen als Datenbankcontainer verfügen über eigene Diagnosetelemetriedaten, die von denen für Datenbanken getrennt sind. Diese Diagnosetelemetriedaten sind standardmäßig nicht aktiviert. 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Konfigurieren des Streamings von Diagnosetelemetriedaten für Pools für elastische Datenbanken

   ![Symbol des Pools für elastische Datenbanken](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

Die folgenden Diagnosetelemetriedaten sind für die Sammlung für Ressourcen von Pools für elastische Datenbanken verfügbar:

| Ressource | Überwachte Telemetriedaten |
| :------------------- | ------------------- |
| **Pool für elastische Datenbanken** | [Alle Metriken](sql-database-metrics-diag-logging.md#all-metrics) enthält eDTU-/CPU-Prozentsatz, eDTU-/CPU-Limit, Prozentsatz der gelesenen physischen Daten, Prozentsatz für Protokollschreibvorgang, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, Speicherbegrenzung, XTP-Speicher in Prozent. |

Um das Streaming von Diagnosetelemetriedaten für **Ressourcen von Pools für elastische Datenbanken** zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zur Ressource des Pools für elastische Datenbanken im Azure-Portal.
2. Klicken Sie auf **Diagnoseeinstellungen**.
3. Klicken Sie auf **Diagnose aktivieren**, wenn noch keine Einstellungen vorgenommen wurden, oder auf **Einstellung bearbeiten**, um eine vorhandene Einstellung zu bearbeiten.

   ![Aktivieren der Diagnose für Pools für elastische Datenbanken](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. Geben Sie den Namen für die Einstellung für die eigene Verwendung ein.
5. Wählen Sie aus, von welcher Ressource Diagnosedaten aus dem Pool für elastische Datenbanken gestreamt werden sollen: **In Speicherkonto archivieren**, **An einen Event Hub streamen** oder **An Log Analytics senden**.
6. Falls Log Analytics ausgewählt ist, klicken Sie auf **Konfigurieren** und erstellen einen neuen Arbeitsbereich, indem Sie auf **+ Neuen Arbeitsbereich erstellen** klicken oder einen vorhandenen Arbeitsbereich auswählen.
7. Aktivieren Sie das Kontrollkästchen für Diagnosetelemetriedaten von Pools für elastische Datenbanken. **AllMetrics**
8. Klicken Sie unten auf der Seite auf **Speichern**.

   ![Konfigurieren der Diagnose für Pools für elastische Datenbanken](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

> [!TIP]
> Wiederholen Sie die oben genannten Schritte für jeden Pool für elastische Datenbanken, der überwacht werden soll.
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>Konfigurieren des Streamings von Diagnosetelemetriedaten für verwaltete Instanzen

   ![Symbol der verwalteten Instanz](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

Die folgenden Diagnosetelemetriedaten sind für die Sammlung für Ressourcen von verwalteten Instanzen verfügbar:

| Ressource | Überwachte Telemetriedaten |
| :------------------- | ------------------- |
| **Verwaltete Instanz** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) enthält Anzahl der virtuellen Kerne, durchschnittlichen CPU-Prozentsatz, E/A-Anforderungen, gelesene/geschriebene Bytes, reservierten Speicherplatz, genutzten Speicherplatz. |

Um das Streaming von Diagnosetelemetriedaten für **Ressourcen von verwalteten Instanzen** zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu der Ressource „Verwaltete Instanz“ im Azure-Portal.
2. Klicken Sie auf **Diagnoseeinstellungen**.
3. Klicken Sie auf **Diagnose aktivieren**, wenn noch keine Einstellungen vorgenommen wurden, oder auf **Einstellung bearbeiten**, um eine vorhandene Einstellung zu bearbeiten.

   ![Aktivieren der Diagnose für verwaltete Instanzen](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

4. Geben Sie den Namen für die Einstellung für die eigene Verwendung ein.
5. Wählen Sie aus, von welcher Ressource Diagnosedaten aus dem Pool für elastische Datenbanken gestreamt werden sollen: **In Speicherkonto archivieren**, **An einen Event Hub streamen** oder **An Log Analytics senden**.
6. Falls Log Analytics ausgewählt ist, erstellen Sie einen Arbeitsbereich, oder verwenden Sie einen vorhandenen Arbeitsbereich.
7. Aktivieren Sie das Kontrollkästchen für Instanzdiagnosetelemetriedaten. **ResourceUsageStats**
8. Klicken Sie unten auf der Seite auf **Speichern**.

   ![Konfigurieren der Diagnose für verwaltete Instanzen](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)

> [!TIP]
> Wiederholen Sie die oben genannten Schritte für jede verwaltete Instanz, die überwacht werden soll.
>

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

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API ändern](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen beim Erstellen von Ressourcen mithilfe der Resource Manager-Vorlage aktivieren](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-azure-sql-analytics"></a>Streamen in Azure SQL-Analyse 

Azure SQL-Analyse ist eine Cloudüberwachungslösung zum bedarfsorientierten und abonnementübergreifenden Überwachen der Leistung von Azure SQL-Datenbanken, Pools für elastische Datenbanken und verwalteten Instanzen über eine zentrale Oberfläche. Die Lösung erfasst und visualisiert wichtige Azure SQL-Datenbank-Leistungsmetriken und umfasst integrierte Intelligenz für die Problembehandlung der Leistung.

![Azure SQL-Analyse – Übersicht](../log-analytics/media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Metriken und Diagnoseprotokolle für SQL-Datenbank können in Azure SQL-Analyse gestreamt werden. Verwenden Sie dazu die integrierte Option **An Log Analytics senden** im Portal auf dem Blatt für die Diagnoseeinstellungen. Sie können Log Analytics auch aktivieren, indem Sie eine Diagnoseeinstellung über PowerShell-Cmdlets, die Azure CLI oder die Azure Monitor-REST-API verwenden.

### <a name="installation-overview"></a>Übersicht über die Installation

Das Überwachen einer Azure SQL-Datenbank-Flotte mit Azure SQL-Analyse ist einfach. Es sind drei Schritte erforderlich:

1. Erstellen der Lösung Azure SQL-Analyse im Azure Marketplace
2. Erstellen des Überwachungsarbeitsbereichs in der Lösung
3. Konfigurieren der Datenbanken zum Streamen von Diagnosetelemetriedaten in den erstellten Arbeitsbereich

Falls Sie Pools für elastische Datenbanken oder verwaltete Instanzen verwenden, müssen Sie zusätzlich zur Konfiguration der Diagnosetelemetriedaten für die Datenbanken auch noch das Streaming von Diagnosetelemetriedaten aus diesen Ressourcen konfigurieren.

### <a name="create-azure-sql-analytics-resource"></a>Erstellen von Azure SQL-Analyse-Ressourcen

1. Suchen Sie Azure SQL-Analyse im Azure Marketplace, und wählen Sie die Lösung aus.

   ![Suchen von Azure SQL-Analyse im Portal](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)
   
2. Wählen Sie in der Übersicht der Lösung **Erstellen** aus.

3. Tragen Sie in das Azure SQL-Analyse-Formular die erforderlichen zusätzlichen Informationen ein: Name des Arbeitsbereichs, Abonnement, Ressourcengruppe, Ort und Tarif.
 
   ![Konfigurieren von Azure SQL-Analyse im Portal](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Bestätigen Sie den Vorgang durch Auswählen von **OK**, und wählen Sie abschließend **Erstellen** aus.

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Konfigurieren von Datenbanken zum Aufzeichnen von Metrik- und Diagnoseprotokollen

Das Azure-Portal stellt die einfachste Möglichkeit dar, zu konfigurieren, wo die Datenbanken ihre Metriken aufzeichnen – wie oben beschrieben. Navigieren Sie im Portal zu Ihrer SQL-Datenbankressource, und wählen Sie **Diagnoseeinstellungen** aus.

Falls Sie Pools für elastische Datenbanken oder verwaltete Instanzen verwenden, müssen Sie auch die Diagnoseeinstellungen in diesen Ressourcen konfigurieren und festlegen, dass ihre eigenen Diagnosetelemetriedaten in den von Ihnen erstellten Arbeitsbereich gestreamt werden.

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

Ein Blobname für alle Metriken kann beispielsweise wie folgt lauten:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Wenn Sie die Daten aus dem Pool für elastische Datenbanken aufzeichnen möchten, weicht der Blobname etwas ab:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>Herunterladen von Metriken und Protokollen aus Storage

Weitere Informationen zum [Herunterladen von Metrik- und Diagnoseprotokollen aus Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

## <a name="data-retention-policy-and-pricing"></a>Datenaufbewahrungsrichtlinie und Preisübersicht

Wenn Sie Event Hubs oder ein Speicherkonto auswählen, können Sie eine Aufbewahrungsrichtlinie angeben. Diese Richtlinie löscht Daten, die älter als ein ausgewählter Zeitraums sind. Wenn Sie Log Analytics angeben, hängt die Aufbewahrungsrichtlinie vom ausgewählten Tarif ab. Für die Nutzung von Diagnosetelemetriedaten über die kostenlosen Einheiten hinaus, die für die Datenerfassung jeden Monat zugeteilt werden, fallen Gebühren an. Die kostenlosen Einheiten der bereitgestellten Datenerfassung ermöglicht die kostenlose Überwachung von mehreren Datenbanken pro Monat. Beachten Sie, dass bei einer größeren Anzahl aktiver Datenbanken mit umfangreicheren Workloads mehr Daten als bei Datenbanken im Leerlauf erfasst werden. Weitere Informationen finden Sie unter [Log Analytics-Preise](https://azure.microsoft.com/pricing/details/monitor/). 

Wenn Sie die Azure SQL-Analyse verwenden, können Sie problemlos Ihre Datenerfassungsnutzung in der Lösung überwachen, indem Sie den OMS-Arbeitsbereich im Navigationsmenü der Azure SQL-Analyse auswählen und dann „Nutzungs- und geschätzte Kosten“ auswählen.

## <a name="metrics-and-logs-available"></a>Verfügbare Metriken und Protokolle

Mit der [SQL-Analyse-Sprache](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) können Sie die gesammelten Überwachungstelemetriedaten für Ihre eigene **benutzerdefinierte Analyse** und **Anwendungsentwicklung** verwenden. Die Struktur der gesammelten Daten, Metriken und Protokolle wird unten aufgeführt.

## <a name="all-metrics"></a>Alle Metriken

### <a name="all-metrics-for-elastic-pools"></a>Alle Metriken für Pools für elastische Datenbanken

|**Ressource**|**Metriken**|
|---|---|
|Pool für elastische Datenbanken|eDTU-Prozentsatz, eDTU-Verwendung, eDTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, Speicherbegrenzung, XTP-Speicher in Prozent |

### <a name="all-metrics-for-azure-sql-database"></a>Alle Metriken für Azure SQL-Datenbank

|**Ressource**|**Metriken**|
|---|---|
|Azure SQL-Datenbank|DTU-Prozentsatz, DTU-Verwendung, DTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status Erfolgreich/Fehlgeschlagen/Durch Firewall blockiert, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, XTP-Speicher in Prozent und Deadlocks |

## <a name="logs"></a>Protokolle

### <a name="logs-for-managed-instance"></a>Protokolle für die verwaltete Instanz

### <a name="resource-usage-stats"></a>Statistiken zur Ressourcennutzung

|Eigenschaft|BESCHREIBUNG|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: ResourceUsageStats.|
|Ressource|Der Name der Ressource.|
|ResourceType|Name des Ressourcentyps Immer: MANAGEDINSTANCES.|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Name der verwalteten Instanz.|
|ResourceId|Der Ressourcen-URI.|
|SKU_s|Produkt-SKU der verwalteten Instanz.|
|virtual_core_count_s|Anzahl von verfügbaren virtuellen Kernen.|
|avg_cpu_percent_s|Durchschnittlicher CPU-Prozentsatz|
|reserved_storage_mb_s|Reservierte Speicherkapazität auf einer verwalteten Instanz.|
|storage_space_used_mb_s|Genutzter Speicherplatz auf einer verwalteten Instanz.|
|io_requests_s|Anzahl von IOPS|
|io_bytes_read_s|Gelesene IOPS-Bytes|
|io_bytes_written_s|Geschriebene IOPS-Bytes|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Protokolle für Azure SQL-Datenbank und Datenbanken in der verwalteten Instanz

### <a name="query-store-runtime-statistics"></a>Laufzeitstatistiken für den Abfragespeicher

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
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

Weitere Informationen zu [Laufzeitstatistikdaten des Abfragespeichers](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Wartestatistiken des Abfragespeichers

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
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

Weitere Informationen zu [Wartestatistikdaten des Abfragespeichers](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Fehlerdataset

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|Message|Die Fehlermeldung im Nur-Text-Format.|
|user_defined_b|Das benutzerdefiniertes Fehlerbit.|
|error_number_d|Fehlercode|
|Severity|Der Schweregrad des Fehlers.|
|state_d|Der Status des Fehlers.|
|query_hash_s|Der Abfragehash der fehlerhaften Abfrage (falls vorhanden).|
|query_plan_hash_s|Der Hash des Abfrageplans der fehlerhaften Abfrage (falls vorhanden).|

Weitere Informationen zu [SQL Server-Fehlermeldungen](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Dataset zur Wartestatistik der Datenbank

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|wait_type_s|Der Name des Wartetyps.|
|start_utc_date_t [UTC]|Gemessener Zeitraum der Startzeit|
|end_utc_date_t [UTC]|Gemessener Zeitraum der Endzeit|
|delta_max_wait_time_ms_d|Maximale Wartezeit pro Ausführung|
|delta_signal_wait_time_ms_d|Die gesamte Signalwartezeit.|
|delta_wait_time_ms_d|Die Gesamtwartezeit im Zeitraum.|
|delta_waiting_tasks_count_d|Die Anzahl der wartenden Aufgaben.|

Weitere Informationen zu [Datenbankwartestatistiken](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Dataset zu Timeouts

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|error_state_d|Der Fehlerstatuscode.|
|query_hash_s|Der Abfragehash (falls verfügbar).|
|query_plan_hash_s|Der Hash des Abfrageplans (falls verfügbar).|

### <a name="blockings-dataset"></a>Dataset der Blockierungen

|Eigenschaft|BESCHREIBUNG|
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
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|lock_mode_s|Der Von der Abfrage verwendeter Sperrmodus.|
|resource_owner_type_s|Besitzer der Sperre|
|blocked_process_filtered_s|Blockierter XML-Prozessbericht|
|duration_d|Die Dauer der Sperre in Millisekunden.|

### <a name="deadlocks-dataset"></a>Dataset zu Deadlocks

|Eigenschaft|BESCHREIBUNG|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC] |Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: Deadlocks|
|NameVorgang|Name des Vorgangs. Immer: DeadlockEvent|
|Ressource|Der Name der Ressource.|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank. |
|ResourceId|Der Ressourcen-URI.|
|deadlock_xml_s|XML-Bericht zu Deadlocks.|

### <a name="automatic-tuning-dataset"></a>Dataset zur automatischen Optimierung

|Eigenschaft|BESCHREIBUNG|
|---|---|
|TenantId|Ihre Mandanten-ID.|
|SourceSystem|Immer: Azure|
|TimeGenerated [UTC]|Der Zeitstempel für den Aufzeichnungsbeginn des Protokolls.|
|Typ|Immer: AzureDiagnostics|
|ResourceProvider|Name des Ressourcenanbieters Immer: MICROSOFT.SQL|
|Category (Kategorie)|Name der Kategorie Immer: AutomaticTuning|
|Ressource|Der Name der Ressource.|
|ResourceType|Name des Ressourcentyps Immer: SERVERS/DATABASES|
|SubscriptionId|Die Abonnement-GUID, zu der die Datenbank gehört.|
|ResourceGroup|Der Name der Ressourcengruppe, zu der die Datenbank gehört.|
|LogicalServerName_s|Der Name des Servers, zu dem die Datenbank gehört.|
|LogicalDatabaseName_s|Der Name der Datenbank.|
|ElasticPoolName_s|Der Name des Pools für elastische Datenbanken, zu dem die Datenbank gehört (falls vorhanden).|
|DatabaseName_s|Der Name der Datenbank.|
|ResourceId|Der Ressourcen-URI.|
|RecommendationHash_s|Eindeutiger Hash der Empfehlung zur automatischen Optimierung.|
|OptionName_s|Vorgang der automatischen Optimierung.|
|Schema_s|Datenbankschema.|
|Table_s|Betroffene Tabelle.|
|IndexName_s|Indexname.|
|IndexColumns_s|Spaltenname.|
|IncludedColumns_s|Enthaltene Spalten.|
|EstimatedImpact_s|Geschätzte Auswirkung der JSON der Empfehlung zur automatischen Optimierung.|
|Event_s|Typ des Ereignisses der automatischen Optimierung.|
|Timestamp_t|Zeitstempel der letzten Aktualisierung.|

### <a name="intelligent-insights-dataset"></a>Dataset von Intelligent Insights
Weitere Informationen zum [Protokollformat von Intelligent Insights](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Aktivieren der Protokollierung und zum Verständnis der Metriken und Protokollkategorien, die von den verschiedenen Azure-Diensten unterstützt werden, finden Sie in den folgenden Themen:

 * [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Übersicht über Azure-Diagnose](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Weitere Informationen zu Event Hubs finden Sie hier:

* [Was ist Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Weitere Informationen zu Storage finden Sie unter [Herunterladen von Metrik- und Diagnoseprotokollen aus Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

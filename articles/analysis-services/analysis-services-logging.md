---
title: Diagnoseprotokollierung für Azure Analysis Services | Microsoft-Dokumentation
description: Informationen zum Einrichten der Diagnoseprotokollierung für Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a8d6080b573cbad1004166f28a3e6596560241be
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426514"
---
# <a name="setup-diagnostic-logging"></a>Einrichten der Diagnoseprotokollierung

Die Überwachung der Leistung Ihrer Server ist ein wesentlicher Bestandteil jeder Analysis Services-Lösung. Mit [Diagnoseprotokollen auf Azure-Ressourcenebene](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) können Sie Protokolle überwachen und an [Azure Storage](https://azure.microsoft.com/services/storage/) senden, diese in [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) streamen und in [Log Analytics](https://azure.microsoft.com/services/log-analytics/), einem Dienst von [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite), exportieren. 

![Diagnoseprotokollierung für Storage, Event Hubs oder Log Analytics](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>Protokollierte Daten

Sie können die Kategorien **Modul**, **Dienst** und **Metriken** auswählen.

### <a name="engine"></a>Motor

Bei Auswahl von **Modul** werden alle [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events) protokolliert. Einzelne Ereignisse können nicht ausgewählt werden. 

|xEvent-Kategorien |Ereignisname  |
|---------|---------|
|Sicherheitsüberwachung    |   Audit Login      |
|Sicherheitsüberwachung    |   Audit Logout      |
|Sicherheitsüberwachung    |   Audit Server Starts And Stops      |
|Statusberichte     |   Progress Report Begin      |
|Statusberichte     |   Progress Report End      |
|Statusberichte     |   Progress Report Current      |
|Abfragen     |  Query Begin       |
|Abfragen     |   Query End      |
|Befehle     |  Command Begin       |
|Befehle     |  Command End       |
|Fehler und Warnungen     |   Error      |
|Entdecken     |   Discover End      |
|Benachrichtigung     |    Benachrichtigung     |
|Sitzung     |  Session Initialize       |
|Sperren    |  Deadlock       |
|Verarbeitung von Abfragen     |   VertiPaq SE Query Begin      |
|Verarbeitung von Abfragen     |   VertiPaq SE Query End      |
|Verarbeitung von Abfragen     |   VertiPaq SE Query Cache Match      |
|Verarbeitung von Abfragen     |   Direct Query Begin      |
|Verarbeitung von Abfragen     |  Direct Query End       |

### <a name="service"></a>Dienst

|Vorgangsname  |Auftreten bei folgendem Vorgang  |
|---------|---------|
|ResumeServer     |    Fortsetzen eines Servers     |
|SuspendServer    |   Anhalten eines Servers      |
|DeleteServer     |    Löschen eines Servers     |
|RestartServer    |     Benutzer startet einen Server über SSMS oder PowerShell neu.    |
|GetServerLogFiles    |    Benutzer exportiert Serverprotokoll über PowerShell.     |
|ExportModel     |   Benutzer exportiert ein Modell im Portal mithilfe von „Öffnen“ in Visual Studio.     |

### <a name="all-metrics"></a>Alle Metriken

Mit der Kategorie „Metriken“ werden dieselben [Servermetriken](analysis-services-monitor.md#server-metrics) protokolliert, die in „Metriken“ angezeigt werden.

## <a name="setup-diagnostics-logging"></a>Einrichten der Diagnoseprotokollierung

### <a name="azure-portal"></a>Azure-Portal

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) > „Server“ im linken Navigationsbereich auf **Diagnoseprotokolle** und dann auf **Diagnose aktivieren**.

    ![Aktivieren der Diagnoseprotokollierung für Azure Cosmos DB im Azure-Portal](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. Geben Sie in **Diagnoseeinstellungen** die folgenden Optionen an: 

    * **Name**: Geben Sie einen Namen für die zu erstellenden Protokolle ein.

    * **In einem Speicherkonto archivieren**. Sie benötigen ein vorhandenes Speicherkonto, mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Siehe [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md). Befolgen Sie die Anweisungen zum Erstellen eines allgemeinen Resource Manager-Kontos, und wählen Sie dann Ihr Speicherkonto aus, indem Sie zu dieser Seite im Portal zurückwechseln. Es dauert möglicherweise einige Minuten, bis neu erstellte Speicherkonten im Dropdownmenü angezeigt werden.
    * **An einen Event Hub streamen**. Sie benötigen einen vorhandenen Event Hub-Namespace und einen Event Hub. mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Weitere Informationen finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md). Kehren Sie anschließend auf diese Seite im Portal zurück, um den Event Hub-Namespace und den Richtliniennamen auszuwählen.
    * **An Log Analytics senden**. Zur Verwendung dieser Option benötigen Sie entweder einen vorhandenen Arbeitsbereich, oder erstellen Sie einen neuen Log Analytics-Arbeitsbereich mithilfe der Schritte zum [Erstellen eines neuen Arbeitsbereichs](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) im Portal. Weitere Informationen zum Anzeigen Ihrer Protokolle in Log Analytics finden Sie unter [Anzeigen von Protokollen in Log Analytics](#view-in-loganalytics).

    * **Modul:** Wählen Sie diese Option aus, um xEvents zu protokollieren. Wenn die Archivierung in einem Speicherkonto erfolgt, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Dienst**. Wählen Sie diese Option aus, um Ereignisse auf Dienstebene zu protokollieren. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Metriken:** Wählen Sie diese Option aus, um ausführliche Daten in [Metriken](analysis-services-monitor.md#server-metrics) zu speichern. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Diagnoseprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.

3. Klicken Sie auf **Speichern**.

    Sie erhalten die Fehlermeldung: „Fehler beim Aktualisieren der Diagnose für \<Arbeitsbereichsname>. Das Abonnement \<Abonnement-ID> ist nicht für die Verwendung von microsoft.insights registriert.“ Befolgen Sie die Anweisungen zur [Problembehandlung bei der Azure-Diagnose](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage), um das Konto zu registrieren, und wiederholen Sie dann dieses Verfahren.

    Wenn Sie ändern möchten, wie die Diagnoseprotokolle zu einem zukünftigen Zeitpunkt gespeichert werden, können Sie zum Ändern der Einstellungen zu dieser Seite zurückkehren.

### <a name="powershell"></a>PowerShell

Hier sind die grundlegenden Befehle zum Einstieg aufgeführt. Detaillierte Hilfe zum Einrichten der Protokollierung in einem Speicherkonto mithilfe von PowerShell finden Sie im Tutorial weiter unten in diesem Artikel.

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

### <a name="rest-api"></a>REST-API

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API ändern](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen beim Erstellen von Ressourcen mithilfe einer Resource Manager-Vorlage aktivieren](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="manage-your-logs"></a>Verwalten Ihrer Protokolle

Die Protokolle stehen in der Regel einige Stunden nach dem Einrichten der Protokollierung zur Verfügung. Die Verwaltung der Protokolle im Speicherkonto ist Ihre Aufgabe:

* Verwenden Sie zum Schützen der Protokolle standardmäßige Azure-Zugriffssteuerungsmethoden, indem Sie einschränken, wer darauf zugreifen kann.
* Löschen Sie Protokolle, die im Speicherkonto nicht mehr aufbewahrt werden sollen.
* Legen Sie eine Beibehaltungsdauer fest, sodass ältere Protokolle aus dem Speicherkonto gelöscht werden.

## <a name="view-logs-in-log-analytics"></a>Anzeigen von Protokollen in Log Analytics

Metriken und Serverereignisse werden in Log Analytics in xEvents integriert, sodass eine parallele Analyse erfolgt. Log Analytics kann zudem so konfiguriert werden, dass Ereignisse von anderen Azure-Diensten empfangen werden, sodass die Diagnoseprotokollierungsdaten Ihrer Architektur ganzheitlich angezeigt werden.

Zum Anzeigen der Diagnosedaten in Log Analytics öffnen Sie die Seite „Protokollsuche“ im linken Menü oder im Bereich „Verwaltung“, wie in der folgenden Abbildung dargestellt.

![Protokollsuchoptionen im Azure-Portal](./media/analysis-services-logging/aas-logging-open-log-search.png)

Nachdem Sie die Datensammlung aktiviert haben, klicken Sie nun in **Protokollsuche** auf **Alle gesammelten Daten**.

Klicken Sie unter **Typ**, auf **AzureDiagnostics** und dann auf **Übernehmen**. AzureDiagnostics umfasst Modul- und Dienstereignisse. Beachten Sie, dass direkt eine Log Analytics-Abfrage erstellt wird. Das Feld EventClass\_s enthält xEvent-Namen, die Ihnen vertraut vorkommen können, wenn Sie xEvents für die lokale Protokollierung verwendet haben.

Wenn Sie auf **EventClass\_s** oder einen der Ereignisnamen klicken, wird in Log Analytics eine Abfrage erstellt. Speichern Sie die Abfragen, um sie zu einem späteren Zeitpunkt wiederverwenden zu können.

Testen Sie auch Log Analytics, das eine Website mit erweiterter Abfrage, Dashboards und Warnfunktionen für gesammelte Daten umfasst.

### <a name="queries"></a>Abfragen

Ihnen stehen Hunderte von Abfragen zur Verwendung zur Verfügung. Es folgen einige Abfragen, um Ihnen den Einstieg zu erleichtern.
Weitere Informationen zur Verwendung der neuen Abfragesprache der Protokollsuche finden Sie unter [Grundlegendes zu Protokollsuchvorgängen in Log Analytics](../log-analytics/log-analytics-log-search-new.md). 

* Fragen Sie an Azure Analysis Services gesendete Rückgabeabfragen ab, die nach mehr als fünf Minuten (300.000 Millisekunden) abgeschlossen wurden.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Identifizieren Sie horizontal hochskalierte Replikate.

    ```
    search * | summarize count() by ServerName_s
    ```
    Bei Verwendung der horizontalen Skalierung können Sie schreibgeschützte Replikate identifizieren, da in den Werten des Felds ServerName\_s die Replikatinstanznummer an den Namen angefügt ist. Das Ressourcenfeld enthält den Namen der Azure-Ressource, der mit dem Servernamen übereinstimmt, den Benutzer sehen. Im Feld „IsQueryScaleoutReadonlyInstance_s“ ist für Replikate „true“ angegeben.



> [!TIP]
> Möchten Sie eine Log Analytics-Abfrage für andere Benutzer freigeben? Wenn Sie über ein GitHub-Konto verfügen, können Sie sie in diesem Artikel hinzufügen. Klicken Sie einfach rechts oben auf dieser Seite auf **Bearbeiten**.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Tutorial: Aktivieren der Protokollierung mit PowerShell
In diesem kurzen Tutorial erstellen Sie ein Speicherkonto im gleichen Abonnement und der gleichen Ressourcengruppe, in denen sich Ihr Analysis Services-Server befindet. Anschließend verwenden Sie Set-AzureRmDiagnosticSetting, um die Diagnoseprotokollierung zu aktivieren, damit Ausgaben an das neue Speicherkonto gesendet werden.

### <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie die folgenden Ressourcen:

* Einen vorhandenen Azure Analysis Services-Server. Anweisungen zum Erstellen einer Serverressource finden Sie unter [Erstellen eines Servers im Azure-Portal](analysis-services-create-server.md) oder [Erstellen eines Azure Analysis Services-Servers mithilfe von PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Herstellen von Verbindungen mit Ihren Abonnements

Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:  

```powershell
Connect-AzureRmAccount
```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Azure PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind, und verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen, müssen Sie unter Umständen ein bestimmtes Abonnement angeben, das zum Erstellen der Azure Key Vault-Instanz verwendet wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzureRmSubscription
```

Geben Sie anschließend Folgendes ein, um das Abonnement anzugeben, das dem protokollierten Azure Analysis Services-Konto zugeordnet ist:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Wenn Ihrem Konto mehrere Abonnements zugeordnet sind, müssen Sie unbedingt das Abonnement angeben.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Erstellen eines neuen Speicherkontos für Ihre Protokolle

Sie können ein vorhandenes Speicherkonto für Ihre Protokolle verwenden, sofern es sich im gleichen Abonnement wie der Server befindet. Für dieses Tutorial erstellen Sie ein neues Speicherkonto für Analysis Services-Protokolle. Der Einfachheit halber speichern Sie die Details zum Speicherkonto in einer Variable mit dem Namen **sa**.

Zudem verwenden Sie dieselbe Ressourcengruppe, in der auch der Analysis Services-Server enthalten ist. Ersetzen Sie die Werte für `awsales_resgroup`, `awsaleslogs` und `West Central US` durch Ihre eigenen Werte:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Identifizieren des Serverkontos für Ihre Protokolle

Legen Sie den Kontonamen auf die Variable **account** fest, wobei ResourceName der Name des Kontos ist.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Aktivieren der Protokollierung

Verwenden Sie zum Aktivieren der Protokollierung das Cmdlet „Set-AzureRmDiagnosticSetting“ zusammen mit den Variablen für das neue Speicherkonto, das Serverkonto und die Kategorie. Führen Sie den folgenden Befehl aus, und legen Sie das Flag **-Enabled** auf **$true** fest:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

Die Ausgabe sollte ungefähr wie folgt aussehen:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

So wird bestätigt, dass die Protokollierung für den Server jetzt aktiviert ist und Informationen im Speicherkonto gespeichert werden.

Sie können außerdem eine Aufbewahrungsrichtlinie für Ihre Protokolle festlegen, sodass ältere Protokolle automatisch gelöscht werden. Richten Sie beispielsweise eine Aufbewahrungsrichtlinie ein, bei der das Flag **-RetentionEnabled** auf **$true** und der Parameter **-RetentionInDays** auf **90** festgelegt sind. Protokolle, die älter als 90 Tage sind, werden dann automatisch gelöscht.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zur [Diagnoseprotokollierung auf Azure-Ressourcenebene](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

Weitere Informationen finden Sie in der PowerShell-Hilfe unter [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting).

---
title: Streamen des Azure-Aktivitätsprotokolls an Event Hubs
description: Hier erfahren Sie, wie Sie das Azure-Aktivitätsprotokoll an Event Hubs streamen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/02/2018
ms.author: johnkem
ms.component: activitylog
ms.openlocfilehash: 45352c1cf4aca9043c23bbe12e94ba770a38c01b
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436704"
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Streamen des Azure-Aktivitätsprotokolls an Event Hubs
Sie können das [Azure-Aktivitätsprotokoll](monitoring-overview-activity-logs.md) nahezu in Echtzeit in jede beliebige Anwendung streamen, indem Sie eine der folgenden Möglichkeiten nutzen:

* Verwenden der integrierten Option zum **Exportieren** im Portal
* Aktivieren der Azure Service Bus-Regel-ID in einem Protokollprofil über die Azure PowerShell-Cmdlets oder Azure CLI

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Verwendungsmöglichkeiten für das Aktivitätsprotokoll und Event Hubs
Hier sind zwei Verwendungsmöglichkeiten für das Streamen des Aktivitätsprotokolls angegeben:

* **Streamen an Protokollierungs- und Telemetriesysteme von Drittanbietern:** Azure Event Hubs-Streaming entwickelt sich mehr und mehr zum Mechanismus für die Weiterreichung des Aktivitätsprotokolls an SIEMs und Protokollanalyselösungen von Drittanbietern.
* **Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform:** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung des Aktivitätsprotokolls. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. Weitere Informationen erhalten Sie im [Video von Dan Rosanova zur Verwendung von Event Hubs auf einer globalen Telemetrieplattform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Aktivieren des Aktivitätsprotokollstreamings
Das Aktivitätsprotokollstreaming kann entweder programmgesteuert oder über das Portal aktiviert werden. In beiden Fällen wählen Sie einen Event Hubs-Namespace und eine SAS-Richtlinie für den Namespace aus. Ein Event Hub mit dem Namen „insights-logs-operationallogs“ wird in diesem Namespace erstellt, wenn das erste neue Aktivitätsprotokollereignis eintritt. 

Wenn Sie nicht über einen Event Hubs-Namespace verfügen, müssen Sie diesen zuerst erstellen. Wenn Sie bereits Aktivitätsprotokollereignisse an diesen Event Hubs-Namespace gestreamt haben, wird der zuvor erstellte Event Hub wiederverwendet. 

Die SAS-Richtlinie definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an Event Hubs werden aktuell Berechtigungen für das **Verwalten**, **Senden** und **Lauschen** benötigt. Sie können freigegebene Zugriffsrichtlinien für den Event Hubs-Namespace im Azure-Portal auf der Registerkarte **Konfigurieren** für Ihren Event Hubs-Namespace erstellen oder ändern. 

Wenn Sie dem Protokollprofil für das Aktivitätsprotokoll das Streamingfeature hinzufügen möchten, muss der Benutzer, der die Änderung vornimmt, in der Event Hubs-Autorisierungsregel über die ListKey-Berechtigung verfügen. Der Event Hubs-Namespace muss sich nicht in demselben Abonnement wie das Abonnement befinden, das Protokolle ausgibt – sofern der Benutzer, der die Einstellung konfiguriert, über den entsprechenden RBAC-Zugriff auf beide Abonnements verfügt.

### <a name="via-the-azure-portal"></a>Über das Azure-Portal
1. Navigieren Sie zum Abschnitt **Aktivitätsprotokoll**, indem Sie die Suche **Alle Dienste** auf der linken Seite des Portals verwenden.
   
   ![Auswählen des Aktivitätsprotokolls aus der Liste mit den Diensten im Portal](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Wählen Sie oben im Protokoll die Schaltfläche **Exportieren**.
   
   ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Beachten Sie, dass die Filtereinstellungen, die Sie beim Anzeigen des Aktivitätsprotokolls in der vorherigen Ansicht angewendet haben, keine Auswirkung auf Ihre Exporteinstellungen haben. Diese sind nur zum Filtern der Anzeige für das Aktivitätsprotokoll im Portal bestimmt.
3. Wählen Sie im angezeigten Abschnitt die Option **Alle Regionen**. Wählen Sie keine bestimmten Regionen aus.
   
   ![Abschnitt „Exportieren“](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Falls Sie etwas anderes als **Alle Regionen** auswählen, entgehen Ihnen wichtige Ereignisse, auf die Sie warten. Da das Aktivitätsprotokoll ein globales Protokoll ist (kein regionales), ist den meisten Ereignissen keine Region zugeordnet. 
   >

4. Wählen Sie **Speichern**, um diese Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.
5. Wenn Sie über mehrere Abonnements verfügen, können Sie diesen Vorgang wiederholen und alle Daten an denselben Event Hub senden.

### <a name="via-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Wenn bereits ein Protokollprofil vorhanden ist, müssen Sie zuerst das vorhandene Protokollprofil entfernen und dann ein neues Profil erstellen.

1. Ermitteln Sie mithilfe von `Get-AzureRmLogProfile`, ob ein Protokollprofil vorhanden ist.  Wenn ein Protokollprofil vorhanden ist, suchen Sie die *name*-Eigenschaft.
2. Verwenden Sie `Remove-AzureRmLogProfile`, um das Protokollprofil mithilfe des Werts aus der *name*-Eigenschaft zu entfernen.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Verwenden Sie `Add-AzureRmLogProfile`, um ein neues Protokollprofil zu erstellen:

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Wenn bereits ein Protokollprofil vorhanden ist, müssen Sie zuerst das vorhandene Protokollprofil entfernen und dann ein neues Profil erstellen.

1. Ermitteln Sie mithilfe von `az monitor log-profiles list`, ob ein Protokollprofil vorhanden ist.
2. Verwenden Sie `az monitor log-profiles delete --name "<log profile name>`, um das Protokollprofil mithilfe des Werts aus der *name*-Eigenschaft zu entfernen.
3. Verwenden Sie `az monitor log-profiles create`, um ein neues Protokollprofil zu erstellen:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>Nutzen der Protokolldaten aus Event Hubs
Das Schema für das Aktivitätsprotokoll ist unter [Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll](monitoring-overview-activity-logs.md) verfügbar. Jedes Ereignis ist ein Array mit JSON-Blobs, die als *Datensätze* bezeichnet werden.

## <a name="next-steps"></a>Nächste Schritte
* [Archivieren Sie das Aktivitätsprotokoll mithilfe eines Speicherkontos.](monitoring-archive-activity-log.md)
* [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](monitoring-overview-activity-logs.md)
* [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](insights-auditlog-to-webhook-email.md)


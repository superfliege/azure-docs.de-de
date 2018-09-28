---
title: Streamen von Azure-Diagnoseprotokollen an Log Analytics
description: Hier erfahren Sie, wie Sie Azure-Diagnoseprotokolle an einen Log Analytics-Arbeitsbereich streamen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: c419a3c44a38f72d56f2b7b362c62e683fc20c7f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993016"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Streamen von Azure-Diagnoseprotokollen an Log Analytics

**[Azure-Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)** können über das Portal, über PowerShell-Cmdlets oder über die Azure CLI nahezu in Echtzeit an Azure Log Analytics gestreamt werden.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Verwendungsmöglichkeiten für Diagnoseprotokolle in Log Analytics

Azure Log Analytics ist ein flexibles Tool zur Protokollsuche und -analyse, mit dem Sie einen Einblick in die unformatierten Protokolldaten erhalten, die aus den Azure-Ressourcen generiert werden. Zu den Funktionen gehören:

* **Protokollsuche**: Schreiben Sie erweiterte Abfragen über Ihre Protokolldaten, stellen Sie Zusammenhänge zwischen Protokollen aus verschiedenen Quellen her, und generieren Sie sogar Diagramme, die an Ihr Azure-Dashboard angeheftet werden können.
* **Warnungen**: Stellen Sie Übereinstimmungen von Ereignissen mit einer bestimmten Abfrage fest, und lassen Sie sich per E-Mail oder Webhookaufruf benachrichtigen.
* **Lösungen**: Verwenden Sie vorgefertigte Ansichten und Dashboards, mit denen Sie sofortige Einblicke in Ihre Protokolldaten erhalten.
* **Erweiterte Analysen**: Wenden Sie Machine Learning- und Musterabgleichsalgorithmen an, um mögliche Probleme zu ermitteln, die durch Ihre Protokolle aufgedeckt werden.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Aktivieren des Streamens von Diagnoseprotokollen an Log Analytics

Das Streamen von Diagnoseprotokollen kann programmgesteuert, über das Portal oder mithilfe der [Azure Monitor-REST-APIs](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)aktiviert werden. In jedem Fall erstellen Sie eine Diagnoseeinstellung, in der Sie einen Log Analytics-Arbeitsbereich sowie die Protokollkategorien und Metriken angeben, die an den Arbeitsbereich gesendet werden sollen. Eine **Diagnoseprotokollkategorie** ist ein Protokolltyp, der von einer Ressource bereitgestellt werden kann.

Der Log Analytics-Arbeitsbereich muss sich nicht unter demselben Abonnement befinden wie die Ressource, die Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel:* Die Metrik „Eingehende Nachrichten“ eines Event Hubs kann auf einer warteschlangenspezifischen Ebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Streamen von Diagnoseprotokollen mithilfe des Portals
1. Navigieren Sie im Portal zu Azure Monitor, und klicken Sie auf **Diagnoseeinstellungen**.

    ![Abschnitt „Überwachung“ von Azure Monitor](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. Filtern Sie optional die Liste nach Ressourcengruppe oder Ressourcentyp, und klicken Sie auf die Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten.

3. Wenn keine Einstellungen für die Ressource vorhanden sind, die Sie ausgewählt haben, werden Sie aufgefordert, eine Einstellung zu erstellen. Klicken Sie auf „Diagnose aktivieren“.

   ![Diagnoseeinstellung hinzufügen – keine Einstellungen vorhanden](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Wenn Einstellungen für die Ressource vorhanden sind, sehen Sie eine Liste der Einstellungen, die bereits für diese Ressource konfiguriert sind. Klicken Sie auf „Diagnoseeinstellung hinzufügen“.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Legen Sie einen Namen für Ihre Einstellung fest, und aktivieren Sie das Kontrollkästchen **An Log Analytics senden**. Wählen Sie dann einen Log Analytics-Arbeitsbereich aus.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. Klicken Sie auf **Speichern**.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Ressource angezeigt, und Diagnoseprotokolle werden an diesen Arbeitsbereich gestreamt, sobald neue Ereignisdaten generiert werden. Beachten Sie, dass zwischen der Ausgabe eines Ereignisses und der Anzeige in Log Analytics möglicherweise bis zu 15 Minuten vergehen.

### <a name="via-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Wenn Sie das Streaming über die [Azure PowerShell-Cmdlets](insights-powershell-samples.md) aktivieren möchten, können Sie das Cmdlet `Set-AzureRmDiagnosticSetting` mit folgenden Parametern verwenden:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Beachten Sie, dass die workspaceID-Eigenschaft die gesamte Azure-Ressourcen-ID des Arbeitsbereichs benötigt, nicht die im Log Analytics-Portal angezeigte Kombination aus Arbeitsbereichs-ID und Schlüssel.

### <a name="via-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Zum Ermöglichen des Streamings per [Azure CLI](insights-cli-samples.md) können Sie den Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) verwenden.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Sie können das Diagnoseprotokoll um weitere Kategorien ergänzen, indem Sie dem JSON-Array, das als `--logs`-Parameter übergeben wird, Wörterbücher hinzufügen.

Das Argument `--resource-group` ist nur erforderlich, wenn `--workspace` keine Objekt-ID ist.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Wie frage ich die Daten in Log Analytics ab?

Auf dem Blatt „Protokollsuche“ im Portal oder auf der Advanced Analytics-Oberfläche als Teil von Log Analytics können Sie Diagnoseprotokolle im Rahmen der Protokollverwaltungslösung unter der AzureDiagnostics-Tabelle abfragen. Es gibt auch [verschiedene Lösungen für Azure-Ressourcen](../log-analytics/log-analytics-add-solutions.md), die Sie installieren können, um sofort Informationen zu den an Log Analytics gesendeten Protokolldaten zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](monitoring-overview-of-diagnostic-logs.md)

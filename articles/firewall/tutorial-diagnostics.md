---
title: 'Tutorial: Überwachen von Azure Firewall-Protokollen'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall-Protokolle aktivieren und verwalten.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: a4922fda80b957138a9929090f9d3c349348185d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991906"
---
# <a name="tutorial-monitor-azure-firewall-logs"></a>Tutorial: Überwachen von Azure Firewall-Protokollen

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

In den Beispielen der Azure Firewall-Artikel wird davon ausgegangen, dass Sie bereits die Public Preview von Azure Firewall aktiviert haben. Weitere Informationen finden Sie unter [Aktivieren der Public Preview von Azure Firewall](public-preview.md).

Azure Firewall kann mithilfe von Firewallprotokollen überwacht werden. Sie können aber auch Aktivitätsprotokolle verwenden, um Vorgänge für Azure Firewall-Ressourcen zu überwachen.

Sie können auf einige dieser Protokolle über das Portal zugreifen. Protokolle können an [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Storage und Event Hubs gesendet und in Log Analytics oder durch andere Tools wie Excel oder Power BI analysiert werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Ermöglichen der Protokollierung über das Azure-Portal
> * Aktivieren der Protokollierung mit PowerShell
> * Anzeigen und Analysieren des Aktivitätsprotokolls
> * Anzeigen und Analysieren der Netzwerk- und Anwendungsregelprotokolle

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

 Für Azure Firewall sind folgende Diagnoseprotokolle verfügbar:

* **Anwendungsregelprotokoll**

   Das Anwendungsregelprotokoll wird nur dann in einem Speicherkonto gespeichert, an Event Hubs gestreamt und/oder an Log Analytics gesendet, wenn Sie es für die einzelnen Azure Firewall-Instanzen aktiviert haben. Jede neue Verbindung, die einer Ihrer konfigurierten Anwendungsregeln entspricht, führt zu einem Protokoll für die akzeptierte/abgelehnte Verbindung. Die Daten werden im JSON-Format protokolliert, wie im folgenden Beispiel zu sehen:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Netzwerkregelprotokoll**

   Das Netzwerkregelprotokoll wird nur dann in einem Speicherkonto gespeichert, an Event Hubs gestreamt und/oder an Log Analytics gesendet, wenn Sie es für die einzelnen Azure Firewall-Instanzen aktiviert haben. Jede neue Verbindung, die einer Ihrer konfigurierten Netzwerkregeln entspricht, führt zu einem Protokoll für die akzeptierte/abgelehnte Verbindung. Die Daten werden im JSON-Format protokolliert, wie im folgenden Beispiel zu sehen:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Sie haben drei Möglichkeiten, um Ihre Protokolle zu speichern:

* **Speicherkonto:** Speicherkonten eignen sich am besten für Protokolle, die eine längere Zeit gespeichert und bei Bedarf überprüft werden.
* **Event Hubs:** Event Hubs sind eine hervorragende Möglichkeit für die Integration in andere SIEM-Tools (Security Information and Event Management), um Warnungen für Ihre Ressourcen zu erhalten.
* **Log Analytics:** Log Analytics ist am besten für eine allgemeine Echtzeitüberwachung Ihrer Anwendung oder zum Beobachten von Trends geeignet.

## <a name="activity-logs"></a>Aktivitätsprotokolle

   Aktivitätsprotokolleinträge werden standardmäßig gesammelt und können im Azure-Portal angezeigt werden.

   Mit dem Feature [Azure-Aktivitätsprotokolle](../azure-resource-manager/resource-group-audit.md) (ehemals Betriebs- und Überwachungsprotokolle) können Sie alle an Ihr Azure-Abonnement übermittelten Vorgänge anzeigen.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Aktivieren der Diagnoseprotokollierung über das Azure-Portal

Nach dem Aktivieren der Diagnoseprotokollierung kann es noch einige Minuten dauern, bis die Daten in Ihren Protokollen erscheinen. Sollte zunächst nichts angezeigt werden, warten Sie noch ein paar Minuten, und versuchen Sie es dann erneut.

1. Öffnen Sie im Azure-Portal die Ressourcengruppe Ihrer Firewall, und klicken Sie auf die Firewall.
2. Klicken Sie unter **Überwachen** auf **Diagnoseprotokolle**.

   Für Azure Firewall sind zwei dienstspezifische Protokolle verfügbar:

   * Anwendungsregelprotokoll
   * Netzwerkregelprotokoll

3. Klicken Sie auf **Diagnose aktivieren** , um die Erfassung von Daten zu starten.
4. Auf der Seite **Diagnoseeinstellungen** befinden sich die Einstellungen für die Diagnoseprotokolle. 
5. Da die Protokolle in diesem Beispiel von Log Analytics gespeichert werden, geben Sie **Firewall log analytics** als Name ein.
6. Klicken Sie auf **An Log Analytics senden**, um Ihren Arbeitsbereich zu konfigurieren. Sie können auch Event Hubs und ein Speicherkonto verwenden, um die Diagnoseprotokolle zu speichern.
7. Klicken Sie unter **Log Analytics** auf **Konfigurieren**.
8. Klicken Sie auf der Seite „OMS-Arbeitsbereiche“ auf **Neuen Arbeitsbereich erstellen**.
9. Geben Sie auf der Seite **Log Analytics-Arbeitsbereich** den Namen **firewall-oms** für den neuen **OMS-Arbeitsbereich** ein.
10. Wählen Sie Ihr Abonnement aus, verwenden Sie die vorhandene Ressourcengruppe für die Firewall (**Test-FW-RG**), und wählen Sie den Standort **USA, Osten** sowie den Tarif **Free** aus.
11. Klicken Sie auf **OK**.
   ![Starten des Konfigurationsprozesses][1]
12. Klicken Sie unter **Protokoll** auf **AzureFirewallApplicationRule** und **AzureFirewallNetworkRule**, um Protokolle für Anwendungs- und Netzwerkregeln zu erfassen.
   ![Speichern der Diagnoseeinstellungen][2]
13. Klicken Sie auf **Speichern**.

## <a name="enable-logging-with-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Die Aktivitätsprotokollierung ist automatisch für alle Resource Manager-Ressourcen aktiviert. Die Diagnoseprotokollierung muss aktiviert werden, um mit der Erfassung der Daten zu beginnen, die über diese Protokolle bereitgestellt werden.

Gehen Sie wie folgt vor, um die Diagnoseprotokollierung zu aktivieren:

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Dieser Wert hat das folgende Format: */subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Storage/storageAccounts/\<Speicherkontoname\>*.

   Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Azure-Portal verwenden, um nach diesen Informationen zu suchen. Die Informationen befinden sich auf der **Eigenschaftenseiteder** Ressource.

2. Notieren Sie sich die Ressourcen-ID Ihrer Firewall, für die die Protokollierung aktiviert wird. Dieser Wert hat das folgende Format: */subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Network/azureFirewalls/\<Firewallname\>*.

   Sie können das Portal verwenden, um nach diesen Informationen zu suchen.

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Für Diagnoseprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

## <a name="view-and-analyze-the-activity-log"></a>Anzeigen und Analysieren des Aktivitätsprotokolls

Mit einer der folgenden Methoden können Sie die Aktivitätsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus dem Aktivitätsprotokoll mit Azure PowerShell, der Azure CLI, der Azure-REST-API oder dem Azure-Portal ab. Detaillierte Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Wenn Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Activity Logs Content Pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Anzeigen und Analysieren der Netzwerk- und Anwendungsregelprotokolle

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) erfasst die Leistungsindikator- und Ereignisprotokolldateien. Die Anwendung umfasst Visualisierungen und leistungsfähige Suchfunktionen zum Analysieren Ihrer Protokolle.

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Nachdem Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, Power BI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von GitHub verwenden.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Firewall für die Erfassung von Protokollen konfiguriert haben, können Sie sich als Nächstes mit dem Anzeigen der Daten in Log Analytics beschäftigen.

> [!div class="nextstepaction"]
> [Azure-Netzwerküberwachungslösungen in Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png

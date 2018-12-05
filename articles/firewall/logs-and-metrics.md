---
title: Übersicht über Azure Firewall-Protokolle
description: Dieser Artikel ist eine Übersicht über die Azure Firewall-Diagnoseprotokolle.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 0698f1dbc491781089ef94eec32f2a427fd3cca4
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422387"
---
# <a name="azure-firewall-logs"></a>Azure Firewall-Protokolle

Azure Firewall kann mithilfe von Firewallprotokollen überwacht werden. Sie können aber auch Aktivitätsprotokolle verwenden, um Vorgänge für Azure Firewall-Ressourcen zu überwachen.

Sie können auf einige dieser Protokolle über das Portal zugreifen. Protokolle können an [Log Analytics](../azure-monitor/insights/azure-networking-analytics.md), Storage und Event Hubs gesendet und in Log Analytics oder durch andere Tools wie Excel oder Power BI analysiert werden.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

 Für Azure Firewall sind folgende Diagnoseprotokolle verfügbar:

* **Anwendungsregelprotokoll**

   Das Anwendungsregelprotokoll wird nur dann in einem Speicherkonto gespeichert, an Event Hubs gestreamt und/oder an Log Analytics gesendet, wenn Sie es für die einzelnen Azure Firewall-Instanzen aktiviert haben. Jede neue Verbindung, die einer Ihrer konfigurierten Anwendungsregeln entspricht, führt zu einem Protokoll für die akzeptierte/abgelehnte Verbindung. Die Daten werden im JSON-Format protokolliert, wie im folgenden Beispiel zu sehen:

   ```
   Category: application rule logs.
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
   Category: network rule logs.
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


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Überwachen von Azure Firewall-Protokollen und Metriken finden Sie unter [Tutorial: Überwachen von Azure Firewall-Protokollen](tutorial-diagnostics.md).
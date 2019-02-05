---
title: Umsteigen von der Legacywarnungen-API von Log Analytics auf die neue Azure-Warnungen-API
description: Übersicht über die Deaktivierung der auf savedSearch basierenden Legacywarnungen-API von Log Analytics und den Prozess zum Umsteigen auf die neue ScheduledQueryRules-API mit Details zu allgemeinen Kundenbedenken.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: e4e935a9c78950517623acdf8196d51793fff18a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462459"
---
# <a name="switch-api-preference-for-log-alerts"></a>Wechseln der API-Einstellung für Protokollwarnungen

> [!NOTE]
> Die aufgeführten Inhalte gelten **nicht** für Benutzer von Azure GovCloud. Sie gelten nur für Benutzer der öffentlichen Azure-Cloud.  

Bis vor kurzem haben Sie Warnungsregeln im Microsoft Operations Management Suite-Portal verwaltet. Die neue Warnungsfunktion wurde in verschiedene Dienste in Microsoft Azure integriert (darunter Log Analytics), und wir haben Sie gebeten, [Ihre Warnungsregeln aus dem OMS-Portal auf Azure](alerts-extend.md) zu erweitern. Um aber eine minimale Beeinträchtigung für Kunden zu gewährleisten, hat der Prozess die befehlsorientierte Benutzerschnittstellen für ihre Nutzung nicht verändert: die [Warnungs-API von Log Analytics](api-alerts.md), basierend auf SavedSearch.

Aber jetzt kündigen Sie für Benutzer, die die Warnungen von Log Analytics verwenden, eine echte programmgesteuerte Azure-Alternative an: [Azure Monitor – ScheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), die sich auch in Ihrer [Azure-Abrechnung für Protokollwarnungen](alerts-unified-log.md#pricing-and-billing-of-log-alerts) widerspiegelt. Weitere Informationen zum Verwalten Ihrer Protokollwarnungen über die API finden Sie unter [Verwalten von Protokollwarnungen mithilfe der Azure-Ressourcenvorlage](alerts-log.md#managing-log-alerts-using-azure-resource-template) und [Verwalten von Protokollwarnungen mit PowerShell, der CLI oder der API](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).

## <a name="benefits-of-switching-to-new-azure-api"></a>Vorteile des Umstiegs auf die neue Azure-API

Es gibt mehrere Vorteile beim Erstellen und Verwalten von Warnungen mithilfe der [scheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) im Vergleich zur [Legacywarnungs-API von Log Analytics](api-alerts.md). Wir haben einige der wichtigsten hier aufgelistet:

- Möglichkeit, [arbeitsbereichübergreifende Protokollsuche](../log-query/cross-workspace-query.md) in Warnungsregeln auszuführen und externen Ressourcen wie Log Analytics-Arbeitsbereiche oder sogar Application Insights-Apps einzubeziehen.
- Wenn mehrere Felder in der Abfrage zum Gruppieren verwendet werden, kann der Benutzer mit der [scheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) angeben, welches Feld im Azure-Portal zusammengefasst werden soll.
- Erstellen von Warnungsregeln in einem Durchgang als einzelne Ressource, ohne dass drei Ebenen von Ressourcen wie mit der [Legacywarnungs-API von Log Analytics](api-alerts.md) erstellt werden müssen.
- Einzelne befehlsorientierte Benutzerschnittstellen für alle Varianten von abfragebasierten Protokollwarnungen in Azure: Die neue [scheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kann zum Verwalten von Regeln für Log Analytics und Application Insights verwendet werden.
- Alle neuen Protokollwarnungsfunktionen und zukünftigen Entwicklungen werden nur über die neue [scheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) verfügbar sein.

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Prozess des Umstiegs von der Legacyprotokollwarnungen-API

Der Prozess des Verschiebens von Warnungsregeln aus der [Warnungsregel-API von Log Analytics](api-alerts.md) bringt keinerlei Änderungen Ihrer Warnungsdefinition, Abfrage oder Konfiguration mit sich. Benutzer können entweder die [Legacywarnungs-API von Log Analytics](api-alerts.md) oder die neue [scheduledQueryRules-API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) verwenden. Warnungsregeln, die von einer der beiden APIs erstellt wurden, können *nur von derselben API verwaltet werden* – ebenso wie über das Azure-Portal. Standardmäßig verwendet Azure Monitor weiterhin die [Legacywarnungs-API von Log Analytics](api-alerts.md) für das Erstellen einer neuen Warnungsregel über das Azure-Portal.

Die Auswirkungen des Wechsels der Einstellung zur scheduledQueryRules-API werden im Folgenden zusammengefasst:

- Alle Interaktionen, die zur Verwaltung von Protokollwarnungen über befehlsorientierte Benutzerschnittstellen durchgeführt werden, müssen nun stattdessen mit [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) durchgeführt werden. Weitere Informationen finden Sie unter [Beispiel für die Verwendung über eine Azure-Ressourcenvorlage](alerts-log.md#managing-log-alerts-using-azure-resource-template) und [Beispiel für die Verwendung über die Azure CLI und PowerShell](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).
- Jede neue Protokollwarnungsregel, die im Azure-Portal erstellt wird, wird nur mit [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) erstellt und ermöglicht es Benutzern, die [zusätzliche Funktionalität der neuen API](#Benefits-of-switching-to-new-Azure-API) auch über das Azure-Portal zu nutzen.

Jeder Kunde, der freiwillig auf die neuen [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) umsteigen und die Nutzung der [Legacywarnungs-API von Log Analytics](api-alerts.md) blockieren möchte, kann dies tun, indem er einen PUT-Aufruf für die unten genannte API ausführt, um alle Warnungsregeln, die dem spezifischen Log Analytics-Arbeitsbereich zugeordnet sind, zu wechseln.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Mit Anforderungstext, der den folgenden JSON-Code enthält.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

Wenn der Wechsel aller Warnungsregeln im Log Analytics-Arbeitsbereich zur Verwendung der neuen [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) erfolgreich ist, wird die folgende Antwort bereitgestellt.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Benutzer können auch den aktuellen Status Ihres Log Analytics-Arbeitsbereichs überprüfen und anzeigen, ob er nur für die Verwendung von [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) umgeschaltet wurde. Zur Überprüfung können Benutzer einen GET-Aufruf für die unten aufgeführte API durchführen.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Wenn der angegebene Log Analytics-Arbeitsbereich so umgeschaltet wurde, dass er nur [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) verwendet, ähnelt der JSON-Antwortcode dem Beispiel unten.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Wenn der angegebene Log Analytics-Arbeitsbereich noch nicht so umgeschaltet wurde, dass er nur [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) verwendet, ähnelt der JSON-Antwortcode dem Beispiel unten.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Monitor: Protokollwarnungen](alerts-unified-log.md).
- Informationen zum Erstellen von [Protokollwarnungen in Azure Alerts](alerts-log.md).
- Erfahren Sie mehr über die neue [Azure Alerts-Benutzeroberfläche](../../azure-monitor/platform/alerts-overview.md).

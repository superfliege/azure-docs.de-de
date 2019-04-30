---
title: Ausführliche Informationen zu Datensammlungen für Verwaltungslösungen in Azure | Microsoft-Dokumentation
description: Bei Verwaltungslösungen in Azure handelt es sich um eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken zu einem bestimmten Problembereich bereitstellen.  Dieser Artikel enthält eine Liste mit verfügbaren Verwaltungslösungen von Microsoft sowie Informationen zur Methode und Häufigkeit der Datensammlung.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: 6da2b63b7a9c8dc490bd30bb02ae6e7d932b2f7b
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683918"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Ausführliche Informationen zu Datensammlungen für Verwaltungslösungen in Azure
Dieser Artikel enthält eine Liste mit verfügbaren [Verwaltungslösungen](solutions.md) von Microsoft sowie Links zur entsprechenden ausführlichen Dokumentation.  Außerdem finden Sie hier Informationen zur jeweiligen Methode und Häufigkeit der Datensammlung in Azure Monitor.  Anhand der Informationen in diesem Artikel können Sie sich über die verschiedenen verfügbaren Lösungen informieren sowie den Datenfluss und die Verbindungsanforderungen für unterschiedliche Verwaltungslösungen nachvollziehen. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-management-solutions"></a>Liste mit Verwaltungslösungen

Die folgende Tabelle enthält die von Microsoft bereitgestellten [Verwaltungslösungen](solutions.md) in Azure. Ein Eintrag in der Spalte bedeutet, dass die Lösung diese Methode verwendet, um Daten in Azure Monitor zu sammeln.  Wenn die Spalten einer Lösung keine Einträge enthalten, werden Daten aus einem anderen Azure-Dienst direkt in Azure Monitor geschrieben. Über den Link der jeweiligen Lösung gelangen Sie zur detaillierten Dokumentation mit weiteren Informationen.

Erläuterung der Spalten:

- **Microsoft Monitoring Agent**: Agent, der unter Windows und Linux zum Ausführen des Managements Packs (SCOM) sowie zum Ausführen von Verwaltungslösungen (Azure) verwendet wird. In dieser Konfiguration ist der Agent direkt mit Azure Monitor und mit keiner Operations Manager-Verwaltungsgruppe verbunden. 
- **Operations Manager**: Gleicher Agent wie Microsoft Monitoring Agent. In dieser Konfiguration besteht eine [Verbindung mit einer Operations Manager-Verwaltungsgruppe](../../azure-monitor/platform/om-agents.md), die wiederum mit Azure Monitor verbunden ist. 
-  **Azure Storage**: Die Lösung sammelt Daten aus einem Azure-Speicherkonto. 
- **Operations Manager erforderlich?**: Für die Datensammlung durch die Verwaltungslösung wird eine verbundene Operations Manager-Verwaltungsgruppe benötigt. 
- **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten:** Wenn der Agent [mit einer SCOM-Verwaltungsgruppe verbunden](../../azure-monitor/platform/om-agents.md) ist, werden Daten vom Verwaltungsserver an Azure Monitor gesendet. In diesem Fall muss der Agent nicht direkt mit Azure Monitor verbunden sein. Falls diese Spalte keinen Eintrag enthält, werden Daten vom Agent direkt an Azure Monitor gesendet, auch wenn der Agent mit einer SCOM-Verwaltungsgruppe verbunden ist. Dazu muss er über das [Log Analytics-Gateway](../../azure-monitor/platform/gateway.md) mit Azure Monitor kommunizieren können.
- **Sammlungshäufigkeit**: Gibt an, wie häufig Daten von der Verwaltungslösung gesammelt werden. 



| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Aktivitätsprotokollanalyse](../../azure-monitor/platform/collect-activity-logs.md) | Azure | | | | | | Bei Benachrichtigung |
| [AD-Bewertung](../../azure-monitor/insights/ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 Tage |
| [AD-Replikationsstatus](../../azure-monitor/insights/ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 Tage |
| [Agent-Integrität](solution-agenthealth.md) | Windows und Linux | &#8226; | &#8226; | | | &#8226; | 1 Minute |
| [Warnungsverwaltung](../../azure-monitor/platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |Bei der Ankunft |
| [Warnungsverwaltung](../../azure-monitor/platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 Minute |
| [Warnungsverwaltung](../../azure-monitor/platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 Minuten |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | – |
| [Application Insights-Connector (Veraltet)](../../azure-monitor/platform/app-insights-connector.md) | Azure | | | |  |  | Bei Benachrichtigung |
| [Automation Hybrid Worker](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | – |
| [Azure Application Gateway-Analyse](../../azure-monitor/insights/azure-networking-analytics.md) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| [Azure-Netzwerksicherheitsgruppen-Analyse (Veraltet)](../../azure-monitor/insights/azure-networking-analytics.md) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| [Azure SQL Analytics (Vorschau)](../../azure-monitor/insights/azure-sql.md) | Windows | | | | | | 1 Minute |
| [Sicherung](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| [Kapazität und Leistung (Vorschau)](../../azure-monitor/insights/capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |Bei der Ankunft |
| [Änderungsnachverfolgung](../../automation/automation-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |[variiert](../../automation/automation-change-tracking.md#change-tracking-data-collection-details) |
| [Änderungsnachverfolgung](../../automation/automation-change-tracking.md) |Linux |&#8226; | | | | |[variiert](../../automation/automation-change-tracking.md#change-tracking-data-collection-details) |
| [Container](../../azure-monitor/insights/containers.md) | Windows und Linux | &#8226; | &#8226; |  |  |  | 3 Minuten |
| [Key Vault-Analysen](../../azure-monitor/insights/azure-key-vault.md) |Windows | | | | | |Bei Benachrichtigung |
| [Bewertung von Schadsoftware](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |Stündlich |
| [Netzwerkleistungsmonitor](../../azure-monitor/insights/network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP-Handshakes werden alle fünf Sekunden gesendet, Daten alle drei Minuten |
| [Office 365-Analyse (Vorschau)](solution-office-365.md) |Windows | | | | | |Bei Benachrichtigung |
| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| [Service Fabric-Analysen](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 Minuten |
| [Dienstzuordnung](../../azure-monitor/insights/service-map.md) | Windows und Linux | &#8226; | &#8226; |  |  |  | 15 Sekunden |
| [SQL-Bewertung](../../azure-monitor/insights/sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 Tage |
| [SurfaceHub](../../azure-monitor/insights/surface-hubs.md) |Windows |&#8226; | | | | |Bei der Ankunft |
| [System Center Operations Manager-Bewertung (Vorschau)](../../azure-monitor/insights/scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | sieben Tage |
| [Updateverwaltung](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates |
| [Upgrade Readiness](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 Tage |
| [VMware-Überwachung (Veraltet)](../../azure-monitor/insights/vmware.md) | Linux | &#8226; |  |  |  |  | 3 Minuten |
| [Wire Data 2.0 (Vorschau)](../../azure-monitor/insights/wire-data.md) |Windows (2012 R2 / 8.1 oder höher) |&#8226; |&#8226; | | | | 1 Minute |




## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit dem [Erstellen von Abfragen](../../azure-monitor/log-query/log-query-overview.md) für die Analyse der von Verwaltungslösungen gesammelten Daten vertraut.

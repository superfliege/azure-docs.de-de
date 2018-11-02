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
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: 82cfa9e62dcc6b3a72dcb1ccf97f1f52a88a75c4
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404102"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Ausführliche Informationen zu Datensammlungen für Verwaltungslösungen in Azure
Dieser Artikel enthält eine Liste mit verfügbaren [Verwaltungslösungen](monitoring-solutions.md) von Microsoft sowie Links zur entsprechenden ausführlichen Dokumentation.  Außerdem finden Sie hier Informationen zur jeweiligen Methode und Häufigkeit der Datensammlung in Log Analytics.  Anhand der Informationen in diesem Artikel können Sie sich über die verschiedenen verfügbaren Lösungen informieren sowie den Datenfluss und die Verbindungsanforderungen für unterschiedliche Verwaltungslösungen nachvollziehen. 

## <a name="list-of-management-solutions"></a>Liste mit Verwaltungslösungen

Die folgende Tabelle enthält die von Microsoft bereitgestellten [Verwaltungslösungen](monitoring-solutions.md) in Azure. Ein Eintrag in der Spalte bedeutet, dass die Lösung diese Methode verwendet, um Daten in Log Analytics zu sammeln.  Enthalten die Spalten einer Lösung keine Einträge, werden Daten aus einem anderen Azure-Dienst direkt in Log Analytics geschrieben. Über den Link der jeweiligen Lösung gelangen Sie zur detaillierten Dokumentation mit weiteren Informationen.

Erläuterung der Spalten:

- **Microsoft Monitoring Agent**: Agent, der unter Windows und Linux zum Ausführen des Managements Packs (SCOM) sowie zum Ausführen von Verwaltungslösungen (Azure) verwendet wird. In dieser Konfiguration ist der Agent direkt mit Log Analytics und mit keiner Operations Manager-Verwaltungsgruppe verbunden. 
- **Operations Manager**: Gleicher Agent wie Microsoft Monitoring Agent. In dieser Konfiguration besteht eine [Verbindung mit einer Operations Manager-Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md), die wiederum mit Log Analytics verbunden ist. 
-  **Azure Storage**: Die Lösung sammelt Daten aus einem Azure-Speicherkonto. 
- **Operations Manager erforderlich?**: Für die Datensammlung durch die Verwaltungslösung wird eine verbundene Operations Manager-Verwaltungsgruppe benötigt. 
- **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten**: Wenn der Agent [mit einer SCOM-Verwaltungsgruppe verbunden](../log-analytics/log-analytics-om-agents.md) ist, werden Daten vom Verwaltungsserver an Log Analytics gesendet. In diesem Fall muss der Agent nicht direkt mit Log Analytics verbunden sein. Falls diese Spalte keinen Eintrag enthält, werden Daten vom Agent direkt an Log Analytics gesendet, auch wenn der Agent mit einer SCOM-Verwaltungsgruppe verbunden ist. Dazu muss er über das [Log Analytics-Gateway](../log-analytics/log-analytics-oms-gateway.md) mit Log Analytics kommunizieren können.
- **Sammlungshäufigkeit**: Gibt an, wie häufig Daten von der Verwaltungslösung gesammelt werden. 



| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Aktivitätsprotokollanalyse](../log-analytics/log-analytics-activity.md) | Azure | | | | | | Bei Benachrichtigung |
| [AD-Bewertung](../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 Tage |
| [AD-Replikationsstatus](../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 Tage |
| [Agent-Integrität](../operations-management-suite/oms-solution-agenthealth.md) | Windows und Linux | &#8226; | &#8226; | | | &#8226; | 1 Minute |
| [Warnungsverwaltung](../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |Bei der Ankunft |
| [Warnungsverwaltung](../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 Minute |
| [Warnungsverwaltung](../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 Minuten |
| [Azure Site Recovery](../site-recovery/site-recovery-overview.md) | Azure | | | | | | – |
| [Application Insights-Connector (Vorschau)](../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | Bei Benachrichtigung |
| [Automation Hybrid Worker](../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | – |
| [Azure Application Gateway-Analyse](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| [Azure-Netzwerksicherheitsgruppen-Analyse (Veraltet)](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| [Azure SQL Analytics (Vorschau)](../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 Minute |
| [Sicherung](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | Bei Benachrichtigung |
| [Kapazität und Leistung (Vorschau)](../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |Bei der Ankunft |
| [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |Stündlich |
| [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md) |Linux |&#8226; | | | | |Stündlich |
| [Container](../log-analytics/log-analytics-containers.md) | Windows und Linux | &#8226; | &#8226; |  |  |  | 3 Minuten |
| [Key Vault-Analysen](../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |Bei Benachrichtigung |
| [Bewertung von Schadsoftware](../log-analytics/log-analytics-malware.md) |Windows |&#8226; |&#8226; | | |&#8226; |Stündlich |
| [Netzwerkleistungsmonitor](../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP-Handshakes werden alle fünf Sekunden gesendet, Daten alle drei Minuten |
| [Office 365-Analyse (Vorschau)](../operations-management-suite/oms-solution-office-365.md) |Windows | | | | | |Bei Benachrichtigung |
| **Verwaltungslösung** | **Plattform** | **Microsoft Monitoring Agent** | **Operations Manager-Agent** | **Azure Storage** | **Operations Manager erforderlich?** | **Über Verwaltungsgruppe gesendete Operations Manager-Agent-Daten** | **Sammlungshäufigkeit** |
| [Service Fabric-Analysen](../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 Minuten |
| [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) | Windows und Linux | &#8226; | &#8226; |  |  |  | 15 Sekunden |
| [SQL-Bewertung](../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 Tage |
| [SurfaceHub](../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |Bei der Ankunft |
| [System Center Operations Manager-Bewertung (Vorschau)](../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | sieben Tage |
| [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates |
| [Upgrade Readiness](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 Tage |
| [VMware-Überwachung (Veraltet)](../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 Minuten |
| [Wire Data 2.0 (Vorschau)](../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2 / 8.1 oder höher) |&#8226; |&#8226; | | | | 1 Minute |




## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit dem [Erstellen von Abfragen](../log-analytics/log-analytics-log-searches.md) für die Analyse der von Verwaltungslösungen gesammelten Daten vertraut.

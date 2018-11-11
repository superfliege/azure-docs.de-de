---
title: Übersicht über die Azure-Überwachungs-Agents | Microsoft-Dokumentation
description: Dieser Artikel bietet eine ausführliche Übersicht über die verfügbaren Azure-Agents, die die Überwachung von virtuellen Azure-Computern unterstützen.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: magoedte
ms.openlocfilehash: 81db6720422de111cc5b390c58e9020d7c19f90a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282032"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Übersicht über die Azure-Agents zur Überwachung von virtuellen Azure-Computern
Microsoft Azure bietet mehrere Möglichkeiten zum Erfassen unterschiedlicher Arten von Daten von virtuellen Computern, die in Azure oder anderen Cloudanbietern gehostet werden und auf denen Microsoft Windows und Linux ausgeführt werden.  In diesem Artikel werden die Unterschiede und die mit jedem Agent verfügbaren Funktionen beschrieben, damit Sie ermitteln können, welcher Agent Ihre Dienstverwaltung oder Ihre allgemeinen Überwachungsanforderungen unterstützt.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Vergleich zwischen der Azure-Diagnoseerweiterung und dem Log Analytics-Agent
Derzeit stehen in Azure zwei Typen von Agents zur Überwachung von virtuellen Azure-Computern zur Verfügung: die Azure-Diagnoseerweiterung und der Log Analytics-Agent für Linux und Windows.  Diese Agents dienen im Wesentlichen zur Erfassung von Metriken und Protokollen und zur Weiterleitung in ein Repository. Damit hören ihre Gemeinsamkeiten jedoch auf.  

Die [Azure-Diagnoseerweiterung](../monitoring-and-diagnostics/azure-diagnostics.md), die zunächst für Azure Cloud Services bereitgestellt und dann 2010 allgemein verfügbar wurde, ist ein Agent, der die einfache Sammlung von Diagnosedaten von einer Azure-IaaS-Ressource, z.B. einem virtuellen Computer, und die Speicherung der Daten in Azure Storage ermöglicht.  Die Daten im Speicher können Sie mit einem von mehreren verfügbaren Tools anzeigen, z.B. mit [Server-Explorer in Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) oder [Azure Storage-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Sie können auswählen, dass Folgendes gesammelt wird:

* Eine vordefinierte Gruppe von Betriebssystem-Leistungsindikatoren und Ereignisprotokollen (oder Sie können die einzelnen zu sammelnden Daten angeben). 
* Alle Anforderungen und/oder fehlerhaften Anforderungen an einen IIS-Webserver
* Ausgabeprotokolle zur Ablaufverfolgung von .NET-Apps
* Ereignisablaufverfolgung für Windows-Ereignisse (ETW) 
* Sammeln von Protokollereignissen aus Syslog  
* Absturzabbilder 

Daten können alternativ auch an [Application Insights](../application-insights/app-insights-cloudservices.md), [Log Analytics](../log-analytics/log-analytics-queries.md) oder über [Event Hub](../event-hubs/event-hubs-about.md) an Nicht-Azure-Dienste weitergeleitet werden. 

Für die erweiterte Überwachung, bei der nicht nur Metriken und eine Teilmenge von Protokollen gesammelt werden sollen, ist der Log Analytics-Agent für Windows und Linux erforderlich.  Mit diesem Agent können Sie Azure-Dienste wie Automation und Log Analytics, einschließlich aller angebotenen Features, nutzen, um eine umfassende Verwaltung Ihrer virtuellen Azure-Computer während des gesamten Lebenszyklus sicherzustellen. Dies umfasst:

* [Azure Automation-Updateverwaltung](../automation/automation-update-management.md) von Betriebssystemupdates
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) zur Beibehaltung eines konsistenten Konfigurationsstatus
* Nachverfolgen von Konfigurationsänderungen mit [Azure Automation für Änderungsnachverfolgung und Bestand](../automation/automation-change-tracking.md)
* Sammlung von benutzerdefinierten Protokollen von Betriebssystemen und gehosteten Anwendungen, z.B. [FluentD](../log-analytics/log-analytics-data-sources-json.md), [benutzerdefinierte Protokolle](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL und Apache](../log-analytics/log-analytics-data-sources-linux-applications.md), mit Log Analytics
* Azure-Dienste, z.B. [Application Insights](https://docs.microsoft.com/azure/application-insights/) und [Azure Security Center](https://docs.microsoft.com/azure/security-center/), speichern ihre Daten nativ direkt in Log Analytics.  

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Anforderungen und verfügbaren Methoden zum Bereitstellen des Agents auf Computern in Ihrem Rechenzentrum oder einer anderen Cloudumgebung finden Sie unter [Sammeln von Daten von Computern in Ihrer Umgebung mit Log Analytics](../log-analytics/log-analytics-concept-hybrid.md).
- Informationen zum Konfigurieren der Datensammlung von virtuellen Azure-Computern finden Sie unter [Sammeln von Daten über virtuelle Azure-Computer](../log-analytics/log-analytics-quick-collect-azurevm.md). 

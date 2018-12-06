---
title: Übersicht über Azure Service Fabric-Überwachung und -Diagnose | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Überwachung und Diagnose für Cluster, Anwendungen und Dienste von Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 82c02c0212fd79d8847d374022b6ac8f862f042a
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291112"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Überwachung und Diagnose für Azure Service Fabric

Dieser Artikel enthält eine Übersicht über die Überwachung und Diagnose für Azure Service Fabric. Die Überwachung und Diagnose sind wichtig für die Entwicklung und Durchführung von Tests sowie für die Bereitstellung von Workloads in beliebigen Cloudumgebungen. So können Sie beispielsweise die Nutzung Ihrer Anwendungen, die Aktionen der Service Fabric-Plattform, die Ressourcenverwendung (mithilfe von Leistungsindikatoren) sowie die allgemeine Integrität Ihres Clusters nachverfolgen. Auf der Grundlage dieser Informationen können Sie Probleme diagnostizieren, beheben und in Zukunft vermeiden. In den folgenden Abschnitten werden kurz die einzelnen Bereiche der Service Fabric-Überwachung erläutert, die für Produktionsworkloads relevant sind. 

## <a name="application-monitoring"></a>Anwendungsüberwachung
Bei der Anwendungsüberwachung wird nachverfolgt, wie die Funktionen und Komponenten Ihrer Anwendung genutzt werden. In der Regel überwachen Sie Anwendungen auf Probleme, die die Benutzer beeinträchtigen könnten. Für die Anwendungsüberwachung sind die Benutzer zuständig, die eine Anwendung und die zugehörigen Dienste entwickeln, da die Überwachung stark von der Geschäftslogik Ihrer Anwendung abhängig ist. Das Überwachen Ihrer Anwendungen kann in den folgenden Szenarien hilfreich sein:
* Wie hoch ist das Datenverkehrsaufkommen für meine Anwendung? (Müssen Sie Ihre Dienste skalieren, um Benutzeranforderungen gerecht zu werden oder einen möglichen Engpass in der Anwendung zu beheben?)
* Sind Aufrufe zwischen Diensten erfolgreich, und werden sie nachverfolgt?
* Welche Aktionen führen die Benutzer meiner Anwendung aus? (Die Erfassung von Telemetriedaten kann bei der Entwicklung zukünftiger Features und bei der Diagnose von Anwendungsfehlern hilfreich sein.)
* Treten in meiner Anwendung unbehandelte Ausnahmen auf? 
* Was passiert in den Diensten, die in meinem Container ausgeführt werden?

Das Praktische an der Anwendungsüberwachung ist, dass Entwickler beliebige Tools und Frameworks verwenden können, da sie innerhalb des Kontexts Ihrer Anwendung stattfindet. Weitere Informationen zur Azure-Lösung für die Anwendungsüberwachung mit Azure Monitor bzw. Application Insights finden Sie unter [Ereignisanalyse und Visualisierung mit Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).
Darüber hinaus erfahren Sie in [diesem Tutorial](service-fabric-tutorial-monitoring-aspnet.md), wie Sie diese Lösung für .NET-Anwendungen einrichten. Das Tutorial zeigt, wie Sie die richtigen Tools installieren. Außerdem erfahren Sie anhand eines Beispiels, wie Sie benutzerdefinierte Telemetrie in Ihrer Anwendung schreiben, und es wird gezeigt, wie Sie die Anwendungsdiagnose und die Telemetriedaten im Azure-Portal anzeigen. 


## <a name="platform-cluster-monitoring"></a>Überwachung der Plattform (bzw. des Clusters)
Ein Benutzer hat die Kontrolle über die Telemetriedaten aus seiner Anwendung, da er den Code selbst schreibt. Aber wie verhält es sich mit den Diagnosen der Service Fabric-Plattform? Ein Ziel von Service Fabric besteht darin, für Anwendungen die Resilienz in Bezug auf Hardwarefehler sicherzustellen. Dies ist möglich, weil die Systemdienste der Plattform in der Lage sind, Infrastrukturprobleme zu erkennen und für Workloads schnell ein Failover auf andere Knoten im Cluster durchzuführen. In diesem speziellen Fall stellt sich aber die Frage, was geschieht, wenn die Systemdienste selbst Probleme haben? Oder was geschieht, wenn beim Bereitstellen oder Verschieben einer Workload Regeln für die Platzierung von Diensten verletzt werden? Service Fabric bietet Diagnosen für diese und viele andere Fälle, um sicherzustellen, dass Sie über die Aktivitäten in Ihrem Cluster informiert sind. Zu den Beispielszenarien für die Clusterüberwachung zählen unter anderem folgende:

* Verhält sich Service Fabric bei der Platzierung von Anwendungen und der gleichmäßigen Verteilung der Workloads im Cluster wie erwartet? 
* Werden Benutzeraktionen in Ihrem Cluster bestätigt und erwartungsgemäß durchgeführt? Beispiel: Skalierung, Failover, Bereitstellungen
* Verfolgt Service Fabric nach, welche Knoten Teil des Clusters sind, und werde ich informiert, wenn ein Problem mit einem Knoten vorliegt?

Die Diagnosen stehen bereits standardmäßig als umfassender Satz von Ereignissen zur Verfügung. Diese [Service Fabric-Ereignisse](service-fabric-diagnostics-events.md) stellen Aktionen dar, die von der Plattform für unterschiedliche Entitäten wie Knoten, Anwendungen, Dienste, Partitionen usw. ausgeführt wurden. Wenn im letzten der obigen Szenarien ein Knoten ausfällt, gibt die Plattform ein Ereignis vom Typ `NodeDown` aus, und Sie werden umgehend von Ihrem bevorzugten Überwachungstool informiert. Andere gängige Beispiele wären etwa `ApplicationUpgradeRollbackStarted` oder `PartitionReconfigured` während eines Failovers. **In Windows- und Linux-Clustern stehen jeweils die gleichen Ereignisse zur Verfügung.**

Die Ereignisse werden sowohl unter Windows als auch unter Linux über Standardkanäle gesendet und können von jedem Überwachungstool gelesen werden, das diese unterstützt. Die Azure Monitor-Lösung ist Log Analytics. Sie können sich gerne ausführlicher über unsere [Log Analytics-Integration](service-fabric-diagnostics-event-analysis-oms.md) informieren. Diese beinhaltet ein benutzerdefiniertes Betriebsdashboard für Ihren Cluster sowie einige Beispielabfragen, die Sie als Grundlage für die Erstellung von Benachrichtigungen verwenden können. Weitere Konzepte für die Clusterüberwachung finden Sie unter [Monitoring the cluster](service-fabric-diagnostics-event-generation-infra.md) (Überwachen des Clusters).

### <a name="health-monitoring"></a>Systemüberwachung
Die Service Fabric-Plattform umfasst ein Integritätsmodell, das erweiterbare Integritätsberichtsfunktionen für den Status von Entitäten in einem Cluster bereitstellt. Alle Knoten, Anwendungen, Dienste, Partitionen, Replikate oder Instanzen verfügen über einen kontinuierlich aktualisierbaren Integritätsstatus. Der Integritätsstatus kann entweder „OK“, „Warnung“ oder „Fehler“ lauten. Service Fabric-Ereignisse können Sie sich als Tätigkeiten vorstellen, die der Cluster für verschiedene Entitäten ausführt, und die Integrität als Eigenschaft der jeweiligen Entität. Bei jeder Änderung der Integrität einer bestimmten Entität wird auch ein Ereignis ausgegeben. Dadurch können Sie in Ihrem bevorzugten Überwachungstool Abfragen und Benachrichtigungen für Integritätsereignisse einrichten (genau wie für andere Ereignisse). 

Benutzer können sogar die Integrität für Entitäten überschreiben. Wenn für Ihre Anwendung ein Upgrade ausgeführt wird und bei Validierungstests Fehler auftreten, können Sie den Service Fabric-Integritätsdienst mithilfe der Health-API darüber informieren, dass Ihre Anwendung nicht mehr fehlerfrei ist, woraufhin Service Fabric automatisch ein Rollback des Upgrades durchführt. Weitere Informationen zum Integritätsmodell finden Sie in der [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md).

![Integritätsdashboard von SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


### <a name="watchdogs"></a>Watchdogs
Ein Watchdog ist im Allgemeinen ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen, Endpunkte pingen und Meldungen zur Integrität aller im Cluster enthaltenen Elemente zurückgeben kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung ohne Benutzeraktion ausführt (z.B. Bereinigen von Protokolldateien im Speicher in bestimmten Zeitintervallen). Ein Beispiel für eine Watchdog-Dienstimplementierung finden Sie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="infrastructure-performance-monitoring"></a>Überwachung der Infrastruktur (Leistung)
Nachdem Sie nun mit den Diagnosen auf Anwendungs- und Plattformebene vertraut sind, stellt sich die Frage, wie Sie feststellen können, ob die Hardware wie erwartet funktioniert. Die Überwachung der zugrunde liegenden Infrastruktur ist wichtig, um einen Eindruck vom Zustand des Clusters und der Ressourcenverwendung zu erhalten. Die Messung der Systemleistung hängt je nach Workloads von zahlreichen (teilweise subjektiven) Faktoren ab. Solche Faktoren werden in der Regel mithilfe von Leistungsindikatoren gemessen. Diese Leistungsindikatoren können aus verschiedensten Quellen stammen – etwa vom Betriebssystem, aus .NET Framework oder von der Service Fabric-Plattform selbst. Sie können unter anderem zur Beantwortung folgender Fragen hilfreich sein:

* Nutze ich meine Hardware effizient? Möchten Sie Ihre CPU zu 90 Prozent auslasten oder nur zu zehn Prozent? Dies ist hilfreich, wenn Sie Ihren Cluster skalieren oder die Prozesse Ihrer Anwendung optimieren.
* Kann ich Infrastrukturprobleme proaktiv vorhersagen? Vielen Problemen geht eine plötzliche Veränderung im Leistungsverhalten (Leistungsabfall) voraus, wodurch sich Probleme mithilfe von Leistungsindikatoren (beispielsweise zu Netzwerk-E/A und CPU-Auslastung) proaktiv vorhersagen und diagnostizieren lassen.

Eine Liste der Leistungsindikatoren, die auf Infrastrukturebene erfasst werden sollten, finden Sie unter [Leistungsmetriken](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric umfasst auch eine Reihe von Leistungsindikatoren für die Programmiermodelle „Reliable Services“ und „Reliable Actors“. Bei Verwendung eines dieser Modelle können die Leistungsindikatoren Informationen liefern, die sicherstellen, dass Actors ordnungsgemäß hoch- und heruntergefahren oder Reliable Services-Anforderungen schnell genug verarbeitet werden. Weitere Informationen finden Sie unter [Diagnose und Leistungsüberwachung für Reliable ServiceRemoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) und [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). 

Die Azure Monitor-Lösung zur Erfassung dieser Informationen ist Log Analytics (genau wie bei der Überwachung auf der Plattformebene). Verwenden Sie den [Log Analytics-Agent](service-fabric-diagnostics-oms-agent.md), um geeignete Leistungsindikatoren zu erfassen, und Log Analytics, um sie anzuzeigen.

## <a name="recommended-setup"></a>Empfohlene Einrichtung
Nachdem Sie nun mit den einzelnen Bereichen für die Überwachung sowie mit entsprechenden Beispielszenarien vertraut sind, finden Sie im Anschluss eine Zusammenfassung der Azure-Überwachungstools und der Einrichtungsschritte, die für die Überwachung aller oben genannten Bereiche erforderlich sind. 

* Anwendungsüberwachung mit [Application Insights](service-fabric-tutorial-monitoring-aspnet.md)
* Clusterüberwachung mit [Diagnose-Agent](service-fabric-diagnostics-event-aggregation-wad.md) und [Log Analytics](service-fabric-diagnostics-oms-setup.md)
* Infrastrukturüberwachung mit [Log Analytics](service-fabric-diagnostics-oms-agent.md)

Sie können auch die [ARM-Beispielvorlage](service-fabric-diagnostics-oms-setup.md#deploy-log-analytics-with-azure-resource-manager) verwenden und anpassen, um die Bereitstellung aller erforderlichen Ressourcen und Agents zu automatisieren. 

## <a name="other-logging-solutions"></a>Weitere Protokollierungslösungen

Die beiden empfohlenen Lösungen ([Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) und [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)) verfügen zwar über eine integrierte Integration in Service Fabric, aber viele Ereignisse werden über ETW-Anbieter geschrieben und können um andere Protokollierungslösungen erweitert werden. Unter Umständen kommt für Sie auch [Elastic Stack](https://www.elastic.co/products) (insbesondere, wenn Sie einen Cluster in einer Offlineumgebung ausführen möchten), [Dynatrace](https://www.dynatrace.com/) oder eine andere Plattform infrage. Eine Liste mit den integrierten Lösungen von Partnerunternehmen finden Sie [hier](service-fabric-diagnostics-partners.md).

Wichtige Aspekte für jede Plattform sind unter anderem, wie gut Sie mit der Benutzeroberfläche, den Abfragefunktionen, den verfügbaren benutzerdefinierten Visualisierungen und Dashboards sowie mit den zusätzlichen Tools zur Erweiterung der Überwachung zurechtkommen. 

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich unter [Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene](service-fabric-diagnostics-event-generation-app.md), falls Sie mit dem Instrumentieren Ihrer Anwendungen beginnen möchten.
* Führen Sie die Schritte unter [Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) aus, um Application Insights für Ihre Anwendung einzurichten.
* Weitere Informationen zum Überwachen der Plattform und der von Service Fabric bereitgestellten Ereignisse finden Sie unter [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md).
* Konfigurieren Sie die Log Analytics-Integration für Service Fabric gemäß der Anleitung unter [Einrichten von Log Analytics für einen Cluster](service-fabric-diagnostics-oms-setup.md).
* Informieren Sie sich unter [Tutorial: Überwachen von Windows-Containern in Service Fabric mit Log Analytics](service-fabric-tutorial-monitoring-wincontainers.md) darüber, wie Sie Log Analytics für die Überwachung von Containern einrichten.
* Beispiele für Diagnoseprobleme und die entsprechenden Lösungen mit Service Fabric finden Sie unter [Diagnostizieren häufiger Szenarien mit Service Fabric](service-fabric-diagnostics-common-scenarios.md).
* Informationen zu anderen Diagnoseprodukten, die in Service Fabric integriert werden können, finden Sie unter [Azure Service Fabric Monitoring Partners](service-fabric-diagnostics-partners.md) (Partner für Azure Service Fabric-Überwachung).
* Informieren Sie sich über die allgemeinen Empfehlungen zur Überwachung von Azure-Ressourcen: [Überwachung und Diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
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
ms.date: 10/18/2018
ms.author: srrengar
ms.openlocfilehash: 5fc2674a145be99fb8867c5cf1b1f65ba860db80
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457832"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Überwachung und Diagnose für Azure Service Fabric

Dieser Artikel enthält eine Übersicht über die Überwachung und Diagnose für Azure Service Fabric. Die Überwachung und Diagnose sind wichtig für die Entwicklung und Durchführung von Tests sowie für die Bereitstellung von Workloads in beliebigen Cloudumgebungen. Durch die Überwachung können Sie nachverfolgen, wie Ihre Anwendungen und Ressourcen genutzt werden, und erhalten einen Überblick über die Clusterintegrität. Mithilfe dieser Informationen lassen sich Probleme diagnostizieren und beheben und in Zukunft vermeiden. 

## <a name="application-monitoring"></a>Anwendungsüberwachung
Bei der Anwendungsüberwachung wird nachverfolgt, wie die Funktionen und Komponenten Ihrer Anwendung genutzt werden. In der Regel überwachen Sie Anwendungen auf Probleme, die die Benutzer beeinträchtigen könnten. Das Überwachen Ihrer Anwendungen kann in den folgenden Szenarien hilfreich sein:
* Bestimmen der Anwendungslast und des Benutzerdatenverkehrs – Müssen Sie Ihre Dienste skalieren, um Benutzeranforderungen gerecht zu werden oder einen möglichen Engpass in der Anwendung zu beheben?
* Identifizieren von Problemen, die bei der Dienstkommunikation und beim Remoting im gesamten Cluster auftreten können
* Ermitteln, zu welchem Zweck Benutzer Ihre Anwendung einsetzen – Das Sammeln von Telemetriedaten in Ihren Anwendungen kann eine Orientierungshilfe für die Entwicklung künftiger Funktionen sein und die Diagnose von App-Fehlern verbessern.
* Überwachen der Vorgänge in Ihren ausgeführten Containern

Service Fabric unterstützt viele Optionen zum Instrumentieren Ihres Anwendungscodes mit den passenden Ablaufverfolgungen und Telemetriefunktionen. Wir empfehlen Ihnen die Verwendung von Application Insights (AI). Die Integration von AI in Service Fabric enthält Tooloberflächen für Visual Studio und das Azure-Portal sowie spezifische Service Fabric-Metriken, sodass Sie eine umfassende fertige Protokollierungsumgebung erhalten. Bei AI werden viele Protokolle zwar automatisch für Sie erstellt und erfasst, aber wir raten Ihnen, Ihren Anwendungen eine weiter gehende benutzerdefinierte Protokollierung hinzuzufügen, um für eine vollständigere Diagnoseumgebung zu sorgen. Weitere Informationen zum Einstieg in Application Insights mit Service Fabric finden Sie unter [Ereignisanalyse und Visualisierung mit Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>Überwachung der Plattform (bzw. des Clusters)
Durch die Überwachung des Service Fabric-Clusters soll sichergestellt werden, dass die Plattform und alle Workloads bestimmungsgemäß arbeiten. Ein Ziel von Service Fabric besteht darin, für Anwendungen die Resilienz in Bezug auf Hardwarefehler sicherzustellen. Dies ist möglich, weil die Systemdienste der Plattform in der Lage sind, Infrastrukturprobleme zu erkennen und für Workloads schnell ein Failover auf andere Knoten im Cluster durchzuführen. In diesem speziellen Fall stellt sich aber die Frage, was geschieht, wenn die Systemdienste selbst Probleme haben? Oder was geschieht, wenn beim Verschieben einer Workload Regeln für die Platzierung von Diensten verletzt werden? Durch die Clusterüberwachung bleiben Sie über alle Aktivitäten in Ihrem Cluster informiert, was die Diagnose und effiziente Behandlung von Problemen erleichtert. Im Folgenden einige wichtige Faktoren, die Sie beachten sollten:
* Verhält sich Service Fabric bei der Platzierung von Anwendungen und der gleichmäßigen Verteilung der Workloads im Cluster erwartungsgemäß? 
* Werden Benutzeraktionen in Ihrem Cluster bestätigt und erwartungsgemäß durchgeführt? Dies ist besonders beim Skalieren eines Clusters von Bedeutung.
* Werden Ihre Daten und die Kommunikation zwischen Diensten innerhalb des Clusters von Service Fabric ordnungsgemäß verwaltet?

Service Fabric bietet einen umfassenden Satz von sofort einsatzfähigen Ereignissen. Der Zugriff auf diese [Service Fabric-Ereignisse](service-fabric-diagnostics-events.md) erfolgt entweder über die EventStore-APIs oder den Betriebskanal (der von der Plattform bereitgestellte Ereigniskanal). 
* EventStore: In EventStore (verfügbar unter Windows in den Versionen 6.2 und höher, Linux bei der letzten Aktualisierung des Artikels noch in Vorbereitung) werden diese Ereignisse über eine Reihe von APIs (Zugriff über REST-Endpunkte oder über die Clientbibliothek) verfügbar gemacht. Weitere Informationen zu EventStore finden Sie unter [Übersicht über EventStore](service-fabric-diagnostics-eventstore.md).
* Service Fabric-Ereigniskanäle: Unter Windows stehen Service Fabric-Ereignisse von einem einzelnen ETW-Anbieter mit einem Satz relevanter `logLevelKeywordFilters` zur Verfügung, die für die Auswahl von Betriebs-, Daten- oder Messaging-Kanal verwendet werden. Auf diese Art und Weise werden ausgehende Service Fabric-Ereignisse separiert, und können nach Bedarf gefiltert werden. Unter Linux laufen alle Service Fabric-Ereignisse über LTTng und werden in eine Speicherabelle eingefügt, in der sie nach Bedarf gefiltert werden können. Diese Kanäle enthalten geordnete, strukturierte Ereignisse, mit deren Hilfe sich der Zustand des Clusters besser nachvollziehen lässt. Die Diagnose ist bei der Erstellung des Clusters standardmäßig aktiviert. Hierbei wird eine Azure Storage-Tabelle erstellt, an die die Ereignisse aus diesen Kanälen gesendet werden und aus der in Zukunft Ereignisse abgefragt werden können. 

Wir empfehlen die Verwendung von EventStore für eine schnelle Analyse und eine Momentaufnahme der Arbeitsweise des Clusters. Anhand dieser Einblicke können Sie feststellen, ob alles erwartungsgemäß funktioniert. Zum Erfassen der Protokolle und Ereignisse, die von Ihrem Cluster generiert werden, empfehlen wir im Allgemeinen die Verwendung der [Azure-Diagnoseerweiterung](service-fabric-diagnostics-event-aggregation-wad.md). Die Erweiterung kann gut in die Service Fabric-Analyse (spezifische Service-Fabric-Lösung von Log Analytics) integriert werden, die ein benutzerdefiniertes Dashboard für die Überwachung von Service Fabric-Clustern bereitstellt und das Abfragen von Clusterereignissen und das Einrichten von Warnungen ermöglicht. Weitere Informationen finden Sie unter [Ereignisanalyse und -visualisierung mit Log Analytics](service-fabric-diagnostics-event-analysis-oms.md). 

 Weitere Informationen zum Überwachen des Clusters finden Sie unter [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md).

## <a name="performance-monitoring"></a>Leistungsüberwachung
Die Überwachung der zugrunde liegenden Infrastruktur ist wichtig, um einen Eindruck vom Zustand des Clusters und der Ressourcenverwendung zu erhalten. Die Messung der Systemleistung hängt von mehreren Faktoren ab, die normalerweise jeweils durch einen eigenen KPI (Key Performance Indicator) bestimmt werden. KPIs, die für Service Fabric relevant sind, können Metriken in Form von Leistungsindikatoren zugeordnet werden. Die Metriken können dabei aus den Knoten im Cluster erfasst werden.
KPIs können helfen,
* Ermitteln der Ressourcenverwendung und -auslastung, beispielsweise für die Clusterskalierung oder die Optimierung der Dienstprozesse
* Vorhersagen von Infrastrukturproblemen: Vielen Problemen geht eine plötzliche Veränderung im Leistungsverhalten (Leistungsabfall) voraus, sodass mithilfe von KPIs, beispielsweise zur Netzwerk-E/A und CPU-Auslastung, Infrastrukturprobleme vorhergesagt und diagnostiziert werden können

Eine Liste der Leistungsindikatoren, die auf Infrastrukturebene erfasst werden sollten, finden Sie unter [Leistungsmetriken](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric umfasst eine Reihe von Leistungsindikatoren für die Programmiermodelle „Reliable Services“ und „Reliable Actors“. Bei Verwendung eines dieser Modelle können die Leistungsindikatoren KPIs bereitstellen, die sicherstellen, dass die Akteure ordnungsgemäß hoch- und heruntergefahren oder dass Reliable Services-Anforderungen schnell genug verarbeitet werden. Weitere Informationen finden Sie unter [Diagnose und Leistungsüberwachung für Reliable ServiceRemoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) und [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). Darüber hinaus bietet Application Insights ebenfalls eine Reihe von Leistungsmetriken, die erfasst werden, sofern sie mit Ihrer Anwendung konfiguriert wurden.

Verwenden Sie den [Log Analytics-Agent](service-fabric-diagnostics-oms-agent.md), um die geeigneten Leistungsindikatoren zu erfassen, und Azure Log Analytics, um diese KPIs anzuzeigen.

![Übersichtsdiagramm zur Diagnose](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Systemüberwachung
Die Service Fabric-Plattform umfasst ein Integritätsmodell, das erweiterbare Integritätsberichtsfunktionen für den Status von Entitäten in einem Cluster bereitstellt. Alle Knoten, Anwendungen, Dienste, Partitionen, Replikate oder Instanzen verfügen über einen kontinuierlich aktualisierbaren Integritätsstatus. Der Integritätsstatus kann entweder „OK“, „Warnung“ oder „Fehler“ lauten. Der Integritätsstatus wird durch Integritätsberichte geändert, die für jede Entität basierend auf Problemen im Cluster ausgegeben werden. Der Integritätsstatus der Entitäten kann jederzeit im Service Fabric Explorer (SFX) überprüft werden, wie unten dargestellt. Alternativ kann er über die Integritäts-API der Plattform abgefragt werden. Sie haben auch die Möglichkeit, Integritätsberichte anzupassen und den Integritätsstatus einer Entität zu ändern, indem Sie eigene Integritätsberichte hinzufügen oder die Integritäts-API nutzen. Weitere Informationen zum Integritätsmodell finden Sie unter [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md).

![Integritätsdashboard von SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Sie können die aktuellen Integritätsberichte in SFX einsehen, zusätzlich steht jeder Bericht aber auch als Ereignis zur Verfügung. Integritätsereignisse können über den Betriebskanal erfasst (siehe [Ereignisaggregation und -sammlung mit der Windows Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) und in Log Analytics gespeichert werden. So stehen sie zukünftig für Warnungen und Abfragen zur Verfügung. Auf diese Weise lassen sich Probleme, die unter Umständen die Anwendungsverfügbarkeit beeinträchtigen, leichter erkennen. Daher wird empfohlen, Warnungen für entsprechende Fehlerszenarien einzurichten (benutzerdefinierte Warnungen über Log Analytics).

## <a name="other-logging-solutions"></a>Weitere Protokollierungslösungen

Die beiden empfohlenen Lösungen ([Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) und [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)) verfügen zwar über eine integrierte Integration in Service Fabric, aber viele Ereignisse werden über ETW-Anbieter geschrieben und können um andere Protokollierungslösungen erweitert werden. Unter Umständen kommt für Sie auch [Elastic Stack](https://www.elastic.co/products) (insbesondere, wenn Sie einen Cluster in einer Offlineumgebung ausführen möchten), [Dynatrace](https://www.dynatrace.com/) oder eine andere Plattform infrage. Eine Liste mit den integrierten Lösungen von Partnerunternehmen finden Sie [hier](service-fabric-diagnostics-partners.md).

Wichtige Punkte für die Auswahl einer Plattform sollten u.a. sein, wie Sie mit der Benutzeroberfläche und den Abfrageoptionen zurechtkommen, welche Optionen zum Visualisieren von Daten und Erstellen übersichtlicher Dashboards enthalten sind und welche zusätzlichen Tools zur Optimierung der Überwachung zur Verfügung stehen (z.B. automatische Warnungen).

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich unter [Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene](service-fabric-diagnostics-event-generation-app.md), falls Sie mit dem Instrumentieren Ihrer Anwendungen beginnen möchten.
* Weitere Informationen zum Überwachen der Plattform und der von Service Fabric bereitgestellten Ereignisse finden Sie unter [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md).
* Führen Sie die Schritte zum Einrichten von AI für Ihre Anwendung unter [Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) aus.
* Erfahren Sie, wie Sie OMS Log Analytics zum Überwachen von Containern einrichten: [Überwachen von Windows-Containern unter Service Fabric mit OMS](service-fabric-tutorial-monitoring-wincontainers.md).
* Beispiele für Diagnoseprobleme und die entsprechenden Lösungen mit Service Fabric finden Sie unter [Diagnostizieren häufiger Szenarien mit Service Fabric](service-fabric-diagnostics-common-scenarios.md).
* Informationen zu anderen Diagnoseprodukten, die in Service Fabric integriert werden können, finden Sie unter [Azure Service Fabric Monitoring Partners](service-fabric-diagnostics-partners.md) (Partner für Azure Service Fabric-Überwachung).
* Informieren Sie sich über die allgemeinen Empfehlungen zur Überwachung von Azure-Ressourcen: [Überwachung und Diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 

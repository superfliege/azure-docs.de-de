---
title: Übersicht über Azure Service Fabric-Überwachung und -Diagnose | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Überwachung und Diagnose für Cluster, Anwendungen und Dienste von Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: dekapur
ms.openlocfilehash: f784576547f0d85a825ad9dd107c6c84cd261092
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Überwachung und Diagnose für Azure Service Fabric

Dieser Artikel bietet einen Überblick darüber, wie Sie die Überwachung und Diagnose für Anwendungen einrichten, die in Service Fabric-Clustern ausgeführt werden. Die Überwachung und Diagnose sind wichtig für die Entwicklung und Durchführung von Tests sowie für die Bereitstellung von Workloads in beliebigen Cloudumgebungen. Durch die Überwachung können Sie nachverfolgen, wie Ihre Anwendungen und Ressourcen genutzt werden, und erhalten einen Überblick über die Clusterintegrität. Mithilfe dieser Informationen lassen sich Probleme innerhalb des Clusters diagnostizieren und beheben und zukünftige Probleme vermeiden. 

Die Hauptziele der Überwachung und Diagnose sind:
* Erkennen und Diagnostizieren von Infrastrukturproblemen
* Erkennen von Anwendungsproblemen
* Nachvollziehen des Ressourcenverbrauchs
* Nachverfolgen der Anwendungs-, Dienst- und Infrastrukturleistung

In einem Service Fabric-Cluster werden Überwachungs- und Diagnosedaten auf drei Ebenen erfasst: Anwendung, Plattform (Cluster) und Infrastruktur. 
* Die [Anwendungsebene](service-fabric-diagnostics-event-generation-app.md) umfasst Leistungsdaten zu Ihren Anwendungen und zusätzliche benutzerdefinierte Protokolle, die Sie konfiguriert haben. Daten der Anwendungsüberwachung sollten von der Anwendung selbst an Application Insights (AI) übermittelt werden. Sie können über das AI SDK, EventFlow oder eine andere beliebige Pipeline übertragen werden.
* Die [Plattformebene](service-fabric-diagnostics-event-generation-infra.md) umfasst Ereignisse, die durch Aktionen im Cluster ausgelöst werden und sich auf die Verwaltung des Clusters und der darin ausgeführten Anwendungen beziehen. Daten der Plattformüberwachung sollten an OMS Log Analytics gesendet werden, wobei die eingehenden Ereignisse mithilfe der Service Fabric-Analyselösung visualisiert werden können. Diese Daten werden in der Regel über die Azure-Diagnoseerweiterung für Windows oder Linux an ein Speicherkonto gesendet. OMS Log Analytics greift über die jeweilige Erweiterung auf die Daten zu. 
* Auf der Infrastrukturebene liegt der Schwerpunkt auf der [Leistungsüberwachung](service-fabric-diagnostics-event-generation-perf.md). Dabei werden wichtige Metriken und Leistungsindikatoren analysiert, um die Ressourcenverwendung und -auslastung zu ermitteln. Die Leistung kann mithilfe eines Agents überwacht werden – wir empfehlen die Verwendung des OMS (Microsoft Monitoring)-Agents. So ist sichergestellt, dass die Leistungsdaten an denselben Ort wie die Plattformereignisse übermittelt werden, was die Diagnose erleichtert, da Änderungen clusterweit korreliert werden können. 

![Übersichtsdiagramm zur Diagnose](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="monitoring-scenarios"></a>Überwachungsszenarien

In diesem Abschnitt werden wichtige Szenarien zur Überwachung eines Service Fabric-Clusters beschrieben, die sich auf die Anwendungs-, Cluster-, Leistungs- und Integritätsüberwachung beziehen. Bei jedem Überwachungsszenario werden der Zweck und die allgemeine Herangehensweise erläutert. Ausführlichere Informationen hierzu und weitere allgemeine Empfehlungen zur Überwachung von Azure-Ressourcen finden Sie unter [Bewährte Methoden – Überwachung und Diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 

Diese Szenarien lassen sich auch grob den drei Überwachungsebenen und den zugehörigen Diagnosedaten zuordnen, die zuvor beschrieben wurden. Das bedeutet, dass die Überwachungs- und Diagnosedaten jeweils auf der entsprechenden Ebene eingehen sollten, damit die einzelnen Szenarien im Cluster ordnungsgemäß funktionieren. Das Szenario für die Integritätsüberwachung bildet hier eine Ausnahme, da es sich auf den gesamten Cluster und alle darin ausgeführten Elemente bezieht.

## <a name="application-monitoring"></a>Anwendungsüberwachung
Bei der Anwendungsüberwachung wird nachverfolgt, wie die Funktionen und Komponenten einer von Ihnen erstellten Anwendung genutzt werden. In der Regel überwachen Sie Anwendungen auf Probleme, die die Benutzer beeinträchtigen könnten. Die Anwendungsüberwachung kann in folgenden Fällen hilfreich sein:
* Bestimmen der Anwendungslast und des Benutzerdatenverkehrs – Müssen Sie Ihre Dienste skalieren, um Benutzeranforderungen gerecht zu werden oder einen möglichen Engpass in der Anwendung zu beheben?
* Identifizieren von Problemen, die bei der Dienstkommunikation und beim Remoting im gesamten Cluster auftreten können
* Ermitteln, zu welchem Zweck Benutzer Ihre Anwendung einsetzen – Die Instrumentierung Ihrer Anwendungen kann eine Orientierungshilfe für die Entwicklung künftiger Funktionen sein und die Diagnose von App-Fehlern verbessern.

Für die Überwachung auf Anwendungsebene in Service Fabric empfehlen wir die Verwendung von Application Insights (AI). Die Integration von Application Insights in Service Fabric umfasst Tools für Visual Studio und das Azure-Portal. Das AI-Dashboard und die Anwendungsübersicht enthalten darüber hinaus Informationen zu Service Fabric-Dienstkontext und -Remoting und ermöglichen so eine umfassende, sofort einsatzbereite Protokollierung. Obwohl viele Protokolle bei der Verwendung von AI automatisch erstellt und erfasst werden, empfehlen wir, Ihre Anwendungen mit zusätzlichen, benutzerdefinierten Protokollierungsfunktionen auszustatten und die Ergebnisse zusätzlich zu den in AI bereitgestellten Daten darzustellen, um die Diagnosemöglichkeiten auszuweiten und die zukünftige Problembehandlung zu optimieren. Weitere Informationen zur Verwendung von AI mit Service Fabric finden Sie unter [Ereignisanalyse und Visualisierung mit Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md). 

Falls Sie direkt mit dem Instrumentieren des Codes zum Überwachen von Anwendungen beginnen möchten, fahren Sie mit [Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene](service-fabric-diagnostics-event-generation-app.md) fort. 

### <a name="monitoring-application-availability"></a>Überwachen der Anwendungsverfügbarkeit
Es kann vorkommen, dass Ihre Anwendungen nicht verfügbar sind oder dass nicht darauf zugegriffen werden kann, obwohl alle Clusterelemente anscheinend erwartungsgemäß ausgeführt werden und keine Fehler gemeldet wurden. Obwohl diese Situation nicht häufig auftreten dürfte, sollten Sie in einem solchen Fall so schnell wie möglich darauf aufmerksam werden, um das Problem einzudämmen. Bei der Verfügbarkeitsüberwachung wird im Wesentlichen die Verfügbarkeit Ihrer Systemkomponenten nachverfolgt, um die „Gesamtbetriebszeit“ des Systems zu ermitteln. In einem Cluster geht es hauptsächlich um die Verfügbarkeit aus der Anwendungsperspektive – die Plattform sorgt dafür, dass Application Lifecycle Management-Szenarien keine Downtime verursachen. Trotzdem können verschiedene Probleme im Cluster die Betriebszeit Ihrer Anwendung beeinträchtigen, was Sie als Besitzer der Anwendung natürlich so schnell wie möglich wissen möchten. Es wird empfohlen, neben allen anderen Anwendungen auch einen Watchdog in Ihrem Cluster bereitzustellen. Die Aufgabe eines Watchdogs ist es, die geeigneten Endpunkte für Ihre Anwendung in einem festgelegten Zeitintervall zu überprüfen und zurückzumelden, dass darauf zugegriffen werden kann. Sie können dazu auch einen externen Dienst verwenden, der den Endpunkt pingt und im angegebene Zeitintervall einen Bericht zurückgibt.

## <a name="cluster-monitoring"></a>Clusterüberwachung
Durch die Überwachung des Service Fabric-Clusters soll sichergestellt werden, dass die Plattform und alle darauf ausgeführten Workloads bestimmungsgemäß arbeiten. Ein Ziel von Service Fabric besteht darin sicherzustellen, dass Anwendungen trotz Hardwarefehlern weiterhin ausgeführt werden. Dies ist möglich, weil die Systemdienste der Plattform in der Lage sind, Infrastrukturprobleme zu erkennen, und für Workloads schnell ein Failover auf andere Knoten im Cluster ausführen können. In diesem speziellen Fall stellt sich aber die Frage, was geschieht, wenn die Systemdienste selbst Probleme haben? Oder was geschieht, wenn beim Verschieben einer Workload Regeln für die Platzierung von Diensten verletzt werden? Durch die Clusterüberwachung bleiben Sie über alle Aktivitäten in Ihrem Cluster informiert, was die Diagnose und effiziente Behandlung von Problemen erleichtert. Im Folgenden einige wichtige Faktoren, die Sie beachten sollten:
* Verhält sich Service Fabric bei der Platzierung von Anwendungen und der gleichmäßigen Verteilung der Workloads im Cluster erwartungsgemäß? 
* Werden Maßnahmen zum Ändern der Clusterkonfiguration erwartungsgemäß bestätigt und umgesetzt? Dies ist besonders beim Skalieren eines Clusters von Bedeutung.
* Werden Ihre Daten und die Kommunikation zwischen Diensten innerhalb des Clusters von Service Fabric ordnungsgemäß verwaltet?

Service Fabric bietet über die Betriebs-, Daten- und Messagingkanäle eine umfassende Reihe vordefinierter Ereignisse. In Windows haben sie die Form eines einzelnen ETW-Anbieters mit einem Satz relevanter `logLevelKeywordFilters`, die zum Auswählen zwischen verschiedenen Kanälen verwendet werden. Unter Linux laufen alle Plattformereignisse über LTTng und werden in eine Tabelle eingefügt, in der sie nach Bedarf gefiltert werden können. 

Diese Kanäle enthalten geordnete, strukturierte Ereignisse, mit deren Hilfe sich der Zustand des Clusters besser nachvollziehen lässt. Die Diagnose ist bei der Erstellung des Clusters standardmäßig aktiviert. Dabei wird eine Azure Storage-Tabelle eingerichtet, an die die Ereignisse aus diesen Kanälen gesendet werden und aus der in Zukunft Ereignisse abgefragt werden können. Weitere Informationen zum Überwachen des Clusters finden Sie unter [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md). Für die Clusterüberwachung wird OMS Log Analytics empfohlen. OMS Log Analytics bietet eine Service Fabric-spezifische Lösung, die Service Fabric-Analyse, die ein benutzerdefiniertes Dashboard für die Überwachung von Service Fabric-Clustern umfasst, sowie die Möglichkeit, Clusterereignisse abzufragen und Warnungen einzurichten. Weitere Informationen finden Sie unter [Ereignisanalyse und Visualisierung mit OMS](service-fabric-diagnostics-event-analysis-oms.md). 

### <a name="watchdogs"></a>Watchdogs
Ein Watchdog ist im Allgemeinen ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen, Endpunkte pingen und Meldungen zur Integrität aller im Cluster enthaltenen Elemente zurückgeben kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung ohne Benutzeraktion ausführt (z.B. Bereinigen von Protokolldateien im Speicher in bestimmten Zeitintervallen). Ein Beispiel für eine Watchdog-Dienstimplementierung finden Sie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="performance-monitoring"></a>Leistungsüberwachung
Die Überwachung der zugrunde liegenden Infrastruktur ist wichtig, um einen Eindruck vom Zustand des Clusters und der Ressourcenverwendung zu erhalten. Die Messung der Systemleistung hängt von mehreren Faktoren ab, die normalerweise jeweils durch einen eigenen KPI (Key Performance Indicator) bestimmt werden. KPIs, die für Service Fabric relevant sind, können Metriken in Form von Leistungsindikatoren zugeordnet werden. Die Metriken können dabei aus den Knoten im Cluster erfasst werden.
KPIs können helfen,
* die Ressourcenverwendung und -auslastung zu ermitteln, beispielsweise für die Clusterskalierung oder die Optimierung der Dienstprozesse.
* Infrastrukturprobleme vorherzusagen. Vielen Problemen geht eine plötzliche Veränderung im Leistungsverhalten (Leistungsabfall) voraus. Daher können mithilfe von KPIs, beispielsweise zur Netzwerk-E/A und CPU-Auslastung, Infrastrukturprobleme vorhergesagt und diagnostiziert werden.

Eine Liste der Leistungsindikatoren, die auf Infrastrukturebene erfasst werden sollten, finden Sie unter [Leistungsmetriken](service-fabric-diagnostics-event-generation-perf.md). 

Für die Leistungsüberwachung auf Anwendungsebene bietet Service Fabric eine Reihe von Leistungsindikatoren für die Programmiermodelle „Reliable Services“ und „Reliable Actors“. Bei Verwendung eines dieser Modelle können die Leistungsindikatoren KPIs bereitstellen, die sicherstellen, dass die Akteure ordnungsgemäß hoch- und heruntergefahren oder dass Reliable Services-Anforderungen schnell genug verarbeitet werden. Weitere Informationen dazu finden Sie unter [Diagnose und Leistungsüberwachung für Reliable ServiceRemoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) und [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). Darüber hinaus bietet Application Insights ebenfalls eine Reihe von Leistungsmetriken, die erfasst werden, sofern sie mit Ihrer Anwendung konfiguriert wurden.

Verwenden Sie den OMS-Agent, um die geeigneten Leistungsindikatoren zu erfassen, und OMS Log Analytics, um diese KPIs anzuzeigen. Sie können auch den Azure-Diagnoseerweiterungs-Agent (oder einen anderen vergleichbaren Agent) nutzen, um die Metriken für die Analyse zu erfassen und zu speichern. 

## <a name="health-monitoring"></a>Systemüberwachung
Die Service Fabric-Plattform umfasst ein Integritätsmodell, das erweiterbare Integritätsberichtsfunktionen für den Status von Entitäten in einem Cluster bereitstellt. Alle Knoten, Anwendungen, Dienste, Partitionen, Replikate oder Instanzen verfügen über einen kontinuierlich aktualisierbaren Integritätsstatus. Der Integritätsstatus kann entweder „OK“, „Warnung“ oder „Fehler“ lauten. Der Integritätsstatus wird durch Integritätsberichte geändert, die für jede Entität basierend auf Problemen im Cluster ausgegeben werden. Der Integritätsstatus der Entitäten kann jederzeit im Service Fabric Explorer (SFX) überprüft werden, wie unten dargestellt. Alternativ kann er über die Integritäts-API der Plattform abgefragt werden. Sie haben auch die Möglichkeit, Integritätsberichte anzupassen und den Integritätsstatus einer Entität zu ändern, indem Sie eigene Integritätsberichte hinzufügen oder die Integritäts-API nutzen. Weitere Informationen zum Integritätsmodell finden Sie unter [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md).

![Integritätsdashboard von SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Sie können die aktuellen Integritätsberichte in SFX einsehen, zusätzlich steht jeder Bericht aber auch als Ereignis zur Verfügung. Integritätsereignisse können über den Betriebskanal erfasst (siehe [Ereignisaggregation und -sammlung mit der Windows Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) und in OMS Log Analytics gespeichert werden. So stehen sie zukünftig für Warnungen und Abfragen zur Verfügung. Auf diese Weise lassen sich Probleme, die u. U. die Anwendungsverfügbarkeit beeinträchtigen, leichter erkennen. Daher wird empfohlen, Warnungen für entsprechende Fehlerszenarien einzurichten (benutzerdefinierte Warnungen über OMS).

## <a name="monitoring-workflow"></a>Überwachungsworkflow 

Der allgemeine Workflow für die Überwachung und Diagnose besteht aus drei Schritten:

1. **Ereignisgenerierung**: Umfasst Ereignisse (Protokolle, Ablaufverfolgungen, benutzerdefinierte Ereignisse) auf Infrastruktur-, Plattform- (Cluster) und Anwendungsebene.
2. **Ereignisaggregation**: Diese Phase entspricht tatsächlich der Pipeline, über die Ihre Ereignisse an ein Tool (wie die Azure-Diagnoseerweiterung oder EventFlow) übermittelt werden, in dem sie analysiert werden können.
3. **Analyse**: Auf die Ereignisse muss in einem Tool zugegriffen werden können, damit sie u. a. analysiert, visualisiert, abgefragt und für Warnungen genutzt werden können.

Mehrere Produkte sind erhältlich, die alle drei Bereiche abdecken. Zudem können Sie für jeden Bereich unterschiedliche Technologien auswählen. Dabei muss sichergestellt werden, dass diese jeweils zusammenpassen, um eine umfassende Überwachungslösung für den Cluster bereitzustellen.

## <a name="event-generation"></a>Ereignisgenerierung

Der erste Schritt im Workflow für die Überwachung und Diagnose besteht darin, die Erstellung und Generierung von Ereignis- und Protokolldaten einzurichten. Diese Ereignisse, Protokolle und Leistungsindikatoren werden auf allen drei Ebenen ausgegeben: Anwendungsebene (Instrumentierungen, die den im Cluster bereitgestellten Apps und Diensten hinzugefügt wurden), Plattformebene (Ereignisse, die basierend auf dem Service Fabric-Betrieb vom Cluster ausgegeben wurden) und Infrastrukturebene (Leistungsmetriken von den einzelnen Knoten). Die auf den einzelnen Ebenen erfassten Diagnosedaten können angepasst werden, Service Fabric aktiviert allerdings eine bestimmte Standardinstrumentierung. 

Weitere Informationen zu den enthaltenen Instrumentierungen und zum Hinzufügen weiterer Instrumentierungen finden Sie unter [Ereignis- und Protokollgenerierung auf Infrastrukturebene](service-fabric-diagnostics-event-generation-infra.md) und [Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene](service-fabric-diagnostics-event-generation-app.md). Die Hauptentscheidung, die Sie hier treffen müssen, besteht darin, wie die Anwendung instrumentiert werden soll. Für .NET-Anwendungen wird ILogger empfohlen, Sie können aber auch EventSource, Serilog und andere vergleichbare Bibliotheken in Betracht ziehen. Für Java empfiehlt sich Log4j. Darüber hinaus gibt es verschiedene weitere Optionen, die abhängig von der Art der Anwendung verwendet werden können. Alternativ können Sie sich informieren, welche Option für Ihren spezifischen Anwendungsfall optimal geeignet ist, oder eine Option wählen, mit der Sie am besten vertraut sind. 

Nachdem Sie festgelegt haben, welchen Protokollanbieter Sie verwenden möchten, müssen Sie sicherstellen, dass die Protokolle ordnungsgemäß aggregiert und gespeichert werden.

## <a name="event-aggregation"></a>Ereignisaggregation

Zum Erfassen der Protokolle und Ereignisse, die von Ihren Anwendungen und Ihrem Cluster generiert werden, empfehlen wir im Allgemeinen die [Azure-Diagnoseerweiterung](service-fabric-diagnostics-event-aggregation-wad.md) (eher vergleichbar mit der Agent-basierten Protokollerfassung) oder [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) (In-Process-Protokollerfassung). Wenn Sie Application Insights verwenden und in .NET oder Java entwickeln, wird die Verwendung des Application Insights SDKs anstelle von EventFlow empfohlen.

Es ist zwar möglich, eine ähnliche Aufgabe mit nur einer dieser Lösungen auszuführen, in den meisten Fällen erhalten Sie durch die Kombination des Azure-Diagnoseerweiterungs-Agents mit einer In-Process-Erfassungspipeline (AI SDK oder EventFlow) jedoch einen zuverlässigeren, umfassenderen Überwachungsworkflow. Der Azure-Diagnoseerweiterungs-Agent stellt Ihre Lösung für Ereignisse auf Plattformebene dar, während Sie das AI SDK oder EventFlow (In-Process-Erfassung) für Protokolle auf Anwendungsebene verwenden. 

Falls Sie nur eine der beiden Lösungen verwenden möchten, sollten Sie folgende wichtige Hinweise beachten.
* Das Erfassen von Anwendungsprotokollen mit der Azure-Diagnoseerweiterung ist eine gute Option für Service Fabric-Dienste, wenn die Protokollquellen und -ziele sich nicht häufig ändern und eine direkte Zuordnung zwischen den Quellen und ihren Zielen besteht. Dies liegt daran, dass die Konfiguration der Azure-Diagnose auf der Ebene des Ressourcen-Managers erfolgt, sodass umfassende Änderungen an der Konfiguration die Aktualisierung des gesamten Clusters erforderlich machen. Daher ist diese Vorgehensweise häufig weniger effizient als die Verwendung des AI SDKs oder von EventFlow.
* Bei Verwendung von EventFlow ist es möglich, dass Dienste ihre Protokolle direkt an eine Analyse- und Visualisierungsplattform und/oder einen Speicher senden. Für den gleichen Zweck können auch andere Bibliotheken (ILogger, Serilog usw.) verwendet werden, EventFlow hat jedoch den Vorteil, dass die Anwendung speziell für die In-Process-Protokollerfassung und die Unterstützung von Service Fabric-Diensten konzipiert wurde. Daraus ergeben sich mehrere Vorteile, beispielsweise eine einfachere Konfiguration und Bereitstellung und eine höhere Flexibilität bei der Unterstützung verschiedener Protokollierungsbibliotheken und Analysetools. 

## <a name="event-analysis"></a>Ereignisanalyse

Für die Analyse und Visualisierung von Überwachungs- und Diagnosedaten sind mehrere hervorragende Plattformen auf dem Markt verfügbar. Wir empfehlen [OMS](service-fabric-diagnostics-event-analysis-oms.md) und [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) aufgrund ihrer Integration in Service Fabric. Möglicherweise kommt für Sie auch [Elastic Stack](https://www.elastic.co/products) (insbesondere, wenn Sie einen Cluster in einer Offlineumgebung ausführen möchten), [Splunk](https://www.splunk.com/) oder eine andere Plattform infrage. 

Wichtige Punkte für die Auswahl einer Plattform sollten u.a. sein, wie Sie mit der Benutzeroberfläche und den Abfrageoptionen zurechtkommen, welche Optionen zum Visualisieren von Daten und Erstellen übersichtlicher Dashboards enthalten sind und welche zusätzlichen Tools zur Optimierung der Überwachung zur Verfügung stehen (z.B. automatische Warnungen).

Zusätzlich zu der ausgewählten Plattform erhalten Sie beim Einrichten eines Service Fabric-Clusters als Azure-Ressource auch Zugriff auf die standardmäßigen Leistungsüberwachungsfunktionen von Azure für Ihre Computer.

### <a name="azure-monitor"></a>Azure Monitor

Sie können [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) zum Überwachen von vielen Azure-Ressourcen verwenden, auf denen ein Service Fabric-Cluster erstellt wird. Ein Satz von Metriken für die [VM-Skalierungsgruppe](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) (Virtual Machine Scale Set, VMSS) und einzelne [virtuelle Computer](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) wird automatisch gesammelt und im Azure-Portal angezeigt. Wählen Sie zum Anzeigen der gesammelten Informationen im Azure-Portal die Ressourcengruppe aus, die den Service Fabric-Cluster enthält. Wählen Sie anschließend die VM-Skalierungsgruppe aus, die Sie anzeigen möchten. Wählen Sie im Abschnitt **Überwachung** (im linken Navigationsbereich) **Metriken** aus, um ein Diagramm mit den Werten anzuzeigen.

![Ansicht des Azure-Portals mit gesammelten Metrikinformationen](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Führen Sie die Schritte der Anleitung unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) aus, um die Diagramme anzupassen. Sie können basierend auf diesen Metriken auch Warnungen erstellen, wie unter [Erstellen von Warnungen in Azure Monitor für Azure-Dienste](../monitoring-and-diagnostics/monitoring-overview-alerts.md) beschrieben. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Überwachen der Plattform und der von Service Fabric bereitgestellten Ereignisse finden Sie unter [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md).
* Falls Sie mit dem Instrumentieren Ihrer Anwendungen beginnen möchten, informieren Sie sich unter [Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene](service-fabric-diagnostics-event-generation-app.md).
* Führen Sie die Schritte zum Einrichten von AI für Ihre Anwendung unter [Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) aus.
* Lesen Sie, wie Sie OMS Log Analytics zum Überwachen von Containern einrichten: [Überwachen von Windows-Containern unter Service Fabric mit OMS](service-fabric-tutorial-monitoring-wincontainers.md)


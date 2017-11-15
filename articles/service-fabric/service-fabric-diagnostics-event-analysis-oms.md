---
title: Azure Service Fabric-Ereignisanalyse mit OMS | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ereignisse unter Verwendung der OMS zur Überwachung und Diagnose von Azure Service Fabric-Clustern visualisieren und analysieren."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 09542c0e7f628ca4fea00a6562c0b9525432c213
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Ereignisanalyse und Visualisierung mit OMS

Die Operations Management Suite (OMS) ist eine Sammlung von Verwaltungsdiensten, die die Überwachung und Diagnose für in der Cloud gehostete Anwendungen und Dienste unterstützen. Eine ausführlichere Übersicht über die OMS und ihre Optionen finden Sie unter [Was ist die OMS?](../operations-management-suite/operations-management-suite-overview.md).

## <a name="log-analytics-and-the-oms-workspace"></a>Log Analytics und der OMS-Arbeitsbereich

Log Analytics sammelt Daten von verwalteten Ressourcen, z.B. von einer Azure Storage-Tabelle oder einem Agent, und verwaltet sie in einem zentralen Repository. Die Daten können dann für Analyse, Warnungen und Visualisierung oder für den weiteren Export verwendet werden. Log Analytics unterstützt Ereignisse, Leistungsdaten oder andere benutzerdefinierten Daten.

Wenn die OMS konfiguriert ist, haben Sie Zugriff auf einen bestimmten *OMS-Arbeitsbereich*, über den Daten abgefragt oder in Dashboards visualisiert werden können.

Nachdem Daten von Log Analytics empfangen wurden, verfügt die OMS über mehrere *Verwaltungslösungen*. Dabei handelt es sich um vorkonfigurierte Lösungen zum Überwachen eingehender Daten, die an verschiedene Szenarien angepasst sind. Dazu gehören eine *Service Fabric-Analyselösung* und eine *Containerlösung*. Dies sind die beiden wichtigsten Lösungen für die Diagnose und Überwachung bei Verwendung von Service Fabric-Clustern. Es gibt aber auch verschiedene andere Lösungen, die in Betracht gezogen werden können. Außerdem ermöglicht die OMS die Erstellung benutzerdefinierter Lösungen. Entsprechende Informationen finden Sie [hier](../operations-management-suite/operations-management-suite-solutions.md). Jede Lösung, für deren Verwendung Sie sich für einen Cluster entscheiden, kann mit Log Analytics im gleichen OMS-Arbeitsbereich konfiguriert werden. Arbeitsbereiche ermöglichen benutzerdefinierte Dashboards und die Visualisierung von Daten sowie Änderungen an den Daten, die Sie sammeln, verarbeiten und analysieren möchten.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Einrichten eines OMS-Arbeitsbereichs mit der Service Fabric-Analyse-Lösung
Es wird empfohlen, dass Sie die Service Fabric-Lösung in Ihrem OMS-Arbeitsbereich einfügen. Sie umfasst ein Dashboard, über das die verschiedenen eingehenden Protokollkanäle von der Plattform- und Anwendungsebene angezeigt werden und die Service Fabric-spezifischen Protokolle abgefragt werden können. Es folgt ein Beispiel für eine relativ einfache Service Fabric-Lösung, mit der eine einzelne Anwendung im Cluster bereitgestellt wird:

![OMS – SF-Lösung](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Um diese Funktionalität für Ihren Cluster zu verwenden, lesen Sie die Informationen unter [Set up OMS Log Analytics for a cluster](service-fabric-diagnostics-oms-setup.md) (Einrichten von OMS Log Analytics für einen Cluster).

## <a name="using-the-oms-agent"></a>Verwenden des OMS-Agents

Es wird empfohlen, EventFlow und WAD als Aggregationslösungen zu verwenden, da diese einen modularen Ansatz für die Diagnose und Überwachung bieten. Wenn Sie beispielsweise die Ausgaben von EventFlow ändern möchten, ist keine Änderung an der eigentlichen Instrumentierung, sondern lediglich eine einfache Änderung an der Konfigurationsdatei erforderlich. Wenn Sie sich jedoch für die Verwendung von OMS Log Analytics entscheiden, sollten Sie den [OMS-Agent](../log-analytics/log-analytics-windows-agents.md) einrichten. Den OMS-Agent sollten Sie auch verwenden, wenn Sie Container für Ihren Cluster bereitstellen, wie weiter unten erläutert. 

Die erforderlichen Schritte hierzu finden Sie unter [Add the OMS Agent to a cluster](service-fabric-diagnostics-oms-agent.md) (Hinzufügen des OMS-Agents zu einem Cluster).

Dies bietet die folgenden Vorteile:

* Umfangreichere Daten für die Leistungsindikatoren und Metriken
* Leicht konfigurierbare Metriken, die aus dem Cluster gesammelt werden, ohne dass Sie Ihre Clusterkonfiguration aktualisieren müssen. Änderungen an den Einstellungen des Agents können aus dem OMS-Portal erfolgen, und der Agent startet in Übereinstimmung mit der erforderlichen Konfiguration automatisch neu. Zum Konfigurieren des OMS-Agents für das Abrufen spezifischer Leistungsindikatoren wechseln Sie zu **Startseite des Arbeitsbereichs > Einstellungen > Daten > Windows-Leistungsindikatoren**, und wählen Sie die zu sammelnden Daten aus.
* Daten werden schneller angezeigt, da sie nicht gespeichert werden müssen, um dann von OMS und Log Analytics abgerufen zu werden.
* Die Überwachung von Containern ist sehr viel einfacher, da Docker-Protokolle (stdout, stderr) und Docker-Statistiken (Leistungsmetriken auf Container- und Knotenebene) abgerufen werden können.

Die Hauptüberlegung hierbei ist, dass möglicherweise die Leistung Ihrer Anwendungen im Cluster beeinflusst werden kann, da der Agent gemeinsam mit Ihren Anwendungen auf Ihrem Cluster bereitgestellt wird.

## <a name="monitoring-containers"></a>Überwachen von Containern

Bei der Bereitstellung von Containern in einem Service Fabric-Cluster wird empfohlen, dass der Cluster mit dem OMS-Agent eingerichtet und Ihrem OMS-Arbeitsbereich die Container-Lösung hinzugefügt wird, um die Überwachung und Diagnose zu ermöglichen. Hier ein Beispiel für die Container-Lösung in einem Arbeitsbereich:

![Einfaches OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Der Agent ermöglicht das Sammeln verschiedener containerspezifischer Protokolle, die in der OMS abgefragt oder zur Visualisierung von Leistungsindikatoren verwendet werden können. Folgende Protokolltypen werden gesammelt:

* ContainerInventory: zeigt Informationen über Containerspeicherort, -name und -images an
* ContainerImageInventory: zeigt Informationen zu bereitgestellten Images an, einschließlich IDs und Größen
* ContainerLog: zeigt bestimmte Fehlerprotokolle, Docker-Protokolle (stdout usw.) und andere Einträge an
* ContainerServiceLog: zeigt die Docker-Daemon-Befehle an, die ausgeführt wurden
* Perf: zeigt Leistungsindikatoren einschließlich Container-CPU, Arbeitsspeicher, Netzwerkdatenverkehr, Datenträger-E/A und benutzerdefinierter Metriken von den Hostcomputern an

Unter [Monitor containers with OMS Log Analytics](service-fabric-diagnostics-oms-containers.md) (Überwachen von Containern mit OMS Log Analytics) werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen zur OMS-Containerlösung finden Sie in der [Dokumentation](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Nächste Schritte

Zum Anpassen eines Arbeitsbereichs entsprechend Ihren Anforderungen können Sie die folgenden Tools und Optionen der OMS verwenden:

* Für lokale Cluster bietet die OMS ein Gateway (HTTP-Weiterleitungsproxy), über das Daten an die OMS gesendet werden können. Weitere Informationen dazu finden Sie unter [Verbinden von Computern ohne Internetzugriff mit der OMS über das OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).
* Konfigurieren Sie die OMS für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
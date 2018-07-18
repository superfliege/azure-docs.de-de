---
title: Überwachen von Containern in Azure Service Fabric mit Log Analytics | Microsoft-Dokumentation
description: Verwenden Sie Log Analytics zum Überwachen von Containern, die in Azure Service Fabric-Clustern ausgeführt werden.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 79d30a47b017379107b63b0006a35534f68c43b9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210775"
---
# <a name="monitor-containers-with-log-analytics"></a>Überwachen von Containern mit Log Analytics
 
Dieser Artikel behandelt die erforderlichen Schritte zum Einrichten der OMS Log Analytics-Containerüberwachungslösung, um Containerereignisse anzuzeigen. Informationen zum Einrichten des Clusters zum Erfassen von Containerereignissen finden Sie in diesem [schrittweisen Tutorial](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>Einrichten der Containerüberwachungslösung

> [!NOTE]
> Log Analytics muss für den Cluster eingerichtet und der OMS-Agent muss auf Ihren Knoten bereitgestellt worden sein. Ist dies nicht der Fall, führen Sie zuerst die Schritte unter [Einrichten von Log Analytics](service-fabric-diagnostics-oms-setup.md) und [Hinzufügen des OMS-Agents zu einem Cluster](service-fabric-diagnostics-oms-agent.md) aus.

1. Wenn Log Analytics und der OMS-Agent für den Cluster eingerichtet wurden, stellen Sie Ihre Container bereit. Warten Sie, bis die Container bereitgestellt wurden, bevor Sie mit dem nächsten Schritt fortfahren.

2. Suchen Sie in Azure Marketplace nach *Container Monitoring Solution* (Containerüberwachungslösung), und klicken Sie auf die Ressource **Container Monitoring Solution**, die unter der Kategorie „Überwachung + Verwaltung“ angezeigt wird.

    ![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Erstellen Sie die Lösung im gleichen Arbeitsbereich, der bereits für den Cluster erstellt wurde. Durch diese Änderung wird der Agent automatisch veranlasst, mit dem Sammeln von Docker-Daten in den Containern zu beginnen. Nach etwa 15 Minuten sollten in der Lösung eingehende Protokolle und Statistiken angezeigt werden. Die Abbildung unten zeigt dies.

    ![Einfaches OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Der Agent ermöglicht das Sammeln verschiedener containerspezifischer Protokolle, die in der OMS abgefragt oder zur Visualisierung von Leistungsindikatoren verwendet werden können. Folgende Protokolltypen werden gesammelt:

* ContainerInventory: zeigt Informationen über Containerspeicherort, -name und -images an
* ContainerImageInventory: zeigt Informationen zu bereitgestellten Images an, einschließlich IDs und Größen
* ContainerLog: zeigt bestimmte Fehlerprotokolle, Docker-Protokolle (stdout usw.) und andere Einträge an
* ContainerServiceLog: zeigt die Docker-Daemon-Befehle an, die ausgeführt wurden
* Perf: zeigt Leistungsindikatoren einschließlich Container-CPU, Arbeitsspeicher, Netzwerkdatenverkehr, Datenträger-E/A und benutzerdefinierter Metriken von den Hostcomputern an



## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur [Containerlösung von OMS](../log-analytics/log-analytics-containers.md).
* Weitere Informationen zur Containerorchestrierung in Service Fabric finden Sie unter [Service Fabric und Container](service-fabric-containers-overview.md).
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* Konfigurieren Sie Log Analytics für die Einrichtung von [automatisierten Warnungsregeln](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
---
title: "Überwachen von Containern in Azure Service Fabric mit OMS Log Analytics | Microsoft-Dokumentation"
description: "Verwenden Sie OMS Log Analytics zum Überwachen von Containern, die in Azure Service Fabric-Clustern ausgeführt werden."
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
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 17121041520160d0d76832bbdbe74ad6a649fdd8
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="monitor-containers-with-oms-log-analytics"></a>Überwachen von Containern mit OMS Log Analytics
 
In diesem Artikel werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen dazu finden Sie unter [Überwachen von Containern](service-fabric-diagnostics-event-analysis-oms.md#monitoring-containers). Ein Schritt-für-Schritt-Tutorial zu diesem Thema finden Sie außerdem unter [Überwachen von Windows-Containern unter Service Fabric mit OMS](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>Einrichten der Containerüberwachungslösung

> [!NOTE]
> OMS Log Analytics muss für den Cluster eingerichtet und der OMS-Agent muss auf Ihren Knoten bereitgestellt worden sein. Ist dies nicht der Fall, führen Sie zuerst die Schritte unter [Set up OMS Log Analytics for a cluster](service-fabric-diagnostics-oms-setup.md) (Einrichten von OMS Log Analytics für einen Cluster) und [Add the OMS Agent to a cluster](service-fabric-diagnostics-oms-agent.md) (Hinzufügen des OMS-Agents zu einem Cluster) aus.

1. Wenn für den Cluster OMS Log Analytics und der OMS-Agent eingerichtet wurden, stellen Sie Ihre Container bereit. Warten Sie, bis die Container bereitgestellt wurden, bevor Sie mit dem nächsten Schritt fortfahren.

2. Suchen Sie in Azure Marketplace nach *Container Monitoring Solution* (Containerüberwachungslösung), und klicken Sie auf die Ressource **Container Monitoring Solution**, die unter der Kategorie „Überwachung + Verwaltung“ angezeigt wird.

    ![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Erstellen Sie die Lösung im gleichen Arbeitsbereich, der bereits für den Cluster erstellt wurde. Durch diese Änderung wird der Agent automatisch veranlasst, mit dem Sammeln von Docker-Daten in den Containern zu beginnen. Nach etwa 15 Minuten sollten in der Lösung eingehende Protokolle und Statistiken angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur Containerorchestrierung in Service Fabric finden Sie unter [Service Fabric und Container](service-fabric-containers-overview.md).
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* Konfigurieren Sie OMS für die Einrichtung von Regeln für [automatisierte Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
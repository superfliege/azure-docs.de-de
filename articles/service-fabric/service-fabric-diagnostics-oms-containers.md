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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 7a775b6d23c144c81650bb3608ee6a117475a9ba
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-containers-with-log-analytics"></a>Überwachen von Containern mit Log Analytics
 
In diesem Artikel werden die erforderlichen Schritte zum Einrichten der Containerüberwachung für Ihren Cluster beschrieben. Weitere Informationen dazu finden Sie unter [Überwachen von Containern](service-fabric-diagnostics-event-analysis-oms.md#monitoring-containers). Ein Schritt-für-Schritt-Tutorial zu diesem Thema finden Sie außerdem unter [Überwachen von Windows-Containern unter Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>Einrichten der Containerüberwachungslösung

> [!NOTE]
> Log Analytics muss für den Cluster eingerichtet und der OMS-Agent muss auf Ihren Knoten bereitgestellt worden sein. Ist dies nicht der Fall, führen Sie zuerst die Schritte unter [Einrichten von Log Analytics](service-fabric-diagnostics-oms-setup.md) und [Hinzufügen des OMS-Agents zu einem Cluster](service-fabric-diagnostics-oms-agent.md) aus.

1. Wenn Log Analytics und der OMS-Agent für den Cluster eingerichtet wurden, stellen Sie Ihre Container bereit. Warten Sie, bis die Container bereitgestellt wurden, bevor Sie mit dem nächsten Schritt fortfahren.

2. Suchen Sie in Azure Marketplace nach *Container Monitoring Solution* (Containerüberwachungslösung), und klicken Sie auf die Ressource **Container Monitoring Solution**, die unter der Kategorie „Überwachung + Verwaltung“ angezeigt wird.

    ![Hinzufügen der Containerlösung](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Erstellen Sie die Lösung im gleichen Arbeitsbereich, der bereits für den Cluster erstellt wurde. Durch diese Änderung wird der Agent automatisch veranlasst, mit dem Sammeln von Docker-Daten in den Containern zu beginnen. Nach etwa 15 Minuten sollten in der Lösung eingehende Protokolle und Statistiken angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur Containerorchestrierung in Service Fabric finden Sie unter [Service Fabric und Container](service-fabric-containers-overview.md).
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* Konfigurieren Sie Log Analytics für die Einrichtung von [automatisierten Warnungsregeln](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
---
title: 'Azure Service Fabric: Leistungsüberwachung mit Log Analytics | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie den OMS-Agent zur Überwachung von Containern und Leistungsindikatoren für Ihre Azure Service Fabric-Cluster einrichten.
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
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: a3ce72e51477c1eda99461b3910bfeeac207be55
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="performance-monitoring-with-log-analytics"></a>Leistungsüberwachung mit Log Analytics

Dieser Artikel beschreibt, wie Sie Ihrem Cluster den Log Analytics-Agent – auch OMS genannt – als VM-Skalierungsgruppenerweiterung hinzufügen und mit Ihrem vorhandenen Azure Log Analytics-Arbeitsbereich verbinden. Dies ermöglicht die Erfassung von Diagnosedaten zu Containern, Anwendungen und zur Leistungsüberwachung. Durch Hinzufügen als Erweiterung der VM-Skalierungsgruppenressource stellt Azure Resource Manager sicher, dass der Agent auf jedem Knoten installiert wird – auch beim Skalieren des Clusters.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Azure Log Analytics-Arbeitsbereich eingerichtet haben. Fahren Sie andernfalls mit [Einrichten von Operations Management Suite Log Analytics für einen Cluster](service-fabric-diagnostics-oms-setup.md) fort.

## <a name="add-the-agent-extension-via-azure-cli"></a>Hinzufügen der Agent-Erweiterung über die Azure-Befehlszeilenschnittstelle

Den OMS-Agent fügen Sie Ihrem Cluster am besten über die VM-Skalierungsgruppen-APIs hinzu, die für die Azure-Befehlszeilenschnittstelle zur Verfügung stehen. Falls Sie die Azure-Befehlszeilenschnittstelle noch nicht eingerichtet haben, wechseln Sie zum Azure-Portal, und öffnen Sie eine [Cloud Shell](../cloud-shell/overview.md)-Instanz, oder [installieren Sie die Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Vergewissern Sie sich nach dem Anfordern Ihrer Cloud Shell, dass Sie in dem Abonnement arbeiten, in dem sich auch Ihre Ressource befindet. Verwenden Sie für diese Überprüfung `az account show`, und vergewissern Sie sich, dass der Wert für „name“ dem Namen des Abonnements Ihres Clusters entspricht.

2. Navigieren Sie im Portal zu der Ressourcengruppe, in der sich Ihr Log Analytics-Arbeitsbereich befindet. Klicken Sie in die Log Analytics-Ressource (der Typ der Ressource ist Log Analytics). Klicken Sie auf der Ressourcenübersichtsseite im linken Menü im Abschnitt „Einstellungen“ auf **Erweiterte Einstellungen**.

    ![Eigenschaftenseite von Log Analytics](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Klicken Sie auf **Windows-Server**, wenn Sie einen Windows-Cluster einrichten möchten, bzw. auf **Linux-Server**, wenn Sie einen Linux-Cluster erstellen möchten. Auf dieser Seite werden Ihre `workspace ID` und `workspace key` angezeigt (als Primärschlüssel im Portal aufgeführt). Sie benötigen beide im nächsten Schritt.

4. Führen Sie den Befehl aus, mit dem der OMS-Agent in Ihrem Cluster installiert wird. Verwenden Sie dazu die `vmss extension set`-API in Ihrer Cloud Shell:

    Windows-Cluster:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Linux-Cluster:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Hier sehen Sie ein Beispiel für das Hinzufügen des OMS-Agents zu einem Windows-Cluster:

    ![OMS-Agent-CLI-Befehl](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Das Hinzufügen des Agents zu Ihren Knoten sollte in weniger als 15 Minuten erfolgreich abgeschlossen sein. Mit der `az vmss extension list`-API können Sie überprüfen, ob die Agents hinzugefügt wurden:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Hinzufügen des Agents über die Resource Manager-Vorlage

Resource Manager-Beispielvorlagen zum Bereitstellen eines Azure Log Analytics-Arbeitsbereichs sowie zum Hinzufügen eines Agents zu den einzelnen Knoten stehen für [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) und [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) zur Verfügung.

Sie können diese Vorlage herunterladen und anpassen, um einen Cluster bereitzustellen, der Ihre Anforderungen am besten erfüllt.

## <a name="view-performance-counters-in-the-log-analytics-portal"></a>Anzeigen von Leistungsindikatoren im Log Analytics-Portal

Jetzt haben Sie den OMS-Agent hinzugefügt und können im Log Analytics-Portal die Leistungsindikatoren auswählen, die Sie erfassen möchten. 

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe, in der Sie die Service Fabric Analytics-Lösung erstellt haben. Wählen Sie **ServiceFabric\<OMS-Arbeitsbereichsname\>** aus, und wechseln Sie zur zugehörigen Übersichtsseite. Klicken Sie im oberen Bereich auf den Link für den Wechsel zum OMS-Portal.

2. Im Portal sehen Sie Kacheln in Form eines Graphs für jede der aktivierten Lösungen, auch eine für Service Fabric. Klicken Sie darauf, um mit Service Fabric Analytics fortzufahren. 

3. Jetzt sehen Sie einige Kacheln mit Graphen zu Betriebskanal- und Reliable Services-Ereignisse. Klicken Sie auf der rechten Seite auf das Zahnradsymbol, um zur Einstellungenseite zu wechseln.

    ![OMS-Einstellungen](media/service-fabric-diagnostics-oms-agent/oms-solutions-settings.png)

4. Klicken Sie auf der Einstellungenseite auf „Daten“, und wählen Sie Windows- oder Linux-Leistungsindikatoren aus. Es gibt eine Liste von Standardindikatoren, deren Aktivierung Sie auswählen und für die Sie auch das Erfassungsintervall festlegen können. Sie können auch [weitere Leistungsindikatoren](service-fabric-diagnostics-event-generation-perf.md) hinzufügen, die erfasst werden sollen. Auf das richtige Format wird in diesem [Artikel](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx) verwiesen.

Sobald die Indikatoren konfiguriert sind, wechseln Sie wieder zur Lösungenseite, wo bald in den Graphen unter **Knotenmetriken** Dateneingänge angezeigt werden. Sie können Leistungsindikatordaten ähnlich wie Clusterereignisse abfragen und mithilfe der Abfragesprache Kusto nach Knoten, Name des Leistungsindikators und Werten filtern. 

![OMS-Leistungsindikatorabfrage](media/service-fabric-diagnostics-oms-agent/oms-perf-counter-query.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfassen Sie relevante [Leistungsindikatoren](service-fabric-diagnostics-event-generation-perf.md). Lesen Sie zum Konfigurieren des OMS-Agents für das Erfassen von bestimmten Leistungsindikatoren [Konfigurieren von Datenquellen](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Konfigurieren Sie Log Analytics für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
* Alternativ können Sie Leistungsindikatoren über die [Azure Diagnostics-Erweiterung erfassen und an Application Insights senden](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).
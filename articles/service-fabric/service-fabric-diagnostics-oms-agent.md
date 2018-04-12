---
title: 'Azure Service Fabric: Einrichten der Überwachung mit dem OMS-Agent | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie den OMS-Agent zur Überwachung von Containern und Leistungsindikatoren für Ihre Azure Service Fabric-Cluster einrichten.
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
ms.date: 04/03/2018
ms.author: dekapur
ms.openlocfilehash: 613e5a2a746d480f020af652e7bbaf5e80ed059d
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="add-the-oms-agent-to-a-cluster"></a>Hinzufügen des OMS-Agents zu einem Cluster

Dieser Artikel beschreibt, wie Sie Ihrem Cluster den OMS-Agent als VM-Skalierungsgruppenerweiterung hinzufügen und mit Ihrem vorhandenen Azure Log Analytics-Arbeitsbereich verbinden. Dies ermöglicht die Erfassung von Diagnosedaten zu Containern, Anwendungen und zur Leistungsüberwachung. Durch Hinzufügen als Erweiterung stellt Azure Resource Manager sicher, dass der Agent auf jedem Knoten installiert wird – auch beim Skalieren des Clusters.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Azure Log Analytics-Arbeitsbereich eingerichtet haben. Fahren Sie andernfalls mit [Einrichten von Operations Management Suite Log Analytics für einen Cluster](service-fabric-diagnostics-oms-setup.md) fort.

## <a name="add-the-agent-extension-via-azure-cli"></a>Hinzufügen der Agent-Erweiterung über die Azure-Befehlszeilenschnittstelle

Den OMS-Agent fügen Sie Ihrem Cluster am besten über die VM-Skalierungsgruppen-APIs hinzu, die für die Azure-Befehlszeilenschnittstelle zur Verfügung stehen. Falls Sie die Azure-Befehlszeilenschnittstelle noch nicht eingerichtet haben, wechseln Sie zum Azure-Portal, und öffnen Sie eine [Cloud Shell](../cloud-shell/overview.md)-Instanz, oder [installieren Sie die Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Vergewissern Sie sich nach dem Anfordern Ihrer Cloud Shell, dass Sie in dem Abonnement arbeiten, in dem sich auch Ihre Ressource befindet. Verwenden Sie für diese Überprüfung `az account show`, und vergewissern Sie sich, dass der Wert für „name“ dem Namen des Abonnements Ihres Clusters entspricht.

2. Navigieren Sie im Portal zu der Ressourcengruppe, in der sich Ihr Log Analytics-Arbeitsbereich befindet. Klicken Sie auf die Log Analytics-Ressource (der Ressourcentyp ist „Log Analytics“). Scrollen Sie anschließend im Navigationsbereich auf der rechten Seite nach unten, und klicken Sie auf **Eigenschaften**.

    ![Eigenschaftenseite von Log Analytics](media/service-fabric-diagnostics-oms-agent/oms-properties.png)

    Notieren Sie sich den Wert von `workspaceId`. 

3. Zum Bereitstellen des Agents wird außerdem `workspaceKey` benötigt. Klicken Sie zum Ermitteln des Schlüssels im Abschnitt *Einstellungen* des linken Navigationsbereichs auf **Erweiterte Einstellungen**. Klicken Sie auf **Windows-Server**, wenn Sie einen Windows-Cluster einrichten möchten, bzw. auf **Linux-Server**, wenn Sie einen Linux-Cluster erstellen möchten. Sie benötigen den angezeigten *Primärschlüssel* (als `workspaceKey`) zur Bereitstellung Ihrer Agents.

4. Führen Sie den Befehl aus, mit dem der OMS-Agent in Ihrem Cluster installiert wird. Verwenden Sie dazu die `vmss extension set`-API in Ihrer Cloud Shell:

    Windows-Cluster:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<LogAnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<LogAnalyticsworkspaceKey>'}"
    ```

    Linux-Cluster:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<LogAnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<LogAnalyticsworkspaceKey>'}"
    ```

    Hier sehen Sie ein Beispiel für das Hinzufügen des OMS-Agents zu einem Windows-Cluster:

    ![OMS-Agent-CLI-Befehl](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Führen Sie den Befehl aus, um diese Konfiguration für Ihre bereits vorhandenen VM-Instanzen zu übernehmen:  

    ```sh
    az vmss update-instances
    ```

    Das Hinzufügen des Agents zu Ihren Knoten sollte in weniger als 15 Minuten erfolgreich abgeschlossen sein. Mit der `az vmss extension list`-API können Sie überprüfen, ob die Agents hinzugefügt wurden:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Hinzufügen des Agents über die Resource Manager-Vorlage

Resource Manager-Beispielvorlagen zum Bereitstellen eines Azure Log Analytics-Arbeitsbereichs sowie zum Hinzufügen eines Agents zu den einzelnen Knoten stehen für [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) und [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) zur Verfügung.

Sie können diese Vorlage herunterladen und anpassen, um einen Cluster bereitzustellen, der Ihre Anforderungen am besten erfüllt.

## <a name="next-steps"></a>Nächste Schritte

* Erfassen Sie relevante [Leistungsindikatoren](service-fabric-diagnostics-event-generation-perf.md). Lesen Sie zum Konfigurieren des OMS-Agents für das Erfassen von bestimmten Leistungsindikatoren [Konfigurieren von Datenquellen](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Konfigurieren Sie Log Analytics für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.

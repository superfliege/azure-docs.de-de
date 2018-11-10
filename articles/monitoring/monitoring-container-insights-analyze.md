---
title: Überwachen der Leistung von AKS-Clustern mit Azure Monitor für Container (Vorschau) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit Azure Monitor für Container die Leistung analysieren und Daten protokollieren können.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2018
ms.author: magoedte
ms.openlocfilehash: daec3d6e6cd8e4df3fdfe45fbb8ee98966c8a38e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214153"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers-preview"></a>Verstehen der Leistung von AKS-Clustern mit Azure Monitor für Container (Vorschau)
Das Anzeigen der Leistung von AKS-Clustern (Azure Kubernetes Service) kann mit Azure Monitor für Container in zwei Perspektiven erfolgen, direkt auf einem AKS-Cluster oder für alle AKS-Cluster in einem Abonnement in Azure Monitor. 

Dieser Artikel soll Ihr Verständnis für die Benutzererfahrung in den beiden Perspektiven schärfen und aufzeigen, wie Sie erkannte Probleme schnell bewerten, untersuchen und beheben.

Weitere Informationen zum Aktivieren von Azure Monitor für Container finden Sie unter [Onboarding von Azure Monitor für Container](monitoring-container-insights-onboard.md).

Azure Monitor bietet eine Multi-Cluster-Ansicht, die den Integritätsstatus aller überwachten AKS-Cluster, die in Ressourcengruppen in Ihrem Abonnement bereitgestellt sind, sowie die von der Lösung nicht überwachten AKS-Cluster darstellt.  Sie können die Clusterintegrität auf einen Blick einschätzen und von diesem Punkt aus einen Drilldown zur Seite des Knotens und der Controllerleistung ausführen oder alternativ zu den Leistungsdiagrammen für den Cluster navigieren.  Für AKS-Cluster, die ermittelt und als nicht überwacht erkannt wurden, können Sie die Überwachung jederzeit aktivieren.  

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Multi-Cluster-Ansicht in Azure Monitor 
Zum Anzeigen des Integritätsstatus aller bereitgestellten AKS-Cluster wählen Sie im linken Seitenbereich im Azure-Portal **Überwachen** aus.  Wählen Sie im Abschnitt **Insights** **Container (Vorschau)** aus.  

![Beispiel für ein Multi-Cluster-Dashboard in Azure Monitor](./media/monitoring-container-insights-analyze/azmon-containers-multiview.png)

Auf der Registerkarte **Überwachte Cluster** finden Sie die folgenden Informationen:

1. Wie viele Cluster sich in einem kritischen oder fehlerhaften Zustand im Vergleich zur Anzahl der Cluster in fehlerfreiem Zustand oder zu denjenigen befinden, die keine Daten übermitteln (was als unbekannter Zustand bezeichnet wird)
2. Wie viele Knoten, Benutzer und Systempods pro Cluster bereitgestellt sind.  

Dies sind die definierten Integritätszustände: 

1. **Fehlerfrei** – es wurden keine Probleme für die VM erkannt, und sie funktioniert wie erforderlich. 
2. **Kritisch** – mindestens ein kritischer Fehler wurde entdeckt, der behandelt werden muss, um den normalen Betriebszustand wiederherzustellen.
3. **Warnung** – es wurde mindestens ein Problem entdeckt, das behandelt werden muss, weil sonst der Übergang zu einem kritischen Integritätszustand erfolgen kann.
4. **Unbekannt** – wenn der Dienst keine Verbindung mit einem Knoten oder einem Pod herstellen konnte, wechselt der Status zu „Unbekannt“.

Der Integritätsstatus berechnet den Gesamtstatus eines Clusters als den *schlechtesten* der drei Zustände, mit einer Ausnahme – wenn einer der drei Zustände *unbekannt* ist, ist der Gesamtstatus des Clusters **Unbekannt**.  

Die folgende Tabelle stellt eine Aufschlüsselung der Berechnung dar, die die Integritätszustände für einen überwachten Cluster in der Multi-Cluster-Ansicht steuert.

| |Status |Verfügbarkeit |  
|-------|-------|-----------------|  
|**Benutzerpod**| | |  
| |Healthy |100 % |  
| |Warnung |90–99 % |  
| |Kritisch |<90 % |  
| |Unknown |Wenn keine Meldung in den letzten 30 Minuten erfolgt ist |  
|**Systempod**| | |  
| |Healthy |100 % |
| |Warnung |N/V |
| |Kritisch |<100 % |
| |Unknown |Wenn keine Meldung in den letzten 30 Minuten erfolgt ist |
|**Node** | | |
| |Healthy |>85 % |
| |Warnung |60–84 % |
| |Kritisch |<60 % |
| |Unknown |Wenn keine Meldung in den letzten 30 Minuten erfolgt ist |

Aus der Liste der Cluster können Sie einen Drilldown zur Seite **Cluster** ausführen, indem Sie auf den Namen eines Clusters klicken, zur Leistungsseite **Knoten**, indem Sie in der Spalte **Knoten** des betreffenden Clusters auf den Rollup der Knoten klicken oder einen Drilldown zur Leistungsseite **Controller** ausführen, indem Sie auf den Rollup in der Spalte **Benutzerpods** oder **Systempods** klicken.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Direktes Anzeigen der Leistung auf einem AKS-Cluster
Der Zugriff auf Azure Monitor für Container ist direkt auf einem AKS-Cluster möglich, indem Sie im linken Randbereich **Insights (Vorschau)** auswählen. Die Darstellung der Informationen zu Ihrem AKS-Cluster ist in vier Perspektiven unterteilt:

- Cluster
- Nodes 
- Controllers  
- Container

Die Standardseite, die beim Klicken auf **Insights (Vorschau)** geöffnet wird, ist **Cluster**, und sie umfasst vier Liniendiagramme, die wichtige Leistungsmetriken Ihres Clusters darstellen. 

![Beispielleistungsdiagramme auf der Registerkarte „Cluster“](./media/monitoring-container-insights-analyze/containers-cluster-perfview.png)

Das Leistungsdiagramm zeigt vier Leistungsmetriken an:

- **Knoten-CPU-Auslastung&nbsp;%**: eine aggregierte Ansicht der CPU-Auslastung für den gesamten Cluster. Sie können die Ergebnisse für den Zeitbereich filtern, indem Sie **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor oberhalb des Diagramms auswählen, entweder einzeln oder kombiniert. 
- **Knoten-Arbeitsspeicherauslastung&nbsp;%**: Eine aggregierte Ansicht der Arbeitsspeicherauslastung für den gesamten Cluster. Sie können die Ergebnisse für den Zeitbereich filtern, indem Sie **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor oberhalb des Diagramms auswählen, entweder einzeln oder kombiniert. 
- **Knotenanzahl**: Die Anzahl von Knoten und der Status von Kubernetes. Die Status der dargestellten Clusterknoten sind *Alle*, *Bereit* und *Nicht bereit*. Der Status kann im Selektor oberhalb des Diagramms einzeln oder kombiniert gefiltert werden. 
- **Aktivitätspodanzahl**: Die Anzahl von Pods und der Status von Kubernetes. Die Status der dargestellten Pods sind *Alle*, *Ausstehend*, *Wird ausgeführt* und *Unbekannt*. Der Status kann im Selektor oberhalb des Diagramms einzeln oder kombiniert gefiltert werden. 

Wenn Sie zur Registerkarte **Knoten**, **Controller** und **Container** wechseln, wird der Eigenschaftenbereich automatisch rechts auf der Seite angezeigt.  Dort werden die Eigenschaften des ausgewählten Elements einschließlich der Bezeichnungen angezeigt, die Sie definieren, um die Kubernetes-Objekte zu organisieren. Klicken Sie auf den **>>**-Link im Bereich, um ihn anzuzeigen\auszublenden.  

![Beispiel für den Eigenschaftenbereich von Kubernetes-Perspektiven](./media/monitoring-container-insights-analyze/perspectives-preview-pane-01.png)

Wenn Sie die Objekte in der Hierarchie erweitern, wird der Eigenschaftenbereich auf Basis des ausgewählten Objekts aktualisiert. Im Bereich können Sie auch Kubernetes-Ereignisse mit vordefinierten Protokollsuchen anzeigen, indem Sie am oberen Rand des Bereichs auf **Kubernetes-Ereignisprotokolle anzeigen** klicken. Weitere Informationen zum Anzeigen von Kubernetes-Protokolldaten finden Sie unter [Suchen von Protokollen zur Datenanalyse](#search-logs-to-analyze-data).

Verwenden Sie oben auf der Seite die Option **+ Filter hinzufügen**, um die Ergebnisse für die Ansicht nach **Dienst**, **Knoten** oder **Namespace** zu filtern. Nach der Auswahl des Filterbereichs wählen Sie dann unter den im Feld **Wert(e) auswählen** angezeigten Werten aus.  Nachdem der Filter konfiguriert wurde, wird er global auf alle Perspektiven des AKS-Clusters angewendet.  Die Formel unterstützt nur das Gleichheitszeichen.  Sie können zusätzlich zum obersten weitere Filter hinzufügen, um Ihre Ergebnisse weiter einzugrenzen.  Wenn Sie beispielsweise einen Filter nach **Knoten** festgelegt haben, könnten Sie mit einem zweiten Filter **Dienst** oder **Namespace** auswählen.  

![Beispiel zur Verwendung des Filters zum Eingrenzen der Ergebnisse](./media/monitoring-container-insights-analyze/add-filter-option-01.png)

Ein auf einer Registerkarte angewendeter Filter bleibt wirksam, wenn Sie zu einer anderen Registerkarte wechseln, und wird durch Klicken auf das **x**-Symbol neben dem angegebenen Filter gelöscht.   

Auf der Registerkarte **Knoten** folgt die Zeilenhierarchie dem Kubernetes-Objektmodell, das mit einem Knoten in Ihrem Cluster beginnt. Erweitern Sie den Knoten, und Sie können mindestens einen Pod ansehen, der auf dem Knoten ausgeführt wird. Wenn mehrere Container zu einem Pod zusammengefasst sind, werden sie als letzte Zeile in der Hierarchie angezeigt. Sie können auch anzeigen, wie viele nicht auf Pods bezogene Workloads auf dem Host ausgeführt werden, falls Prozessor oder Arbeitsspeicher des Hosts überlastet sind.

![Beispiel für eine Kubernetes-Knotenhierarchie in der Leistungsansicht](./media/monitoring-container-insights-analyze/containers-nodes-view.png)

Auf einem erweiterten Knoten können Sie per Drilldown von dem Pod oder Container, der auf dem Knoten ausgeführt wird, zum Controller navigieren, um für diesen Controller gefilterte Leistungsdaten anzuzeigen. Klicken Sie auf den Wert in der Spalte **Controller** für den spezifischen Knoten.   

![Exemplarischer Drilldownvorgang vom Knoten zum Controller in der Leistungsansicht](./media/monitoring-container-insights-analyze/drill-down-node-controller.png)

Im oberen Bereich der Seite können Sie Controller oder Container auswählen und den Status sowie die Ressourcenauslastung für diese Objekte überprüfen.  Wenn Sie stattdessen die Arbeitsspeicherauslastung überprüfen möchten, wählen Sie aus der Dropdownliste **Metrik** die Option **Arbeitsspeicher RSS** oder **Arbeitssatz für Arbeitsspeicher** aus. **Arbeitsspeicher RSS** wird nur für die Kubernetes-Version 1.8 und höher unterstützt. Andernfalls werden Werte für **Min&nbsp;%** als *NaN&nbsp;%* angezeigt. Dieser numerische Datentypwert stellt einen nicht definierten oder nicht darstellbaren Wert dar. 

![Leistungsansicht zu den Containerknoten](./media/monitoring-container-insights-analyze/containers-node-metric-dropdown.png)

Standardmäßig beziehen sich Leistungsdaten auf die letzten sechs Stunden, jedoch können Sie den Zeitraum mithilfe der Option **TimeRange** oben links ändern. Außerdem haben Sie die Möglichkeit, die Ergebnisse im Zeitbereich durch Auswahl von **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor zu filtern. 

![Perzentilauswahl für die Datenfilterung](./media/monitoring-container-insights-analyze/containers-metric-percentile-filter.png)

Beachten Sie im folgenden Beispiel, dass für den Knoten *aks-nodepool1-* der Wert von **Container** 14 ist, was einen Rollup der Gesamtzahl der bereitgestellten Container darstellt.

![Beispiel für Rollup der Container pro Knoten](./media/monitoring-container-insights-analyze/containers-nodes-containerstotal.png)

So können Sie schnell feststellen, ob Sie das richtige Verhältnis zwischen Containern und Knoten im Cluster haben. 

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Knoten erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Hosts. |
| Status | Kubernetes-Ansicht des Knotenstatus. |
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Durchschnittlicher Prozentsatz von Knoten basierend auf dem Perzentil für die ausgewählte Dauer. |
| Mittelw., Min., Max., 50., 90. | Tatsächlicher Durchschnittswert der Knoten basierend auf dem Perzentil für den ausgewählten Zeitraum. Der Mittelwert wird anhand des festgelegten CPU-/Arbeitsspeichergrenzwerts für einen Knoten gemessen. Bei Pods und Containern ist dies der vom Host gemeldete Mittelwert. |
| Container | Anzahl von Containern |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Knotens verstrichen ist. |
| Controllers | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. Nicht alle Pods befinden sind in einem Controller, sodass einige Spalten **N/V** anzeigen. | 
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Balkendiagrammtrend, der die Perzentilmetrik des Controllers in Prozent anzeigt. |

Wählen Sie im Selektor **Controller** aus.

![Auswählen der Controlleransicht](./media/monitoring-container-insights-analyze/containers-controllers-tab.png)

In dieser Ansicht können Sie die Leistungsintegrität Ihrer Controller ansehen.

![Leistungsansicht des Controllers <Name>](./media/monitoring-container-insights-analyze/containers-controllers-view.png)

Die Zeilenhierarchie beginnt mit einem Controller. Wenn Sie einen Controller erweitern, wird mindestens ein Pod angezeigt.  Wenn Sie einen Pod erweitern, wird in der letzten Zeile der Container angezeigt, der mit dem Pod verknüpft ist. Von einem erweiterten Controller aus können Sie per Drilldown zu dem Knoten navigieren, auf dem er ausgeführt wird, um für diesen Controller gefilterte Leistungsdaten anzuzeigen. Klicken Sie auf den Wert in der Spalte **Knoten** für den spezifischen Controller.   

![Exemplarischer Drilldownvorgang vom Knoten zum Controller in der Leistungsansicht](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Controllern erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers.|
| Status | Rollupstatus der Container, wenn deren Ausführung mit einem Status abgeschlossen wurde, z. B. *OK*, *Beendet*, *Fehler*, *Gestoppt* oder *Angehalten*. Wenn der Container ausgeführt wird, der Status jedoch entweder nicht richtig angezeigt oder nicht vom Agent übernommen wurde und seit über 30 Minuten nicht antwortet, ist der Status *Unbekannt*. Zusätzliche Details zu dem Symbol „Status“ werden in der folgenden Tabelle angegeben.|
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Durchschnittliches Rollup des durchschnittlichen Prozentsatzes jeder Entität für die ausgewählte Metrik und das ausgewählte Perzentil. |
| Mittelw., Min., Max., 50., 90.  | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers für das ausgewählte Perzentil. Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Container | Gesamtanzahl der Container für den Controller oder Pod. |
| Neustarts | Rollup der Anzahl von Containerneustarts. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Knoten | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. | 
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;%| Balkendiagrammtrend, der die Perzentilmetrik des Controllers anzeigt. |

Die Symbole im Statusfeld zeigen den Onlinestatus von Containern an:
 
| Symbol | Status | 
|--------|-------------|
| ![Statussymbol für bereit und ausgeführt](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | Wird ausgeführt (bereit)|
| ![Statussymbol für wartend oder angehalten](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Wartend oder angehalten|
| ![Symbol für zuletzt gemeldeten Ausführungsstatus](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Zuletzt als ausgeführt gemeldet, hat aber seit mehr als 30 Minuten nicht geantwortet|
| ![Statussymbol für erfolgreiches Anhalten/Stoppen](./media/monitoring-container-insights-analyze/containers-green-icon.png) | Erfolgreich beendet oder Fehler beim Beenden|

Das Statussymbol zeigt basierend auf dem, was der Pod bereitstellt, eine Anzahl an. Es werden die beiden schlechtesten Status angezeigt. Wenn Sie den Mauszeiger über den Status bewegen, wird ein Rollupstatus aller Pods im Container angezeigt. Wenn der Status „Bereit“ nicht vorliegt, wird der Statuswert **(0)** angezeigt. 

Wählen Sie im Selektor **Container** aus.

![Auswählen der Containeransicht](./media/monitoring-container-insights-analyze/containers-containers-tab.png)

Hier können Sie die Leistungsintegrität Ihrer Azure Kubernetes-Container anzeigen.  

![Leistungsansicht des Controllers <Name>](./media/monitoring-container-insights-analyze/containers-containers-view.png)

Von einem Container aus können Sie per Drilldown zu einem Pod oder Knoten navigieren, um für das entsprechende Objekt gefilterte Leistungsdaten anzuzeigen. Klicken Sie auf den Wert in der Spalte **Pod** oder **Knoten** für den spezifischen Container.   

![Exemplarischer Drilldownvorgang vom Knoten zum Controller in der Leistungsansicht](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Containern erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers.|
| Status | Status der Container, sofern vorhanden. Zusätzliche Informationen zum Statussymbol finden Sie in der folgenden Tabelle.|
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Rollup des durchschnittlichen Prozentsatzes jeder Entität für die ausgewählte Metrik und das ausgewählte Perzentil. |
| Mittelw., Min., Max., 50., 90.  | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers für das ausgewählte Perzentil. Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Pod | Container, in dem sich der Pod befindet.| 
| Knoten |  Der Knoten, in dem sich der Container befindet. | 
| Neustarts | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Containers verstrichen ist. |
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Balkendiagrammtrend, der die durchschnittliche Metrik des Containers in Prozent anzeigt. |

Die Symbole im Statusfeld zeigen die Onlinestatus der Pods an, wie in der folgenden Tabelle beschrieben:
 
| Symbol | Status |  
|--------|-------------|  
| ![Statussymbol für bereit und ausgeführt](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | Wird ausgeführt (bereit)|  
| ![Statussymbol für wartend oder angehalten](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Wartend oder angehalten|  
| ![Symbol für zuletzt gemeldeten Ausführungsstatus](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Zuletzt als ausgeführt gemeldet, hat aber seit mehr als 30 Minuten nicht geantwortet|  
| ![Statussymbol für „Beendet“](./media/monitoring-container-insights-analyze/containers-terminated-icon.png) | Erfolgreich beendet oder Fehler beim Beenden|  
| ![Symbol für den Status „Fehler“](./media/monitoring-container-insights-analyze/containers-failed-icon.png) | Status „Fehler“ |  

## <a name="container-data-collection-details"></a>Details zur Datensammlung in Containern
Container Insights sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern. Alle drei Minuten werden Daten gesammelt.

### <a name="container-records"></a>Containerdatensätze

Die folgende Tabelle zeigt Beispiele für Datensätze, die von Azure Monitor für Container gesammelt werden, und Datentypen, die in Protokollsuchergebnissen angezeigt werden:

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | `ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |
| Containerknotenbestand | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Containerprozess | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Bestand der Pods in einem Kubernetes-Cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Bestand der Knoten als Teil eines Kubernetes-Clusters | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-Ereignisse | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message,  SourceSystem | 
| Dienste im Kubernetes-Cluster | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Knoten | Perf &#124; where ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuUsageNanoCorescpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Container | Perf &#124; where ObjectName == “K8SContainer” | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Suchen von Protokollen zur Datenanalyse
Mit Log Analytics können Sie nach Trends suchen, Engpässe diagnostizieren, Prognosen erstellen oder Daten korrelieren, die Ihnen die Einschätzung ermöglichen, ob die aktuelle Clusterkonfiguration optimal funktioniert. Ihnen werden vordefinierte Protokollsuchen für die sofortige Verwendung bereitgestellt. Alternativ können Sie diese so anpassen, dass die Informationen auf die gewünschte Weise zurückgegeben werden. 

Im Arbeitsbereich können Sie interaktive Datenanalysen durchführen, indem Sie im Vorschaubereich die Option **Kubernetes-Ereignisprotokolle anzeigen** oder **Containerprotokolle anzeigen** auswählen. Die Seite **Protokollsuche** wird rechts von der Azure-Portalseite angezeigt, die Sie besucht haben.

![Analysieren von Daten in Log Analytics](./media/monitoring-container-insights-analyze/container-health-log-search-example.png)   

Die Containerprotokolle, die an Log Analytics weitergeleitet wurden, geben STDOUT und STDERR aus. Da Azure Monitor AKS (Azure Kubernetes Service) überwacht, wird „kube-system“ aufgrund des großen Umfangs der generierten Daten zurzeit nicht gesammelt. 

### <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Abfrageerstellung ausgehend von einem oder zwei Beispielen zu beginnen und diese dann den Anforderungen entsprechend anzupassen. Sie können mit den folgenden Beispielabfragen experimentieren, um komplexere Abfragen zu erstellen:

| Abfragen | BESCHREIBUNG | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Liste mit Lebenszyklusinformationen aller Container| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Kubernetes-Ereignisse|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Imagebestand | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf<br> &#124; where ObjectName == "Container" and CounterName == "% Processor Time"<br> &#124; summarize AvgCPUPercent = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Container-CPU | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf &#124; where ObjectName == "Container" and CounterName == "Memory Usage MB"<br> &#124; summarize AvgUsedMemory = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Containerspeicher |

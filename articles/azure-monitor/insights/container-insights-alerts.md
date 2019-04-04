---
title: Erstellen von Leistungswarnungen mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie benutzerdefinierte Azure-Warnungen basierend auf Protokollabfragen für die Arbeitsspeicher- und CPU-Auslastung von Azure Monitor für Container erstellen können.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: magoedte
ms.openlocfilehash: e6fdb0d57a44578647c1f16dc76c557296f20ddb
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886767"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Einrichten von Warnungen für Leistungsprobleme in Azure Monitor für Container
Azure Monitor für Container überwacht die Leistung von Containerworkloads, die entweder in Azure Container Instances oder in Managed Kubernetes-Clustern bereitgestellt sind, die im Azure Kubernetes Service (AKS) gehostet werden. 

In diesem Artikel wird beschrieben, wie Sie Warnungen für die folgenden Situationen aktivieren können:

* Bei CPU- und Arbeitsspeicherauslastung auf Knoten des Clusters oder bei Überschreitung des definierten Schwellenwerts.
* Wenn die CPU- oder Arbeitsspeicherauslastung auf einem der Container innerhalb eines Controllers Ihren definierten Schwellenwert im Vergleich zu dem für die entsprechende Ressource festgelegten Grenzwert überschreitet.

Zum Ausgeben einer Warnung bei hoher CPU- oder Speicherauslastung für einen Cluster oder Controller erstellen Sie eine Warnungsregel vom Typ „Metrische Maßeinheit“, die auf bereitgestellten Protokollabfragen basiert. Die Abfragen vergleichen mithilfe des now-Operators einen datetime-Wert mit dem vorhandenen Wert und gehen um eine Stunde zurück. Alle Daten, die von Azure Monitor für Container gespeichert werden, sind im UTC-Format.

Wenn Sie mit Warnungen in Azure Monitor nicht vertraut sind, sollten Sie zunächst den [Überblick über Warnungen in Microsoft Azure](../platform/alerts-overview.md) lesen. Weitere Informationen zu Warnungen mithilfe von Protokollabfragen finden Sie unter [Protokollwarnungen in Azure Monitor](../platform/alerts-unified-log.md)

## <a name="resource-utilization-log-search-queries"></a>Suchabfragen für die Ressourcenverwendung
Die Abfragen in diesem Abschnitt werden zur Unterstützung der einzelnen Warnungsszenarien bereitgestellt. Die Abfragen sind für Schritt 7 unter dem nachfolgenden Abschnitt [Erstellen einer Warnung](#create-alert-rule) erforderlich.  

Die folgende Abfrage berechnet die durchschnittliche CPU-Auslastung als Mittelwert der CPU-Auslastung von Memberknoten pro Minute.  

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuCapacityNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```

Die folgende Abfrage berechnet die durchschnittliche Speicherauslastung als Mittelwert der Speicherauslastung von Memberknoten pro Minute.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryCapacityBytes';
let usageCounterName = 'memoryRssBytes';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```
>[!IMPORTANT]
>Die folgenden Abfragen enthalten Zeichenkettenwerte als Platzhalter für Ihre Cluster- und Controllernamen: <Ihr-Cluster-Name> und <Ihr-Controller-Name>. Ersetzen Sie Platzhalter durch umgebungsspezifische Werte, bevor Sie Warnungen einrichten. 


Die folgende Abfrage berechnet die durchschnittliche CPU-Auslastung aller Container in einem Controller als Durchschnitt der CPU-Auslastung der einzelnen Containerinstanzen in einem Controller für jede Minute als Prozentsatz des für einen Container eingerichteten Grenzwerts.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuLimitNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

Die folgende Abfrage berechnet die durchschnittliche Arbeitsspeicherauslastung aller Container in einem Controller als Durchschnitt der Arbeitsspeicherauslastung der einzelnen Containerinstanzen in einem Controller für jede Minute als Prozentsatz des für einen Container eingerichteten Grenzwerts.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryLimitBytes';
let usageCounterName = 'memoryRssBytes';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

## <a name="create-alert-rule"></a>Erstellen einer Warnungsregel
Führen Sie die folgenden Schritte aus, um eine Protokollwarnung in Azure Monitor mit einer der zuvor angegebenen Regeln für die Protokollsuche zu erstellen.  

>[!NOTE]
>Das folgende Verfahren erfordert, dass Sie wie unter [Wechseln der API-Einstellung für Protokollwarnungen](../platform/alerts-log-api-switch.md) beschrieben zu einer neuen Protokollwarnungs-API wechseln, wenn Sie eine Warnungsregel für die Nutzung von Containerressourcen erstellen. 
>

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Azure-Portal im linken Bereich die Option **Überwachen** aus. Wählen Sie im Abschnitt **Insights** die Option **Container** aus.    
3. Wählen Sie auf der Registerkarte **Überwachte Cluster** einen Cluster aus der Liste aus, indem Sie auf den Namen des Clusters klicken.
4. Wählen Sie im linken Bereich im Abschnitt **Überwachung** die Option **Protokolle** aus, um die Seite mit Azure Monitor-Protokollen zu öffnen, die zum Schreiben und Ausführen von Azure Log Analytics-Abfragen verwendet wird.
5. Klicken Sie auf der Seite **Protokolle** auf **+ Neue Warnungsregel**.
6. Klicken Sie im Abschnitt **Bedingung** auf die vordefinierte Protokollbedingung **Immer wenn die benutzerdefinierte Protokollsuche <logic undefined> ist**. Der Signaltyp **benutzerdefinierte Protokollsuche** ist automatisch ausgewählt, da die Erstellung einer Warnungsregel direkt von der Seite mit Azure Monitor-Protokollen initiiert wurde.  
7. Fügen Sie eine der zuvor bereitgestellten [Abfragen](#resource-utilization-log-search-queries) in das Feld **Suchabfrage** ein. 

8. Konfigurieren Sie die Warnung mit den folgenden Informationen:

    a. Wählen Sie in der Dropdownliste **Basierend auf** die Option **Metrische Maßeinheit** aus. Mit „Metrische Maßeinheit“ wird eine Warnung für jedes Objekt in der Abfrage mit einem Wert erzeugt, der den angegebenen Schwellenwert überschreitet.  
    b. Wählen Sie für **Bedingung** die Option **Größer als** aus, und geben Sie **75** als anfänglichen **Schwellenwert** oder einen Wert entsprechend Ihren Kriterien ein.  
    c. Wählen Sie im Abschnitt **Warnung auslösen basierend auf** die Option **Aufeinanderfolgende Sicherheitsverletzungen** und in der Dropdownliste **Größer als** aus, und geben Sie den Wert **2** ein.  
    d. Wenn Sie eine Warnung für die CPU- oder Arbeitsspeicherauslastung des Containers konfigurieren, wählen Sie unter **Aggregieren auf** die Option **ContainerName** aus der Dropdownliste aus.  
    e. Ändern Sie im Abschnitt **Auswertung basierend auf** den Wert **Zeitraum** in 60 Minuten. Die Regel wird alle fünf Minuten ausgeführt und gibt Datensätze zurück, die innerhalb der letzten Stunde erstellt wurden. Durch die Verwendung eines weiter gefassten Zeitfensters wird potenziellen Datenlatenzen Rechnung getragen und sichergestellt, dass die Abfrage Daten zurückgibt, um ein falsch negatives Ergebnis zu vermeiden, bei dem die Warnung nie ausgelöst wird. 

9. Klicken Sie auf **Fertig**, um die Warnungsregel fertig zu stellen.
10. Geben Sie einen Namen für die Warnung in das Feld **Name der Warnungsregel** ein. Geben Sie eine **Beschreibung** mit Details zu Ihrer Warnung ein, und wählen Sie einen geeigneten Schweregrad aus den bereitgestellten Optionen aus.
11. Um die Warnungsregel direkt bei der Erstellung zu aktivieren, übernehmen Sie den Standardwert für **Regel beim Erstellen aktivieren**.
12. Im letzten Schritt wählen Sie eine vorhandene **Aktionsgruppe** aus, oder erstellen Sie eine neue Gruppe. Dadurch wird sichergestellt, dass bei jeder Warnungsauslösung die gleichen Aktionen ausgeführt werden und diese für jede definierte Regel verwendet werden können. Führen Sie die Konfiguration entsprechend der Handhabung von Vorfällen durch IT- oder DevOps-Vorgänge aus. 
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnungsregel fertig zu stellen. Die Ausführung beginnt sofort.

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich einige der [Beispiele für Protokollabfragen](container-insights-analyze.md#search-logs-to-analyze-data) an, um mehr über die vordefinierten Abfragen oder Beispiele zum Bewerten und Verwenden oder Anpassen für andere Warnungsszenarien zu erfahren. 
* Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).

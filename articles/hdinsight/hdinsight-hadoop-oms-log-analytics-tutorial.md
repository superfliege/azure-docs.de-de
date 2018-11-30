---
title: Verwenden von Log Analytics zum Überwachen von HDInsight-Clustern
description: Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Aufträgen verwenden, die in einem HDInsight-Cluster ausgeführt werden.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 23c8098927a051da48c7da2bac0c96173da1d2f2
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713298"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern

Es wird beschrieben, wie Sie Azure Log Analytics für die Überwachung von Hadoop-Clustervorgängen in HDInsight aktivieren und eine HDInsight-Überwachungslösung hinzufügen.

[Log Analytics](../log-analytics/log-analytics-overview.md) ist ein Dienst in Azure Monitor, der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Log Analytics-Arbeitsbereich**. Sie können sich diesen Arbeitsbereich als einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Eine Anleitung hierzu finden Sie unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

* **Ein Azure HDInsight-Cluster**. Derzeit können Sie Log Analytics mit den folgenden HDInsight-Cluster-Typen verwenden:

  * Hadoop
  * hbase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Anweisungen zum Erstellen von HDInsight-Clustern finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

> [!NOTE]
> Es wird empfohlen, sowohl den HDInsight-Cluster als auch den Log Analytics-Arbeitsbereich in derselben Region anzuordnen, um die Leistung zu erhöhen. Azure Log Analytics ist nicht in allen Azure-Regionen verfügbar.

## <a name="enable-log-analytics-by-using-the-portal"></a>Aktivieren von Log Analytics mithilfe des Portals

In diesem Abschnitt konfigurieren Sie einen vorhandenen HDInsight Hadoop-Cluster zur Verwendung eines Azure Log Analytics-Arbeitsbereichs zum Überwachen von Aufträgen, Debugprotokollen usw.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster.
2. Wählen Sie im linken Bereich die Option **Überwachung**.
3. Wählen Sie im rechten Bereich die Option **Aktivieren** und dann einen vorhandenen Log Analytics-Arbeitsbereich aus. Klicken Sie anschließend auf **Speichern**.

    ![Aktivieren der Überwachung für HDInsight-Cluster](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Aktivieren der Überwachung für HDInsight-Cluster")

    Das Speichern der Einstellung dauert einige Zeit.

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Aktivieren von Log Analytics mithilfe von Azure PowerShell

Sie können Log Analytics mithilfe von Azure PowerShell aktivieren. Das zu verwendende Cmdlet ist:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

Siehe [Enable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0)

Zum Deaktivieren wird folgendes Cmdlet verwendet:

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

Siehe [Disable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0)

## <a name="install-hdinsight-cluster-management-solutions"></a>Installieren von HDInsight-Clusterverwaltungslösungen

HDInsight bietet clusterspezifische Verwaltungslösungen, die Sie zu Azure Log Analytics hinzufügen können. [Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) erweitern den Funktionsumfang von Log Analytics und stellen zusätzliche Daten und Analysetools bereit. Diese Lösungen sammeln wichtige Leistungsmetriken aus Ihrem HDInsight-Cluster und stellen die Tools bereit, um die Metriken zu durchsuchen. Außerdem bieten diese Lösungen Visualisierungen und Dashboards für die meisten in HDInsight unterstützten Clustertypen. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen.

Dies sind die verfügbaren HDInsight-Lösungen:

* HDInsight Hadoop-Überwachung
* HDInsight HBase-Überwachung
* HDInsight Interactive Query-Überwachung
* HDInsight Kafka-Überwachung
* HDInsight Spark-Überwachung
* HDInsight Storm-Überwachung

Die Anleitung zum Installieren einer Verwaltungslösung finden Sie unter [Verwaltungslösungen in Azure](../azure-monitor/insights/solutions.md#install-a-management-solution). Installieren Sie eine HDInsight Hadoop-Überwachungslösung, um zu experimentieren. Wenn der Vorgang abgeschlossen ist, wird unter **Zusammenfassung** die Kachel **HDInsightHadoop** angezeigt. Wählen Sie die Kachel **HDInsightHadoop** aus. Die HDInsightHadoop-Lösung sieht wie folgt aus:

![HDInsight-Überwachungslösung – Ansicht](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Im Bericht werden keine Aktivitäten angezeigt, da es sich um einen brandneuen Cluster handelt.

## <a name="next-steps"></a>Nächste Schritte

* [Query Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-use-queries.md) (Abfragen von Azure Log Analytics zum Überwachen der HDInsight-Cluster [Vorschau])

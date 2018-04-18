---
title: Verwenden von Log Analytics zum Überwachen von Azure HDInsight-Clustern | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Aufträgen verwenden, die in einem HDInsight-Cluster ausgeführt werden.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: be71b065999c30546432895804df8633da21b43e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern

Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Hadoop-Clustervorgängen in HDInsight verwenden.

[Log Analytics](../log-analytics/log-analytics-overview.md) ist ein Dienst, der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen. 

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

* **Ein Azure HDInsight-Cluster**. Derzeit können Sie Log Analytics mit den folgenden HDInsight-Cluster-Typen verwenden:

    * Hadoop
    * hbase
    * Interactive Query
    * Kafka
    * Spark
    * Storm

    Anweisungen zum Erstellen von HDInsight-Clustern finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Ein Log Analytics-Arbeitsbereich**. Sie können sich diesen Arbeitsbereich als einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Sie müssen bereits einen solchen Arbeitsbereich erstellt haben, den Sie Azure HDInsight-Clustern zuordnen können. Anweisungen hierzu finden Sie unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="enable-log-analytics-by-using-the-portal"></a>Aktivieren von Log Analytics mithilfe des Portals

In diesem Abschnitt konfigurieren Sie einen vorhandenen HDInsight Hadoop-Cluster zur Verwendung eines Azure Log Analytics-Arbeitsbereichs zum Überwachen von Aufträgen, Debugprotokollen usw.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster.
2. Klicken Sie im linken Bereich auf **Überwachung**.
3. Klicken Sie im rechten Bereich auf **Aktivieren**, wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf **Speichern**.

    ![Aktivieren der Überwachung für HDInsight-Cluster](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Aktivieren der Überwachung für HDInsight-Cluster")

    Das Speichern der Einstellung dauert einige Zeit.  Sobald der Vorgang abgeschlossen ist, sehen Sie im oberen Bildschirmbereich die Schaltfläche **OMS-Dashboard öffnen**. 

    ![Operations Management Suite-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "OMS-Dashboard öffnen")

5. Klicken Sie auf **OMS-Dashboard öffnen**.
6. Geben Sie nach Aufforderung Ihre Azure-Anmeldeinformationen ein.

    ![Operations Management Suite-Portal](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite-Portal")

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Aktivieren von Log Analytics mithilfe von Azure PowerShell

Sie können Log Analytics mithilfe von Azure PowerShell aktivieren. Das zu verwendende Cmdlet ist:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
```

Siehe [Enable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0)

Zum Deaktivieren wird folgendes Cmdlet verwendet: 

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
```

Siehe [Disable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0)


## <a name="next-steps"></a>Nächste Schritte
* [Add HDInsight cluster management solutions to Log Analytics (Preview)](hdinsight-hadoop-oms-log-analytics-management-solutions.md) (Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics [Vorschau])

---
title: "Verwenden von Log Analytics zum Überwachen von Azure HDInsight-Clustern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Aufträgen verwenden, die in einem HDInsight-Cluster ausgeführt werden."
services: hdinsight
documentationcenter: 
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
ms.date: 11/08/2017
ms.author: nitinme
ms.openlocfilehash: 73c472140861a0d0d270021ab268e8c1113c23b5
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern

Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Hadoop-Clustervorgängen in HDInsight verwenden.

[Log Analytics](../log-analytics/log-analytics-overview.md) ist ein Dienst in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen. 

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

* **Ein Azure HDInsight-Cluster**. Derzeit können Sie Azure OMS mit den folgenden HDInsight-Cluster-Typen verwenden:

    * Hadoop
    * HBase
    * Interactive Query
    * Kafka
    * Spark
    * Storm

    Anweisungen zum Erstellen von HDInsight-Clustern finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Ein Log Analytics-Arbeitsbereich**. Sie können sich diesen Arbeitsbereich als einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Sie müssen bereits einen solchen Arbeitsbereich erstellt haben, den Sie Azure HDInsight-Clustern zuordnen können. Anweisungen hierzu finden Sie unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-log-analytics"></a>Konfigurieren eines HDInsight-Clusters zur Verwendung von Log Analytics

In diesem Abschnitt konfigurieren Sie einen vorhandenen HDInsight Hadoop-Cluster zur Verwendung eines Azure Log Analytics-Arbeitsbereichs zum Überwachen von Aufträgen, Debugprotokollen usw.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster.
2. Klicken Sie im linken Bereich auf **Überwachung**.
3. Klicken Sie im rechten Bereich auf **Aktivieren**, wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf **Speichern**.

    ![Aktivieren der Überwachung für HDInsight-Cluster](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Aktivieren der Überwachung für HDInsight-Cluster")

    Das Speichern der Einstellung dauert einige Zeit.  Sobald der Vorgang abgeschlossen ist, sehen Sie im oberen Bildschirmbereich die Schaltfläche **OMS-Dashboard öffnen**. 

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "OMS-Dashboard öffnen")

5. Klicken Sie auf **OMS-Dashboard öffnen**.
6. Geben Sie nach Aufforderung Ihre Azure-Anmeldeinformationen ein.

    ![Operations Management Suite-Portal](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite-Portal")

## <a name="next-steps"></a>Nächste Schritte
* [Add HDInsight cluster management solutions to Log Analytics (Preview)](hdinsight-hadoop-oms-log-analytics-management-solutions.md) (Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics [Vorschau])

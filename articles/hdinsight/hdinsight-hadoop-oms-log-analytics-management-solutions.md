---
title: Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Log Analytics benutzerdefinierte Ansichten für HDInsight-Cluster erstellen.
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
ms.openlocfilehash: ab959d2cdaf840a1a88a71a0b8f29d84a8979330
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Log Analytics

HDInsight bietet clusterspezifische Verwaltungslösungen, die Sie zu Azure Log Analytics hinzufügen können. [Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) erweitern den Funktionsumfang von Log Analytics und stellen zusätzliche Daten und Analysetools bereit. Diese Lösungen sammeln wichtige Leistungsmetriken aus Ihrem HDInsight-Cluster und stellen die Tools bereit, um die Metriken zu durchsuchen. Außerdem bieten diese Lösungen Visualisierungen und Dashboards für die meisten in HDInsight unterstützten Clustertypen. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. 

In diesem Artikel erfahren Sie, wie Sie einem Log Analytics-Arbeitsbereich clusterspezifische Verwaltungslösungen hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen einen HDInsight-Cluster zur Verwendung von Azure Log Analytics konfiguriert haben. Anweisungen hierzu finden Sie unter [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern [Vorschau]).

## <a name="add-cluster-specific-management-solutions"></a>Hinzufügen von clusterspezifischen Verwaltungslösungen

In diesem Abschnitt fügen Sie eine HBase-Clusterverwaltungslösung einem vorhandenen Log Analytics-Arbeitsbereich hinzu.

1. Öffnen Sie einen HDInsight-Cluster im Azure-Portal, klicken Sie auf **Überwachung** und dann auf **OMS-Dashboard öffnen**.

    ![Operations Management Suite-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

1. Klicken Sie auf dem Dashboard im linken Bereich auf **Lösungskatalog** oder auf das Symbol **Designer anzeigen**.

    ![Hinzufügen der Verwaltungslösung in Log Analytics](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Hinzufügen der Verwaltungslösung in Operations Management Suite")

2. Klicken Sie im Lösungskatalog auf eine der folgenden Kacheln:

    - HDInsight Hadoop-Überwachung
    - HDInsight HBase-Überwachung (Vorschau)
    - HDInsight Kafka-Überwachung
    - HDInsight Storm-Überwachung
    - HDInsight Spark-Überwachung

3. Klicken Sie im nächsten Bildschirm auf **Hinzufügen**.  Im folgenden Screenshot ist die Schaltfläche „Hinzufügen“ für die HBase-Überwachung dargestellt.

     ![Hinzufügen der HBase-Verwaltungslösung](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "Hinzufügen der HBase-Verwaltungslösung")

4. Sie sehen jetzt auf dem Dashboard eine Kachel für die HBase-Verwaltungslösung. Wenn der Cluster, den Sie (im Rahmen der Voraussetzungen für diesen Artikel) der Operations Management Suite zugeordnet haben, ein HBase-Cluster ist, zeigt die Kachel den Namen des Clusters und die Anzahl der Knoten im Cluster an.

    ![Hinzugefügte HBase-Verwaltungslösung](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "Hinzugefügte HBase-Verwaltungslösung")

## <a name="next-steps"></a>Nächste Schritte

* [Query Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-use-queries.md) (Abfragen von Azure Log Analytics zum Überwachen der HDInsight-Cluster [Vorschau])

## <a name="see-also"></a>Weitere Informationen

* [OMS Log Analytics: Create Tiles, Drill-ins and Dashboards with the View Designer](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/) (OMS Log Analytics: Erstellen von Kacheln, Drill-ins und Dashboards mit dem Ansicht-Designer)
* [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)

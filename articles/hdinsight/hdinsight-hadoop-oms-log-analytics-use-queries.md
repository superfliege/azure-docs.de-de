---
title: "Abfragen von Azure Log Analytics zum Überwachen von Azure HDInsight-Clustern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure Log Analytics Abfragen zum Überwachen von Aufträgen ausführen, die in einem HDInsight-Cluster ausgeführt werden."
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
ms.openlocfilehash: e4ee80826a710bde9483d130a4d1c986a72645ca
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Abfragen von Azure Log Analytics zum Überwachen von HDInsight-Clustern

Betrachten Sie einige grundlegende Szenarien zur Verwendung von Azure Log Analytics für die Überwachung von Azure HDInsight-Clustern:

* [Analysieren von HDInsight-Clustermetriken](#analyze-hdinsight-cluster-metrics)
* [Suchen nach bestimmten Protokollmeldungen](#search-for-specific-log-messages)
* [Erstellen von Ereigniswarnungen](#create-alerts-for-tracking-events)

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen einen HDInsight-Cluster zur Verwendung von Azure Log Analytics konfiguriert haben. Anweisungen hierzu finden Sie unter [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern [Vorschau]).

* Sie müssen dem OMS-Arbeitsbereich die für HDInsight-Cluster spezifischen Verwaltungslösungen hinzugefügt haben, wie in [Add HDInsight cluster management solutions to Log Analytics (Preview)](hdinsight-hadoop-oms-log-analytics-management-solutions.md) (Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics [Vorschau]) beschrieben.

## <a name="analyze-hdinsight-cluster-metrics"></a>Analysieren von HDInsight-Clustermetriken

Erfahren Sie, wie Sie nach bestimmten Metriken für Ihren HDInsight-Cluster suchen.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster, den Sie Azure Log Analytics zugeordnet haben.
2. Klicken Sie auf **Überwachung** und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im linken Menü auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Geben Sie im Suchfeld die folgende Abfrage ein, um nach allen verfügbaren Metriken für alle HDInsight-Cluster, die für die Verwendung von Azure Log Analytics konfiguriert sind, zu suchen, und drücken Sie dann die **EINGABETASTE**.

        `search *` 

    ![Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Suchen aller Metriken")

    Die Ausgabe ähnelt der folgenden:

    ![Ausgabe bei Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Ausgabe bei Suchen aller Metriken")

5. Wählen Sie im linken Bereich unter **Typ** eine Metrik aus, die Sie näher untersuchen möchten, und klicken Sie auf **Anwenden**. Im folgenden Screenshot ist der Typ `metrics_resourcemanager_queue_root_default_CL` ausgewählt. 

    > [!NOTE]
    > Möglicherweise müssen Sie auf die Schaltfläche **[+]Weitere** klicken, um die gesuchte Metrik zu finden. Außerdem befindet sich die Schaltfläche **Übernehmen** am unteren Rand der Liste, sodass Sie nach unten scrollen müssen, um sie anzuzeigen.
    > 
    >    

    Beachten Sie, dass die Abfrage im Textfeld jetzt in die Abfrage geändert wird, die im folgenden Screenshot im hervorgehoben Feld angezeigt wird:

    ![Suchen nach bestimmten Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Suchen nach bestimmten Metriken")

6. Sie können diese spezifische Metrik jetzt näher untersuchen. Beispielsweise können Sie über die folgende Abfrage die vorhandene Ausgabe basierend auf dem Mittelwert der in einem Intervall von 10 Minuten verwendeten Ressourcen verfeinern, wobei die Kategorisierung nach dem Clusternamen erfolgt:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. Anstelle der auf dem Mittelwert der verwendeten Ressourcen basierenden Optimierung können Sie die Ergebnisse mit der folgenden Abfrage basierend auf dem Zeitpunkt in einem 10-Minuten-Fenster optimieren, an dem die meisten Ressourcen verwendet wurden (sowie den Zeitpunkten beim 90. und 95. Perzentil):

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Suchen nach bestimmten Protokollmeldungen

Erfahren Sie, wie Sie in einem bestimmten Zeitfenster nach Fehlermeldungen suchen. Die folgenden Schritte sind nur ein Beispiel dafür, wie Sie die Fehlermeldung finden können, die Sie interessiert. Sie können jede verfügbare Eigenschaft verwenden, um nach den Fehlern zu suchen, die Sie finden möchten.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster, den Sie Azure Log Analytics zugeordnet haben.
2. Klicken Sie auf **Überwachung** und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im OMS-Dashboard auf der Startseite auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Geben Sie die folgende Abfrage ein, um nach allen Fehlermeldungen für alle HDInsight-Cluster zu suchen, die für die Verwendung von Azure Log Analytics konfiguriert sind, und drücken Sie dann die **EINGABETASTE**. 

         search "Error"

    Es wird eine Ausgabe ähnlich der folgenden angezeigt:

    ![Ausgabe bei Suchen aller Fehler](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Ausgabe bei Suchen aller Fehler")

5. Wählen Sie im linken Bereich unter der Kategorie **Typ** einen Fehlertyp aus, den Sie näher untersuchen möchten, und klicken Sie dann auf **Anwenden**.  Beachten Sie, dass die Ergebnisse optimiert werden, sodass nur Fehler des ausgewählten Typs angezeigt werden.
7. Sie können diese Fehlerliste mithilfe der im linken Bereich verfügbaren Optionen näher untersuchen. Beispiel: 

    - So zeigen Sie Fehlermeldungen von einem bestimmten Workerknoten an

        ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Ausgabe bei Suchen nach bestimmten Fehlern")

    - So zeigen Sie Fehler an, die zu einem bestimmten Zeitpunkt aufgetreten sind

        ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Ausgabe bei Suchen nach bestimmten Fehlern")

9. So zeigen Sie den genauen Fehler an Sie können auf **[+] Mehr anzeigen** klicken, um die eigentliche Fehlermeldung anzuzeigen.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Ausgabe bei Suchen nach bestimmten Fehlern")

## <a name="create-alerts-for-tracking-events"></a>Erstellen von Warnungen zum Nachverfolgen von Ereignissen

Damit eine Warnung erstellt werden kann, muss eine Abfrage vorhanden sein, auf deren Basis die Warnung ausgelöst wird. Der Einfachheit halber verwenden wir die folgende Abfrage, die eine Liste auf HDInsight-Clustern ausgeführter Anwendungen bereitstellt, bei denen Fehler aufgetreten sind.

    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

Sie können jede beliebige Abfrage verwenden, um eine Warnung zu erstellen.

1. Öffnen Sie im Azure-Portal einen HDInsight-Cluster, den Sie Azure Log Analytics zugeordnet haben.
2. Klicken Sie auf **Überwachung** und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im OMS-Dashboard auf der Startseite auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Führen Sie die folgende Abfrage aus, zu der Sie eine Warnung erstellen möchten, und drücken dann die **EINGABETASTE**.

        metrics_resourcemanager_queue_root-default-CL | where AppsFailed_d > 0

4. Klicken Sie oben auf der Seite auf **Warnung**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Abfrage eingeben, um eine Warnung zu erstellen")

4. Geben Sie im Fenster **Warnungsregel hinzufügen** die Abfrage und andere Details zum Erstellen einer Warnung ein, und klicken Sie dann auf **Speichern**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Abfrage eingeben, um eine Warnung zu erstellen")

    Im Screenshot ist die Konfiguration dargestellt, mit der eine E-Mail-Benachrichtigung gesendet wird, wenn die Warnungsabfrage eine Ausgabe zurückgibt.

5. Sie können auch eine vorhandene Warnung bearbeiten oder löschen. Klicken Sie dazu auf einer anderen Seite im OMS-Portal auf das Symbol **Einstellungen**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Abfrage eingeben, um eine Warnung zu erstellen")

6. Klicken Sie auf der Seite **Einstellungen** auf **Warnungen**, um die Warnungen anzuzeigen, die Sie erstellt haben. Sie können eine Warnung auch aktivieren oder deaktivieren, bearbeiten oder löschen. Weitere Informationen finden Sie unter [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Weitere Informationen

* [OMS Log Analytics: Create Tiles, Drill-ins and Dashboards with the View Designer](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/) (OMS Log Analytics: Erstellen von Kacheln, Drill-ins und Dashboards mit dem Ansicht-Designer)
* [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)

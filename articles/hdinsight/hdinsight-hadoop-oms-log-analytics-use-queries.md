---
title: Abfragen von Azure Log Analytics zum Überwachen von Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie in Azure Log Analytics Abfragen zum Überwachen von Aufträgen ausführen, die in einem HDInsight-Cluster ausgeführt werden.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: hrasheed
ms.openlocfilehash: 895ffdf2641cc1d8b61917d35b307b013da49eb6
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013206"
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Abfragen von Azure Log Analytics zum Überwachen von HDInsight-Clustern

Betrachten Sie einige grundlegende Szenarien zur Verwendung von Azure Log Analytics für die Überwachung von Azure HDInsight-Clustern:

* [Analysieren von HDInsight-Clustermetriken](#analyze-hdinsight-cluster-metrics)
* [Suchen nach bestimmten Protokollmeldungen](#search-for-specific-log-messages)
* [Erstellen von Ereigniswarnungen](#create-alerts-for-tracking-events)

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen einen HDInsight-Cluster konfiguriert haben, um Azure Log Analytics verwenden zu können, und die spezifischen Log Analytics-Verwaltungslösungen für den HDInsight-Cluster dem Arbeitsbereich hinzugefügt haben. Anweisungen hierzu finden Sie unter [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern [Vorschau]).

## <a name="analyze-hdinsight-cluster-metrics"></a>Analysieren von HDInsight-Clustermetriken

Erfahren Sie, wie Sie nach bestimmten Metriken für Ihren HDInsight-Cluster suchen.

1. Öffnen Sie den Log Analytics-Arbeitsbereich, der Ihrem HDInsight-Cluster zugeordnet ist, im Azure-Portal.
2. Wählen Sie die Kachel **Protokollsuche** aus.
3. Geben Sie im Suchfeld die folgende Abfrage ein, um nach allen verfügbaren Metriken für alle HDInsight-Cluster, die für die Verwendung von Azure Log Analytics konfiguriert sind, zu suchen, und wählen Sie dann **AUSFÜHREN**.

        search *

    ![Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Suchen aller Metriken")

    Die Ausgabe ähnelt der folgenden:

    ![Ausgabe bei Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Ausgabe bei Suchen aller Metriken")

5. Wählen Sie im linken Bereich unter **Typ** eine Metrik aus, die Sie näher untersuchen möchten, und anschließend **Anwenden**. Im folgenden Screenshot ist der Typ `metrics_resourcemanager_queue_root_default_CL` ausgewählt.

    > [!NOTE]
    > Unter Umständen müssen Sie die Schaltfläche **[+]Weitere** wählen, um die gesuchte Metrik zu finden. Außerdem befindet sich die Schaltfläche **Übernehmen** am unteren Rand der Liste, sodass Sie nach unten scrollen müssen, um sie anzuzeigen.

    Beachten Sie, dass die Abfrage im Textfeld jetzt in die Abfrage geändert wird, die im folgenden Screenshot im hervorgehoben Feld angezeigt wird:

    ![Suchen nach bestimmten Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Suchen nach bestimmten Metriken")

6. Sie können diese spezifische Metrik jetzt näher untersuchen. Beispielsweise können Sie über die folgende Abfrage die vorhandene Ausgabe basierend auf dem Mittelwert der in einem Intervall von 10 Minuten verwendeten Ressourcen verfeinern, wobei die Kategorisierung nach dem Clusternamen erfolgt:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. Anstelle der auf dem Mittelwert der verwendeten Ressourcen basierenden Optimierung können Sie die Ergebnisse mit der folgenden Abfrage basierend auf dem Zeitpunkt in einem 10-Minuten-Fenster optimieren, an dem die meisten Ressourcen verwendet wurden (sowie den Zeitpunkten beim 90. und 95. Perzentil):

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Suchen nach bestimmten Protokollmeldungen

Erfahren Sie, wie Sie in einem bestimmten Zeitfenster nach Fehlermeldungen suchen. Die folgenden Schritte sind nur ein Beispiel dafür, wie Sie die Fehlermeldung finden können, die Sie interessiert. Sie können jede verfügbare Eigenschaft verwenden, um nach den Fehlern zu suchen, die Sie finden möchten.

1. Öffnen Sie den Log Analytics-Arbeitsbereich, der Ihrem HDInsight-Cluster zugeordnet ist, im Azure-Portal.
2. Wählen Sie die Kachel **Protokollsuche** aus.
3. Geben Sie die folgende Abfrage ein, um nach allen Fehlermeldungen für alle HDInsight-Cluster zu suchen, die für die Verwendung von Azure Log Analytics konfiguriert sind, und wählen Sie dann **AUSFÜHREN**. 

         search "Error"

    Es wird eine Ausgabe ähnlich der folgenden angezeigt:

    ![Ausgabe bei Suchen aller Fehler](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Ausgabe bei Suchen aller Fehler")

4. Wählen Sie im linken Bereich unter der Kategorie **Typ** einen Fehlertyp aus, den Sie näher untersuchen möchten, und anschließend **Anwenden**.  Beachten Sie, dass die Ergebnisse optimiert werden, sodass nur Fehler des ausgewählten Typs angezeigt werden.
5. Sie können diese Fehlerliste mithilfe der im linken Bereich verfügbaren Optionen näher untersuchen. Beispiel: 

    - So zeigen Sie Fehlermeldungen von einem bestimmten Workerknoten an

        ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Ausgabe bei Suchen nach bestimmten Fehlern")

    - So zeigen Sie Fehler an, die zu einem bestimmten Zeitpunkt aufgetreten sind

        ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Ausgabe bei Suchen nach bestimmten Fehlern")

6. So zeigen Sie den genauen Fehler an Sie können **[+] Mehr anzeigen** wählen, um die eigentliche Fehlermeldung anzuzeigen.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Ausgabe bei Suchen nach bestimmten Fehlern")

## <a name="create-alerts-for-tracking-events"></a>Erstellen von Warnungen zum Nachverfolgen von Ereignissen

Damit eine Warnung erstellt werden kann, muss eine Abfrage vorhanden sein, auf deren Basis die Warnung ausgelöst wird. Sie können jede beliebige Abfrage verwenden, um eine Warnung zu erstellen.

1. Öffnen Sie den Log Analytics-Arbeitsbereich, der Ihrem HDInsight-Cluster zugeordnet ist, im Azure-Portal.
2. Wählen Sie die Kachel **Protokollsuche** aus.
3. Führen Sie die folgende Abfrage aus, zu der Sie eine Warnung erstellen möchten, und wählen Sie dann **AUSFÜHREN**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    Die Abfrage stellt eine Liste mit fehlerhaften Anwendungen bereit, die in HDInsight-Clustern ausgeführt werden.

4. Wählen Sie oben auf der Seite die Option **Neue Warnungsregel**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Abfrage eingeben, um eine Warnung zu erstellen")

5. Geben Sie im Fenster **Regel erstellen** die Abfrage und andere Details zum Erstellen einer Warnung ein, und wählen Sie dann **Warnungsregel erstellen**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Abfrage eingeben, um eine Warnung zu erstellen")

Bearbeiten oder löschen Sie eine vorhandene Warnung wie folgt:

1. Öffnen Sie im Azure-Portal den Log Analytics-Arbeitsbereich.
2. Wählen Sie im Menü auf der linken Seite die Option **Warnung**.
3. Wählen Sie die Warnung aus, die Sie bearbeiten oder löschen möchten.
4. Sie haben folgende Optionen: **Speichern**, **Verwerfen**, **Deaktivieren** und **Löschen**.

    ![HDInsight Log Analytics – Warnung – Löschen/Bearbeiten](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Weitere Informationen finden Sie unter [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Weitere Informationen

* [OMS Log Analytics: Create Tiles, Drill-ins and Dashboards with the View Designer](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/) (OMS Log Analytics: Erstellen von Kacheln, Drill-ins und Dashboards mit dem Ansicht-Designer)
* [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)

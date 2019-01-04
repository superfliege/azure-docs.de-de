---
title: Verwenden von Apache Hive in der Abfrage-Konsole in HDInsight – Azure
description: In diesem Artikel erfahren Sie, wie Sie mit der HDInsight-Abfragekonsole Apache Hive-Abfragen auf einem Hadoop-Cluster in HDInsight in Ihrem Browser ausführen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 1e638bd348b7a5272dd8bfbe25aa841f38a51b9a
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409699"
---
# <a name="run-apache-hive-queries-using-the-query-console"></a>Ausführen von Apache Hive-Abfragen mithilfe der Abfragekonsole
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie mit der HDInsight-Abfragekonsole Apache Hive-Abfragen auf einem Hadoop-Cluster in HDInsight in Ihrem Browser ausführen.

> [!IMPORTANT]  
> Die HDInsight-Abfragekonsole ist nur auf Windows-basierten HDInsight-Clustern verfügbar. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Informationen zum Ausführen von Hive-Abfragen über einen Webbrowser für HDInsight 3.4 oder höher finden Sie unter [Ausführen von Apache Hive-Abfragen in Ambari Hive View](apache-hadoop-use-hive-ambari-view.md).

## <a id="prereq"></a>Voraussetzungen
Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Windows-basierten Hadoop-Cluster in HDInsight
* Einen zeitgemäßen Webbrowser

## <a id="run"></a> Ausführen von Apache Hive-Abfragen mithilfe der Abfragekonsole
1. Öffnen Sie einen Webbrowser, und navigieren Sie zu **https://CLUSTERNAME.azurehdinsight.net**, wobei **CLUSTERNAME** der Name des HDInsight-Clusters ist. Geben Sie nach der entsprechenden Aufforderung den Benutzernamen samt Kennwort ein, den Sie beim Erstellen des Clusters eingegeben haben.
2. Wählen Sie am Seitenanfang in den Links die Option **Hive-Editor**aus. Dadurch wird ein Formular angezeigt, mit dem Sie HiveQL-Anweisungen eingeben können, die Sie auf dem HDInsight-Cluster ausführen möchten.

    ![im Hive-Editor](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Ersetzen Sie den Text `Select * from hivesampletable` durch die folgenden HiveQL-Anweisungen:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen die folgenden Aktionen aus:

   * **DROP TABLE**: Löscht die Tabelle und Datendatei, falls die Tabelle bereits vorhanden ist.
   * **CREATE EXTERNAL TABLE**: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen dienen nur zum Speichern der Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.

     > [!NOTE]  
     > Wenn erwartet wird, dass die zugrunde liegenden Daten über eine externe Quelle (z. B. einen automatisierten Prozess zum Hochladen von Daten) oder über einen anderen MapReduce-Vorgang aktualisiert, aber von Hive immer die neuesten Daten verwendet werden, sollten externe Tabellen verwendet werden.
     >
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.
     >
     >
   * **ROW FORMAT**: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.
   * **STORED AS TEXTFILE LOCATION**: Teilt Hive den Speicherort der Daten (das Verzeichnis „example/data“) und die Information mit, dass die Speicherung als Text erfolgt.
   * **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Dadurch sollte der Wert **3** zurückgegeben werden, da dieser Wert in drei Zeilen enthalten ist.
   * **INPUT__FILE__NAME LIKE '%.log'**: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Dies schränkt die Suche auf die Datei "sample.log" ein, die die Daten enthält, und verhindert, dass Daten aus anderen Beispieldatendateien zurückgegeben werden, die nicht dem von uns definierten Schema entsprechen.
3. Klicken Sie auf **Submit**. Die **Auftragssitzung** am unteren Seitenrand sollte Details für den Auftrag anzeigen.
4. Sobald sich die Angabe im Feld **Status** in **Abgeschlossen** ändert, wählen Sie **Details anzeigen** für den Auftrag aus. Auf der Detailseite enthält die **Auftragsausgabe** die Zeichenfolge `[ERROR]    3`. Sie können die Schaltfläche **Herunterladen** unter diesem Feld verwenden, um eine Datei herunterzuladen, die die Ausgabe des Auftrags enthält.

## <a id="summary"></a>Zusammenfassung
Wie Sie sehen können, bietet die Abfragekonsole eine einfache Möglichkeit, um Hive-Abfragen auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

Um weitere Informationen zu Hive mithilfe der Abfragekonsole zu erhalten, wählen Sie am oberen Rand der Abfragekonsole **Erste Schritte** aus, und verwenden Sie dann die Beispiele. In jedem Beispiel werden Sie durch die Datenanalyse mit Hive geführt, einschließlich der Erläuterungen zu HiveQL-Anweisungen, die in diesem Beispiel verwendet werden.

## <a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Apache Pig mit Apache Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit Tez mit Hive verwenden, finden Sie in den folgenden Dokumenten Informationen zum Debuggen:

* [Verwenden der Apache Tez-Benutzeroberfläche in Windows-basiertem HDInsight](../hdinsight-debug-tez-ui.md)
* [Verwenden der Apache Ambari-Tez-Ansicht in Linux-basiertem HDInsight](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

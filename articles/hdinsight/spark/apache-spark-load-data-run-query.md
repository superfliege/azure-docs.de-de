---
title: 'Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight | Microsoft-Dokumentation'
description: Informationen zum Laden von Daten und Ausführen interaktiver Abfragen auf Spark-Clustern in Azure HDInsight.
services: azure-hdinsight
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: tutorial
ms.author: jgao
ms.date: 05/07/2018
ms.openlocfilehash: 63a876dc148129cd2a3eb93ed7ab6baf06a07c62
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight

In diesem Tutorial erfahren Sie, wie Sie einen Dataframe aus einer CSV-Datei erstellen und interaktive Spark SQL-Abfragen für einen Apache Spark-Cluster in Azure HDInsight ausführen. In Spark ist ein Dataframe eine verteilte Sammlung von Daten, die in benannten Spalten organisiert sind. Dataframe entspricht vom Konzept her einer Tabelle in einer relationalen Datenbank oder einem Datenrahmen in R/Python.
 
In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Dataframes aus einer CSV-Datei
> * Ausführen von Abfragen gegen Dataframes

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Abschließen von [Erstellen eines Apache Spark-Clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-dataframe-from-a-csv-file"></a>Erstellen eines Dataframes aus einer CSV-Datei

Anwendungen können Dataframes aus vorhandenen Resilient Distributed Datasets (RDDs), Hive-Tabellen oder Datenquellen mit dem SQLContext-Objekt erstellen. Der folgende Screenshot zeigt eine Momentaufnahme der in diesem Tutorial verwendeten HVAC.csv-Datei. Die CSV-Datei enthält alle HDInsight Spark-Cluster. Die Daten erfassen die Temperaturunterschiede in einigen Gebäuden.
    
![Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage")


1. Öffnen Sie das Jupyter Notebook, das Sie im Voraussetzungenabschnitt erstellt haben.
2. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Mit dem Code werden die Typen importiert, die für dieses Szenario benötigt werden:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Beim Ausführen einer interaktiven Abfrage in Jupyter wird in der Titelleiste Ihres Webbrowserfensters oder Ihrer Registerkarte neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

    ![Status der interaktiven Spark SQL-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status der interaktiven Spark SQL-Abfrage")

3. Führen Sie den folgenden Code aus, um einen Dataframe und eine temporäre Tabelle (**hvac**) zu erstellen. Der Code extrahiert nicht alle verfügbaren Spalten in der CSV-Datei. 

    ```PySpark
    # Create an RDD from sample data
    hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
    # Create a schema for the data
    Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')
    
    # Parse the data and create a schema
    hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
    hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
    
    # Infer the schema and create a table       
    hvacTable = sqlContext.createDataFrame(hvac)
    hvacTable.registerTempTable('hvactemptable')
    dfw = DataFrameWriter(hvacTable)
    dfw.saveAsTable('hvac')
    ```

    > [!NOTE]
    > Wenn Sie den PySpark-Kernel zum Erstellen eines Notebooks verwenden, werden SQL-Kontexte automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie müssen nicht explizit Kontext erstellen.


## <a name="run-queries-on-the-dataframe"></a>Ausführen von Abfragen gegen Dataframes

Nach dem Erstellen der Tabelle führen Sie eine interaktive Abfrage für die Daten aus.

1. Führen Sie den folgenden Code in einer leeren Zelle des Notebooks aus:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Da Sie einen PySpark-Kernel im Notebook verwenden, können Sie jetzt direkt eine interaktive SQL-Abfrage für die temporäre Tabelle **hvac** ausführen.

   Die folgende Ausgabe in Tabellenform wird angezeigt.

     ![Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage")

3. Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Wählen Sie zum Anzeigen eines Bereichsdiagramms für die gleiche Ausgabe **Bereich** aus, und legen Sie die anderen Werte wie gezeigt fest.

    ![Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage")

10. Wählen Sie im Menü **File** (Datei) des Notebooks die Option **Save and Checkpoint** (Speichern und Prüfpunkt) aus. 

11. Wenn Sie jetzt mit dem [nächsten Tutorial](apache-spark-use-bi-tools.md) fortfahren, lassen Sie das Notebook geöffnet. Falls nicht, fahren Sie das Notebook herunter, um die Clusterressourcen freizugeben: Wählen Sie im Menü **File** (Datei) des Notebooks **Close and Halt** (Schließen und Anhalten) aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Mit HDInsight werden Ihre Daten in Azure Storage oder Azure Data Lake Store gespeichert, sodass Sie einen Cluster ohne Weiteres löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Wenn Sie vorhaben, sofort mit dem nächsten Tutorial fortzufahren, können Sie den Cluster beibehalten.

Öffnen Sie den Cluster im Azure-Portal, und wählen Sie **Löschen** aus.

![Löschen eines HDInsight-Clusters](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Löschen eines HDInsight-Clusters")

Sie können auch den Namen der Ressourcengruppe auswählen, um die Seite für die Ressourcengruppe zu öffnen, und dann **Ressourcengruppe löschen** auswählen. Indem Sie die Ressourcengruppe löschen, löschen Sie sowohl den HDInsight-Spark-Cluster als auch das Standardspeicherkonto.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen Sie einen Spark-Dataframe.
* Führen Sie Spark SQL für den Dataframe aus.

Fahren Sie mit dem nächsten Artikel fort, um festzustellen, wie die Daten, die Sie in Spark registriert haben, in ein BI-Analyse-Tool wie Power BI gezogen werden können. 
> [!div class="nextstepaction"]
> [Analysieren von Daten mithilfe von BI-Tools](apache-spark-use-bi-tools.md)


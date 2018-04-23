---
title: Ausführen interaktiver Abfragen in einem Azure HDInsight Spark-Cluster | Microsoft-Dokumentation
description: HDInsight Spark-Schnellstartanleitung zum Erstellen eines Apache Spark-Clusters in HDInsight.
keywords: Spark Schnellstart,interaktiv Spark,interaktive Abfrage,HDInsight Spark,Azure Spark
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 177fb47c72e9abbafcda69416643fbd3848373bd
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="run-interactive-queries-on-spark-clusters-in-hdinsight"></a>Ausführen interaktiver Abfragen auf Spark-Clustern in HDInsight

Erfahren Sie, wie Sie mit Jupyter Notebook interaktive Spark-SQL-Abfragen für einen Spark-Cluster ausführen. 

[Jupyter Notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html) ist eine browserbasierte Anwendung, die die konsolenbasierte interaktive Oberfläche im Web erweitert. Spark in HDInsight enthält auch [Zeppelin Notebook](apache-spark-zeppelin-notebook.md). In diesem Tutorial wird Jupyter Notebook verwendet.

Jupyter Notebooks in HDInsight-Clustern unterstützen drei Kernels: **PySpark**, **PySpark3** und **Spark**. In diesem Tutorial wird der Kernel **PySpark** verwendet. Weitere Informationen zu den Kernels und den Vorteilen von **PySpark**finden Sie unter [Kernel für Jupyter-Notebook in Spark-Clustern in Azure HDInsight](apache-spark-jupyter-notebook-kernels.md). Informationen zum Verwenden von Zeppelin Notebook finden Sie unter [Verwenden von Zeppelin Notebooks mit Apache Spark-Cluster in Azure HDInsight](./apache-spark-zeppelin-notebook.md).

In diesem Tutorial fragen Sie Daten in einer CSV-Datei ab. Zuerst müssen Sie die Daten als Dataframe in Spark laden. Anschließend können Sie mit Jupyter Notebook Abfragen auf den Dataframe ausführen. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure HDInsight Spark-Cluster**. Informationen dazu finden Sie unter [Erstellen eines Apache Spark-Clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* **Ein Jupyter Notebook mit PySpark**. Informationen dazu finden Sie unter [Erstellen eines Jupyter Notebooks](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

## <a name="create-a-dataframe-from-a-csv-file"></a>Erstellen eines Dataframes aus einer CSV-Datei

Mit einem SQLContext können Anwendungen Dataframes aus vorhandenen RDDs, Hive-Tabellen oder Datenquellen erstellen. 

**Erstellen eines Dataframes aus einer CSV-Datei**

1. Erstellen Sie ein Jupyter Notebook mit PySpark, wenn Sie noch keins besitzen. Informationen dazu finden Sie unter [Erstellen eines Jupyter Notebooks](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Mit dem Code werden die Typen importiert, die für dieses Szenario benötigt werden:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```
    Wenn Sie den PySpark-Kernel zum Erstellen eines Notebooks verwenden, werden Spark- und Hive-Kontexte automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie müssen nicht explizit Kontext erstellen.

    Beim Ausführen einer interaktiven Abfrage in Jupyter wird in der Titelleiste Ihres Webbrowserfensters oder Ihrer Registerkarte neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

    ![Status der interaktiven Spark SQL-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status der interaktiven Spark SQL-Abfrage")

3. Führen Sie den folgenden Code zum Erstellen eines Dataframes und einer temporären Tabelle (**hvac**) aus, indem Sie den folgenden Code ausführen: Der Code extrahiert nicht alle verfügbaren Spalten in der CSV-Datei. 

    ```PySpark
    # Create an RDD from sample data
    hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
    # Create a schema for our data
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
    Der folgende Screenshot zeigt eine Momentaufnahme der HVAC.csv-Datei. Die CSV-Datei enthält alle HDInsigt Spark-Cluster. Die Daten erfassen die Temperaturunterschiede in einem Gebäude.

    ![Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Momentaufnahme der Daten für die interaktive Spark-SQL-Abfrage")

## <a name="run-queries-on-the-dataframe"></a>Ausführen von Abfragen gegen Dataframes

Nach dem Erstellen der Tabelle führen Sie eine interaktive Abfrage für die Daten aus.

**Ausführen einer Abfrage**

1. Führen Sie den folgenden Code in einer leeren Zelle des Notebooks aus:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Da Sie einen PySpark-Kernel im Notebook verwenden, können Sie jetzt direkt eine interaktive SQL-Abfrage für die temporäre Tabelle **hvac** ausführen, die Sie mit der `%%sql`-Magic erstellt haben. Weitere Informationen zur `%%sql`-Magic sowie anderen für den PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Spark-Clustern unter HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Die folgende Ausgabe in Tabellenform wird standardmäßig angezeigt.

     ![Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage")

3. Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Wählen Sie zum Anzeigen eines Bereichsdiagramms für die gleiche Ausgabe **Bereich** aus, und legen Sie die anderen Werte wie gezeigt fest.

    ![Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage")

10. Klicken Sie im Menü **Datei** des Notebooks auf die Option **Save and Checkpoint** (Speichern und Prüfpunkt). 

11. Wenn Sie jetzt mit dem [nächsten Tutorial](apache-spark-use-bi-tools.md) fortfahren, lassen Sie das Notebook geöffnet. Falls nicht, fahren Sie das Notebook herunter, um die Clusterressourcen freizugeben: Klicken Sie im Menü **Datei** des Notebooks auf **Schließen und Anhalten**.

## <a name="next-step"></a>Nächster Schritt

In diesem Artikel haben Sie erfahren, wie Sie interaktive Abfragen in Spark mithilfe von Jupyter Notebook ausführen. Fahren Sie mit dem nächsten Artikel fort, um festzustellen, wie die Daten, die Sie in Spark registriert haben, in ein BI-Analyse-Tool wie Power BI und Tableau gezogen werden können. 

> [!div class="nextstepaction"]
>[Apache Spark BI mit Datenvisualisierungstools unter Azure HDInsight](apache-spark-use-bi-tools.md)





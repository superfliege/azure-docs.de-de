---
title: "Erstellen von Apache Spark-Machine Learning-Anwendungen für Azure HDInsight | Microsoft-Dokumentation"
description: Schrittweise Anleitung zum Erstellen einer Apache Spark-Machine Learning-Anwendung in HDInsight Spark-Clustern mit Jupyter-Notebook
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 74dcd368d8696df26c5ad294c5657161fbe7f408
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Erstellen von Apache Spark-Machine Learning-Anwendungen für Azure HDInsight

Erfahren Sie, wie Sie eine Apache Spark-Machine Learning-Anwendung mit einem Spark-Cluster unter HDInsight erstellen. In diesem Artikel wird beschrieben, wie Sie das Jupyter-Notebook für den Cluster zum Erstellen und Testen dieser Anwendung verwenden. Für die Anwendung werden die HVAC.csv-Beispieldaten genutzt, die standardmäßig auf allen Clustern verfügbar sind.

[MLib](https://spark.apache.org/docs/1.1.0/mllib-guide.html) ist die skalierbare Machine Learning-Bibliothek von Spark mit gängigen Lernalgorithmen und Hilfsprogrammen – einschließlich Klassifizierung, Regression, Clustering, kombinierten Filtern und Reduktion der Anzahl von Dimensionen sowie zugrunde liegenden Optimierungsprimitiven.

**Voraussetzungen:**

Sie benötigen Folgendes:

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Grundlegendes zum Dataset

Die folgenden Daten zeigen die Zieltemperatur und die Ist-Temperatur von Gebäuden an, in denen HVAC-Systeme installiert sind. Die Spalte **System** enthält die System-ID und die Spalte **SystemAge** eine Angabe in Jahren, wie lange das HVAC-System im Gebäude bereits verwendet wird. Sie sagen anhand der Daten in diesem Tutorial vorher, ob es in einem Gebäude basierend auf der Zieltemperatur zu warm oder zu kalt ist. Hierfür werden die System-ID und das Alter des Systems verwendet.

![Momentaufnahme der für das Spark-Machine Learning-Beispiel verwendeten Daten](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Momentaufnahme der für das Spark-Machine Learning-Beispiel verwendeten Daten")

Die Datendatei **HVAC.csv** befindet sich in allen HDInsight-Clustern unter **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

## <a name="app"></a>Schreiben einer Spark-Machine Learning-Anwendung mit Spark MLlib
In dieser Anwendung verwenden Sie eine Spark [ML-Pipeline](https://spark.apache.org/docs/2.2.0/ml-pipeline.html), um eine Dokumentklassifizierung durchzuführen. ML-Pipelines bieten eine einheitliche Gruppe von auf Dataframes aufgesetzten High-Level-APIs, mit deren Hilfe Benutzer praktische Machine Learning-Pipelines erstellen und optimieren können. In der Pipeline teilen Sie das Dokument in Wörter auf, konvertieren die Wörter in einen numerischen Featurevektor und erstellen dann mit den Featurevektoren und Beschriftungen ein Vorhersagemodell. Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen:

1. Erstellen Sie ein Jupyter Notebook mit dem PySpark-Kernel. Informationen dazu finden Sie unter [Erstellen eines Jupyter Notebooks](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Importieren Sie die Typen, die für dieses Szenario benötigt werden. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABETASTE**. 

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row

    import os
    import sys
    from pyspark.sql.types import *

    from pyspark.mllib.classification import LogisticRegressionWithSGD
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array
    ```
3. Lasen Sie die Daten (hvac.csv), analysieren Sie sie, und verwenden Sie sie zum Trainieren des Modells. 

    ```PySpark
    # Define a type called LabelDocument
    LabeledDocument = Row("BuildingID", "SystemInfo", "label")

    # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
    def parseDocument(line):
        values = [str(x) for x in line.split(',')]
        if (values[3] > values[2]):
            hot = 1.0
        else:
            hot = 0.0        

        textValue = str(values[4]) + " " + str(values[5])

        return LabeledDocument((values[6]), textValue, hot)

    # Load the raw HVAC.csv file, parse it using the function
    data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
    training = documents.toDF()
    ```

    Im Codeausschnitt definieren Sie eine Funktion, die die Ist-Temperatur mit der Zieltemperatur vergleicht. Wenn die Ist-Temperatur höher ist, wird angegeben, dass es in dem Gebäude zu warm ist (Wert **1,0**). Andernfalls wird angegeben, dass es im Gebäude zu kalt ist (Wert **0.0**). 

4. Konfigurieren Sie die Spark Machine Learning-Pipeline, die drei Phasen umfasst: Tokenizer, hashingTF und lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    Weitere Informationen dazu, was eine Pipeline ist und wie sie funktioniert, finden Sie unter <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark Machine Learning-Pipeline</a>.

5. Passen Sie die Pipeline an das Schulungsdokument an.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Überprüfen Sie das Schulungsdokument, um Ihre Fortschritte in Bezug auf die Anwendung zu ermitteln.
   
    ```PySpark
    training.show()
    ```
   
    Sie sollten eine Ausgabe erhalten, die Folgendem ähnelt:

    ```
    +----------+----------+-----+
    |BuildingID|SystemInfo|label|
    +----------+----------+-----+
    |         4|     13 20|  0.0|
    |        17|      3 20|  0.0|
    |        18|     17 20|  1.0|
    |        15|      2 23|  0.0|
    |         3|      16 9|  1.0|
    |         4|     13 28|  0.0|
    |         2|     12 24|  0.0|
    |        16|     20 26|  1.0|
    |         9|      16 9|  1.0|
    |        12|       6 5|  0.0|
    |        15|     10 17|  1.0|
    |         7|      2 11|  0.0|
    |        15|      14 2|  1.0|
    |         6|       3 2|  0.0|
    |        20|     19 22|  0.0|
    |         8|     19 11|  0.0|
    |         6|      15 7|  0.0|
    |        13|      12 5|  0.0|
    |         4|      8 22|  0.0|
    |         7|      17 5|  0.0|
    +----------+----------+-----+
    ```

    Vergleichen Sie die Ausgabe mit der CSV-Rohdatei. Die erste Zeile der CSV-Datei enthält beispielsweise folgende Daten:

    ![Momentaufnahme der Ausgabedaten für das Spark-Machine Learning-Beispiel](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Momentaufnahme der Ausgabedaten für das Spark-Machine Learning-Beispiel")

    Beachten Sie, dass die Ist-Temperatur unterhalb der Zieltemperatur liegt. Im Gebäude ist es also zu kalt. In der Schulungsausgabe lautet der Wert **label** in der ersten Zeile daher **0,0**. Dies bedeutet, dass es im Gebäude nicht zu warm ist.

7. Bereiten Sie ein Dataset vor, für das das Schulungsmodell ausgeführt werden kann. Hierzu übergeben Sie eine System-ID und ein Systemalter (in der Schulungsausgabe als **SystemInfo** bezeichnet). Mit dem Modell wird dann vorhergesagt, ob es im Gebäude mit der jeweiligen System-ID und dem Systemalter wärmer (1,0) oder kälter (0,0) ist.
   
    ```PySpark   
    # SystemInfo here is a combination of system ID followed by system age
    Document = Row("id", "SystemInfo")
    test = sc.parallelize([(1L, "20 25"),
                    (2L, "4 15"),
                    (3L, "16 9"),
                    (4L, "9 22"),
                    (5L, "17 10"),
                    (6L, "7 22")]) \
        .map(lambda x: Document(*x)).toDF() 
    ```
8. Treffen Sie als Letztes die Vorhersagen für die Testdaten. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   In der ersten Zeile der Vorhersage können Sie sehen, dass das Gebäude für ein HVAC-System mit ID 20 und einem Systemalter von 25 Jahren zu warm ist (**prediction=1.0**). Der erste Wert für DenseVector (0,49999) entspricht der Vorhersage 0,0, und der zweite Wert (0,5001) entspricht der Vorhersage 1,0. Obwohl der zweite Wert in der Ausgabe nur unwesentlich höher ist, zeigt das Modell **prediction=1.0**an.
10. Fahren Sie das Notebook herunter, um die Ressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**. Dadurch wird das Notebook heruntergefahren und geschlossen.

## <a name="anaconda"></a>Verwenden der Anaconda-scikit-learn-Bibliothek für Spark-Machine Learning
Apache Spark-Cluster unter HDInsight enthalten Anaconda-Bibliotheken. Dazu gehört auch die **scikit-learn** -Bibliothek für Machine Learning. Außerdem enthält die Bibliothek verschiedene Datasets, mit denen Sie Beispielanwendungen direkt über ein Jupyter Notebook erstellen können. Beispiele zur Verwendung der scikit-learn-Bibliothek finden Sie unter [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeit-Streaminganwendungen mithilfe von Spark in HDInsight](apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md

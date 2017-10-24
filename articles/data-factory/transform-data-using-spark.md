---
title: "Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Daten durch Ausführen von Spark-Programmen in einer Azure Data-Factory-Pipeline mithilfe der Spark-Aktivität transformieren können."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: shengc
ms.openlocfilehash: f1548c6ad397a7154482fa73e992aef9201c5752
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-spark.md)
> * [Version 2 – Vorschauversion](transform-data-using-spark.md)

Die Spark-Aktivität in einer Data Factory-[Pipeline](concepts-pipelines-activities.md) führt Spark-Programme in [Ihrem eigenen](compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsabhängigen](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-Cluster aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet. Wenn Sie einen bedarfsgesteuerten mit Spark verknüpften Dienst verwenden, erstellt Data Factory für Sie automatisch bei Bedarf einen Spark-Cluster zum Verarbeiten der Daten und löscht dann den Cluster, sobald die Verarbeitung abgeschlossen ist. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Spark-Aktivität in V1](v1/data-factory-spark.md).

> [!IMPORTANT]
> Die Spark-Aktivität unterstützt keine HDInsight Spark-Cluster, die Azure Data Lake Store als primären Speicher verwenden.

## <a name="spark-activity-properties"></a>Eigenschaften von Spark-Aktivitäten
Dies ist die JSON-Beispieldefinition einer Spark-Aktivität:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark\\pyFiles",
        "entryFilePath": "test.py",
        "arguments": [ "arg1", "arg2" ],
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden:

| Eigenschaft              | Beschreibung                              | Erforderlich |
| --------------------- | ---------------------------------------- | -------- |
| Name                  | Der Name der Aktivität in der Pipeline.    | Ja      |
| Beschreibung           | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.  | Nein       |
| Typ                  | Für die Spark-Aktivität ist der Aktivitätstyp „HDInsightSpark“. | Ja      |
| linkedServiceName     | Name des mit HDInsight Spark verknüpften Diensts, in dem das Spark-Programm ausgeführt wird. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| SparkJobLinkedService | Der verknüpfte Azure Storage-Dienst, der die Datei sowie die Abhängigkeiten und Protokolle für den Spark-Auftrag enthält.  Wenn Sie für diese Eigenschaft keinen Wert angeben, wird der Speicher verwendet, der dem HDInsight-Cluster zugeordnet ist. | Nein       |
| rootPath              | Der Azure-Blobcontainer und -ordner mit der Spark-Datei. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. Details zur Struktur dieses Ordners finden Sie im Abschnitt „Ordnerstruktur“ (nächster Abschnitt). | Ja      |
| entryFilePath         | Der relative Pfad zum Stammordner des Spark-Codes bzw. -Pakets. | Ja      |
| className             | Die Java-/Spark-Hauptklasse der Anwendung.      | Nein       |
| arguments             | Eine Liste der Befehlszeilenargumente für das Spark-Programm. | Nein       |
| proxyUser             | Das Benutzerkonto, dessen Identität angenommen werden soll, um das Spark-Programm auszuführen. | Nein       |
| sparkConfig           | Geben Sie Werte für Spark-Konfigurationseigenschaften an, die im Thema [Spark-Konfiguration – Anwendungseigenschaften](https://spark.apache.org/docs/latest/configuration.html#available-properties) aufgeführt werden. | Nein       |
| getDebugInfo          | Gibt an, ob die Spark-Protokolldateien in den Azure-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von sparkJobLinkedService angegeben wird. Zulässige Werte: Keine, Immer oder Fehler. Standardwert: Keine | Nein       |

## <a name="folder-structure"></a>Ordnerstruktur
Spark-Aufträge lassen sich besser erweitern als Pig- oder Hive-Aufträge. Bei Spark-Aufträgen können Sie mehrere Abhängigkeiten wie z.B. jar-Pakete (im Java-CLASSPATH platziert), Python-Dateien (im PYTHONPATH platziert) sowie beliebige andere Dateien bereitstellen.

Erstellen Sie folgende Ordnerstruktur in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist. Laden Sie dann abhängige Dateien in die entsprechenden Unterordner in dem Stammordner hoch, der von **entryFilePath** repräsentiert wird. Python-Dateien werden beispielsweise in den Unterordner „pyFiles“ und jar-Dateien in den Unterordner „jars“ des Stammordners hochgeladen. Zur Laufzeit erwartet der Data Factory-Dienst die folgende Ordnerstruktur im Azure-Blobspeicher:     

| path                  | Beschreibung                              | Erforderlich | Typ   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (Stamm)            | Der Stammpfad des Spark-Auftrags im verknüpften Speicherdienst. | Ja      | Ordner |
| &lt;benutzerdefiniert&gt; | Der Pfad, der auf die Eingabedatei des Spark-Auftrags zeigt. | Ja      | File   |
| ./jars                | Alle Dateien in diesem Ordner werden hochgeladen und im Java-CLASSPATH des Clusters platziert. | Nein       | Ordner |
| ./pyFiles             | Alle Dateien in diesem Ordner werden hochgeladen und im PYTHONPATH des Clusters platziert. | Nein       | Ordner |
| ./files               | Alle Dateien in diesem Ordner werden hochgeladen und im Executor-Arbeitsverzeichnis platziert. | Nein       | Ordner |
| ./archives            | Alle Dateien in diesem Ordner sind nicht komprimiert. | Nein       | Ordner |
| ./logs                | Der Ordner, der die Protokolle aus dem Spark-Cluster enthält. | Nein       | Ordner |

Hier finden Sie ein Beispiel für einen Speicher mit zwei Spark-Auftragsdateien in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)

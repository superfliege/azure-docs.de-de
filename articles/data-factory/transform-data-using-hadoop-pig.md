---
title: Transformieren von Daten mit der Hadoop Pig-Aktivität in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Pig-Aktivität in Azure Data Factory verwenden können, um Pig-Abfragen in einem bedarfsgesteuerten/eigenen HDInsight-Cluster auszuführen.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 9e769cc436011defe89b12680150e6f9c3b3faf8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049316"
---
# <a name="transform-data-using-hadoop-pig-activity-in-azure-data-factory"></a>Transformieren von Daten mit der Hadoop Pig-Aktivität in Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die Version des Data Factory-Dienstes aus, den Sie verwenden:"]
> * [Version 1](v1/data-factory-pig-activity.md)
> * [Aktuelle Version](transform-data-using-hadoop-pig.md)

Die HDInsight Pig-Aktivität in einer Data Factory-[Pipeline](concepts-pipelines-activities.md) wendet Pig-Abfragen auf [Ihren eigenen](compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsgesteuerten](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-Cluster an. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

Wenn Sie noch nicht mit Azure Data Factory vertraut sind, lesen Sie zunächst den Artikel [Einführung in Azure Data Factory](introduction.md), und durchlaufen Sie anschließend das Tutorial [Transformieren von Daten](tutorial-transform-data-spark-powershell.md), bevor Sie diesen Artikel lesen. 

## <a name="syntax"></a>Syntax

```json
{
    "name": "Pig Activity",
    "description": "description",
    "type": "HDInsightPig",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\PigScripts\\MyPigSript.pig",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }   
}
```
## <a name="syntax-details"></a>Syntaxdetails

| Eigenschaft            | BESCHREIBUNG                              | Erforderlich |
| ------------------- | ---------------------------------------- | -------- |
| name                | Der Name der Aktivität                     | Ja      |
| Beschreibung         | Ein Text, der beschreibt, wofür die Aktivität verwendet wird. | Nein        |
| type                | Für die Hive-Aktivität ist der Aktivitätstyp „HDInsightPig“. | Ja      |
| linkedServiceName   | Verweis auf den HDInsight-Cluster, der als verknüpfter Dienst in Data Factory registriert ist. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| scriptLinkedService | Verweis auf einen verknüpften Azure Storage-Dienst, der zum Speichern des auszuführenden Pig-Skripts verwendet wird. Wenn Sie diesen verknüpften Dienst nicht angeben, wird der im verknüpften HDInsight-Dienst definierte verknüpfte Azure Storage-Dienst genutzt. | Nein        |
| scriptPath          | Geben Sie den Pfad der Skriptdatei an, die im Azure Storage-Speicher gespeichert ist, auf den „scriptLinkedService“ verweist. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Nein        |
| getDebugInfo        | Gibt an, ob die Protokolldateien in den Azure Storage-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von „scriptLinkedService“ angegeben wird. Zulässige Werte: Keine, Immer oder Fehler. Standardwert: Keine | Nein        |
| arguments           | Gibt ein Array von Argumenten für einen Hadoop-Auftrag an. Die Argumente werden als Befehlszeilenargumente an jeden Vorgang übergeben. | Nein        |
| defines             | Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Pig-Skripts an. | Nein        |

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)

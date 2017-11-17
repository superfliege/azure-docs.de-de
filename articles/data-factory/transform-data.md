---
title: Transformieren mit Azure Data Factory | Microsoft-Dokumentation
description: Informationen Sie zum Transformieren von Daten oder Verarbeiten von Daten in Azure Data Factory mit Hadoop, Machine Learning und Azure Data Lake Analytics.
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: shengc
ms.openlocfilehash: 88ff71956c82d79fb244f1a687debeb726291ae8
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="transform-data-in-azure-data-factory"></a>Transformieren von Data in Azure Data Factory
> [!div class="op_single_selector"]
> * [Hive](transform-data-using-hadoop-hive.md)  
> * [Pig](transform-data-using-hadoop-pig.md)  
> * [MapReduce](transform-data-using-hadoop-map-reduce.md)  
> * [Hadoop-Datenströme](transform-data-using-hadoop-streaming.md)
> * [Spark](transform-data-using-spark.md)
> * [Machine Learning](transform-data-using-machine-learning.md) 
> * [Gespeicherte Prozedur](transform-data-using-stored-procedure.md)
> * [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md)
> * [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)

## <a name="overview"></a>Übersicht
Dieser Artikel enthält Informationen zu Datentransformationsaktivitäten in Azure Data Factory, mit denen Sie Ihre Rohdaten in Vorhersagen und Einblicke transformieren und verarbeiten können. Transformationsaktivitäten erfolgen in einer Compute-Umgebung wie Azure HDInsight-Cluster oder einem Azure Batch. Sie finden hier Links zu Artikeln mit detaillierten Informationen zu jeder Transformationsaktivität.

Data Factory unterstützt die folgenden Transformationsaktivitäten, die [Pipelines](concepts-pipelines-activities.md) entweder einzeln oder mit einer anderen Aktivität verkettet hinzugefügt werden können.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, finden Sie unter [Transform data in Azure Data Factory](v1/data-factory-data-transformation-activities.md) (Transformieren von Daten in Azure Data Factory) weitere Informationen.
 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive-Aktivität
Die HDInsight Hive-Aktivität in einer Data Factory-Pipeline wendet Hive-Abfragen auf Ihren eigenen oder bedarfsgesteuerten Windows-/Linux-basierten HDInsight-Cluster an. Im Artikel [Hive-Aktivität](transform-data-using-hadoop-hive.md) finden Sie Details zu dieser Aktivität. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-Aktivität
Die HDInsight Pig-Aktivität in einer Data Factory-Pipeline wendet Pig-Abfragen auf Ihren eigenen oder bedarfsgesteuerten Windows-/Linux-basierten HDInsight-Cluster an. Im Artikel [Pig-Aktivität](transform-data-using-hadoop-pig.md) finden Sie Details zu dieser Aktivität. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-Aktivität
Die HDInsight MapReduce-Aktivität in einer Data Factory-Pipeline wendet MapReduce-Programme auf Ihren eigenen oder bedarfsgesteuerten Windows-/Linux-basierten HDInsight-Cluster an. Im Artikel [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md) finden Sie Details zu dieser Aktivität.

## <a name="hdinsight-streaming-activity"></a>HDInsight-Streamingaktivität
Die HDInsight-Streamingaktivität in einer Data Factory-Pipeline wendet Hadoop-Streamingprogramme auf Ihren eigenen oder bedarfsgesteuerten Windows-/Linux-basierten HDInsight-Cluster an. Unter [HDInsight-Streamingaktivität](transform-data-using-hadoop-streaming.md) finden Sie ausführliche Informationen zu dieser Aktivität.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-Aktivität
Die HDInsight Spark-Aktivität in einer Data Factory-Pipeline führt Spark-Programme in Ihrem eigenen HDInsight-Cluster aus. Weitere Informationen finden Sie unter [Aufrufen von Spark-Programmen aus Azure Data Factory](transform-data-using-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning-Aktivitäten
Azure Data Factory ermöglicht die einfache Erstellung von Pipelines, die einen veröffentlichten Azure Machine Learning-Webdienst für Predictive Analytics nutzen. Bei Verwendung der [Batchausführungsaktivität](transform-data-using-machine-learning.md) in einer Azure Data Factory-Pipeline können Sie einen Machine Learning-Webdienst aufrufen, um Vorhersagen zu den Daten im Batch zu treffen.

Im Laufe der Zeit müssen die Vorhersagemodelle in den Machine Learning-Bewertungsexperimenten mit neuen Eingabedatasets neu trainiert werden. Wenn Sie mit dem erneuten Trainieren fertig sind, sollten Sie den Bewertungswebdienst mit dem neu trainierten Machine Learning-Modell aktualisieren. Verwenden Sie die [Ressourcenaktualisierungsaktivität](update-machine-learning-models.md), um den Webdienst mit dem neu trainierten Modell zu aktualisieren.  

Unter [Verwenden von Machine Learning-Aktivitäten](transform-data-using-machine-learning.md) finden Sie Einzelheiten zu diesen Machine Learning-Aktivitäten. 

## <a name="stored-procedure-activity"></a>Aktivität „Gespeicherte Prozedur“
Sie können die SQL Server-Aktivität „Gespeicherte Prozedur“ in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einem der folgenden Dateispeicher nutzen: Azure SQL-Datenbank, Azure SQL Data Warehouse, SQL Server-Datenbank in Ihrem Unternehmen oder Azure-VM. Unter [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md) finden Sie Details.  

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-Aktivität für Data Lake Analytics
Die U-SQL-Aktivität für Data Lake Analytics wendet ein U-SQL-Skript auf einen Azure Data Lake Analytics-Cluster an. Unter [U-SQL-Aktivität für Data Lake Analytics](transform-data-using-data-lake-analytics.md) finden Sie Details. 

## <a name="net-custom-activity"></a>Benutzerdefinierte .NET-Aktivität
Wenn Sie Daten auf eine Weise transformieren/verarbeiten müssen, die von Data Factory nicht unterstützt wird, können Sie eine benutzerdefinierte Aktivität mit Ihrer eigenen Datenverarbeitungslogik erstellen und in der Pipeline verwenden. Sie können die benutzerdefinierte .NET-Aktivität so konfigurieren, dass sie entweder mithilfe eines Azure Batch-Diensts oder eines Azure HDInsight-Clusters ausgeführt wird. Unter [Verwenden benutzerdefinierter Aktivitäten](transform-data-using-dotnet-custom-activity.md) finden Sie Einzelheiten. 

Sie können eine benutzerdefinierte Aktivität erstellen, um R-Skripts in Ihrem HDInsight-Cluster mit installiertem R auszuführen. Informationen hierzu finden Sie unter [RunRScriptUsingADFSample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Compute-Umgebungen
Sie erstellen einen verknüpften Dienst für die Computeumgebung und verwenden dann den verknüpften Dienst, wenn Sie eine Transformationsaktivität definieren. Es gibt zwei Typen von Compute-Umgebungen, die von Data Factory unterstützt werden. 

- **Bei Bedarf:** In diesem Fall wird die Compute-Umgebung vollständig von Data Factory verwaltet. Der Data Factory-Dienst erstellt diese Umgebung automatisch, bevor ein Auftrag zur Verarbeitung von Daten übermittelt wird. Sobald der Auftrag abgeschlossen wurde, wird die Umgebung entfernt. Sie können differenzierte Einstellungen für die bedarfsgesteuerte Computeumgebung zur Auftragsausführung, Clusterverwaltung sowie für Bootstrappingaktionen konfigurieren und steuern. 
- **Bring Your Own:** In diesem Fall können Sie Ihre eigene Compute-Umgebung (z. B. HDInsight-Cluster) als verknüpften Dienst in Data Factory registrieren. Die Compute-Umgebung wird von Ihnen verwaltet und von Data Factory zum Ausführen von Aktivitäten verwendet. 

Unter dem Artikel [Verknüpfte Computedienste](compute-linked-services.md) finden Sie Informationen zu Computediensten, die von Data Factory unterstützt werden. 

## <a name="next-steps"></a>Nächste Schritte
Ein Beispiel für die Verwendung einer Transformationsaktivität finden Sie im folgenden Tutorial: [Transformieren von Daten mit Spark](tutorial-transform-data-spark-powershell.md)
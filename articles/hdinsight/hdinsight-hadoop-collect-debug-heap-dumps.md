---
title: Debuggen und Analysieren von Apache Hadoop-Diensten mit Heapdumps – Azure
description: Lassen Sie Heapdumps für Apache Hadoop-Dienste automatisch sammeln und zum Debuggen und Analysieren in das Azure Blob Storage-Konto einfügen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 698806bdedd9994f2c9de53118cb42c9df1c36cd
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724401"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Sammeln von Heapdumps in Blob Storage zum Debuggen und Analysieren von Apache Hadoop-Diensten
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps enthalten eine Momentaufnahme des Speichers der Anwendung, einschließlich der Werte von Variablen zum Zeitpunkt der Dumperstellung. Daher sind sie zum Diagnostizieren von Problemen nützlich, die bei der Ausführung auftreten. Heapdumps können für [Apache Hadoop](https://hadoop.apache.org/)-Dienste automatisch erfasst und im Azure Blob Storage-Konto eines Benutzers unter „HDInsightHeapDumps/“ abgelegt werden.

Die Sammlung von Heapdumps für verschiedene Dienste muss für Dienste in einzelnen Clustern aktiviert werden. Standardmäßig ist dieses Feature für einen Cluster deaktiviert. Diese Heapdumps können groß sein, sodass es ratsam ist, das Blobspeicherkonto zu überwachen, in dem sie gespeichert werden, nachdem die Sammlung aktiviert wurde.

> [!IMPORTANT]  
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement). Die Informationen in diesem Artikel gelten nur für Windows-basierte HDInsight-Cluster. Informationen zu Linux-basiertem HDInsight finden Sie unter [Aktivieren von Heapdumps für Apache Hadoop-Dienste in Linux-basierten HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md).


## <a name="eligible-services-for-heap-dumps"></a>Verfügbare Dienste für Heapdumps
Sie können Heapdumps für die folgenden Dienste aktivieren:

* **Apache hcatalog**: tempelton
* **Apache hive**: hiveserver2, metastore, derbyserver
* **mapreduce** : jobhistoryserver
* **Apache yarn**: resourcemanager, nodemanager, timelineserver
* **Apache hdfs**: datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Die Konfigurationselemente, mit denen Heapdumps aktiviert werden
Um Heapdumps für einen Dienst zu aktivieren, müssen Sie die entsprechenden Konfigurationselemente im Abschnitt des jeweiligen Diensts festlegen, der von **service_name** angegeben wird.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Der Wert von **service_name** kann einer der hier angegebenen Dienste sein: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode oder namenode.

## <a name="enable-using-azure-powershell"></a>Aktivieren mithilfe von Azure PowerShell
Um Heapdumps mit Azure PowerShell z.B. für den Dienst jobhistoryserver zu aktivieren, können Sie das folgende Skript verwenden:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Aktivieren mithilfe von .NET SDK
Um Heapdumps mit dem .NET SDK für Azure HDInsight z.B. für jobhistoryserver zu aktivieren, können Sie folgenden Code verwenden:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

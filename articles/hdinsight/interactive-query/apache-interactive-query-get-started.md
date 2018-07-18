---
title: Verwenden von Interactive Query mit Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Interactive Query (Hive LLAP) mit HDInsight verwenden.
keywords: ''
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 9450f60b88c63e2cd818130521d67daf435f0b17
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31407104"
---
# <a name="use-interactive-query-with-hdinsight"></a>Verwenden von Interactive Query mit HDInsight
Interactive Query (auch Hive LLAP oder [Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP) genannt) ist ein Azure HDInsight-[Clustertyp](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactive Query unterstützt speicherinternes Zwischenspeichern, wodurch Hive-Abfragen schneller und viel interaktiver werden.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Ein Interactive Query-Cluster unterscheidet sich von einem Hadoop-Cluster. Er enthält nur den Hive-Dienst. 

> [!NOTE]
> Sie können im Interactive Query-Cluster nur über die Ambari-Hive-Ansicht, Beeline und den Microsoft Hive Open Database Connectivity-Treiber (ODBC Hive) auf den Hive-Dienst zugreifen. Sie können nicht über die Hive-Konsole, Templeton, das Azure-Befehlszeilentool (Azure CLI) oder Azure PowerShell darauf zugreifen. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Erstellen eines Interactive Query-Clusters
Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Wählen Sie den Clustertyp „Interactive Query“ aus.

## <a name="execute-hive-queries-from-interactive-query"></a>Ausführen von Hive-Abfragen über Interactive Query
Um Hive-Abfragen auszuführen, können Sie folgende Optionen nutzen:

* Verwenden von Power BI

    Siehe [Visualisieren von Interactive Query-Hive-Daten mit Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md). Siehe [Visualisieren von Big Data mit Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Verwenden von Zeppelin

    Informationen finden Sie unter [Use Zeppelin to run Hive queries in Azure HDInsight (Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight)](../hdinsight-connect-hive-zeppelin.md).

* Verwenden von Visual Studio

    Informationen finden Sie unter [Connect to Azure HDInsight and run Hive queries using Data Lake Tools for Visual Studio (Verbinden mit Azure HDInsight und Ausführen von Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio)](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Verwenden von Visual Studio Code

    Informationen finden Sie unter [Use Visual Studio Code for Hive, LLAP or pySpark (Verwenden von Visual Studio für Hive, LLAP oder pySpark)](../hdinsight-for-vscode.md).
* Ausführen von Hive per Ambari-Hive-Ansicht.
  
    Informationen finden Sie unter [Use Hive View with Hadoop in Azure HDInsight (Verwenden der Hive-Ansicht mit Hadoop in Azure HDInsight)](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Ausführen von Hive per Beeline.
  
    Informationen finden Sie unter [Use Hive with Hadoop in HDInsight with Beeline (Verwenden von Hive mit Hadoop in HDInsight über Beeline)](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Sie können Beeline entweder über den Hauptknoten oder von einem leeren Edgeknoten aus verwenden. Am besten verwenden Sie Beeline von einem leeren Edgeknoten aus. Informationen zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten finden Sie unter [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight](../hdinsight-apps-use-edge-node.md).
* Ausführen von Hive per Hive ODBC.
  
    Informationen finden Sie unter [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

So suchen Sie die Verbindungszeichenfolge für die Java Database Connectivity (JDBC):

1. Melden Sie sich mit folgender URL bei Ambari an: https://\<Clustername\>.AzureHDInsight.net.
2. Wählen Sie im Menü auf der linken Seite die Option **Hive** aus.
3. Wählen Sie das Symbol für die Zwischenablage zum Kopieren der URL aus:
   
   ![HDInsight Hadoop Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Interactive Query-Cluster in HDInsight erstellen](../hdinsight-hadoop-provision-linux-clusters.md).
* Erfahren Sie, wie Sie [Big Data mit Power BI in Azure HDInsight visualisieren](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Erfahren Sie, wie Sie [Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight verwenden](../hdinsight-connect-hive-zeppelin.md).
* Erfahren Sie, wie Sie [Hive-Abfragen mit Data Lake-Tools für Visual Studio ausführen](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Erfahren Sie, wie Sie [HDInsight-Tools für Visual Studio Code verwenden](../hdinsight-for-vscode.md).
* Erfahren Sie, wie Sie [die Hive-Ansicht mit Hadoop in HDInsight verwenden](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Erfahren Sie, wie Sie den [Beeline-Client mit Apache Hive verwenden](../hadoop/apache-hadoop-use-hive-beeline.md).
* Erfahren Sie, wie Sie [Excel mithilfe des Microsoft Hive ODBC-Treibers mit Hadoop in Azure HDInsight verbinden](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).


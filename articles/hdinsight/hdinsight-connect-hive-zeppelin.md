---
title: Verwenden von Apache Zeppelin zum Ausführen von Apache Hive-Abfragen in Azure HDInsight
description: Erfahren Sie, wie Sie Apache Zeppelin zum Ausführen von Apache Hive-Abfragen verwenden.
keywords: hdinsight, hadoop, hive, interactive query, LLAP
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 417e9f8ae78889374983bf77900ee00fa7fc6338
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718937"
---
# <a name="use-apache-zeppelin-to-run-apache-hive-queries-in-azure-hdinsight"></a>Verwenden von Apache Zeppelin zum Ausführen von Apache Hive-Abfragen in Azure HDInsight 

Interactive Query-Cluster von HDInsight umfassen [Apache Zeppelin](https://zeppelin.apache.org/)-Notebooks, mit denen Sie interaktive Hive-Abfragen ausführen können. In diesem Artikel erfahren Sie, wie Sie Apache Zeppelin zum Ausführen von [Apache Hive](https://hive.apache.org/)-Abfragen in Azure HDInsight verwenden. 

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diesen Artikel durchgehen können, benötigen Sie Folgendes:

* **Interactive Query-Cluster von HDInsight**. Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Cluster erstellen](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).  Wählen Sie den Typ „Interactive Query“ aus. 

## <a name="create-an-apache-zeppelin-note"></a>Erstellen einer Apache Zeppelin-Notiz

1. Navigieren Sie zu folgender URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.

2. Geben Sie Ihren Hadoop-Benutzernamen und Ihr Kennwort ein. Auf der Zeppelin-Seite können Sie entweder eine neue Notiz erstellen oder vorhandene Notizen öffnen. HiveSample enthält einige Beispiel-Hive-Abfragen.  

    ![HDInsight Interactive Query Zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Klicken Sie auf **Create new Note** (Neue Notiz erstellen).
4. Geben Sie folgende Werte ein bzw. wählen diese aus:

    - Notizname: Geben Sie einen Namen für die Notiz ein.
    - Standardinterpreter: Wählen Sie **JDBC** aus.

5. Klicken Sie auf **Create Note** (Notiz erstellen).
6. Führen Sie die folgende Hive-Abfrage aus:

        %jdbc(hive)
        show tables

    ![HDInsight Interactive Query Zeppelin führt Abfrage aus](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    Die Anweisung **%jdbc(hive)** in der ersten Zeile gibt dem Notebook an, dass es den Hive-JDBC-Interpreter verwenden soll.

    Die Abfrage soll eine Hive-Tabelle namens *hivesampletable* zurückgeben.

    Es folgen zwei weitere Hive-Abfragen, die Sie auf „hivesampletable“ ausführen können. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Verglichen mit früheren Hive-Abfragen werden die Abfrageergebnisse viel schneller zurückgegeben.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von [Microsoft Power BI](https://powerbi.microsoft.com/) visualisieren.  Weitere Informationen finden Sie in den folgenden Artikeln:

* [Visualisieren von Apache Hive-Daten mit Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualisieren von Interactive Query-Apache Hive-Daten mit Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinden von Excel mit Apache Hadoop mithilfe von Power Query](hadoop/apache-hadoop-connect-excel-power-query.md)
* [Verbinden mit Azure HDInsight und Ausführen von Apache Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md)
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](hdinsight-for-vscode.md)
* [Upload data to HDInsight (Hochladen von Daten in HDInsight)](./hdinsight-upload-data.md).

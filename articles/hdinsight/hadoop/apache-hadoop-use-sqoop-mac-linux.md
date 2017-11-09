---
title: "Apache Sqoop mit Hadoop – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie mehr darüber, wie Sie mithilfe von Apache Sqoop Daten zwischen Hadoop unter HDInsight und einer Azure SQL-Datenbank importieren und exportieren."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: Hadoop Sqoop, Sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: larryfr
ms.openlocfilehash: 250fb1dfed5cdab5308c2d91744e0cf051c32ccc
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Importieren und Exportieren von Daten zwischen Hadoop unter HDInsight und einer SQL-Datenbank mithilfe von Apache Sqoop

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie Sie Apache Sqoop zum Importieren und Exportieren von Daten zwischen einem Hadoop-Cluster unter Azure HDInsight und einer Azure SQL- oder Microsoft SQL Server-Datenbank verwenden. Die Schritte in diesem Artikel verwenden den Befehl `sqoop` direkt vom Hauptknoten des Hadoop-Clusters aus. Sie können SSH verwenden, um die Verbindung zum Hauptknoten herzustellen und die Befehle in diesem Artikel auszuführen.

> [!IMPORTANT]
> Die Schritte in diesem Dokument funktionieren nur mit einem HDInsight-Cluster unter Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!WARNING]
> Die einzelnen Schritte, die in diesem Dokument beschrieben werden, setzen voraus, dass Sie bereits eine Azure SQL-Datenbank mit dem Namen `sqooptest` erstellt haben.
>
> Ihnen werden T-SQL-Anweisungen, die zum Erstellen und Abfragen einer Tabelle in SQL-Datenbank verwendet werden, zur Verfügung gestellt. Es gibt viele Clients, für die Sie die Anweisungen mit SQL-Datenbank verwenden können. Folgende Clients werden empfohlen:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * Das [SQLCMD](https://docs.microsoft.com/sql/tools/sqlcmd-utility)-Hilfsprogramm.

## <a name="create-the-table-in-sql-database"></a>Erstellen der Tabelle in einer SQL-Datenbank

> [!IMPORTANT]
> Wenn Sie den HDInsight-Cluster und die SQL-Datenbank verwenden, die Sie unter [Erstellen des Clusters und der SQL-Datenbank](hdinsight-use-sqoop.md) erstellt haben, überspringen Sie die Schritte in diesem Abschnitt. Die Datenbank und die Tabelle wurden im Rahmen der Schritte im Artikel [Erstellen des Clusters und der SQL-Datenbank](hdinsight-use-sqoop.md) erstellt.

Verwenden Sie einen SQL-Client, um eine Verbindung mit der `sqooptest`-Datenbank in Ihrer SQL-Datenbank herzustellen. Verwenden Sie anschließend folgende T-SQL-Anweisung, um eine Tabelle mit dem Namen `mobiledata` zu erstellen:

```sql
CREATE TABLE [dbo].[mobiledata](
[clientid] [nvarchar](50),
[querytime] [nvarchar](50),
[market] [nvarchar](50),
[deviceplatform] [nvarchar](50),
[devicemake] [nvarchar](50),
[devicemodel] [nvarchar](50),
[state] [nvarchar](50),
[country] [nvarchar](50),
[querydwelltime] [float],
[sessionid] [bigint],
[sessionpagevieworder] [bigint])
GO
CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
GO
```

## <a name="sqoop-export"></a>Sqoop-Export

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Der folgende Befehl stellt z.B. eine Verbindung zum primären Hauptknoten eines Clusters mit dem Namen `mycluster` her:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob Sqoop Ihre SQL-Datenbank erreichen kann:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Geben Sie nach Aufforderung das Kennwort für die SQL-Datenbank-Anmeldung ein.

    Dieser Befehl gibt eine Liste mit Datenbanken zurück, in der auch die zuvor verwendete **sqooptest**-Datenbank enthalten ist.

3. Verwenden Sie den folgenden Befehl, um Daten aus der Hive-Tabelle **hivesampletable** zu exportieren und sie anschließend in die Tabelle **mobiledata** in SQL-Datenbank zu importieren:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Verwenden Sie die folgende Abfrage aus Ihrem SQL-Client, über die Ihnen die exportierten Daten angezeigt werden, um zu überprüfen, ob die Daten exportiert wurden:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata;"
    ```

    Über diesen Befehl werden 50 Zeilen aufgelistet, die in die Tabelle importiert wurden.

## <a name="sqoop-import"></a>Sqoop-Import

1. Importieren Sie mit dem folgenden Befehl Daten aus der Tabelle **mobiledata** in der SQL-Datenbank in das Verzeichnis **wasb:///tutorials/usesqoop/importeddata** in HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    In den Daten sind die Felder durch ein Tabstoppzeichen getrennt und die Zeilen durch ein Zeilenumbruchzeichen abgeschlossen.

    > [!IMPORTANT]
    > Der Pfad `wasb:///` funktioniert mit Clustern, die Azure Storage als Standardclusterspeicher verwenden. Verwenden Sie für Cluster, die Azure Data Lake Store nutzen, stattdessen `adl:///`.

2. Sobald der Import abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Daten in dem neuen Verzeichnis:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Verwenden von SQL Server

Für den Import und Export von SQL Server-Daten können Sie ebenfalls Sqoop verwenden. Die Unterschiede zwischen der Verwendung von SQL-Datenbank und SQL Server sind:

* Sowohl HDInsight als auch SQL Server müssen sich im selben virtuellen Azure-Netzwerk befinden.

    Ein Beispiel finden Sie im Dokument [Connect HDInsight to your on-premises network](./../connect-on-premises-network.md) (Herstellen einer Verbindung von HDInsight mit Ihrem lokalen Netzwerk).

    Weitere Informationen zur Verwendung von HDInsight mit einem virtuellen Azure-Netzwerk finden Sie im Dokument [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md). Weitere Informationen zu Azure Virtual Network erhalten Sie im Dokument [Übersicht über virtuelle Netzwerke](../../virtual-network/virtual-networks-overview.md).

* SQL Server muss so konfiguriert sein, dass SQL-Authentifizierung erlaubt ist. Weitere Informationen finden Sie im Artikel [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/ms144284.aspx).

* Sie müssen SQL Server möglicherweise für Remoteverbindungen konfigurieren. Weitere Informationen finden Sie im Artikel [How to Troubleshoot Connecting to the SQL Server Database Engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) (Beheben von Fehlern bei der Verbindung zum SQL Server-Datenbankmodul).

* Verwenden Sie die folgenden Transact-SQL-Anweisungen zum Erstellen der Tabelle **mobiledata**:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* Beim Herstellen einer Verbindung mit SQL Server von HDInsight müssen Sie möglicherweise die IP-Adresse der SQL Server-Instanz verwenden. Beispiel:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P <adminPassword> -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Einschränkungen

* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector keine Masseneinfügung.

* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="next-steps"></a>Nächste Schritte

Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

* [Verwenden von Oozie mit HDInsight](../hdinsight-use-oozie.md): Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
* [Analysieren von Daten zu Flugverspätungen mit HDInsight](../hdinsight-analyze-flight-delay-data.md): Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
* [Hochladen von Daten in HDInsight](../hdinsight-upload-data.md): Andere Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

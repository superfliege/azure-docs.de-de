---
title: Apache Sqoop mit Apache Hadoop – Azure HDInsight
description: Erfahren Sie mehr darüber, wie Sie mithilfe von Apache Sqoop Daten zwischen Apache Hadoop unter HDInsight und einer Azure SQL-Datenbank importieren und exportieren.
keywords: Hadoop Sqoop, Sqoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 0f96ee3a24a811d7e3ab7e65ba4ff14050541638
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443377"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Importieren und Exportieren von Daten zwischen Apache Hadoop unter HDInsight und einer SQL-Datenbank mithilfe von Apache Sqoop

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie Sie Apache Sqoop zum Importieren und Exportieren von Daten zwischen einem Apache Hadoop-Cluster unter Azure HDInsight und einer Azure SQL- oder Microsoft SQL Server-Datenbank verwenden. Die Schritte in diesem Artikel verwenden den Befehl `sqoop` direkt vom Hauptknoten des Hadoop-Clusters aus. Sie können SSH verwenden, um die Verbindung zum Hauptknoten herzustellen und die Befehle in diesem Artikel auszuführen.

> [!IMPORTANT]  
> Die Schritte in diesem Dokument funktionieren nur mit einem HDInsight-Cluster unter Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!WARNING]  
> Die einzelnen Schritte, die in diesem Dokument beschrieben werden, setzen voraus, dass Sie bereits eine Azure SQL-Datenbank mit dem Namen `sqooptest` erstellt haben.
>
> Ihnen werden T-SQL-Anweisungen, die zum Erstellen und Abfragen einer Tabelle in SQL-Datenbank verwendet werden, zur Verfügung gestellt. Es gibt viele Clients, für die Sie die Anweisungen mit SQL-Datenbank verwenden können. Folgende Clients werden empfohlen:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * Das [SQLCMD](https://docs.microsoft.com/sql/tools/sqlcmd-utility)-Hilfsprogramm

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
    ssh mycluster-ssh.azurehdinsight.net
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
    SELECT * FROM mobiledata;
    ```

    Über diesen Befehl werden 50 Zeilen aufgelistet, die in die Tabelle importiert wurden.

## <a name="sqoop-import"></a>Sqoop-Import

1. Importieren Sie mit dem folgenden Befehl Daten aus der Tabelle **mobiledata** in der SQL-Datenbank in das Verzeichnis **wasb:///tutorials/usesqoop/importeddata** in HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    In den Daten sind die Felder durch ein Tabstoppzeichen getrennt und die Zeilen durch ein Zeilenumbruchzeichen abgeschlossen.

    > [!IMPORTANT]  
    > Der Pfad `wasb:///` funktioniert mit Clustern, die Azure Storage als Standardclusterspeicher verwenden. Verwenden Sie für Cluster, die Azure Data Lake Storage Gen2 nutzen, stattdessen `abfs:///`. Verwenden Sie für Cluster, die Azure Data Lake Storage Gen1 nutzen, stattdessen `adl:///`.

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

* Sie müssen SQL Server möglicherweise für Remoteverbindungen konfigurieren. Weitere Informationen finden Sie im Artikel [Beheben von Fehlern bei der Verbindung mit der SQL Server-Datenbank-Engine](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx).

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
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Einschränkungen

* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector keine Masseneinfügung.

* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="next-steps"></a>Nächste Schritte

Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

* [Verwenden von Apache Oozie mit HDInsight](../hdinsight-use-oozie-linux-mac.md): Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
* [Analysieren von Daten zu Flugverspätungen mit HDInsight](../hdinsight-analyze-flight-delay-data-linux.md): Verwenden von Apache Hive zum Analysieren von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in Azure SQL-Datenbank.
* [Hochladen von Daten in HDInsight](../hdinsight-upload-data.md): Andere Methoden zum Hochladen von Daten in HDInsight/Azure Blob Storage.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

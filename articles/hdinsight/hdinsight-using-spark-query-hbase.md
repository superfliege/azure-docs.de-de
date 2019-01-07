---
title: 'Verwenden von Spark zum Lesen und Schreiben von HBase-Daten: Azure HDInsight'
description: Verwenden Sie den Spark HBase-Connector, um Daten aus einem Spark-Cluster zu lesen und in einen HBase-Cluster zu schreiben.
services: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.openlocfilehash: f76dfc8499d294c2c682c4d9c4cf2cd2f858432f
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408854"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Verwenden von Apache Spark zum Lesen und Schreiben von Apache HBase-Daten

Apache HBase wird üblicherweise über die Low-Level-API (scan-, get- und put-Abfragen) oder mit einer SQL-Syntax unter Verwendung von Apache Phoenix abgefragt. Apache stellt auch den Apache Spark HBase-Connector bereit, der eine praktische und leistungsfähige Alternative zum Abfragen und Ändern von Daten darstellt, die von HBase gespeichert wurden.

## <a name="prerequisites"></a>Voraussetzungen

* Zwei separate installierte HDInsight-Cluster: ein HBase-Cluster und ein Spark-Cluster mit Spark 2.1 (HDInsight 3.6).
* Der Spark-Cluster muss mit minimaler Latenz direkt mit dem HBase-Cluster kommunizieren, daher besteht die empfohlene Konfiguration darin, beide Cluster im gleichen virtuellen Netzwerk bereitzustellen. Weitere Informationen finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md).
* SSH-Zugriff auf jeden Cluster.
* Zugriff auf den Standardspeicher jedes Clusters.

## <a name="overall-process"></a>Übersicht über den Prozess

Der allgemeine Prozess zum Aktivieren Ihres Spark-Clusters für die Abfrage Ihres HDInsight-Clusters sieht wie folgt aus:

1. Bereiten Sie einige Beispieldaten in HBase vor.
2. Rufen Sie die Datei „hbase-site.xml“ aus dem Konfigurationsordner Ihres HBase-Clusters ab (/etc/hbase/conf).
3. Speichern Sie eine Kopie von „hbase-site.xml“ in Ihrem Spark 2-Konfigurationsordner (/etc/spark2/conf).
4. Führen Sie `spark-shell` aus, und verweisen Sie in der Option `packages` mit den Maven-Koordinaten auf den Spark HBase-Connector.
5. Definieren Sie einen Katalog, der das Spark-Schema zu HBase zuordnet.
6. Interagieren Sie entweder über die RDD- oder die Dataframe-APIs mit den HBase-Daten.

## <a name="prepare-sample-data-in-apache-hbase"></a>Vorbereiten von Beispieldaten in Apache HBase

In diesem Schritt erstellen Sie einfache Tabelle in Apache HBase und füllen sie auf. Diese Tabelle können Sie dann mit Spark abfragen.

1. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres HBase-Clusters her. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
2. Führen Sie die HBase-Shell aus:

        hbase shell

3. Erstellen Sie eine `Contacts`-Tabelle mit den Spaltenfamilien `Personal` und `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Laden Sie einige Beispieldatenzeilen:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Abrufen von „hbase-site.xml“ aus Ihrem HBase-Cluster

1. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres HBase-Clusters her.
2. Kopieren Sie die Datei „hbase-site.xml“ aus dem lokalen Speicher in das Stammverzeichnis des Standardspeichers Ihres HBase-Clusters:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem HBase-Cluster.
4. Wählen Sie Speicherkonten aus. 

    ![Speicherkonten](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Wählen Sie das Speicherkonto aus der Liste aus, das in der Spalte „Standard“ ein Häkchen aufweist.

    ![Standardspeicherkonto](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. Wählen Sie im Speicherkontobereich die Kachel „Blobs“ aus.

    ![Kachel „Blobs“](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Wählen Sie in der Liste der Container den Container aus, der von Ihrem HBase-Cluster verwendet wird.
8. Wählen Sie in der Liste `hbase-site.xml` aus.

    ![hbase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. Wählen Sie im Bereich mit den Blobeigenschaften die Option „Herunterladen“ aus, und speichern Sie `hbase-site.xml` in einem Speicherort auf Ihrem lokalen Computer.

    ![Download](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Ablegen von „hbase-site.xml“ in Ihrem Spark-Cluster

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Spark-Cluster.
2. Wählen Sie Speicherkonten aus.

    ![Speicherkonten](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Wählen Sie das Speicherkonto aus der Liste aus, das in der Spalte „Standard“ ein Häkchen aufweist.

    ![Standardspeicherkonto](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. Wählen Sie im Speicherkontobereich die Kachel „Blobs“ aus.

    ![Kachel „Blobs“](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Wählen Sie in der Liste der Container den Container aus, der von Ihrem Spark-Cluster verwendet wird.
6. Wählen Sie „Hochladen“ aus.

    ![Hochladen](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Wählen Sie die `hbase-site.xml`-Datei aus, die Sie zuvor auf Ihren lokalen Computer heruntergeladen haben.

    ![Hochladen von „hbase-site.xml“](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Wählen Sie „Hochladen“ aus.
9. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres Spark-Clusters her.
10. Kopieren Sie `hbase-site.xml` aus dem Standardspeicher Ihres Spark-Clusters in den Spark 2-Konfigurationsordner im lokalen Speicher des Clusters:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Ausführen der Spark-Shell und Verweisen auf den Spark HBase-Connector

1. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres Spark-Clusters her.
2. Starten Sie die Spark-Shell, und geben Sie das Spark HBase-Connector-Paket an:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11 --repositories http://repo.hortonworks.com/content/groups/public/

3. Lassen Sie diese Instanz der Spark-Shell geöffnet, und fahren Sie mit dem nächsten Schritt fort.

## <a name="define-a-catalog-and-query"></a>Definieren von Katalog und Abfrage

In diesem Schritt definieren Sie ein Katalogobjekt, das das Apache Spark-Schema Apache HBase zuordnet. 

1. Führen Sie in der offenen Spark-Shell-Instanz folgende `import`-Anweisungen aus:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. Definieren Sie einen Katalog für die Tabelle mit Kontakten, die Sie in HBase erstellt haben:
    1. Definieren Sie ein Katalogschema für die HBase-Tabelle namens `Contacts`.
    2. Identifizieren Sie das rowkey-Element als `key`, und ordnen Sie die in Spark verwendeten Spaltennamen der Spaltenfamilie, dem Spaltennamen und dem Spaltentyp zu, die in HBase verwendet werden.
    3. Das rowkey-Element muss im Detail auch als benannte Spalte (`rowkey`) definiert werden, die über eine spezifische `cf`-Spaltenfamilie `rowkey` verfügt.

            def catalog = s"""{
                |"table":{"namespace":"default", "name":"Contacts"},
                |"rowkey":"key",
                |"columns":{
                |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
                |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
                |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
                |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
                |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
                |}
            |}""".stripMargin

3. Definieren Sie eine Methode, die einen Datenrahmen um Ihre `Contacts`-Tabelle in HBase bereitstellt:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. Erstellen Sie eine Instanz des Datenrahmens:

        val df = withCatalog(catalog)

5. Fragen Sie den Datenrahmen ab:

        df.show()

6. Es sollten zwei Zeilen mit Daten angezeigt werden:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Registrieren Sie eine temporäre Tabelle, sodass Sie die HBase-Tabelle mithilfe von Spark SQL abfragen können:

        df.registerTempTable("contacts")

8. Erstellen Sie eine SQL-Abfrage für die `contacts`-Tabelle:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Die Ergebnisse sollten wie folgt aussehen:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Einfügen neuer Daten

1. Um einen neuen Kontaktdatensatz einzufügen, definieren Sie eine `ContactRecord`-Klasse:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Erstellen Sie eine Instanz von `ContactRecord`, und fügen Sie sie in ein Array ein:

        val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

        var newData = new Array[ContactRecord](1)
        newData(0) = newContact

3. Speichern Sie das Array mit den neuen Daten in HBase:

        sc.parallelize(newData).toDF.write
        .options(Map(HBaseTableCatalog.tableCatalog -> catalog))
        .format("org.apache.spark.sql.execution.datasources.hbase").save()

4. Untersuchen Sie die Ergebnisse:
    
        df.show()

5. Es sollte in etwa folgende Ausgabe angezeigt werden:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Nächste Schritte

* [Apache Spark HBase-Connector](https://github.com/hortonworks-spark/shc)

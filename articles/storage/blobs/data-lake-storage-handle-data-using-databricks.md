---
title: 'Tutorial: Ausführen von ELT-Vorgängen (Extrahieren, Laden, Transformieren) mithilfe von Azure Databricks'
description: In diesem Tutorial erfahren Sie, wie Sie Daten aus Azure Data Lake Storage Gen2 (Vorschauversion) in Azure Databricks extrahieren, transformieren und anschließend in Azure SQL Data Warehouse laden.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 12/06/2018
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 6b2812e31174c4e5d61ae9941563e39357de9522
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107088"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks

In diesem Tutorial führen Sie mithilfe von Azure Databricks einen ETL-Vorgang (Extrahieren, Transformieren und Laden) für Daten aus. Sie verschieben Daten aus einem Azure Storage-Konto mit aktiviertem Azure Data Lake Storage Gen2 in Azure SQL Data Warehouse.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs
> * Erstellen eines Spark-Clusters in Azure Databricks
> * Erstellen eines Azure Data Lake Storage Gen2-fähigen Kontos
> * Hochladen von Daten in Azure Data Lake Storage Gen2
> * Erstellen eines Notebooks in Azure Databricks
> * Extrahieren von Daten aus Data Lake Storage Gen2
> * Transformieren von Daten in Azure Databricks
> * Laden von Daten in Azure SQL Data Warehouse

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Erstellen Sie eine Azure SQL Data Warehouse-Instanz, erstellen Sie eine Firewallregel auf Serverebene, und stellen Sie als Serveradministrator eine Verbindung mit dem Server her. Führen Sie die Schritte des Artikels [Schnellstart: Erstellen einer Azure SQL Data Warehouse-Instanz](../../sql-data-warehouse/create-data-warehouse-portal.md) aus.
* Erstellen Sie einen Datenbank-Hauptschlüssel für die Azure SQL Data Warehouse-Instanz. Führen Sie dazu die unter [Erstellen eines Datenbank-Hauptschlüssels](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) angegebenen Schritte aus.
* [Erstellen Sie ein Azure Data Lake Storage Gen2-Konto.](data-lake-storage-quickstart-create-account.md)
* Laden Sie **small_radio_json.json** aus dem Repository [U-SQL Examples and Issue Tracking](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) (U-SQL-Beispiele und Problemverfolgung) herunter, und notieren Sie sich den Pfad, unter dem Sie die Datei speichern.
* Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-the-workspace"></a>Erstellen des Arbeitsbereichs

In diesem Abschnitt erstellen Sie einen Azure Databricks-Arbeitsbereich über das Azure-Portal.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Analysen** > **Azure Databricks**.

    ![Databricks im Azure-Portal](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks im Azure-Portal")

1. Geben Sie unter **Azure Databricks-Dienst** die folgenden Werte an, um einen Databricks-Arbeitsbereich zu erstellen:

    |Eigenschaft  |Beschreibung  |
    |---------|---------|
    |**Arbeitsbereichsname**     | Geben Sie einen Namen für Ihren Databricks-Arbeitsbereich an.        |
    |**Abonnement**     | Wählen Sie in der Dropdownliste Ihr Azure-Abonnement aus.        |
    |**Ressourcengruppe**     | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). |
    |**Location**     | Wählen Sie **USA, Westen 2** aus.        |
    |**Tarif**     |  Wählen Sie **Standard** aus.     |

    ![Erstellen eines Azure Databricks-Arbeitsbereichs](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Erstellen eines Azure Databricks-Arbeitsbereichs")

1. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie anschließend auf **Erstellen**.

1. Die Kontoerstellung dauert einige Minuten. Während der Erstellung des Kontos wird im Portal auf der rechten Seite die Kachel **Bereitstellung für "Azure Databricks" wird gesendet** angezeigt. Den Status des Vorgangs können Sie anhand der Statusanzeige im oberen Bereich verfolgen.

    ![Kachel zur Bereitstellung von Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Kachel zur Bereitstellung von Databricks")

## <a name="create-the-spark-cluster"></a>Erstellen des Spark-Clusters

Für die Vorgänge in diesem Tutorial wird ein Spark-Cluster benötigt. Führen Sie die folgenden Schritte aus, um den Spark-Cluster zu erstellen:

1. Navigieren Sie im Azure-Portal zum erstellten Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

1. Sie werden zum Azure Databricks-Portal weitergeleitet. Klicken Sie im Portal auf **Cluster**.

    ![Databricks in Azure](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks in Azure")

1. Geben Sie auf der Seite **Neuer Cluster** die erforderlichen Werte an, um einen Cluster zu erstellen.

    ![Erstellen eines Databricks-Spark-Clusters in Azure](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Erstellen eines Databricks-Spark-Clusters in Azure")

1. Geben Sie Werte für die folgenden Felder an, und übernehmen Sie bei den anderen Feldern die Standardwerte:

    * Geben Sie einen Namen für den Cluster ein.
    * Erstellen Sie für diesen Artikel einen Cluster mit der Runtime **5.1**.
    * Aktivieren Sie das Kontrollkästchen **Terminate after \_\_ minutes of inactivity** (Nach \_\_ Minuten Inaktivität beenden). Falls der Cluster nicht verwendet wird, geben Sie an, nach wie vielen Minuten er beendet werden soll.

1. Klicken Sie auf **Cluster erstellen**.

Wenn der Cluster ausgeführt wird, können Sie Notebooks an den Cluster anfügen und Spark-Aufträge ausführen.

## <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Um Daten in Ihrem Data Lake Storage Gen2-Speicherkonto speichern zu können, müssen Sie ein Dateisystem erstellen.

Erstellen Sie zunächst ein Notebook in Ihrem Azure Databricks-Arbeitsbereich, und führen Sie anschließend Codeausschnitte aus, um das Dateisystem in Ihrem Speicherkonto zu erstellen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum erstellten Azure Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

1. Wählen Sie im linken Bereich die Option **Arbeitsbereich** aus. Wählen Sie in der Dropdownliste **Arbeitsbereich** die Option **Erstellen** > **Notebook** aus.

    ![Erstellen eines Notebooks in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Erstellen eines Notebooks in Databricks")

1. Geben Sie im Dialogfeld **Notizbuch erstellen** einen Namen für das Notebook ein. Wählen Sie **Scala** als Sprache und anschließend den zuvor erstellten Spark-Cluster aus.

    ![Angeben von Details für ein Notebook in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "Angeben von Details für ein Notebook in Databricks")

    Klicken Sie auf **Erstellen**.

1. Geben Sie in die erste Zelle des Notebooks den folgenden Code ein, und führen Sie ihn aus. Ersetzen Sie die in Klammern angegebenen Platzhalter durch Ihre eigenen Werte:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
     
    dbutils.fs.mount(
        source = "abfss://<file-system-name>@<account-name>.dfs.core.windows.net/[<directory-name>]",
        mount_point = "/mnt/<mount-name>",
        extra_configs = configs)
    ```

1. Drücken Sie UMSCHALT+EINGABE, um den Code auszuführen.

Daraufhin wird das Dateisystem für das Speicherkonto erstellt.

## <a name="upload-the-sample-data"></a>Hochladen der Beispieldaten

Im nächsten Schritt wird eine Beispieldatendatei in das Speicherkonto hochgeladen, um sie später in Azure Databricks transformieren zu können.

Laden Sie die heruntergeladenen Beispieldaten in Ihr Speicherkonto hoch. Mit welcher Methode Sie die Daten in Ihr Speicherkonto hochladen, hängt davon ab, ob der hierarchische Namespace aktiviert ist.

Zum Hochladen können Sie Azure Data Factory, distp oder AzCopy (Version 10) verwenden. Version 10 von AzCopy steht derzeit nur als Vorschauversion zur Verfügung. Fügen Sie den folgenden Code in ein Befehlsfenster ein, um AzCopy zu verwenden:

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
```

## <a name="extract-the-data"></a>Extrahieren der Daten

Um die Beispieldaten in Databricks verwenden zu können, müssen Sie die Daten aus Ihrem Speicherkonto extrahieren.

Kehren Sie zu Ihrem Databricks-Notebook zurück, und geben Sie den folgenden Code in eine neue Zelle ein.

Fügen Sie den folgenden Codeausschnitt in eine leere Codezelle ein. Ersetzen Sie die in Klammern angegebenen Platzhalter durch die Werte aus dem Speicherkonto, die Sie zuvor gespeichert haben.

```scala
dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
```

Drücken Sie UMSCHALT+EINGABE, um den Code auszuführen.

Nun können Sie die JSON-Beispieldatei als Datenrahmen in Azure Databricks laden. Fügen Sie den folgenden Code in eine neue Zelle ein. Ersetzen Sie die in Klammern angegebenen Platzhalter durch Ihre Werte.

```scala
val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
```

Drücken Sie UMSCHALT+EINGABE, um den Code auszuführen.

Führen Sie den folgenden Code aus, um den Inhalt des Datenrahmens anzuzeigen:

```scala
df.show()
```

Es wird ungefähr folgender Codeausschnitt angezeigt:

```bash
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
|               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
| El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
| Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
| Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
...
...
```

Damit haben Sie die Daten aus Azure Data Lake Storage Gen2 in Azure Databricks extrahiert.

## <a name="transform-the-data"></a>Transformieren der Daten

Die unformatierten Beispieldaten aus der Datei **small_radio_json.json** dienen zur Erfassung der Zuhörer eines Radiosenders und enthalten eine Reihe von Spalten. In diesem Abschnitt transformieren Sie die Daten, um nur bestimmte Spalten aus dem Dataset abzurufen.

Rufen Sie zunächst nur die Spalten **firstName**, **lastName**, **gender**, **location** und **level** aus dem bereits erstellten Datenrahmen ab.

```scala
val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
```

Daraufhin erhalten Sie eine Ausgabe wie die folgende:

```bash
+---------+----------+------+--------------------+-----+
|firstname|  lastname|gender|            location|level|
+---------+----------+------+--------------------+-----+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
+---------+----------+------+--------------------+-----+
```

Diese Daten können noch weiter transformiert werden, um die Spalte **level** in **subscription_type** umzubenennen:

```scala
val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
renamedColumnsDF.show()
```

Daraufhin erhalten Sie eine Ausgabe wie die folgende:

```bash
+---------+----------+------+--------------------+-----------------+
|firstname|  lastname|gender|            location|subscription_type|
+---------+----------+------+--------------------+-----------------+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
+---------+----------+------+--------------------+-----------------+
```

## <a name="load-the-data"></a>Laden der Daten

In diesem Abschnitt laden Sie die transformierten Daten in Azure SQL Data Warehouse hoch. Sie verwenden den Azure SQL Data Warehouse-Connector für Azure Databricks, um einen Datenrahmen direkt als Tabelle in eine SQL Data Warehouse-Instanz hochzuladen.

Der SQL Date Warehouse-Connector verwendet Azure Blob Storage als temporären Speicher, um Daten zwischen Azure Databricks und Azure SQL Data Warehouse hochzuladen. Geben Sie daher zunächst die Konfiguration für die Verbindungsherstellung mit dem Speicherkonto an. Das Konto muss im Rahmen der Vorbereitung für diesen Artikel bereits erstellt worden sein.

Geben Sie die Konfiguration für den Zugriff auf das Azure Storage-Konto über Azure Databricks an.

```scala
val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
val fileSystemName = "<FILE_SYSTEM_NAME>"
val accessKey =  "<ACCESS_KEY>"
```

Geben Sie einen temporären Ordner für die Verschiebung von Daten zwischen Azure Databricks und Azure SQL Data Warehouse an.

```scala
val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
```

Führen Sie den folgenden Codeausschnitt aus, um Azure Blob-Speicherzugriffsschlüssel in der Konfiguration zu speichern. Dadurch wird sichergestellt, dass Sie den Zugriffsschlüssel im Notebook nicht als Klartext speichern müssen.

```scala
val acntInfo = "fs.azure.account.key."+ storageURI
sc.hadoopConfiguration.set(acntInfo, accessKey)
```

Geben Sie die Werte für die Verbindungsherstellung mit der Azure SQL Data Warehouse-Instanz an. Im Rahmen der Vorbereitung muss bereits eine SQL Data Warehouse-Instanz erstellt worden sein.

```scala
//SQL Data Warehouse related settings
val dwDatabase = "<DATABASE NAME>"
val dwServer = "<DATABASE SERVER NAME>" 
val dwUser = "<USER NAME>"
val dwPass = "<PASSWORD>"
val dwJdbcPort =  "1433"
val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
```

Führen Sie den folgenden Codeausschnitt aus, um den transformierten Datenrahmen (**renamedColumnsDF**) als Tabelle in einer SQL Data Warehouse-Instanz zu laden. Dieser Codeausschnitt erstellt in der SQL-Datenbank eine Tabelle namens **SampleTable**.

```scala
spark.conf.set(
    "spark.sql.parquet.writeLegacyFormat",
    "true")
    
renamedColumnsDF.write
    .format("com.databricks.spark.sqldw")
    .option("url", sqlDwUrlSmall) 
    .option("dbtable", "SampleTable")
    .option( "forward_spark_azure_storage_credentials","True")
    .option("tempdir", tempDir)
    .mode("overwrite")
    .save()
```

Stellen Sie eine Verbindung mit der SQL-Datenbank her, und vergewissern Sie sich, dass eine Datenbank namens **SampleTable** vorhanden ist.

![Überprüfen der Beispieltabelle](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "Überprüfen der Beispieltabelle")

Führen Sie zur Überprüfung des Tabelleninhalts eine SELECT-Abfrage aus. Die Tabelle sollte die gleichen Daten enthalten wie der Datenrahmen **renamedColumnsDF**.

![Überprüfen des Inhalts der Beispieltabelle](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "Überprüfen des Inhalts der Beispieltabelle")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss des Tutorials können Sie den Cluster beenden. Wählen Sie hierzu im linken Bereich des Azure Databricks-Arbeitsbereichs die Option **Cluster** aus. Zeigen Sie zum Beenden des Clusters unter **Aktionen** auf die Auslassungspunkte (...), und wählen Sie das Symbol **Beenden** aus.

![Beenden eines Databricks-Clusters](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Beenden eines Databricks-Clusters")

Wenn Sie den Cluster nicht manuell beenden, wird er automatisch beendet, sofern Sie bei der Erstellung des Clusters das Kontrollkästchen **Terminate after \_\_ minutes of inactivity** (Nach \_\_ Minuten Inaktivität beenden) aktiviert haben. In diesem Fall wird der Cluster automatisch beendet, wenn er für den angegebenen Zeitraum inaktiv war.

## <a name="next-steps"></a>Nächste Schritte

Das nächste Tutorial beschäftigt sich mit dem Streamen von Echtzeitdaten an Azure Databricks unter Verwendung von Azure Event Hubs.

> [!div class="nextstepaction"]
>[Streamen von Daten an Azure Databricks unter Verwendung von Event Hubs](../../azure-databricks/databricks-stream-from-eventhubs.md)

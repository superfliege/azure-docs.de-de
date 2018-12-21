---
title: Ausführen von ELT-Vorgängen (Extrahieren, Laden, Transformieren) mithilfe von Azure Databricks
description: Hier erfahren Sie, wie Sie Daten aus Azure Data Lake Storage Gen2 (Vorschauversion) in Azure Databricks extrahieren, transformieren und anschließend in Azure SQL Data Warehouse laden.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 12/06/2018
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 108495c367bdb50ed4799aab929e4d26ad14ab87
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974183"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks

In diesem Tutorial führen Sie einen ETL-Vorgang (Extrahieren, Transformieren und Laden von Daten) aus, um Daten aus einem Azure Storage-Konto mit Azure Data Lake Storage Gen2 unter Verwendung von Azure Databricks in Azure SQL Data Warehouse zu verschieben.

In der folgenden Abbildung ist der Anwendungsfluss dargestellt:

![Azure Databricks mit Data Lake Storage Gen2 und SQL Data Warehouse](./media/data-lake-storage-handle-data-using-databricks/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks mit Data Lake Storage Gen2 und SQL Data Warehouse")

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs
> * Erstellen eines Spark-Clusters in Azure Databricks
> * Erstellen eines Azure Data Lake Storage Gen2-fähigen Kontos
> * Hochladen von Daten in Azure Data Lake Storage Gen2
> * Erstellen eines Notebooks in Azure Databricks
> * Extrahieren von Daten aus Data Lake Storage Gen2
> * Transformieren von Daten in Azure Databricks
> * Laden von Daten in Azure SQL Data Warehouse

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Erstellen Sie eine Azure SQL Data Warehouse-Instanz, erstellen Sie eine Firewallregel auf Serverebene, und stellen Sie als Serveradministrator eine Verbindung mit dem Server her. Befolgen Sie die Anweisungen unter [Schnellstart: Erstellen einer Azure SQL Data Warehouse-Instanz](../../sql-data-warehouse/create-data-warehouse-portal.md)
* Erstellen Sie einen Datenbank-Hauptschlüssel für die Azure SQL Data Warehouse-Instanz. Führen Sie dazu die unter [Erstellen eines Datenbank-Hauptschlüssels](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) angegebenen Schritte aus.
* [Erstellen Sie ein Azure Data Lake Storage Gen2-Konto.](data-lake-storage-quickstart-create-account.md)

## <a name="sign-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-an-azure-databricks-workspace"></a>Erstellen eines Azure Databricks-Arbeitsbereichs

In diesem Abschnitt erstellen Sie einen Azure Databricks-Arbeitsbereich über das Azure-Portal.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Analysen** > **Azure Databricks**.

    ![Databricks im Azure-Portal](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks im Azure-Portal")

2. Geben Sie unter **Azure Databricks-Dienst** die Werte für die Erstellung eines Databricks-Arbeitsbereichs an.

    ![Erstellen eines Azure Databricks-Arbeitsbereichs](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Erstellen eines Azure Databricks-Arbeitsbereichs")

    Geben Sie außerdem die folgenden Werte an:

    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |**Arbeitsbereichsname**     | Geben Sie einen Namen für Ihren Databricks-Arbeitsbereich an.        |
    |**Abonnement**     | Wählen Sie in der Dropdownliste Ihr Azure-Abonnement aus.        |
    |**Ressourcengruppe**     | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). |
    |**Location**     | Wählen Sie **USA, Westen 2** aus. Informationen zu weiteren verfügbaren Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).        |
    |**Tarif**     |  Wählen Sie zwischen **Standard** und **Premium**. Weitere Informationen zu diesen Tarifen, finden Sie unter [Azure Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/).       |

    Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie anschließend auf **Erstellen**.

3. Die Kontoerstellung dauert einige Minuten. Während der Erstellung des Kontos wird im Portal auf der rechten Seite die Kachel **Submitting deployment for Azure Databricks** (Bereitstellung für Azure Databricks wird übermittelt) angezeigt. Möglicherweise müssen Sie im Dashboard nach rechts scrollen, um die Kachel zu sehen. Am oberen Bildschirmrand wird auch eine Statusanzeige angezeigt. Sie können den Status beider Bereiche beobachten.

    ![Kachel zur Bereitstellung von Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Kachel zur Bereitstellung von Databricks")

## <a name="create-a-spark-cluster-in-databricks"></a>Erstellen eines Spark-Clusters in Databricks

1. Navigieren Sie im Azure-Portal zum erstellten Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

2. Sie werden zum Azure Databricks-Portal weitergeleitet. Klicken Sie im Portal auf **Cluster**.

    ![Databricks in Azure](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks in Azure")

3. Geben Sie auf der Seite **Neuer Cluster** die erforderlichen Werte an, um einen Cluster zu erstellen.

    ![Erstellen eines Databricks-Spark-Clusters in Azure](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Erstellen eines Databricks-Spark-Clusters in Azure")

    Geben Sie Werte für die folgenden Felder an, und übernehmen Sie bei den anderen Feldern die Standardwerte:

    * Geben Sie einen Namen für den Cluster ein.
    * Erstellen Sie im Rahmen dieses Artikels einen Cluster mit der Runtime **4.2**.
    * Aktivieren Sie das Kontrollkästchen **Terminate after \_\_ minutes of inactivity** (Nach _ Minuten Inaktivität beenden). Geben Sie an, nach wie vielen Minuten der Cluster beendet werden soll, wenn er nicht verwendet wird.

    Klicken Sie auf **Cluster erstellen**. Sobald der Cluster ausgeführt wird, können Sie Notizbücher an den Cluster anfügen und Spark-Aufträge ausführen.

## <a name="create-storage-account-file-system"></a>Erstellen des Dateisystems für das Speicherkonto

In diesem Abschnitt erstellen Sie ein Notizbuch im Azure Databricks-Arbeitsbereich und führen anschließend Codeausschnitte aus, um das Speicherkonto zu konfigurieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum erstellten Azure Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

2. Klicken Sie im linken Bereich auf **Arbeitsbereich**. Wählen Sie in der Dropdownliste **Arbeitsbereich** die Option **Erstellen** > **Notebook** aus.

    ![Erstellen eines Notizbuchs in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Erstellen eines Notizbuchs in Databricks")

3. Geben Sie im Dialogfeld **Notizbuch erstellen** einen Namen für das Notebook ein. Wählen Sie **Scala** als Sprache und anschließend den zuvor erstellten Spark-Cluster aus.

    ![Erstellen eines Notizbuchs in Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "Erstellen eines Notizbuchs in Databricks")

    Klicken Sie auf **Erstellen**.

4. Geben Sie in die erste Zelle den folgenden Code ein, und führen Sie ihn aus. Denken Sie daran, die in Klammern angezeigten Platzhalter im Beispiel durch Ihre eigenen Werte zu ersetzen:

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

5. Drücken Sie **UMSCHALT+EINGABE**, um die Codezelle auszuführen.

Daraufhin wird das Dateisystem für das Speicherkonto erstellt.

## <a name="upload-data-to-the-storage-account"></a>Hochladen von Daten in das Speicherkonto

Im nächsten Schritt wird eine Beispieldatendatei in das Speicherkonto hochgeladen, um sie später in Azure Databricks transformieren zu können. 

> [!NOTE]
> Falls Sie noch nicht über ein Konto verfügen, das für Azure Data Lake Storage Gen2 geeignet ist, können Sie [mit dieser Schnellstartanleitung ein Konto erstellen](./data-lake-storage-quickstart-create-account.md).

1. Laden Sie **small_radio_json.json** aus dem Repository [U-SQL Examples and Issue Tracking](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) (U-SQL-Beispiele und Problemverfolgung) herunter, und notieren Sie sich den Pfad, unter dem Sie die Datei speichern.

2. Laden Sie als Nächstes die Beispieldaten in Ihr Speicherkonto. Mit welcher Methode Sie die Daten in Ihr Speicherkonto hochladen, hängt davon ab, ob der hierarchische Namespace aktiviert ist.

    Wenn der hierarchische Namespace für Ihr Azure Storage-Speicherkonto aktiviert ist, können Sie Azure Data Factory, distp oder AzCopy (Version 10) für den Upload verwenden. Die AzCopy-Version 10 steht derzeit nur als Vorschauversion zur Verfügung. Fügen Sie den folgenden Code in ein Befehlsfenster ein, um AzCopy zu verwenden:

    ```bash
    set ACCOUNT_NAME=<ACCOUNT_NAME>
    set ACCOUNT_KEY=<ACCOUNT_KEY>
    azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
    ```
    
## <a name="extract-data-from-azure-storage"></a>Extrahieren von Daten aus Azure Storage

Kehren Sie zu Ihrem Databricks-Notizbuch zurück, und geben Sie den folgenden Code in eine neue Zelle ein:

1. Fügen Sie den folgenden Codeausschnitt in eine leere Codezelle ein, und ersetzen Sie die Platzhalterwerte durch die Werte, die Sie zuvor für das Speicherkonto gespeichert haben.

    ```scala
    dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
    dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
    ```

    Drücken Sie **UMSCHALT+EINGABE**, um die Codezelle auszuführen.

2. Nun können Sie die JSON-Beispieldatei als Datenrahmen in Azure Databricks laden. Fügen Sie den folgenden Codeausschnitt in eine neue Codezelle ein, und drücken Sie anschließend**UMSCHALT+EINGABE**. (Vergessen Sie dabei nicht, die Platzhalterwerte zu ersetzen).

    ```scala
    val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
    ```

3. Führen Sie den folgenden Code aus, um den Inhalt des Datenrahmens anzuzeigen.

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

## <a name="transform-data-in-azure-databricks"></a>Transformieren von Daten in Azure Databricks

Die unformatierten Beispieldaten aus **small_radio_json.json** dienen zur Erfassung der Zuhörer eines Radiosenders und enthalten eine Reihe von Spalten. In diesem Abschnitt transformieren Sie die Daten, um nur bestimmte Spalten aus dem Dataset abzurufen.

1. Rufen Sie zunächst nur die Spalten *firstName*, *lastName*, *gender*, *location* und *level* aus dem bereits erstellten Datenrahmen ab.

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

2.  Diese Daten können noch weiter transformiert werden, um die Spalte **level** in **subscription_type** umzubenennen:

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

## <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL Data Warehouse

In diesem Abschnitt laden Sie die transformierten Daten in Azure SQL Data Warehouse hoch. Mithilfe des Azure SQL Data Warehouse-Connectors für Azure Databricks können Sie einen Datenrahmen direkt als Tabelle in SQL Data Warehouse hochladen.

Der SQL Date Warehouse-Connector verwendet, wie bereits erwähnt, Azure Blob Storage als temporären Speicher, um Daten zwischen Azure Databricks und Azure SQL Data Warehouse hochzuladen. Geben Sie daher zunächst die Konfiguration für die Verbindungsherstellung mit dem Speicherkonto an. Das Konto muss im Rahmen der Vorbereitung für diesen Artikel bereits erstellt worden sein.

1. Geben Sie die Konfiguration für den Zugriff auf das Azure Storage-Konto über Azure Databricks an.

    ```scala
    val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
    val fileSystemName = "<FILE_SYSTEM_NAME>"
    val accessKey =  "<ACCESS_KEY>"
    ```

2. Geben Sie einen temporären Ordner an, der beim Verschieben von Daten zwischen Azure Databricks und Azure SQL Data Warehouse verwendet werden soll.

    ```scala
    val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
    ```

3. Führen Sie den folgenden Codeausschnitt aus, um Azure Blob-Speicherzugriffsschlüssel in der Konfiguration zu speichern. Dadurch wird sichergestellt, dass Sie den Zugriffsschlüssel im Notebook nicht als Klartext speichern müssen.

    ```scala
    val acntInfo = "fs.azure.account.key."+ storageURI
    sc.hadoopConfiguration.set(acntInfo, accessKey)
    ```

4. Geben Sie die Werte für die Verbindungsherstellung mit der Azure SQL Data Warehouse-Instanz an. Im Rahmen der Vorbereitung muss bereits eine SQL Data Warehouse-Instanz erstellt worden sein.

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

5. Führen Sie den folgenden Codeausschnitt aus, um den transformierten Datenrahmen (**renamedColumnsDF**) als Tabelle in SQL Data Warehouse zu laden. Dieser Codeausschnitt erstellt in der SQL-Datenbank eine Tabelle namens **SampleTable**.

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

6. Stellen Sie eine Verbindung mit der SQL-Datenbank her, und vergewissern Sie sich, dass eine Tabelle namens **SampleTable** vorhanden ist.

    ![Überprüfen der Beispieltabelle](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "Überprüfen der Beispieltabelle")

7. Führen Sie zur Überprüfung des Tabelleninhalts eine SELECT-Abfrage aus. Es sollten die gleichen Daten enthalten sein wie im Datenrahmen **renamedColumnsDF**.

    ![Überprüfen des Inhalts der Beispieltabelle](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "Überprüfen des Inhalts der Beispieltabelle")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss des Tutorials können Sie den Cluster beenden. Klicken Sie hierzu im linken Bereich des Azure Databricks-Arbeitsbereichs auf **Cluster**. Bewegen Sie den Cursor zum Beenden des Clusters auf die Auslassungspunkte in der Spalte **Aktionen**, und klicken Sie auf das Symbol **Beenden**.

![Beenden eines Databricks-Clusters](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Beenden eines Databricks-Clusters")

Wenn Sie den Cluster nicht manuell beenden, wird er automatisch beendet, sofern Sie bei der Erstellung des Clusters das Kontrollkästchen **Terminate after \_\_ minutes of inactivity** (Nach __ Minuten Inaktivität beenden) aktiviert haben. Der Cluster wird dann automatisch beendet, wenn er für den angegebenen Zeitraum inaktiv war.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs
> * Erstellen eines Spark-Clusters in Azure Databricks
> * Erstellen eines Azure Data Lake Storage Gen2-fähigen Kontos
> * Hochladen von Daten in Azure Data Lake Storage Gen2
> * Erstellen eines Notebooks in Azure Databricks
> * Extrahieren von Daten aus Data Lake Storage Gen2
> * Transformieren von Daten in Azure Databricks
> * Laden von Daten in Azure SQL Data Warehouse

Das nächste Tutorial beschäftigt sich mit dem Streamen von Echtzeitdaten an Azure Databricks unter Verwendung von Azure Event Hubs.

> [!div class="nextstepaction"]
>[Stream data into Azure Databricks using Event Hubs](../../azure-databricks/databricks-stream-from-eventhubs.md) (Streamen von Daten an Azure Databricks unter Verwendung von Event Hubs)

---
title: 'Tutorial: Ausführen von ETL-Vorgängen (Extrahieren, Transformieren und Laden) mithilfe von Apache Hive in Azure HDInsight'
description: In diesem Tutorial erfahren Sie, wie Sie Daten aus einem unformatierten CSV-Dataset extrahieren, mithilfe von Apache Hive in Azure HDInsight transformieren und die transformierten Daten dann mit Sqoop in eine Azure SQL-Datenbank laden.
services: storage
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: jamesbak
ms.openlocfilehash: 65d2d69c788a54371664d1a443a79bd121332470
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105150"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-apache-hive-on-azure-hdinsight"></a>Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight

In diesem Tutorial führen Sie einen ETL-Vorgang durch: Daten extrahieren, transformieren und laden. Sie verwenden eine unformatierte CSV-Datendatei, importieren diese Datei in einen Azure HDInsight-Cluster, transformieren sie mit Apache Hive und laden sie mit Apache Sqoop in eine Azure SQL-Datenbank.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Extrahieren und Hochladen der Daten in einen HDInsight-Cluster
> * Transformieren der Daten mithilfe von Apache Hive
> * Laden der Daten in eine Azure SQL-Datenbank mithilfe von Sqoop

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **Linux-basierter Hadoop-Cluster in HDInsight**: Informationen zum Erstellen eines neuen Linux-basierten HDInsight-Clusters finden Sie unter [Einrichten von Clustern in HDInsight mit Hadoop, Spark, Kafka usw.](./data-lake-storage-quickstart-create-connect-hdi-cluster.md)

* **Azure SQL-Datenbank**: Sie verwenden eine Azure SQL-Datenbank als Zieldatenspeicher. Wenn Sie keine SQL-Datenbank besitzen, finden Sie Informationen unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](../../sql-database/sql-database-get-started.md).

* **Azure CLI**: Wenn Sie die Azure-Befehlszeilenschnittstelle (Azure CLI) nicht installiert haben, finden Sie alle erforderlichen Informationen unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Ein SSH-Client:** Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Apache Hadoop) per SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Die Schritte in diesem Artikel erfordern einen HDInsight-Cluster unter Linux. Linux ist das einzige Betriebssystem, das in Azure HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="download-the-flight-data"></a>Herunterladen der Flugdaten

In diesem Tutorial werden Flugdaten des Bureau of Transportation Statistics verwendet, um die Durchführung eines ETL-Vorgangs zu veranschaulichen. Sie müssen diese Daten zum Durchführen des Tutorials herunterladen.

1. Rufen Sie die Website von [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time) (RITA) auf.

1. Wählen Sie auf der Website die folgenden Werte aus:

   | NAME | Wert |
   | --- | --- |
   | **Filter Year** (Jahr filtern) |2013 |
   | **Filter Period** (Zeitraum filtern) |January |
   | **Felder** |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |

1. Entfernen Sie die Häkchen bei allen anderen Feldern.

1. Wählen Sie **Herunterladen** aus. Sie erhalten eine ZIP-Datei mit den ausgewählten Datenfeldern.

## <a name="extract-and-upload-the-data"></a>Extrahieren und Hochladen der Daten

In diesem Abschnitt verwenden Sie `scp`, um Daten in Ihren HDInsight-Cluster hochzuladen.

Öffnen Sie eine Eingabeaufforderung, und verwenden Sie den folgenden Befehl, um die ZIP-Datei in den Hauptknoten des HDInsight-Clusters hochzuladen:

```bash
scp <FILE_NAME>.zip <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net:<FILE_NAME.zip>
```

* Ersetzen Sie \<FILE_NAME> durch den Namen der ZIP-Datei.
* Ersetzen Sie \<SSH_USER_NAME> durch den SSH-Anmeldenamen für den HDInsight-Cluster.
* Ersetzen Sie \<CLUSTER_NAME> durch den Namen des HDInsight-Clusters.

Wenn Sie für die Authentifizierung Ihrer SSH-Anmeldung ein Kennwort verwenden, werden Sie zur Eingabe dieses Kennworts aufgefordert. 

Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden und den Pfad zum passenden privaten Schlüssel angeben. Beispiel: `scp -i ~/.ssh/id_rsa FILE_NAME.zip USER_NAME@CLUSTER_NAME-ssh.azurehdinsight.net:`.

Stellen Sie nach Abschluss des Uploadvorgangs eine SSH-Verbindung mit dem Cluster her. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein:

```bash
ssh <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net
```

Extrahieren Sie die ZIP-Datei mit folgendem Befehl:

```bash
unzip <FILE_NAME>.zip
```

Dieser Befehl extrahiert eine **CSV**-Datei, die ungefähr 60 MB groß ist.

Verwenden Sie die folgenden Befehle, um ein Verzeichnis zu erstellen, und kopieren Sie die *CSV*-Datei in das Verzeichnis:

```bash
hdfs dfs -mkdir -p abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data
hdfs dfs -put <FILE_NAME>.csv abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data/
```

Verwenden Sie den folgenden Befehl, um das Data Lake Storage Gen2-Dateisystem zu erstellen.

```bash
hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/
```

## <a name="transform-the-data"></a>Transformieren der Daten

In diesem Abschnitt verwenden Sie Beeline, um einen Apache Hive-Auftrag auszuführen.

Im Rahmen des Apache Hive-Auftrags importieren Sie die Daten aus der CSV-Datei in eine Apache Hive-Tabelle namens **delays**.

Verwenden Sie an der SSH-Eingabeaufforderung, die bereits für den HDInsight-Cluster vorhanden ist, den folgenden Befehl, um eine neue Datei namens **flightdelays.hql** zu erstellen und zu bearbeiten:

```bash
nano flightdelays.hql
```

Verwenden Sie als Inhalt der Datei den folgenden Text:

```hiveql
 DROP TABLE delays_raw;
 -- Creates an external table over the csv file
 CREATE EXTERNAL TABLE delays_raw (
    YEAR string,
    FL_DATE string,
    UNIQUE_CARRIER string,
    CARRIER string,
    FL_NUM string,
    ORIGIN_AIRPORT_ID string,
    ORIGIN string,
    ORIGIN_CITY_NAME string,
    ORIGIN_CITY_NAME_TEMP string,
    ORIGIN_STATE_ABR string,
    DEST_AIRPORT_ID string,
    DEST string,
    DEST_CITY_NAME string,
    DEST_CITY_NAME_TEMP string,
    DEST_STATE_ABR string,
    DEP_DELAY_NEW float,
    ARR_DELAY_NEW float,
    CARRIER_DELAY float,
    WEATHER_DELAY float,
    NAS_DELAY float,
    SECURITY_DELAY float,
    LATE_AIRCRAFT_DELAY float)
 -- The following lines describe the format and location of the file
 ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
 LINES TERMINATED BY '\n'
 STORED AS TEXTFILE
 LOCATION 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data';

-- Drop the delays table if it exists
DROP TABLE delays;
-- Create the delays table and populate it with data
-- pulled in from the CSV file (via the external table defined previously)
CREATE TABLE delays
LOCATION abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/processed
AS
SELECT YEAR AS year,
    FL_DATE AS flight_date,
    substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
    substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
    substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
    ORIGIN_AIRPORT_ID AS origin_airport_id,
    substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
    substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
    substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
    DEST_AIRPORT_ID AS dest_airport_id,
    substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
    substring(DEST_CITY_NAME,2) AS dest_city_name,
    substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
    DEP_DELAY_NEW AS dep_delay_new,
    ARR_DELAY_NEW AS arr_delay_new,
    CARRIER_DELAY AS carrier_delay,
    WEATHER_DELAY AS weather_delay,
    NAS_DELAY AS nas_delay,
    SECURITY_DELAY AS security_delay,
    LATE_AIRCRAFT_DELAY AS late_aircraft_delay
FROM delays_raw;
```

Um die Datei zu speichern, drücken Sie die ESC-TASTE, und geben Sie dann `:x` ein.

Starten Sie Hive mit dem folgenden Befehl, und führen Sie die Datei **flightdelays.hql** aus:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
```

Öffnen Sie nach der Ausführung des Skripts __flightdelays.hql__ mithilfe des folgenden Befehls eine interaktive Beeline-Sitzung:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
```

Wenn Sie die `jdbc:hive2://localhost:10001/>`-Eingabeaufforderung erhalten, rufen Sie die Daten mit der folgenden Abfrage aus den importierten Flugverspätungsdaten ab:

```hiveql
INSERT OVERWRITE DIRECTORY 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output'
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
SELECT regexp_replace(origin_city_name, '''', ''),
    avg(weather_delay)
FROM delays
WHERE weather_delay IS NOT NULL
GROUP BY origin_city_name;
```

Mit dieser Abfrage werden eine Liste von Orten, in denen Verspätungen infolge des Wetters auftraten, sowie die durchschnittliche Verspätung abgerufen. Die Liste wird anschließend in `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` gespeichert. Sqoop liest später die Daten an diesem Speicherort und exportiert sie in die Azure SQL-Datenbank.

Geben Sie zum Beenden von Beeline `!quit` an der Eingabeaufforderung ein.

## <a name="create-a-sql-database-table"></a>Erstellen einer SQL-Datenbanktabelle

Für diesen Vorgang benötigen Sie den Servernamen Ihrer SQL-Datenbank. Führen Sie die folgenden Schritte aus, um den Servernamen festzustellen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **SQL-Datenbanken** aus.

1. Filtern Sie nach dem Namen der Datenbank, die Sie verwenden möchten. Der Servername befindet sich in der Spalte **Servername**.

1. Filtern Sie nach dem Namen der Datenbank, die Sie verwenden möchten. Der Servername befindet sich in der Spalte **Servername**.

    ![Abrufen von Details für den Azure SQL-Server](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Abrufen von Details für den Azure SQL-Server")

    Es gibt viele Möglichkeiten, eine Verbindung mit der SQL-Datenbank herzustellen und eine Tabelle zu erstellen. Die folgenden Schritte verwenden [FreeTDS](http://www.freetds.org/) aus dem HDInsight-Cluster.

Verwenden Sie zum Installieren von FreeTDS den folgenden Befehl über eine SSH-Verbindung mit dem Cluster:

```bash
sudo apt-get --assume-yes install freetds-dev freetds-bin
```

Stellen Sie nach Abschluss der Installation mithilfe des folgenden Befehls eine Verbindung mit dem SQL-Datenbankserver her.

* Ersetzen Sie \<SERVER_NAME> durch den Namen des SQL-Datenbankservers.
* Ersetzen Sie \<ADMIN_LOGIN> durch den Administratoranmeldenamen für die SQL-Datenbank.
* Ersetzen Sie \<DATABASE_NAME> durch den Datenbanknamen.

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -p 1433 -D <DATABASE_NAME>
```

Geben Sie das Kennwort für die SQL-Datenbank-Administratoranmeldung ein, wenn Sie dazu aufgefordert werden.

Eine Ausgabe ähnlich folgendem Text wird angezeigt:

```
locale is "en_US.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
Default database being set to sqooptest
1>
```

Geben Sie an der Eingabeaufforderung `1>` die folgenden Anweisungen ein:

```hiveql
CREATE TABLE [dbo].[delays](
[origin_city_name] [nvarchar](50) NOT NULL,
[weather_delay] float,
CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
([origin_city_name] ASC))
GO
```

Nach Eingabe der Anweisung `GO` werden die vorherigen Anweisungen ausgewertet.
Diese Abfrage erstellt eine Tabelle namens **delays**, die über einen gruppierten Index verfügt.

Stellen Sie mithilfe der folgenden Abfrage sicher, dass die Tabelle erstellt wurde:

```hiveql
SELECT * FROM information_schema.tables
GO
```

Die Ausgabe sieht in etwa wie folgender Text aus:

```
TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
databaseName       dbo             delays        BASE TABLE
```

EINGABE `exit` at the `1>` ein.

## <a name="export-and-load-the-data"></a>Exportieren und Laden der Daten

In den vorherigen Abschnitten haben Sie die transformierten Daten unter `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` kopiert. In diesem Abschnitt verwenden Sie Sqoop, um die Daten aus `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` in die Tabelle zu exportieren, die Sie in der Azure SQL-Datenbank erstellt haben.

Verwenden Sie den folgenden Befehl, um zu überprüfen, ob Sqoop Ihre SQL-Datenbank erreichen kann:

```bash
sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
```

Der Befehl gibt eine Liste von Datenbanken zurück, in der auch die Datenbank enthalten ist, in der Sie die Tabelle **delays** erstellt haben.

Verwenden Sie den folgenden Befehl, um Daten aus der Tabelle **hivesampletable** in die Tabelle **delays** zu exportieren:

```bash
sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<FILE_SYSTEM_NAME>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
```

Sqoop stellt eine Verbindung mit der Datenbank her, die die Tabelle **delays** enthält, und exportiert Daten aus dem Verzeichnis `/tutorials/flightdelays/output` in die Tabelle **delays**.

Nach Abschluss des `sqoop`-Befehls stellen Sie über das Hilfsprogramm tsql eine Verbindung mit der Datenbank her:

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
```

Überprüfen Sie mithilfe der folgenden Anweisungen, ob die Daten in die Tabelle **delays** exportiert wurden:

```sql
SELECT * FROM delays
GO
```

Es sollte eine Liste der Tabellendaten angezeigt werden. Die Tabelle enthält den Namen der Stadt und die durchschnittliche Flugverspätung für diese Stadt.

Geben Sie `exit` ein, um das Hilfsprogramm tsql zu beenden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial wurden nur Ressourcen verwendet, die bereits vorhanden waren. Eine Bereinigung ist nicht erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Arbeiten mit Daten in HDInsight finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks](./data-lake-storage-use-hdi-cluster.md)

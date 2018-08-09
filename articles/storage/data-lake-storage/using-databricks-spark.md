---
title: Zugreifen auf Azure-Daten vom Typ „Data Lake Storage Gen2“ (Vorschauversion) mit Azure Databricks unter Verwendung von Spark | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Spark-Abfragen in einem Azure Databricks-Cluster ausführen, um auf Daten in einem Azure-Speicherkonto vom Typ „Data Lake Storage Gen2“ zuzugreifen.
services: hdinsight,storage
tags: azure-portal
author: dineshm
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 6/27/2018
ms.author: dineshm
ms.openlocfilehash: 4f01352f13b7a831791c7e74520cc08b98afa661
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521454"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Zugreifen auf Azure-Daten vom Typ „Data Lake Storage Gen2“ (Vorschauversion) mit Azure Databricks unter Verwendung von Spark

In diesem Tutorial erfahren Sie, wie Sie Spark-Abfragen in einem Azure Databricks-Cluster ausführen, um Daten in einem Azure-Konto abzufragen, das für „Data Lake Storage Gen2“ (Vorschauversion) geeignet ist.

> [!div class="checklist"]
> * Erstellen eines Databricks-Clusters
> * Erfassen von unstrukturierten Daten in einem Speicherkonto
> * Auslösen einer Azure-Funktion zum Verarbeiten von Daten
> * Ausführen von Analysen für Ihre Daten im Blobspeicher

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird veranschaulicht, wie Sie die Flugdaten einer Fluggesellschaft nutzen und abfragen, die vom [Verkehrsministerium der Vereinigten Staaten](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time) zur Verfügung gestellt werden. Laden Sie Flugdaten für einen Zeitraum von mindestens zwei Jahren herunter (alle Felder auswählen), und speichern Sie das Ergebnis auf Ihrem Computer. Achten Sie darauf, dass Sie sich den Dateinamen und -pfad Ihres Downloads notieren. Sie benötigen diese Angaben in einem späteren Schritt.

> [!NOTE]
> Klicken Sie auf das Kontrollkästchen **Prezipped file** (ZIP-Datei), um alle Datenfelder auszuwählen. Der Download hat eine Größe von einigen GB, aber diese Datenmenge ist für die Analyse erforderlich.

## <a name="create-an-azure-data-lake-storage-gen2-account"></a>Erstellen eines Azure-Kontos vom Typ „Data Lake Storage Gen2“

Erstellen Sie zunächst ein neues [Azure-Konto vom Typ „Data Lake Storage Gen2“](quickstart-create-account.md), und geben Sie ihm einen eindeutigen Namen. Navigieren Sie anschließend zu diesem Speicherkonto, um Konfigurationseinstellungen abzurufen.

> [!IMPORTANT]
> Während der Vorschauphase funktionieren Azure-Funktionen nur mit Azure-Konten vom Typ „Data Lake Storage Gen2“, die mit einem flachen Namespace erstellt wurden.

1. Klicken Sie unter **Einstellungen** auf **Zugriffsschlüssel**.
3. Klicken Sie auf die Schaltfläche **Kopieren** neben **key1**, um den Schlüsselwert zu kopieren.

Sowohl der Kontoname als auch der Schlüssel sind für weitere Schritte dieses Tutorials erforderlich. Öffnen Sie einen Text-Editor, und notieren Sie sich darin den Kontonamen und Schlüssel zur späteren Verwendung.

## <a name="create-a-databricks-cluster"></a>Erstellen eines Databricks-Clusters

Im nächsten Schritt wird ein [Databricks-Cluster](https://docs.azuredatabricks.net/) erstellt, um einen Datenarbeitsbereich erstellen zu können.

1. Erstellen Sie einen [Databricks-Dienst](https://ms.portal.azure.com/#create/Microsoft.Databricks), und geben Sie ihm den Namen **myFlightDataService**. (Aktivieren Sie beim Erstellen des Diensts das Kontrollkästchen *An Dashboard anheften*.)
2. Klicken Sie auf **Launch Workspace** (Arbeitsbereich starten), um den Arbeitsbereich in einem neuen Browserfenster zu öffnen.
3. Klicken Sie in der Navigationsleiste auf der linken Seite auf **Cluster**.
4. Klicken Sie auf **Cluster erstellen**.
5. Geben Sie im Feld *Clustername* den Cluster **myFlightDataCluster** ein.
6. Wählen Sie im Feld *Worker-Typ* die Option **Standard_D8s_v3**.
7. Ändern Sie den Wert **Min Workers** (Min. Worker) in *4*.
8. Klicken Sie oben auf der Seite auf **Cluster erstellen** (dieser Prozess kann bis zu fünf Minuten dauern).
9. Nachdem der Prozess abgeschlossen ist, können Sie oben links in der Navigationsleiste **Azure Databricks** wählen.
10. Wählen Sie in der unteren Hälfte der Seite im Abschnitt **Neu** die Option **Notebook**.
11. Geben Sie im Feld **Name** den gewünschten Namen ein, und wählen Sie **Python** als Sprache aus.
12. Für alle anderen Felder können die Standardwerte übernommen werden.
13. Klicken Sie auf **Erstellen**.
14. Fügen Sie den folgenden Code in die Zelle **Cmd 1** ein, und ersetzen Sie die Werte durch die Werte, die Sie sich für Ihr Speicherkonto notiert haben.

    ```bash
    spark.conf.set("fs.azure.account.key.<account_name>.dfs.core.windows.net", "<account_key>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfs://<file_system>@<account_name>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
    ```

## <a name="ingest-data"></a>Erfassen von Daten

### <a name="copy-source-data-into-the-storage-account"></a>Kopieren von Quelldaten in das Speicherkonto

Die nächste Aufgabe ist die Nutzung von AzCopy zum Kopieren der Daten aus der *CSV*-Datei in den Azure-Speicher. Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie die folgenden Befehle ein. Achten Sie darauf, die Platzhalter `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` und `<ACCOUNT_KEY>` durch die entsprechenden Werte zu ersetzen, die Sie in einem vorherigen Schritt notiert haben.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Verwenden des Databricks-Notebooks zum Konvertieren von CSV in Parquet

Öffnen Sie Databricks erneut in Ihrem Browser, und führen Sie die folgenden Schritte aus:

1. Wählen Sie oben links in der Navigationsleiste die Option **Azure Databricks**.
2. Wählen Sie in der unteren Hälfte der Seite im Abschnitt **Neu** die Option **Notebook**.
3. Geben Sie im Feld **Name** den Namen **CSV2Parquet** ein.
4. Für alle anderen Felder können die Standardwerte übernommen werden.
5. Klicken Sie auf **Erstellen**.
6. Fügen Sie den folgenden Code in die Zelle **Cmd 1** ein (mit diesem Code wird im Editor das automatische Speichern durchgeführt).

    ```python
    #mount Azure Blob Storage as an HDFS file system to your databricks cluster
    #you need to specify a storage account and container to connect to. 
    #use a SAS token or an account key to connect to Blob Storage.  
    accountname = "<insert account name>' 
    accountkey = " <insert account key>'
    fullname = "fs.azure.account.key." +accountname+ ".blob.core.windows.net"
    accountsource = "abfs://dbricks@" +accountname+ ".blob.core.windows.net/folder1"
    #create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/On_Time_On_Time*.csv")
    #read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet(accountsource + '/parquet/flights')

    #read the flight details parquet file 
    #flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")
    print("Done")
    ```

## <a name="explore-data-using-hadoop-distributed-file-system"></a>Untersuchen von Daten per Hadoop Distributed File System

Kehren Sie zum Databricks-Arbeitsbereich zurück, und klicken Sie auf der linken Navigationsleiste auf **Zuletzt verwendet**.

1. Klicken Sie auf das Notebook **Flight Data Analytics** (Flugdatenanalyse).
2. Drücken Sie **STRG+ALT+N**, um eine neue Zelle zu erstellen.

Geben Sie die folgenden Codeblöcke unter **Cmd 1** ein, und drücken Sie **CMD+EINGABE**, um das Python-Skript auszuführen.

Führen Sie das folgende Skript aus, um eine Liste mit CSV-Dateien abzurufen, die per AzCopy hochgeladen wurden:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"
dbutils.fs.ls(source + "/temp")
display(dbutils.fs.ls(source + "/temp/"))
```

Führen Sie dieses Skript aus, um eine neue Datei zu erstellen und die Dateien im Ordner *parquet/flights* aufzulisten:

```python
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"

dbutils.fs.help()

dbutils.fs.put(source + "/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls(source + "/temp/parquet/flights")
```
Mit diesen Codebeispielen haben Sie die hierarchische Auslegung von HDFS untersucht, indem Sie Daten eines Azure-Kontos genutzt haben, das für „Data Lake Storage Gen2“ geeignet ist.

## <a name="query-the-data"></a>Abfragen von Daten

Als Nächstes können Sie damit beginnen, die Daten abzufragen, die Sie in Azure Data Lake Storage hochgeladen haben. Geben Sie die folgenden Codeblöcke unter **Cmd 1** ein, und drücken Sie **CMD+EINGABE**, um das Python-Skript auszuführen.

### <a name="simple-queries"></a>Einfache Abfragen

Führen Sie das folgende Skript aus, um Dataframes für Ihre Datenquellen zu erstellen:

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie den Platzhalter **<YOUR_CSV_FILE_NAME>** durch den Dateinamen ersetzen, den Sie am Anfang dieses Tutorials heruntergeladen haben.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet(accountsource + '/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created via the Azure Function
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferrably run this in a separate cmd cell
display(flightDF)
```

Führen Sie das folgende Skript aus, um Analyseabfragen für die Daten auszuführen:

```python
#Run each of these queries, preferrably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet(accountsource + '/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet(accountsource + '/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```
### <a name="complex-queries"></a>Komplexe Abfragen

Führen Sie die Segmente im Notebook jeweils einzeln aus, und sehen Sie sich die Ergebnisse an, um die folgenden komplexeren Abfragen durchzuführen.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="next-steps"></a>Nächste Schritte

* [Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](tutorial-extract-transform-load-hive.md)
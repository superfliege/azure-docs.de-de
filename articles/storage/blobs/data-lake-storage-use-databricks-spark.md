---
title: Zugreifen auf Azure-Daten vom Typ „Data Lake Storage Gen2“ (Vorschauversion) mit Azure Databricks unter Verwendung von Spark | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Spark-Abfragen in einem Azure Databricks-Cluster ausführen, um auf Daten in einem Azure-Speicherkonto vom Typ „Data Lake Storage Gen2“ zuzugreifen.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: dineshm
ms.openlocfilehash: 88a05eb8fa59740012ca6c7a8d8508d565854dc7
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974157"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Zugreifen auf Azure-Daten vom Typ „Data Lake Storage Gen2“ (Vorschauversion) mit Azure Databricks unter Verwendung von Spark

In diesem Tutorial erfahren Sie, wie Sie Spark-Abfragen für einen Azure Databricks-Cluster ausführen, um Daten in einem Azure-Speicherkonto abzufragen, wenn die Vorschauversion von Azure Data Lake Storage Gen2 aktiviert ist.

> [!div class="checklist"]
> * Erstellen eines Databricks-Clusters
> * Erfassen von unstrukturierten Daten in einem Speicherkonto
> * Ausführen von Analysen für Ihre Daten im Blobspeicher

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird veranschaulicht, wie Sie die Flugdaten einer Fluggesellschaft nutzen und abfragen, die vom [Verkehrsministerium der Vereinigten Staaten](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time) zur Verfügung gestellt werden. Laden Sie Flugdaten für einen Zeitraum von mindestens zwei Jahren herunter (wählen Sie alle Felder aus), und speichern Sie das Ergebnis auf Ihrem Computer. Achten Sie darauf, dass Sie sich den Dateinamen und -pfad Ihres Downloads notieren. Sie benötigen diese Angaben in einem späteren Schritt.

> [!NOTE]
> Klicken Sie auf das Kontrollkästchen **Prezipped file** (ZIP-Datei), um alle Datenfelder auszuwählen. Der Download hat eine Größe von einigen GB, aber diese Datenmenge ist für die Analyse erforderlich.

## <a name="create-an-azure-storage-account-with-analytic-capabilities"></a>Erstellen eines Azure-Speicherkontos mit Analysefunktionen

Erstellen Sie zunächst ein neues [Speicherkonto mit Analysefunktionen](data-lake-storage-quickstart-create-account.md), und geben Sie ihm einen eindeutigen Namen. Navigieren Sie anschließend zu diesem Speicherkonto, um Konfigurationseinstellungen abzurufen.

1. Klicken Sie unter **Einstellungen** auf **Zugriffsschlüssel**.
2. Klicken Sie auf die Schaltfläche **Kopieren** neben **key1**, um den Schlüsselwert zu kopieren.

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
14. Fügen Sie den folgenden Code in die Zelle **Cmd 1** ein. Vergessen Sie nicht, die in Klammern angegebenen Platzhalter durch Ihre eigenen Werte zu ersetzen:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
        
    dbutils.fs.mount(
        source = "abfss://dbricks@<account-name>.dfs.core.windows.net/folder1",
        mount_point = "/mnt/flightdata",
        extra_configs = configs)
    ```
15. Drücken Sie **UMSCHALT+EINGABE**, um die Codezelle auszuführen.

## <a name="ingest-data"></a>Erfassen von Daten

### <a name="copy-source-data-into-the-storage-account"></a>Kopieren von Quelldaten in das Speicherkonto

Die nächste Aufgabe ist die Nutzung von AzCopy zum Kopieren der Daten aus der *CSV*-Datei in den Azure-Speicher. Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie die folgenden Befehle ein. Achten Sie darauf, die Platzhalter `<DOWNLOAD_FILE_PATH>` und `<ACCOUNT_KEY>` durch die entsprechenden Werte zu ersetzen, die Sie in einem vorherigen Schritt notiert haben.

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
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
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
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Führen Sie dieses Skript aus, um eine neue Datei zu erstellen und die Dateien im Ordner *parquet/flights* aufzulisten:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Anhand dieser Codebeispiele haben Sie die hierarchische Struktur von HDFS unter Verwendung von Daten untersucht, die in einem Speicherkonto gespeichert sind, für das Azure Data Lake Storage Gen2 aktiviert ist.

## <a name="query-the-data"></a>Abfragen von Daten

Als Nächstes können Sie damit beginnen, die Daten abzufragen, die Sie in Ihr Speicherkonto hochgeladen haben. Geben Sie die folgenden Codeblöcke unter **Cmd 1** ein, und drücken Sie **CMD+EINGABE**, um das Python-Skript auszuführen.

### <a name="simple-queries"></a>Einfache Abfragen

Führen Sie das folgende Skript aus, um Dataframes für Ihre Datenquellen zu erstellen:

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie den Platzhalter **<YOUR_CSV_FILE_NAME>** durch den Dateinamen ersetzen, den Sie am Anfang dieses Tutorials heruntergeladen haben.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

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
#preferably run this in a separate cmd cell
display(flightDF)
```

Führen Sie das folgende Skript aus, um Analyseabfragen für die Daten auszuführen:

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
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

* [Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)

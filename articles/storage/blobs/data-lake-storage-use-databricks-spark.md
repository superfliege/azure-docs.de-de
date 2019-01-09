---
title: 'Tutorial: Zugreifen auf Azure-Daten vom Typ „Data Lake Storage Gen2“ (Vorschauversion) mit Azure Databricks unter Verwendung von Spark | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Spark-Abfragen in einem Azure Databricks-Cluster ausführen, um auf Daten in einem Azure-Speicherkonto vom Typ „Data Lake Storage Gen2“ zuzugreifen.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: dineshm
ms.openlocfilehash: b0382d31f9d16228ca3447ace9c7d4f171b206f6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548985"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Zugreifen auf Azure-Daten in Data Lake Storage Gen2 (Vorschauversion) mit Azure Databricks unter Verwendung von Spark

In diesem Tutorial erfahren Sie, wie Sie Ihren Azure Databricks-Cluster mit Daten in einem Azure-Speicherkonto verbinden, für das Azure Data Lake Storage Gen2 (Vorschauversion) aktiviert ist. Diese Verbindung ermöglicht die native Ausführung von Datenabfragen und -analysen über Ihren Cluster.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Databricks-Clusters
> * Erfassen von unstrukturierten Daten in einem Speicherkonto
> * Ausführen von Analysen für Ihre Daten im Blobspeicher

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird veranschaulicht, wie Sie die Flugdaten einer Fluggesellschaft nutzen und abfragen, die vom [Verkehrsministerium der Vereinigten Staaten](https://transtats.bts.gov/DL_SelectFields.asp) zur Verfügung gestellt werden. 

1. Aktivieren Sie das Kontrollkästchen **Prezipped file** (ZIP-Datei), um alle Datenfelder auszuwählen.
2. Wählen Sie **Herunterladen** aus, und speichern Sie die Ergebnisse auf Ihrem Computer.
3. Notieren Sie sich den Dateinamen und den Pfad des Downloads. Diese Angaben werden in einem späteren Schritt benötigt.

Für dieses Tutorial benötigen Sie ein Speicherkonto mit Analysefunktionen. Wir empfehlen, anhand der entsprechenden [Schnellstartanleitung](data-lake-storage-quickstart-create-account.md) ein geeignetes Speicherkonto zu erstellen. Navigieren Sie anschließend zu dem erstellten Speicherkonto, um Konfigurationseinstellungen abzurufen.

1. Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
2. Wählen Sie neben **key1** die Schaltfläche **Kopieren** aus, um den Schlüsselwert zu kopieren.

Sowohl der Kontoname als auch der Schlüssel sind für weitere Schritte dieses Tutorials erforderlich. Öffnen Sie einen Text-Editor, und notieren Sie sich darin den Kontonamen und Schlüssel zur späteren Verwendung.

## <a name="create-a-databricks-cluster"></a>Erstellen eines Databricks-Clusters

Im nächsten Schritt wird ein Databricks-Cluster erstellt, um einen Datenarbeitsbereich erstellen zu können.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** aus.
2. Geben Sie **Azure Databricks** in das Suchfeld ein.
3. Wählen Sie auf dem Blatt „Azure Databricks“ die Option **Erstellen** aus.
4. Nennen Sie Ihren Databricks-Dienst **myFlightDataService**. (Aktivieren Sie beim Erstellen des Diensts das Kontrollkästchen *An Dashboard anheften*.)
5. Wählen Sie **Launch Workspace** (Arbeitsbereich starten) aus, um den Arbeitsbereich in einem neuen Browserfenster zu öffnen.
6. Wählen Sie auf der linken Navigationsleiste **Cluster** aus.
7. Wählen Sie **Cluster erstellen** aus.
8. Geben Sie im Feld **Clustername** den Cluster **myFlightDataCluster** ein.
9. Wählen Sie im Feld **Worker-Typ** die Option **Standard_D8s_v3**.
10. Ändern Sie den Wert **Min Workers** (Min. Worker) in **4**.
11. Wählen Sie am oberen Seitenrand **Cluster erstellen** aus. (Dieser Vorgang kann bis zu fünf Minuten dauern.)
12. Wählen Sie nach Abschluss des Vorgangs links oben auf der Navigationsleiste die Option **Azure Databricks** aus.
13. Wählen Sie in der unteren Hälfte der Seite im Abschnitt **Neu** die Option **Notebook**.
14. Geben Sie im Feld **Name** den gewünschten Namen ein, und wählen Sie **Python** als Sprache aus.
15. Für alle anderen Felder können die Standardwerte übernommen werden.
16. Klicken Sie auf **Erstellen**.
17. Fügen Sie den folgenden Code in die Zelle **Cmd 1** ein. Ersetzen Sie die in Klammern angegebenen Platzhalter durch Ihre eigenen Werte:

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
18. Drücken Sie **UMSCHALT+EINGABE**, um die Codezelle auszuführen.

## <a name="ingest-data"></a>Erfassen von Daten

### <a name="copy-source-data-into-the-storage-account"></a>Kopieren von Quelldaten in das Speicherkonto

Die nächste Aufgabe ist die Nutzung von AzCopy zum Kopieren der Daten aus der *CSV*-Datei in den Azure-Speicher. Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie die folgenden Befehle ein. Die Platzhalter `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` und `<ACCOUNT_KEY>` müssen durch die entsprechenden Werte ersetzt werden, die Sie in einem vorherigen Schritt notiert haben.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Verwenden des Databricks-Notebooks zum Konvertieren von CSV in Parquet

Öffnen Sie Databricks erneut in Ihrem Browser, und führen Sie die folgenden Schritte aus:

1. Wählen Sie links oben auf der Navigationsleiste die Option **Azure Databricks** aus.
2. Wählen Sie in der unteren Hälfte der Seite im Abschnitt **Neu** die Option **Notebook**.
3. Geben Sie im Feld **Name** den Namen **CSV2Parquet** ein.
4. Für alle anderen Felder können die Standardwerte übernommen werden.
5. Klicken Sie auf **Erstellen**.
6. Fügen Sie den folgenden Code in die Zelle **Cmd 1** ein. (Dieser Code dient zur automatischen Speicherung im Editor.)

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data"></a>Erkunden von Daten

Kehren Sie zum Databricks-Arbeitsbereich zurück, und wählen Sie auf der linken Navigationsleiste das Symbol **Zuletzt verwendet** aus.

1. Wählen Sie das Notebook **Flight Data Analytics** (Flugdatenanalyse) aus.
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

### <a name="run-simple-queries"></a>Ausführen einfacher Abfragen

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

### <a name="run-complex-queries"></a>Ausführen komplexer Abfragen

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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe für das Speicherkonto und anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

[!div class="nextstepaction"] 
> [Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)


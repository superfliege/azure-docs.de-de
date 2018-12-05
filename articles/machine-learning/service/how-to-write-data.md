---
title: Schreiben von Daten mit dem Azure Machine Learning Data Prep SDK – Python
description: Erfahren Sie mehr über das Schreiben von Daten mit dem Azure Machine Learning Data Prep SDK. Sie können Daten an jeder beliebigen Stelle in einem Datenfluss und in Dateien in unseren unterstützten Speicherorten (lokales Dateisystem, Azure Blob Storage und Azure Data Lake Storage) schreiben.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 4a2af832fda8a85ee8a4aba395a8f436172153ed
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308561"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Schreiben von Daten mit dem Azure Machine Learning Data Prep SDK

In diesem Artikel lernen Sie verschiedene Methoden zum Schreiben von Daten mit dem Azure Machine Learning Data Prep SDK kennen. Ausgabedaten können an beliebigem Punkt in einen Datenfluss geschrieben werden, und diese Schreibvorgänge werden dem Ergebnisdatenfluss als Schritte hinzugefügt und bei jedem Ausführen des Datenflusses ausgeführt. Daten werden in mehrere Partitionsdateien geschrieben, um parallele Schreibvorgänge zuzulassen.

Da es keine Einschränkungen für die Anzahl von Schreibschritten in einer Pipeline gibt, können Sie problemlos zusätzliche Schreibschritte hinzufügen, um Zwischenergebnisse bei der Problembehandlung oder für andere Pipelines abzurufen.

Jedes Mal, wenn Sie einen Schreibschritt ausführen, wird ein vollständiger Pull für die Daten im Datenfluss ausgeführt. So wird beispielsweise ein Datenfluss mit drei Schreibschritten jeden Datensatz im Dataset dreimal lesen und verarbeiten.

## <a name="supported-data-types-and-location"></a>Unterstützte Datentypen und Speicherorte

Die folgenden Dateiformate werden unterstützt:
-   Durch Trennzeichen getrennte Dateien (CSV, TSV usw.)
-   Parquet-Dateien

Mit dem [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) können Sie Daten in diese Speicherorte schreiben:
+ Ein lokales Dateisystem
+ Azure Blob Storage
+ Azure Data Lake Store

## <a name="spark-considerations"></a>Überlegungen zu Spark

Beim Ausführen eines Datenflusses in Spark müssen Sie in einen leeren Ordner schreiben. Der Versuch, in einen vorhandenen Ordner zu schreiben, führt zu einem Fehler. Stellen Sie sicher, dass Ihr Zielordner leer ist, oder verwenden Sie für jede Ausführung einen anderen Zielort, da der Schreibvorgang sonst fehlschlägt.

## <a name="monitoring-write-operations"></a>Überwachen von Schreibvorgängen

Der Einfachheit halber wird nach Abschluss des Schreibvorgangs eine Sentineldatei mit dem Namen „SUCCESS“ erzeugt. Sie hilft Ihnen zu erkennen, wann ein Zwischenschreibvorgang abgeschlossen ist, ohne dass Sie warten müssen, bis die gesamte Pipeline abgeschlossen ist.

## <a name="example-write-code"></a>Beispielhafter Schreibcode

Laden Sie zuerst Daten in einen Datenfluss. Diese Daten werden mit unterschiedlichen Formaten wiederverwendet.

```python
import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```

Beispielausgabe:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000,0 |   99999,0 |   Keine|       NO|     NO  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003,0 |   99999,0 |   Keine|       NO|     NO  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010,0|    99999,0|    Keine|   NO| JN| ENJA|   70933,0|    –8667,0 |90,0|
|3| 10013,0|    99999,0|    Keine|   NO| NO| |   NaN|    NaN|    NaN|
|4| 10014,0|    99999,0|    Keine|   NO| NO| ENSO|   59783,0|    5350,0| 500,0|
|5| 10015,0|    99999,0|    Keine|   NO| NO| ENBL|   61383,0|    5867,0| 3270,0|
|6| 10016,0 |99999,0|   Keine|   NO| NO|     |64850,0|   11233,0|    140,0|
|7| 10017,0|    99999,0|    Keine|   NO| NO| ENFR|   59933,0|    2417,0| 480,0|
|8| 10020,0|    99999,0|    Keine|   NO| SV|     |80050,0|   16250,0|    80,0|
|9| 10030,0|    99999,0|    Keine|   NO| SV|     |77000,0|   15500,0|    120,0|

### <a name="delimited-file-example"></a>Beispiel für eine durch Trennzeichen getrennte Datei

Der folgende Code verwendet die `write_to_csv`-Funktion zum Schreiben von Daten in eine durch Trennzeichen getrennte Datei.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Beispielausgabe:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000,0 |   99999,0 |   ERROR |       NO|     NO  |   ENRS    |ERROR    |   ERROR |   ERROR|    
|   1|      10003,0 |   99999,0 |   ERROR |       NO|     NO  |   ENSO|       ERROR|        ERROR |ERROR|   
|   2|  10010,0|    99999,0|    ERROR |   NO| JN| ENJA|   70933,0|    –8667,0 |90,0|
|3| 10013,0|    99999,0|    ERROR |   NO| NO| |   ERROR|    ERROR|    ERROR|
|4| 10014,0|    99999,0|    ERROR |   NO| NO| ENSO|   59783,0|    5350,0| 500,0|
|5| 10015,0|    99999,0|    ERROR |   NO| NO| ENBL|   61383,0|    5867,0| 3270,0|
|6| 10016,0 |99999,0|   ERROR |   NO| NO|     |64850,0|   11233,0|    140,0|
|7| 10017,0|    99999,0|    ERROR |   NO| NO| ENFR|   59933,0|    2417,0| 480,0|
|8| 10020,0|    99999,0|    ERROR |   NO| SV|     |80050,0|   16250,0|    80,0|
|9| 10030,0|    99999,0|    ERROR |   NO| SV|     |77000,0|   15500,0|    120,0|

In der vorhergehenden Ausgabe treten mehrere Fehler in den numerischen Spalten auf, weil Zahlen nicht korrekt analysiert wurden. Beim Schreiben in eine CSV-Datei werden Nullwerte standardmäßig durch die Zeichenfolge „ERROR“ ersetzt.

Fügen Sie Parameter als Teil Ihres Schreibaufrufs hinzu, und geben Sie eine Zeichenfolge an, die zur Darstellung von Nullwerten verwendet werden soll.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Der vorhergehende Code gibt folgende Ausgabe zurück:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000,0 |   99999,0 |   BadData |       NO|     NO  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003,0 |   99999,0 |   BadData |       NO|     NO  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010,0|    99999,0|    BadData |   NO| JN| ENJA|   70933,0|    –8667,0 |90,0|
|3| 10013,0|    99999,0|    BadData |   NO| NO| |   BadData|    BadData|    BadData|
|4| 10014,0|    99999,0|    BadData |   NO| NO| ENSO|   59783,0|    5350,0| 500,0|
|5| 10015,0|    99999,0|    BadData |   NO| NO| ENBL|   61383,0|    5867,0| 3270,0|
|6| 10016,0 |99999,0|   BadData |   NO| NO|     |64850,0|   11233,0|    140,0|
|7| 10017,0|    99999,0|    BadData |   NO| NO| ENFR|   59933,0|    2417,0| 480,0|
|8| 10020,0|    99999,0|    BadData |   NO| SV|     |80050,0|   16250,0|    80,0|
|9| 10030,0|    99999,0|    BadData |   NO| SV|     |77000,0|   15500,0|    120,0|

### <a name="parquet-file-example"></a>Beispiel für eine Parquet-Datei

Ähnlich wie Funktion `write_to_csv`, gibt Funktion `write_to_parquet` einen neuen Datenfluss zurück, der einen Schritt mit einem Schreibvorgang in eine Parquet-Datei enthält, der beim Ausführen des Datenflusses ausgeführt wird.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Führen Sie den Datenfluss aus, um den Schreibvorgang zu starten.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```

Der vorhergehende Code gibt folgende Ausgabe zurück:
|   |  Column1 |    Column2 | Column3 | Column4  |Column5   | Column6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000,0 |   99999,0 |   MiscreantData |       NO|     NO  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003,0 |   99999,0 |   MiscreantData |       NO|     NO  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010,0|    99999,0|    MiscreantData |   NO| JN| ENJA|   70933,0|    –8667,0 |90,0|
|3| 10013,0|    99999,0|    MiscreantData |   NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014,0|    99999,0|    MiscreantData |   NO| NO| ENSO|   59783,0|    5350,0| 500,0|
|5| 10015,0|    99999,0|    MiscreantData |   NO| NO| ENBL|   61383,0|    5867,0| 3270,0|
|6| 10016,0 |99999,0|   MiscreantData |   NO| NO|     |64850,0|   11233,0|    140,0|
|7| 10017,0|    99999,0|    MiscreantData |   NO| NO| ENFR|   59933,0|    2417,0| 480,0|
|8| 10020,0|    99999,0|    MiscreantData |   NO| SV|     |80050,0|   16250,0|    80,0|
|9| 10030,0|    99999,0|    MiscreantData |   NO| SV|     |77000,0|   15500,0|    120,0|

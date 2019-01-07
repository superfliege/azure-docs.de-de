---
title: 'Transformation: Datenvorbereitung mit dem Python SDK'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über das Transformieren und Bereinigen von Daten mit dem Azure Machine Learning Data Prep SDK. Verwenden Sie Transformationsmethoden, um Spalten hinzuzufügen, nicht benötigte Zeilen oder Spalten herauszufiltern und fehlende Werte zu imputieren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d32244cd49ebd42192b2388215f79a64cacb3caa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186139"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Transformieren von Daten mit dem Azure Machine Learning Data Prep SDK

In diesem Artikel lernen Sie verschiedene Methoden zum Laden von Daten mit dem [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) kennen. Das SDK enthält Funktionen zum einfachen Hinzufügen von Spalten, Herausfiltern von nicht benötigten Zeilen oder Spalten und Imputieren von fehlenden Werten.

Derzeit sind Funktionen für die folgenden Aufgaben verfügbar:

- [Hinzufügen einer Spalte mithilfe eines Ausdrucks](#column)
- [Imputieren von fehlenden Werten](#impute-missing-values)
- [Ableiten einer Spalte nach Beispiel](#derive-column-by-example)
- [Filterung](#filtering)
- [Benutzerdefinierte Python-Transformationen](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Hinzufügen einer Spalte mithilfe eines Ausdrucks

Das Azure Machine Learning Data Prep SDK enthält `substring`-Ausdrücke, mit denen Sie einen Wert aus vorhandenen Spalten berechnen und anschließend in eine neue Spalte einfügen können. In diesem Beispiel werden Daten geladen, und es wird versucht, diesen Eingabedaten Spalten hinzuzufügen.

```python
import azureml.dataprep as dprep

# loading data
dataflow = dprep.read_csv(path=r'data\crime0-10.csv')
dataflow.head(3)
```

||ID|Fallnummer|Datum|Block|IUCR|Primärer Typ|BESCHREIBUNG|Standortbeschreibung|Verhaftung|Inland|...|Bezirk|Gemeindebezirk|FBI-Code|x-Koordinate|y-Koordinate|Jahr|Aktualisiert am|Breitengrad|Längengrad|Standort|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|


Verwenden Sie den Ausdruck `substring(start, length)`, um das Präfix aus der Spalte „Fallnummer“ zu extrahieren, und fügen Sie diese Zeichenfolge in eine neue Spalte `Case Category` ein. Durch Übergeben der Variablen `substring_expression` an den Parameter `expression` wird eine neue berechnete Spalte erstellt, die den Ausdruck für jeden Datensatz ausführt.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dataflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||ID|Fallnummer|Fallkategorie|Datum|Block|IUCR|Primärer Typ|BESCHREIBUNG|Standortbeschreibung|Verhaftung|...|Bezirk|Gemeindebezirk|FBI-Code|x-Koordinate|y-Koordinate|Jahr|Aktualisiert am|Breitengrad|Längengrad|Standort|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|HY|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|HY|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|HY|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|



Verwenden Sie den Ausdruck `substring(start)`, um nur die Zahl aus der Spalte „Fallnummer“ zu extrahieren und eine neue Spalte zu erstellen. Konvertieren Sie sie mithilfe der Funktion `to_number()` in einen numerischen Datentyp, und übergeben Sie den Zeichenfolgenspaltennamen als Parameter.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dataflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
case_id.head(3)
```

||ID|Fallnummer|Fall-ID|Datum|Block|IUCR|Primärer Typ|BESCHREIBUNG|Standortbeschreibung|Verhaftung|...|Bezirk|Gemeindebezirk|FBI-Code|x-Koordinate|y-Koordinate|Jahr|Aktualisiert am|Breitengrad|Längengrad|Standort|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|329907.0|07/05/2015 11:50:00 PM|050XX N NEWLAND AVE|0820|THEFT|$500 AND UNDER|STREET|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466, -87.800174996)|
|1|10139776|HY329265|329265.0|07/05/2015 11:30:00 PM|011XX W MORSE AVE|0460|BATTERY|SIMPLE|STREET|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017, -87.65955018)|
|2|10140270|HY329253|329253.0|07/05/2015 11:20:00 PM|121XX S FRONT AVE|0486|BATTERY|DOMESTIC BATTERY SIMPLE|STREET|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|

## <a name="impute-missing-values"></a>Imputieren von fehlenden Werten

Das SDK kann fehlende Werte in den angegebenen Spalten imputieren. In diesem Beispiel laden Sie Werte für Breiten- und Längengrade und imputieren dann fehlende Werte in den Eingabedaten.

```python
import azureml.dataprep as dprep

# loading input data
df = dprep.read_csv(r'data\crime0-10.csv')
df = df.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
df = df.to_number(['Latitude', 'Longitude'])
df.head(5)
```

||ID|Verhaftung|Breitengrad|Längengrad|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Im dritten Datensatz fehlen Werte für Breiten- und Längengrade. Zum Imputieren dieser fehlenden Werte verwenden Sie `ImputeMissingValuesBuilder`, um einen festen Ausdruck zu lernen. Sie können die Spalten entweder mit einem berechneten `MIN`-, `MAX`- oder `MEAN`-Wert oder einem `CUSTOM`-Wert imputieren. Wenn `group_by_columns` angegeben ist, werden fehlende Werte nach Gruppe imputiert, wobei `MIN`, `MAX`, und `MEAN` pro Gruppe berechnet werden.

Überprüfen Sie den Wert `MEAN` der Spalte „Breitengrad“ mithilfe der Funktion `summarize()`. Diese Funktion akzeptiert ein Array von Spalten im Parameter `group_by_columns` zur Festlegung der Aggregationsebene. Der Parameter `summary_columns` akzeptiert einen `SummaryColumnsValue`-Aufruf. Mit diesem Funktionsaufruf wird der aktuelle Spaltenname, der Name des neuen berechneten Felds sowie die auszuführende `SummaryFunction` festlegt.

```python
df_mean = df.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
df_mean = df_mean.filter(dprep.col('Arrest') == 'false')
df_mean.head(1)
```

||Verhaftung|Latitude_MEAN|
|-----|-----|----|
|0|false|41.878961|

Der `MEAN`-Wert der Breitengrade sieht korrekt aus, verwenden Sie die Funktion `ImputeColumnArguments`, um ihn zu imputieren. Diese Funktion akzeptiert eine `column_id`-Zeichenfolge und eine `ReplaceValueFunction` zum Festlegen des Impute-Typs. Der fehlende Längengradwert wird basierend auf externen Informationen mit 42 imputiert.

Impute-Schritte können mit der Generatorfunktion `impute_missing_values()` zu einem `ImputeMissingValuesBuilder`-Objekt zusammengefügt werden. Der Parameter `impute_columns` akzeptiert ein Array von `ImputeColumnArguments`-Objekten. Rufen Sie die Funktion `learn()` auf, um die Impute-Schritte zu speichern und dann mit `to_dataflow()` auf ein Dataflow-Objekt anzuwenden.

```python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id='Latitude',
                                          impute_function=dprep.ReplaceValueFunction.MEAN)
# impute with custom value 42
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=42)
# get instance of ImputeMissingValuesBuilder
impute_builder = df.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
# call learn() to learn a fixed program to impute missing values
impute_builder.learn()
# call to_dataflow() to get a data flow with impute step added
df_imputed = impute_builder.to_dataflow()

# check impute result
df_imputed.head(5)
```

||ID|Verhaftung|Breitengrad|Längengrad|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Wie im oben genannten Ergebnis gezeigt wird, wurde der fehlende Breitengrad mit dem Wert `MEAN` der Gruppe `Arrest=='false'` imputiert. Der fehlende Längengrad wurde mit 42 imputiert.

```python
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Ableiten von Spalten anhand eines Beispiels

Zu den erweiterten Tools im Azure Machine Learning Data Prep SDK zählt die Möglichkeit, Spalten anhand von Beispielen für die gewünschten Ergebnisse abzuleiten. Auf diese Weise können Sie dem SDK ein Beispiel hinzufügen, damit es zur Erzielung der gewünschten Transformation Code generieren kann.

```python
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dataflow.head(10)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|
|4|1/1/2015 3:54|FM-15|24|46|13|
|5|1/1/2015 4:00|FM-12|24|46|13|
|6|1/1/2015 4:54|FM-15|22|52|15|
|7|1/1/2015 5:54|FM-15|23|48|17|
|8|1/1/2015 6:54|FM-15|23|50|14|
|9|1/1/2015 7:00|FM-12|23|50|14|

Es wird davon ausgegangen, dass Sie diese Datei mit einem Dataset verknüpfen müssen, in dem Datum und Uhrzeit im Format „Mar 10, 2018 | 2AM-4AM“ angegeben sind.

```python
builder = dataflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=df.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview() 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|Jan 1, 2015 12AM-2AM|
|1|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|
|2|1/1/2015 1:54|Jan 1, 2015 12AM-2AM|
|3|1/1/2015 2:54|Jan 1, 2015 2AM-4AM|
|4|1/1/2015 3:54|Jan 1, 2015 2AM-4AM|
|5|1/1/2015 4:00|Jan 1, 2015 4AM-6AM|
|6|1/1/2015 4:54|Jan 1, 2015 4AM-6AM|
|7|1/1/2015 5:54|Jan 1, 2015 4AM-6AM|
|8|1/1/2015 6:54|Jan 1, 2015 6AM-8AM|
|9|1/1/2015 7:00|Jan 1, 2015 6AM-8AM|

Der obige Code erstellt zunächst einen Generator für die abgeleitete Spalte. Sie geben ein Array der zu berücksichtigenden Quellspalten (`DATE`) und einen Namen für die neue Spalte an, die hinzugefügt werden soll. Im ersten Beispiel übergeben Sie die zweite Zeile (Index 1) und einen erwarteten Wert für die abgeleitete Spalte an.

Zu guter Letzt rufen Sie `builder.preview()` auf und können neben der Quellspalte die abgeleitete Spalte sehen. Das Format wirkt auf den ersten Blick richtig, Sie sehen jedoch nur Werte für das gleiche Datum „Jan 1, 2015“.

Übergeben Sie nun die Anzahl der Zeilen, die Sie vom Anfang aus überspringen möchten (`skip`), um die Zeilen weiter unten zu sehen.

```
builder.preview(skip=30)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|31|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|32|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|33|1/2/2015 0:54|Feb 1, 2015 12AM-2AM|
|34|1/2/2015 1:00|Feb 1, 2015 12AM-2AM|
|35|1/2/2015 1:54|Feb 1, 2015 12AM-2AM|
|36|1/2/2015 2:54|Feb 1, 2015 2AM-4AM|
|37|1/2/2015 3:54|Feb 1, 2015 2AM-4AM|
|38|1/2/2015 4:00|Feb 1, 2015 4AM-6AM|
|11,9|1/2/2015 4:54|Feb 1, 2015 4AM-6AM|

Hier können Sie ein Problem mit dem generierten Programm sehen: Das abgeleitete Programm, das ausschließlich auf dem oben angegebenen Beispiel basiert, hat das Datum im Format „Tag/Monat/Jahr“ analysiert, was in diesem Fall jedoch nicht gewünscht ist. Um dieses Problem zu beheben, geben Sie ein weiteres Beispiel unter Verwendung der Funktion `add_example()` mit der `builder`-Variablen an.

```python
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|31|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|32|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|33|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|
|34|1/2/2015 1:00|Jan 2, 2015 12AM-2AM|
|35|1/2/2015 1:54|Jan 2, 2015 12AM-2AM|
|36|1/2/2015 2:54|Jan 2, 2015 2AM-4AM|
|37|1/2/2015 3:54|Jan 2, 2015 2AM-4AM|
|38|1/2/2015 4:00|Jan 2, 2015 4AM-6AM|
|11,9|1/2/2015 4:54|Jan 2, 2015 4AM-6AM|

Nun verarbeiten die Zeilen „1/2/2015“ ordnungsgemäß als „Jan 2, 2015“. Wenn Sie sich jedoch den Bereich weiter unten in der abgeleiteten Spalte ansehen, sehen Sie, dass am Ende der abgeleiteten Spalte keine Werte angezeigt werden. Um dies zu beheben, müssen Sie ein weiteres Beispiel für Zeile 66 angeben.

```python
builder.add_example(source_data=preview_df.iloc[66], example_value='Jan 29, 2015 8PM-10PM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Jan 1, 2015 10PM-12AM|
|1|1/1/2015 23:54|Jan 1, 2015 10PM-12AM|
|2|1/1/2015 23:59|Jan 1, 2015 10PM-12AM|
|3|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|
|4|1/2/2015 1:00|Jan 2, 2015 12AM-2AM|
|5|1/2/2015 1:54|Jan 2, 2015 12AM-2AM|
|6|1/2/2015 2:54|Jan 2, 2015 2AM-4AM|
|7|1/2/2015 3:54|Jan 2, 2015 2AM-4AM|
|8|1/2/2015 4:00|Jan 2, 2015 4AM-6AM|
|9|1/2/2015 4:54|Jan 2, 2015 4AM-6AM|

Zum Trennen von Datum und Uhrzeit mit „|“ fügen Sie ein weiteres Beispiel hinzu. Anstatt Werte in eine Zeile aus der Vorschau zu übergeben, erstellen Sie dieses Mal ein Wörterbuch mit einer Name-Wert-Spalte für den Parameter `source_data`.

```python
builder.add_example(source_data={'DATE': '11/11/2015 0:54'}, example_value='Nov 11, 2015 | 12AM-2AM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|Keine|
|1|1/1/2015 23:54|Keine|
|2|1/1/2015 23:59|Keine|
|3|1/2/2015 0:54|Keine|
|4|1/2/2015 1:00|Keine|
|5|1/2/2015 1:54|Keine|
|6|1/2/2015 2:54|Keine|
|7|1/2/2015 3:54|Keine|
|8|1/2/2015 4:00|Keine|
|9|1/2/2015 4:54|Keine|

Dies hatte eindeutig negative Auswirkungen, da nun die einzigen Zeilen, die Werte in der abgeleiteten Spalte enthalten, die Zeilen sind, die genau den angegebenen Beispielen entsprechen. Rufen Sie `list_examples()` für das Generatorobjekt auf, um eine Liste der aktuellen Beispielableitungen anzuzeigen.

```python
examples = builder.list_examples()
```

| |DATE|Beispiel|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|Jan 1, 2015 12AM-2AM|-1|
|1|1/2/2015 0:54|Jan 2, 2015 12AM-2AM|-2|
|2|1/29/2015 20:54|Jan 29, 2015 8PM-10PM|-3|
|3|11/11/2015 0:54|Nov 11, 2015 \| 12AM-2AM|-4|

In diesem Fall wurden inkonsistente Beispiele bereitgestellt. Um das Problem zu beheben, ersetzen Sie die ersten drei Beispiele durch richtige Beispiele (einschließlich „|“ zwischen Datum und Uhrzeit einfügen).

Korrigieren Sie inkonsistente Beispiele, indem Sie die fehlerhaften Beispiele löschen (entweder durch Übergeben von `example_row` aus dem Pandas-DataFrame oder durch Übergeben des Werts `example_id`) und neue Beispiele hinzufügen.

```python
builder.delete_example(example_id=-1)
builder.delete_example(example_row=examples.iloc[1])
builder.delete_example(example_row=examples.iloc[2])
builder.add_example(examples.iloc[0], 'Jan 1, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[1], 'Jan 2, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[2], 'Jan 29, 2015 | 8PM-10PM')
builder.preview()
```

| | DATE | date_timerange |
| -------- | -------- | -------- |
| 0 | 1/1/2015 0:54 | Jan 1, 2015 \| 12AM-2AM |
| 1 | 1/1/2015 1:00 | Jan 1, 2015 \| 12AM-2AM |
| 2 | 1/1/2015 1:54 | Jan 1, 2015 \| 12AM-2AM |
| 3 | 1/1/2015 2:54 | Jan 1, 2015 \| 2AM-4AM |
| 4 | 1/1/2015 3:54 | Jan 1, 2015 \| 2AM-4AM |
| 5 | 1/1/2015 4:00 | Jan 1, 2015 \| 4AM-6AM|
| 6 | 1/1/2015 4:54 | Jan 1, 2015 \| 4AM-6AM|
| 7 | 1/1/2015 5:54 | Jan 1, 2015 \| 4AM-6AM|
| 8 | 1/1/2015 6:54 | Jan 1, 2015 \| 6AM-8AM|
| 9 | 1/1/2015 7:00 | Jan 1, 2015 \| 6AM-8AM|

Nun sehen die Daten einwandfrei aus, und Sie können im Generator `to_dataflow()` aufrufen, wodurch ein Datenfluss zurückgegeben wird, dem die gewünschten abgeleiteten Spalten hinzugefügt wurden.

```python
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filterung

Das SDK enthält die Methoden `Dataflow.drop_columns` und `Dataflow.filter`, mit denen Sie Spalten oder Zeilen herausfiltern können.

### <a name="initial-setup"></a>Anfangssetup

```python
import azureml.dataprep as dprep
from datetime import datetime
dataflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Filtern von Spalten

Um Spalten zu filtern, verwenden Sie `Dataflow.drop_columns`. Diese Methode akzeptiert eine Liste von zu löschenden Spalten oder ein komplexeres Argument namens `ColumnSelector`.

#### <a name="filtering-columns-with-list-of-strings"></a>Filtern von Spalten mit einer Liste von Zeichenfolgen

In diesem Beispiel akzeptiert `drop_columns` eine Liste von Zeichenfolgen. Jede Zeichenfolge sollte exakt der zu löschenden Spalte entsprechen.

```python
dataflow = dataflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|0|0|0|0|1|.00|0|0|3.25|

#### <a name="filtering-columns-with-regex"></a>Filtern von Spalten mit RegEx

Alternativ können Sie den Ausdruck `ColumnSelector` verwenden, um Spalten zu löschen, die einem RegEx-Ausdruck entsprechen. In diesem Beispiel löschen Sie alle Spalten, die dem Ausdruck `Column*|.*longitude|.*latitude` entsprechen.

```python
dataflow = dataflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|.00|0|0|21.25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|1|.00|0|0|3.25|

## <a name="filtering-rows"></a>Filtern von Zeilen

Verwenden Sie zum Filtern von Zeilen `DataFlow.filter`. Bei dieser Methode wird ein Azure Machine Learning Data Prep SDK-Ausdruck als Argument verwendet und ein neuer Datenfluss mit den Zeilen zurückgegeben, die der Ausdruck als „True“ auswertet. Ausdrücke werden mithilfe von Ausdrucks-Generatoren (`col`, `f_not`, `f_and`, `f_or`) und regulären Operatoren (>, <, >=, <=, ==, !=) erstellt.

### <a name="filtering-rows-with-simple-expressions"></a>Filtern von Zeilen mit einfachen Ausdrücken

Verwenden Sie den Ausdrucksgenerator `col`, und geben Sie den Namen der Spalte als Zeichenfolgenargument `col('column_name')` an. Verwenden Sie diesen Ausdruck in Verbindung mit einer der folgenden Standardoperatoren (>, <, >=, <=, ==, !=), um einen Ausdruck wie z. B. `col('Tip_amount') > 0` zu erstellen. Übergeben Sie abschließend den erstellten Ausdruck an die Funktion `Dataflow.filter`.

In diesem Beispiel gibt `dataflow.filter(col('Tip_amount') > 0)` einen neuen Datenfluss mit den Zeilen zurück, bei denen der Wert von `Tip_amount` größer als 0 ist.

> [!NOTE] 
> `Tip_amount` wird zuerst in einen numerischen Wert konvertiert, damit wir einen Ausdruck zum Vergleich mit anderen numerischen Werten erstellen können.

```python
dataflow = dataflow.to_number(['Tip_amount'])
dataflow = dataflow.filter(dprep.col('Tip_amount') > 0)
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0.08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|.00|0,30|0|3.8|
|2|2013-08-05 14:11:42|2013-08-05 14:12:47|1|.00|1.05|0|4.55|
|3|2013-08-05 14:15:56|2013-08-05 14:18:04|5|.00|2.22|0|5.72|
|4|2013-08-05 14:42:14|2013-08-05 14:42:38|1|.00|0.88|0|4.38|

### <a name="filtering-rows-with-complex-expressions"></a>Filtern von Zeilen mit komplexen Ausdrücken

Um anhand von komplexen Ausdrücken zu filtern, kombinieren Sie mindestens einen einfachen Ausdruck mit den Ausdrucks-Generatoren `f_not`, `f_and` oder `f_or`.

In diesem Beispiel gibt `Dataflow.filter` einen neuen Datenfluss mit den Zeilen zurück, bei denen `'Passenger_count'` kleiner als 5 und `'Tolls_amount'` größer als 0 ist.

```python
dataflow = dataflow.to_number(['Passenger_count', 'Tolls_amount'])
dataflow = dataflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:16:00|2013-08-08 12:16:00|1.0|.00|2,25|5.00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5.28|6.46|5.33|32.29|
|2|2013-08-12 19:48:12|2013-08-12 20:03:42|1.0|5.50|1.00|10.66|30.66|
|3|2013-08-13 06:11:06|2013-08-13 06:30:28|1.0|9.57|7.47|5.33|44.8|
|4|2013-08-16 20:33:50|2013-08-16 20:48:50|1.0|5.63|3.00|5.33|27.83|

Es ist auch möglich, Zeilen mit mehr als einem Ausdrucks-Generator zu filtern, um einen geschachtelten Ausdruck zu erstellen.

> [!NOTE]
> `lpep_pickup_datetime` und `Lpep_dropoff_datetime` werden zuerst in einen datetime-Wert konvertiert, damit wir einen Ausdruck zum Vergleich mit anderen datetime-Werten erstellen können.

```python
dataflow = dataflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dataflow = dataflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dataflow = dataflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-13 06:11:06+00:00|2013-08-13 06:30:28+00:00|1.0|9.57|7.47|5.33|44.80|
|1|2013-08-23 12:28:20+00:00|2013-08-23 12:50:28+00:00|2.0|8,22|8.08|5.33|40.41|
|2|2013-08-25 09:12:52+00:00|2013-08-25 09:34:34+00:00|1.0|8.80|8.33|5.33|41.66|
|3|2013-08-25 16:46:51+00:00|2013-08-25 17:13:55+00:00|2.0|9.66|7.37|5.33|44.20|
|4|2013-08-25 17:42:11+00:00|2013-08-25 18:02:57+00:00|1.0|9.60|6.87|5.33|41.20|

## <a name="custom-python-transforms"></a>Benutzerdefinierte Python-Transformationen

Es wird immer Szenarien geben, in denen die einfachste Möglichkeit, eine Transformation durchzuführen, das Schreiben eines eigenen Skripts ist. Das SDK bietet drei Erweiterungspunkte, die Sie für benutzerdefinierte Python-Scripts verwenden können.

- Neue Skriptspalte
- Neuer Skriptfilter
- Transformieren der Partition

Jede der Erweiterungen wird in der Laufzeit zum zentralen Hochskalieren und horizontalen Skalieren unterstützt. Ein wichtiger Vorteil bei der Verwendung dieser Erweiterungspunkte ist, dass Sie nicht alle Daten abrufen müssen, um einen Datenframe zu erstellen. Ihr benutzerdefinierter Python-Code wird wie bei anderen Transformationen im großen Maßstab, partitionsweise und in der Regel parallel ausgeführt.

### <a name="initial-data-preparation"></a>Anfängliche Datenvorbereitung

Laden Sie zunächst einige Daten aus Azure Blob Storage.

```python
import azureml.dataprep as dprep
col = dprep.col

df = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
df.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158| |
|1|ALABAMA|1|101710|Hale County|10171002162| |
|2|ALABAMA|1|101710|Hale County|10171002156| |
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589| |

Kürzen Sie das Dataset, und führen Sie einige grundlegende Transformationen durch.

```python
df = df.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
df = df.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
df = df.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
df.head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|Keine|
|1|ALABAMA|Hale County|1.017100e+10|Keine|
|2|ALABAMA|Hale County|1.017100e+10|Keine|
|3|ALABAMA|Hale County|1.017100e+10|2|
|4|ALABAMA|Hale County|1.017100e+10|Keine|

Suchen Sie mithilfe des folgenden Filters nach NULL-Werten.

```python
df.filter(col('MAM_MTH00numvalid_1011').is_null()).head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|Keine|
|1|ALABAMA|Hale County|1.017100e+10|Keine|
|2|ALABAMA|Hale County|1.017100e+10|Keine|
|3|ALABAMA|Hale County|1.017100e+10|Keine|
|4|ALABAMA|Hale County|1.017100e+10|Keine|

### <a name="transform-partition"></a>Transformieren der Partition

Verwenden Sie eine Pandas-Funktion, um alle NULL-Werte durch „0“ zu ersetzen. Dieser Code wird nicht für das gesamte Dataset gleichzeitig ausgeführt, sondern partitionsweise. Das heißt, dass dieser Code bei einem großen Dataset parallel ausgeführt werden kann, während die Laufzeit die Daten partitionsweise verarbeitet.

Das Python-Skript muss eine Funktion namens `transform()` definieren, die zwei Argumente akzeptiert, `df` und `index`. Das Argument `df` ist ein Pandas-Datenrahmen, der die Daten für die Partition enthält, und das Argument `index` ist ein eindeutiger Bezeichner der Partition. Die Transform-Funktion kann die im Datenrahmen übergebenen Daten vollständig bearbeiten, muss aber einen Datenrahmen zurückgeben. Alle Bibliotheken, die das Python-Skript importiert, müssen in der Umgebung vorhanden sein, in der der Dataflow ausgeführt wird.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(5)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|0.0|
|1|ALABAMA|Hale County|1.017100e+10|0.0|
|2|ALABAMA|Hale County|1.017100e+10|0.0|
|3|ALABAMA|Hale County|1.017100e+10|2.0|
|4|ALABAMA|Hale County|1.017100e+10|0.0|

### <a name="new-script-column"></a>Neue Skriptspalte

Mit dem Python-Code können Sie eine neue Spalte erstellen, die den Namen des Countys und den des Bundesstaats enthält, und den Namen des Bundesstaats großschreiben. Verwenden Sie hierfür die Methode `new_script_column()` für den Datenfluss.

Das Python-Skript muss eine Funktion namens `newvalue()` definieren, die ein einzelnes `row`-Argument akzeptiert. Das Argument `row` ist ein Wörterbuch (`key`:Spaltenname, `val`: aktueller Wert) und wird für jede Zeile im Dataset an diese Funktion übergeben. Diese Funktion muss einen Wert zurückgeben, der in der neuen Spalte verwendet wird. Alle Bibliotheken, die das Python-Skript importiert, müssen in der Umgebung vorhanden sein, in der der Dataflow ausgeführt wird.

```python
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|1|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|2|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|
|3|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|2.0|
|4|ALABAMA|Hale County|Hale County, Alabama|1.017100e+10|0.0|

### <a name="new-script-filter"></a>Neuer Skriptfilter

Erstellen Sie einen Python-Ausdruck, um das Dataset nur auf die Zeilen zu filtern, in denen „Hale“ nicht in der neuen Spalte `county_state` enthalten ist. Der Ausdruck gibt `True` zurück, wenn die Zeile beibehalten werden soll, und `False`, um die Zeile zu löschen.

```python
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e+10|1.0|
|1|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e+10|0.0|
|2|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e+10|0.0|
|3|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e+10|0.0|
|4|ALABAMA|Jefferson County|Jefferson County, Alabama|1.019200e+10|0.0|

---
title: DateTime-Pfadmuster für die Azure Stream Analytics-Blobausgabe (Vorschau)
description: Dieser Artikel beschreibt das Feature für benutzerdefinierte DateTime-Pfadmuster für die Blobspeicherausgabe von Azure Stream Analytics-Aufträgen.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: ba386539c3f3c6740b843575bbccd4b028b8a5a7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090777"
---
# <a name="custom-datetime-path-patterns-for-azure-stream-analytics-blob-storage-output-preview"></a>Benutzerdefinierte DateTime-Pfadmuster für die Blob Storage-Ausgabe von Azure Stream Analytics (Vorschau)

Azure Stream Analytics unterstützt benutzerdefinierte Datums- und Zeitformatbezeichner im Dateipfad für Blob Storage-Ausgaben. Mit benutzerdefinierten DateTime-Pfadmustern können Sie ein Ausgabeformat festlegen, das mit den Hive Streaming-Konventionen übereinstimmt, damit Azure Stream Analytics Daten an Azure HDInsight und Azure Databricks zur weiteren Verarbeitung senden kann. Benutzerdefinierte DateTime-Pfadmuster können einfach mit dem Schlüsselwort `datetime` im Feld „Pfadpräfix“ der Blobausgabe und dem Formatbezeichner implementiert werden. Beispiel: `{datetime:yyyy}`.

Verwenden Sie diesen Link für das [Azure-Portal](https://portal.azure.com/?Microsoft_Azure_StreamAnalytics_bloboutputcustomdatetimeformats=true), um das Featureflag umzuschalten, das die benutzerdefinierten DateTime-Pfadmuster für die Vorschau der Blob Storage-Ausgabe aktiviert. Dieses Feature wird in Kürze im Hauptportal aktiviert.

## <a name="supported-tokens"></a>Unterstützte Token

Die folgenden Formatbezeichnertoken können einzeln oder in Kombination verwendet werden, um benutzerdefinierte DateTime-Formate zu erhalten:

|Formatbezeichner   |BESCHREIBUNG   |Ergebnisse zur Beispielzeit 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|Das Jahr als vierstellige Zahl|2018|
|{datetime:MM}|Monat von 01 bis 12|01|
|{datetime:M}|Monat von 1 bis 12|1|
|{datetime:dd}|Tag von 01 bis 31|02|
|{datetime:d}|Tag von 1 bis 12|2|
|{datetime:HH}|Stunde im 24-Stunden-Format, von 00 bis 23|10|
|{datetime:mm}|Minuten von 00 bis 24|06|
|{datetime:m}|Minuten von 0 bis 24|6|
|{datetime:ss}|Sekunden von 00 bis 60|08|

Wenn Sie keine benutzerdefinierten DateTime-Muster verwenden möchten, können Sie das Token {date} und/oder {time} zum Pfadpräfix hinzufügen, um eine Dropdownliste mit integrierten DateTime-Formaten zu generieren.

![Alte DateTime-Formate von Stream Analytics](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

## <a name="extensibility-and-restrictions"></a>Erweiterbarkeit und Einschränkungen

Sie können beliebig viele Token, `{datetime:<specifier>}`, im Pfadmuster verwenden, bis Sie den Zeichengrenzwert für das Pfadpräfix erreichen. Formatbezeichner können innerhalb eines einzelnen Tokens nicht über die bereits in den Dropdownlisten für Datum und Uhrzeit aufgeführten Kombinationen hinaus kombiniert werden. 

Für eine Pfadpartition von `logs/MM/dd`:

|Gültiger Ausdruck   |Ungültiger Ausdruck   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Sie können denselben Formatbezeichner mehrmals im Pfadpräfix verwenden. Das Token muss jedes Mal wiederholt werden.

## <a name="hive-streaming-conventions"></a>Hive-Streaming-Konventionen

Benutzerdefinierte Pfadmuster für Blob Storage können mit der Hive-Streaming-Konvention verwendet werden, die erwartet, dass Ordner im Ordnernamen mit `column=` gekennzeichnet sind.

Beispiel: `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Die benutzerdefinierte Ausgabe erübrigt das mühevolle Ändern von Tabellen und das manuelle Hinzufügen von Partitionen zu Portdaten zwischen Azure Stream Analytics und Hive. Stattdessen können viele Ordner automatisch wie folgt hinzugefügt werden:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Beispiel

Erstellen Sie ein Speicherkonto, eine Ressourcengruppe, einen Stream Analytics-Auftrag und eine Eingabequelle gemäß der Schnellstartanleitung [Azure Stream Analytics Azure-Portal](stream-analytics-quick-create-portal.md). Verwenden Sie dieselben Beispieldaten wie in der Schnellstartanleitung, die auch auf [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) verfügbar sind.

Erstellen Sie eine Blobausgabesenke mit der folgenden Konfiguration:

![Stream Analytics erstellt Blobausgabesenke](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Das vollständige Pfadmuster sieht wie folgt aus:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


Wenn Sie den Auftrag starten, wird in Ihrem Blobcontainer eine Ordnerstruktur basierend auf dem Pfadmuster erstellt. Sie können einen Drilldown bis zur Tagesebene ausführen.

![Stream Analytics-Blobausgabe mit benutzerdefiniertem Pfadmuster](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zu den Ausgaben von Azure Stream Analytics](stream-analytics-define-outputs.md)

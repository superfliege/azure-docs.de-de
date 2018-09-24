---
title: Arbeiten mit Werten für Datum und Uhrzeit in Azure Log Analytics-Abfragen | Microsoft-Dokumentation
description: Beschreibt das Arbeiten mit Daten für Datum und Uhrzeit in Log Analytics-Abfragen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 9b0c58fdbfb0d55b3b8998f4edfc1222b9a3d4aa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988597"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Arbeiten mit Werten für Datum und Uhrzeit in Azure Log Analytics-Abfragen

> [!NOTE]
> Vor der Durchführung dieser Lektion sollten Sie [Erste Schritte mit dem Analytics-Portal](get-started-analytics-portal.md) und [Erste Schritte mit Abfragen](get-started-queries.md) lesen.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

In diesem Artikel wird das Arbeiten mit Daten für Datum und Uhrzeit in Log Analytics-Abfragen beschrieben.


## <a name="date-time-basics"></a>Grundlagen zu Datum und Uhrzeit
Die Log Analytics-Abfragesprache verfügt über zwei Hauptdatentypen, die Datums- und Uhrzeitangaben zugeordnet sind: datetime und timespan. Alle Datumsangaben werden in UTC ausgedrückt. Es werden zwar mehrere datetime-Formate unterstützt, jedoch wird das ISO8601-Format bevorzugt. 

Zeiträume werden als Dezimalwert gefolgt von einer Zeiteinheit ausgedrückt:

|Kurzform   | Zeiteinheit    |
|:---|:---|
|d           | day          |
|h.           | hour         |
|m           | Minute       |
|s           | Sekunde       |
|ms          | Millisekunde  |
|Mikrosekunde | Mikrosekunde  |
|Takt        | Nanosekunde   |

datetime-Werte können durch Umwandeln einer Zeichenfolge mithilfe des Operators `todatetime` erstellt werden. Zur Überprüfung der in einem bestimmten Zeitraum gesendeten VM-Heartbeats können Sie beispielsweise den[Operator „between“](https://docs.loganalytics.io/docs/Language-Reference/Scalar-operators/between-operator) verwenden, der sich für die Angabe eines bestimmten Zeitraums eignet...

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Ein weiteres gängiges Szenario besteht in dem Vergleich eines datetime-Werts mit dem vorhandenen Wert. Wenn Sie beispielsweise sämtliche Heartbeats der letzten zwei Minuten anzeigen möchten, können Sie den Operator `now` zusammen mit einem timespan-Wert verwenden, der zwei Minuten darstellt:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Für diese Funktion ist auch eine Verknüpfung verfügbar:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

Die kürzeste und am besten lesbare Methode besteht jedoch in der Verwendung des Operators `ago`:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Angenommen, Ihnen ist die Start- und Endzeit nicht bekannt, sondern die Startzeit und die Dauer. Sie können die Abfrage wie folgt erneut generieren:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Konvertieren von Zeiteinheiten
Es kann hilfreich sein, einen datetime- oder timespan-Wert in einer nicht standardmäßigen Zeiteinheit auszudrücken. Angenommen beispielsweise, Sie überprüfen Fehlerereignisse der letzten 30 Minuten und benötigen eine berechnete Spalte, in der angezeigt wird, wann das Ereignis aufgetreten ist:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Sie können sehen, dass die Spalte _timeAgo_ Werte wie z.B. „00:09:31.5118992“ enthält. Diese sind demnach wie folgt formatiert: hh:mm:ss.fffffff. Wenn Sie diese Werte in _numver_ von Minuten seit der Startzeit formatieren möchten, müssen Sie diesen Wert einfach durch „1 Minute“ teilen:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Aggregationen und Zuordnung von Buckets nach Zeitintervallen
Ein weiteres sehr gängiges Szenario besteht in der Notwendigkeit, Statistiken über einen bestimmten Zeitraum in einem bestimmten Aggregationsintervall abzurufen. Hierzu kann ein `bin`-Operator als Teil einer -Operator kann als Teil einer Summarize-Klausel verwendet werden.

Mit der folgenden Abfrage können Sie die Anzahl der Ereignisse abrufen, die in der letzten halben Stunde alle 5 Minuten aufgetreten sind:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Dadurch wird die folgende Tabelle erzeugt:  
|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Eine weitere Möglichkeit zum Erstellen von Buckets für die Ergebnisse besteht in der Verwendung von Funktionen, wie z.B. `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Dadurch werden die folgenden Ergebnisse erzeugt:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7.136|
|2018-07-29T00:00:00.000|12.315|
|2018-07-30T00:00:00.000|16.847|
|2018-07-31T00:00:00.000|12.616|
|2018-08-01T00:00:00.000|5.416  |


## <a name="time-zones"></a>Zeitzonen
Da alle datetime-Werte in UTC ausgedrückt werden, ist es häufig hilfreich, diese Werte in die lokale Zeitzone zu konvertieren. Verwenden Sie beispielsweise die folgende Berechnung zum Konvertieren von UTC- in PST-Zeiten:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Zugehörige Funktionen

| Category (Kategorie) | Funktion |
|:---|:---|
| Konvertieren von Datentypen | [todatetime](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/todatetime())  [totimespan](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/totimespan())  |
| Runden eines Werts auf die bin-Größe | [bin](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/bin()) |
| Abrufen einer bestimmten Datums oder einer bestimmten Uhrzeit | [ago](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago()) [now](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/now())   |
| Abrufen eines Teils des Werts | [datetime_part](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/datetime_part()) [getmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getmonth()) [monthofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/monthofyear()) [getyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getyear()) [dayofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofmonth()) [dayofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofweek()) [dayofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofyear()) [weekofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/weekofyear()) |
| Abrufen eines Datums relativ zum Wert  | [endofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofday()) [endofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofweek()) [endofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofmonth()) [endofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofyear()) [startofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofday()) [startofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofweek()) [startofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofmonth()) [startofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofyear()) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung der Log Analytics-Abfragesprache finden Sie in folgenden weiteren Lektionen:

- [Zeichenfolgenvorgänge](string-operations.md)
- [Aggregationsfunktionen](aggregations.md)
- [Erweiterte Aggregationen](advanced-aggregations.md)
- [JSON und Datenstrukturen](json-data-structures.md)
- [Schreiben von erweiterten Abfragen](advanced-query-writing.md)
- [Verknüpfungen](joins.md)
- [Diagramme](charts.md)
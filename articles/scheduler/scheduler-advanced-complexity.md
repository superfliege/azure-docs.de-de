---
title: Erstellen komplexer Zeitpläne und erweiterter Serien mit Azure Scheduler
description: Erfahren Sie, wie komplexe Zeitpläne und erweiterte Serien mit Azure Scheduler erstellt werden.
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4293442e13fc4bae871b1f32a3ed4231d9f32632
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
ms.locfileid: "29692333"
---
# <a name="build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Erstellen komplexer Zeitpläne und erweiterter Serien mit Azure Scheduler

Das Herzstück eines Azure Scheduler-Auftrags ist der Zeitplan. Der Zeitplan bestimmt, wann und wie Scheduler den Auftrag ausführt. 

Mit Scheduler können Sie mehrere einmalige Ausführungen und Zeitplanserien für einen Auftrag festlegen. Einmalige Zeitpläne werden ein Mal zu einem angegebenen Zeitpunkt ausgelöst. Einmalige Zeitpläne sind effektiv Zeitplanserien, die nur ein Mal ausgeführt werden. Wiederholungszeitpläne werden gemäß einer vorgegebenen Häufigkeit ausgelöst.

Durch diese Flexibilität können Sie Scheduler für eine Vielzahl von Geschäftsszenarien verwenden:

* **Periodische Datenbereinigung**. Löschen Sie beispielsweise jeden Tag alle Tweets, die älter als drei Monate sind.
* **Archivierung**. Beispielsweise können Sie jeden Monat den Rechnungsverlauf an einen Sicherungsdienst pushen.
* **Anforderungen für externe Daten**. Sie können z.B. alle 15 Minuten einen neuen Wintersport-Wetterbericht von NOAA abrufen.
* **Bildverarbeitung**. Sie können z.B. die hochgeladenen Bilder des aktuellen Tages mithilfe von Cloud Computing an jedem Wochentag außerhalb der Spitzenzeiten komprimieren.

Dieser Artikel enthält Beispielaufträge, die Sie mit Scheduler erstellen können. Hier finden Sie auch die JSON-Daten zur Beschreibung der einzelnen Zeitpläne. Bei Verwendung der [Scheduler-REST-API](https://msdn.microsoft.com/library/mt629143.aspx) können Sie diese JSON-Daten zum [Erstellen eines Scheduler-Auftrags](https://msdn.microsoft.com/library/mt629145.aspx) verwenden.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Beispiele in diesem Artikel veranschaulichen die Bandbreite der Szenarien, die Scheduler unterstützt. Die Beispiele veranschaulichen in groben Zügen, wie Zeitpläne für viele Verhaltensmuster erstellt werden können:

* Einmalige Ausführung zu einem bestimmten Zeitpunkt (Datum und Uhrzeit).
* Ausführung und Ausführungsserie für eine bestimmte Anzahl von Malen.
* Sofortige Ausführung und Ausführungsserie.
* Ausführung ab einem bestimmten Zeitpunkt und Ausführungsserie alle *n* Minuten, Stunden, Tage, Wochen oder Monate.
* Ausführung und wöchentliche oder monatliche Ausführungsserie, jedoch nur an bestimmten Wochentagen oder Tagen des Monats.
* Ausführung und Ausführungsserie mehrere Male in einem bestimmten Zeitraum. Zum Beispiel am letzten Freitag und letzten Montag eines jeden Monats, oder jeden Tag um 05:15 Uhr und um 17:15 Uhr.

## <a name="date-and-date-time"></a>Datum und Datum/Uhrzeit
Datumsangaben in Scheduler-Aufträgen entsprechen der [ISO-8601-Spezifikation](http://en.wikipedia.org/wiki/ISO_8601) und enthalten nur das Datum.

Datums-/Uhrzeitangaben in Scheduler-Aufträgen entsprechen der [ISO-8601-Spezifikation](http://en.wikipedia.org/wiki/ISO_8601) und enthalten ein Datum und eine Uhrzeit. Eine Datums-/Uhrzeitangabe ohne UTC-Offset wird als UTC-Angabe interpretiert.  

## <a name="use-json-and-the-rest-api-to-create-a-schedule"></a>Verwenden von JSON und der REST-API zum Erstellen eines Zeitplans
Um einen grundlegenden Zeitplan mithilfe der [Scheduler-REST-API](https://msdn.microsoft.com/library/mt629143) zu erstellen, [registrieren Sie Ihr Abonnement zunächst bei einem Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790548.aspx). Der Anbietername für Scheduler ist **Microsoft.Scheduler**. [Erstellen Sie dann eine Auftragssammlung](https://msdn.microsoft.com/library/mt629159.aspx). Schließlich [erstellen Sie einen Auftrag](https://msdn.microsoft.com/library/mt629145.aspx). 

Bei der Auftragserstellung können Sie mithilfe von JSON den Zeitplan und die Ausführungsserie angeben, wie der folgende Auszug zeigt:

    {
        "startTime": "2012-08-04T00:00Z", // Optional
         …
        "recurrence":                     // Optional
        {
            "frequency": "week",     // Can be "year", "month", "day", "week", "hour", or "minute"
            "interval": 1,                // How often to fire
            "schedule":                   // Optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // Optional (default to recur infinitely)
            "endTime": "2012-11-04",      // Optional (default to recur infinitely)
        },
        …
    }

## <a name="job-schema-basics"></a>Grundlagen zum Auftragsschema
Die folgende Tabelle enthält eine allgemeine Übersicht über die wichtigsten Elemente für das Festlegen der Ausführungsserie und Zeitplanung in einem Auftrag:

| JSON-Name | BESCHREIBUNG |
|:--- |:--- |
| **startTime** |Ein Datums-/Uhrzeitwert. Bei einfachen Zeitplänen ist **startTime** das erste Vorkommen. Bei komplexen Zeitplänen wird der Auftrag frühestens bei **startTime** gestartet. |
| **recurrence** |Gibt die Wiederholungsregeln für den Auftrag und die Wiederholung an, mit der der Auftrag ausgeführt wird. Dass recurrence-Objekt unterstützt die Elemente **frequency**, **interval**, **endTime**, **count** und **schedule**. Wenn **recurrence** definiert ist, ist **frequency** erforderlich. Andere **recurrence**-Elemente sind optional. |
| **frequency** |Eine Zeichenfolge, die die Häufigkeitseinheit für die Auftragsserie darstellt. Unterstützte Werte sind „minute“, „hour“, „day“, „week“ und „month“. |
| **interval** |Eine positive ganze Zahl **interval** gibt das Intervall für den Wert **frequency** an, der bestimmt, wie oft der Auftrag ausgeführt wird. Wenn **interval** beispielsweise auf „3“ und **frequency** auf „week“ festgelegt ist, wird der Auftrag alle drei Wochen ausgeführt.<br /><br />Scheduler unterstützt für **interval** maximal 18 Monate bei monatlicher Häufigkeit, 78 Wochen bei wöchentlicher Häufigkeit bzw. 548 Tage (bei täglicher Häufigkeit). Bei Stunden und Minuten wird folgender Bereich unterstützt: 1 <= **interval** <= 1000. |
| **endTime** |Eine Zeichenfolge, die das Datum und die Uhrzeit angibt, ab dem bzw. der der Auftrag nicht mehr ausgeführt wird. Sie können einen Wert für **endTime** festlegen, der in der Vergangenheit liegt. Ohne Angabe von **endTime** oder **count** wird der Auftrag unendlich lange ausgeführt. Sie können **endTime** und **count** nicht im gleichen Auftrag verwenden. |
| **count** |Eine positive ganze Zahl (größer als null), die angibt, wie oft dieser Auftrag ausgeführt wird, bevor er abgeschlossen ist.<br /><br />**count** stellt die Anzahl der Ausführungen des Auftrags dar, bevor er als abgeschlossen betrachtet wird. So ist beispielsweise ein täglich auszuführender Auftrag, für den **count** auf „5“ und das Startdatum auf Montag festgelegt ist, nach der Ausführung am Freitag abgeschlossen. Liegt das Startdatum in der Vergangenheit, wird die erste Ausführung auf der Grundlage des Erstellungszeitpunkts berechnet.<br /><br />Ohne Angabe von **endTime** oder **count** wird der Auftrag unendlich ausgeführt. Sie können **endTime** und **count** nicht im gleichen Auftrag verwenden. |
| **schedule** |Die Wiederholung eines Auftrags mit einer bestimmten Häufigkeit wird auf der Grundlage eines Wiederholungszeitplans angepasst. Ein **schedule**-Wert enthält Änderungen auf der Grundlage von Minuten, Stunden, Wochentagen, Tagen des Monats und der Wochennummer. |

## <a name="job-schema-defaults-limits-and-examples"></a>Standardwerte des Auftragsschemas, Einschränkungen und Beispiele
Später in diesem Artikel behandeln wir jedes der folgenden Elemente im Detail:

| JSON-Name | Werttyp | Erforderlich? | Standardwert | Gültige Werte | Beispiel |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** |Zeichenfolge |Nein  |Keine |Datum/Uhrzeit (nach ISO 8601) |`"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** |object |Nein  |Keine |Das Serienobjekt |`"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **frequency** |Zeichenfolge |Ja |Keine |"minute", "hour", "day", "week", "month" |`"frequency" : "hour"` |
| **interval** |number |Ja |Keine |1 bis 1.000 |`"interval":10` |
| **endTime** |Zeichenfolge |Nein  |Keine |Ein Datums-/Uhrzeitwert, der eine Zeit in der Zukunft darstellt. |`"endTime" : "2013-02-09T09:30:00-08:00"` |
| **count** |number |Nein  |Keine |>= 1 |`"count": 5` |
| **schedule** |object |Nein  |Keine |Das Zeitplanobjekt |`"schedule" : { "minute" : [30], "hour" : [8,17] }` |

## <a name="deep-dive-starttime"></a>Ausführliche Betrachtung: „startTime“
Die folgende Tabelle beschreibt, wie **startTime** die Art und Weise steuert, wie ein Auftrag ausgeführt wird:

| startTime-Wert | Keine Serie | Serie, kein Zeitplan | Wiederholung mit Zeitplan |
|:--- |:--- |:--- |:--- |
| **Keine Startzeit** |Sofortige einmalige Ausführung. |Sofortige einmalige Ausführung. Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit. |Sofortige einmalige Ausführung.<br /><br />Berechnet weitere Ausführungen auf Grundlage des Serienzeitplans. |
| **Startuhrzeit in der Vergangenheit** |Sofortige einmalige Ausführung. |Berechnung der ersten zukünftigen Ausführungszeit nach der Startzeit und Ausführung zu diesem Zeitpunkt.<br /><br />Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit. <br /><br />Weitere Informationen finden Sie im Beispiel, das auf diese Tabelle folgt. |Auftrag startet *frühestens* zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<br /><br />Berechnet weitere Ausführungen auf Grundlage des Serienzeitplans. |
| **Startzeit in der Zukunft oder aktuelle Uhrzeit** |Einmalige Ausführung zur angegebenen Startzeit. |Einmalige Ausführung zur angegebenen Startzeit.<br /><br />Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.|Auftrag startet *frühestens* zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<br /><br />Berechnet weitere Ausführungen auf Grundlage des Serienzeitplans. |

Das folgende Beispiel veranschaulicht, was passiert, wenn **startTime** in der Vergangenheit liegt und nur „recurrence“, aber kein Zeitplan angegeben ist.  In diesem Beispiel wird davon ausgegangen, dass die aktuelle Zeit „2015-04-08-13:00“ ist, **startTime** auf „2015-04-07 14:00“ festgelegt ist und für **recurrence** ein Zwei-Tages-Intervall (mit „**frequency**: day“ und „**interval**: 2“) angegeben wurde. Beachten Sie, dass **startTime** in der Vergangenheit liegt.

Unter diesen Umständen erfolgt die erste Ausführung am 9.4.2015 um 14:00 Uhr. Die Scheduler-Engine berechnet die Ausführungen auf Grundlage der Startzeit. In der Vergangenheit liegende Instanzen werden verworfen. Die Engine verwendet die nächste in der Zukunft liegende Instanz. In diesem Fall ist **startTime** auf den 7.4.2015 um 14:00 Uhr festgelegt. Die nächste Instanz folgt zwei Tage nach diesem Zeitpunkt, also am 9.4.2015 um 14:00 Uhr.

Beachten Sie, dass die erste Ausführung auch dann zu diesem Zeitpunkt stattfinden würde, wenn als **startTime** der 5.4.2015 oder der 1.4.2015 (jeweils 14:00 Uhr) angegeben würde.\. Nach der ersten Ausführung werden nachfolgende Ausführungen anhand des Zeitplans berechnet. Sie finden am 11.4.2015 um 14:00 Uhr, am 13.4.2015 um 14:00 Uhr, am 15.4.2015 um 14:00 Uhr usw. statt.

Wenn für den Auftrag ein Zeitplan, aber keine Stunden- und Minutenangabe festgelegt ist, werden als Werte standardmäßig die Stunden und Minuten der ersten Ausführung verwendet.

## <a name="deep-dive-schedule"></a>Ausführliche Untersuchung: „schedule“
Mithilfe von **schedule** lässt sich die Anzahl der Auftragsausführungen *begrenzen*. Beispiel: Wird für einen Auftrag mit einer monatlichen Häufigkeit (**frequency** ist „month“) ein Zeitplan angegeben, der nur am 31. Tag ausgeführt wird, wird der Auftrag nur in Monaten mit 31 Tagen ausgeführt.

Mithilfe von **schedule** lässt sich die Anzahl der Auftragsausführungen auch *erweitern*. Beispiel: Wird für einen Auftrag mit einer monatlichen Häufigkeit (**frequency** ist „month“) ein Zeitplan angegeben, der am ersten und zweiten Tag des Monats ausgeführt wird, wird der Auftrag nicht nur ein Mal im Monat, sondern jeweils am ersten und zweiten Tag des Monats ausgeführt.

Wenn Sie mehrere Zeitplanelemente angeben, werden diese in absteigender Reihenfolge ausgewertet: von der Wochennummer über den Tag des Monats und den Wochentag bis hin zu Stunde und Minute.

Die folgende Tabelle enthält eine ausführliche Beschreibung der Zeitplanelemente:

| JSON-Name | BESCHREIBUNG | Gültige Werte |
|:--- |:--- |:--- |
| **minutes** |Minuten der Stunde, in der der Auftrag ausgeführt wird. |Ein Array mit ganzen Zahlen. |
| **hours** |Stunden des Tages, an dem der Auftrag ausgeführt wird. |Ein Array mit ganzen Zahlen. |
| **weekDays** |Die Wochentage, an denen der Auftrag ausgeführt wird. Kann nur bei wöchentlicher Häufigkeit angegeben werden. |Ein Array aus den folgenden Werten (die maximale Arraygröße ist 7):<br />– „Monday“<br />– „Tuesday“<br />– „Wednesday“<br />– „Thursday“<br />– „Friday“<br />– „Saturday“<br />– „Sunday“<br /><br />Keine Beachtung der Groß-/Kleinschreibung. |
| **monthlyOccurrences** |Bestimmt, an welchen Tagen im Monat der Auftrag ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. |Ein Array aus **monthlyOccurrence**-Objekten:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **day** ist der Wochentag, an dem der Auftrag ausgeführt wird. Beispielsweise bedeutet *{Sunday}*, dass der Auftrag an jedem Sonntag des Monats ausgeführt wird. Erforderlich.<br /><br />**occurrence** ist das Vorkommen des Tages innerhalb des Monats. Beispielsweise bedeutet *{Sunday, -1}* den letzten Sonntag des Monats. Optional. |
| **monthDays** |Der Tag des Monats, an dem der Auftrag ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. |Ein Array mit den folgenden Werten:<br />– Beliebiger Wert, für den Folgendes gilt: <= -1 und >= -31<br />– Beliebiger Wert, für den Folgendes gilt: >= 1 und <= 31|

## <a name="examples-recurrence-schedules"></a>Beispiele: Serienzeitpläne
Die folgenden Beispiele zeigen verschiedene Serienzeitpläne. Die Beispiele beziehen sich auf das schedule-Objekt und seine untergeordneten Elemente.

Bei diesen Zeitplänen wird davon ausgegangen, dass **interval** auf 1 festgelegt ist.\. Die Beispiele gehen auch von den richtigen **frequency**-Werten für die Werte in **schedule** aus. Beispielsweise können nicht der **frequency**-Wert „day“ und die Änderung **monthDays** in **schedule** angegeben werden. Wir haben diese Einschränkungen weiter oben in diesem Artikel beschrieben.

| Beispiel | BESCHREIBUNG |
|:--- |:--- |
| `{"hours":[5]}` |Ausführung täglich um 05:00 Uhr.<br /><br />Scheduler gleicht jeden Wert für „hours“ nacheinander mit den Werten für „minutes“ ab, um eine Liste mit allen Zeiten zu erstellen, zu denen der Auftrag ausgeführt werden soll. |
| `{"minutes":[15], "hours":[5]}` |Ausführung täglich um 05:15 Uhr. |
| `{"minutes":[15], "hours":[5,17]}` |Ausführung täglich um 05:15 und 17:15 Uhr. |
| `{"minutes":[15,45], "hours":[5,17]}` |Ausführung täglich um 05:15 Uhr, 05:45 Uhr, 17:15 Uhr und 17:45 Uhr. |
| `{"minutes":[0,15,30,45]}` |Ausführung alle 15 Minuten. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Stündliche Ausführung.<br /><br />Dieser Auftrag wird einmal pro Stunde ausgeführt. Die Minute wird durch den Wert für **startTime** gesteuert, wenn dieser angegeben wird. Wenn kein **startTime**-Wert angegeben ist, werden die Minuten von der Erstellungszeit gesteuert. Lautet die Start- oder Erstellungszeit also beispielsweise 12:25 Uhr, wird der Auftrag um 00:25 Uhr, 01:25 Uhr, 02:25 Uhr, …, 23:25 Uhr ausgeführt.<br /><br />Der Zeitplan entspricht einem Auftrag mit einer **frequency** von „hour“ und einem **interval** von 1 (ohne Angabe eines **schedule**-Werts). Der Unterschied besteht darin, dass dieser Zeitplan mit unterschiedlichen Angaben für **frequency**- und **interval**-Werte auch zur Erstellung anderer Aufträge verwendet werden kann. Wird **frequency** beispielsweise auf „month“ festgelegt, wird der Zeitplan nur ein Mal im Monat ausgeführt (und nicht täglich wie bei einem **frequency**-Wert von „day“). |
| `{minutes:[0]}` |Ausführung jeweils zur vollen Stunde.<br /><br />Dieser Auftrag wird ebenfalls stündlich ausgeführt, aber immer zur vollen Stunde (also beispielsweise um 24: 00 Uhr, 01:00 Uhr, 02:00 Uhr usw.). Dies ist äquivalent zu einem Auftrag mit einer **frequency** von „hour“, einem **startTime**-Wert von null Minuten und keinem **schedule**, wenn die Häufigkeit „day“ ist. Wenn der **frequency**-Wert jedoch „week“ oder „month“ ist, wird der Zeitplan nur an einem Tag in der Woche bzw. im Monat ausgeführt. |
| `{"minutes":[15]}` |Stündliche Ausführung jeweils 15 Minuten nach der vollen Stunde.<br /><br />Ausführung immer zur vollen Stunde ab 00:15 Uhr (dann 01:15 Uhr, 02:15 Uhr usw.). Die Ausführung endet um 23:15 Uhr. |
| `{"hours":[17], "weekDays":["saturday"]}` |Wöchentliche Ausführung, immer samstags um 17:00 Uhr. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:00 Uhr. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:15 und 17:45 Uhr. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 05:00 Uhr und um 17:00 Uhr. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 05:15 Uhr, 05:45 Uhr, 17:15 Uhr und 17:45 Uhr. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Ausführung an Wochentagen im 15-Minuten-Takt. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Ausführung an Wochentagen zwischen 09:00 Uhr und 16:45 Uhr im 15-Minuten-Takt. |
| `{"weekDays":["sunday"]}` |Ausführung an Sonntagen zur Startzeit. |
| `{"weekDays":["tuesday", "thursday"]}` |Ausführung jeweils dienstags und donnerstags zur Startzeit. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Ausführung um 06:00 Uhr am 28. Tag des Monats (bei einem **frequency**-Wert von „month“). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Ausführung um 06:00 Uhr am letzten Tag des Monats.<br /><br />Wenn Sie einen Auftrag am letzten Tag eines Monats ausführen möchten, verwenden Sie „-1“ anstelle von Tag 28, 29, 30 oder 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Ausführung jeweils am ersten und letzten Tag jedes Monats um 06:00 Uhr. |
| `{monthDays":[1,-1]}` |Ausführung jeweils am ersten und letzten Tag jedes Monats zur Startzeit. |
| `{monthDays":[1,14]}` |Ausführung jeweils am ersten und 14. Tag jedes Monats zur Startzeit. |
| `{monthDays":[2]}` |Ausführung jeweils am zweiten Tag des Monats zur Startzeit. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Ausführung am ersten Freitag jedes Monats um 05:00 Uhr. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Ausführung am ersten Freitag jedes Monats zur Startzeit. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Monatliche Ausführung am dritten Freitag ab Monatsende zur Startzeit. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Ausführung am ersten und letzten Freitag jedes Monats um 05:15 Uhr. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Ausführung am ersten und letzten Freitag jedes Monats zur Startzeit. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Ausführung am fünften Freitag jedes Monats zur Startzeit.<br /><br />Wenn kein fünfter Freitag im Monat vorhanden ist, wird der Auftrag nicht ausgeführt. Soll der Auftrag am letzten Freitag des Monats ausgeführt werden, empfiehlt sich unter Umständen die Verwendung von „-1“ anstelle von „5“. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Ausführung im 15-Minuten-Takt am letzten Freitag des Monats. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Ausführung um 05:15, 05:45, 17:15 und 17:45 Uhr am dritten Mittwoch jedes Monats. |

## <a name="see-also"></a>Weitere Informationen

- [Was ist Azure Scheduler?](scheduler-intro.md)
- [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)
- [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)
- [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)
- [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)
- [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)
- [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)
- [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)
- [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)


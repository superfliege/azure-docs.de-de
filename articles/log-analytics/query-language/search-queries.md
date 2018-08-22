---
title: Suchabfragen in Log Analytics | Microsoft-Dokumentation
description: Dieser Artikel enthält ein Tutorial zu den ersten Schritten beim Schreiben von Suchabfragen in Log Analytics.
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
ms.date: 08/06/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 7c6bade68e6190ec52cbc136c057906be6d9d37c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39633204"
---
# <a name="search-queries-in-log-analytics"></a>Suchabfragen in Log Analytics

> [!NOTE]
> Vor der Durchführung dieses Tutorials sollten Sie [Erste Schritte mit Abfragen in Log Analytics](get-started-queries.md) lesen.

Azure Log Analytics-Abfragen können entweder mit einem Tabellennamen oder einem Suchbefehl beginnen. Dieses Tutorial behandelt suchbasierte Abfragen. Jede Methode bringt ihre eigenen Vorteile mit sich.

Tabellenbasierte Abfragen definieren zunächst den Bereich der Abfrage und sind daher meist effizienter als Suchabfragen. Suchabfragen sind weniger strukturiert, weshalb Sie bei der Suche nach einem bestimmten Wert in Spalten oder Tabellen die bessere Wahl darstellen. Mit **search** kann der angegebene Wert in allen Spalten in einer bestimmten Tabelle oder in allen Tabellen gesucht werden. Die Menge der verarbeiteten Daten kann enorm sein, weshalb diese Abfragen mehr Zeit in Anspruch nehmen und sehr umfangreiche Resultsets zurückgeben können.

## <a name="search-a-term"></a>Suchen nach einer Benennung
Der **search**-Befehl wird in der Regel zum Suchen einer bestimmten Benennung verwendet. Im folgenden Beispiel werden alle Spalten in allen Tabellen auf die Benennung „error“ überprüft:

```OQL
search "error"
| take 100
```

Abfragen ohne Bereichseinschränkung wie die oben gezeigten sind zwar einfach in ihrer Verwendung, jedoch nicht effizient und geben sehr wahrscheinlich viele irrelevante Ergebnisse zurück. Eine bessere Vorgehensweise wäre es, in der relevanten Tabelle oder sogar in einer bestimmten Spalte zu suchen.

### <a name="table-scoping"></a>Bereichseinschränkung für Tabellen
Um nach einer Benennung in einer bestimmten Tabelle zu suchen, fügen Sie `in (table-name)` direkt nach dem Operator **search** hinzu:

```OQL
search in (Event) "error"
| take 100
```

Um in mehreren Tabellen zu suchen, fügen Sie Folgendes hinzu:
```OQL
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Bereichseinschränkung für Tabellen und Spalten
Standardmäßig wertet **search** alle Spalten im Dataset aus. Um nur nach einer bestimmten Spalte zu suchen, verwenden Sie die folgende Syntax:

```OQL
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Wenn Sie `==` anstelle von `:` verwenden, würden die Ergebnisse Datensätze enthalten, bei denen die Spalte *Quelle* exakt den Wert „error“ mit genau dieser Schreibweise aufweist. Mit „:“ werden Datensätze, bei denen *Quelle* Werte wie „error code 404“ oder „Error“ aufweist, nicht miteinbezogen.

## <a name="case-sensitivity"></a>Groß-/Kleinschreibung
Standardmäßig wird die Groß-/Kleinschreibung bei der Suche nach Benennungen nicht berücksichtigt, damit z.B. die Suche nach „dns“ Ergebnisse wie „DNS“, „dns“ oder „Dns“ zurückgibt. Damit die Groß-/Kleinschreibung bei der Suche berücksichtigt wird, verwenden Sie die Option `kind`:

```OQL
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Verwenden von Platzhaltern
Der **search**-Befehl unterstützt Platzhalter am Anfang, am Ende oder innerhalb einer Benennung.

Um Benennungen zu suchen, die mit „win“ beginnen, gehen Sie wie folgt vor:
```OQL
search in (Event) "win*"
| take 100
```

Um Benennungen zu suchen, die mit „.com“ enden, gehen Sie wie folgt vor:
```OQL
search in (Event) "*.com"
| take 100
```

Um Benennungen zu suchen, die „www“ enthalten, gehen Sie wie folgt vor:
```OQL
search in (Event) "*www*"
| take 100
```

Um Benennungen zu suchen, die mit „corp“ beginnen und auf „.com“ enden (z.B. „corp.mydomain.com“), gehen Sie wie folgt vor:

```OQL
search in (Event) "corp*.com"
| take 100
```

Sie können auch alles in eine Tabelle mit nur einem Platzhalter (`search in (Event) *`) einfügen. Dies würde jedoch zu den gleichen Ergebnissen wie bei der Angabe von `Event` führen.

> [!TIP]
> Mit `search *` können Sie zwar jede Spalte aus jeder Tabelle abrufen, allerdings sollten Sie den Bereich Ihrer Abfragen immer auf bestimmte Tabellen beschränken. Abfragen ohne Bereichseinschränkung können sehr lange dauern und möglicherweise zu viele Ergebnisse zurückgeben.

## <a name="add-and--or-to-search-queries"></a>Hinzufügen von *and*/*or* zu Suchabfragen
Verwenden Sie **and**, um nach Datensätzen zu suchen, die mehrere Benennungen enthalten:

```OQL
search in (Event) "error" and "register"
| take 100
```

Verwenden Sie **or**, um Datensätze abzurufen, die mindestens eine der Benennungen enthalten:

```OQL
search in (Event) "error" or "register"
| take 100
```

Wenn Sie mehrere Suchbedingungen verwenden, können Sie sie mithilfe von Klammern in einer einzelnen Abfrage kombinieren:

```OQL
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Die Ergebnisse dieses Beispiels wären Datensätze, die die Benennung „error“ und zudem entweder „register“ oder eine Benennung, die mit „marshal“ beginnt, enthalten.

## <a name="pipe-search-queries"></a>Übergeben von Suchabfragen
Genau wie jeder andere Befehl kann **search** übergeben werden, um Suchergebnisse zu filtern, zu sortieren und zu aggregieren. Um beispielsweise die Anzahl der *Event*-Datensätze, die „win“ enthalten, abzurufen, gehen Sie wie folgt vor:

```OQL
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Nächste Schritte

- Weitere Tutorials finden Sie auf der [Website zur Log Analytics – Abfragesprache](http://http://docs.loganalytics.io).
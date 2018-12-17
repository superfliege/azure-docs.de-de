---
title: Cheat Sheet für den Wechsel von der Abfragesprache SQL zu Azure Log Analytics | Microsoft-Dokumentation
description: Allgemeine Funktionen, die für unterschiedliche Szenarien in Log Analytics-Abfragen verwendet werden.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 35438644842d5280bd789efa135805ba9943cb8b
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183046"
---
# <a name="sql-to-log-analytics-query-language-cheat-sheet"></a>Cheat Sheet für den Wechsel von der Abfragesprache SQL zu Log Analytics 

Die folgende Tabelle hilft Benutzern, die mit SQL, vertraut sind, die Log Analytics-Abfragesprache zu erlernen. Werfen Sie einen Blick auf den Befehl T-SQL für das Lösen eines gängigen Szenariums und der Entsprechung in Log Analytics.

## <a name="sql-to-log-analytics"></a>Von SQL zu Log Analytics

BESCHREIBUNG                             |SQL-Abfrage                                                                                          |Azure Log Analytics-Abfrage
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Alle Daten in einer Tabelle auswählen            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Bestimmte Spalten in einer Tabelle auswählen    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
100 Datensätze in einer Tabelle auswählen         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Null-Auswertung                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Zeichenfolgenvergleich: Gleichheit             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Zeichenfolgenvergleich: Untergeordnete Zeichenfolge            |`SELECT * FROM dependencies WHERE like "%bcd%"`                                                    |<code>dependencies <br>&#124; where name contains "bcd"</code>
Zeichenfolgenvergleich: Platzhalter             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Datumsvergleich: Letzter Tag             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Datumsvergleich: Datumsbereich             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Boolescher Vergleich                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False"</code>
Sortieren                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc</code>
Unterscheiden                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type</code>
Gruppierung, Aggregation                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name</code>
Spaltenaliase, Erweitern                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Top n-Datensätze nach Kennzahl                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Union                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Union: mit Bedingungen                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions value < 5`              |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Join                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Nächste Schritte

- Arbeiten Sie eine Lektion über das [Schreiben von Abfragen in Log Analytics](get-started-queries.md) durch.
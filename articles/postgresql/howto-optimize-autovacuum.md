---
title: Optimieren von Autovacuum auf einem Azure Database for PostgreSQL-Server
description: In diesem Artikel wird beschrieben, wie Sie Autovacuum auf einem Azure Database for PostgreSQL-Server optimieren können.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 21ac48ff473dcf494f96f87210bdfe09e4d82646
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103393"
---
# <a name="optimizing-autovacuum-on-azure-database-for-postgresql-server"></a>Optimieren von Autovacuum auf einem Azure Database for PostgreSQL-Server 
In diesem Artikel wird beschrieben, wie Sie Autovacuum unter Azure Database for PostgreSQL effektiv optimieren.

## <a name="overview-of-autovacuum"></a>Übersicht über Autovacuum
PostgreSQL verwendet MVCC für eine bessere Datenbankparallelität. Jede Aktualisierung führt zu einer Einfügung und einer Löschung, und jede Löschung führt dazu, dass die entsprechenden Zeilen vorläufig zum Löschen markiert werden. Die vorläufige Markierung sorgt dafür, dass inaktive Tupel identifiziert und später endgültig gelöscht werden. PostgreSQL erledigt diesen Vorgang durch Ausführen eines Vacuum-Auftrags.

Ein Vacuum-Auftrag kann manuell oder automatisch ausgelöst werden. Wenn in der Datenbank viele Aktualisierungs- oder Löschvorgänge stattfinden, sind mehr inaktive Tupel vorhanden, im Leerlauf weniger.  Ein häufigeres Ausführen von Vacuum ist wichtiger, wenn die Datenbank stark ausgelastet ist. Dadurch ist das **manuelle** Ausführen von Vacuum-Aufträgen unpraktisch.

Autovacuum kann so konfiguriert werden und profitiert von einer Optimierung. Die Standardwerte, mit denen PostgreSQL ausgeliefert wird, sollen sicherstellen, dass das Produkt auf allen Arten von Geräten einschließlich Raspberry Pis funktioniert, und die idealen Konfigurationswerte sind von einer Reihe von Faktoren abhängig:
- Gesamtanzahl der Ressourcen – SKU und Speichergröße
- Ressourcennutzung
- Merkmale der einzelnen Objekte

## <a name="autovacuum-benefits"></a>Vorteile von Autovacuum
Wenn Sie nicht hin und wieder Vacuum ausführen, können sich inaktive Tupel ansammeln und zu Problemen führen:
- Datenüberfrachtung – größere Datenbanken und Tabellen
- Größere suboptimale Indizes
- Steigende E/A

## <a name="monitoring-bloat-with-autovacuum-queries"></a>Überwachen der Überfrachtung mit Autovacuum-Abfragen
Die folgende Beispielabfrage wird konzipiert, um die Anzahl der inaktiven und aktiven Tupel in einer Tabelle mit dem Namen „XYZ“ zu identifizieren: 'SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup/ n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables WHERE relname = 'XYZ' order by n_dead_tup DESC;'

## <a name="autovacuum-configurations"></a>Autovacuum-Konfigurationen
Die Konfigurationsparameter, die Autovacuum steuern, werden durch zwei zentrale Fragen beeinflusst:
- Wann sollte Autovacuum starten?
- Wie viel sollte nach dem Start bereinigt werden?

Im Folgenden finden Sie einige Autovacuum-Konfigurationsparameter, die Sie anhand der oben genannten Fragen aktualisieren können, sowie einige Hinweise zu ihnen:
Parameter|BESCHREIBUNG|Standardwert
---|---|---
autovacuum_vacuum_threshold|Gibt die minimale Anzahl von aktualisierten oder gelöschten Tupeln vor dem Auslösen eines VACUUM-Vorgangs in einer Tabelle an. Der Standardwert ist 50 Tupel. Dieser Parameter kann nur in der Datei „postgresql.conf“ oder über die Serverbefehlszeile festgelegt werden. Die Einstellung kann für einzelne Tabellen durch Ändern der Tabellenspeicherparameter überschrieben werden.|50
autovacuum_vacuum_scale_factor|Gibt einen Anteil der Tabellengröße an, der autovacuum_vacuum_threshold bei der Entscheidung über die Auslösung eines VACUUM hinzugefügt wird. Der Standardwert ist 0,2 (20 % der Tabellengröße). Dieser Parameter kann nur in der Datei „postgresql.conf“ oder über die Serverbefehlszeile festgelegt werden. Die Einstellung kann für einzelne Tabellen durch Ändern der Tabellenspeicherparameter überschrieben werden.|5 %
autovacuum_vacuum_cost_limit|Gibt den Kostengrenzwert an, der bei automatischen VACUUM-Vorgängen verwendet wird. Wenn „–1“ angegeben ist (der Standard), wird der normale Wert vacuum_cost_limit verwendet. Der Wert wird zwischen den ausgeführten Autovacuum-Workern proportional aufgeteilt, wenn mehrere Worker vorhanden sind. Die Summe der Grenzwerte für die einzelnen Worker überschreitet nicht den Wert dieser Variable. Dieser Parameter kann nur in der Datei „postgresql.conf“ oder über die Serverbefehlszeile festgelegt werden. Die Einstellung kann für einzelne Tabellen durch Ändern der Tabellenspeicherparameter überschrieben werden.|-1
autovacuum_vacuum_cost_delay|Gibt die Kostenverzögerung an, die bei automatischen VACUUM-Vorgängen verwendet wird. Wenn „–1“ angegeben ist, wird der normale Wert vacuum_cost_delay verwendet. Der Standardwert sind 20 Millisekunden. Dieser Parameter kann nur in der Datei „postgresql.conf“ oder über die Serverbefehlszeile festgelegt werden. Die Einstellung kann für einzelne Tabellen durch Ändern der Tabellenspeicherparameter überschrieben werden.|20 ms
autovacuum_nap_time|Gibt die minimale Verzögerung zwischen Autovacuum-Ausführungen in einer bestimmten Datenbank an. In jeder Runde untersucht der Daemon die Datenbank und gibt nach Bedarf VACUUM- und ANALYZE-Befehle für Tabellen in dieser Datenbank aus. Die Verzögerung wird in Sekunden gemessen, und der Standardwert ist 1 Minute. Dieser Parameter kann nur in der Datei „postgresql.conf“ oder über die Serverbefehlszeile festgelegt werden.|15 s
autovacuum_max_workers|Gibt die maximale Anzahl von Autovacuum-Prozessen (mit Ausnahme des Autovacuum-Startprogramms) an, die gleichzeitig ausgeführt werden können. Der Standardwert ist 3. Dieser Parameter kann nur beim Serverstart festgelegt werden.|3
Die angegebene Einstellung kann für einzelne Tabellen durch Ändern der Tabellenspeicherparameter überschrieben werden.  

## <a name="autovacuum-cost"></a>Autovacuum-Kosten
Im Folgenden sind die „Kosten“ für das Ausführen eines Vakuum-Vorgangs angegeben:
- Die Datenseiten, für die Vacuum ausgeführt wird, werden gesperrt.
- Beim Ausführen von Vacuum werden Compute und Speicher verwendet.

Das bedeutet, dass Vacuum weder zu häufig noch zu selten ausgeführt werden sollte, sondern an die Workload angepasst sein muss. Es wird empfohlen, alle Autovacuum-Parameteränderungen zu überprüfen, da alle Vor- und Nachteile aufweisen.

## <a name="autovacuum-start-trigger"></a>Autovacuum-Startauslöser
Autovacuum wird ausgelöst, wenn die Anzahl der inaktiven Tupel autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor * reltuples überschreitet, wobei reltuples hier eine Konstante ist.

Die Bereinigung durch Autovacuum muss die Datenbankauslastung berücksichtigen, da es andernfalls vorkommen kann, dass nicht mehr genügend Speicher vorhanden ist und eine allgemeine Verlangsamung bei den Abfragen auftritt. Über die Zeit betrachtet sollte die Rate, mit der Tupel durch Vacuum gelöscht werden, mit der Rate übereinstimmen, mit der inaktive Tupel erstellt werden.

Datenbanken mit vielen Aktualisierungen/Löschungen weisen mehr inaktive Tupel auf und benötigen mehr Speicherplatz. Im Allgemeinen sind für Datenbanken mit vielen Aktualisierungen/Löschungen niedrige Werte für autovacuum_vacuum_scale_factor und niedrige Werte für autovacuum_vacuum_threshold gut geeignet, um eine längere Ansammlung von inaktiven Tupeln zu verhindern. Sie können bei kleineren Datenbanken für beide Parameter höhere Werte verwenden, da Vacuum weniger dringend ist. Zur Erinnerung: Ein häufiges Ausführen von Vacuum belegt Compute- und Speicherressourcen.

Der Standardskalierungsfaktor von 20 Prozent funktioniert gut für Tabellen mit einem niedrigen Anteil an inaktiven Tupeln, aber nicht für Tabellen mit einem hohen Anteil an inaktiven Tupeln. Beispielsweise ist dies für eine 20-GB-Tabelle ein Wert von 4 GB für inaktive Tupel und für eine 1-TB-Tabelle ein Wert von 200 GB für inaktive Tupel.

Mit PostgreSQL können Sie diese Parameter auf Tabellenebene oder Instanzebene festlegen. In Azure Database for PostgreSQL können diese Parameter derzeit nur auf Tabellenebene festgelegt werden.

## <a name="estimating-the-cost-of-autovacuum"></a>Schätzen der Kosten für Autovacuum
Das Ausführen von Autovacuum ist „kostenaufwendig“, und die Laufzeit von Vaccum-Vorgängen kann mit Parametern gesteuert werden. Die folgenden Parameter helfen dabei, die Kosten für die Ausführung von Vacuum zu schätzen:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

Der Vacuum-Prozess liest physische Seiten und sucht nach inaktiven Tupeln. Für jede Seite in shared_buffers wird von Kosten von 1 (vacuum_cost_page_hit) ausgegangen, für alle anderen Seiten wird von Kosten von 20 (vacuum_cost_page_dirty) bei vorliegenden inaktiven Tupeln ausgegangen oder von 10 (vacuum_cost_page_miss), wenn keine inaktiven Tupel vorhanden sind. Der Vacuum-Vorgang wird beendet, wenn der Prozess autovacuum_vacuum_cost_limit überschreitet.  

Nachdem der Grenzwert erreicht wird, verbringt der Prozess die durch den Parameter autovacuum_vacuum_cost_delay angegebene Zeit im Ruhezustand, bevor er erneut gestartet wird. Wenn der Grenzwert nicht erreicht wird, wird Autovacuum nach der durch den Parameter autovacuum_nap_time angegebenen Zeit wieder gestartet.

Zusammenfassung: Die Parameter autovacuum_vacuum_cost_delay und autovacuum_vacuum_cost_limit steuern, wie viel Datenbereinigung pro Zeiteinheit zulässig ist. Beachten Sie, dass die Standardwerte für die meisten Tarife zu niedrig sind. Die optimalen Werte für diese Parameter sind vom Tarif abhängig und sollten entsprechend konfiguriert werden.

Der Parameter autovacuum_max_workers legt die maximale Anzahl von Autovacuum-Prozessen fest, die gleichzeitig ausgeführt werden können.

Mit PostgreSQL können Sie diese Parameter auf Tabellenebene oder Instanzebene festlegen. In Azure Database for PostgreSQL können diese Parameter derzeit nur auf Tabellenebene festgelegt werden.

## <a name="optimizing-autovacuum-per-table"></a>Optimieren von Autovacuum pro Tabelle
Alle oben genannten Konfigurationsparameter können pro Tabelle konfiguriert werden, z.B.:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum ist ein synchroner Prozess pro Tabelle. Je größer der Anteil an inaktiven Tupeln in einer Tabelle ist, desto höher sind die „Kosten“ für Autovacuum.  Das Aufteilen von Tabellen mit einem hohen Grad an Aktualisierungen/Löschungen in mehrere Tabellen trägt dazu bei, Autovacuum zu parallelisieren und die „Kosten“ zum Abschließen von Autovacuum-Vorgängen für eine Tabelle zu reduzieren. Sie können auch die Anzahl der parallelen Autovacuum-Worker erhöhen, um sicherzustellen, dass Worker großzügig geplant werden.

## <a name="next-steps"></a>Nächste Schritte
In den folgenden PostgreSQL-Dokumentationen erhalten Sie weitere Informationen zur Verwendung und zum Optimieren von Autovacuum:
 - PostgreSQL-Dokumentation: [Kapitel 18, Serverkonfiguration](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - PostgreSQL-Dokumentation: [Kapitel 24, routinemäßige Datenbankwartungstasks](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)

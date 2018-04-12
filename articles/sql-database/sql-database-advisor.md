---
title: Empfehlungen zur Leistung – Azure SQL-Datenbank | Microsoft-Dokumentation
description: Azure SQL-Datenbank stellt Empfehlungen für Ihre SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: b88b8a313a03ef2ad72aeb053b89505201c23819
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="performance-recommendations-for-sql-database"></a>Empfehlungen zur Leistung für SQL-Datenbank

Azure SQL-Datenbank lernt und passt sich an Ihre Anwendung an. Das Produkt bietet maßgeschneiderte Empfehlungen, die es Ihnen ermöglichen, die Leistung Ihrer SQL-Datenbanken zu maximieren. Die SQL-Datenbank wertet kontinuierlich den Nutzungsverlauf Ihrer SQL-Datenbanken aus und analysiert ihn. Die Empfehlungen, die bereitgestellt werden, basieren auf für die Datenbank eindeutigen Workloadmustern und tragen zur Verbesserung der Leistung bei.

> [!TIP]
> Die [automatische Optimierung](sql-database-automatic-tuning.md) ist die empfohlene Methode zur Leistungsoptimierung. [Intelligent Insights](sql-database-intelligent-insights.md) ist die empfohlene Methode zum Überwachen der Leistung. 
>

## <a name="create-index-recommendations"></a>Empfehlungen zum Erstellen eines Index
SQL-Datenbank überwacht kontinuierlich die ausgeführten Abfragen und erkennt die Indizes, die die Leistung verbessern könnten. Sobald genügend Vertrauen besteht, dass ein bestimmter Index fehlt, wird eine neue Empfehlung **Index erstellen** erstellt.

 Azure SQL-Datenbank baut Vertrauen auf, indem berechnet wird, zu welchem Leistungsgewinn der Index mit der Zeit führen würde. Je nach geschätztem Leistungsgewinn werden Empfehlungen als „Hoch“, „Mittel“ oder „Niedrig“ eingestuft. 

Indizes, die mit Empfehlungen erstellt wurden, sind immer als automatisch erstellte Indizes gekennzeichnet. Sie können in der Ansicht „sys.indexes“ erkennen, welche Indizes automatisch erstellt wurden. Automatisch erstellte Indizes blockieren keine ALTER/RENAME-Befehle. 

Wenn Sie versuchen, die Spalte mit einem automatisch erstellten Index zu löschen, wird der Befehl übergeben. Der automatisch erstellte Index wird ebenfalls mit dem Befehl gelöscht. Reguläre Indizes blockieren den Befehl ALTER/RENAME für Spalten, die indiziert sind.

Nachdem die Empfehlung zum Erstellen eines Indexes angewendet wurde, vergleicht Azure SQL-Datenbank die Leistung der Abfragen mit der Baselineleistung. Wenn der neue Index die Leistung verbessert, wird die Empfehlung als erfolgreich gekennzeichnet, und der Auswirkungsbericht steht zur Verfügung. Wenn der Index nicht zu einer Leistungsverbesserung geführt hat, wird er automatisch rückgängig gemacht. SQL-Datenbank verwendet diesen Prozess, um sicherzustellen, dass Empfehlungen die Leistung der Datenbank verbessern.

Jede Empfehlung **Index erstellen** verfügt über eine Backoffrichtlinie, die verhindert, dass die Empfehlung bei einem hohen Ressourcenverbrauch einer Datenbank oder eines Pools angewendet wird. In der Backoffrichtlinie werden CPU, Daten-E/A, Protokoll-E/A und verfügbarer Speicher berücksichtigt. 

Wenn CPU, Daten-E/A oder Protokoll-E/A während der letzten 30 Minuten über 80 % lagen, wird die Empfehlung zum Erstellen eines Indexes aufgeschoben. Wenn der verfügbare Speicher nach der Indexerstellung unter 10 % liegt, geht die Empfehlung in den Status „Fehler“ über. Falls nach einigen Tagen die automatische Optimierung weiterhin davon ausgeht, dass dieser Index von Vorteil wäre, wird der Prozess erneut gestartet. 

Dieser Prozess wird wiederholt, bis genügend freier Speicherplatz zum Erstellen eines Indexes verfügbar ist oder der Index nicht mehr als vorteilhaft betrachtet wird.

## <a name="drop-index-recommendations"></a>Empfehlungen zum Löschen eines Indexes
Zusätzlich zum Erkennen fehlender Indizes analysiert die SQL-Datenbank kontinuierlich die Leistung der vorhandenen Indizes. Wenn der Index nicht verwendet wird, empfiehlt die Azure SQL-Datenbank seine Löschung. Das Löschen eines Indexes wird in zwei Fällen empfohlen:
* Der Index ist ein Duplikat eines anderes Indexes (gleiche indizierte und enthaltene Spalte, gleiches Partitionsschema und gleiche Filter).
* Der Index wurde für einen längeren Zeitraum (93 Tage) nicht verwendet.

„Index löschen“-Empfehlungen durchlaufen auch die Überprüfung nach der Implementierung. Wenn die Leistung verbessert wird, ist ein Auswirkungsbericht verfügbar. Wenn die Leistung beeinträchtigt wird, wird die Empfehlung zurückgesetzt.


## <a name="parameterize-queries-recommendations"></a>Empfehlungen zum Parametrisieren von Abfragen
Empfehlungen zum *Parametrisieren von Abfragen* werden angezeigt, wenn eine oder mehrere Abfragen ständig neu kompiliert werden, aber am Ende immer derselbe Abfrageausführungsplan daraus hervorgeht. Diese Bedingung eröffnet die Möglichkeit, erzwungene Parametrisierung anzuwenden. Durch die erzwungene Parametrisierung können Abfragepläne in Zukunft zwischengespeichert und wiederverwendet werden. Dies verbessert die Leistung und verringert den Ressourcenverbrauch. 

Jede für SQL Server ausgestellte Abfrage muss zunächst kompiliert werden, um einen Ausführungsplan zu erstellen. Jeder generierte Plan wird dem Plancache hinzugefügt. Nachfolgende Ausführungen derselben Abfrage können dieser Plan aus dem Cache wiederverwenden. Auf diese Weise wird eine erneute Kompilierung überflüssig. 

Abfragen mit nicht parametrisierten Werten können zu Mehrkosten bei der Leistung führen, da der Ausführungsplan jedes Mal neu kompiliert wird, wenn die nicht parametrisierten Werte unterschiedlich sind. In vielen Fällen werden für die gleichen Abfragen mit unterschiedlichen Parameterwerten die gleichen Ausführungspläne generiert. Diese Pläne werden dem Plancache jedoch separat hinzugefügt. 

Der Vorgang der Neukompilierung von Ausführungsplänen verbraucht Datenbankressourcen, erhöht die Abfragedauer und führt zu einem Überlauf des Plancaches. Diese Ereignisse bewirken ihrerseits, dass Pläne aus dem Cache entfernt werden. Sie können das entsprechende SQL Server-Verhalten ändern, indem Sie für die Datenbank die Option für erzwungene Parametrisierung festlegen. 

Damit Sie die Auswirkungen dieser Empfehlung besser abschätzen können, wird ein Vergleich zwischen der tatsächlichen CPU-Auslastung und der prognostizierten CPU-Auslastung nach Anwendung der Empfehlung bereitgestellt. Diese Empfehlung kann Sie dabei unterstützen, CPU-Einsparungen zu erzielen. Sie kann Ihnen auch helfen, die Abfragedauer und den Mehraufwand für den Plancache zu verringern. Dies bedeutet, dass eine größere Anzahl der Pläne im Cache verbleiben und wiederverwendet werden kann. Sie können diese Empfehlung schnell umsetzen, indem Sie den Befehl **Anwenden** auswählen. 

Nachdem Sie diese Empfehlung angewendet haben, wird innerhalb von Minuten erzwungenen Parametrisierung für Ihre Datenbank aktiviert. Sie startet den Überwachungsvorgang, der ungefähr 24 Stunden andauert. Nach Ablauf dieses Zeitraums können Sie den Überprüfungsbericht anzeigen. Dieser Bericht zeigt die CPU-Auslastung Ihrer Datenbank 24 Stunden bevor und nachdem die Empfehlung angewendet wurde. Der SQL Database Advisor verfügt über einen Sicherheitsmechanismus, der die angewendete Empfehlung automatisch wieder zurücknimmt, falls ein Absinken der Leistung erkannt wird.

## <a name="fix-schema-issues-recommendations-preview"></a>Empfehlungen zum Beheben von Schemaproblemen (Vorschau)

> [!IMPORTANT]
> Empfehlungen zum Beheben von Schemaproblemen werden in Kürze von Microsoft eingestellt. Wir empfehlen die Verwendung von [Intelligent Insights](sql-database-intelligent-insights.md) zum Überwachen Ihrer Datenbankleistungsprobleme, einschließlich der Schemaprobleme, die zuvor durch die Empfehlungen zum Beheben von Schemaproblemen abgedeckt wurden.
> 

Empfehlungen zum **Beheben von Schemaproblemen** werden angezeigt, wenn der SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer SQL-Datenbank auftreten. Diese Empfehlung wird in der Regel angezeigt, wenn in Ihrer Datenbank innerhalb einer Stunde mehrere schemabezogene Fehler auftreten (ungültiger Spaltenname, ungültiger Objektname usw.).

„Schemaprobleme“ sind eine Klasse von Syntaxfehlern in SQL Server. Sie treten auf, wenn die Definition der SQL-Abfrage und die Definition des Datenbankschemas nicht übereinstimmen. Dies kann z.B. der Fall sein, wenn eine der von der Abfrage erwarteten Spalten in der Zieltabelle fehlt oder umgekehrt. 

Die Empfehlung zum Beheben von Schemaproblemen wird angezeigt, wenn der Azure SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer SQL-Datenbank auftreten. Die folgende Tabelle zeigt die möglichen Fehler in Bezug auf Schemaprobleme:

| SQL-Fehlercode | Message |
| --- | --- |
| 201 |Die*'*'*-Prozedur oder -Funktion erwartet den '*'-Parameter, der nicht bereitgestellt wurde. |
| 207 |Ungültiger Spaltenname '*'. |
| 208 |Ungültiger Objektname '*'. |
| 213 |Der Spaltenname oder die Anzahl der bereitgestellten Werte entspricht nicht der Tabellendefinition. |
| 2812 |Die gespeicherte Prozedur '*' wurde nicht gefunden. |
| 8144 |Für die '*'-Prozedur oder -Funktion wurden zu viele Argumente angegeben. |

## <a name="next-steps"></a>Nächste Schritte
Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Der SQL Database Advisor setzt die Überwachung fort und stellt Empfehlungen bereit, mit denen sich die Leistung Ihrer Datenbank verbessern lässt. 

* Weitere Informationen zur automatischen Optimierung von Datenbankindizes und Abfrageausführungsplänen finden Sie unter [Automatische Optimierung der Azure SQL-Datenbank](sql-database-automatic-tuning.md).
* Weitere Informationen zum automatischen Überwachen der Datenbankleistung mit der automatisierten Diagnose und der Ursachenanalyse von Leistungsproblemen finden Sie unter [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md).
*  Weitere Informationen zur Verwendung von Leistungsempfehlungen im Azure-Portal finden Sie unter [Performance recommendations in the Azure portal (Leistungsempfehlungen im Azure-Portal)](sql-database-advisor-portal.md).
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung anzeigen und untersuchen können.



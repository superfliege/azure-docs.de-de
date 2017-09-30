---
title: Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights | Microsoft-Dokumentation
description: Intelligent Insights hilft Ihnen, Probleme mit der Leistung von Azure SQL-Datenbank zu beheben.
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/25/2017
ms.author: v-daljep
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: edd8c526a681c9cd5226ede6110f21c3362aaef6
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights

Diese Seite bietet Informationen zu Problemen mit der Leistung von Azure SQL-Datenbank, die mithilfe des Diagnoseprotokolls von [Intelligent Insights](sql-database-intelligent-insights.md) erkannt wurden. Dieses Diagnoseprotokoll kann an [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-azure-storage) oder eine Drittanbieterlösung für benutzerdefinierte DevOps-Warnungs- und Berichterstellungsfunktionen gesendet werden.

> [!NOTE]
> Eine Kurzanleitung zur Behandlung von Problemen mit der Leistung von Azure SQL-Datenbank finden Sie im Flussdiagramm [Empfohlene Vorgehensweise bei der Problembehandlung](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) in diesem Dokument.
>

## <a name="detectable-database-performance-patterns"></a>Erkennbare Muster bei der Datenbankleistung

Intelligent Insights erkennt Probleme mit der Leistung von Azure SQL-Datenbank automatisch anhand der Wartezeiten bei der Ausführung von Abfragen, von Fehlern und Zeitüberschreitungen. Erkannte Leistungsmuster werden im Diagnoseprotokoll ausgegeben. Erkennbare Leistungsmuster sind in der folgenden Tabelle zusammengefasst:

| Erkennbare Leistungsmuster | Ausgegebene Details |
| :------------------- | ------------------- |
| [Erreichen von Ressourcengrenzwerten](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Der Verbrauch von im überwachten Abonnement verfügbaren Ressourcen (Datenübertragungseinheiten, DTUs), Datenbankarbeitsthreads und -anmeldesitzungen hat Grenzwerte erreicht, die Probleme mit der Leistung von Azure SQL-Datenbank verursachen. |
| [Gestiegene Workload](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | Eine Zunahme der Workload oder fortlaufende Häufung von Workloads in der Datenbank wurde erkannt, was Probleme mit der Leistung von Azure SQL-Datenbank verursacht. |
| [Hohe Arbeitsspeicherauslastung](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Worker, die Speicherzuweisungen anfordern, warten statistisch gesehen lange auf Speicherbelegungen, oder es liegt eine Häufung des Anstiegs von Workern vor, die Speicherzuweisungen anfordern, was sich auf die Leistung von Azure SQL-Datenbank auswirkt. |
| [Sperren](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Übermäßige Datenbanksperren wurden erkannt, wodurch die Leistung von Azure SQL-Datenbank beeinträchtigt wird. |
| [Erhöhter Wert für „Maximaler Grad an Parallelität“](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | Die Option „Maximaler Grad an Parallelität“ wurde geändert, was sich auf die Effizienz der Ausführung von Abfragen auswirkt.  |
| [Seitenlatchkonflikt](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Ein Seitenlatchkonflikt wurde erkannt, wodurch die Leistung von Azure SQL-Datenbank beeinträchtigt wird. Mehrere Threads versuchen gleichzeitig, auf dieselben Datenpufferseiten im Arbeitsspeicher zuzugreifen. Dies führt zu längeren Wartezeiten, was sich auf die Leistung von Azure SQL-Datenbank auswirkt. |
| [Fehlender Index](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | Das Problem eines fehlenden Indexes wurde erkannt, wodurch die Leistung von Azure SQL-Datenbank beeinträchtigt wird. |
| [Neue Abfrage](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Eine neue Abfrage wurde erkannt, die die allgemeine Leistung von Azure SQL-Datenbank beeinträchtigt.  |
| [Ungewöhnliche Statistik bei Wartezeiten](sql-database-intelligent-insights-troubleshoot-performance.md#unusual-wait-statistic) | Ungewöhnliche Datenbankwartezeiten wurden erkannt, welche die Leistung von Azure SQL-Datenbank beeinträchtigen. |
| [TempDB-Konflikt](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Mehrere Threads versuchen, auf die gleichen TempDB-Ressourcen zuzugreifen, was einen Engpass verursacht, der die Leistung von Azure SQL-Datenbank mindert. |
| [Mangel an DTUs im Pool für elastische Datenbanken](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Ein Mangel an verfügbaren DTUs im Pool für elastische Datenbanken wirkt sich auf die Leistung von Azure SQL-Datenbank aus. |
| [Planregression](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Ein neuer Plan oder eine Änderung bei der Workload in einem bestehenden Plan wurde festgestellt, was die Leistung von Azure SQL-Datenbank beeinträchtigt. |
| [Änderung eines Werts der datenbankweit gültigen Konfiguration](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | Eine Konfigurationsänderung in der Datenbank beeinträchtigt die Leistung von Azure SQL-Datenbank. |
| [Langsamer Client](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Ein langsamer Anwendungsclient, der nicht in der Lage ist, die Ausgabe der Azure SQL-Datenbank-Instanz schnell genug zu nutzen, wurde festgestellt. Dadurch wird die Leistung von Azure SQL-Datenbank verschlechtert. |
| [Tarifdowngrade](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Durch ein Tarifdowngrade haben sich verfügbare Ressourcen verringert, was die Leistung von Azure SQL-Datenbank mindert. |

> [!TIP]
> Zur kontinuierlichen Optimierung der Leistung von Azure SQL-Datenbank sollten Sie ggf. das Aktivieren des [Azure SQL-Datenbank-Features „Automatische Optimierung“](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-automatic-tuning) erwägen. Dieses besondere Feature bietet integrierte intelligente Funktionen zum laufenden Überwachen Ihrer Azure SQL-Datenbank-Instanz, zum automatischen Optimieren von Indizes und zum Anwenden von Korrekturen auf Abfrageausführungspläne.
>

Im folgenden Abschnitt werden die zuvor genannten erkennbaren Leistungsmuster genauer beschrieben.

## <a name="reaching-resource-limits"></a>Erreichen von Ressourcengrenzwerten

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster kombiniert Leistungsprobleme im Zusammenhang mit dem Erreichen geltender Ressourcen-, Worker- und Sitzungsgrenzwerte. Sobald dieses Leistungsproblem erkannt wird, gibt ein Beschreibungsfeld im Diagnoseprotokoll an, ob es mit Ressourcen-, Worker- oder Sitzungsgrenzwerten zusammenhängt.

Ressourcen für Azure SQL-Datenbank werden üblicherweise als [DTU-Ressourcen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-what-is-a-dtu) bezeichnet, die aus einer Kombination von CPU- und E/A-Ressourcen (Daten- und Transaktionsprotokoll-E/A) bestehen. Das Erreichen des Musters „Ressourcengrenzwerte“ wird erkannt, wenn eine aufgetretene Verschlechterung der Abfrageleistung durch Erreichen eines der gemessenen Ressourcengrenzwerte verursacht wird.

Ressourcengrenzwerte für Sitzungen beziehen sich auf die Anzahl der verfügbaren gleichzeitigen Anmeldungen bei der Azure SQL-Datenbank-Instanz. Dieses Leistungsmuster wird dann erkannt, wenn Anwendungen, die sich mit den Azure SQL-Datenbank-Instanzen verbinden, die Anzahl der verfügbaren gleichzeitigen Anmeldungen bei der Datenbank erreicht haben. Falls Anwendungen versuchen, mehr Sitzungen zu nutzen, als für eine Datenbank verfügbar sind, wird die Abfrageleistung beeinträchtigt.

Das Erreichen von Workergrenzwerten ist ein spezieller Fall des Erreichens von Ressourcengrenzwerten, da verfügbare Worker nicht in die DTU-Nutzung einbezogen werden. Das Erreichen der Workergrenzwerte einer Datenbank kann eine Erhöhung ressourcenspezifischer Wartezeiten und damit eine Verschlechterung der Abfrageleistung zur Folge haben.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Abfragehashes von Abfragen aus, die sich auf die Leistung und den prozentualen Ressourcenverbrauch ausgewirkt haben. Sie können diese Informationen als Ausgangspunkt für die Optimierung der Workload Ihrer Datenbank verwenden. Insbesondere sollten Sie das Optimieren der Abfragen, die die Leistungsverschlechterung verursachen, das Hinzufügen von Indizes oder das Optimieren von Anwendungen durch eine gleichmäßigere Workloadverteilung in Betracht ziehen. Wenn Sie nicht in der Lage sind, Workloads zu reduzieren oder Optimierungen vorzunehmen, sollten Sie ggf. den Tarif Ihres Azure SQL-Datenbank-Abonnements erhöhen, damit Sie über mehr Ressourcen verfügen.

Wenn Sie die geltenden Sitzungsgrenzwerte erreicht haben, sollten Sie die Optimierung Ihrer Anwendungen so angehen, dass die Anzahl der Anmeldungen bei der Datenbank reduziert wird. Wenn Sie die Anzahl der Anmeldungen Ihrer Anwendungen bei der Datenbank nicht reduzieren können, sollten Sie möglicherweise Ihren Datenbanktarif erhöhen. Sie können Ihre Datenbank auch aufteilen und in mehrere Datenbanken verschieben, um die Workload gleichmäßiger zu verteilen.

Weitere Vorschläge zum Beheben von Problemen mit Sitzungsgrenzwerten finden Sie unter [How to deal with the limits of Azure SQL Database maximum logins](https://blogs.technet.microsoft.com/latam/2015/06/01/how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/) (Umgang mit den maximalen Grenzwerten für Anmeldungen bei Azure SQL-Datenbank). Unter [Ressourceneinschränkungen für Azure SQL-Datenbank](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-resource-limits) finden Sie die geltenden Ressourcengrenzwerte für Ihren Abonnementtarif.

## <a name="workload-increase"></a>Gestiegene Workload

### <a name="what-is-happening"></a>Was passiert?

Dieses Leistungsmuster identifiziert Probleme, die durch eine Zunahme der Workload oder, in ihrer schwerwiegenderen Form, durch einen Aufstauen von Workloads verursacht werden.

Diese Erkennung erfolgt mithilfe einer Kombination mehrerer Metriken. Die gemessene Basismetrik ist das Erkennen eines Anstiegs der Workload im Vergleich mit der Baseline der vorherigen Workload. Die andere Form der Erkennung basiert auf der Messung einer großen Zunahme aktiver Arbeitsthreads, die groß genug ist, um die Abfrageleistung negativ zu beeinflussen.

Eine strengere Form des Problems liegt vor, wenn sich die Workload ständig aufstaut, weil die Azure SQL-Datenbank-Instanz nicht in der Lage ist, die Workload zu bewältigen. Das Ergebnis ist eine ständig wachsende Workload bzw. ein „Workloadstau“ genannter Zustand. Aufgrund dieses Zustands verlängert sich die Zeit, die die Workload auf die Ausführung wartet, was eines der gravierendsten Datenbankleistungsprobleme ist. Dieses Problem wird durch Überwachen des Anstiegs der Anzahl abgebrochener Arbeitsthreads erkannt. 

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt die Anzahl der Abfragen aus, deren Ausführung sich erhöht hat, sowie den Abfragehash der Abfrage mit dem größten Anteil an der Workloaderhöhung. Diese Informationen können Sie als Ausgangspunkt für die Optimierung der Workload und insbesondere für die Abfrage verwenden, die erwiesenermaßen den größten Anteil an der Erhöhung der Workload hat.

Sie sollten auch eine gleichmäßigere Verteilung der Workloads auf die Datenbank in Betracht ziehen. Es empfiehlt sich auch eine Optimierung der Abfrage, die sich auf die Leistung auswirkt, und das Hinzufügen von Indizes. Eine weitere Möglichkeit, die in Frage kommt, ist die Verteilung Ihrer Workload auf mehrere Datenbanken. Falls dies nicht möglich ist, sollten Sie ggf. den Tarif Ihres Azure SQL-Datenbank-Abonnements erhöhen, damit Sie über mehr Ressourcen verfügen.

## <a name="memory-pressure"></a>Hohe Arbeitsspeicherauslastung

### <a name="what-is-happening"></a>Was passiert?

Dieses Leistungsmuster ist ein Hinweis auf eine Verschlechterung der aktuellen Datenbankleistung aufgrund einer hohen Arbeitsspeicherauslastung oder, in seiner schwerwiegenderen Form, auf einen Arbeitsspeicherstau im Vergleich mit der Baseline der Leistung in den letzten sieben Tagen.

Eine hohe Arbeitsspeicherauslastung kennzeichnet eine Leistungsbedingung, bei der eine ausreichend große Anzahl von Arbeitsthreads Speicherzuweisungen in der Azure SQL-Datenbank-Instanz anfordern. Dies verursacht eine hohe Arbeitsspeicherbelegung, aufgrund der die Azure SQL-Datenbank-Instanz Arbeitsspeicher nicht effizient allen Workern zuweisen kann, die diesen anfordern. Eine der häufigsten Ursachen dieses Problems ist einerseits die Größe des Arbeitsspeichers, die der Azure SQL-Datenbank-Instanz zur Verfügung steht, und andererseits ein Anstieg der Workload, die für die Erhöhung der Arbeitsthreads und die hohe Arbeitsspeicherauslastung sorgt.

Eine schwerwiegendere Form einer hohen Arbeitsspeicherauslastung ist ein Arbeitsspeicherstau, bei dem eine höhere Anzahl von Arbeitsthreads Speicherzuweisungen anfordert, als es Abfragen gibt, die den Arbeitsspeicher freigeben. Diese Anzahl von Arbeitsthreads, die Speicherzuweisungen anfordern, kann ebenfalls kontinuierlich ansteigen (d.h. sich aufstauen), da das Modul nicht in der Lage ist, Arbeitsspeicher effizient zuzuteilen, um den Bedarf zu erfüllen. Ein Arbeitsspeicherstau stellt eines der schwerwiegendsten Datenbankleistungsprobleme dar.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt die Speicherdetails des Arbeitsspeicherobjekts aus, wobei der Arbeitsspeicherclerk als Hauptgrund für die hohe Arbeitsspeicherbelegung und die entsprechenden Zeitstempel markiert ist. Diese Informationen können als Grundlage der Problembehandlung verwendet werden. 

Sie sollten das Optimieren oder Entfernen von Abfragen im Zusammenhang mit den Arbeitsspeicherclerks mit der höchsten Speicherbelegung erwägen. Sie sollten auch sicherstellen, dass Sie keine Daten abfragen, die Sie nicht verwenden. Eine empfohlene Vorgehensweise ist, immer eine WHERE-Klausel in Ihren Abfragen anzugeben. Darüber hinaus empfiehlt es sich, nicht gruppierte Indizes zu erstellen, um die Daten zu suchen, anstatt sie zu durchsuchen.

Sie sollten auch erwägen, die Workload zu reduzieren, zu optimieren oder auf mehrere Datenbanken zu verteilen. Eine weitere Möglichkeit, die in Frage kommt, ist die Verteilung Ihrer Workload auf mehrere Datenbanken. Falls dies nicht möglich ist, sollten Sie ggf. den Tarif Ihres Azure SQL-Datenbank-Abonnements erhöhen, um die der Datenbank zur Verfügung stehenden Arbeitsspeicherressourcen zu vergrößern.

Weitere Vorschläge zur Problembehandlung finden Sie unter [Memory Grants Meditation: The mysterious SQL Server memory consumer with Many Names](https://blogs.msdn.microsoft.com/sqlmeditation/2013/01/01/memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/) (Meditation über Speicherzuweisungen: Der mysteriöse SQL Server-Arbeitsspeicherconsumer mit vielen Namen).

## <a name="locking"></a>Sperren

### <a name="what-is-happening"></a>Was passiert?

Dieses Leistungsmuster deutet auf eine Verschlechterung der aktuellen Datenbankleistung hin, da im Vergleich zur Leistungsbaseline der letzten sieben Tage übermäßige Datenbanksperren festgestellt wurden. 

In modernen Managementsystemen für relationale Datenbanken sind Sperren wesentlich für die Umsetzung von Multithreadsystemen. Bei diesen Systemen wird die Leistung maximiert, indem nach Möglichkeit mehrere gleichzeitige Workerprozesse und parallele Datenbanktransaktionen ausgeführt werden. Vereinfacht ausgedrückt bezieht sich Sperren in diesem Zusammenhang auf den integrierten Zugriffsmechanismus. Dabei kann nur ausschließlich eine Transaktion auf die benötigten Zeilen, Seiten, Tabellen und Dateien zugreifen, ohne in Konkurrenz zu anderen Transaktionen um Ressourcen zu treten. Wenn die Transaktion, die die Ressourcen für die Verwendung gesperrt hat, abgeschlossen ist, wird die Sperre für diese Ressourcen freigegeben. Daraufhin können andere Transaktionen auf die benötigen Ressourcen zugreifen. Weitere Informationen zu Sperren finden Sie unter [Sperren im Datenbankmodul](https://msdn.microsoft.com/library/ms190615.aspx).

Wenn Transaktionen, die vom SQL-Modul ausgeführt werden, längere Zeit auf den Zugriff auf Ressourcen warten, die gesperrt sind, wird die Ausführungszeit der Workload verlangsamt. 

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Details zu Sperren aus, die als Grundlage für die Problembehandlung verwendet werden können. Sie sollten die gemeldeten blockierenden Abfragen analysieren, die für die Verschlechterung der Sperrleistung verantwortlich sind, und diese entfernen. In einigen Fällen verspricht das Optimieren der blockierenden Abfragen Erfolg.
Die einfachste und sicherste Möglichkeit, das Problem zu beseitigen, besteht darin, Transaktionen kurz zu halten und den Sperrumfang der aufwändigsten Abfragen zu verkleinern. Erwägen Sie ggf. das Aufteilen eines großen Batches von Vorgängen in kleinere Vorgänge. Eine bewährte Methode zum Verringern des Umfangs von Abfragesperren ist, die Abfrage so effizient wie möglich zu gestalten. Reduzieren Sie nach Möglichkeit umfangreiche Scanvorgänge, da sie die Wahrscheinlichkeit von Deadlocks erhöhen und sich negativ auf die allgemeine Datenbankleistung auswirken. Für ermittelte Abfragen, die Sperren verursachen, sollten Sie das Erstellen neuer Indizes oder Hinzufügen von Spalten zum vorhandenen Index erwägen, um Tabellenscans zu vermeiden. Weitere Vorschläge finden Sie unter [Behebung von Blockierungsproblemen, die durch eine Sperrenausweitung in SQL Server verursacht werden](https://support.microsoft.com/en-us/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Erhöhter Wert für „Maximaler Grad an Parallelität“

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster deutet eine Bedingung an, bei der ein ausgewählter Abfrageausführungsplan übermäßig parallelisiert wurde. Der Abfrageoptimierer von Azure SQL-Datenbank erhöht die Workloadleistung, indem eine Entscheidung zur parallelen Ausführung von Abfragen getroffen wird, um Vorgänge zu beschleunigen. In einigen Fällen verbringen jedoch parallele Worker, die eine Abfrage verarbeiten, mehr Zeit damit, aufeinander zu warten, um Ergebnisse zu synchronisieren und zusammenzuführen, als wenn dieselbe Abfrage mit weniger parallelen Workern oder sogar mitunter nur in einem einzigen Arbeitsthread ausgeführt würde.

Das Expertensystem analysiert die aktuelle Datenbankleistung im Vergleich mit dem Baselinezeitraum. Es bestimmt, ob eine zuvor ausgeführte Abfrage aufgrund der übermäßigen Parallelisierung des Ausführungsplan der Abfrage langsamer als vorher ausgeführt wird.

Die Serverkonfigurationsoption „Maximaler Grad an Parallelität“ für Azure SQL-Datenbank dient zum Steuern, wie viele CPU-Kerne zum parallelen Ausführen derselben Abfrage verwendet werden können. 

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokolls gibt Abfragehashes im Zusammenhang mit den Abfragen aus, deren Dauer sich aufgrund einer übermäßigen Parallelisierung erhöht hat. Das Protokoll gibt außerdem CXP-Wartezeiten aus. Dies ist die Zeit, die ein einzelner Organisator-/Koordinatorthread (Thread 0) auf das Beenden aller anderen Threads wartet, ehe die Ergebnisse zusammengeführt werden und der Vorgang fortgesetzt wird. Darüber hinaus gibt das Diagnoseprotokoll die Zeiten aus, die Abfragen mit schlechter Leistung insgesamt auf die Ausführung gewartet haben. Diese Informationen sind die Grundlage der Behandlung dieses Problems.

Es empfiehlt sich, zunächst zu prüfen, ob komplexe Abfragen optimiert oder vereinfacht werden können. Eine bewährte Methode ist das Aufteilen langer Batchaufträge in kleinere Aufträge. Stellen Sie darüber hinaus sicher, dass Sie Indizes zur Unterstützung Ihrer Abfragen erstellt haben. Sie können auch erwägen, die Option „Maximaler Grad an Parallelität“ für eine bestimmte Abfrage zu erzwingen, die mit schlechter Leistung gekennzeichnet wurde. Dieser Vorgang wird mithilfe von T-SQL konfiguriert. Siehe dazu [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

Das Festlegen der Serverkonfigurationsoption „Max. Grad an Parallelität“ auf 0 als Standardwert bedeutet, dass Azure SQL-Datenbank alle verfügbaren logischen CPU-Kerne nutzt, um Threads zum Ausführen einer einzelnen Abfrage zu parallelisieren. Das Festlegen von „Max. Grad an Parallelität“ auf 1 bedeutet, dass nur ein Kern für eine einzelne Abfrageausführung verwendet werden kann, was in der Praxis bedeutet, dass Parallelität deaktiviert ist. Je nach vorliegendem Fall, den für die Datenbank verfügbaren Kernen und den Informationen im Diagnoseprotokoll können Sie die Option „Max. Grad an Parallelität“ auf die Anzahl von Kernen für die parallele Abfrageausführung festlegen, die das Problem in Ihrem Fall behoben hat.

## <a name="pagelatch-contention"></a>Seitenlatchkonflikt

### <a name="what-is-happening"></a>Was passiert?

Dieses Leistungsmuster zeigt die aktuelle Leistungsminderung der Datenbankworkload aufgrund eines Seitenlatchkonflikts im Vergleich zur Workloadbaseline der letzten sieben Tage.

Latches sind einfache Synchronisierungsmechanismen, die von Azure SQL-Datenbank verwendet werden, um Multithreading zu ermöglichen und die Konsistenz von Strukturen im Arbeitsspeicher wie Indizes, Datenseiten und anderen internen Strukturen zu gewährleisten.

Für Azure SQL-Datenbank sind viele Arten von Latches verfügbar. Dabei gilt Folgendes: Pufferlatches werden zum Schützen von Seiten im Arbeitsspeicher im Pufferpool verwendet. E/A-Latches werden genutzt, um Seiten zu schützen, die noch nicht in den Pufferpool geladen wurden. Wenn im Pufferpool Daten auf eine Seite geschrieben oder von ihr gelesen werden, muss ein Arbeitsthread zuerst ein Pufferlatch für die Seite erhalten. Wenn ein Arbeitsthread versucht, auf eine Seite zuzugreifen, die im Pufferpool im Arbeitsspeicher noch nicht verfügbar ist, erfolgt eine E/A-Anforderung zum Laden der benötigten Informationen aus dem Speicher. Dies ist ein Hinweis auf eine schwerwiegendere Form der Leistungsminderung.

Ein Konflikt bei den Seitenlatches tritt auf, wenn mehrere Threads gleichzeitig versuchen, Latches für dieselbe Struktur im Arbeitsspeicher abzurufen, wodurch sich die Wartezeit bis zur Ausführung der Abfrage verlängert. Bei einem E/A-Seitenlatchkonflikt, wenn auf Daten im Speicher zugegriffen werden muss, ist diese Wartezeit noch länger und kann sich erheblich auf die Workloadleistung auswirken. Ein Seitenlatchkonflikt ist das gängigste Szenario von Threads, die aufeinander warten und auf mehreren CPU-Systemen um Ressourcen konkurrieren.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt die Details des Seitenlatchkonflikts aus, die als Grundlage für die Problembehandlung verwendet werden können.

Da ein Seitenlatch ein interner Steuermechanismus von Azure SQL-Datenbank ist, wird seine Verwendung automatisch bestimmt. Entscheidungen zu Anwendungen, wie z.B. der Schemaentwurf, können es sich aufgrund des deterministischen Verhaltens von Latches auf das Verhalten von Seitenlatches auswirken.
Eine Methode zur Behandlung von Latchkonflikten ist das Ersetzen eines sequenziellen Indexschlüssels durch einen nicht sequenziellen Schlüssel, um Einfügevorgänge gleichmäßig auf einen Indexbereich zu verteilen. Typischerweise erfolgt dies mithilfe einer führenden Spalte im Index, die die Workload proportional verteilt. Eine andere mögliche Methode ist die Tabellenpartitionierung. Das Erstellen eines Hashpartitionierungsschemas mit einer berechneten Spalte für eine partitionierte Tabelle ist eine gängige Methode zur Vermeidung übermäßiger Latchkonflikte. Im Fall von E/A-Seitenlatchkonflikten wird empfohlen, Indizes einzuführen, um dieses Leistungsproblem zu beheben. Weitere Informationen finden Sie unter [Diagnosing and Resolving Latch Contention on SQL Server](http://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (PDF-Download) (Diagnostizieren und Beheben von Latchkonflikten in SQL Server).

## <a name="missing-index"></a>Fehlender Index

### <a name="what-is-happening"></a>Was passiert?

Dieses Leistungsmuster zeigt die aktuelle Leistungsminderung der Datenbankworkload aufgrund eines fehlenden Indexes im Vergleich zur Workloadbaseline der letzten sieben Tage.

Ein Index wird verwendet, um die Leistung von Abfragen zu beschleunigen. Er ermöglicht den schnellen Zugriff auf Tabellendaten bei gleichzeitiger Reduzierung der Anzahl von Datasetseiten, die besucht oder durchsucht werden müssen.

Bestimmte Abfragen, die eine Verschlechterung der Leistung verursacht haben, werden mithilfe dieser Erkennungsmethode bestimmt. Für sie wäre das Erstellen von Indizes für die Leistung von Vorteil.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Abfragehashes für die Abfragen aus, die sich negativ auf die Leistung der Workload auswirken. Für diese Abfragen bietet sich das Erstellen von Indizes an. Sie sollten außerdem das Optimieren oder Entfernen dieser Abfragen in Betracht ziehen, wenn sie nicht benötigt werden. Aus Leistungsgründen empfiehlt es sich unbedingt, das Abfragen nicht verwendeter Daten zu vermeiden.

> [!TIP]
> Wussten Sie, dass die in Azure integrierten intelligenten Funktionen automatisch die leistungsfähigsten Indizes für Ihre Datenbanken verwalten?
>
> Zur kontinuierlichen Optimierung der Leistung von Azure SQL-Datenbank sollten Sie das Aktivieren des [Azure SQL-Datenbank-Features „Automatische Optimierung“](sql-database-automatic-tuning.md) erwägen. Dieses besondere Feature bietet integrierte intelligente Funktionen zum laufenden Überwachen Ihrer Azure SQL-Datenbank-Instanz und automatischen Erstellen und Optimieren von Indizes für Ihre Datenbanken.
>

## <a name="new-query"></a>Neue Abfrage

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster deutet an, dass eine neue Abfrage erkannt wurde, die im Vergleich mit der Leistungsbaseline der letzten sieben Tage eine schwache Leistung bietet und deshalb die Workload beeinträchtigt.

Das Schreiben einer leistungsfähigen Abfrage kann mitunter eine Herausforderung darstellen. Weitere Informationen zum Schreiben von Abfragen finden Sie im Artikel zum [Schreiben von SQL-Abfragen](https://msdn.microsoft.com/library/bb264565.aspx). Informationen zum Optimieren der Leistung vorhandener Abfragen finden Sie unter [Optimieren von Abfragen](https://msdn.microsoft.com/library/ms176005.aspx).

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Informationen für bis zu zwei neuen Abfragen mit der höchsten CPU-Auslastung aus, einschließlich ihrer Abfragehashes. Da die erkannte Abfrage sich auf die Workloadleistung auswirkt, bietet sich das Optimieren Ihrer Abfrage an. Empfehlenswert ist der Verzicht auf das Abrufen von Daten, die Sie nicht verwenden. Das Verwenden von Abfragen mit WHERE-Klausel wird ebenfalls empfohlen. Das Vereinfachen komplexer Abfragen und Aufteilen in kleinere Abfragen ist auch ein guter Tipp. Außerdem ist das Unterteilen großer Batchabfragen in kleinere Batchabfragen auch eine Verbesserungsmöglichkeit. Das Einführen von Indizes für neue Abfragen ist in der Regel empfehlenswert, um dieses Leistungsproblem zu beseitigen.

Erwägen Sie den Einsatz von [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md).

## <a name="unusual-wait-statistic"></a>Ungewöhnliche Statistik bei Wartezeiten

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster deutet auf eine Verschlechterung der Workloadleistung dahingehend hin, dass im Vergleich mit einer Workloadbaseline der letzten sieben Tage Abfragen mit schlechter Leistung ausgemacht wurden.

In diesem Fall konnte das System die Abfragen mit schlechter Leistung nicht in andere standardmäßige erkennbarer Leistungskategorien einstufen. Es hat aber eine Wartestatistik erkannt, die für die Regression verantwortlich ist, und betrachtet diese deshalb als Abfragen mit *ungewöhnlicher Wartestatistik*, wobei die für die Regression zuständige Wartestatistik ebenfalls verfügbar gemacht wird. 

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Details zu ungewöhnlichen Wartezeiten, Abfragehashes der betroffenen Abfragen und Wartezeiten aus.

Da das System in diesem Fall nicht die Ursache für Abfragen mit schlechter Leistung bestimmen konnte, können die Diagnoseinformationen zu den Abfragen mit schlechter Leistung als geeigneter Ausgangspunkt für eine manuelle Problembehandlung dienen. Erwägen Sie das Optimieren der Leistung dieser Abfragen. Es empfiehlt sich in der Regel, keine Daten abzurufen, die Sie nicht verwenden, und komplexe Abfragen zu vereinfachen und in kleinere Abfragen zu unterteilen. Weitere Informationen zum Optimieren der Leistung von Abfragen finden Sie unter [Optimieren von Abfragen](https://msdn.microsoft.com/library/ms176005.aspx).

## <a name="tempdb-contention"></a>TempDB-Konflikt

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster ist ein Hinweis auf eine Datenbankleistungsbedingung, bei der es einen Engpass bei Threads gibt, die versuchen, auf TempDB-Ressourcen zuzugreifen (wobei diese Bedingung nicht E/A-bezogen ist). Das typische Szenario für dieses Leistungsproblem sind Hunderte gleichzeitiger Abfragen, die alle kleine TempDB-Tabellen erstellen, nutzen und dann löschen. Das System hat festgestellt, dass die Anzahl gleichzeitiger Abfragen unter Verwendung der gleichen TempDB-Tabellen im Vergleich mit der Leistungsbaseline der letzten sieben Tage statistisch so signifikant angestiegen ist, dass die Datenbankleistung darunter leidet.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt die Details des TempDB-Konflikts aus, die als Ausgangspunkt der Behandlung von Leistungsproblemen verwendet werden können. Es gibt mehrere Möglichkeiten, diese Art von Konflikt zu verhindern und den Gesamtdurchsatz der Workload zu erhöhen. Möglicherweise sollten Sie auf den Einsatz temporärer Tabellen verzichten. Sie sollten auch speicheroptimierte Tabellen in Erwägung ziehen. Informationen dazu finden Sie unter [Einführung in speicheroptimierte Tabellen](https://docs.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables). 

## <a name="elastic-pool-dtu-shortage"></a>Mangel an DTUs im Pool für elastische Datenbanken

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster ist ein Hinweis auf eine Verschlechterung der aktuellen Leistung der Datenbankworkload im Vergleich mit einer Baseline der letzten sieben Tage. Grund ist der Mangel an verfügbaren DTUs im Pool für elastische Datenbanken in Ihrem Abonnement. 

Ressourcen für Azure SQL-Datenbank werden üblicherweise als [DTU-Ressourcen](sql-database-what-is-a-dtu.md) bezeichnet, die aus einer Kombination von CPU- und E/A-Ressourcen (Daten- und Transaktionsprotokoll-E/A) bestehen. [Azure-Ressourcen für den Pool für elastische Datenbanken](sql-database-elastic-pool.md) dienen als Pool verfügbarer eDTU-Ressourcen, die zu Skalierungszwecken von mehreren Datenbanken gemeinsam genutzt werden. Wenn die verfügbaren eDTU-Ressourcen in Ihrem Pool elastischer Datenbanken nicht ausreichen, um alle Datenbanken im Pool zu unterstützen, wird das Problem „Mangel an DTUs im Pool für elastische Datenbanken“ vom System erkannt.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Informationen zum Pool für elastische Datenbanken aus, listet die Datenbanken mit dem höchsten DTU-Verbrauch auf und gibt einen Prozentwert der DTUs des Pools an, die von den Datenbanken mit dem höchsten Verbrauch verwendet werden.

Da sich diese Leistungsbedingung auf mehrere Datenbanken bezieht, die denselben Pool von eDTUs im Pool für elastische Datenbanken verwenden, müssen sich die Schritte zur Problembehandlung auf die Datenbanken mit dem höchsten DTU-Verbrauch konzentrieren. Erwägen Sie das Reduzieren der Workload der Datenbanken mit dem höchsten DTU-Verbrauch und das Optimieren der Abfragen dieser Datenbanken. Sie sollten auch sicherstellen, dass Sie keine Daten abfragen, die Sie nicht verwenden. Ein anderer Ansatz ist das Optimieren von Anwendungen, die die Datenbanken mit dem höchsten DTU-Verbrauch verwenden, und die Umverteilung der Workload auf mehrere Datenbanken.

Wenn die Reduzierung und Optimierung der aktuellen Workload Ihrer Datenbanken mit dem höchsten DTU-Verbrauch nicht möglich ist, ziehen Sie das Heraufstufen des Tarifs für Ihren Pool für elastische Datenbanken in Betracht. Eine solche Heraufstufung führt zur Erhöhung der im Pool für elastische Datenbanken verfügbaren DTUs.

## <a name="plan-regression"></a>Planregression

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster deutet auf eine Bedingung hin, bei der Azure SQL-Datenbank beginnt, einen suboptimalen Abfrageausführungsplan zu nutzen. Der suboptimale Plan verursacht in der Regel eine längere Abfrageausführung, was zu längeren Wartezeiten für die aktuelle und andere Abfragen führt.

Azure SQL-Datenbank bestimmt den Abfrageausführungsplan mit den geringsten Kosten für eine Abfrageausführung. Wenn sich jedoch der Typ von Abfragen und Workloads ändert, sind die vorhandenen Pläne mitunter nicht mehr effizient, oder Azure SQL-Datenbank hat keine gute Bewertung vorgenommen. Als Korrekturmaßnahme können Abfrageausführungspläne manuell erzwungen werden.

Dieses erkennbare Leistungsmuster kombiniert drei Fälle von Planregression: neue Planregression, alte Planregression und geänderte Workload in vorhandenen Plänen. Den Typ der aufgetretenen Planregression finden Sie im Diagnoseprotokoll in der Eigenschaft *details*.

Die Bedingung „Neue Planregression“ bezieht sich auf einen Zustand, in dem Azure SQL-Datenbank mit der Ausführung eines neuen Ausführungsplans beginnt, der nicht so effizient wie der alte Plan ist. Die Bedingung „Alte Planregression“ bezieht sich auf den Zustand, in dem Azure SQL-Datenbank von einem neuen, effizienteren Plan zu einem alten Plan wechselt, der nicht so effizient wie der neue Plan ist. Die Regression „Geänderte Workload in vorhandenen Plänen“ bezieht sich auf den Zustand, in dem zwei Pläne, der alte und der neue Plan, sich laufend abwechseln, wobei die Waage mehr in Richtung des Plans mit der schlechten Leistung ausschlägt.

Weitere Informationen zu Planregressionen finden Sie unter [What is plan regression in SQL server](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/06/09/what-is-plan-regression-in-sql-server/) (Was ist Planregression in SQL Server?). 

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Abfragehashes, die ID des guten Plans, die ID des schlechten Plans und Abfrage-IDs zurück, die als Grundlage der Behandlung der Leistungsprobleme bei dieser Bedingung dienen können.

Sie können auch für Ihre spezifischen Abfragen, die Sie mithilfe der bereitgestellten Abfragehashes bestimmen können, analysieren, welcher Plan leistungsfähiger ist. Nachdem Sie den Plan bestimmt haben, der für Ihre Abfragen besser funktioniert, können Sie ihn manuell erzwingen. Weitere Informationen finden Sie unter [How SQL Server prevents plan regressions](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/25/you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/) (Wie SQL Server Planregressionen verhindert).

> [!TIP]
> Wussten Sie, dass die in Azure integrierten intelligenten Funktionen automatisch die leistungsfähigsten Abfrageausführungspläne für Ihre Datenbanken verwalten?
>
> Zur kontinuierlichen Optimierung der Leistung von Azure SQL-Datenbank sollten Sie das Aktivieren des [Azure SQL-Datenbank-Features „Automatische Optimierung“](sql-database-automatic-tuning.md) erwägen. Dieses besondere Feature bietet integrierte intelligente Funktionen zum laufenden Überwachen Ihrer Azure SQL-Datenbank-Instanz und automatischen Erstellen und Optimieren der leistungsfähigsten Abfrageausführungspläne für Ihre Datenbanken.
>

## <a name="database-scoped-configuration-value-change"></a>Änderung eines Werts der datenbankweit gültigen Konfiguration

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster ist ein Hinweis auf eine Bedingung dahingehend, dass eine Änderung an der datenbankweit gültigen Konfiguration erfolgt ist, die zu einem Leistungsverlust führt. Die Bedingung wurde im Vergleich mit dem Verhalten der Datenbankworkload in den letzten sieben Tagen bestimmt. Dies bedeutet, dass die letzte Änderung an der datenbankweit gültigen Konfiguration sich anscheinend nicht positiv auf die Leistung der Datenbank auswirkt.

Die Änderung der datenbankweit gültigen Konfiguration kann für jede einzelne Datenbank festgelegt werden. Diese Konfiguration wird fallweise verwendet, um die Leistung Ihrer Datenbank individuell zu optimieren. Die folgenden Optionen können für jede einzelne Datenbank konfiguriert werden: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES und CLEAR PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Problembehandlung

Das Diagnoseprotokoll gibt Änderungen an der datenbankweit gültigen Konfiguration aus, die vor Kurzem erfolgt sind und im Vergleich mit dem Workloadverhalten der vorherigen sieben Tage die Leistungsminderung verursacht haben. Sie können die Konfigurationsänderungen auf die vorherigen Werte zurücksetzen oder die Werte einzeln optimieren, bis der gewünschte Leistungsgrad erreicht ist. Sie können auch das Kopieren von Werten einer datenbankweit gültigen Konfiguration aus einer ähnlichen Datenbank mit zufriedenstellender Leistung erwägen. Wenn die Leistungsprobleme nicht behoben werden können, ziehen Sie eine Zurücksetzung auf die Standardwerte von Azure SQL-Datenbank in Betracht, und versuchen Sie eine Optimierung ausgehend von dieser Baseline.

Weitere Informationen zum Optimieren der datenbankweit gültigen Konfiguration und zur T-SQL-Syntax zum Ändern der Konfiguration finden Sie unter [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](https://msdn.microsoft.com/en-us/library/mt629158.aspx).

## <a name="slow-client"></a>Langsamer Client

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster gibt eine Bedingung an, bei der der mit der Azure SQL-Datenbank-Instanz arbeitende Client nicht in der Lage ist, die Ausgabe der Datenbank so schnell zu nutzen, wie die Datenbank die Ergebnisse senden kann. Da Azure SQL-Datenbank Ergebnisse der ausgeführten Abfragen nicht in einem Puffer speichert, muss die Datenbank warten, bis der langsame Client die übermittelten Abfrageergebnisse nutzt, ehe fortgefahren wird. Diese Bedingung kann auch auf ein langsames Netzwerk zurückzuführen sein, das nicht in der Lage ist, die Ausgaben der Azure SQL-Datenbank-Instanz schnell genug an den nutzenden Client zu übertragen.

Diese Bedingung wird nur generiert, wenn im Vergleich mit dem Verhalten der Datenbankworkload in den letzten sieben Tagen eine Leistungsverschlechterung vorliegt. Dies stellt sicher, dass dieses Leistungsproblem nur dann erkannt wird, wenn es zu einer statistisch signifikanten Leistungsminderung im Vergleich zum vorherigen Leistungsverhalten gekommen ist.

### <a name="troubleshooting"></a>Problembehandlung

Dieses erkennbare Leistungsmuster ist ein Hinweis auf eine Bedingung auf Clientseite, wobei eine Problembehandlung in der clientseitigen Anwendung oder im clientseitigen Netzwerk erforderlich ist. Das Diagnoseprotokoll gibt die Abfragehashes und längsten Wartezeiten bei der Nutzung der Ausgabe durch den Client in den letzten beiden Stunden zurück. Diese Informationen können als Grundlage der Problembehandlung verwendet werden.

Erwägen Sie die Optimierung der Leistung Ihrer Anwendung für die Nutzung dieser Abfragen. Sie sollten auch mögliche Netzwerklatenzprobleme berücksichtigen. Da das Leistungsminderungsproblem auf einer Änderung im Vergleich mit der Leistungsbaseline der letzten sieben Tage basiert, sollten Sie untersuchen, ob es in diesem Zeitraum zu Änderungen an Anwendungen oder Netzwerkbedingungen gekommen ist, die ggf. dieses Leistungsminderungsereignis verursacht haben. 

## <a name="pricing-tier-downgrade"></a>Tarifdowngrade

### <a name="what-is-happening"></a>Was passiert?

Dieses erkennbare Leistungsmuster gibt eine Bedingung an, bei der der Tarif Ihrer Azure SQL-Datenbank-Instanz herabgestuft wurde. Aufgrund der Reduzierung der für die Datenbank verfügbaren Ressourcen (DTUs) hat das System einen Abfall der aktuellen Datenbankleistung im Vergleich mit der Baseline der letzten sieben Tage erkannt.

Darüber hinaus kann eine Bedingung vorliegen, bei der innerhalb eines kurzen Zeitraums der Tarif Ihres Azure SQL-Datenbank-Abonnements herabgestuft und anschließend auf einen höheren Tarif heraufgestuft wurde. Dies ist ein Hinweis auf die Erkennung einer temporären Leistungsminderung, die im Abschnitt „details“ des Diagnoseprotokolls als Tarifdowngrade und -upgrade ausgegeben wird.

### <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Ihren Tarif herabgestuft und sich die für die Azure SQL-Datenbank-Instanz verfügbaren DTUs verringert haben und Sie mit der Leistung zufrieden sind, müssen Sie zu diesem Zeitpunkt nichts weiter tun. Wenn Sie im Anschluss an die Herabstufung Ihres Tarifs mit der Azure SQL-Datenbank-Leistung unzufrieden sind, können Sie entweder Ihre Datenbankworkloads verringern oder den Tarif heraufstufen.

## <a name="recommended-troubleshooting-flow"></a>Empfohlene Vorgehensweise bei der Problembehandlung

Im nachstehenden Flussdiagramm finden Sie die empfohlene Vorgehensweise bei der Behandlung von Leistungsproblemen mithilfe von Intelligent Insights.

Sie greifen auf Intelligent Insights über das Azure-Portal zu, indem Sie zu Azure SQL-Analyse navigieren. Versuchen Sie, eine eingehende Leistungswarnung zu finden, und klicken Sie darauf. Ermitteln Sie auf der Seite „Erkennungen“ die Sachlage. Beachten Sie die bereitgestellte Fehlerursachenanalyse für das Problem, den Abfragetext, die Trends bei den Abfragezeiten und die Entwicklung von Incidents. Versuchen Sie, mithilfe der Empfehlung von Intelligent Insights das Leistungsproblem zu lösen. 

[![Flussdiagramm zur Problembehandlung](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> Klicken Sie auf das Flussdiagramm, um eine PDF-Version dieses Flussdiagramms herunterzuladen.

Intelligent Insights benötigt in der Regel eine Stunde für die Fehlerursachenanalyse eines Leistungsproblems. Falls Sie Ihr Problem nicht in Intelligent Insights finden (meist sind dies Probleme, die weniger als eine Stunde alt sind) und dieses Problem wichtig für Sie ist, nutzen Sie den Abfragedatenspeicher, um die Ursache des Leistungsproblems manuell zu ermitteln. Weitere Informationen finden Sie unter [Leistungsüberwachung mit dem Abfragespeicher](https://docs.microsoft.com/en-us/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store).

## <a name="next-steps"></a>Nächste Schritte
- Kennenlernen der [Intelligent Insights](sql-database-intelligent-insights.md)-Konzepte
- [Verwenden des Intelligent Insights-Diagnoseprotokolls für die Leistung von Azure SQL-Datenbank](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-sql)
- Informationen zum [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)


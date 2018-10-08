---
title: Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten | Microsoft Docs
description: Erfahren Sie, wie Sie allgemeine Leistungsprobleme mithilfe der dynamischen Verwaltungssichten zum Überwachen von Microsoft Azure SQL-Datenbank ermitteln und diagnostizieren.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 08/08/2018
ms.openlocfilehash: 97907eee9982fdf6a804bc13edbf8c14efa4ce42
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161385"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten
Die Microsoft Azure SQL-Datenbank unterstützt eine Teilmenge dynamischer Verwaltungssichten für die Diagnose von Leistungsproblemen, die auf blockierte Abfragen oder Abfragen mit langen Laufzeiten, fehlerhafte Abfragepläne usw. zurückzuführen sind. Dieses Thema enthält Informationen zum Erkennen häufiger Leistungsprobleme mithilfe von dynamischen Verwaltungssichten.

Die SQL-Datenbank unterstützt teilweise drei Kategorien von dynamischen Verwaltungssichten:

* Dynamische Verwaltungssichten für die Datenbank
* Dynamische Verwaltungssichten für die Ausführung
* Dynamische Verwaltungssichten für Transaktionen

Ausführliche Informationen zu dynamischen Verwaltungssichten finden Sie unter [Dynamische Verwaltungssichten und -funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in der SQL Server-Onlinedokumentation.

## <a name="permissions"></a>Berechtigungen
Die Abfrage einer dynamischen Verwaltungsansicht erfordert in der SQL-Datenbank die Berechtigung **VIEW DATABASE STATE** . Die Berechtigung **VIEW DATABASE STATE** gibt Informationen zu allen Objekten innerhalb der aktuellen Datenbank zurück.
Zum Erteilen der Berechtigung **VIEW DATABASE STATE** für einen bestimmten Benutzer führen Sie die folgende Abfrage aus:

```GRANT VIEW DATABASE STATE TO database_user; ```

Bei einer lokalen Instanz von SQL Server geben dynamische Verwaltungssichten Informationen zum Serverstatus zurück. In SQL-Datenbank geben sie lediglich Informationen zur aktuellen logischen Datenbank zurück.

## <a name="calculating-database-size"></a>Berechnen der Datenbankgröße
Die folgende Abfrage gibt die Größe Ihrer Datenbank (in Megabyte) zurück:

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

Die folgende Abfrage gibt die Größe der einzelnen Objekte in Ihrer Datenbank in Megabyte zurück:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Überwachen von Verbindungen
Sie können die Sicht [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) zum Abrufen von Informationen über die Verbindungen, die mit einem bestimmten Azure SQL-Datenbank-Server hergestellt wurden, sowie der Details zu den einzelnen Verbindungen verwenden. Darüber hinaus ist die Sicht [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) für das Abrufen von Informationen zu allen aktiven Benutzerverbindungen und internen Aufgaben nützlich.
Verwenden Sie die folgende Abfrage zum Abrufen der Informationen zur aktuellen Verbindung:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Wenn Sie **sys.dm_exec_requests** und **sys.dm_exec_sessions views** ausführen und die Berechtigung **VIEW DATABASE STATE** für die Datenbank besitzen, werden Ihnen alle zurzeit ausgeführten Sitzungen in der Datenbank angezeigt. Andernfalls wird Ihnen nur die aktuelle Sitzung angezeigt.
> 
> 

## <a name="monitor-resource-use"></a>Überwachen der Ressourcennutzung

Sie können die Ressourcennutzung mithilfe von [Query Performance Insight für SQL-Datenbank](sql-database-query-performance.md) und [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx) überwachen.

Außerdem lässt sich die Nutzung über diese beiden Ansichten überwachen:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Sie können die Sicht [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in jeder SQL-Datenbank verwenden. Die Sicht **sys.dm_db_resource_stats** enthält Daten zur Ressourcennutzung in der letzten Zeit relativ zur Dienstebene. Durchschnittliche Prozentsätze für CPU, Dateneingang/-ausgang, Protokollschreibvorgänge und Arbeitsspeicher werden alle 15 Sekunden aufgezeichnet und eine Stunde lang aufbewahrt.

Da diese Ansicht eine detailliertere Darstellung der Ressourcennutzung ist, sollten Sie für alle Analysen des aktuellen Zustands oder für die Problembehandlung zuerst **sys.dm_db_resource_stats** verwenden. Mit dieser Abfrage wird beispielsweise die durchschnittliche und maximale Ressourcennutzung für die aktuelle Datenbank in der letzten Stunde angezeigt:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Beispiele für andere Abfragen finden Sie unter [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats
Die Ansicht [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in der **master**-Datenbank enthält zusätzliche Informationen, die zur Überwachung der Leistung Ihrer SQL-Datenbank innerhalb des jeweiligen Diensttarifs und der Computegröße hilfreich sind. Die Daten werden alle fünf Minuten gesammelt und c.a. 14 Tage lang aufbewahrt. Diese Sicht ist für eine längere Verlaufsanalyse der Ressourcennutzung Ihrer SQL-Datenbank hilfreich.

Der folgende Graph zeigt die CPU-Ressourcennutzung für eine Premium-Datenbank mit der Computegröße P2 für jede Stunde einer Woche. Dieser Graph beginnt mit einem Montag und zeigt fünf Arbeitstage und dann das Wochenende, an dem die Anwendung deutlich weniger gefragt ist.

![Ressourcennutzung für SQL-Datenbank](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Die Daten verdeutlichen, dass diese Datenbank derzeit über eine CPU-Spitzenlast von etwas mehr als 50 Prozent CPU-Auslastung in Bezug zur Computegröße P2 verfügt (Dienstagmittag). Falls die CPU der entscheidende Faktor im Ressourcenprofil der Anwendung ist, entscheiden Sie sich ggf. für die Computegröße P2, um die Bewältigung der Workload stets sicherstellen zu können. Wenn eine Anwendung im Laufe der Zeit voraussichtlich größer wird, ist die Verwendung eines zusätzlichen Ressourcenpuffers ratsam, damit für die Anwendung nie der Grenzwert für die Leistungsebene erreicht wird. Wenn Sie die Computegröße erhöhen, können Sie für den Kunden sichtbare Fehler vermeiden. Diese können ggf. auftreten, wenn eine Datenbank nicht über genügend Leistung zum effektiven Verarbeiten von Anforderungen verfügt, vor allem in latenzsensiblen Umgebungen. Ein Beispiel ist eine Datenbank für eine Anwendung, mit der Webseiten basierend auf den Ergebnissen von Datenbankaufrufen farbig gestaltet werden.

Andere Anwendungstypen können denselben Graphen unter Umständen anders interpretieren. Wenn eine Anwendung beispielsweise jeden Tag versucht, Gehaltsabrechnungsdaten zu verarbeiten, und dasselbe Diagramm gilt, wird diese Art von „Batchauftrag“-Modell bei der Computegröße P1 wahrscheinlich zufriedenstellend ausgeführt. Computegröße P1 verfügt über 100 DTUs im Vergleich zu 200 DTUs bei Computegröße P2. Computegröße P1 stellt gegenüber Computegröße P2 die halbe Leistungsfähigkeit bereit. Eine Nutzung von 50 Prozent CPU-Auslastung bei P2 entspricht also 100 Prozent CPU-Auslastung bei P1. Wenn die Anwendung nicht über Timeouts verfügt, spielt es unter Umständen keine Rolle, ob ein Auftrag 2 oder 2,5 Stunden dauert, solange er noch am selben Tag abgeschlossen wird. Für eine Anwendung in dieser Kategorie reicht wahrscheinlich die Computegröße P1 aus. Sie können die Tatsache nutzen, dass es am Tag Zeiten gibt, in denen die Ressourcennutzung niedriger ist. Dies bedeutet, dass „Spitzen“ ggf. in einen der Zeiträume später am Tag verlagert werden können. Die Computegröße P1 ist für diese Art von Anwendung ggf. gut geeignet (und spart Kosten), solange die Aufträge jeden Tag pünktlich abgeschlossen werden können.

Azure SQL-Datenbank macht die verbrauchten Ressourceninformationen für jede aktive Datenbank in der Sicht **sys.resource_stats** der **master**-Datenbank jedes Servers verfügbar. Die Daten in der Tabelle werden zu Intervallen von fünf Minuten zusammengefasst. Bei den Dienstebenen Basic, Standard und Premium kann es länger als fünf Minuten dauern, bis sie in der Tabelle angezeigt werden. Diese Daten sind also besser für Verlaufsanalysen als für Analysen nahezu in Echtzeit geeignet. Fragen Sie die Sicht **sys.resource_stats** ab, um den kürzlichen Verlauf einer Datenbank anzuzeigen und zu überprüfen, ob die gewählte Reservierung zur gewünschten Leistung zur richtigen Zeit geführt hat.

> [!NOTE]
> Sie müssen mit der **master**-Datenbank Ihres logischen SQL-Datenbankservers verbunden sein, um **sys.resource_stats** in den folgenden Beispielen abzufragen.
> 
> 

Dieses Beispiel veranschaulicht, wie die Daten in dieser Sicht verfügbar gemacht werden:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Katalogsicht „sys.resource_stats“](./media/sql-database-performance-guidance/sys_resource_stats.png)

Das nächste Beispiel enthält unterschiedliche Möglichkeiten zum Einsatz der **sys.resource_stats**-Katalogsicht, um Informationen zur Nutzung von Ressourcen durch Ihre SQL-Datenbank zu erhalten:

1. Sie können diese Abfrage ausführen, um die Ressourcennutzung der letzten Woche für die Datenbank „userdb1“ anzuzeigen:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Um auszuwerten, wie gut Ihre Workload zur Computegröße passt, müssen Sie die einzelnen Aspekte der Ressourcenmetriken untersuchen: CPU, Lesevorgänge, Schreibvorgänge, Anzahl von Workern und Anzahl von Sitzungen. Hier wird von einer überarbeiteten Abfrage **sys.resource_stats** verwendet, um den Durchschnitt und die Höchstwerte dieser Ressourcenmetriken zu melden:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Mit diesen Informationen zu den Durchschnitts- und Höchstwerten der einzelnen Ressourcenmetriken können Sie bewerten, wie gut Ihre Workload zur ausgewählten Computegröße passt. Normalerweise erhalten Sie mit den Durchschnittswerten aus **sys.resource_stats** eine gute Grundlage gegenüber der Zielgröße. Dies sollte Ihre primäre Messlatte sein. Beispielsweise können Sie den Standarddiensttarif mit der Computegröße S2 verwenden. Die durchschnittlichen Nutzungsprozentsätze für Lese- und -Schreibvorgänge für CPU und E/A liegen unter 40 Prozent, die durchschnittliche Anzahl von Workern unter 50 und die durchschnittliche Sitzungsanzahl unter 200. Für diese Workload ist unter Umständen die Computegröße S1 geeignet. Es ist leicht zu erkennen, ob Ihre Datenbank die Grenzen für Worker und Sitzungen einhält. Um zu ermitteln, ob sich für eine Datenbank in Bezug auf CPU, Lesevorgänge und Schreibvorgänge eine niedrigere Computegröße eignet, dividieren Sie die DTU-Anzahl der niedrigeren Computegröße durch die DTU-Anzahl der aktuellen Computegröße, und multiplizieren Sie dann das Ergebnis mit 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Als Ergebnis erhalten Sie den relativen Leistungsunterschied zwischen den beiden Computegrößen in Prozent. Wenn die Ressourcennutzung diesen Betrag nicht überschreitet, kann für Ihre Workload ggf. die niedrigere Computegröße geeignet sein. Sie sollten sich aber alle Bereiche der Ressourcennutzungswerte ansehen und anhand des Prozentsatzes ermitteln, wie oft für Ihre Datenbankworkload die niedrigere Computegröße geeignet wäre. Mit der folgenden Abfrage wird der Prozentsatz für die Eignung pro Ressourcendimension basierend auf dem in diesem Beispiel berechneten Schwellenwert von 40% ausgegeben:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Auf Basis Ihres Diensttarifs für die Datenbank können Sie entscheiden, ob für Ihre Workload die niedrigere Computegröße geeignet ist. Wenn das Ziel für die Datenbankworkload 99,9 Prozent beträgt und die obige Abfrage höhere Werte als 99,9 Prozent für alle drei Ressourcendimensionen zurückgibt, ist die Wahrscheinlichkeit hoch, dass sich die niedrigere Computegröße für Ihre Workload eignet.
   
    Wenn Sie sich den Prozentsatz für die Eignung ansehen, erhalten Sie auch Informationen dazu, ob Sie zur nächsthöheren Computegröße wechseln müssen, um das Ziel zu erreichen. „userdb1“ weist beispielsweise die folgende CPU-Auslastung für die letzte Woche auf:
   
   | Durchschnittlicher CPU-Prozentwert | Maximaler CPU-Prozentwert |
   | --- | --- |
   | 24,5 |100,00 |
   
    Der durchschnittliche CPU-Wert beträgt ca. ein Viertel der Obergrenze der Computegröße. Dies würde also gut zur Computegröße der Datenbank passen. Der Höchstwert zeigt aber, dass die Datenbank die Obergrenze der Computegröße erreicht. Müssen Sie also zur nächsthöheren Computegröße wechseln? Prüfen Sie, wie häufig Ihre Workload 100 Prozent erreicht, und vergleichen Sie dies dann mit Ihrem Ziel für die Datenbankworkload.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Wenn diese Abfrage für eine der drei Ressourcendimensionen einen Wert unterhalb von 99,9 Prozent zurückgibt, können Sie entweder die Umstellung auf die nächsthöhere Computegröße erwägen oder Verfahren zur Anwendungsoptimierung nutzen, um die Last für die SQL-Datenbank zu reduzieren.
4. Bei dieser Übung wird auch die projizierte zukünftige Zunahme der Workload berücksichtigt.

In Pools für elastische Datenbanken können Sie einzelne Datenbanken mit den in diesem Abschnitt beschriebenen Methoden überwachen. Sie können den Pool jedoch auch als Ganzes überwachen. Informationen dazu finden Sie unter [Überwachen und Verwalten von Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Maximale Anzahl von gleichzeitigen Anforderungen
Um die Anzahl gleichzeitiger Anforderungen anzuzeigen, führen Sie in der SQL-Datenbank diese Transact-SQL-Abfrage aus:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Ändern Sie diese Abfrage zum Analysieren der Workload einer lokalen SQL Server-Datenbank, um nach der entsprechenden Datenbank zu filtern, die analysiert werden soll. Wenn Sie beispielsweise über eine lokale Datenbank mit dem Namen „MyDatabase“ verfügen, wird mit dieser Transact-SQL-Abfrage die Anzahl von gleichzeitigen Abfragen in dieser Datenbank zurückgegeben:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dies ist nur eine Momentaufnahme zu einem bestimmten Zeitpunkt. Um ein besseres Verständnis Ihrer Workload und der Anforderungen an gleichzeitige Anforderungen zu entwickeln, müssten Sie im Laufe der Zeit viele Beispiele sammeln.

### <a name="maximum-concurrent-logins"></a>Maximale Anzahl von gleichzeitigen Anmeldungen
Sie können Ihre Benutzer- und Anmeldungsmuster analysieren, um Informationen zur Häufigkeit der Anmeldungen zu erhalten. Sie können auch reale Auslastungen in einer Testumgebung ausführen, um sicherzustellen, dass diese oder andere in diesem Artikel beschriebene Grenzwerte nicht erreicht oder überschritten werden. Es gibt keine einzelne Abfrage oder dynamische Verwaltungssicht (DMV), mit der Sie die Anzahl von gleichzeitigen Anmeldungen oder den Verlauf anzeigen können.

Der Dienst authentifiziert jede Anmeldung, wenn mehrere Clients die gleiche Verbindungszeichenfolge verwenden. Wenn zehn Benutzer gleichzeitig mit dem gleichen Benutzernamen und Kennwort mit einer Datenbank verbunden sind, wären dies also zehn gleichzeitige Anmeldungen. Diese Obergrenze gilt nur für die Dauer der Anmeldung und Authentifizierung. Wenn sich die gleichen zehn Benutzer nacheinander an der Datenbank anmelden, wäre die Anzahl von gleichzeitigen Anmeldungen niemals höher als 1.

> [!NOTE]
> Diese Beschränkung gilt derzeit nicht für Datenbanken in Pools für elastische Datenbanken.
> 
> 

### <a name="maximum-sessions"></a>Maximale Anzahl von Sitzungen
Um die Anzahl aktueller aktiver Sitzungen anzuzeigen, führen Sie in der SQL-Datenbank diese Transact-SQL-Abfrage aus:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ändern Sie die Abfrage beim Analysieren einer lokalen SQL Server-Workload, um sie auf eine bestimmte Datenbank auszurichten. Diese Abfrage ist zum Ermitteln der möglichen Sitzungsanforderungen für die Datenbank hilfreich, wenn Sie eine Verschiebung zur Azure SQL-Datenbank erwägen.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Diese Abfragen geben wieder eine Anzahl zu einem bestimmten Zeitpunkt zurück. Wenn Sie im Laufe der Zeit mehrere Beispielwerte sammeln, können Sie sich am besten über Ihre Sitzungsnutzung informieren.

Für die SQL-Datenbankanalyse können Sie Verlaufsstatistiken für Sitzungen abrufen, indem Sie [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) abfragen und die Spalte **active_session_count** überprüfen. 

## <a name="monitoring-query-performance"></a>Überwachen der Abfrageleistung
Langsame Abfragen oder Abfragen mit langen Ausführungszeiten können beträchtliche Systemressourcen beanspruchen. In diesem Abschnitt wird veranschaulicht, wie mit dynamischen Verwaltungssichten häufig auftretende Leistungsprobleme bei Abfragen ermittelt werden können. Eine frühere, aber immer noch nützliche Referenz zur Problembehandlung ist der Microsoft TechNet-Artikel [Behandlung von Leistungsproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) .

### <a name="finding-top-n-queries"></a>Suchen der ersten N Abfragen
Das folgende Beispiel gibt Informationen über die „Top-Fünf“-Abfragen gemessen an durchschnittlicher CPU-Zeit zurück. In diesem Beispiel werden die Abfragen entsprechend ihrem Abfragenhash zusammengefasst, sodass logisch äquivalente Abfragen nach ihrem kumulativen Ressourcenverbrauch gruppiert werden.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Überwachen blockierter Abfragen
Langsame Abfragen oder Abfragen mit langer Laufzeit können zu einer übermäßigen Ressourcennutzung beitragen und auf blockierte Abfragen zurückzuführen sein. Ursache für das Blockieren kann ein mangelhafter Anwendungsentwurf, fehlerhafte Abfragepläne oder ein Mangel an nützlichen Indizes usw. sein. Verwenden Sie die sys.dm_tran_locks-Sichten, um Informationen über die aktuellen Sperraktivitäten in der Azure SQL-Datenbank abzurufen. Beispielcode finden Sie unter [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in der SQL Server-Onlinedokumentation.

### <a name="monitoring-query-plans"></a>Überwachen von Abfrageplänen
Auch ein ineffizienter Abfrageplan kann die CPU-Auslastung erhöhen. Im folgenden Beispiel wird die Sicht [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) verwendet, um festzustellen, welche Abfrage die CPU kumulativ am meisten auslastet.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Weitere Informationen
[Einführung in SQL-Datenbank](sql-database-technical-overview.md)


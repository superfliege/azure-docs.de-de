---
title: "Überwachen Sie Ihre arbeitsauslastung mit DMVs | Microsoft Docs"
description: "Erfahren Sie, wie Ihre arbeitsauslastung mit DMVs überwachen."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 7ce6c2cdf1e28852da536414533ccdcdaeb437e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-your-workload-using-dmvs"></a>Überwachen Sie Ihre arbeitsauslastung mit DMVs
Dieser Artikel beschreibt, wie Sie dynamische Verwaltungssichten (DMVs) verwenden, um die arbeitsauslastung überwachen und untersuchen Sie die Ausführung von Abfragen in Azure SQL Data Warehouse.

## <a name="permissions"></a>Berechtigungen
Wenn die DMVs in diesem Artikel abgefragt werden soll, benötigen Sie die VIEW DATABASE STATE oder CONTROL-Berechtigung. Durch Erteilen der VIEW DATABASE STATE ist in der Regel die bevorzugte Berechtigung, wie viel restriktiver ist.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitor-Verbindungen
Alle Anmeldungen in SQL Data Warehouse werden protokolliert, um [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Diese DMV enthält die letzten 10.000 Anmeldungen.  Die Session_id ist der Primärschlüssel und wird für jede neue Anmeldung sequenziell zugewiesen.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Überwachen der Ausführung von Abfragen
Alle Abfragen in SQL Data Warehouse ausgeführt werden protokolliert [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Diese DMV enthält die letzten 10.000 ausgeführten Abfragen.  Die Request_id eindeutig identifiziert wird jede Abfrage und ist der Primärschlüssel für diese DMV.  Die Request_id wird für jede neue Abfrage sequenziell zugewiesen und QID, das für die Abfrage-ID steht vorangestellt ist  Diese DMV für einen bestimmten Session_id Abfragen zeigt alle Abfragen für eine bestimmte Anmeldung.

> [!NOTE]
> Gespeicherte Prozeduren verwenden mehrere Anforderungs-IDs.  Anforderung-IDs werden in sequenzieller Reihenfolge zugewiesen. 
> 
> 

Es folgen die Schritte zu befolgen, um die Abfrageausführungspläne und Uhrzeiten für eine bestimmte Abfrage zu untersuchen.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Schritt 1: Identifizieren Sie die Abfrage, die Sie untersuchen möchten.
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Aus den vorherigen Abfrageergebnissen **Beachten Sie die Anforderungs-ID** der Abfrage, die Sie untersuchen möchten.

Abfragen in der **Suspended** Zustand wird in der Warteschlange aufgrund Parallelitätsgrenzen. Diese Abfragen werden auch in der sys.dm_pdw_waits Wartevorgänge Abfrage mit einem UserConcurrencyResourceType angezeigt. Finden Sie unter [Parallelität und die Arbeitslast Management] [ Concurrency and workload management] für Weitere Informationen zu den Parallelitätsgrenzen. Abfragen können auch warten, bis andere Gründe wie z. B. für das Objektsperren.  Wenn die Abfrage für eine Ressource wartet, finden Sie unter [untersuchen die Abfrage auf Ressourcen wartet] [ Investigating queries waiting for resources] weiter unten in diesem Artikel.

Verwenden Sie zur Vereinfachung der Suche nach einer Abfrage in der Tabelle sys.dm_pdw_exec_requests [Bezeichnung] [ LABEL] zuweisen ein Kommentars der Abfrage, die in der Ansicht sys.dm_pdw_exec_requests gesucht werden kann.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Schritt 2: Untersuchen des Abfrageplans
Verwenden Sie die Anforderungs-ID zum Abrufen der Abfrage verteilte SQL (DSQL)-Plan aus [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wenn ein Plan DSQL länger dauert ist als erwartet, kann die Ursache eines komplexen Plans mit vielen DSQL Schritte oder nur einen Schritt benötigt einen langen Zeitraum sein.  Wenn der Plan viele Schritte mit mehreren Verschiebevorgänge ist, erwägen Sie, optimieren Ihre Tabelle-Verteilungen, um die datenverschiebung zu reduzieren. Die [Tabelle Verteilung] [ Table distribution] Artikel wird erläutert, warum die Daten verschoben werden müssen, um eine Abfrage zu lösen und erläutert einige Verteilungsstrategien zum Verschieben von Daten zu minimieren.

Weitere Details zu einem einzigen Schritt Untersuchen der *Operation_type* Spalte die lang andauernde abfrageschritt, und beachten Sie die **Schrittindex**:

* Fahren Sie fort mit Schritt 3a für **SQL-Vorgänge**: OnOperation RemoteOperation, ReturnOperation.
* Fahren Sie fort mit Schritt 3 b für **Datenverschiebung Vorgänge**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Schritt 3a: Untersuchen von SQL auf verteilten Datenbanken
Verwenden Sie die Anforderungs-ID und den Schrittindex beim Abrufen der Details von [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], die Ausführungsinformationen des Schritts Abfrage auf alle verteilten Datenbanken enthält.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wenn der abfrageschritt ausgeführt wird, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kann zum Abrufen des geschätzten Plans von SQL Server aus dem Plancache SQL Server für den Schritt, die unter einer bestimmten Verteilung verwendet werden.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Schritt 3 b: Untersuchen der datenverschiebung auf verteilten Datenbanken
Verwenden Sie die Anforderungs-ID und den Schrittindex zum Abrufen von Informationen zu einem auf jede Verteilung von Daten Bewegung [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Überprüfen Sie die *Total_elapsed_time* Spalte, um festzustellen, ob eine bestimmte Verteilung wesentlich länger als andere für das Verschieben von Daten dauert.
* Überprüfen Sie für die lang andauernde Verteilung der *Rows_processed* Spalte, um festzustellen, ob die Anzahl der Zeilen aus, auf denen Verteilungspunkte verschobene wesentlich größer als die andere ist. Wenn dies der Fall ist, kann dies Neigung der zugrunde liegenden Daten hinweisen.

Wenn die Abfrage ausgeführt wird, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kann zum Abrufen des geschätzten Plans von SQL Server aus dem Plancache für die SQL Server für den aktuell ausgeführten SQL-Schritt innerhalb einer bestimmten Verteilung verwendet werden.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Überwachen der wartende Abfragen
Wenn Sie feststellen, dass die Abfrage nicht Fortschritte erzielt wird, da sie für eine Ressource wartet, wird hier eine Abfrage, die alle Ressourcen, die anzeigt, dass eine Abfrage wartet.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Wenn die Abfrage auf die Ressourcen von einer anderen Abfrage aktiv wartet, dann ist des Status **AcquireResources**.  Wenn die Abfrage enthält die erforderlichen Ressourcen, die Status werden **gewährt**.

## <a name="monitor-tempdb"></a>Monitor tempdb
Hohe Tempdb-Auslastung kann die Ursache für eine geringe Leistung und unzureichenden Arbeitsspeicher sein. Zuerst überprüfen Sie, ob Sie Data skew oder schlechter Qualität Zeilengruppen haben, und führen Sie die entsprechenden Aktionen. Erwägen Sie, Ihr Datawarehouse Skalierung, wenn Sie Tempdb erreichen seine Grenzen beim Ausführen der Abfrage finden. Im folgenden wird beschrieben, wie Tempdb-Auslastung pro Abfrage auf jedem Knoten zu identifizieren. 

Erstellen Sie die folgende Ansicht aus, um die entsprechenden Knoten-Id für sys.dm_pdw_sql_requests zuzuordnen. Dadurch können Sie andere Pass-Through-DMVs nutzen, und verknüpfen diese Tabellen mit sys.dm_pdw_sql_requests.

```sql
-- sys.dm_pdw_sql_requests with the correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
Führen Sie die folgende Abfrage aus, um Tempdb zu überwachen:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>Überwachen des Arbeitsspeichers

Arbeitsspeicher kann die Ursache für eine geringe Leistung und unzureichenden Arbeitsspeicher sein. Zuerst überprüfen Sie, ob Sie Data skew oder schlechter Qualität Zeilengruppen haben, und führen Sie die entsprechenden Aktionen. Erwägen Sie, Ihr Datawarehouse Skalierung, wenn Sie SQL Server-speicherauslastung erreichen seine Grenzen beim Ausführen der Abfrage finden.

Die folgende Abfrage gibt die SQL Server Verwendung und ungenügendem Arbeitsspeicher pro Knoten zurück:   
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>Überwachen der Größe des Transaktionsprotokolls
Die folgende Abfrage gibt die Größe des Transaktionsprotokolls auf jede Verteilung zurück. Überprüfen Sie, ob Sie Data skew oder schlechter Qualität Zeilengruppen haben, und führen Sie die entsprechenden Aktionen. Wenn eine der Protokolldateien 160 GB erreicht ist, sollten Sie Zentrales Skalieren Ihrer Instanz oder beschränken die Transaktionsgröße. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>Überwachen von Transaktionsrollback-Protokoll
Wenn Ihre Abfragen fehlschlagen oder sehr lange dauert, um den Vorgang fortzusetzen, können Sie überprüfen und überwachen, wenn Sie alle Transaktionen ein Rollback verfügen.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>Nächste Schritte
Finden Sie unter [Systemsichten] [ System views] Weitere Informationen zu DMVs.
Finden Sie unter [bewährte SQL Data Warehouse] [ SQL Data Warehouse best practices] für Weitere Informationen zu bewährten Methoden

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx

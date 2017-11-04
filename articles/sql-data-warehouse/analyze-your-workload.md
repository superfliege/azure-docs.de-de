---
title: "Analysieren Ihrer Workload – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Lernen Sie Techniken zum Analysieren von Abfrageprioritäten für Ihre Workload in Azure SQL Data Warehouse kennen."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 98617f6b8366662e52d00420adc4c81abffc598d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="analyze-your-workload"></a>Analysieren Ihrer Workload
Lernen Sie Techniken zum Analysieren von Abfrageprioritäten für Ihre Workload in Azure SQL Data Warehouse kennen.

## <a name="workload-groups"></a>Workloadgruppen 
SQL Data Warehouse implementiert Ressourcenklassen anhand von Workloadgruppen. Es gibt insgesamt acht Workloadgruppen, die das Verhalten der Ressourcenklassen über die verschiedenen DWU-Größen hinweg steuern. In jeder DWU verwendet SQL Data Warehouse nur vier der acht Workloadgruppen. Dies ergibt Sinn, da jede Workloadgruppe einer der vier Ressourcenklassen „smallrc“, „mediumrc“, „largerc“ oder „xlargerc“ zugewiesen ist. Sie müssen diese Workloadgruppen verstehen, da für einige dieser Workloadgruppen eine höhere *Wichtigkeit*festgelegt ist. Die Wichtigkeit wird für die CPU-Zeitplanung verwendet. Abfragen, die mit hoher Wichtigkeit ausgeführt werden, erhalten dreimal mehr CPU-Zyklen als Abfragen mit mittlerer Wichtigkeit. Daher wird anhand von Parallelitätsslotzuordnungen auch die CPU-Priorität bestimmt. Wenn für eine Abfrage 16 oder mehr Slots verwendet werden, ist die Wichtigkeit hoch.

In der folgenden Tabelle sind die Wichtigkeitszuordnungen für die einzelnen Workloadgruppen angegeben.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Zuordnungen von Workloadgruppen zu Parallelitätsslots und Wichtigkeit

| Workloadgruppen | Zuordnung von Parallelitätsslots | MB/Verteilung (Elastizität) | MB/Verteilung (Compute) | Wichtigkeitszuordnung |
|:---------------:|:------------------------:|:------------------------------:|:---------------------------:|:------------------:|
| SloDWGroupC00   | 1                        |    100                         | 250                         | Mittel             |
| SloDWGroupC01   | 2                        |    200                         | 500                         | Mittel             |
| SloDWGroupC02   | 4                        |    400                         | 1000                        | Mittel             |
| SloDWGroupC03   | 8                        |    800                         | 2000                        | Mittel             |
| SloDWGroupC04   | 16                       |  1.600                         | 4000                        | Hoch               |
| SloDWGroupC05   | 32                       |  3.200                         | 8.000                        | Hoch               |
| SloDWGroupC06   | 64                       |  6.400                         | 16.000                      | Hoch               |
| SloDWGroupC07   | 128                      | 12.800                         | 32.000                      | Hoch               |
| SloDWGroupC08   | 256                      | 25.600                         | 64.000                      | Hoch               |

<!-- where are the allocation and consumption of concurrency slots charts? -->
In der Tabelle **Zuordnung und Verbrauch von Parallelitätsslots** sehen Sie, dass ein DW500 jeweils 1, 4, 8 bzw. 16 Parallelitätsslots für smallrc-, mediumrc-, largerc- und xlargerc-Ressourcenklassen verwendet. Schlagen Sie diese Werte in der vorherigen Tabelle nach, um die Wichtigkeit für jede Ressourcenklasse zu ermitteln.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500-Zuordnung zwischen Ressourcenklassen und Wichtigkeit
| Ressourcenklasse | Workloadgruppe | Verwendete Parallelitätsslots | MB/Verteilung | Wichtigkeit |
|:-------------- |:-------------- |:----------------------:|:-----------------:|:---------- |
| smallrc        | SloDWGroupC00  | 1                      | 100               | Mittel     |
| mediumrc       | SloDWGroupC02  | 4                      | 400               | Mittel     |
| largerc        | SloDWGroupC03  | 8                      | 800               | Mittel     |
| xlargerc       | SloDWGroupC04  | 16                     | 1.600             | Hoch       |
| staticrc10     | SloDWGroupC00  | 1                      | 100               | Mittel     |
| staticrc20     | SloDWGroupC01  | 2                      | 200               | Mittel     |
| staticrc30     | SloDWGroupC02  | 4                      | 400               | Mittel     |
| staticrc40     | SloDWGroupC03  | 8                      | 800               | Mittel     |
| staticrc50     | SloDWGroupC03  | 16                     | 1.600             | Hoch       |
| staticrc60     | SloDWGroupC03  | 16                     | 1.600             | Hoch       |
| staticrc70     | SloDWGroupC03  | 16                     | 1.600             | Hoch       |
| staticrc80     | SloDWGroupC03  | 16                     | 1.600             | Hoch       |

## <a name="view-workload-groups"></a>Anzeigen von Workloadgruppen
Sie können die folgende DMV-Abfrage verwenden, um die Unterschiede bei der Arbeitsspeicherressourcen-Zuweisung detailliert aus Sicht des Resource Governors anzuzeigen. Außerdem können Sie die derzeitige und zurückliegende Nutzung der Workloadgruppen bei der Problembehandlung analysieren.

```sql
WITH rg
AS
(   SELECT  
     pn.name                                AS node_name
    ,pn.[type]                              AS node_type
    ,pn.pdw_node_id                         AS node_id
    ,rp.name                                AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                                AS group_name
    ,wg.importance                          AS group_importance
    ,wg.request_max_memory_grant_percent    AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                             AS group_max_dop
    ,wg.effective_max_dop                   AS group_effective_max_dop
    ,wg.total_request_count                 AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
    AND     rp.name    = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
        node_name
,       group_request_max_memory_grant_pcnt
,       group_importance
;
```

## <a name="queued-query-detection-and-other-dmvs"></a>Erkennung von Abfragen in der Warteschlange und andere DMVs
Sie können die DMV `sys.dm_pdw_exec_requests` verwenden, um Abfragen zu identifizieren, die in eine Parallelitätswarteschlange eingereiht wurden. Abfragen, die auf einen Parallelitätsslot warten, weisen den Status **Angehalten**auf.

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

Workloadverwaltungsrollen können mit `sys.database_principals`angezeigt werden.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

Mit der folgenden Abfrage wird angezeigt, welcher Rolle die einzelnen Benutzer zugewiesen sind.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL Data Warehouse verfügt über die folgenden Wartetypen:

* **LocalQueriesConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich außerhalb des Parallelitätsslot-Frameworks befinden. DMV-Abfragen und Systemfunktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
* **UserConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich innerhalb des Parallelitätsslot-Frameworks befinden. Abfragen für Endbenutzertabellen sind ein Beispiel für die Verwendung dieses Ressourcentyps.
* **DmsConcurrencyResourceType**: Bezieht sich auf Wartezeiten, die sich aufgrund von Datenverschiebungen ergeben.
* **BackupConcurrencyResourceType**: Diese Wartezeit gibt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn mehrere Sicherungen gleichzeitig angefordert wurden, werden die restlichen Sicherungen in die Warteschlange eingereiht.

Mit der DMV `sys.dm_pdw_waits` kann angezeigt werden, auf welche Ressourcen eine Abfrage wartet.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

Mit der DMV `sys.dm_pdw_resource_waits` werden nur die Ressourcenwartezeiten einer bestimmten Abfrage angezeigt. Mit der Ressourcenwartezeit wird nur gemessen, wie lange auf die Bereitstellung von Ressourcen gewartet wird. Bei der Signalwartezeit (Signal Wait Time) wird dagegen gemessen, wie lange die zugrunde liegenden SQL Server-Instanzen zum Planen der Abfrage in der CPU benötigen.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```
Sie können auch die DMV `sys.dm_pdw_resource_waits` verwenden, um die Anzahl der gewährten Parallelitätsslots zu berechnen.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots 
FROM    sys.[dm_pdw_resource_waits] 
WHERE   [state]           = 'Granted' 
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

Die DMV `sys.dm_pdw_wait_stats` kann für Verlaufsanalysen von Wartezeiten verwendet werden.

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Datenbankbenutzern und der Sicherheit finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Weitere Informationen dazu, wie größere Ressourcenklassen die Qualität von gruppierten Columnstore-Indizes verbessern können, finden Sie unter [Neuerstellen von Indizes zur Verbesserung der Segmentqualität].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes zur Verbesserung der Segmentqualität]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->

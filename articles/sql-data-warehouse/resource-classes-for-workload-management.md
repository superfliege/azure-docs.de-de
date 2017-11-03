---
title: "Ressourcenklassen für die Workloadverwaltung – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Enthält eine Anleitung für die Verwendung von Ressourcenklassen zum Verwalten der Parallelität und von Computeressourcen für Abfragen in Azure SQL Data Warehouse."
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
ms.openlocfilehash: 122646f73b6e4e7c62eb0e6d4b6672b603d8acb2
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="resource-classes-for-workload-management"></a>Ressourcenklassen für die Workloadverwaltung
Dieser Artikel enthält eine Anleitung für die Verwendung von Ressourcenklassen zum Verwalten der Anzahl von gleichzeitigen Abfragen und von Computeressourcen für Abfragen in Azure SQL Data Warehouse.
 
## <a name="what-is-workload-management"></a>Worum geht es bei der Workloadverwaltung?
Die Workloadverwaltung ist die Möglichkeit zum Optimieren der Gesamtleistung aller Abfragen. Bei einer ausreichenden Optimierung der Workload werden Abfragen und Ladevorgänge unabhängig davon, ob sie rechenintensiv oder E/A-intensiv sind, effizient ausgeführt. 

SQL Data Warehouse enthält Workloadverwaltungsfunktionen für Umgebungen mit mehreren Benutzern. Ein Data Warehouse ist nicht für Workloads mit mehreren Mandanten ausgelegt.

## <a name="what-are-resource-classes"></a>Was sind Ressourcenklassen?
Ressourcenklassen sind im Voraus festgelegte Ressourceneinschränkungen zur Steuerung der Abfragenausführung. SQL Data Warehouse schränkt die Computeressourcen für jede Abfrage gemäß der Ressourcenklasse ein. 

Mit Ressourcenklassen können Sie die Gesamtleistung Ihrer Data Warehouse-Workload verwalten. Die Verwendung von Ressourcenklassen erleichtert Ihnen das Verwalten der Workload, indem Sie die Anzahl von Abfragen, die gleichzeitig ausgeführt werden, und die den Abfragen jeweils zugewiesenen Computeressourcen einschränken. 

- Für kleinere Ressourcenklassen werden weniger Computeressourcen verwendet, aber sie ermöglichen eine höhere allgemeine Parallelität von Abfragen.
- Bei größeren Ressourcenklassen sind mehr Computeressourcen verfügbar, aber die Parallelität von Abfragen ist eingeschränkt.

Ressourcenklassen sind für Datenverwaltungs- und -änderungsaktivitäten ausgelegt. Einige sehr komplexe Abfragen profitieren auch vom Vorhandensein großer Verknüpfungen und Sortierungen, um zu erreichen, dass das System die Abfrage im Arbeitsspeicher ausführt und nicht auf den Datenträger überlaufen lässt.

Die folgenden Vorgänge werden von Ressourcenklassen gesteuert:

* INSERT-SELECT, UPDATE, DELETE
* SELECT (wenn Benutzertabellen abgefragt werden)
* ALTER INDEX – REBUILD oder REORGANIZE
* ALTER TABLE REBUILD
* CREATE INDEX
* CREATE CLUSTERED COLUMNSTORE INDEX
* CREATE TABLE AS SELECT (CTAS)
* Laden von Daten
* Datenverschiebungsvorgänge, die vom DMS (Data Movement Service) ausgeführt werden

> [!NOTE]  
> SELECT-Anweisungen für dynamische Verwaltungssichten (Dynamic Management Views, DMVs) oder andere Systemsichten werden nicht durch Einschränkungen der Parallelität gesteuert. Sie können das System unabhängig von der Anzahl der darin ausgeführten Abfragen überwachen.
> 
> 

## <a name="static-and-dynamic-resource-classes"></a>Statische und dynamische Ressourcenklassen

Es gibt zwei Arten von Ressourcenklassen: dynamisch und statisch.

- Bei **statischen Ressourcenklassen** wird unabhängig von der aktuellen Dienstebene die gleiche Menge an Arbeitsspeicher zugeteilt. Dies wird in [Data Warehouse-Einheiten](what-is-a-data-warehouse-unit-dwu-cdwu.md) (Data Warehouse Units, DWUs) gemessen. Diese statische Zuteilung bedeutet für höhere Dienstebenen, dass Sie in jeder Ressourcenklasse mehr Abfragen ausführen können.  Die statischen Ressourcenklassen haben die Namen staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70 und staticrc80. Diese Ressourcenklassen eignen sich am besten für Lösungen, bei denen die Ressourcenklasse heraufgesetzt wird, um zusätzliche Computeressourcen zu erhalten.

- Bei **dynamischen Ressourcenklassen** wird je nach aktueller Dienstebene eine variable Menge an Arbeitsspeicher zugeteilt. Ihre Abfragen erhalten so automatisch mehr Arbeitsspeicher, wenn Sie zentral auf eine höhere Dienstebene hochskalieren. Die dynamischen Ressourcenklassen haben die Namen smallrc, mediumrc, largerc und xlargerc. Diese Ressourcenklassen sind am besten für Lösungen geeignet, bei denen die Computeskalierung erhöht wird, um zusätzliche Ressourcen zu erhalten. 

Für die [Leistungsstufen](performance-tiers.md) werden die gleichen Ressourcenklassennamen verwendet, aber unterschiedliche [Spezifikationen für den Arbeitsspeicher und die Parallelität](performance-tiers.md). 


## <a name="assigning-resource-classes"></a>Zuweisen von Ressourcenklassen

Ressourcenklassen werden implementiert, indem Benutzer Datenbankrollen zugewiesen werden. Wenn ein Benutzer eine Abfrage ausführt, wird die Abfrage mit der Ressourcenklasse des Benutzers ausgeführt. Ist ein Benutzer beispielsweise Mitglied der Datenbankrolle smallrc oder staticrc10 ist, werden die dazugehörigen Abfragen mit kleinen Arbeitsspeichermengen ausgeführt. Wenn ein Datenbankbenutzer Mitglied der Datenbankrollen xlargerc oder staticrc80 ist, werden die dazugehörigen Abfragen mit großen Arbeitsspeichermengen ausgeführt. 

Verwenden Sie die gespeicherte Prozedur [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql), um die Ressourcenklasse eines Benutzers heraufzusetzen. 

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Verwenden Sie [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql), um die Ressourcenklasse herabzusetzen.  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

Die Ressourcenklasse des Dienstadministrators ist festgelegt und kann nicht geändert werden.  Der Dienstadministrator ist der Benutzer, der während des Bereitstellungsprozesses erstellt wird.

### <a name="default-resource-class"></a>Standardressourcenklasse
Standardmäßig ist jeder Benutzer Mitglied der kleinen Ressourcenklasse **smallrc**. 

### <a name="resource-class-precedence"></a>Rangfolge von Ressourcenklassen
Benutzer können Mitglieder mehrerer Ressourcenklassen sein. Wenn ein Benutzer mehr als einer Ressourcenklasse angehört:

- Dynamische Ressourcenklassen haben Vorrang vor statischen Ressourcenklassen. Wenn ein Benutzer beispielsweise sowohl Mitglied von mediumrc(dynamic) als auch von staticrc80 (static) ist, werden die Abfragen mit mediumrc ausgeführt.
- Größere Ressourcenklassen haben Vorrang vor kleineren Ressourcenklassen. Wenn ein Benutzer beispielsweise Mitglied von mediumrc und largerc ist, werden Abfragen mit largerc ausgeführt. Wenn ein Benutzer sowohl Mitglied von staticrc20 als auch von statirc80 ist, werden Abfragen entsprechend mit staticrc80-Ressourcenzuteilungen ausgeführt.

### <a name="queries-exempt-from-resource-classes"></a>Aus Ressourcenklassen ausgeschlossene Abfragen
Einige Abfragen werden immer in der Ressourcenklasse smallrc ausgeführt, und zwar auch dann, wenn der Benutzer Mitglied einer größeren Ressourcenklasse ist. Diese ausgenommenen Abfragen werden für das Parallelitätslimit nicht berücksichtigt. Wenn das Parallelitätslimit beispielsweise auf 16 festgelegt ist, können viele Benutzer eine Auswahl aus Systemsichten treffen, ohne dass sich dies auf die verfügbaren Parallelitätsslots auswirkt.

Die folgenden Anweisungen sind von Ressourcenklassen ausgenommen und werden immer in smallrc ausgeführt:

* CREATE oder DROP TABLE
* ALTER TABLE ... SWITCH, SPLIT oder MERGE PARTITION
* ALTER INDEX DISABLE
* DROP INDEX
* CREATE, UPDATE oder DROP STATISTICS
* TRUNCATE TABLE
* ALTER AUTHORIZATION
* CREATE LOGIN
* CREATE, ALTER oder DROP USER
* CREATE, ALTER oder DROP PROCEDURE
* CREATE oder DROP VIEW
* INSERT VALUES
* SELECT aus Systemsichten und DMVs
* EXPLAIN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="recommendations"></a>Recommendations
Wir empfehlen Ihnen, einen Benutzer zu erstellen, der speziell für das Ausführen eines bestimmten Typs von Abfrage oder Ladevorgang festgelegt ist. Versehen Sie den Benutzer dann mit einer permanenten Ressourcenklasse, anstatt die Ressourcenklasse häufig zu ändern. Da bei statischen Ressourcenklassen eine stärkere umfassende Steuerung der Workload möglich ist, sollten Sie diese zuerst verwenden, bevor Sie den Einsatz von dynamischen Ressourcenklassen erwägen.

### <a name="resource-classes-for-load-users"></a>Ressourcenklassen für Benutzer, die Ladevorgänge durchführen
Für `CREATE TABLE` werden standardmäßig Columnstore-Indizes verwendet. Das Komprimieren von Daten in einem Columnstore-Index ist ein speicherintensiver Vorgang, und eine hohe Arbeitsspeicherauslastung kann dazu führen, dass die Indexqualität nicht optimal ist. Aus diesem Grund benötigen Sie beim Laden von Daten voraussichtlich eine höhere Ressourcenklasse. Gehen Sie wie folgt vor, um sicherzustellen, dass für Ladevorgänge genügend Arbeitsspeicher vorhanden ist: Erstellen Sie einen Benutzer, der für das Ausführen von Ladevorgängen vorgesehen ist, und weisen Sie diesen Benutzer einer höheren Ressourcenklasse zu.

Der zum effizienten Verarbeiten von Lasten erforderliche Arbeitsspeicher richtet sich nach der Art der geladenen Tabelle und der Datengröße. Weitere Informationen zu Arbeitsspeicheranforderungen finden Sie unter [Maximieren der Zeilengruppenqualität für Columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Nachdem Sie die Arbeitsspeicheranforderung ermittelt haben, können Sie auswählen, ob der Benutzer für Ladevorgänge einer statischen oder dynamischen Ressourcenklasse zugewiesen werden soll.

- Verwenden Sie eine statische Ressourcenklasse, wenn Tabellenspeicheranforderungen in einen bestimmten Bereich fallen. Ladevorgänge werden mit dem passenden Arbeitsspeicher ausgeführt. Wenn Sie das Data Warehouse skalieren, benötigen die Ladevorgänge nicht mehr Arbeitsspeicher. Mit einer statischen Ressourcenklasse bleiben die Arbeitsspeicherzuteilungen konstant. Bei diesem konsistenten Ansatz wird Arbeitsspeicher gespart, und es können mehr Abfragen gleichzeitig ausgeführt werden. Wir empfehlen Ihnen, für neue Lösungen zuerst statische Ressourcenklassen zu verwenden, da diese eine bessere Kontrolle ermöglichen.
- Nutzen Sie eine dynamische Ressourcenklasse, wenn die Tabellenspeicheranforderungen stark variieren. Für Ladevorgänge ist unter Umständen mehr Arbeitsspeicher erforderlich, als von der aktuellen DWU- oder cDWU-Ebene bereitgestellt werden. Durch das Skalieren des Data Warehouse wird daher mehr Arbeitsspeicher für Ladevorgänge hinzugefügt, sodass diese Vorgänge schneller durchgeführt werden können.

### <a name="resource-classes-for-queries"></a>Ressourcenklassen für Abfragen

Einige Abfragen sind rechenintensiv, während dies für andere nicht zutrifft.  

- Wählen Sie eine dynamische Ressourcenklasse, wenn Abfragen komplex sind, aber keine hohe Parallelität benötigen.  Das Erstellen von täglichen oder wöchentlichen Berichten ist für Ressourcen beispielsweise eine nur gelegentlich auftretende Anforderung. Wenn für die Berichte große Datenmengen verarbeitet werden, wird durch das Skalieren des Data Warehouse mehr Arbeitsspeicher für die vorhandene Ressourcenklasse des Benutzers bereitgestellt.
- Wählen Sie eine statische Ressourcenklasse, wenn sich die Anforderungen an Ressourcen im Laufe des Tages immer wieder ändern. Eine statische Ressourcenklasse ist beispielsweise gut geeignet, wenn das Data Warehouse von vielen Benutzern abgefragt wird. Beim Skalieren des Data Warehouse kommt es nicht zu einer Änderung der Arbeitsspeichermenge, die dem Benutzer zugeteilt ist. Daher können für das System mehr Abfragen parallel ausgeführt werden.

Die Wahl der richtigen Speicherzuweisung hängt von vielen Faktoren ab, z.B. der Menge der abgefragten Daten, der Art der Tabellenschemas und von verschiedenen Prädikaten für das Verknüpfen, Auswählen und Gruppieren. Im Allgemeinen ermöglicht das Zuteilen von mehr Arbeitsspeicher eine schnellere Durchführung von Abfragen, aber die allgemeine Parallelität wird verringert. Wenn Parallelität keine Rolle spielt, hat eine übermäßige Arbeitsspeicherzuweisung keine negativen Auswirkungen auf den Durchsatz. 

Verwenden Sie zum Optimieren der Leistung unterschiedliche Ressourcenklassen. Im nächsten Abschnitt ist eine gespeicherte Prozedur angegeben, mit der Sie die beste Ressourcenklasse ermitteln können.

## <a name="example-code-for-finding-the-best-resource-class"></a>Beispielcode zum Ermitteln der besten Ressourcenklasse
 
Mithilfe der folgenden gespeicherten Prozedur können Sie Parallelitäts- und Arbeitsspeicherzuweisung pro Ressourcenklasse für ein bestimmtes Servicelevelziel sowie die naheliegend beste Ressourcenklasse für speicherintensive CCI-in einer nicht partitionierten CCI-Tabelle für eine bestimmte Ressourcenklasse ermitteln:

Hier wird der Zweck dieser gespeicherten Prozedur beschrieben:  
1. Anzeigen der Parallelitäts- und Speicherzuweisung pro Ressourcenklasse für ein bestimmtes Servicelevelziel. Der Benutzer muss hierzu wie im Beispiel gezeigt NULL als Schema und Tabellenname angeben.  
2. Anzeigen der am besten geeigneten Ressourcenklasse für speicherintensive CCI-Vorgänge (Laden, Kopieren einer Tabelle, Indexneuerstellung usw.) in einer nicht partitionierten CCI-Tabelle für eine bestimmte Ressourcenklasse. Die gespeicherte Prozedur verwendet ein Tabellenschema, um die erforderliche Speicherzuweisung zu ermitteln.

### <a name="dependencies--restrictions"></a>Abhängigkeiten und Einschränkungen:
- Diese gespeicherte Prozedur dient nicht zum Berechnen des Arbeitsspeicherbedarfs für eine partitionierte CCI-Tabelle.    
- Diese gespeicherte Prozedur berücksichtigt keine Arbeitsspeicheranforderungen für den SELECT-Teil von CTAS/INSERT-SELECT und geht davon aus, dass es sich um SELECT handelt.
- Für diese gespeicherte Prozedur wird eine temporäre Tabelle verwendet, die in der Sitzung verfügbar ist, in der diese gespeicherte Prozedur erstellt wurde.    
- Diese gespeicherte Prozedur hängt von den aktuellen Angeboten (z.B. Hardwarekonfiguration, DMS-Konfiguration) ab und funktioniert nicht richtig, wenn eines davon geändert wird.  
- Diese gespeicherte Prozedur hängt vom vorhandenen angebotenen Parallelitätslimit ab und funktioniert nicht richtig, wenn dieses geändert wird.  
- Diese gespeicherte Prozedur hängt von vorhandenen Ressourcenklassenangeboten ab und funktioniert nicht richtig, wenn diese geändert werden.  

>  [!NOTE]  
>  Wenn Sie nach der Ausführung der gespeicherten Prozedur mit den bereitgestellten Parametern keine Ausgabe erhalten, kann dies zwei Gründe haben. <br />1. Der DW-Parameter enthält einen ungültigen SLO-Wert. <br />2. Für den CCI-Vorgang in der Tabelle ist keine passende Ressourcenklasse vorhanden. <br />Beispielsweise beträgt bei DW100 die höchste verfügbare Arbeitsspeicherzuweisung auch dann 400 MB, wenn das Tabellenschema breit genug ist, um die Anforderung von 400 MB zu überschreiten.
      
### <a name="usage-example"></a>Verwendungsbeispiel:
Syntax:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:: Gibt entweder einen NULL-Parameter zum Extrahieren der aktuellen DWU aus der DW DB oder eine beliebige unterstützte DWU im Format „DW100“ an.
2. @SCHEMA_NAME:: Gibt einen Schemanamen der Tabelle an.
3. @TABLE_NAME:: Gibt einen relevanten Tabellennamen an.

Beispiele für Ausführung dieser gespeicherte Prozedur:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Mit der folgenden Anweisung wird Table1 erstellt, die in den vorherigen Beispielen verwendet wird.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>Definition der gespeicherten Prozedur

```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temp table (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Datenbankbenutzern und der Sicherheit finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Weitere Informationen dazu, wie größere Ressourcenklassen die Qualität des gruppierten Columnstore-Index verbessern können, finden Sie unter [Arbeitsspeicheroptimierung für Columnstore-Komprimierung](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->

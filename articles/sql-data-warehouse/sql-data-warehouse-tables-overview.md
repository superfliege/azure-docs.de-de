---
title: "Einführung in das Entwerfen von Tabellen – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Einführung in das Entwerfen von Tabellen in Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 01/05/2018
ms.author: barbkess
ms.openlocfilehash: 8e48d771ffcefe31c89a0d70f65ca867653a2163
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2018
---
# <a name="introduction-to-designing-tables-in-azure-sql-data-warehouse"></a>Einführung in das Entwerfen von Tabellen in Azure SQL Data Warehouse

Erfahren Sie mehr über die Schlüsselkonzepte des Tabellenentwurfs in Azure SQL Data Warehouse. 

## <a name="determining-table-category"></a>Bestimmen der Tabellenkategorie 

In einem [Sternschema](https://en.wikipedia.org/wiki/Star_schema) werden Daten in Fakten-und Dimensionstabellen organisiert. Einige Tabellen werden für die Integration oder das Staging von Daten verwendet, bevor sie in eine Fakten- oder Dimensionstabelle verschoben werden. Wenn Sie eine Tabelle entwerfen, entscheiden Sie, ob die Tabellendaten in einer Faktentabelle, eine Dimensionstabelle oder eine Integrationstabelle gehören. Diese Entscheidung bestimmt die Tabellenstruktur und die Verteilung. 

- **Faktentabellen** enthalten quantitative Daten, die häufig in einem transaktionalen System generiert und dann in das Data Warehouse geladen werden. Beispielsweise generiert ein Einzelhandelsunternehmen täglich Verkaufstransaktionen und lädt dann die Daten zur Analyse in eine Faktentabelle im Data Warehouse.

- **Dimensionstabellen** enthalten Attributdaten, die sich ändern können, es in der Regel jedoch nur selten tun. Beispielsweise werden der Name und die Adresse eines Kunden in einer Dimensionstabelle gespeichert und nur aktualisiert, wenn sich das Profil des Kunden ändert. Damit eine Faktentabelle nicht zu groß wird, kann darauf verzichtet werden, den Namen und die Adresse des Kunden in jede Zeile der Faktentabelle aufzunehmen. Stattdessen kann eine Kunden-ID von der Faktentabelle und der Dimensionstabelle gemeinsam genutzt werden. Die beiden Tabellen können mithilfe einer Abfrage verknüpft werden, um das Profil eines Kunden den Transaktionen zuzuordnen. 

- **Integrationstabellen** werden für das Integrieren oder Staging von Daten verwendet. Diese Tabellen können als reguläre Tabellen, externe Tabellen oder temporäre Tabellen erstellt werden. Sie können z. B. Daten in eine Stagingtabelle laden, Transformationen für die Daten in der Stagingumgebung durchführen und die Daten dann in eine Produktionstabelle einfügen.

## <a name="schema-and-table-names"></a>Schema und Tabellennamen
In SQL Data Warehouse ist ein Data Warehouse ist ein Datenbanktyp. Alle Tabellen im Data Warehouse sind in der gleichen Datenbank enthalten.  Das Verknüpfen von Tabellen über mehrere Data Warehouses ist nicht möglich. Dieses Verhalten unterscheidet sich von SQL Server, wo datenbankübergreifende Verknüpfungen unterstützt werden. 

In einer SQL Server-Datenbank können Sie „fact“, „dim“ oder „integrate“ für Schemanamen verwenden. Wenn Sie eine SQL Server-Datenbank in SQL Data Warehouse übertragen, migrieren Sie am besten alle Fakten-, Dimensions- und Integrationstabellen in ein Schema in SQL Data Warehouse und speichern sie dort. Sie können z. B. alle Tabellen im Data Warehouse-Beispiel [WideWorldImportersDW](/sql/sample/world-wide-importers/database-catalog-wwi-olap) in einem Schema namens „WWI“ speichern. Mit dem folgenden Code wird ein [benutzerdefiniertes Schema](/sql/t-sql/statements/create-schema-transact-sql) mit dem Namen „WWI“ erstellt.

```sql
CREATE SCHEMA WWI;
```

Um die Organisation der Tabellen in SQL Data Warehouse anzuzeigen, verwenden Sie „fact“, „dim“ und „int“ als Präfixe für die Tabellennamen. Die folgende Tabelle zeigt einige der Schema- und Tabellennamen für „WideWorldImportersDW“. Die Namen in SQL Server und SQL Data Warehouse werden miteinander verglichen. 

| WWI-Dimensionstabellen  | SQL Server | SQL Data Warehouse |
|:-----|:-----|:------|
| City | Dimension.City | WWI.DimCity |
| Customer | Dimension.Customer | WWI.DimCustomer |
| Date | Dimension.Date | WWI.DimDate |

| WWI-Faktentabellen | SQL Server | SQL Data Warehouse |
|:---|:---|:---|
| Order | Fact.Order | WWI.FactOrder |
| Sale  | Fact.Sale  | WWI.FactSale  |
| Purchase | Fact | WWI.FactPurchase |


## <a name="table-definition"></a>Tabellendefinition 

In den folgenden Themen werden die wichtigsten Aspekte der Tabellendefinition erläutert. 

### <a name="standard-table"></a>Standardtabelle

Eine Standardtabelle wird in Azure Storage als Teil des Data Warehouse gespeichert. Die Tabelle und die Daten werden unabhängig davon beibehalten, ob eine Sitzung geöffnet ist.  In diesem Beispiel wird eine Tabelle mit zwei Spalten erstellt. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Temporäre Tabelle
Eine temporäre Tabelle ist nur für die Dauer der Sitzung vorhanden. Sie können eine temporäre Tabelle verwenden, um zu verhindern, dass andere Benutzer temporäre Ergebnisse sehen, und um die Notwendigkeit von Bereinigungen zu reduzieren.  Da temporäre Tabellen auch lokalen Speicher nutzen, bieten Sie für einige Vorgänge eine schnellere Leistung.  Weitere Informationen finden Sie unter [Temporäre Tabellen](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Externe Tabelle
Eine externe Tabelle verweist auf Daten in Azure Blob Storage oder Azure Data Lake Store. Bei Verwendung in Verbindung mit der CREATE TABLE AS SELECT-Anweisung werden aus einer externen Tabelle ausgewählte Daten in SQL Data Warehouse importiert. Externe Tabellen eignen sich daher zum Laden von Daten. Ein Tutorial zum Ladevorgang finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage](load-data-from-azure-blob-storage-using-polybase.md).

### <a name="data-types"></a>Datentypen
SQL Data Warehouse unterstützt die gängigsten Datentypen. Eine Liste der unterstützten Datentypen finden Sie in der CREATE TABLE-Anweisung unter [Datentypen](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes). Durch Minimieren die Größe von Datentypen wird die Abfrageleistung verbessert. Eine Anleitung zu Datentypen finden Sie unter [Datentypen](sql-data-warehouse-tables-data-types.md).

### <a name="distributed-tables"></a>Verteilte Tabellen
Eine grundlegende Funktion von SQL Data Warehouse ist die Möglichkeit, Tabellen über 60 verteilte Standorte, als Verteilungen bezeichnet, im verteilten System zu speichern.  SQL Data Warehouse kann eine Tabelle auf eine der folgenden drei Arten speichern:

- **Roundrobin**: Hierbei werden die Tabellenzeilen zufällig, aber gleichmäßig auf die Verteilungen verteilt. Mit der Roundrobintabelle wird eine hohe Ladeleistung erzielt, sie erfordert jedoch mehr Datenverschiebungen als die anderen Methoden für Abfragen, die Spalten verknüpfen. 
- **Hash**: Hierbei werden die Zeilen auf Basis des Werts in der Verteilungsspalte verteilt. Die Tabelle mit Hashverteilung hat das größte Potenzial für eine hohe Leistung bei Abfrageverknüpfungen für große Tabellen. Es gibt mehrere Faktoren, die die Auswahl der Verteilungsspalte beeinflussen. Weitere Informationen finden Sie unter [Verteilte Tabellen](sql-data-warehouse-tables-distribute.md).
- **Repliziert**: Mit replizierten Tabellen wird eine vollständige Kopie der Tabelle auf jedem Serverknoten verfügbar gemacht. Abfragen werden für replizierte Tabellen schnell ausgeführt, da bei Verknüpfungen replizierter Tabellen kein Verschieben von Daten erforderlich ist. Für die Replikation wird zusätzlicher Speicherplatz benötigt, und sie ist für große Tabellen nicht geeignet. Weitere Informationen finden Sie unter [Entwurfsleitfaden für replizierte Tabellen](design-guidance-for-replicated-tables.md).

Die Tabellenkategorie bestimmt oftmals, welche Option für das Verteilen der Tabelle ausgewählt wird.  

| Tabellenkategorie | Empfohlene Verteilungsoption |
|:---------------|:--------------------|
| Fakt           | Verwenden Sie die Hashverteilung mit gruppiertem Columnstore-Index. Die Leistung wird verbessert, wenn zwei Hashtabellen über die gleiche Verteilungsspalte verknüpft sind. |
| Dimension      | Verwenden Sie bei kleineren Tabellen die Replikation. Wenn Tabellen zu groß sind, um sie auf jedem Serverknoten zu speichern, verwenden Sie die Hashverteilung. |
| Staging        | Verwenden Sie für die Stagingtabelle das Roundrobinprinzip. Das Laden lässt sich mit CTAS beschleunigen. Sobald sich die Daten in der Stagingtabelle befinden, verwenden Sie INSERT...SELECT, um die Daten in eine Produktionstabelle zu verschieben. |

### <a name="table-partitions"></a>Tabellenpartitionen
In einer partitionierten Tabelle werden die Tabellenzeilen nach Datenbereichen gespeichert und Vorgänge entsprechend ausgeführt. Beispielsweise könnte eine Tabelle nach Tag, Monat oder Jahr partitioniert werden. Sie können die Abfrageleistung durch eine Partitionsbeseitigung verbessern, wobei ein Abfragescan auf Daten in einer Partition begrenzt wird. Auch können Sie die Daten durch Partitionswechsel verwalten. Da die Daten in SQL Data Warehouse bereits verteilt sind, können zu viele Partitionen die Abfrageleistung beeinträchtigen. Weitere Informationen finden Sie unter [Partitionieren von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-partition.md).

### <a name="columnstore-indexes"></a>ColumnStore-Indizes
Standardmäßig speichert SQL Data Warehouse eine Tabelle als gruppierten Columnstore-Index. Mit dieser Form der Datenspeicherung wird eine hohe Datenkomprimierung und Abfrageleistung für große Tabellen erreicht.  Der gruppierte Columnstore-Index ist in der Regel die beste Wahl, aber in einigen Fällen ist ein gruppierter Index oder ein Heap die geeignete Speicherstruktur.

Eine Liste der Columnstore-Funktionen finden Sie unter [Columnstore-Indizes – Neuigkeiten](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Informationen zum Verbessern der Leistung von Columnstore-Indizes finden Sie unter [Maximieren der Zeilengruppenqualität für Columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

### <a name="statistics"></a>Statistiken
Der Abfrageoptimierer verwendet beim Erstellen des Plans für die Ausführung einer Abfrage Statistiken auf Spaltenebene. Um die Abfrageleistung zu verbessern, ist es wichtig, Statistiken für einzelne Spalten zu erstellen, insbesondere für in Abfrageverknüpfungen verwendete Spalten. Die Erstellung und die Aktualisierung von Statistiken werden nicht automatisch ausgeführt. [Erstellen Sie Statistiken](/sql/t-sql/statements/create-statistics-transact-sql), nachdem eine Tabelle erstellt wurde. Führen Sie die Statistikaktualisierung durch, wenn eine erhebliche Anzahl von Zeilen hinzugefügt oder geändert wurde. Aktualisieren Sie Statistiken z. B. nach einem Ladevorgang. Weitere Informationen finden Sie unter [Verwalten von Statistiken für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-statistics.md).

## <a name="ways-to-create-tables"></a>Methoden zum Erstellen von Tabellen
Sie können eine Tabelle als neue leere Tabelle erstellen. Alternativ können Sie eine Tabelle erstellen und mit den Ergebnissen einer SELECT-Anweisung füllen. Es folgen die T-SQL-Befehle zum Erstellen einer Tabelle.

| T-SQL-Anweisung | BESCHREIBUNG |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Erstellt eine leere Tabelle durch die Definition der Tabellenspalten und Optionen. |
| [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql) | Erstellt eine externe Tabelle. Die Definition der Tabelle wird in SQL Data Warehouse gespeichert. Die Tabellendaten werden in Azure Blob Storage oder Azure Data Lake Store abgelegt. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Füllt eine neue Tabelle mit den Ergebnissen einer SELECT-Anweisung. Die Spalten und Datentypen der Tabelle basieren auf den Ergebnissen der SELECT-Anweisung. Mit dieser Anweisung können Daten zum Importieren aus einer externen Tabelle ausgewählt werden. |
| [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Erstellt eine neue externe Tabelle durch den Export der Ergebnisse einer SELECT-Anweisung an einen externen Speicherort.  Dieser Speicherort ist entweder Azure Blob Storage oder Azure Data Lake Store. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Ausrichten von Quelldaten am Data Warehouse

Data Warehouse-Tabellen werden durch das Laden von Daten aus einer anderen Datenquelle gefüllt. Um einen Ladevorgang erfolgreich ausführen zu können, müssen die Anzahl und die Datentypen der Spalten in den Quelldaten an der Tabellendefinition im Data Warehouse ausgerichtet werden. Diese Ausrichtung der Daten zu bewältigen, ist möglicherweise der schwierigste Aspekt beim Entwerfen von Tabellen. 

Wenn Daten aus mehreren Datenspeichern stammen, können Sie die Daten in das Data Warehouse überführen und in einer Integrationstabelle speichern. Sobald sich die Daten in der Integrationstabelle befinden, können Sie die Leistungsfähigkeit von SQL Data Warehouse nutzen, um Transformationsvorgänge auszuführen.

## <a name="unsupported-table-features"></a>Nicht unterstützte Tabellenfunktionen
SQL Data Warehouse unterstützt viele, aber nicht alle Tabellenfunktionen anderer Datenbanken.  Die folgende Liste enthält einige der Tabellenfunktionen, die in SQL Data Warehouse nicht unterstützt werden.

- Primary Key, Foreign Key, Unique, Check – [Tabelleneinschränkungen](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Berechnete Spalten](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Indizierte Sichten](/sql/relational-databases/views/create-indexed-views)
- [Sequenz](/sql/t-sql/statements/create-sequence-transact-sql)
- [Spalten mit geringer Dichte](/sql/relational-databases/tables/use-sparse-columns)
- [Ersatzschlüssel](). Implementierung mit [Identity](sql-data-warehouse-tables-identity.md).
- [Synonyme](/sql/t-sql/statements/create-synonym-transact-sql)
- [Trigger](/sql/t-sql/statements/create-trigger-transact-sql)
- [Eindeutige Indizes](/sql/t-sql/statements/create-index-transact-sql)
- [Benutzerdefinierte Typen](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Abfragen für die Tabellengröße
Eine einfache Möglichkeit, den Speicherplatz- und Zeilenverbrauch einer Tabelle in jeder der 60 Verteilungen zu ermitteln, ist die Verwendung von [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Die Verwendung von DBCC-Befehlen kann aber auch mit größeren Einschränkungen verbunden sein.  Dynamische Verwaltungssichten (DMVs) zeigen mehr Details als DBCC-Befehle. Erstellen Sie zunächst diese Sicht.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tabellenspeicherplatz – Zusammenfassung

Diese Abfrage gibt die Zeilen und den Speicherplatz nach Tabelle zurück.  Sie eignet sich dazu anzuzeigen, welche Tabellen am größten sind und ob es sich um Roundrobintabellen, replizierte Tabellen oder Tabellen mit Hashverteilung handelt.  Für Tabellen mit Hashverteilung wird zudem die Verteilungsspalte angezeigt.  In den meisten Fällen sollte es sich bei der größten Tabelle um eine Tabelle mit Hashverteilung mit einem gruppierten Columnstore-Index handeln.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabellenspeicherplatz nach Verteilungstyp

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabellenspeicherplatz nach Indextyp

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Verteilungsspeicherplatz – Zusammenfassung

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen der Tabellen für das Data Warehouse werden im nächsten Schritt Daten in die Tabelle geladen.  Ein Tutorial zum Ladevorgang finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).
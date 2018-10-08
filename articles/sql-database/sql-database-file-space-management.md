---
title: Dateispeicherplatzverwaltung mit Azure SQL-Datenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt, wie Sie mit Azure SQL-Datenbank Dateispeicherplatz verwalten, und bietet Codebeispiele, mit denen Sie feststellen können, ob eine Datenbank verkleinert werden muss. Außerdem erhalten Sie hierin die entsprechenden Anweisungen zum Verkleinern der Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: a46192c79d32ddf5f178541c3be128893e8f6109
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159940"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Verwalten von Dateispeicherplatz in Azure SQL-Datenbank
Dieser Artikel beschreibt verschiedene Arten von Speicherplatz in der Azure SQL-Datenbank und Schritte, die ausgeführt werden können, wenn der für Datenbanken und Pools für elastische Datenbanken zugewiesene Speicherplatz explizit verwaltet werden muss.

## <a name="overview"></a>Übersicht

In Azure SQL-Datenbank messen die meisten im Azure-Portal angezeigten Speicherplatzmetriken und die folgenden APIs die Anzahl von verwendeten Datenseiten für Datenbanken und Pool für elastische Datenbanken:
- Azure Resource Manager-basierte Metrik-APIs einschließlich PowerShell [get-metrics](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL:  [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Es gibt Workloadmuster, bei denen die Zuteilung von zugrunde liegenden Datendateien für Datenbanken größer als die Menge der verwendeten Datenseiten werden kann.  Dies kann auftreten, wenn der Platzbedarf zunimmt und die Daten anschließend gelöscht werden.  Der Grund ist, dass der zugeordnete Dateispeicherplatz nicht automatisch wieder freigegeben wird, wenn Daten gelöscht werden.  In diesen Szenarien kann der zugeordnete Speicherplatz für eine Datenbank oder einen Pool die unterstützten Grenzwerte übersteigen und das Datenwachstum sowie Änderungen des Diensttarifs und der Computegröße verhindern. Außerdem kann die Verkleinerung von Datendateien erzwungen werden, um dieses Problem zu lösen.

Der SQL-Datenbankdienst verkleinert Datendateien aufgrund der möglichen Auswirkungen auf die Datenbankleistung nicht automatisch, um ungenutzten zugewiesenen Speicherplatz freizugeben.  Kunden können Datendateien aber jederzeit selbst verkleinern, indem sie die Schritte unter [Freigeben von ungenutztem zugewiesenem Speicherplatz](#reclaim-unused-allocated-space) ausführen. 

> [!NOTE]
> Im Gegensatz zu Datendateien verkleinert der SQL-Datenbankdienst die Protokolldateien automatisch, da dieser Vorgang die Datenbankleistung nicht beeinträchtigt. 

## <a name="understanding-types-of-storage-space-for-a-database"></a>Grundlegendes zu den Arten von Speicherplatz für eine Datenbank

Es wichtig, dass Sie mit den folgenden Speicherplatzmengen vertraut sind, damit Sie den Dateispeicherplatz einer Datenbank verwalten können.

|Datenbankmenge|Definition|Kommentare|
|---|---|---|
|**Genutzter Speicherplatz**|Die Menge des Speicherplatzes, der zum Speichern von Daten in 8-KB-Seiten verwendet wird.|In der Regel erhöht (verringert) sich der Platzbedarf bei eingefügten (gelöschten) Daten. In manchen Fällen ändert sich der genutzte Speicherplatz beim Einfügen oder Löschen von Daten nicht, je nach Menge und Muster der an dem Vorgang beteiligten Daten und einer eventuellen Fragmentierung. Beispielsweise wird der genutzte Speicherplatz durch Löschen einer Zeile auf jeder Datenseite nicht zwangsläufig gesenkt.|
|**Zugeordneter Datenspeicherplatz**|Die Menge an formatiertem Dateispeicherplatz zum Speichern von Daten.|Die Menge des zugeordneten Speicherplatzes wächst automatisch an, wird aber nach dem Löschen nicht kleiner. Dieses Verhalten stellt sicher, dass Daten später schneller eingefügt werden, da der Platz nicht neu formatiert werden muss.|
|**Zugeordneter Datenspeicherplatz (ungenutzt)**|Die Differenz zwischen der Menge des zugeordneten Datenspeicherplatzes und des genutzten Datenspeicherplatzes.|Diese Menge ist die maximale Menge des freien Speicherplatzes, die freigegeben werden kann, indem Datendateien von Datenbanken verkleinert werden.|
|**Maximale Datengröße**|Die maximale Speicherplatzmenge, die zum Speichern von Datenbankdaten verwendet werden kann.|Die Menge des zugeordneten Datenspeicherplatzes kann die maximale Datengröße nicht überschreiten.|
||||

Das folgende Diagramm veranschaulicht die Beziehung zwischen den verschiedenen Arten von Speicherplatz für eine Datenbank.

![Arten von Speicherplatz und Beziehungen](./media/sql-database-file-space-management/storage-types.png) 

## <a name="query-a-database-for-storage-space-information"></a>Abfragen einer Datenbank nach Speicherplatzinformationen

Die folgenden Abfragen können verwendet werden, um die Speicherplatzmengen für eine Datenbank zu ermitteln.  

### <a name="database-data-space-used"></a>Genutzter Speicherplatz in Datenbank
Ändern Sie die folgende Abfrage, um den belegten Speicherplatz der Datenbank zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Zugeordneter Datenbank-Datenspeicherplatz und ungenutzter zugeordneter Speicherplatz
Verwenden Sie die folgende Abfrage, um die Menge des Datenbank-Datenspeicherplatzes und die Menge des zugeordneten ungenutzten Speicherplatzes zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-data-max-size"></a>Max. Größe für Datenbankdaten
Ändern Sie die folgende Abfrage, um die maximale Größe für Datenbankdaten zurückgegeben.  Als Einheit für das Abfrageergebnis wird Byte verwendet.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Grundlegendes zu den Arten von Speicherplatz für einen Pool für elastische Datenbanken

Es wichtig, dass Sie mit den folgenden Speicherplatzmengen vertraut sind, damit Sie den Dateispeicherplatz eines Pools für elastische Datenbanken verwalten können.

|Menge des Pools für elastische Datenbanken|Definition|Kommentare|
|---|---|---|
|**Genutzter Speicherplatz**|Die Summierung des Datenspeicherplatzes, der von allen Datenbanken im Pool für elastische Datenbanken verwendet wird.||
|**Zugeordneter Datenspeicherplatz**|Die Summierung des Datenspeicherplatzes, der von allen Datenbanken im Pool für elastische Datenbanken zugeordnet wird.||
|**Zugeordneter Datenspeicherplatz (ungenutzt)**|Die Differenz zwischen der Menge an zugeordnetem Datenspeicherplatz und dem Datenspeicherplatz, der von allen Datenbanken im Pool für elastische Datenbanken verwendet wird.|Diese Menge gibt die maximale Menge von Speicherplatz an, der für den Pool für elastische Datenbanken zugeordnet wird und freigegeben werden kann, indem die Datenbank-Datendateien verkleinert werden.|
|**Maximale Datengröße**|Die maximale Menge an Datenspeicherplatz, der vom Pool für elastische Datenbanken für alle seine Datenbanken verwendet werden kann.|Der zugeordnete Speicherplatz des Pools für elastische Datenbanken darf die maximale Größe des Pools nicht überschreiten.  Wenn dies der Fall ist, kann zugeordneter Speicherplatz, der nicht genutzt wird, freigegeben werden, indem Datenbank-Datendateien verkleinert werden.|
||||

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Abfragen von Speicherplatzinformationen für einen Pools für elastische Datenbanken

Die folgenden Abfragen können verwendet werden, um die Speicherplatzmengen für einen Pool für elastische Datenbanken zu ermitteln.  

### <a name="elastic-pool-data-space-used"></a>Pool für elastische Datenbanken – ungenutzter Speicherplatz
Ändern Sie die folgende Abfrage, um den belegten Speicherplatz eines Pools für elastische Datenbanken zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Zugeordneter Datenspeicherplatz für Pool für elastische Datenbanken und ungenutzter zugeordneter Speicherplatz

Ändern Sie das folgende PowerShell-Skript, um eine Tabelle zurückzugeben, in der der zugeordnete Speicherplatz und der ungenutzte zugeordnete Speicherplatz für jede Datenbank eines Pools für elastische Datenbanken aufgeführt ist. Die Tabelle ordnet Datenbanken absteigend von der größten Menge an ungenutztem zugeordnetem Speicherplatz zur geringsten Menge an ungenutztem zugeordnetem Speicherplatz.  Als Einheit für das Abfrageergebnis wird MB verwendet.  

Die Abfrageergebnisse zum Bestimmen des für jede Datenbank im Pool zugeordneten Speicherplatzes können addiert werden, um den gesamten Speicherplatz zu ermitteln, der für den Pool für elastische Datenbanken zugeordnet ist. Der zugewiesene Speicherplatz des Pools für elastische Datenbanken darf die maximale Größe des Pools nicht überschreiten.  

Für die Installation des PowerShell-Skripts ist das SQL Server PowerShell-Modul erforderlich – Siehe [Herunterladen des PowerShell-Moduls](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module?view=sql-server-2017).

```powershell
# Resource group name
$resourceGroupName = "rg1" 
# Server name
$serverName = "ls2" 
# Elastic pool name
$poolName = "ep1"
# User name for server
$userName = "name"
# Password for server
$password = "password"

# Get list of databases in elastic pool
$databasesInPool = Get-AzureRmSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
  
foreach ($database in $databasesInPool)
{
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn `
        -Database $database.DatabaseName `
        -Username $userName `
        -Password $password `
        -Query $sqlCommand)
}
# Display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort `
    -Property DatabaseDataSpaceAllocatedUnusedInMB `
    -Descending | Format-Table
```

Der folgende Screenshot zeigt ein Beispiel für die Ausgabe des Skripts:

![Pool für elastische Datenbanken – zugeordneter Speicherplatz und ungenutzter zugewiesener Speicherplatz – Beispiel](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Maximale Größe der Daten des Pools für elastische Datenbanken

Ändern Sie die folgende T-SQL-Abfrage, um die maximale Größe der Daten des Pools für elastische Datenbanken zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Freigeben von ungenutztem zugewiesenem Speicherplatz

Nachdem die Datenbanken für das Freigeben von ungenutztem zugeordnetem Speicherplatz ermittelt wurden, können Sie den folgenden Befehl ändern, um die Datendateien für jede Datenbank zu verkleinern.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Weitere Informationen zu diesem Befehl finden Sie unter [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 

> [!IMPORTANT] 
> Ziehen Sie auch in Betracht, Datenbankindizes neu zu erstellen. Nach dem Verkleinern von Datenbankdateien können Indizes fragmentiert werden und bei der Leistungsoptimierung nicht mehr effektiv eingesetzt werden. In diesem Fall sollten die Indizes neu erstellt werden. Weitere Informationen zum Fragmentieren und Neuerstellen von Indizes finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur maximalen Datenbankgröße finden Sie unter:
  - [Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Ressourcenlimits für Einzeldatenbanken, die das Kaufmodell auf DTU-Basis verwenden](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Limits des V-Kern-basierten Kaufmodells für Pools für elastische Datenbanken in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Grenzwerte für Ressourcen für elastische Pools, die das Kaufmodell auf DTU-Basis verwenden](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Weitere Informationen zum `SHRINKDATABASE`-Befehl finden Sie unter [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Weitere Informationen zum Fragmentieren und Neuerstellen von Indizes finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

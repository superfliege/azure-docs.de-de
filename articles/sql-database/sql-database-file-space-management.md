---
title: Dateispeicherplatzverwaltung mit Azure SQL-Datenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt, wie Sie mit Azure SQL-Datenbank Dateispeicherplatz verwalten, und bietet Codebeispiele, mit denen Sie feststellen können, ob eine Datenbank verkleinert werden muss. Außerdem erhalten Sie hierin die entsprechenden Anweisungen zum Verkleinern der Datenbank.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 1ecc0ce08ef42f5f5935bca29e8269be2ea142f0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39416003"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Verwalten von Dateispeicherplatz in Azure SQL-Datenbank

Dieser Artikel beschreibt verschiedene Arten von Speicherplatz in der Azure SQL-Datenbank und Schritte, die durchgeführt werden können, wenn der für Datenbanken und Pools für elastische Datenbanken zugewiesene Speicherplatz vom Kunden verwaltet werden muss.

## <a name="overview"></a>Übersicht

In Azure SQL Database messen die im Azure-Portal angezeigten Speichergrößenmetriken und die folgenden APIs die Anzahl der verwendeten Datenseiten für Datenbanken und Pool für elastische Datenbanken:
- Azure Resource Manager-basierte Metrik-APIs einschließlich PowerShell [get-metrics](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL:  [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Es gibt Workloadmuster, bei denen die Zuweisung von Speicherplatz in den zugrunde liegenden Datendateien für Datenbanken größer wird als die Anzahl der verwendeten Datenseiten in den Datendateien. Dieses Szenario kann auftreten, wenn der Platzbedarf zunimmt und die Daten anschließend gelöscht werden. Wenn die Daten gelöscht werden, wird der zugewiesene Speicherplatz nicht automatisch freigegeben, wenn die Daten gelöscht werden. In solchen Szenarien kann der zugewiesene Speicherplatz für eine Datenbank oder einen Pool die für die Datenbank festgelegten (oder unterstützten) Höchstgrenzen überschreiten und somit ein Datenwachstum oder eine Änderung der Leistungsstufe verhindern, auch wenn der tatsächlich genutzte Speicherplatz unter der Höchstgrenze liegt. Um dies zu verhindern, müssen Sie die Datenbank möglicherweise verkleinern, um den zugewiesenen, aber ungenutzten Speicherplatz in der Datenbank zu reduzieren.

Der SQL-Datenbankdienst verkleinert Datenbankdateien aufgrund der möglichen Auswirkungen auf die Datenbankleistung nicht automatisch, um ungenutzten zugewiesenen Speicherplatz freizugeben. Sie können die Datei in einer Datenbank jedoch jederzeit anhand der unter [Freigeben von ungenutztem zugewiesenem Speicherplatz](#reclaim-unused-allocated-space) aufgeführten Schritte verkleinern. 

> [!NOTE]
> Im Gegensatz zu Datendateien verkleinert der SQL-Datenbankdienst die Protokolldateien automatisch, da dieser Vorgang die Datenbankleistung nicht beeinträchtigt.

## <a name="understanding-the-types-of-storage-space-for-a-database"></a>Grundlegendes zu den Arten von Speicherplatz für eine Datenbank

Um Dateispeicher zu verwalten, müssen Sie die folgenden Begriffe verstehen, die sich auf die Datenbankspeicherung sowohl für eine einzelne Datenbank als auch für einen Pool für elastische Datenbanken beziehen.

|Speicherplatzbegriff|Definition|Kommentare|
|---|---|---|
|**Genutzter Speicherplatz**|Die Menge des Speicherplatzes, der zum Speichern von Daten in 8-KB-Seiten verwendet wird.|In der Regel erhöht (verringert) sich dieser Platzbedarf bei eingefügten (gelöschten) Daten. In manchen Fällen ändert sich der genutzte Speicherplatz beim Einfügen oder Löschen von Daten nicht, je nach Menge und Muster der an dem Vorgang beteiligten Daten und einer eventuellen Fragmentierung. Beispielsweise wird der genutzte Speicherplatz durch Löschen einer Zeile auf jeder Datenseite nicht zwangsläufig gesenkt.|
|**Zugewiesener Speicherplatz**|Die Menge an formatiertem Dateispeicherplatz zum Speichern von Daten|Der zugewiesene Speicherplatz wächst automatisch an, wird jedoch nach dem Löschen nicht kleiner. Dieses Verhalten stellt sicher, dass Daten später schneller eingefügt werden, da der Platz nicht neu formatiert werden muss.|
|**Zugewiesener aber ungenutzter Speicherplatz**|Die Menge an ungenutztem Speicherplatz für die Datendatei, der für die Datenbank zugewiesen ist.|Diese Menge ist die Differenz zwischen dem zugewiesenen und dem belegten Speicherplatz und stellt die maximale Menge an Speicherplatz dar, die durch das Verkleinern von Datenbankdateien freigegeben werden kann.|
|**Max. Größe**|Die Höchstmenge an Datenspeicherplatz, die von der Datenbank verwendet werden kann.|Der zugewiesene Datenspeicherplatz kann die maximale Datengröße nicht überschreiten.|
||||

Das folgende Diagramm veranschaulicht die Beziehung zwischen den verschiedenen Arten von Speicherplatz.

![Arten von Speicherplatz und Beziehungen](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-database-for-storage-space-information"></a>Abfragen einer Datenbank nach Speicherplatzinformationen

Um festzustellen, ob eine einzelne Datenbank zugewiesenen aber ungenutzten Datenspeicherplatz enthält, den Sie ggf. freigeben können, verwenden Sie die folgenden Abfragen:

### <a name="database-data-space-used"></a>Genutzter Speicherplatz in Datenbank
Ändern Sie die folgende Abfrage, um den belegten Speicherplatz der Datenbank in MB zurückzugeben.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-allocated-and-allocated-space-unused"></a>Datenbankdaten – zugewiesener und ungenutzter zugewiesener Speicherplatz
Ändern Sie die folgende Abfrage, um den zugewiesenen und ungenutzten zugewiesenen Speicherplatz der Datenbank zurückzugeben.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-max-size"></a>Maximale Datenbankgröße
Ändern Sie die folgende Abfrage, um die maximale Datenbankgröße in Bytes zurückgegeben.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Abfragen von Speicherplatzinformationen für einen Pools für elastische Datenbanken

Um festzustellen, ob ein Pool für elastische Datenbanken zugewiesenen aber ungenutzten Datenspeicherplatz enthält, den Sie ggf. freigeben können, verwenden Sie die folgenden Abfragen:

### <a name="elastic-pool-data-space-used"></a>Pool für elastische Datenbanken – ungenutzter Speicherplatz
Ändern Sie die folgende Abfrage, um den belegten Speicherplatz eines Pools für elastische Datenbanken in MB zurückzugeben.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-allocated-and-allocated-space-unused"></a>Pool für elastische Datenbanken – zugewiesener und ungenutzter zugewiesener Speicherplatz

Ändern Sie das folgende PowerShell-Skript, um eine Tabelle zurückzugeben, in der der gesamte zugewiesene Speicherplatz und der ungenutzte zugewiesene Speicherplatz für jede Datenbank und jeden Pool für elastische Datenbanken aufgeführt ist. Die Tabelle ordnet Datenbanken absteigend von der größten Menge an ungenutztem zugewiesenem Speicherplatz zur geringsten Menge an ungenutztem zugewiesenem Speicherplatz.  

Die Abfrageergebnisse zum Bestimmen des für jede Datenbank im Pool zugewiesenen Speicherplatzes können addiert werden, um den für den Pool für elastische Datenbanken zugewiesenen Speicherplatz zu ermitteln. Der zugewiesene Speicherplatz des Pools für elastische Datenbanken darf die maximale Größe des Pools nicht überschreiten.  

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
# Requires SQL Server PowerShell module – see here to install.  
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

### <a name="elastic-pool-max-size"></a>Pool für elastische Datenbanken – maximale Größe

Verwenden Sie die folgende T-SQL-Abfrage, um die maximale Größe des Pools in MB zurückgegeben.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Freigeben von ungenutztem zugewiesenem Speicherplatz

Wenn Sie festgestellt haben, dass ungenutzter zugewiesener Speicherplatz vorhanden ist, den Sie freigeben können, verwenden Sie den folgenden Befehl, um den zugewiesenen Datenbankspeicherplatz zu verkleinern. 

> [!IMPORTANT]
> Für Datenbanken in einem Pool sollten Datenbanken mit dem größten ungenutzten Speicherplatz zuerst verkleinert werden, um den Speicherplatz möglichst schnell freizugeben.  

Verwenden Sie den folgenden Befehl zum Verkleinern aller Datendateien in der angegebenen Datenbank:

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'<database_name>')
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
---
title: Abfragen von Azure SQL-Sharddatenbanken | Microsoft-Dokumentation
description: "Führen Sie mithilfe der Clientbibliothek für elastische Datenbanken Abfragen mehrerer Shards durch."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: torsteng
ms.openlocfilehash: 33128357bd5b2bd744c5c1c3032f658ebe865d49
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="multi-shard-querying"></a>Abfragen von mehreren Shards
## <a name="overview"></a>Übersicht
Mit den [Tools für elastische Datenbanken](sql-database-elastic-scale-introduction.md)können Sie die horizontal partitionierten Datenbanklösungen erstellen. **Abfragen von mehreren Shards** wird für Aufgaben wie Datensammlung/Berichterstellung verwendet, für die das Ausführen einer Abfrage über mehrere Shards erforderlich ist. (Vergleichen Sie dies mit dem [datenabhängigen Routing](sql-database-elastic-scale-data-dependent-routing.md), bei dem die gesamte Arbeit in einem einzigen Shard durchgeführt wird.) 

1. Rufen Sie **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) oder **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807370.aspx)) mithilfe der Methode **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), **TryGetListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)) oder **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) ab. Weitere Informationen finden Sie unter **[Erstellen eines ShardMapManager](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager)** und **[Abrufen einer RangeShardMap oder ListShardMap](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)**.
2. Erstellen Sie ein **MultiShardConnection**-Objekt ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_connection), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)).
3. Erstellen Sie ein **MultiShardStatement- oder MultiShardCommand**-Objekt ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)). 
4. Legen Sie die **CommandText**-Eigenschaft ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)) auf einen T-SQL-Befehl fest.
5. Führen Sie den Befehl durch Aufrufen der Methode **ExecuteQueryAsync oder ExecuteReader** ([Java](), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)) auf.
6. Zeigen Sie die Ergebnisse mithilfe der Klasse **MultiShardResultSet oder MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_result_set), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)) an. 

## <a name="example"></a>Beispiel
Das folgende Codebeispiel veranschaulicht die Verwendung von Abfragen mehrerer Shards mithilfe eines bestimmten **ShardMap** -Elements mit dem Namen *myShardMap*. 

```csharp
using (MultiShardConnection conn = new MultiShardConnection(myShardMap.GetShards(), myShardConnectionString)) 
{ 
    using (MultiShardCommand cmd = conn.CreateCommand())
    { 
        cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
        cmd.CommandType = CommandType.Text; 
        cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
        cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

        using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
        { 
            while (sdr.Read())
            { 
                var c1Field = sdr.GetString(0); 
                var c2Field = sdr.GetFieldValue<int>(1); 
                var c3Field = sdr.GetFieldValue<Int64>(2);
            } 
        } 
    } 
} 
```

Ein wichtiger Unterschied ist die Erstellung von Verbindungen mit mehreren Shards. Wenn **SqlConnection** in einer Einzeldatenbank ausgeführt wird, verwendet **MultiShardConnection** eine ***Sammlung von Shards*** als Eingabe. Füllen Sie die Sammlung von Shards aus einer Shard-Map auf. Die Abfrage wird dann mithilfe der **UNION ALL** -Semantik für die Sammlung von Shards ausgeführt, um ein einziges Gesamtergebnis zu erhalten. Optional kann der Name des Shards, aus dem die Zeile stammt, mithilfe der **ExecutionOptions** -Eigenschaft im Befehl der Ausgabe hinzugefügt werden. 

Beachten Sie, dass **myShardMap.GetShards()**aufgerufen wird. Diese Methode ruft alle Shards aus der Shard-Zuordnung ab und stellt damit eine einfache Möglichkeit zum Ausführen einer Abfrage an alle Datenbanken bereit. Die Sammlung von Shards für eine Abfrage mehrerer Shards kann durch Ausführen einer LINQ-Abfrage über die Sammlung, die von dem Aufruf von **myShardMap.GetShards()**zurückgegeben wird, weiter optimiert werden. In Kombination mit der Teilergebnisrichtlinie wurde die aktuelle Funktion bei der Abfrage mehrerer Shards so entwickelt, dass diese gut mit bis zu Hunderten von Shards funktioniert.

Eine Einschränkung von Abfragen mehrerer Shards ist derzeit die fehlende Überprüfung von Shards und Shardlets, die abgefragt werden. Während beim datenabhängigem Routing überprüft wird, dass ob ein bestimmtes Shard Teil der Shard-Map zum Zeitpunkt der Abfrage ist, führen Abfragen mehrerer Shards dieses Überprüfung nicht durch. Dies kann dazu führen, dass Abfragen mehrerer Shards für Datenbanken ausgeführt werden, die aus der Shardzuordnung entfernt wurden.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Abfragen mehrerer Shards und Aufteilungs-/Zusammenführungsvorgänge
Beim Abfragen mehrerer Shards wird nicht überprüft, ob Shardlets im abgefragten Shard an laufenden Aufteilungs-/Zusammenführungsvorgängen teilnehmen. (Siehe [Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md).) Dies kann zu Inkonsistenzen führen, wenn Zeilen aus demselben Shardlet für mehrere Datenbanken in derselben Abfrage mehrerer Shards angezeigt werden. Beachten Sie diese Einschränkungen, und ziehen Sie beim Ausführen von Abfragen mehrerer Shards einen Ausgleich laufender Aufteilungs-/Zusammenführungsvorgänge und Änderungen an der Shard-Zuordnung in Betracht.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



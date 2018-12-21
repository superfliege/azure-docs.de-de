---
title: Abfragen von Containern in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Container in Azure Cosmos DB abfragen.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 11c68b61802f6c7b3755da71c176ea777f171e4c
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409835"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Abfragen von Containern in Azure Cosmos DB

In diesem Artikel erfahren Sie, wie Sie einen Container (Sammlung, Diagramm, Tabelle) in Azure Cosmos DB abfragen.

## <a name="in-partition-query"></a>Abfrage innerhalb einer Partition

Bei Abfragen von Daten in Containern leitet Azure Cosmos DB die Abfrage automatisch an die Partitionen weiter, die den im Filter angegebenen Partitionsschlüsselwerten entsprechen, wenn für eine Abfrage Partitionsschlüsselfilter angegeben sind. Die folgende Abfrage wird beispielsweise an die DeviceId-Partition weitergeleitet, die alle Dokumente enthält, die dem Partitionsschlüsselwert „XMS-0001“ entsprechen.

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Partitionsübergreifende Abfrage

Die folgende Abfrage verfügt nicht über einen Filter für den Partitionsschlüssel (DeviceId) und wird an alle Partitionen verteilt, wo sie auf dem Partitionsindex ausgeführt wird. Wenn Sie eine Abfrage über mehrere Partitionen hinweg ausführen möchten, legen Sie **EnableCrossPartitionQuery** auf „true“ fest (oder „x-ms-documentdb-query-enablecrosspartition“ in der REST-API).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB unterstützt die Aggregatfunktionen „COUNT“, „MIN“, „MAX“ und „AVG“ für Container mithilfe von SQL. Die Aggregatfunktionen für Container sind ab der SDK-Version 1.12.0 verfügbar. Abfragen müssen einen einzelnen Aggregat-Operator und einen einzelnen Wert in der Projektion enthalten.

## <a name="parallel-cross-partition-query"></a>Parallele partitionsübergreifende Abfrage

Cosmos DB SDKs ab Version 1.9.0 unterstützen Optionen für die Ausführung paralleler Abfragen.  Parallele partitionsübergreifende Abfragen ermöglichen partitionsübergreifende Abfragen mit geringer Wartezeit. Die folgende Abfrage ist z.B. so konfiguriert, dass sie partitionsübergreifend parallel ausgeführt wird.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Sie können die parallele Ausführung von Abfragen verwalten, indem Sie die folgenden Parameter optimieren:

- **MaxDegreeOfParallelism**: Legt die maximale Anzahl gleichzeitiger Verbindungen mit den Partitionen des Containers fest. Wenn Sie diese Eigenschaft auf „-1“ festlegen, wird der Grad der Parallelität vom SDK verwaltet. Wird „MaxDegreeOfParallelism“ nicht angegeben oder auf „0“ (Standardwert) festgelegt, besteht eine einzelne Netzwerkverbindung mit den Partitionen des Containers.

- **MaxBufferedItemCount**: Steuert das Verhältnis zwischen Abfragewartezeit und clientseitiger Arbeitsspeichernutzung. Wird diese Option weggelassen oder auf „-1“ festgelegt, wird die Anzahl von Elementen, die während der Ausführung paralleler Abfragen gepuffert werden, vom SDK verwaltet.

Bei gleichem Zustand der Sammlung gibt eine parallele Abfrage Ergebnisse in der gleichen Reihenfolge wie bei einer seriellen Ausführung zurück. Bei einer partitionsübergreifenden Abfrage mit Sortieroperatoren („ORDER BY“ und/oder „TOP“) gibt das Azure Cosmos DB SDK die Abfrage parallel an verschiedene Partitionen aus und führt teilweise sortierte Ergebnisse auf der Clientseite zusammen, um global sortierte Ergebnisse zu generieren.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Partitionierung in Cosmos DB finden Sie in den folgenden Artikeln:

- [Partitioning in Azure Cosmos DB](partitioning-overview.md) (Partitionierung in Azure Cosmos DB)
- [Create a synthetic partition key](synthetic-partition-keys.md) (Erstellen eines synthetischen Partitionsschlüssels)

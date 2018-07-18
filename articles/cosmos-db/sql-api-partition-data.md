---
title: Partitionierung und Skalierung in Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e3c15ce201cee88d400b9d5fb9272b584293a2f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797431"
---
# <a name="partitioning-in-azure-cosmos-db-using-the-sql-api"></a>Partitionieren in Azure Cosmos DB mithilfe der SQL-API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) ist ein globaler, verteilter Datenbankdienst, der mehrere Modelle unterstützt und dafür konzipiert wurde, dass Sie eine schnelle, vorhersagbare Leistung für Ihre Anwendung erzielen und diese während des Wachstums nahtlos skalieren können. 

Dieser Artikel enthält eine Übersicht über das Partitionieren von Cosmos DB-Containern mit der SQL-API. Eine Übersicht über Konzepte und bewährte Methoden für die Partitionierung mit einer beliebigen Azure Cosmos DB-API finden Sie unter [Partitionieren und horizontales Skalieren](../cosmos-db/partition-data.md). 

Laden Sie für die ersten Schritte mit dem Code das Projekt von [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark) herunter. 

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:   

* Wie funktioniert die Partitionierung in Azure Cosmos DB?
* Wie konfiguriere ich die Partitionierung in Azure Cosmos DB?
* Was sind Partitionsschlüssel, und wie kann ich den richtigen Partitionsschlüssel für meine Anwendung auswählen?

Sie können das Projekt aus dem [Azure Cosmos DB-Treiberbeispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark) herunterladen, um mit dem Programmieren zu beginnen. 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partitionsschlüssel

In der SQL-API geben Sie die Partitionsschlüsseldefinition in Form eines JSON-Pfads an. Die folgende Tabelle zeigt Beispiele für Definitionen der Partitionsschlüssel und die entsprechenden Werte. Der Partitionsschlüssel wird als Pfad angegeben. So stellt beispielsweise `/department` die Eigenschaft „Abteilung“ dar. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitionsschlüssel</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/department</p></td>
            <td valign="top"><p>Entspricht dem Wert „doc.department“, wobei „doc“ das Element ist.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/properties/name</p></td>
            <td valign="top"><p>Entspricht dem Wert „doc.properties.name“, wobei „doc“ das Element (verschachtelte Eigenschaft) ist.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/id</p></td>
            <td valign="top"><p>Entspricht dem Wert „doc.id“ („id“ und „partition key“ sind dieselbe Eigenschaft).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/"Abteilungsname"</p></td>
            <td valign="top"><p>Entspricht dem Wert „doc["Abteilungsname"]“, wobei „doc“ das Element ist.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> Die Syntax für den Partitionsschlüssel ähnelt der Pfadspezifikation für die Indizierung von Richtlinienpfaden. Der wichtigste Unterschied besteht darin, dass der Pfad der Eigenschaft anstelle des Werts entspricht, d. h., es gibt keinen Platzhalter am Ende. Beispielsweise würden Sie „/department/?“ angeben, um die Werte unter „department“ zu indizieren. Als Definition des Partitionsschlüssels würden Sie jedoch „/department“ angeben. Der Partitionsschlüssel wird implizit indiziert und kann nicht mithilfe von Überschreibungen für Indizierungsrichtlinien von der Indizierung ausgeschlossen werden.
> 
> 

Sehen wir uns an, wie sich die Auswahl der Partitionsschlüssel auf die Leistung Ihrer Anwendung auswirkt.

## <a name="working-with-the-azure-cosmos-db-sdks"></a>Arbeiten mit den Azure Cosmos DB-SDKs
Azure Cosmos DB bietet nun zusätzliche Unterstützung für die automatische Partitionierung durch [REST-API-Version 2015-12-16](/rest/api/cosmos-db/). Um partitionierte Container erstellen zu können, müssen Sie die SDK-Versionen 1.6.0 oder neuer in einer der unterstützten SDK Plattformen (.NET, Node.js, Java, Python, MongoDB) herunterladen. 

### <a name="creating-containers"></a>Erstellen von Containern
Das folgende Beispiel zeigt einen Ausschnitt für .NET, zum Erstellen eines Containers, um Gerätetelemetriedaten von 20.000 Anforderungseinheiten pro Durchsatzsekunde zu speichern. Das SDK legt den Wert für „OfferThroughput“ fest (der wiederum den Anforderungsheader `x-ms-offer-throughput` in der REST-API festlegt). Hier legen Sie `/deviceId` als Partitionsschlüssel fest. Die Wahl des Partitionsschlüssels wird zusammen mit dem Rest der Metadaten des Containers wie Name und Indizierungsrichtlinie gespeichert.

Für dieses Beispiel haben Sie `deviceId` ausgewählt, da es (a) möglich ist, die Schreibvorgänge aufgrund der großen Anzahl von Geräten gleichmäßig auf Partitionen zu verteilen, und Sie so die Datenbanken hochskalieren können, um riesige Datenmengen erfassen zu können, und da (b) viele der Anforderungen, wie das Abrufen der letzten Lesevorgänge für ein Gerät, einer einzelnen Geräte-ID zugeordnet sind und von einer einzelnen Partition abgerufen werden können.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Auf diese Weise wird ein REST-API-Aufruf in Cosmos DB durchgeführt, und der Dienst stellt eine Anzahl von Partitionen basierend auf dem angeforderten Durchsatz bereit. Sie können den Durchsatz eines Containers oder mehrerer Container ändern, wenn Sie eine höhere Leistung benötigen. 

### <a name="reading-and-writing-items"></a>Lesen und Schreiben von Elementen
Jetzt fügen wir Daten in Cosmos DB ein. Nachstehend finden Sie eine Beispielklasse, die eine Geräteanzeige enthält sowie einen Aufruf von „CreateDocumentAsync“, um ein neues Gerät mit Lesevorgang einem Container hinzuzufügen. Nachfolgend sehen Sie einen Beispielcodeblock, der die SQL-API nutzt:

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

Nun rufen wir das Element mithilfe seines Partitionsschlüssels und seiner ID auf, aktualisieren es und löschen es schließlich unter Angabe dieser beiden Werte. Der Lesevorgang enthält einen Wert für „PartitionKey“ (entsprechend dem Anforderungsheader `x-ms-documentdb-partitionkey` in der REST-API).

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a>Abfragen von partitionierten Containern
Beim Abfragen von Daten in partitionierten Containern leitet Cosmos DB die Abfrage automatisch an die Partitionen weiter, die den im Filter angegebenen Partitionsschlüsselwerten entsprechen (sofern vorhanden). Diese Abfrage wird z.B. nur an die Partition weitergeleitet, die den Partitionsschlüssel „XMS-0001“ enthält.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Die folgende Abfrage verfügt nicht über einen Filter für den Partitionsschlüssel (DeviceId) und wird an alle Partitionen verteilt, wo sie auf dem Partitionsindex ausgeführt wird. Sie müssen „EnableCrossPartitionQuery“ (`x-ms-documentdb-query-enablecrosspartition` in der REST-API) angeben, damit das SDK eine partitionsübergreifende Abfrage ausführen kann.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB unterstützt die [Aggregatfunktionen](sql-api-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX` und `AVG` in partitionierten Containern mit SQL, beginnend mit SDKs 1.12.0 und höher. Abfragen müssen einen einzelnen Aggregate-Operator und einen einzelnen Wert in der Projektion enthalten.

### <a name="parallel-query-execution"></a>Ausführung paralleler Abfragen
Ab dem Cosmos DB SDK 1.9.0 werden Optionen zur parallelen Ausführung von Abfragen unterstützt. Dadurch können Sie Abfragen mit niedriger Latenz auf partitionierte Sammlungen anwenden, auch wenn eine große Anzahl von Sammlungen berücksichtigt werden muss. Die folgende Abfrage ist z.B. so konfiguriert, dass sie partitionsübergreifend parallel ausgeführt wird.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Sie können die parallele Ausführung von Abfragen verwalten, indem Sie die folgenden Parameter optimieren:

* Durch Festlegen von `MaxDegreeOfParallelism` können Sie den Grad der Parallelität steuern, d.h. die maximale Anzahl gleichzeitiger Netzwerkverbindungen mit den Partitionen des Containers. Wenn Sie diese Eigenschaft auf „-1“ festlegen, wird der Grad der Parallelität vom SDK verwaltet. Falls `MaxDegreeOfParallelism` nicht angegeben oder auf 0 (Standardwert) festgelegt wurde, besteht eine einzelne Netzwerkverbindung mit den Partitionen des Containers.
* Durch Festlegen von `MaxBufferedItemCount` können Sie eine Abstimmung zwischen Abfragelatenz und Speicherauslastung auf Clientseite ermöglichen. Wenn Sie diesen Parameter weglassen oder diese Eigenschaft auf „-1“ festlegen, wird die Anzahl der Elemente, die während der Ausführung paralleler Abfragen gepuffert wird, vom SDK verwaltet.

Bei gleichem Zustand der Sammlung gibt eine parallele Abfrage Ergebnisse in der gleichen Reihenfolge wie bei einer seriellen Ausführung zurück. Beim Durchführen einer partitionsübergreifenden Abfrage mit Sortierung (ORDER BY und/oder TOP) führt das Azure Cosmos DB-SDK die Abfrage parallel auf den Partitionen durch und führt teilweise sortierte Ergebnisse auf Clientseite zusammen, um global sortierte Ergebnisse zu generieren.

### <a name="executing-stored-procedures"></a>Ausführen von gespeicherten Prozeduren
Sie können auch atomarische Transaktionen für Dokumente mit derselben Geräte-ID ausführen, etwa wenn Sie Aggregate oder den aktuellen Status eines Geräts in einem einzelnen Element verwalten. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
Im nächsten Abschnitt untersuchen Sie, wie partitionierte Container aus Containern mit nur einer Partition verschoben werden können.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde eine Übersicht über die Partitionierung von Azure Cosmos DB-Containern mit der SQL-API bereitgestellt. Eine Übersicht über Konzepte und bewährte Methoden für die Partitionierung mit einer beliebigen Azure Cosmos DB-API finden Sie auch unter [Partitionieren und horizontales Skalieren](../cosmos-db/partition-data.md). 

* Nutzen Sie Azure Cosmos DB, um Skalierungs- und Leistungstests durchzuführen. Ein Beispiel finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Erste Schritte bei der Codierung mit den [SDKs](sql-api-sdk-dotnet.md) oder der [REST-API](/rest/api/cosmos-db/)
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).


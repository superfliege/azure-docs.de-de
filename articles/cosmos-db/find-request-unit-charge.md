---
title: Ermitteln der Gebühr für eine Anforderungseinheit (Request Unit, RU) in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie die Gebühr für eine Anforderungseinheit für einen beliebigen Vorgang ermitteln, der für einen Azure Cosmos-Container ausgeführt wurde.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: thweiss
ms.openlocfilehash: 7afa815f81e2a61db8ac83623baafb97cb986b2c
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925356"
---
# <a name="find-the-request-unit-ru-charge-in-azure-cosmos-db"></a>Ermitteln der Gebühr für eine Anforderungseinheit (Request Unit, RU) in Azure Cosmos DB

In diesem Artikel werden verschiedene Methoden beschrieben, mit denen Sie den Verbrauch von [Anforderungseinheiten](request-units.md) für einen beliebigen Vorgang ermitteln können, der für einen Azure Cosmos-Container ausgeführt wurde. Der Verbrauch kann aktuell entweder über das Azure-Portal oder mithilfe eines der SDKs anhand der von Azure Cosmos DB zurückgegebenen Antwort ermittelt werden.

## <a name="sql-core-api"></a>SQL-API (Core-API)

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Über das Azure-Portal kann aktuell nur die Anforderungsgebühr für eine SQL-Abfrage ermittelt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-sql-api-dotnet.md#create-account), und fügen Sie ihm Daten hinzu, oder wählen Sie ein vorhandenes Azure Cosmos-Konto aus, das bereits Daten enthält.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie den gewünschten Container aus.

1. Klicken Sie auf **Neue SQL-Abfrage**.

1. Geben Sie eine gültige Abfrage ein, und klicken Sie auf **Abfrage ausführen**.

1. Klicken Sie auf **Query Stats** (Abfragestatistik), um die tatsächliche Anforderungsgebühr für die soeben ausgeführte Anforderung anzuzeigen.

![Screenshot: Anforderungsgebühr für eine SQL-Abfrage im Azure-Portal](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk-v2"></a>Verwenden des .NET SDK v2

Vom [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) zurückgegebene Objekte machen eine Eigenschaft vom Typ `RequestCharge` verfügbar. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-sql-api-dotnet.md).)

```csharp
ResourceResponse<Document> fetchDocumentResponse = await client.ReadDocumentAsync(
    UriFactory.CreateDocumentUri("database", "container", "itemId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
var requestCharge = fetchDocumentResponse.RequestCharge;

StoredProcedureResponse<string> storedProcedureCallResponse = await client.ExecuteStoredProcedureAsync<string>(
    UriFactory.CreateStoredProcedureUri("database", "container", "storedProcedureId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
requestCharge = storedProcedureCallResponse.RequestCharge;

IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri("database", "container"),
    "SELECT * FROM c",
    new FeedOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    }).AsDocumentQuery();
while (query.HasMoreResults)
{
    FeedResponse<dynamic> queryResponse = await query.ExecuteNextAsync<dynamic>();
    requestCharge = queryResponse.RequestCharge;
}
```

### <a name="use-the-java-sdk"></a>Verwenden des Java SDK

Vom [Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) zurückgegebene Objekte machen eine Methode vom Typ `getRequestCharge()` verfügbar. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-sql-api-java.md).)

```java
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<ResourceResponse<Document>> readDocumentResponse = client.readDocument(String.format("/dbs/%s/colls/%s/docs/%s", "database", "container", "itemId"), requestOptions);
readDocumentResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

Observable<StoredProcedureResponse> storedProcedureResponse = client.executeStoredProcedure(String.format("/dbs/%s/colls/%s/sprocs/%s", "database", "container", "storedProcedureId"), requestOptions, null);
storedProcedureResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

FeedOptions feedOptions = new FeedOptions();
feedOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<FeedResponse<Document>> feedResponse = client
    .queryDocuments(String.format("/dbs/%s/colls/%s", "database", "container"), "SELECT * FROM c", feedOptions);
feedResponse.forEach(result -> {
    double requestCharge = result.getRequestCharge();
});
```

### <a name="use-the-nodejs-sdk"></a>Verwenden des Node.js SDK

Vom [Node.js SDK](https://www.npmjs.com/package/@azure/cosmos) zurückgegebene Objekte machen ein Unterobjekt vom Typ `headers` verfügbar, das alle von der zugrunde liegenden HTTP-API zurückgegebenen Header zuordnet. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-sql-api-nodejs.md).) Die Anforderungsgebühr ist unter dem Schlüssel `x-ms-request-charge` verfügbar.

```javascript
const item = await client
    .database('database')
    .container('container')
    .item('itemId', 'partitionKey')
    .read();
var requestCharge = item.headers['x-ms-request-charge'];

const storedProcedureResult = await client
    .database('database')
    .container('container')
    .storedProcedure('storedProcedureId')
    .execute({
        partitionKey: 'partitionKey'
    });
requestCharge = storedProcedureResult.headers['x-ms-request-charge'];

const query = client.database('database')
    .container('container')
    .items
    .query('SELECT * FROM c', {
        partitionKey: 'partitionKey'
    });
while (query.hasMoreResults()) {
    var result = await query.executeNext();
    requestCharge = result.headers['x-ms-request-charge'];
}
```

### <a name="use-the-python-sdk"></a>Verwenden des Python SDK

Das Objekt `CosmosClient` des [Python SDK](https://pypi.org/project/azure-cosmos/) macht ein Wörterbuch vom Typ `last_response_headers` verfügbar, das alle Header zuordnet, die von der zugrunde liegenden HTTP-API für den letzten ausgeführten Vorgang zurückgegeben wurden. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-sql-api-python.md).) Die Anforderungsgebühr ist unter dem Schlüssel `x-ms-request-charge` verfügbar.

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB-API für MongoDB

Die Gebühr für die Anforderungseinheit wird durch einen benutzerdefinierten [Datenbankbefehl](https://docs.mongodb.com/manual/reference/command/) namens `getLastRequestStatistics.` verfügbar gemacht. Dieser Befehl gibt ein Dokument zurück, das den Namen des letzten ausgeführten Vorgangs sowie dessen Anforderungsgebühr und Dauer enthält.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Über das Azure-Portal kann aktuell nur die Anforderungsgebühr für eine Abfrage ermittelt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-mongodb-dotnet.md#create-a-database-account), und fügen Sie ihm Daten hinzu, oder wählen Sie ein vorhandenes Konto aus, das bereits Daten enthält.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie die gewünschte Sammlung aus.

1. Klicken Sie auf **Neue Abfrage**.

1. Geben Sie eine gültige Abfrage ein, und klicken Sie auf **Abfrage ausführen**.

1. Klicken Sie auf **Query Stats** (Abfragestatistik), um die tatsächliche Anforderungsgebühr für die soeben ausgeführte Anforderung anzuzeigen.

![Screenshot: Anforderungsgebühr für eine MongoDB-Abfrage im Azure-Portal](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>Verwenden des .NET-Treibers von MongoDB

Bei Verwendung des [offiziellen .NET-Treibers von MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/) kann die Methode `RunCommand` für ein Objekt vom Typ `IMongoDatabase` aufgerufen werden, um Befehle auszuführen. (Informationen zur Verwendung des Treibers finden Sie in [dieser Schnellstartanleitung](create-mongodb-dotnet.md).) Diese Methode erfordert eine Implementierung der abstrakten Klasse `Command<>`.

```csharp
class GetLastRequestStatisticsCommand : Command<Dictionary<string, object>>
{
    public override RenderedCommand<Dictionary<string, object>> Render(IBsonSerializerRegistry serializerRegistry)
    {
        return new RenderedCommand<Dictionary<string, object>>(new BsonDocument("getLastRequestStatistics", 1), serializerRegistry.GetSerializer<Dictionary<string, object>>());
    }
}

Dictionary<string, object> stats = database.RunCommand(new GetLastRequestStatisticsCommand());
double requestCharge = (double)stats["RequestCharge"];
```

### <a name="use-the-mongodb-java-driver"></a>Verwenden des Java-Treibers von MongoDB

Bei Verwendung des [offiziellen Java-Treibers von MongoDB](https://mongodb.github.io/mongo-java-driver/) kann die Methode `runCommand` für ein Objekt vom Typ `MongoDatabase` aufgerufen werden, um Befehle auszuführen. (Informationen zur Verwendung des Treibers finden Sie in [dieser Schnellstartanleitung](create-mongodb-java.md).)

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

### <a name="use-the-mongodb-nodejs-driver"></a>Verwenden des Node.js-Treibers von MongoDB

Bei Verwendung des [offiziellen Node.js-Treibers von MongoDB](https://mongodb.github.io/node-mongodb-native/) kann die Methode `command` für ein Objekt vom Typ `db` aufgerufen werden, um Befehle auszuführen. (Informationen zur Verwendung des Treibers finden Sie in [dieser Schnellstartanleitung](create-mongodb-nodejs.md).)

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

## <a name="cassandra-api"></a>Cassandra-API

Wenn Sie Vorgänge für die Cassandra-API von Azure Cosmos DB ausführen, wird die Gebühr für die Anforderungseinheit in der eingehenden Nutzlast als Feld namens `RequestCharge` zurückgegeben.

### <a name="use-the-net-sdk"></a>Verwenden des .NET SDK

Bei Verwendung des [.NET SDK](https://www.nuget.org/packages/CassandraCSharpDriver/) kann die eingehende Nutzlast unter der Eigenschaft `Info` eines Objekts vom Typ `RowSet` abgerufen werden. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-cassandra-dotnet.md).)

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"], 0);
```

### <a name="use-the-java-sdk"></a>Verwenden des Java SDK

Bei Verwendung des [Java SDK](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core) kann zum Abrufen der eingehenden Nutzlast die Methode `getExecutionInfo()` für ein Objekt vom Typ `ResultSet` aufgerufen werden. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-cassandra-java.md).)

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

## <a name="gremlin-api"></a>Gremlin-API

### <a name="use-drivers-and-sdk"></a>Verwenden von Treibern und SDK

Von der Gremlin-API zurückgegebene Header werden benutzerdefinierten Statusattributen zugeordnet, die aktuell durch das Gremlin .NET SDK und das Gremlin Java SDK bereitgestellt werden. Die Anforderungsgebühr ist unter dem Schlüssel `x-ms-request-charge` verfügbar.

### <a name="use-the-net-sdk"></a>Verwenden des .NET SDK

Bei Verwendung des [Gremlin .NET SDK](https://www.nuget.org/packages/Gremlin.Net/) stehen Statusattribute unter der Eigenschaft `StatusAttributes` des Objekts `ResultSet<>` zur Verfügung. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-graph-dotnet.md).)

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

### <a name="use-the-java-sdk"></a>Verwenden des Java SDK

Bei Verwendung des [Gremlin Java SDK](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) kann zum Abrufen von Statusattributen die Methode `statusAttributes()` für das Objekt `ResultSet` aufgerufen werden. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-graph-java.md).)

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

## <a name="table-api"></a>Tabelle-API

Das [.NET Standard SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) ist derzeit das einzige SDK, das die Gebühr für eine Anforderungseinheit für Tabellenvorgänge zurückgibt. (Informationen zur Verwendung des SDK finden Sie in [dieser Schnellstartanleitung](create-table-dotnet.md).) Das Objekt `TableResult` macht eine Eigenschaft vom Typ `RequestCharge` verfügbar, die durch das SDK aufgefüllt wird, wenn sie für die Tabellen-API von Azure Cosmos DB verwendet wird.

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie, wie Sie den Verbrauch von Anforderungseinheiten optimieren:

* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)
* [Optimieren der Kosten für bereitgestellten Durchsatz in Azure Cosmos DB](optimize-cost-throughput.md)
* [Optimieren der Abfragekosten in Azure Cosmos DB](optimize-cost-queries.md)
* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Provision throughput for an Azure Cosmos DB container](how-to-provision-container-throughput.md) (Bereitstellen des Durchsatzes für einen Azure Cosmos DB-Container)

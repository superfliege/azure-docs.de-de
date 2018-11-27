---
title: Verwalten der Konsistenz in Azure Cosmos DB
description: Verwalten der Konsistenz in Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 68c8c3767ff3a3d2873c1ff50928ab8d2cada4b1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263743"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Verwalten von Konsistenzebenen in Azure Cosmos DB

In diesem Artikel werden die verschiedenen Möglichkeiten beschrieben, mit denen Sie die Standardkonsistenz festlegen können. Außerdem finden Sie hier Informationen zur Außerkraftsetzung dieser Konsistenz auf dem Client, zur manuellen Verwaltung von Sitzungstoken sowie zur PBS-Metrik (Probabilistically Bounded Staleness).

## <a name="configure-the-default-consistency-level"></a>Konfigurieren der Standardkonsistenzebene

Die Standardkonsistenzebene ist die Konsistenzebene, die Clients standardmäßig verwenden. Sie kann von den Clients außer Kraft gesetzt werden.

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

Mit dem folgenden Beispiel wird ein neues Cosmos DB-Konto mit Multimaster-Aktivierung in den Regionen „USA, Osten“ und „USA, Westen“ erstellt und für die Standardkonsistenzrichtlinie die Option „Sitzung“ festgelegt.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="Session"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

### <a name="portal"></a>Portal

Melden Sie sich zum Anzeigen oder Ändern der Standardkonsistenzebene beim Azure-Portal an. Navigieren Sie zu Ihrem Cosmos DB-Konto, und öffnen Sie den Bereich **Standardkonsistenz**. Wählen Sie dort die Konsistenzebene aus, die Sie als neue Standardebene verwenden möchten, und klicken Sie anschließend auf „Speichern“.

![Abbildung des Konsistenzmenüs im Azure-Portal](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Außerkraftsetzen der Standardkonsistenzebene

Clients können die vom Dienst festgelegte Standardkonsistenzebene außer Kraft setzen. Dies kann für den gesamten Client oder pro Anforderung gelten.

### <a id="override-default-consistency-dotnet"></a>.NET SDK

```csharp
// Override consistency at the client level
ConsistencyPolicy consistencyPolicy = new ConsistencyPolicy
    {
        DefaultConsistencyLevel = ConsistencyLevel.BoundedStaleness,
        MaxStalenessIntervalInSeconds = 5,
        MaxStalenessPrefix = 100
    };
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, consistencyPolicy);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

### <a id="override-default-consistency-java-async"></a>Java Async SDK

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

### <a id="override-default-consistency-java-sync"></a>Java Sync SDK

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Strong);
```

### <a id="override-default-consistency-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a id="override-default-consistency-python"></a>Python SDK

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Strong)
```

## <a name="utilize-session-tokens"></a>Verwenden von Sitzungstoken

Wenn Sie Sitzungstoken manuell verwalten möchten, können Sie sie aus Antworten abrufen und pro Anforderung festlegen. Ist keine manuelle Verwaltung von Sitzungstoken erforderlich, können Sie die folgenden Beispiele ignorieren. Das SDK verfolgt Sitzungstoken automatisch nach und verwendet das aktuelle Sitzungstoken, wenn Sie das Sitzungstoken nicht selbst festlegen.

### <a id="utilize-session-tokens-dotnet"></a>.NET SDK

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

### <a id="utilize-session-tokens-java-async"></a>Java Async SDK

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

### <a id="utilize-session-tokens-java-sync"></a>Java Sync SDK

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```

### <a id="utilize-session-tokens-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a id="utilize-session-tokens-python"></a>Python SDK

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Überwachen der PBS-Metrik (Probabilistically Bounded Staleness)

Navigieren Sie zum Anzeigen der PBS-Metrik im Azure-Portal zu Ihrem Cosmos DB-Konto, und öffnen Sie den Bereich **Metriken**. Klicken Sie auf die Registerkarte **Konsistenz**, und sehen Sie sich den Graphen **Wahrscheinlichkeit stark konsistenter Lesevorgänge basierend auf Ihrer Workload (siehe PBS)** an.

![Abbildung des PBS-Graphen im Azure-Portal](./media/how-to-manage-consistency/pbs-metric.png)

Verwenden Sie das Metrikmenü von Cosmos DB, um diese Metrik anzuzeigen. In der Metrikumgebung der Azure-Überwachung wird sie nicht angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Anhand der folgenden Dokumente können Sie sich ausführlicher über den Umgang mit Datenkonflikten informieren oder sich mit dem nächsten zentralen Konzept in Cosmos DB vertraut machen:

* [Behandeln von Konflikten zwischen Regionen](how-to-manage-conflicts.md)
* [Partitionierung und Datenverteilung](partition-data.md)

---
title: Konfigurieren von Multimaster-Features in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Multimaster-Features in Ihren Anwendungen in Azure Cosmos DB konfigurieren.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: b862c59002369662d37b6d6a9de28370b0000497
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682269"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Konfigurieren von Multimaster-Features in Ihren Anwendungen, die Azure Cosmos DB verwenden

Damit Sie Multimaster-Funktionen in Ihren Anwendungen nutzen können, müssen Sie Schreibvorgänge in mehreren Regionen aktivieren und die Multihoming-Funktionen in Azure Cosmos DB konfigurieren. Konfigurieren Sie Multihoming, indem Sie die Region auf den Bereitstellungsort der Anwendung festlegen.

## <a id="netv2"></a>.NET SDK v2

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `UseMultipleWriteLocations` auf „true“ und `SetCurrentLocation` auf die Region fest, in der die Anwendung bereitgestellt und Azure Cosmos DB repliziert wird.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3 (Vorschauversion)

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `UseCurrentRegion` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java Async SDK

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `policy.setUsingMultipleWriteLocations(true)` und `policy.setPreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>Node.js, JavaScript, TypeScript SDK

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `connectionPolicy.UseMultipleWriteLocations` auf „true“ und `connectionPolicy.PreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>Python SDK

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `connection_policy.UseMultipleWriteLocations` auf „true“ und `connection_policy.PreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie die folgenden Artikel lesen:

* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Konflikttypen und Konfliktauflösungsrichtlinien](conflict-resolution-policies.md)
* [Hochverfügbarkeit mit Azure Cosmos DB](high-availability.md)
* [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)
* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)
* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)

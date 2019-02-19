---
title: Konfigurieren von Multimaster-Features in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Multimaster-Features in Ihren Anwendungen in Azure Cosmos DB konfigurieren.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: effe6fa942ce0cabace08e72dba90baf8646680e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118825"
---
# <a name="how-to-configure-multi-master-in-your-applications-in-azure-cosmos-db"></a>Konfigurieren von Multimaster-Features in Ihren Anwendungen in Azure Cosmos DB

Damit Sie Multimaster-Features in Ihren Anwendungen verwenden können, müssen Sie Schreibvorgänge in mehreren Regionen in Ihrer Anwendung aktivieren und die Multihoming-Funktionen konfigurieren. Legen Sie dazu die aktuelle Region fest, in der die Anwendung bereitgestellt wird.

## <a id="netv2"></a>.NET SDK v2

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `UseMultipleWriteLocations` auf „true“ und `SetCurrentLocation` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

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

Legen Sie zum Aktivieren von Multimaster in Ihren Anwendungen `policy.setUsingMultipleWriteLocations(true)` auf „true“ und `policy.setPreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt und Cosmos DB repliziert wird.

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

Informieren Sie sich umfassender über die globale Verteilung und Konsistenz in Azure Cosmos DB. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)

* [Konflikttypen und Konfliktauflösungsrichtlinien](conflict-resolution-policies.md)

* [Hochverfügbarkeit mit Azure Cosmos DB](high-availability.md)

* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)

* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)

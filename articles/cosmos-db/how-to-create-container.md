---
title: Erstellen eines Containers in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie einen Container in Azure Cosmos DB erstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 8ce890500c31c1966254e5bca9d23c8fcdd7bb67
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258291"
---
# <a name="create-an-azure-cosmos-container"></a>Erstellen eines Azure Cosmos-Containers

In diesem Artikel werden die verschiedenen Möglichkeiten zur Erstellung eines Containers (Sammlung, Tabelle oder Diagramm) erläutert. Sie verwenden das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder unterstützte SDKs. In diesem Artikel erfahren Sie, wie Sie einen Container erstellen, den Partitionsschlüssel angeben und den Durchsatz bereitstellen.

## <a name="create-a-container-by-using-azure-portal"></a>Erstellen eines Containers über das Azure-Portal

### <a id="portal-sql"></a>SQL-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Sammlungs-ID ein.
   * Geben Sie einen Partitionsschlüssel ein.
   * Geben Sie einen Durchsatz (beispielsweise 1.000 RUs) ein.
   * Klicken Sie auf **OK**.

![Screenshot des Bereichs „Daten-Explorer“ mit hervorgehobener Option „Neue Sammlung“](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB-API für MongoDB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-mongodb-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Sammlungs-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Shardschlüssel ein.
   * Geben Sie einen Durchsatz (beispielsweise 1.000 RUs) ein.
   * Klicken Sie auf **OK**.

![Screenshot der Azure Cosmos DB-API für MongoDB, Dialogfeld „Sammlung hinzufügen“](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-cassandra-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie einen neuen Keyspace erstellen oder einen vorhandenen Keyspace verwenden.
   * Geben Sie einen Tabellennamen ein.
   * Geben Sie die Eigenschaften ein, und geben Sie einen Primärschlüssel an.
   * Geben Sie einen Durchsatz (beispielsweise 1.000 RUs) ein.
   * Klicken Sie auf **OK**.

![Screenshot der Cassandra-API, Dialogfeld „Tabelle hinzufügen“](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Bei der Cassandra-API wird der Primärschlüssel als Partitionsschlüssel verwendet.

### <a id="portal-gremlin"></a>Gremlin-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-graph-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **New Graph** (Neues Diagramm) aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Diagramm-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Partitionsschlüssel für Vertices ein.
   * Geben Sie einen Durchsatz (beispielsweise 1.000 RUs) ein.
   * Klicken Sie auf **OK**.

![Screenshot der Gremlin-API, Dialogfeld „Diagramm hinzufügen“](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Tabellen-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-table-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie eine Tabellen-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Durchsatz (beispielsweise 1.000 RUs) ein.
   * Klicken Sie auf **OK**.

![Screenshot der Tabellen-API, Dialogfeld „Tabelle hinzufügen“](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Bei der Tabellen-API wird der Partitionsschlüssel jedes Mal angegeben, wenn Sie eine neue Zeile hinzufügen.

## <a name="create-a-container-by-using-azure-cli"></a>Erstellen eines Containers mithilfe der Azure-Befehlszeilenschnittstelle

### <a id="cli-sql"></a>SQL-API

```azurecli-interactive
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-mongodb"></a>Azure Cosmos DB-API für MongoDB

```azurecli-interactive
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

### <a id="cli-cassandra"></a>Cassandra-API

```azurecli-interactive
# Create a table with a partition/primary key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 1000
```

### <a id="cli-gremlin"></a>Gremlin-API

```azurecli-interactive
# Create a graph with a partition key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-table"></a>Tabellen-API

```azurecli-interactive
# Create a table with 1000 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 1000
```

## <a name="create-a-container-by-using-net-sdk"></a>Erstellen eines Containers mithilfe des .NET SDK

### <a id="dotnet-sql-graph"></a>SQL-API und Gremlin-API

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>Azure Cosmos DB-API für MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB Wire Protocol fordert keine Einheiten an. Wenn Sie eine neue Sammlung mit Durchsatz erstellen möchten, verwenden Sie das Azure-Portal oder die SQL-API.

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Nächste Schritte

- [Partitioning in Azure Cosmos DB](partitioning-overview.md) (Partitionierung in Azure Cosmos DB)

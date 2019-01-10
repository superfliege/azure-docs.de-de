---
title: Erstellen eines Containers in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie einen Container in Azure Cosmos DB erstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 7434407c7d1afe197a560d9ed9c4f1e7e94b6c2b
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033535"
---
# <a name="create-a-container-in-azure-cosmos-db"></a>Erstellen eines Containers in Azure Cosmos DB

In diesem Artikel werden die verschiedenen Möglichkeiten zur Erstellung eines Containers (Sammlung, Tabelle, Diagramm) erläutert. Container können über das Azure-Portal, über die Azure-Befehlszeilenschnittstelle oder mithilfe unterstützter SDKs erstellt werden. In diesem Artikel erfahren Sie, wie Sie einen Container erstellen, den Partitionsschlüssel angeben und den Durchsatz bereitstellen.

## <a name="create-a-container-using-azure-portal"></a>Erstellen eines Containers über das Azure-Portal

### <a id="portal-sql"></a>SQL-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-sql-api-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Erstellen Sie eine neue Datenbank, oder verwenden Sie eine bereits vorhandene Datenbank.
   * Geben Sie eine Sammlungs-ID ein.
   * Geben Sie einen Partitionsschlüssel ein.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Erstellung einer Sammlung durch die SQL-API](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB-API für MongoDB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-mongodb-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Erstellen Sie eine neue Datenbank, oder verwenden Sie eine bereits vorhandene Datenbank.
   * Geben Sie eine Sammlungs-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Shardschlüssel ein.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Erstellen einer Sammlung mit der Azure Cosmos DB-API für MongoDB](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-cassandra-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Erstellen Sie einen neuen Keyspace, oder verwenden Sie einen bereits vorhandenen Keyspace.
   * Geben Sie einen Tabellennamen ein.
   * Geben Sie die Eigenschaften ein, und geben Sie einen PRIMÄRSCHLÜSSEL an.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Erstellung einer Sammlung durch die Cassandra-API](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Bei der Cassandra-API wird der Primärschlüssel als Partitionsschlüssel verwendet.

### <a id="portal-gremlin"></a>Gremlin-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-graph-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **New Graph** (Neues Diagramm) aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Erstellen Sie eine neue Datenbank, oder verwenden Sie eine bereits vorhandene Datenbank.
   * Geben Sie eine Diagramm-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Partitionsschlüssel für Vertices ein.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Erstellung einer Sammlung durch die Gremlin-API](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Tabellen-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-table-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Geben Sie eine Tabellen-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Erstellung einer Sammlung durch die Tabellen-API](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Bei der Tabellen-API wird der Partitionsschlüssel jedes Mal angegeben, wenn Sie eine neue Zeile hinzufügen.

## <a name="create-a-container-using-azure-cli"></a>Erstellen eines Containers über die Azure-Befehlszeilenschnittstelle

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

## <a name="create-a-container-using-net-sdk"></a>Erstellen eines Containers mithilfe des .NET SDK

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
MongoDB Wire Protocol verfügt über kein Konzept für Anforderungseinheiten. Wenn Sie eine neue Sammlung mit Durchsatz erstellen möchten, verwenden Sie das Azure-Portal oder die SQL-API, wie in den vorherigen Beispielen gezeigt.

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Partitionierung in Cosmos DB finden Sie in den folgenden Artikeln:

- [Partitioning in Azure Cosmos DB](partitioning-overview.md) (Partitionierung in Azure Cosmos DB)

---
title: Bereitstellen des Containerdurchsatzes in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie in Azure Cosmos DB Durchsatz auf der Containerebene bereitstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 550201e692bb79197d50c2f44017c43ab9ea2016
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477326"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Bereitstellen von Durchsatz für einen Azure Cosmos-Container

In diesem Artikel erfahren Sie, wie Sie Durchsatz für einen Container (Sammlung, Diagramm, Tabelle) in Azure Cosmos DB bereitstellen. Sie können Durchsatz für einen einzelnen Container oder [für eine Datenbank](how-to-provision-database-throughput.md) bereitstellen und ihn für die in der Datenbank enthaltenen Container freigeben. Der Durchsatz für einen Container kann über das Azure-Portal, über die Azure-Befehlszeilenschnittstelle oder mithilfe von Cosmos DB SDKs bereitgestellt werden.

## <a name="provision-throughput-using-azure-portal"></a>Bereitstellen des Durchsatzes mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. [Erstellen Sie ein neues Cosmos DB-Konto](create-sql-api-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Sammlung** aus. Füllen Sie das Formular mit folgenden Angaben aus:

   * Erstellen Sie eine neue Datenbank, oder verwenden Sie eine bereits vorhandene Datenbank.
   * Geben Sie eine Sammlungs-ID (oder eine Tabelle/ein Diagramm) ein.
   * Geben Sie einen Partitionsschlüsselwert ein, etwa `/userid`.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![SQL-API: Bereitstellen des Containerdurchsatzes](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Bereitstellen des Durchsatzes mithilfe der Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

Wenn Sie Durchsatz für ein Cosmos-Konto bereitstellen, das mit der Azure Cosmos DB-API für MongoDB konfiguriert wurde, verwenden Sie „/myShardKey“ für den Partitionsschlüsselpfad. Wenn Sie Durchsatz für ein Cosmos-Konto bereitstellen, das für die Cassandra-API konfiguriert wurde, verwenden Sie „/myPrimaryKey“ für den Partitionsschlüsselpfad.

## <a name="provision-throughput-using-net-sdk"></a>Bereitstellen des Durchsatzes mithilfe des .NET SDK

> [!Note]
> Verwenden Sie die SQL-API, um Durchsatz für alle APIs (mit Ausnahme der Cassandra-API) bereitzustellen.

### <a id="dotnet-most"></a>SQL-, MongoDB-, Gremlin- und Tabellen-API

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Durchsatzbereitstellung in Cosmos DB finden Sie in den folgenden Artikeln:

* [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-database-throughput.md)
* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)

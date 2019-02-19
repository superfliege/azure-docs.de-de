---
title: Bereitstellen des Datenbankdurchsatzes in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie in Azure Cosmos DB Durchsatz auf Datenbankebene bereitstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 759adf95604e66209cf3ec5083246d16e952114a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884187"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB

In diesem Artikel wird erläutert, wie Sie Durchsatz für eine Datenbank in Azure Cosmos DB bereitstellen. Sie können Durchsatz für einen einzelnen [Container](how-to-provision-container-throughput.md) oder für eine Datenbank bereitstellen und den Durchsatz für die darin enthaltenen Container freigeben. Durchsatz auf Datenbankebene kann über das Azure-Portal oder über Azure Cosmos DB SDKs bereitgestellt werden.

## <a name="provision-throughput-by-using-azure-portal"></a>Bereitstellen des Durchsatzes mithilfe des Azure-Portals

### <a id="portal-sql"></a>SQL-API (Core-API)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos DB-Konto](create-sql-api-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Datenbank** aus. Geben Sie die folgenden Details an:

   * Geben Sie eine Datenbank-ID ein. 
   * Aktivieren Sie das Kontrollkästchen **Durchsatz bereitstellen**.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

![Screenshot: Dialogfeld „Neue Datenbank“](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-net-sdk"></a>Bereitstellen des Durchsatzes mithilfe des .NET SDK

> [!Note]
> Verwenden Sie die SQL-API, um Durchsatz für alle APIs bereitzustellen. Sie können optional auch das folgende Beispiel für die Cassandra-API verwenden.

### <a id="dotnet-all"></a>Alle APIs

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen des Durchsatzes in Azure Cosmos DB finden Sie in den folgenden Artikeln:

* [Provision throughput for an Azure Cosmos DB container](how-to-provision-container-throughput.md) (Bereitstellen des Durchsatzes für einen Azure Cosmos DB-Container)
* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)

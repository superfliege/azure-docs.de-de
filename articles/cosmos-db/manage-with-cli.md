---
title: Verwalten von Azure Cosmos DB-Ressourcen mit der Azure CLI
description: Verwenden Sie die Azure CLI zum Verwalten Ihrer Konten, Datenbanken und Containern für Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: c3028fd18bd9afefaa18f7f515a43a852ddef78a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464397"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Verwalten von Azure Cosmos-Ressourcen mit der Azure CLI

Der folgende Leitfaden erläutert die Befehle zum Automatisieren der Verwaltung Ihrer Azure Cosmos DB-Konten und -Container mithilfe von Azure CLI. Außerdem finden Sie hier Befehle zum Skalieren des Containerdurchsatzes. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](https://docs.microsoft.com/cli/azure/cosmosdb) verfügbar. Weitere Beispiele finden Sie auch in [Azur CLI-Beispiele für Azure Cosmos DB](cli-samples.md), einschließlich der Erstellung und Verwaltung von Cosmos DB-Konten, Datenbanken und Containern für MongoDB, Gremlin, Cassandra und die Tabellen-API.

Mit diesem CLI-Beispielskript erstellen Sie ein SQL-API-Konto, eine Datenbank und einen Container für Azure Cosmos DB.  

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Zum Erstellen eines Azure Cosmos DB-Kontos mit der SQL-API, Sitzungskonsistenz und Multimasterunterstützung für die Regionen „USA, Osten“ und „USA, Westen“, öffnen Sie die Azure CLI oder Cloud Shell und führen den folgenden Befehl aus:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>Erstellen einer Datenbank

Zum Erstellen einer Cosmos DB-Datenbank, öffnen Sie die Azure CLI oder Cloud-Shell, und führen Sie den folgenden Befehl aus:

```azurecli-interactive
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>Erstellen eines Containers

Um einen Cosmos DB-Container mit 1000 RU/s und einem Partitionsschlüssel zu erstellen, öffnen Sie Azure CLI oder Cloud-Shell, und führen Sie den folgenden Befehl aus:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>Ändern des Durchsatzes eines Containers

Um den Durchsatz eines Cosmos DB-Containers mit 400 RU/s und zu ändern, öffnen Sie Azure CLI oder Cloud-Shell, und führen Sie den folgenden Befehl aus:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>Auflisten von Kontoschlüsseln

Wenn Sie ein Azure Cosmos DB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das Azure Cosmos DB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht Azure Cosmos DB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das Azure Cosmos DB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär). Anhand des folgenden Befehls können Sie die Schlüssel für Ihr Konto abrufen:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>Auflisten von Verbindungszeichenfolgen

Die Verbindungszeichenfolge zum Verbinden Ihrer Anwendung mit einem Cosmos DB-Konto können mit dem folgenden Befehl abgerufen werden.

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>Erneutes Generieren eines Kontoschlüssels

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr Azure Cosmos DB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum Azure Cosmos DB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in:

- [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)
- [Azure-CLI-Referenz](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Zusätzliche Azure CLI-Beispiele für Azure Cosmos DB](cli-samples.md)

---
title: Azure CLI-Beispiele für Azure Cosmos DB | Microsoft-Dokumentation
description: Azure CLI-Beispiele – Erstellen und Verwalten von Konten, Datenbanken, Containern, Regionen und Firewalls von Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 461207d0c9d27ed645dcac98e6256431bb23f8ad
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005987"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure CLI-Beispielskripts für Azure Cosmos DB. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](/cli/azure/cosmosdb) verfügbar.

| |  |
|---|---|
|**Erstellen von Konten, Datenbanken und Containern für Azure Cosmos DB**||
| [Erstellen eines SQL-API-Kontos](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines SQL-API-Kontos, einer Datenbank und eines Containers für Azure Cosmos DB |
| [Erstellen eines Kontos für die MongoDB-API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines Kontos, einer Datenbank und eines Containers für eine einzige MongoDB-API von Azure Cosmos DB |
| [Erstellen eines Gremlin-API-Kontos](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines Gremlin-API-Kontos, einer Datenbank und eines Graphs für Azure Cosmos DB |
| [Erstellen eines Cassandra-API-Kontos](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines Cassandra-API-Kontos und einer Datenbank für Azure Cosmos DB |
| [Erstellen eines Tabellen-API-Kontos](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines Tabellen-API-Kontos, einer Datenbank und einer Tabelle für Azure Cosmos DB |
|**Skalieren von Azure Cosmos DB**||
| [Skalieren des Containerdurchsatzes](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Ändern des in einem Container bereitgestellten Durchsatzes|
| [Replizieren des Datenbankkontos von Azure Cosmos DB in mehreren Regionen und Konfigurieren der Failoverprioritäten](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globales Replizieren von Kontodaten in verschiedenen Regionen mit einer festgelegten Failoverpriorität|
|**Schützen von Azure Cosmos DB**||
| [Abrufen von Kontoschlüsseln](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der primären und sekundären Hauptschlüssel zum Schreiben und der sekundären schreibgeschützten Schlüssel für das Konto|
| [Abrufen der MongoDB-Verbindungszeichenfolge](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der Verbindungszeichenfolge zum Herstellen einer Verbindung zwischen der MongoDB-App und dem Azure Cosmos DB-Konto|
| [Erneutes Generieren von Kontoschlüsseln](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Erneutes Generieren der Schlüssel für das Konto|
| [Erstellen einer Firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen einer Zugriffssteuerungsrichtlinie für eingehende IPs zur Beschränkung des Zugriffs auf das Konto über eine genehmigte Gruppe von Computern und/oder Clouddiensten|
|**Hochverfügbarkeit, Notfallwiederherstellung, Sicherung und Wiederherstellung**||
| [Konfigurieren der Failoverrichtlinie](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Festlegen der Failoverpriorität einzelner Regionen, in denen das Konto repliziert wird|
|**Herstellen einer Verbindung zwischen Azure Cosmos DB und Ressourcen**||
| [Herstellen einer Verbindung zwischen einer Web-App und Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Erstellen und Herstellen einer Verbindung zwischen einer Azure Cosmos DB-Datenbank und einer Azure-Web-App|
|||

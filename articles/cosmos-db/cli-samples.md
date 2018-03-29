---
title: Azure CLI-Beispiele für Azure Cosmos DB | Microsoft-Dokumentation
description: Azure CLI-Beispiele – Erstellen und Verwalten von Konten, Datenbanken, Containern, Regionen und Firewalls von Azure Cosmos DB
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 11/29/2017
ms.author: mimig
ms.openlocfilehash: 5a649279385da882687c50edd53e16f5ebaed163
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure CLI-Beispielskripts für Azure Cosmos DB. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zu Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb) verfügbar.

| |  |
|---|---|
|**Erstellen von Konten, Datenbanken und Containern für Azure Cosmos DB**||
|[Erstellen eines SQL-API-Kontos](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines Kontos, einer Datenbank und eines Containers für eine einzige API von Azure Cosmos DB für die Verwendung mit der SQL-API |
| [Erstellen eines Kontos für die MongoDB-API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines Kontos, einer Datenbank und eines Containers für eine einzige MongoDB-API von Azure Cosmos DB |
|**Skalieren von Azure Cosmos DB**||
| [Skalieren des Containerdurchsatzes](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Ändern des in einem Container bereitgestellten Durchsatzes|
|[Replizieren des Datenbankkontos von Azure Cosmos DB in mehreren Regionen und Konfigurieren der Failoverprioritäten](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globales Replizieren von Kontodaten in verschiedenen Regionen mit einer festgelegten Failoverpriorität|
|**Schützen von Azure Cosmos DB**||
| [Abrufen von Kontoschlüsseln](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der primären und sekundären Hauptschlüssel zum Schreiben und der sekundären schreibgeschützten Schlüssel für das Konto|
| [Abrufen der MongoDB-Verbindungszeichenfolge](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der Verbindungszeichenfolge zum Herstellen einer Verbindung zwischen der MongoDB-App und dem Azure Cosmos DB-Konto|
|[Erneutes Generieren von Kontoschlüsseln](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Erneutes Generieren des Haupt- oder schreibgeschützten Schlüssels für das Konto|
|[Erstellen einer Firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen einer Zugriffssteuerungsrichtlinie für eingehende IPs zur Beschränkung des Zugriffs auf das Konto über eine genehmigte Gruppe von Computern und/oder Clouddiensten|
|**Hochverfügbarkeit, Notfallwiederherstellung, Sicherung und Wiederherstellung**||
|[Konfigurieren der Failoverrichtlinie](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Festlegen der Failoverpriorität einzelner Regionen, in denen das Konto repliziert wird|
|**Herstellen einer Verbindung zwischen Azure Cosmos DB und den Ressourcen**||
|[Herstellen einer Verbindung zwischen einer Web-App und Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Erstellen und Herstellen einer Verbindung zwischen einer Azure Cosmos DB-Datenbank und einer Azure-Web-App|
|||

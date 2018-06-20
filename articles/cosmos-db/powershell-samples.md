---
title: Azure PowerShell-Beispiele für Azure Cosmos DB | Microsoft-Dokumentation
description: 'Azure PowerShell-Beispiele: Skripts, die Ihnen beim Erstellen und Verwalten von Azure Cosmos DB-Konten helfen.'
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 71e75b4b2bb60ad2c1ae1a468991fe1f5520136c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700365"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure PowerShell-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure Cosmos DB. Derzeit können Sie nur das Azure Cosmos DB-Konto über PowerShell verwalten. Andere Ressourcen wie Datenbanken und Sammlungen können nicht über PowerShell verwaltet werden.

| |  |
|---|---|
|**Erstellen eines Azure Cosmos DB-Kontos**||
|[Erstellen eines SQL-API-Kontos](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines einzelnen Azure Cosmos DB-Kontos zur Verwendung mit der SQL-API |
|[Erstellen eines Kontos für die MongoDB-API](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines einzelnen Azure Cosmos DB-Kontos zur Verwendung mit der MongoDB-API |
|[Erstellen eines Gremlin-API-Kontos](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines einzelnen Azure Cosmos DB-Kontos zur Verwendung mit der Gremlin-API |
|**Skalieren von Azure Cosmos DB**||
|[Replizieren des Azure Cosmos DB-Kontos in mehreren Regionen und Konfigurieren der Failoverprioritäten](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Globales Replizieren von Kontodaten in verschiedenen Regionen mit einer festgelegten Failoverpriorität|
|**Schützen von Azure Cosmos DB**||
| [Abrufen von Kontoschlüsseln](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Abrufen der primären und sekundären Hauptschlüssel zum Schreiben und der sekundären schreibgeschützten Schlüssel für das Konto|
| [Abrufen der MongoDB-Verbindungszeichenfolge](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Abrufen der Verbindungszeichenfolge zum Herstellen einer Verbindung zwischen der MongoDB-App und dem Azure Cosmos DB-Konto|
|[Erneutes Generieren von Kontoschlüsseln](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Erneutes Generieren des Haupt- oder schreibgeschützten Schlüssels für das Konto|
|[Erstellen einer Firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen einer Zugriffssteuerungsrichtlinie für eingehende IPs zur Beschränkung des Zugriffs auf das Konto über eine genehmigte Gruppe von Computern und/oder Clouddiensten|
|**Hochverfügbarkeit, Notfallwiederherstellung, Sicherung und Wiederherstellung**||
|[Konfigurieren der Failoverrichtlinie](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Festlegen der Failoverpriorität einzelner Regionen, in denen das Konto repliziert wird|
|||

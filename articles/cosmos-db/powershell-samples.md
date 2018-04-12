---
title: Azure PowerShell-Beispiele für Azure Cosmos DB | Microsoft-Dokumentation
description: 'Azure PowerShell-Beispiele: Skripts, die Ihnen beim Erstellen und Verwalten von Azure Cosmos DB-Konten helfen.'
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: fc2ae1ee87b525f2ec6725cea2b19a8b8b3cef86
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure PowerShell-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure Cosmos DB. Derzeit können Sie nur den Azure Cosmos DB-Accountlayer über PowerShell verwalten; andere Ressourcen wie Datenbanken und Sammlungen können nicht über PowerShell verwaltet werden.

| |  |
|---|---|
|**Erstellen eines Azure Cosmos DB-Kontos**||
|[Erstellen eines SQL-API-Kontos](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen eines einzelnen Azure Cosmos DB-Kontos zur Verwendung mit der SQL-API |
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

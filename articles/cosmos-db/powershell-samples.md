---
title: Azure PowerShell-Beispiele für Azure Cosmos DB
description: 'Azure PowerShell-Beispiele: Skripts, die Ihnen beim Erstellen und Verwalten von Azure Cosmos DB-Konten helfen.'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/08/2019
ms.author: mjbrown
ms.openlocfilehash: 68e845a05f4ebe2d1f25b55c00042c8925c8109e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069304"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure PowerShell-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure Cosmos DB für Core (SQL) API.

| |  |
|---|---|
|**Azure Cosmos DB-Konten**||
|[Erstellen eines Kontos](scripts/powershell/sql/ps-account-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt ein Azure Cosmos SQL-API-Konto. |
|[Konto abrufen](scripts/powershell/sql/ps-account-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen der Eigenschaften eines Azure Cosmos DB-Kontos. |
|[Region hinzufügen](scripts/powershell/sql/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen eines Azure Cosmos-Kontos und Hinzufügen einer Region zur Liste der Speicherorte. |
|[Failoverpriorität ändern](scripts/powershell/sql/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ändern der Failoverpriorität eines Azure Cosmos-Kontos mit einem manuellen Failovertrigger. |
|[Tags aktualisieren](scripts/powershell/sql/ps-account-tags-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aktualisieren der Tags für ein Azure Cosmos-Konto. |
|[Abrufen von Kontoschlüsseln](scripts/powershell/sql/ps-account-key-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen der Primär- und Sekundärschlüssel eines Azure Cosmos-Kontos. |
|[Erneutes Generieren von Kontoschlüsseln](scripts/powershell/sql/ps-account-key-regenerate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erneutes Generieren der Primär- und Sekundärschlüssel eines Azure Cosmos-Kontos. |
|[Verbindungszeichenfolgen auflisten](scripts/powershell/sql/ps-account-connection-string-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Abrufen Sie der primären und sekundären Verbindungszeichenfolgen eines Azure Cosmos-Kontos. |
|[IP-Firewall erstellen](scripts/powershell/sql/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellen einer IP-Firewall für ein Azure Cosmos-Konto. |
|[Azure Cosmos DB-Konto löschen](scripts/powershell/sql/ps-account-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Löschen eines Azure Cosmos-Kontos. |
|**Azure Cosmos-Datenbanken**||
| [Erstellen einer Datenbank](scripts/powershell/sql/ps-database-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen einer Datenbank innerhalb eines Azure-Cosmos-Kontos.|
| [Datenbank mit geteiltem Durchsatz/Durchsatz auf Datenbankebene erstellen](scripts/powershell/sql/ps-database-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Eine Azure Cosmos-Datenbank mit Durchsatz auf Datenbankebene erstellen, die für ihre Container freigegeben ist.|
| [Alle Datenbanken auflisten](scripts/powershell/sql/ps-database-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Auflisten aller Datenbanken in einem Azure Cosmos-Konto.|
| [Datenbank abrufen](scripts/powershell/sql/ps-database-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Abrufen der Eigenschaften einer Azure Cosmos-Datenbank.|
|**Azure Cosmos-Container**||
| [Container erstellen](scripts/powershell/sql/ps-container-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos-Containers mit dediziertem Durchsatz.|
| [Container mit freigegebenem Durchsatz erstellen](scripts/powershell/sql/ps-container-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Einen Azure Cosmos-Container mit für andere Container in der Datenbank freigegebenem Durchsatz erstellen.|
| [Container mit Indexrichtlinie erstellen](scripts/powershell/sql/ps-container-create-index-custom.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure Cosmos-Container mit einer benutzerdefinierten Indexrichtlinie erstellen.|
| [Container ohne Indexrichtlinie erstellen](scripts/powershell/sql/ps-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos DB-Containers mit deaktivierter Indexrichtlinie.|
| [Container mit eindeutigen Schlüsseln und TTL erstellen](scripts/powershell/sql/ps-container-create-unique-key-ttl.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos-Containers mit einer eindeutigen Schlüsseleinschränkung und konfigurierter Gültigkeitsdauer.|
| [Container mit Konfliktlösung erstellen](scripts/powershell/sql/ps-container-create-conflict-policy.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellen eines Azure Cosmos-Containers mit einer Konfliktlösungsrichtlinie, die dem letzten Schreiber Vorrang gibt.|
| [Alle Container auflisten](scripts/powershell/sql/ps-container-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Auflisten aller Container in einer Azure Cosmos-Datenbank.|
| [Container abrufen](scripts/powershell/sql/ps-container-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Abrufen der Eigenschaften für einen Container in einer Azure Cosmos-Datenbank.|
| [Löschen eines Containers](scripts/powershell/sql/ps-container-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Löschen eines Containers in einer Azure Cosmos-Datenbank.|
|||
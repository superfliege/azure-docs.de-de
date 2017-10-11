---
title: "Azure PowerShell-Beispiele für Azure-Cosmos-DB | Microsoft Docs"
description: "Azure PowerShell-Beispiele - Skripts helfen Ihnen beim Erstellen und Verwalten von Konten für Azure-Cosmos-Datenbank."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure PowerShell-Beispiele für Azure-Cosmos-DB

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure-Cosmos-Datenbank.

| |  |
|---|---|
|**Erstellen Sie ein Azure-Cosmos-DB-Konto**||
|[Erstellen Sie ein Konto für die DocumentDB-API](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt ein einzelnes Konto für Azure-Cosmos-Datenbank mit der DocumentDB-API verwenden. |
|**Skalieren von Azure-Cosmos-DB**||
|[Azure-Cosmos-DB-Konto in mehreren Regionen zu replizieren und Prioritäten für Failover konfigurieren](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Global-Kontodaten in mehrere Bereiche mit einer angegebenen Failoverpriorität repliziert werden.|
|**Sichern von Azure-Cosmos-DB**||
| [Kontoschlüssel abrufen](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ruft das primäre und sekundäre master Schreib-Schlüssel und die primäre und sekundäre nur-Lese Schlüssel für das Konto.|
| [Abrufen von MongoDB-Verbindungszeichenfolge](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Ruft die Verbindungszeichenfolge für die MongoDB-app mit Ihrem Azure-Cosmos-DB-Konto verbinden.|
|[Account-Schlüssel generieren](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Generiert einen neuen master oder nur-Lese Schlüssel für das Konto.|
|[Erstellen Sie eine firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine eingehende IP-Zugriffssteuerungsrichtlinien um beschränken Sie den Zugriff auf das Konto über eine genehmigte Gruppe von Computern und/oder cloud-Dienste.|
|**Hohe Verfügbarkeit, notfallwiederherstellung, Sicherung und Wiederherstellung**||
|[Konfigurieren der Failoverrichtlinie](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Legt die Failoverpriorität des jede, die Region, in der das Konto repliziert wird.|
|||
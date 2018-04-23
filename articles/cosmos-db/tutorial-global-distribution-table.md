---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der Tabellen-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der Tabellen-API einrichten.
services: cosmos-db
keywords: globale Verteilung, Tabelle
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: f877baa33d94dad07250da9a10209555dbca65c9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Tabellen-API

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit der [Tabellen-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der Tabellen-API

Um von der [globalen Verteilung](distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Legen Sie dazu die Eigenschaft [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations) fest. Das Azure Cosmos DB Table API SDK wählt den optimalen Endpunkt für die Kommunikation auf der Grundlage der Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Präferenzliste aus.

„PreferredLocations“ sollte eine durch Trennzeichen getrennte Liste der bevorzugten Standorte (Multihosting) für Lesevorgänge enthalten. Jede Clientinstanz kann eine Teilmenge dieser Regionen in der bevorzugten Reihenfolge für Lesevorgänge mit geringer Latenz angeben. Die Regionen müssen mit ihren [Anzeigenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx) benannt sein, z.B. `West US`.

Alle Lesevorgänge werden an die erste verfügbare Region in der Liste „PreferredLocations“ gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region auf der Liste durch, usw.

Die SDKs versuchen, in den in „PreferredLocations“ angegebenen Regionen Lesevorgänge auszuführen. Wenn ein Datenbankkonto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für PreferredLocations angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Das SDK sendet automatisch alle Schreibvorgänge an die aktuelle Schreibregion.

Wenn die PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mithilfe der Table-APIs von Azure Cosmos DB


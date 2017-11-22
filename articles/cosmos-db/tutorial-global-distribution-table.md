---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der Tabellen-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der Tabellen-API einrichten.
services: cosmos-db
keywords: globale Verteilung, Tabelle
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 93e75429d66a30bfc4588a3070e32d58eec0df4b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Tabellen-API

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit der [Tabellen-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der Tabellen-API

Um von der [globalen Verteilung](distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Hierzu kann der Konfigurationswert `TablePreferredLocations` in der Datei „app.config“ für das Azure Cosmos DB Table API SDK festgelegt werden. Das Azure Cosmos DB Table API SDK wählt den optimalen Endpunkt für die Kommunikation auf der Grundlage der Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Präferenzliste aus.

`TablePreferredLocations` sollte eine durch Trennzeichen getrennte Liste der bevorzugten Standorte (Multihosting) für Lesevorgänge enthalten. Jede Clientinstanz kann eine Teilmenge dieser Regionen in der bevorzugten Reihenfolge für Lesevorgänge mit geringer Latenz angeben. Die Regionen müssen mit ihren [Anzeigenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx) benannt sein, z.B. `West US`.

Alle Lesevorgänge werden an die erste verfügbare Region in der Liste `TablePreferredLocations` gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region auf der Liste durch, usw.

Das SDK versucht, aus den in `TablePreferredLocations` angegebenen Regionen zu lesen. Wenn z.B. ein Datenbankkonto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für `TablePreferredLocations` angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Das SDK sendet automatisch alle Schreibvorgänge an die aktuell für solche Vorgänge ausgewählte Region.

Wenn die Eigenschaft `TablePreferredLocations` nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mithilfe der Table-APIs von Azure Cosmos DB


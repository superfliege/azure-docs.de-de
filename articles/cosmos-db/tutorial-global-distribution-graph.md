---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der Graph-API | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der Graph-API einrichten.
services: cosmos-db
keywords: globale Verteilung, Graph, Gremlin
author: luisbosquez
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 61004a735f731cfd1303cf40b6e60cba496e942a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763679"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Graph-API

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal die globale Verteilung von Azure Cosmos DB einrichten und dann mithilfe der Graph-API eine Verbindung herstellen.

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit den [Graph-APIs](graph-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der Graph-API über das .NET SDK

Die Graph-API wird als Erweiterungsbibliothek zusätzlich zur SQL-API verfügbar gemacht.

Um von der [globalen Verteilung](distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Dies lässt sich durch Einrichten einer Verbindungsrichtlinie erreichen. Basierend auf der Azure Cosmos DB-Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Liste der bevorzugten Regionen wählt das SDK den optimalen Endpunkt für Schreib- und Lesevorgänge aus.

Diese Liste wird beim Initialisieren einer Verbindung mithilfe der SDKs angegeben. Die SDKs akzeptieren einen optionalen Parameter „PreferredLocations“, bei dem es sich um eine sortierte Liste von Azure-Regionen handelt.

* **Schreibvorgänge**: Das SDK sendet automatisch alle Schreibvorgänge an die aktuelle Schreibregion.
* **Lesevorgänge**: Alle Lesevorgänge werden an die erste verfügbare Region in der Liste „PreferredLocations“ gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region in der Liste durch, und so weiter. Die SDKs versuchen nur, in den in „PreferredLocations“ angegebenen Regionen Lesevorgänge auszuführen. Wenn z. B. das Cosmos DB-Konto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für „PreferredLocations“ angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Die Anwendung kann die vom SDK ausgewählten aktuellen Schreib- und Leseendpunkte anhand von zwei Eigenschaften überprüfen: WriteEndpoint und ReadEndpoint. Diese stehen im SDK der Version 1.8 und höher zur Verfügung. Wenn die PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

### <a name="using-the-sdk"></a>Verwenden des SDK

Beispiel: Im .NET SDK weist der Parameter `ConnectionPolicy` für den Konstruktor `DocumentClient` eine Eigenschaft mit dem Namen `PreferredLocations` auf. Diese Eigenschaft kann auf eine Liste von Regionsnamen festgelegt werden. Die Anzeigenamen für [Azure-Regionen][regions] können als Teil von `PreferredLocations` angegeben werden.

> [!NOTE]
> Die URLs für die Endpunkte sollten nicht als langfristige Konstanten betrachtet werden. Sie können jederzeit vom Dienst aktualisiert werden. Das SDK verarbeitet diese Änderung automatisch.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit den SQL-APIs

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie mit dem lokalen Azure Cosmos DB-Emulator lokal entwickeln können.

> [!div class="nextstepaction"]
> [Lokales Entwickeln mit dem Emulator](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/


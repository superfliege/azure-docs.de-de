---
title: Verwenden des Azure Cosmos DB-Multimasterfeatures mit Open Source-NoSQL-Datenbanken
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963899"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Verwenden des Azure Cosmos DB-Multimasterfeatures mit Open Source-NoSQL-Datenbanken

Die Unterstützung für Azure Cosmos DB-Multimaster ist eine native serverseitige Implementierung, die für alle von Cosmos DB unterstützten Open Source-NoSQL-Angebote verfügbar ist. Alle von Cosmos DB unterstützten SDKs können darauf zugreifen.

Azure Cosmos DB ist weltweit der erste Dienst, der Multimasterunterstützung für diese Open Source-NoSQL-Datenbanken anbietet.

|Modell  |Support  |
|---------|---------|
|MongoDB  | Aktiv/Aktiv  |
|Graph  | Aktiv/Aktiv |
|Cassandra  | Aktiv/Aktiv   |

## <a name="use-mongodb-clients-with-multi-master"></a>Verwendung von MongoDB-Clients mit Multimaster

Das Multimasterfeature wird für MongoDB-API-Konten auf die gleiche Weise aktiviert wie für andere Azure Cosmos DB-APIs. Nach der Aktivierung des Multimasterfeatures für MongoDB-API-Konten kann jede Instanz einer Clientanwendung ihre bevorzugte Region für Lese- und Schreibvorgänge auswählen. Aus Perspektive des MongoDB-Treibers wird die bevorzugte Region dem Client als primäre Replikatgruppe angezeigt. Auf diese Weise kann jede Region Ihrer verteilten Datenbank als primäre Replikatgruppe agieren. Mit Azure Cosmos DB-Multimaster können Sie die Schreiblatenzen für Ihre global verteilten MongoDB-Anwendungen erheblich reduzieren. 

### <a name="set-the-primary-region"></a>Festlegen der primären Region

Jede Instanz eines MongoDB-Clients kann die primäre Region auswählen, indem `@<preferred_primary_region_name>` an das Feld „application name“ angehängt wird, das vom MongoDB-Clienttreiber akzeptiert wird. Die meisten Treiber akzeptieren dies in der Verbindungszeichenfolge, wie in folgendem Beispiel gezeigt:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

Nach dem Herstellen der Verbindung kann einer der folgenden Fälle eintreten:

* Wenn die bevorzugte Region als Schreibregion verfügbar ist, wählt Azure Cosmos DB diese als primäre Region aus.

* Wenn keine bevorzugte Region angegeben ist, wählt Azure Cosmos DB eine Standardregion als primäre Region aus.

* Wenn eine bevorzugte Region angegeben, aber für das Datenbankkonto nicht als Schreibregion verfügbar ist, wählt Azure Cosmos DB die nächstgelegene verfügbare Schreibregion als primäre Region aus.

Bestimmte Treiber wie z.B. der NodeJS-Treiber geben Schreibvorgänge immer zuerst an den Host aus, der in der ursprünglichen Verbindungszeichenfolge angegeben ist. Um bei solchen Treibern sicherzustellen, dass Schreibvorgänge an die bevorzugte weitergeleitet werden, sollten Sie zusätzlich zur Angabe des App-Namens den DNS-Namen in der Verbindungszeichenfolge ändern und den Regionsnamen darin einschließen. Stellen Sie sicher, dass Sie den Regionsnamen ohne Leerzeichen angeben. Beispiel: 

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Konfliktlösungsmodus

Der Konfliktlösungsmodus für Azure Cosmos DB-MongoDB-API-Konten lautet immer „Letzter Schreibvorgang gewinnt“ (Last Write Wins). Dabei wird der Zeitstempel des Regionsservers verwendet, der den Schreibvorgang akzeptiert hat.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie die Multimasterunterstützung für Azure Cosmos DB-MongoDB-API-Konten funktioniert. Sehen Sie sich als Nächstes folgende Ressourcen an:

* [Aktivieren von Multimaster für Azure Cosmos DB-Konten](enable-multi-master.md)

* [Grundlegendes zur Konfliktauflösung in Azure Cosmos DB](multi-master-conflict-resolution.md)

---
title: Konsistenzebenen und Azure Cosmos DB-APIs | Microsoft-Dokumentation
description: Grundlegendes zu den Konsistenzebenen von APIs in Azure Cosmos DB.
keywords: Konsistenz, Azure Cosmos DB, Azure, Modelle, MongoDB, Cassandra, Diagramm, Tabelle, Microsoft Azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 974531cd5907e4f69e7d064125d3e51fa4974949
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50956382"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Konsistenzebenen und Azure Cosmos DB-APIs

Die fünf von Azure Cosmos DB bereitgestellten Konsistenzmodelle werden nativ von der Cosmos DB-SQL-API unterstützt, der Standard-API bei Verwendung von Cosmos DB. Zusätzlich zur SQL-API bietet Cosmos DB auch native Unterstützung für Wire Protocol-kompatible APIs für gängige Datenbanken wie MongoDB, Apache Cassandra, Gremlin und Azure Tables. Diese Datenbanken bieten weder genau definierte Konsistenzmodelle noch von der SLA unterstützte Garantien für die Konsistenzstufen. Diese Datenbanken bieten in der Regel nur eine Teilmenge der fünf von Cosmos DB angebotenen Konsistenzmodelle. Für die SQL-, Gremlin und Tabellen-API wird die im Cosmos-Konto konfigurierte Standardkonsistenzebene verwendet.

Die folgenden Abschnitte zeigen das Mapping zwischen der von einem OSS-Clienttreiber angeforderten Datenkonsistenz für Apache Cassandra 4.x und MongoDB 3.4 bei Verwendung der Cassandra-API bzw. MongoDB-API und die entsprechenden Cosmos DB-Konsistenzebenen.

## <a id="cassandra-mapping"></a>Mapping zwischen Apache Cassandra und Cosmos DB-Konsistenzebenen

Die folgende Tabelle zeigt das Mapping für die Lesekonsistenz zwischen dem Apache Cassandra 4.x-Client und der Cosmos DB-Standardkonsistenzebene für Bereitstellungen in mehreren Regionen sowie in einer einzelnen Region.

| **Apache Cassandra 4.x** | **Cosmos DB (mehrere Regionen)** | **Cosmos DB (einzelne Region)** |
| - | - | - |
| ONE, TWO, THREE | Präfixkonsistenz | Präfixkonsistenz |
| LOCAL_ONE | Präfixkonsistenz | Präfixkonsistenz |
| QUORUM, ALL, SERIAL | Begrenzte Veraltung (Standard) oder stark (in der privaten Vorschau) | STARK (Strong) |
| LOCAL_QUORUM | Begrenzte Veraltung (Bounded staleness) | STARK (Strong) |
| LOCAL_SERIAL | Begrenzte Veraltung (Bounded staleness) | STARK (Strong) |

## <a id="mongo-mapping"></a>Zuordnung zwischen MongoDB 3.4- und Cosmos DB-Konsistenzebenen

Die folgende Tabelle zeigt das Mapping für das Leseinteresse zwischen dem MongoDB 3.4-Client und der Cosmos DB-Standardkonsistenzebene für Bereitstellungen in mehreren Regionen sowie in einer einzelnen Region.

| **MongoDB 3.4** | **Cosmos DB (mehrere Regionen)** | **Cosmos DB (einzelne Region)** |
| - | - | - |
| Linearisierbar | STARK (Strong) | STARK (Strong) |
| Mehrheit | Begrenzte Veraltung (Bounded staleness) | STARK (Strong) |
| Lokal | Präfixkonsistenz | Präfixkonsistenz |

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie in den folgenden Artikeln mehr über die Konsistenzebenen und die Kompatibilität zwischen Cosmos DB-APIs und Open-Source-APIs:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Von der MongoDB-API von Cosmos DB unterstützte MongoDB-Features](mongodb-feature-support.md)
* [Von der Cassandra-API von Cosmos DB unterstützte Apache Cassandra-Features](cassandra-support.md)
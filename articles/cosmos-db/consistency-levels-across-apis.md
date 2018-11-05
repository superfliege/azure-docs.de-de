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
ms.openlocfilehash: ed08b90b9e216ee8713bfe445e98144bf2ba02d4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243995"
---
# <a name="consistency-levels-and-cosmos-db-apis"></a>Konsistenzebenen und Cosmos DB-APIs

Die fünf Konsistenzmodelle werden nativ von der SQL-API unterstützt, die bei Verwendung von Cosmos DB die Standard-API ist. Zusätzlich zur SQL-API bietet Cosmos DB auch native Unterstützung für Wire Protocol-kompatible APIs für gängige Datenbanken wie MongoDB, Apache Cassandra, Gremlin und Azure Tables. Diese Datenbanken bieten weder genau definierte Konsistenzmodelle noch die SLA-gestützten Garantien für die Konsistenzebenen und stellen in der Regel nur eine Teilmenge der fünf Konsistenzmodelle dar, die Cosmos DB bietet. Für die SQL-API, die Gremlin-API und die Tabellen-API wird die im Cosmos-Konto konfigurierte Standardkonsistenzebene verwendet.

Die folgende Tabelle zeigt die Zuordnung zwischen der von einem OSS-Clienttreiber angeforderten Datenkonsistenz für Apache Cassandra 4.x und MongoDB 3.4 bei Verwendung der Cassandra-API bzw. MongoDB-API und den entsprechenden Cosmos DB-Konsistenzebenen.

## <a id="cassandra-mapping"></a>Zuordnen von Apache Cassandra- und Cosmos DB-Konsistenzebenen

Die folgende Tabelle zeigt die Zuordnung für die Lesekonsistenz zwischen dem Apache Cassandra 4.x-Client und der Cosmos DB-Konsistenzebene „Standard“ für eine Bereitstellung sowohl in mehreren Regionen wie auch in einer einzelnen Region.

| **Apache Cassandra 4.x** | **Cosmos DB (mehrere Regionen)** | **Cosmos DB (einzelne Region)** |
| - | - | - |
| ONE, TWO, THREE | Präfixkonsistenz | Präfixkonsistenz |
| LOCAL_ONE | Präfixkonsistenz | Präfixkonsistenz |
| QUORUM, ALL, SERIAL | Begrenzte Veraltung (Standard) oder stark (in der privaten Vorschau) | STARK (Strong) |
| LOCAL_QUORUM | Begrenzte Veraltung (Bounded staleness) | STARK (Strong) |
| LOCAL_SERIAL | Begrenzte Veraltung (Bounded staleness) | STARK (Strong) |

## <a id="mongo-mapping"></a>Zuordnung zwischen MongoDB 3.4- und Cosmos DB-Konsistenzebenen

Die folgende Tabelle zeigt die Zuordnung für die „Lesebestätigungen“ der MongoDB 3.4- und der Cosmos DB-Konsistenzebene „Standard“ für eine Bereitstellung sowohl in mehreren Regionen wie auch in einer einzelnen Region.

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
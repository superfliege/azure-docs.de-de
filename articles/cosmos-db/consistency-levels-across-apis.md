---
title: Konsistenzebenen und Azure Cosmos DB-APIs
description: Grundlegendes zu den Konsistenzebenen von APIs in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: 76ebbc8cc8dbea4b7f8f8226cf1d8570a421e8cf
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034334"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Konsistenzebenen und Azure Cosmos DB-APIs

Die fünf in Azure Cosmos DB bereitgestellten Konsistenzmodelle werden nativ von der Azure Cosmos DB-SQL-API unterstützt. Bei Verwendung von Azure Cosmos DB ist die SQL-API die Standard-API. 

Azure Cosmos DB bietet auch native Unterstützung für Wire Protocol-kompatible APIs für gängige Datenbanken. Dazu zählen MongoDB, Apache Cassandra, Gremlin und Azure Table Storage. Diese Datenbanken bieten keine genau definierten Konsistenzmodelle und keine von der SLA unterstützten Garantien für Konsistenzebenen. Sie umfassen in der Regel nur eine Teilmenge der fünf in Azure Cosmos DB bereitgestellten Konsistenzmodelle. Für die SQL-API, die Gremlin-API und die Tabellen-API wird die im Azure Cosmos DB-Konto konfigurierte Standardkonsistenzebene verwendet. 

In den folgenden Abschnitten wird die Zuordnung der von einem OSS-Clienttreiber angeforderten Datenkonsistenz für Apache Cassandra 4.x und MongoDB 3.4 gezeigt. Zudem sind die entsprechenden Azure Cosmos DB-Konsistenzebenen für Apache Cassandra und MongoDB aufgeführt.

## <a id="cassandra-mapping"></a>Zuordnung zwischen Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen

In dieser Tabelle ist die Zuordnung für die Lesekonsistenz zwischen dem Apache Cassandra 4.x-Client und der Standardkonsistenzebene in Azure Cosmos DB aufgeführt. Es sind Bereitstellungen in mehreren Regionen sowie in einer einzelnen Region aufgeführt.

| **Apache Cassandra 4.x** | **Azure Cosmos DB (mehrere Regionen)** | **Azure Cosmos DB (einzelne Region)** |
| - | - | - |
| ONE, TWO, THREE | Konsistentes Präfix | Konsistentes Präfix |
| LOCAL_ONE | Konsistentes Präfix | Konsistentes Präfix |
| QUORUM, ALL, SERIAL | Begrenzte Veraltung (Bounded staleness) ist die Standardeinstellung. Stark (Strong) befindet sich in der privaten Vorschau. | STARK (Strong) |
| LOCAL_QUORUM | Bounded staleness | STARK (Strong) |
| LOCAL_SERIAL | Bounded staleness | STARK (Strong) |

## <a id="mongo-mapping"></a>Zuordnung zwischen MongoDB 3.4- und Azure Cosmos DB-Konsistenzebenen

In der folgenden Tabelle ist die Zuordnung für das Leseinteresse zwischen dem MongoDB 3.4-Client und der Standardkonsistenzebene in Azure Cosmos DB aufgeführt. Es sind Bereitstellungen in mehreren Regionen sowie in einer einzelnen Region aufgeführt.

| **MongoDB 3.4** | **Azure Cosmos DB (mehrere Regionen)** | **Azure Cosmos DB (einzelne Region)** |
| - | - | - |
| Linearisierbar | STARK (Strong) | STARK (Strong) |
| Mehrheit | Bounded staleness | STARK (Strong) |
| Lokal | Konsistentes Präfix | Konsistentes Präfix |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konsistenzebenen und die Kompatibilität zwischen Azure Cosmos DB-APIs und Open-Source-APIs. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [MongoDB-Features, die von der API für MongoDB von Azure Cosmos DB unterstützt werden](mongodb-feature-support.md)
* [Apache Cassandra-Features, die von der Cassandra-API für Azure Cosmos DB unterstützt werden](cassandra-support.md)
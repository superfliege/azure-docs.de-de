---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen.
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: dd62e0f4ff110ec8454031f1b66b56025328c33c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101478"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitionierung und horizontale Skalierung in Azure Cosmos DB

In diesem Artikel werden physische und logische Partitionen in Azure Cosmos DB und die bewährten Methoden für die Skalierung und Partitionierung erläutert. 

## <a name="logical-partitions"></a>Logische Partitionen

Eine logische Partition besteht aus einem Satz von Elementen mit demselben Partitionsschlüssel. Wenn Sie beispielsweise einen Container haben, in dem alle Elemente eine `City`-Eigenschaft aufweisen, können Sie `City` als Partitionsschlüssel für den Container verwenden. Gruppen von Elementen mit bestimmten Werten für `City`, z.B. „London“, „Paris“, „NYC“ usw., bilden eine eindeutige logische Partition.

In Azure Cosmos DB ist ein Container die grundlegende Einheit für die Skalierbarkeit. Die Daten, die dem Container hinzugefügt werden, und der Durchsatz, den Sie für den Container bereitstellen, werden automatisch (horizontal) über einen Satz von logischen Partitionen partitioniert. Sie werden basierend auf dem Partitionsschlüssel, den Sie für den Cosmos-Container angeben, partitioniert. Weitere Informationen finden Sie im Artikel [Angeben des Partitionsschlüssels für Ihre Cosmos-Container](how-to-create-container.md).

Eine logische Partition definiert den Bereich für Datenbanktransaktionen. Sie können Elemente in einer logischen Partition mithilfe einer Transaktion mit Momentaufnahmeisolation aktualisieren. Wenn dem Container neue Elemente hinzugefügt werden, werden neue logische Partitionen transparent vom System erstellt.

## <a name="physical-partitions"></a>Physische Partitionen

Ein Azure Cosmos-Container wird skaliert, indem die Daten und der Durchsatz auf eine große Anzahl von logischen Partitionen verteilt werden. Intern werden eine oder mehrere logische Partitionen einer **physischen Partition** zugeordnet, die aus einem auch als Replikatgruppe bezeichneten Satz von Replikaten besteht. Jede Replikatgruppe hostet eine Instanz der Azure Cosmos-Datenbank-Engine. Eine Replikatgruppe macht die in der physischen Partition gespeicherten Daten dauerhaft, hochverfügbar und konsistent. Eine physische Partition unterstützt eine feste, maximale Menge an Speicher und RUs. Jedes Replikat der physischen Partition erbt das Speicherkontingent. Und alle Replikate einer physischen Partition unterstützen den Durchsatz, der der physischen Partition zugeordnet wurde. Die folgende Abbildung zeigt, wie logische Partitionen physischen Partitionen zugeordnet werden, die global verteilt sind:

![Partitionierung in Azure Cosmos DB](./media/partition-data/logical-partitions.png)

Der für einen Container bereitgestellte Durchsatz wird gleichmäßig auf die physischen Partitionen aufgeteilt. Aus diesem Grund kann ein Partitionsschlüsselentwurf, der die Durchsatzanforderungen nicht gleichmäßig verteilt, „heiße“ Partitionen verursachen. Heiße Partitionen können zur einer Ratenreduzierung und einer ineffizienten Nutzung des bereitgestellten Durchsatzes führen.

Im Gegensatz zu logischen Partitionen sind physische Partitionen eine interne Implementierung des Systems. Sie können weder ihre Größe, noch die Platzierung, noch die Anzahl oder die Zuordnung zwischen den logischen Partitionen und den physischen Partitionen steuern. Sie können allerdings die Anzahl der logischen Partitionen und die Verteilung der Daten und des Durchsatzes steuern, indem Sie den richtigen Partitionsschlüssel auswählen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde eine Übersicht über die Datenpartitionierung und die bewährten Methoden für die Skalierung und Partitionierung in Azure Cosmos DB bereitgestellt. 

* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).
* Erfahren Sie etwas über das [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz in einem Cosmos-Container](how-to-provision-container-throughput.md).
* Informieren Sie sich über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

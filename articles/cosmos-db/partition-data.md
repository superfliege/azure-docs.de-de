---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: 5dd1926496351f5bbfe8e5b3e4d1e0b68e82d272
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283392"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitionierung und horizontale Skalierung in Azure Cosmos DB

In diesem Artikel werden physische und logische Partitionen in Azure Cosmos DB und die bewährten Methoden für die Skalierung und Partitionierung erläutert. 

## <a name="logical-partitions"></a>Logische Partitionen

Eine logische Partition besteht aus einem Satz von Elementen mit demselben Partitionsschlüssel. Wenn Sie beispielsweise einen Container haben, in dem alle Elemente eine `City`-Eigenschaft aufweisen, können Sie `City` als Partitionsschlüssel für den Container verwenden. Gruppen von Elementen mit bestimmten Werten für `City`, z.B. „London“, „Paris“, „NYC“ usw., bilden eine eindeutige logische Partition.

In Azure Cosmos DB ist ein Container die grundlegende Einheit für die Skalierbarkeit. Die Daten, die dem Container hinzugefügt werden, und der Durchsatz, den Sie für den Container bereitstellen, werden automatisch (horizontal) über einen Satz von logischen Partitionen partitioniert. Sie werden basierend auf dem Partitionsschlüssel, den Sie für den Cosmos-Container angeben, partitioniert. Weitere Informationen finden Sie im Artikel [Angeben des Partitionsschlüssels für Ihre Cosmos-Container](how-to-create-container.md).

Eine logische Partition definiert den Bereich für Datenbanktransaktionen. Sie können Elemente in einer logischen Partition mithilfe einer Transaktion mit Momentaufnahmeisolation aktualisieren.

Wenn dem Container neue Elemente hinzugefügt werden oder der für den Container bereitgestellte Durchsatz erhöht wird, werden vom System transparent neue logische Partitionen erstellt.

## <a name="physical-partitions"></a>Physische Partitionen

Ein Cosmos-Container wird skaliert, indem die Daten und der Durchsatz auf eine große Anzahl von logischen Partitionen verteilt werden. Intern werden eine oder mehrere logische Partitionen einer **Ressourcenpartition** zugeordnet, die aus einem auch als Replikatgruppe bezeichneten Satz von Replikaten besteht. Jede Replikatgruppe hostet eine Instanz der Cosmos-Datenbank-Engine. Eine Replikatgruppe macht die in der Ressourcenpartition gespeicherten Daten dauerhaft, hochverfügbar und konsistent. Eine Ressourcenpartition unterstützt eine feste, maximale Menge an Speicher und RUs. Jedes Replikat der Ressourcenpartition erbt das Speicherkontingent. Und alle Replikate einer Ressourcenpartition unterstützen den Durchsatz, der der Ressourcenpartition zugeordnet wurde. Die folgende Abbildung zeigt, wie logische Partitionen physischen Partitionen zugeordnet werden, die global verteilt sind:

![Partitionierung in Azure Cosmos DB](./media/partition-data/logical-partitions.png)

Der für einen Container bereitgestellte Durchsatz wird gleichmäßig auf die physischen Partitionen aufgeteilt. Aus diesem Grund kann ein Partitionsschlüsselentwurf, der die Durchsatzanforderungen nicht gleichmäßig verteilt, „heiße“ Partitionen verursachen. Heiße Partitionen können zur einer Ratenreduzierung und einer ineffizienten Nutzung des bereitgestellten Durchsatzes führen.

Im Gegensatz zu logischen Partitionen sind physische Partitionen eine interne Implementierung des Systems. Sie können weder ihre Größe, noch die Platzierung, noch die Anzahl oder die Zuordnung zwischen den logischen Partitionen und den physischen Partitionen steuern. Sie können allerdings die Anzahl der logischen Partitionen und die Verteilung der Daten und des Durchsatzes steuern, indem Sie den richtigen Partitionsschlüssel auswählen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde eine Übersicht über die Datenpartitionierung und die bewährten Methoden für die Skalierung und Partitionierung in Azure Cosmos DB bereitgestellt. 

* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).
* Erfahren Sie etwas über das [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz in einem Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz in einer Cosmos-Datenbank](how-to-provision-database-throughput.md).

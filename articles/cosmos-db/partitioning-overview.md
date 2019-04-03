---
title: Partitionierung in Azure Cosmos DB
description: Übersicht über die Partitionierung in Azure Cosmos DB
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: a8db510bea57fa3d6ee873571e586bcef7508b26
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961634"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Partitionierung in Azure Cosmos DB

Azure Cosmos DB arbeitet mit Partitionierung, um einzelne Container in einer Datenbank entsprechend den Leistungsanforderungen Ihrer Anwendung zu skalieren. Mithilfe der Partitionierung werden die Elemente in einem Container in Teilgruppen unterteilt, die als *logische Partitionen* bezeichnet werden. Logische Partitionen werden basierend auf dem Wert des *Partitionsschlüssels* erstellt, die jedem Element in einem Container zugeordnet ist. Alle Elemente in einer logischen Partition haben denselben Partitionsschlüsselwert.

Angenommen, ein Container enthält Dokumente. Jedes Dokument hat für die `UserID`-Eigenschaft einen eindeutigen Wert. Wenn `UserID` als Partitionsschlüssel für die Elemente in einem Container dient und es 1.000 eindeutige `UserID`-Werte gibt, werden für den Container 1.000 logische Partitionen erstellt.

Zusätzlich zu einem Partitionsschlüssel, der die logische Partition des Elements bestimmt, hat jedes Element in einem Container eine (innerhalb der logischen Partition eindeutige) *Element-ID*. Die Kombination des Partitionsschlüssels und der Element-ID erzeugt den *Index* des Elements, der das Element eindeutig identifiziert.

Die [Wahl eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey) ist eine wichtige Entscheidung, die Auswirkungen auf die Leistung Ihrer Anwendung hat.

## <a name="managing-logical-partitions"></a>Verwalten logischer Partitionen

Azure Cosmos DB verwaltet die Platzierung logischer Partitionen in physischen Partitionen (Ihrer Serverinfrastruktur) transparent und automatisch, um die Anforderungen an Skalierbarkeit und Leistung des Containers effizient zu erfüllen. Wenn sich die Anforderungen einer Anwendungsinstanz an Durchsatz und Speicher erhöhen, verschiebt Azure Cosmos DB logische Partitionen, um die Last automatisch auf eine größere Anzahl von Servern zu verteilen. 

Azure Cosmos DB verwendet eine hashbasierte Partitionierung, um logische Partitionen auf physische Partitionen zu verteilen. Azure Cosmos DB erstellt für den Partitionsschlüsselwert eines Elements einen Hashwert. Das Hashergebnis bestimmt die physische Partition. Anschließendverteilt Azure Cosmos DB den Schlüsselbereich der Partitionsschlüsselhashes gleichmäßig auf die physischen Partitionen.

Abfragen, die auf Daten in einer einzelnen Partition zugreifen, sind kostengünstiger als Abfragen mit Zugriff auf mehrere Partitionen. Transaktionen (in gespeicherten Prozeduren oder Triggern) sind nur für Elemente in einer einzelnen logischen Partition zulässig.

Informationen dazu, wie Azure Cosmos DB Partitionen verwaltet, finden Sie unter [Logische Partitionen](partition-data.md). (Für die Erstellung oder Ausführung Ihrer Anwendungen ist es nicht notwendig, diese Details zu verstehen.)

## <a id="choose-partitionkey"></a>Auswählen eines Partitionsschlüssels

Beachten Sie bei der Wahl eines Partitionsschlüssel Folgendes:

* Eine einzelne logische Partition darf maximal 10 GB Speicherplatz haben.  

* Partitionierte Container haben einen Mindestdurchsatz von 400 Anforderungseinheiten pro Sekunde (Request units per second, RU/s). Anforderungen an denselben Partitionsschlüssel dürfen den einer Partition zugeordneten Durchsatz nicht überschreiten. Wenn Anforderungen den zugeordneten Durchsatz überschreiten, gilt für die Anforderungen eine Ratenbegrenzung. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu partiellen starken Auslastungen innerhalb Ihrer Anwendung führt.

* Wählen Sie einen Partitionsschlüssel, mit dem die Workload gleichmäßig auf alle Partitionen und über die Zeit verteilt wird. Ihre Auswahl des Partitionsschlüssels sollte die Anforderungen an effiziente Partitionsabfragen und -transaktionen erfüllen und gleichzeitig die Elemente auf mehrere Partitionen verteilen, um Skalierbarkeit zu erzielen.

* Wählen Sie einen Partitionsschlüssel aus, der über eine Vielzahl von Werten und Zugriffsmustern verfügt, die gleichmäßig auf logische Partitionen verteilt sind. Dies hilft, die Daten und Aktivitäten in Ihrem Container auf eine Gruppe von logischen Partitionen zu verteilen, damit die Ressourcen für die Datenspeicherung und den Durchsatz auf die logischen Partitionen verteilt werden können.

* Mögliche Partitionsschlüssel stellen Eigenschaften dar, die in Ihren Abfragen häufig als Filter verwendet werden. Abfragen können effizient weitergeleitet werden, indem der Partitionsschlüssel in das Filterprädikat eingeschlossen wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [Partitionen](partition-data.md).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).

---
title: Partitionierung in Azure Cosmos DB
description: Übersicht über die Partitionierung in Azure Cosmos DB
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: cc4b165b4fa04e22c9c57547df1657a0c6fc4537
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263375"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Partitionierung in Azure Cosmos DB

Die Partitionierung ist das Verfahren, mit dem Cosmos DB einzelne Container in einer Datenbank entsprechend den Leistungsanforderungen Ihrer Anwendung skaliert. Mithilfe der Partitionierung werden die Elemente in einem Container in verschiedene Teilmengen unterteilt, die als logische Partitionen bezeichnet werden. Die logischen Partitionen werden basierend auf dem Wert einer Eigenschaft des Partitionsschlüssels erstellt, die jedem Element zugeordnet ist.

Eine logische Partition ist eine eindeutige Untermenge von Elementen in einem Container. Die Elemente in einer logischen Partition werden durch den Partitionsschlüsselwert identifiziert, der von allen Elementen in der logischen Partition gemeinsam verwendet wird.  In einem Container mit Dokumenten verfügt beispielsweise jedes Dokument über eine `UserID`-Eigenschaft.  Wenn `UserID` als Partitionsschlüssel für die Elemente in einem Container dient und es 1.000 eindeutige `UserID`-Werte gibt, werden für den Container 1.000 logische Partitionen erstellt.

Jedes Element in einem Container verfügt über einen **Partitionsschlüssel**, der die **logische Partition** des Elements bestimmt. Außerdem hat jedes Element eine **Element-ID** (die innerhalb der logischen Partition eindeutig ist).  Der **Index** eines Elements dient dessen eindeutiger Identifikation. Er wird durch die Kombination von Partitionsschlüssel und Element-ID gebildet.

Die Auswahl eines Partitionsschlüssels ist eine wichtige Entscheidung, die Auswirkungen auf die Leistung Ihrer Anwendung hat.  Weitere Informationen und eine ausführliche Anleitung finden Sie im Artikel [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).

## <a name="logical-partition-management"></a>Verwalten logischer Partitionen

Cosmos DB verwaltet die Platzierung der logischen Partitionen auf den physischen Partitionen (Serverinfrastruktur) transparent und automatisch, um die Anforderungen an Skalierbarkeit und Leistung des Containers effizient zu erfüllen. Wenn sich die Anforderungen der Anwendung an Durchsatz und Speicher erhöhen, verschiebt Cosmos DB logische Partitionen, um die Last automatisch auf eine größere Anzahl von Servern zu verteilen. Informationen dazu, wie Cosmos DB die Partitionen verwaltet, finden Sie im Artikel [Logische Partitionen](partition-data.md). Es ist nicht notwendig, diese Einzelheiten zu verstehen, um Ihre Anwendungen zu erstellen oder auszuführen.

Cosmos DB verwendet eine hashbasierte Partitionierung, um logische Partitionen auf physische Partitionen zu verteilen.  Der Partitionsschlüsselwert eines Elements wird von Cosmos DB mit einem Hashwert versehen, und dieses Hashergebnis bestimmt die physische Partition. Cosmos DB verteilt den Schlüsselbereich der Partitionsschlüsselhashes gleichmäßig auf die „N“ physischen Partitionen.

Abfragen, die auf Daten in einer einzelnen Partition zugreifen, sind kostengünstiger als Abfragen mit Zugriff auf mehrere Partitionen. Transaktionen (in gespeicherten Prozeduren oder Triggern) sind nur für Elemente in einer einzelnen logischen Partition zulässig.  

## <a id="choose-partitionkey"></a>Auswählen eines Partitionsschlüssels

Beachten Sie bei der Auswahl eines Partitionsschlüssel Folgendes:

* Eine einzelne logische Partition darf maximal 10 GB Speicher einnehmen.  

* Partitionierte Container werden mit einem minimalen Durchsatz von 400 RU/s konfiguriert. Anforderungen an denselben Partitionsschlüssel dürfen den für eine Partition bereitgestellten Durchsatz nicht überschreiten. Wenn sie den zugeordneten Durchsatz überschreiten, werden die Anforderungen ratenbegrenzt. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu partiellen starken Auslastungen innerhalb Ihrer Anwendung führt.

* Wählen Sie einen Partitionsschlüssel aus, mit dem die Workload gleichmäßig auf alle Partitionen und über die Zeit verteilt wird.  Ihre Auswahl des Partitionsschlüssels sollte die Anforderungen an effiziente Partitionsabfragen und/oder -transaktionen erfüllen und gleichzeitig die Elemente auf mehrere Partitionen verteilen, um Skalierbarkeit zu erzielen.

* Wählen Sie einen Partitionsschlüssel aus, der über eine Vielzahl von Werten und Zugriffsmustern verfügt, die gleichmäßig auf logische Partitionen verteilt sind. Die grundlegende Idee ist, die Daten und Aktivitäten in Ihrem Container auf eine Gruppe von logischen Partitionen zu verteilen, damit die Ressourcen für die Datenspeicherung und den Durchsatz über die logischen Partitionen verteilt werden können.

* Mögliche Partitionsschlüssel stellen die Eigenschaften dar, die in Ihren Abfragen häufig als Filter auftreten. Abfragen können effizient weitergeleitet werden, indem der Partitionsschlüssel in das Filterprädikat eingeschlossen wird.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie etwas über das [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).
* Informieren Sie sich über [Partitionen](partition-data.md).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).

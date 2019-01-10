---
title: Synthetische Partitionsschlüssel in Azure Cosmos DB
description: Erfahren Sie, wie Sie synthetische Partitionsschlüssel in Ihren Azure Cosmos DB-Containern verwenden.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: mjbrown
ms.openlocfilehash: 37d220a13aec99de94afa3357db1462d11f8662c
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043366"
---
# <a name="create-a-synthetic-partition-key"></a>Erstellen eines synthetischen Partitionsschlüssels

Es hat sich bewährt, einen Partitionsschlüssel mit vielen unterschiedlichen Werten (Hunderte oder Tausende) auszuwählen. Ziel ist es, Ihre Daten und die Workload gleichmäßig auf die Elemente zu verteilen, die diesen Partitionsschlüsselwerten zugeordnet sind. Wenn in den Daten keine solche Eigenschaft enthalten ist, kann ein synthetischer Partitionsschlüssel erstellt werden. In den folgenden Abschnitten werden einige grundlegende Verfahren zum Generieren synthetischer Partitionsschlüssel für Ihre Container beschrieben.

## <a name="concatenating-multiple-properties-of-an-item"></a>Verketten mehrerer Eigenschaften eines Elements

Sie können einen Partitionsschlüssel bilden, indem Sie mehrere Eigenschaftswerte zu einer einzigen künstlichen `partitionKey`-Eigenschaft verketten. Diese Schlüssel werden als synthetische Schlüssel bezeichnet. Betrachten Sie etwa das folgende Beispieldokument:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Für das obige Dokument besteht eine Option darin, /deviceId oder /date als Partitionsschlüssel festzulegen, wenn Sie Ihren Container basierend auf der Geräte-ID oder dem Datum partitionieren möchten. Eine andere Möglichkeit ist ein Verketten dieser beiden Werte in einer synthetischen `partitionKey`-Eigenschaft, die als Partitionsschlüssel verwendet wird.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

In realen Szenarien verfügen Sie in einer Datenbank u.U. über Tausende von Dokumenten, daher sollten Sie die synthetischen Schlüssel nicht manuell hinzufügen, sondern clientseitige Logik definieren, um die Werte zu verketten und die synthetischen Schlüssel in die Dokumente einzufügen.

## <a name="using-a-partition-key-with-random-suffix"></a>Verwenden von Partitionsschlüsseln mit zufälligen Suffixen

Noch eine weitere Strategie für ein gleichmäßigeres Verteilen der Workload stellt das Anfügen einer Zufallszahl am Ende des Partitionsschlüsselwerts dar. Wenn Sie die Elemente auf diese Weise verteilen, können Sie Schreibvorgänge über Partitionen hinweg parallel ausführen.

Wenn ein Partitionsschlüssel z.B. ein Datum darstellt, können Sie eine Zufallszahl zwischen 1 und 400 auswählen und als Suffix mit dem Datum verketten. Diese Methode führt zu Partitionsschlüsselwerten wie 2018-08-09.1, 2018-08-09.2 usw. bis 2018-08-09.400. Da Sie den Partitionsschlüssel zufällig zuordnen, werden die Schreibvorgänge im Container an jedem Tag gleichmäßig auf mehrere Partitionen verteilt. Diese Methode führt zu mehr Parallelität und einem insgesamt höheren Durchsatz.

## <a name="using-a-partition-key-with-pre-calculated-suffixes"></a>Verwenden von Partitionsschlüsseln mit vorab berechneten Suffixen 

Auch wenn die Strategie mit Zufallszahlen den Schreibdurchsatz erheblich verbessern kann, ist es schwierig, ein bestimmtes Element zu lesen, da Sie den beim Schreiben des Elements verwendeten Suffixwert nicht kennen. Um das Lesen einzelner Elemente zu vereinfachen, können Sie vorab berechnete Suffixe verwenden. Anstelle einer Zufallszahl verwenden Sie für die Verteilung der Elemente auf die Partitionen eine Zahl, die basierend auf etwas, das Sie abfragen möchten, berechnet wird.

Gehen Sie wieder vom vorherigen Beispiel aus, bei dem in einem Container das Datum im Partitionsschlüssel verwendet wird. Gehen Sie nun davon aus, dass jedes Element über ein VIN-Attribut (Vehicle-Identification-Number) verfügt, auf das zugegriffen werden kann und das Sie häufig zusätzlich zum Datum abfragen, um Elemente zu ermitteln. Bevor Ihre Anwendung das Element in den Container schreibt, kann es basierend auf der VIN ein Hashsuffix berechnen und an das Partitionsschlüsseldatum anfügen. Bei der Berechnung kann eine Zahl zwischen 1 und 400 generiert werden, die gleichmäßig verteilt ist – ähnlich der Strategie mit der Zufallszahl. Der Partitionsschlüsselwert ist dann eine Verkettung aus dem Datum und dem berechneten Ergebnis.

Mit dieser Strategie werden die Schreibvorgänge gleichmäßig auf die Partitionsschlüsselwerte und damit auf die Partitionen verteilt. Sie können ein bestimmtes Element und das Datum ganz einfach lesen, da Sie den Partitionsschlüsselwert für eine bestimmte VIN berechnen können. Diese Methode hat den Vorteil, dass Sie es vermeiden, einen einzelnen aktiven Partitionsschlüssel (Partitionsschlüssel, der die gesamte Workload aufnimmt) zu erstellen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konzept der Partitionierung finden Sie in den folgenden Artikeln:

* Erfahren Sie mehr über [Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für Cosmos-Container und -Datenbanken](set-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz in einem Cosmos-Container](how-to-provision-container-throughput.md).
* Informieren Sie sich über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

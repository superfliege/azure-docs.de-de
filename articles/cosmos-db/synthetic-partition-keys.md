---
title: Erstellen eines synthetischen Partitionsschlüssel in Azure Cosmos DB zum gleichmäßigen Verteilen Ihrer Daten und Workloads
description: Erfahren Sie, wie Sie synthetische Partitionsschlüssel in Ihren Azure Cosmos-Containern verwenden.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: mjbrown
ms.openlocfilehash: 8becfe375f2e887348729cf1d76820fc41156d2a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997099"
---
# <a name="create-a-synthetic-partition-key"></a>Erstellen eines synthetischen Partitionsschlüssels

Es hat sich bewährt, einen Partitionsschlüssel mit vielen unterschiedlichen Werten (Hunderte oder Tausende) auszuwählen. Ziel ist es, Ihre Daten und die Workload gleichmäßig auf die Elemente zu verteilen, die diesen Partitionsschlüsselwerten zugeordnet sind. Wenn in den Daten keine solche Eigenschaft enthalten ist, können Sie einen synthetischen Partitionsschlüssel erstellen. In den folgenden Abschnitten werden einige grundlegende Verfahren zum Generieren synthetischer Partitionsschlüssel für Ihre Container beschrieben.

## <a name="concatenate-multiple-properties-of-an-item"></a>Verketten mehrerer Eigenschaften eines Elements

Sie können einen Partitionsschlüssel bilden, indem Sie mehrere Eigenschaftswerte zu einer einzigen künstlichen `partitionKey`-Eigenschaft verketten. Diese Schlüssel werden als synthetische Schlüssel bezeichnet. Betrachten Sie etwa das folgende Beispieldokument:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Für das vorherige Dokument besteht eine Option darin, „/deviceId“ oder „/date“ als Partitionsschlüssel festzulegen. Verwenden Sie diese Option, wenn Sie Ihren Container entweder anhand der Geräte-ID oder des Datums partitionieren möchten. Eine andere Möglichkeit ist ein Verketten dieser beiden Werte in einer synthetischen `partitionKey`-Eigenschaft, die als Partitionsschlüssel verwendet wird.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

In Echtzeitszenarien können Sie Tausende von Dokumenten in einer Datenbank ablegen. Anstatt den synthetischen Schlüssel manuell hinzuzufügen, definieren Sie die clientseitige Logik zur Verkettung von Werten, und fügen Sie den synthetischen Schlüssel in die Dokumente ein.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Verwenden eines Partitionsschlüssels mit einem zufälligen Suffix

Noch eine weitere Strategie für ein gleichmäßigeres Verteilen der Workload stellt das Anfügen einer Zufallszahl am Ende des Partitionsschlüsselwerts dar. Wenn Sie Elemente auf diese Weise verteilen, können Sie Schreibvorgänge über Partitionen hinweg parallel ausführen.

Wenn ein Partitionsschlüssel z.B. ein Datum darstellt,  können Sie eine Zufallszahl zwischen 1 und 400 auswählen und als Suffix mit dem Datum verketten. Diese Methode führt zu Partitionsschlüsselwerten wie 2018-08-09.1, 2018-08-09.2 usw. bis 2018-08-09.400. Da Sie den Partitionsschlüssel zufällig zuordnen, werden die Schreibvorgänge im Container an jedem Tag gleichmäßig auf mehrere Partitionen verteilt. Diese Methode führt zu mehr Parallelität und einem insgesamt höheren Durchsatz.

## <a name="use-a-partition-key-with-precalculated-suffixes"></a>Verwenden eines Partitionsschlüssels mit vorab berechnetem Suffix 

Die Randomisierungsstrategie kann den Schreibdurchsatz erheblich verbessern, aber es ist schwierig, ein bestimmtes Element zu lesen. Sie kennen den Suffixwert nicht, der beim Schreiben des Elements verwendet wird. Um das Lesen einzelner Elemente zu vereinfachen, verwenden Sie vorab berechnete Suffixe. Anstelle einer Zufallszahl verwenden Sie für die Verteilung der Elemente auf die Partitionen eine Zahl, die basierend auf etwas, das Sie abfragen möchten, berechnet wird.

Gehen Sie wieder vom vorherigen Beispiel aus, bei dem in einem Container das Datum im Partitionsschlüssel verwendet wird. Nehmen wir nun an, dass jedes Element über ein Attribut „VIN“ (Fahrzeug-Identifizierungsnummer) verfügt. Nehmen wir weiter an, dass Sie häufig Abfragen durchführen, um Elemente zusätzlich zum Datum über die Fahrgestellnummer zu finden. Bevor Ihre Anwendung das Element in den Container schreibt, kann es basierend auf der VIN ein Hashsuffix berechnen und an das Partitionsschlüsseldatum anfügen. Die Berechnung kann eine gleichmäßig verteilte Zahl zwischen 1 und 400 erzeugen. Dieses Ergebnis ist vergleichbar mit den Ergebnissen der Zufallsstrategiemethode. Der Partitionsschlüsselwert ist dann eine Verkettung aus dem Datum und dem berechneten Ergebnis.

Mit dieser Strategie werden die Schreibvorgänge gleichmäßig auf die Partitionsschlüsselwerte und damit auf die Partitionen verteilt. Sie können ein bestimmtes Element und das Datum ganz einfach lesen, da Sie den Partitionsschlüsselwert für eine bestimmte VIN berechnen können. Diese Methode hat den Vorteil, dass Sie es vermeiden, einen einzelnen aktiven Partitionsschlüssel zu erstellen. Ein aktiver Partitionsschlüssel ist der Partitionsschlüssel, der die gesamte Workload aufnimmt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konzept der Partitionierung finden Sie in den folgenden Artikeln:

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für Cosmos-Container und -Datenbanken](set-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).

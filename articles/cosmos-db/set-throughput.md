---
title: Bereitstellen des Durchsatzes für Azure Cosmos-Container und -Datenbanken
description: Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Container und -Datenbanken festlegen.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: cbcfaab7dc1b716b3978f1b19870d945e0eabf89
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459077"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Bereitstellen des Durchsatzes für Container und Datenbanken

Eine Azure Cosmos-Datenbank ist eine Verwaltungseinheit für eine Gruppe von Containern. Eine Datenbank besteht aus einem Satz von schemaunabhängigen Containern. Ein Azure Cosmos-Container ist die Skalierungseinheit für Durchsatz und Speicher. Ein Container wird horizontal über eine Gruppe von Computern innerhalb einer Azure-Region partitioniert und auf alle Azure-Regionen, die Ihrem Azure Cosmos-Konto zugeordnet sind, verteilt.

Mit Azure Cosmos DB können Sie den Durchsatz in zwei Größenordnungen konfigurieren: **Azure Cosmos-Container** und **Azure Cosmos-Datenbanken**.

## <a name="setting-throughput-on-a-container"></a>Festlegen des Durchsatzes für einen Container  

Der für einen Azure Cosmos-Container bereitgestellte Durchsatz ist ausschließlich für diesen Container reserviert. Der Container erhält den bereitgestellten Durchsatz durchgängig. Der bereitgestellte Durchsatz für einen Container wird finanziell durch SLAs gesichert. Informationen zum Konfigurieren des Durchsatzes für einen Container finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).

Das Festlegen von bereitgestelltem Durchsatz für einen Container ist eine häufig genutzte Option. Sie können den Durchsatz für einen Container zwar elastisch skalieren, indem Sie einen beliebigen Durchsatz (in RUs) bereitstellen, aber Sie können nicht selektiv den Durchsatz für logische Partitionen angeben. Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der der jeweiligen logischen Partition zugewiesen wurde, werden Ihre Vorgänge ratenbegrenzt. Bei einer Ratenbegrenzung können Sie entweder den Durchsatz für den gesamten Container erhöhen oder den Vorgang wiederholen. Weitere Informationen zur Partitionierung finden Sie unter [Logische Partitionen](partition-data.md).

Es wird empfohlen, den Durchsatz auf Containerebene zu konfigurieren, wenn Sie eine garantierte Leistung für den Container benötigen.

Der für einen Azure Cosmos-Container bereitgestellte Durchsatz wird gleichmäßig auf alle logischen Partitionen des Containers aufgeteilt. Da mindestens eine logische Partition eines Containers auf einer physischen Partition gehostet wird, gehören die physischen Partitionen ausschließlich zu dem Container und unterstützen den Durchsatz für den Container. Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen eines Containers hostet:

![Physische Partition](./media/set-throughput/resource-partition.png)

## <a name="setting-throughput-on-a-database"></a>Festlegen des Durchsatzes für eine Datenbank

Wenn Sie Durchsatz für eine Azure Cosmos-Datenbank bereitstellen, wird der Durchsatz von allen Containern in der Datenbank gemeinsam genutzt, sofern Sie nicht einen bereitgestellten Durchsatz für bestimmte Container angegeben haben. Die gemeinsame Nutzung des Datenbankdurchsatzes durch die Container entspricht dem Hosten einer Datenbank in einem Computercluster. Da alle Container in einer Datenbank die auf einem Computer verfügbaren Ressourcen gemeinsam nutzen, erhalten Sie natürlich keine vorhersagbare Leistung für einen bestimmten Container. Informationen zum Konfigurieren des Durchsatzes für eine Datenbank finden Sie unter [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

Mit dem Festlegen des Durchsatzes für eine Azure Cosmos-Datenbank wird sichergestellt, dass Sie zu jeder Zeit den bereitgestellten Durchsatz erhalten. Da alle Container in der Datenbank den bereitgestellten Durchsatz gemeinsam nutzen, garantiert Azure Cosmos DB keinen vorhersagbaren Durchsatz für einen bestimmten Container in der Datenbank. Der Anteil des Durchsatzes, den ein bestimmter Container erhält, hängt von den folgenden Faktoren ab:

* Anzahl von Containern
* Auswahl der Partitionsschlüssel für verschiedene Container
* Verteilung der Workload auf die verschiedenen logischen Partitionen der Container 

Es wird empfohlen, den Durchsatz für eine Datenbank zu konfigurieren, wenn der Durchsatz von mehreren Containern gemeinsam genutzt werden soll, Sie aber keinen spezifischen Durchsatz für bestimmte Container bereitstellen möchten. Es folgen einige Beispiele, in denen das Bereitstellen von Durchsatz auf Datenbankebene empfehlenswert ist:

* Die gemeinsame Nutzung des für eine Datenbank bereitgestellten Durchsatzes durch eine Gruppe von Containern ist bei mehrinstanzenfähigen Anwendungen nützlich. Jeder Benutzer kann durch einen eigenen Azure Cosmos-Container dargestellt werden.

* Die gemeinsame Nutzung des bereitgestellten Durchsatzes einer Datenbank durch eine Gruppe von Containern ist nützlich, wenn Sie eine NoSQL-Datenbank (z.B. MongoDB, Cassandra), die in einem Cluster von virtuellen Computern oder auf lokalen physischen Servern gehostet wird, zu Azure Cosmos DB migrieren. Sie können sich den bereitgestellten Durchsatz, der für Ihre Azure Cosmos-Datenbank konfiguriert wurde, als logische Entsprechung (jedoch kostengünstiger und flexibler) zur Computekapazität des MongoDB- oder Cassandra-Clusters vorstellen.  

Alle in einer Datenbank erstellten Container mit bereitgestelltem Durchsatz müssen mit einem Partitionsschlüssel erstellt werden. Der einem Container innerhalb einer Datenbank bereitgestellte Durchsatz wird zu jedem Zeitpunkt auf alle logischen Partitionen des Containers aufgeteilt. Wenn Ihre Container den für eine Datenbank bereitgestellten Durchsatz gemeinsam nutzen, können Sie den Durchsatz nicht selektiv einem bestimmten Container oder einer logischen Partition zuordnen. Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der der jeweiligen logischen Partition zugewiesen wurde, werden Ihre Vorgänge ratenbegrenzt. Bei einer Ratenbegrenzung können Sie entweder den Durchsatz für den gesamten Container erhöhen oder den Vorgang wiederholen. Weitere Informationen zur Partitionierung finden Sie unter [Logische Partitionen](partition-data.md).

Sie können mehrere logische Partitionen, die den für eine Datenbank bereitgestellten Durchsatz gemeinsam nutzen, in einer einzelnen physischen Partition hosten. Während eine einzelne logische Partition eines Containers immer zum Gültigkeitsbereich einer physischen Partition gehört, können „L“ logische Partitionen für „C“ Container, die den bereitgestellten Durchsatz einer Datenbank gemeinsam nutzen, auf „R“ physischen Partitionen gehostet werden. Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen hosten kann, die zu unterschiedlichen Containern innerhalb einer Datenbank gehören:

![Physische Partition](./media/set-throughput/resource-partition2.png)

## <a name="setting-throughput-on-a-database-and-a-container"></a>Festlegen des Durchsatzes für eine Datenbank und einen Container

Sie können die beiden Modelle kombinieren und Durchsatz sowohl auf Datenbank- als auch auf Containerebene bereitstellen. Das folgende Beispiel zeigt die Vorgehensweise bei der Bereitstellung von Durchsatz für eine Azure Cosmos-Datenbank und einen Container:

* Sie können eine Azure Cosmos-Datenbank mit dem Namen „Z“ erstellen und für diese als Durchsatz „K“ RUs bereitstellen. 
* Als Nächstes erstellen Sie die fünf Container „A“, „B“, „C“, „D“ und „E“ innerhalb der Datenbank.
* Sie können explizit „P“ RUs des bereitgestellten Durchsatzes für den Container „B“ konfigurieren.
* Der Durchsatz von „K“ RUs wird durch die vier Container „A“, „C“, „D“ und „E“ gemeinsam genutzt. Die genaue Durchsatzmenge, die für „A“, „C“, „D“ oder „E“ zur Verfügung steht, variiert dann, und es gibt keine SLAs für die einzelnen Containerdurchsätze.
* Für den Container „B“ wird jederzeit ein Durchsatz von „P“ RUs sichergestellt und durch SLAs abgedeckt.

## <a name="comparison-of-models"></a>Vergleich der Modelle

|**Kontingent**  |**Bereitgestellter Durchsatz für eine Datenbank**  |**Bereitgestellter Durchsatz für einen Container**|
|---------|---------|---------|
|Minimale RUs |400 (Nach den ersten vier Containern benötigt jeder weitere Container mindestens 100 RU/s.) |400|
|Minimale RUs pro Container|100|400|
|Erforderliche minimale RUs für die Nutzung von 1 GB Speicher|40|40|
|Maximale RUs|Unbegrenzt, in der Datenbank|Unbegrenzt, im Container|
|RUs zugewiesen/verfügbar für einen bestimmten Container|Keine Garantien. Die einem bestimmten Container zugewiesenen RUs hängen von verschiedenen Eigenschaften ab, z.B. der Auswahl von Partitionsschlüsseln für Container, die den Durchsatz gemeinsam nutzen, der Verteilung der Workload und der Anzahl von Containern. |Alle RUs, die für den Container konfiguriert wurden, sind ausschließlich für diesen Container reserviert.|
|Maximale Speicherkapazität für einen Container|Unbegrenzt|Unbegrenzt|
|Maximaler Durchsatz pro logischer Partition eines Containers|10.000 RUs|10.000 RUs|
|Maximale Speicherkapazität (Daten und Index) pro logischer Partition eines Containers|10 GB|10 GB|

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Informieren Sie sich über das [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).
* Informieren Sie sich über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).


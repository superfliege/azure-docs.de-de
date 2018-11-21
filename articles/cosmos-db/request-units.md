---
title: Anforderungseinheiten und Durchsatz in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie die erforderlichen Anforderungseinheiten in Azure Cosmos DB angeben und einschätzen.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: 709cd16c7eee30dd2d88ea87b5f704ad20530ffb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621342"
---
# <a name="request-units-in-azure-cosmos-db"></a>Anforderungseinheiten in Azure Cosmos DB

Bei Azure Cosmos DB zahlen Sie für den bereitgestellten Durchsatz und den verwendeten Speicher auf Stundenbasis. Durchsatz muss bereitgestellt werden, um zu gewährleisten, dass jederzeit genügend Systemressourcen für Ihre Cosmos-Datenbank zur Verfügung stehen, um die Cosmos DB-SLA zu erfüllen oder zu übertreffen.

Cosmos DB unterstützt eine Vielzahl von APIs (SQL, MongoDB, Cassandra, Gremlin und Tabelle). Jede API hat ihren eigenen Satz von Datenbankvorgängen – von einfachen Lese- und Schreibvorgängen für Datenpunkte bis hin zu komplexen Abfragen. Jeder Datenbankvorgang beansprucht je nach Komplexität eine bestimmte Menge an Systemressourcen.  Die Kosten sämtlicher Datenbankvorgänge werden von Cosmos DB normalisiert und in Form von Anforderungseinheiten (Request Units, RUs) ausgedrückt. Eine Anforderungseinheit (1 RU) entspricht den Kosten für das Lesen eines Elements mit einer Größe von 1 KB. Allen anderen Datenbankvorgängen werden analog dazu ebenfalls Kosten in Form von RUs zugewiesen. Die Kosten werden immer in RUs angegeben – unabhängig davon, über welche API Sie mit Ihrem Cosmos-Container interagieren und welchen Datenbankvorgang (Lesen, Schreiben, Abfragen) Sie ausführen.

RU/s ist also gewissermaßen die Währung für Durchsatz. RU/s ist eine ratenbasierte Währung zur Abstrahierung der Systemressourcen wie CPU, IOPS und Arbeitsspeicher, die zum Ausführen der von Cosmos DB unterstützten Datenbankvorgänge erforderlich sind. Die folgende Abbildung zeigt die Anforderungseinheiten, die von verschiedenen Datenbankvorgängen genutzt werden:

![Von Datenbankvorgängen genutzte Anforderungseinheiten](./media/request-units/request-units.png)

Zur Verwaltung und Planung der Kapazität stellt Cosmos DB sicher, dass die Anzahl von RUs für einen bestimmten Datenbankvorgang und ein bestimmtes Dataset deterministisch ist. Die von einem Datenbankvorgang genutzte Anzahl von RUs kann anhand des Antwortheaders ermittelt werden. Wenn Sie mit den Faktoren, die sich auf die Gebühren für Anforderungseinheiten auswirken, sowie mit den Durchsatzanforderungen Ihrer Anwendung vertraut sind, können Sie Ihre Anwendung kostengünstig ausführen.

Die Abrechnung erfolgt zwar auf Stundenbasis, die Anzahl von Anforderungseinheiten für Ihre Anwendung wird jedoch auf Sekundenbasis (in Schritten von jeweils 100 RU/s) bereitgestellt. Um den bereitgestellten Durchsatz für Ihre Anwendung zu skalieren, können Sie jederzeit die Anzahl von RUs (in Schritten von jeweils 100 RUs) erhöhen oder verringern – entweder programmgesteuert oder über das Azure-Portal.

Durchsatz kann in zwei Granularitäten bereitgestellt werden: 

* **Container:** Weitere Informationen finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).
* **Datenbanken:** Weitere Informationen finden Sie unter [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten

Berücksichtigen Sie beim Einschätzen der bereitzustellenden Anzahl von RU/s die folgenden Faktoren:

* **Elementgröße:** Je größer ein Element, desto mehr RUs werden beim Lesen oder Schreiben des Elements genutzt.

* **Dokumentindizierung:** Standardmäßig wird jedes Element automatisch indiziert. Wenn Sie einige Elemente in einem Container nicht indizieren, werden weniger Anforderungseinheiten genutzt.

* **Anzahl von Elementeigenschaften:** Bei Verwendung der standardmäßigen Indizierung aller Eigenschaften erhöht sich die Anzahl von RUs, die beim Schreiben eines Elements genutzt werden, wenn sich die Anzahl von Elementeigenschaften erhöht.

* **Indizierte Eigenschaften:** Eine Indexrichtlinie für jeden Container bestimmt, welche Eigenschaften standardmäßig indiziert werden. Sie können die für Schreibvorgänge genutzten Anforderungseinheiten verringern, indem Sie die Anzahl indizierter Eigenschaften begrenzen.

* **Datenkonsistenz:** Im Vergleich zu anderen, weniger strengen Konsistenzebenen nutzen die Konsistenzebenen „Stark“ und „Begrenzte Veraltung“ bei Lesevorgängen ungefähr zweimal mehr RUs.

* **Abfragemuster:** Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten bei einem Vorgang genutzt werden. Die Kosten von Abfragevorgängen hängen von der Anzahl von Abfrageergebnissen und Prädikaten, der Art der Prädikate, der Anzahl von benutzerdefinierten Funktionen, der Größe der Quelldaten, der Größe des Resultsets und den Projektionen ab. Cosmos DB garantiert, dass die gleiche Abfrage mit den gleichen Daten bei wiederholter Ausführung immer die gleiche Anzahl von RUs beansprucht.

* **Skriptnutzung:** Bei gespeicherten Prozeduren und Triggern hängt die Nutzung von RUs genau wie bei Abfragen von der Komplexität der ausgeführten Vorgänge ab. Sehen Sie sich beim Entwickeln Ihrer Anwendung den request-charge-Header an, um zu ermitteln, wie viele Anforderungseinheiten der jeweilige Vorgang erfordert.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für Cosmos DB-Container und -Datenbanken](set-throughput.md).
* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über [globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md).
* Informieren Sie sich über das [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).
* Informieren Sie sich über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

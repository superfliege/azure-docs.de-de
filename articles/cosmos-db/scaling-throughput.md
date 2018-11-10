---
title: Skalieren des Durchsatzes in Azure Cosmos DB
description: Dieser Artikel beschreibt, wie Azure Cosmos DB den Durchsatz elastisch skaliert.
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: adbac964e6654e16f6c405b9a5b8669326583f3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243945"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Skalieren des Durchsatzes in Azure Cosmos DB

In Azure Cosmos DB wird der bereitgestellte Durchsatz als Anforderungseinheit/Sekunde (RU/s, Mehrzahl: RUs) dargestellt. Anforderungseinheiten messen die Kosten für Lese- und Schreibvorgänge für Ihre Cosmos-Container.

![Anforderungseinheiten](./media/scale-throughput/figure1.png)

Sie können Anforderungseinheiten für einen Cosmos-Container oder eine Cosmos-Datenbanken bereitstellen. In einem Container bereitgestellte Anforderungseinheiten stehen ausschließlich für Vorgänge zur Verfügung, die in diesem Container ausgeführt werden. In einer Datenbank bereitgestellte Anforderungseinheiten werden für alle Container in dieser Datenbank (ausgenommen Container mit exklusiv zugewiesenen RUs) freigegeben.

Sie können jederzeit die bereitgestellte RU/s erhöhen oder verringern, um den Durchsatz elastisch zentral hoch- oder herunterzuskalieren. Weitere Informationen finden Sie im Artikel über die Bereitstellung von Durchsatz und die elastische Skalierung von Cosmos-Containern und -Datenbanken. Um den Durchsatz global zu skalieren, können Sie Ihrem Cosmos-Konto jederzeit Regionen hinzufügen oder diese daraus entfernen. Weitere Informationen finden Sie im Artikel über das Hinzufügen und Entfernen von Regionen im Cosmos-Konto. Die Zuordnung mehrerer Regionen zu einem Cosmos-Konto ist in vielen Szenarien wichtig, um eine geringe Latenz und [Hochverfügbarkeit](high-availability.md) auf der ganzen Welt zu erreichen.

## <a name="throughput-scaling-details"></a>Details zur Durchsatzskalierung

Wenn Sie r RUs in einem Cosmos-Container (oder einer Datenbank) bereitstellen, stellt Cosmos DB sicher, dass sie in *jeder* Region verfügbar sind, die mit Ihrem Cosmos-Konto verknüpft ist. Jedes Mal, wenn Sie Ihrem Cosmos-Konto eine neue Region hinzufügen, stellt Cosmos DB automatisch r RUs in der neu hinzugefügten Region bereit. Die Vorgänge, die für Ihren Cosmos-Container ausgeführt werden, erhalten garantiert r RUs in jeder Region, die mit Ihrem Cosmos-Konto verbunden ist. Sie können die RUs nicht selektiv einer bestimmten Region zuordnen. Die für einen Cosmos-Container (oder eine Datenbank) bereitgestellten RUs werden für alle mit Ihrem Cosmos-Konto verbundenen Regionen bereitgestellt.

Angenommen, ein Cosmos-Container ist mit r RUs konfiguriert und es gibt n Regionen, die dem Cosmos-Konto zugeordnet sind:

- Wenn das Cosmos-Konto mit einer einzigen Schreibregion konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: r x n.
- Wenn das Cosmos-Konto mit mehreren Schreibregionen konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: r x (n+1). Die zusätzlichen r RUs werden automatisch bereitgestellt, um Updatekonflikte und Anti-Entropie-Datenverkehr in den Regionen zu verarbeiten.

Die Wahl des [Konsistenzmodells](consistency-levels.md) wirkt sich auch auf den Durchsatz aus. Für „Sitzung“, „Präfixkonsistenz„ und „letztliche Konsistenz“ können Sie den etwa 2-fachen Lesedurchsatz im Vergleich zu „starke Konsistenz“ und „begrenzte Veraltung“ erzielen.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie, wie Sie den Durchsatz konfigurieren:

* [Festlegen und Abrufen des Durchsatzes für Container und Datenbanken](set-throughput.md) 


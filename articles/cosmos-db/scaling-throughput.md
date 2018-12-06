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
ms.openlocfilehash: 7e3f6d053e9466f07e15b0c2c1092fece76c98a4
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160663"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Skalieren des Durchsatzes in Azure Cosmos DB

In Azure Cosmos DB wird der bereitgestellte Durchsatz als Anforderungseinheit/Sekunde (RU/s, Mehrzahl: RUs) dargestellt. Anforderungseinheiten messen die Kosten für Lese- und Schreibvorgänge für Ihre Cosmos-Container wie in der folgenden Abbildung dargestellt:

![Anforderungseinheiten](./media/scale-throughput/figure1.png)

Sie können Anforderungseinheiten für einen Cosmos-Container oder eine Cosmos-Datenbanken bereitstellen. In einem Container bereitgestellte Anforderungseinheiten stehen ausschließlich für Vorgänge zur Verfügung, die in diesem Container ausgeführt werden. In einer Datenbank bereitgestellte Anforderungseinheiten werden für alle Container in dieser Datenbank (ausgenommen Container mit exklusiv zugewiesenen RUs) freigegeben.

Sie können jederzeit die bereitgestellte RU/s erhöhen oder verringern, um den Durchsatz elastisch zu skalieren. Weitere Informationen finden Sie im Artikel zum [Bereitstellen von Durchsatz](set-throughput.md) und zur elastischen Skalierung von Cosmos-Containern und -Datenbanken. Um den Durchsatz global zu skalieren, können Sie Ihrem Cosmos-Konto jederzeit Regionen hinzufügen oder diese daraus entfernen. Weitere Informationen finden Sie unter [Verwalten von Datenbankkonten in Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Die Zuordnung mehrerer Regionen zu einem Cosmos-Konto ist in vielen Szenarien wichtig, um eine geringe Latenz und [Hochverfügbarkeit](high-availability.md) auf der ganzen Welt zu erreichen.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Verteilen des bereitgestellten Durchsatzes über mehrere Regionen

Wenn Sie „R“ RUs in einem Cosmos-Container (oder einer Datenbank) bereitstellen, stellt Cosmos DB sicher, dass sie in *jeder* Region verfügbar sind, die mit Ihrem Cosmos-Konto verknüpft ist. Jedes Mal, wenn Sie Ihrem Konto eine neue Region hinzufügen, stellt Cosmos DB automatisch „R“ RUs in der neu hinzugefügten Region bereit. Die Vorgänge, die für Ihren Cosmos-Container ausgeführt werden, erhalten garantiert „R“ RUs in jeder Region. Sie können RUs nicht selektiv einer bestimmten Region zuweisen. Die für einen Cosmos-Container (oder eine Datenbank) bereitgestellten RUs werden für alle mit Ihrem Cosmos-Konto verbundenen Regionen bereitgestellt.

Angenommen, ein Cosmos-Container ist mit „R“ RUs konfiguriert und es gibt „N“ Regionen, die dem Cosmos-Konto zugeordnet sind:

- Wenn das Cosmos-Konto mit einer einzigen Schreibregion konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: r x n.

- Wenn das Cosmos-Konto mit mehreren Schreibregionen konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: r x (n+1). Die zusätzlichen r RUs werden automatisch bereitgestellt, um Updatekonflikte und Anti-Entropie-Datenverkehr in den Regionen zu verarbeiten.

Die Wahl des [Konsistenzmodells](consistency-levels.md) wirkt sich auch auf den Durchsatz aus. Für „Sitzung“, „Präfixkonsistenz„ und „letztliche Konsistenz“ können Sie den etwa 2-fachen Lesedurchsatz im Vergleich zu „starke Konsistenz“ und „begrenzte Veraltung“ erzielen.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie, wie Sie den Durchsatz konfigurieren:

* [Festlegen und Abrufen des Durchsatzes für Container und Datenbanken](set-throughput.md) 


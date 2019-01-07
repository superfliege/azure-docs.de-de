---
title: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
description: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
keywords: Konsistenz, Leistung, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: af17815d2dcf36909ba9b2109f0f9939c79508c0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848696"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB

Bei verteilten Datenbanken, die mithilfe der Replikation Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherstellen, müssen Kompromisse eingegangen werden. Diese Kompromisse betreffen die Lesekonsistenz im Vergleich zu Verfügbarkeit, Latenz und Durchsatz. 

Azure Cosmos DB bietet in Bezug auf die Datenkonsistenz vielfältige Auswahlmöglichkeiten. Dieser Ansatz beinhaltet mehr Optionen als die beiden Extreme der starken und der letztlichen Konsistenz. Sie können im Hinblick auf die Konsistenz aus fünf gut definierten Modellen auswählen. Diese sind (vom stärksten bis zum schwächsten Modell):

- Starke Konsistenz 
- Begrenzte Veraltung 
- Sitzungskonsistenz 
- Präfixkonsistenz 
- Letztliche Konsistenz 

Jedes der fünf Modelle bietet Verfügbarkeits- und Leistungskompromisse und wird durch eine umfassende SLA abgesichert.

## <a name="consistency-levels-and-latency"></a>Konsistenzebenen und Latenz

- Für alle Konsistenzebenen wird eine Leselatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Diese Leselatenz wird durch die SLA abgesichert. Die durchschnittliche Leselatenz (im 50. Perzentil) beträgt typischerweise 2 Millisekunden oder weniger. Eine Ausnahme bei dieser Garantie bilden Azure Cosmos-Konten, die sich über mehrere Regionen erstrecken und mit starker Konsistenz konfiguriert sind.

-  Für die verbleibenden Konsistenzebenen wird eine Schreiblatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Diese Schreiblatenz wird durch die SLA abgesichert. Die durchschnittliche Schreiblatenz (im 50. Perzentil) beträgt üblicherweise 5 Millisekunden oder weniger.

Einige Azure Cosmos-Konten umfassen möglicherweise mehrere Regionen mit starker Konsistenz. In diesem Fall beträgt die garantierte Schreiblatenz < (2 * RTT [Roundtripzeit]) + 10 Millisekunden im 99. Perzentil. Für die Ermittlung der RTT werden die beiden am weitesten entfernten Regionen für Ihr Azure Cosmos-Konto herangezogen. Sie entspricht der Roundtripzeit zwischen den zwei am weitesten voneinander entfernten Regionen, die Ihrem Azure Cosmos-Konto zugeordnet sind. Diese Option befindet sich zurzeit in der Vorschau. 

Die exakte RTT-Latenz richtet sich nach der physischen Entfernung und der Azure-Netzwerktopologie. Azure-Netzwerke bieten keine Latenz-SLAs für die RTT zwischen zwei Azure-Regionen. Die Replikationslatenzzeiten für Ihr Azure Cosmos-Konto werden im Azure-Portal angezeigt. Sie können das Azure-Portal nutzen, um die Replikationslatenz zwischen verschiedenen Regionen zu überwachen, die Ihrem Konto zugeordnet sind.

## <a name="consistency-levels-and-throughput"></a>Konsistenzebenen und Durchsatz

- Für die gleiche Anzahl von Anforderungseinheiten (RUs) bieten Sitzungskonsistenz, Präfixkonsistenz und letztliche Konsistenz im Vergleich zur starken Konsistenz und der begrenzten Veraltung etwa das 2-fache an Lesedurchsatz.

- Für bestimmte Schreibvorgänge (z.B. Einfügen, Ersetzen, Upsert, Löschen) ist der Schreibdurchsatz für RUs für alle Konsistenzebenen identisch.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel bieten weitere Informationen zur globalen Verteilung und zu den allgemeinen Konsistenzkompromissen in verteilten Systemen. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Hochverfügbarkeit](high-availability.md)
* [Azure Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

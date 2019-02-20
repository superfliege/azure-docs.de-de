---
title: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
description: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ee0dc1bec39bf95cbf4f3bf7ecea92b877a78b88
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113752"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung 

Bei verteilten Datenbanken, die mithilfe der Replikation Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherstellen, müssen Kompromisse eingegangen werden. Diese Kompromisse betreffen die Lesekonsistenz im Vergleich zu Verfügbarkeit, Latenz und Durchsatz.

Azure Cosmos DB bietet in Bezug auf die Datenkonsistenz vielfältige Auswahlmöglichkeiten. Dieser Ansatz beinhaltet mehr Optionen als die beiden Extreme der starken und der letztlichen Konsistenz. Sie können im Hinblick auf die Konsistenz aus fünf gut definierten Modellen auswählen. Diese sind (vom stärksten bis zum schwächsten Modell):

- STARK (Strong)
- Bounded staleness
- Sitzung
- Konsistentes Präfix
- Letztlich (Eventual)

Jedes der fünf Modelle bietet Verfügbarkeits- und Leistungskompromisse und wird durch eine umfassende SLA abgesichert.

## <a name="consistency-levels-and-latency"></a>Konsistenzebenen und Latenz

- Für alle Konsistenzebenen wird eine Leselatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Diese Leselatenz wird durch die SLA abgesichert. Die durchschnittliche Leselatenz (im 50. Perzentil) beträgt typischerweise 2 Millisekunden oder weniger. Eine Ausnahme bei dieser Garantie bilden Azure Cosmos-Konten, die sich über mehrere Regionen erstrecken und mit starker Konsistenz konfiguriert sind.

- Für die verbleibenden Konsistenzebenen wird eine Schreiblatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Außerdem wird sie durch die SLA unterstützt. Die durchschnittliche Schreiblatenz (im 50. Perzentil) beträgt üblicherweise 5 Millisekunden oder weniger.

Einige Azure Cosmos-Konten umfassen möglicherweise mehrere Regionen mit starker Konsistenz. In diesem Fall beträgt die garantierte Schreiblatenz < (2 RTT [Roundtripzeit]) + 10 Millisekunden im 99. Perzentil. Für die Ermittlung der RTT werden die beiden am weitesten entfernten Regionen für Ihr Azure Cosmos-Konto herangezogen. Sie entspricht der Roundtripzeit zwischen den zwei am weitesten voneinander entfernten Regionen, die Ihrem Azure Cosmos-Konto zugeordnet sind. Diese Option befindet sich zurzeit in der Vorschau.

Die exakte RTT-Latenz richtet sich nach der physischen Entfernung und der Azure-Netzwerktopologie. Azure-Netzwerke bieten keine Latenz-SLAs für die RTT zwischen zwei Azure-Regionen. Die Replikationslatenzzeiten für Ihr Azure Cosmos-Konto werden im Azure-Portal angezeigt. Sie können das Azure-Portal nutzen, um die Replikationslatenz zwischen verschiedenen Regionen zu überwachen, die Ihrem Konto zugeordnet sind.

## <a name="consistency-levels-and-throughput"></a>Konsistenzebenen und Durchsatz

- Für die gleiche Anzahl von Anforderungseinheiten (RUs) bieten Sitzungskonsistenz, Präfixkonsistenz und letztliche Konsistenz im Vergleich zur starken Konsistenz und der begrenzten Veraltung etwa das 2-fache an Lesedurchsatz.

- Für bestimmte Schreibvorgänge (z.B. Einfügen, Ersetzen, Upsert, Löschen) ist der Schreibdurchsatz für RUs für alle Konsistenzebenen identisch.

## <a name="consistency-levels-and-data-durability"></a>Konsistenzebenen und Datendauerhaftigkeit

Bei einer global verteilten Datenbankumgebung besteht eine direkte Beziehung zwischen der Konsistenzebene und der Datendauerhaftigkeit bei einem Ausfall in der gesamten Region. In der Tabelle wird die Beziehung zwischen dem Konsistenzmodell und der Datendauerhaftigkeit bei einem regionsweiten Ausfall definiert. Beachten Sie unbedingt, dass es in einem verteilten System auch bei starker Konsistenz unmöglich ist, eine verteilte Datenbank mit einem RPO- und RTO-Wert von 0 (null) zu erzielen. Grund ist das CAP-Theorem. Weitere Informationen finden Sie unter  [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).

|**Region(en)**|**Replikationsmodus**|**Konsistenzebene**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Einzel- oder Multimaster|Jede Konsistenzebene|< 240 Minuten|< 1 Woche|
|> 1|Einzelmaster|Sitzung, Präfixkonsistenz, Letztlich|< 15 Minuten|< 15 Minuten|
|> 1|Einzelmaster|Begrenzte Veraltung (Bounded staleness)|K & T*|< 15 Minuten|
|> 1|Multimaster|Sitzung, Präfixkonsistenz, Letztlich|< 15 Minuten|0|
|> 1|Multimaster|Begrenzte Veraltung (Bounded staleness)|K & T*|0|
|> 1|Einzel- oder Multimaster|STARK (Strong)|0|< 15 Minuten|

* K & T = Anzahl von „K“-Versionen (Updates) eines Elements. Oder „T“-Zeitintervall.



## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel bieten weitere Informationen zur globalen Verteilung und zu den allgemeinen Konsistenzkompromissen in verteilten Systemen. Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Hochverfügbarkeit](high-availability.md)
- [Azure Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

---
title: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen | Microsoft-Dokumentation
description: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
keywords: Konsistenz, Leistung, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 061a7c223d0e1c6fa7384d7defe9f84f470236ce
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243825"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels"></a>Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen

Verteilte Datenbanken, die auf Replikation für Hochverfügbarkeit, niedrige Latenz oder beides angewiesen sind, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz und Durchsatz. Azure Cosmos DB betrachtet Datenkonsistenz als ein Spektrum von Auswahlmöglichkeiten und reduziert sie nicht auf die beiden Extreme „starke Konsistenz“ und „letztliche Konsistenz“. Mit Cosmos DB können Entwickler zwischen fünf klar definierten Konsistenzmodellen aus dem gesamten Konsistenzspektrum wählen. Diese sind (von der stärksten bis zur schwächsten Option): **starke Konsistenz**, **begrenzte Veraltung**, **Sitzung**, **Präfixkonsistenz** und **letztliche Konsistenz**. Jedes der fünf Konsistenzmodelle bietet klare Verfügbarkeits- und Leistungskompromisse und wird durch umfassende SLAs unterstützt.

## <a name="consistency-levels-and-latency"></a>Konsistenzebenen und Latenz

- Die für alle Konsistenzebenen garantierte **Leselatenz** liegt jederzeit unter 10 Millisekunden im 99. Perzentil und wird durch das SLA unterstützt. Die durchschnittliche Leselatenz (im 50. Perzentil) beträgt i.d.R. maximal 2 Millisekunden.
- Mit Ausnahme von Cosmos-Konten, die mehrere Regionen umfassen und mit der starken Konsistenz konfiguriert sind, beträgt die **Schreiblatenz** für alle Konsistenzebenen jederzeit weniger als 10 Millisekunden im 99. Perzentil und wird durch das SLA unterstützt. Die durchschnittliche Schreiblatenz (im 50. Perzentil) beträgt i.d.R. maximal 5 Millisekunden.
- Für die Cosmos-Konten mit mehreren Regionen, die mit starker Konsistenz konfiguriert sind (derzeit in der Vorschau), ist die garantierte **Schreiblatenz** < (2 * RTT (Roundtripzeit)) + 10 Millisekunden im 99. Perzentil. Die RTT bezieht sich auf die beiden am weitesten entfernten Regionen, die mit Ihrem Cosmos-Konto verbunden sind. Die exakte RTT-Latenz ist eine Funktion für schnelle Geschwindigkeit und die genaue Azure-Netzwerktopologie. Azure-Netzwerke bieten keine Latenz-SLAs für die RTT zwischen zwei beliebigen Azure-Regionen. Die Cosmos DB-Replikationslatenzen werden im Azure-Portal für Ihr Cosmos-Konto angezeigt, sodass Sie die Replikationslatenzen zwischen verschiedenen Regionen überwachen können, die mit Ihrem Cosmos-Konto verbunden sind.

## <a name="consistency-levels-and-throughput"></a>Konsistenzebenen und Durchsatz

- Für die gleiche Anzahl von Anforderungseinheiten (RUs) bieten „Sitzung“, „Präfixkonsistenz„ und „letztliche Konsistenz“ den etwa 2-fachen Lesedurchsatz im Vergleich zu „starke Konsistenz“ und „begrenzte Veraltung“.
- Für bestimmte Schreibvorgänge (z.B. Einfügen, Ersetzen, Upsert, Löschen) ist der Schreibdurchsatz für RUs für alle Konsistenzebenen identisch.

## <a name="consistency-levels-and-durability"></a>Konsistenzebenen und Dauerhaftigkeit

Bevor ein Schreibvorgang dem Client gegenüber bestätigt wird, werden die Daten von einem Quorum von Replikaten in der Region dauerhaft committet, die die Schreibvorgänge akzeptiert. Wenn der Container mit einer konsistenten Indizierungsrichtlinie konfiguriert ist, wird der Index außerdem synchron aktualisiert, repliziert und vom Quorum der Replikate dauerhaft committet, bevor der Schreibvorgang dem Client gegenüber bestätigt wird. 

Die folgende Tabelle fasst den Bereich des potenziellen Datenverlusts im Falle eines regionalen Ausfalls für die Cosmos-Konten mit mehreren Regionen zusammen.

| **Konsistenzebene** | **Bereich des potenziellen Datenverlusts bei einem regionalen Ausfall** |
| - | - |
| STARK (Strong) | Null |
| Begrenzte Veraltung (Bounded staleness) | Beschränkt auf den vom Entwickler für das Cosmos-Konto konfigurierten Veraltungsbereich |
| Sitzung | Bis zu 5 Sekunden |
| Präfixkonsistenz | Bis zu 5 Sekunden |
| Letztlich (Eventual) | Bis zu 5 Sekunden |

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zur globalen Verteilung und zu den allgemeinen Konsistenzkompromissen in verteilten Systemen:

* [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Hochverfügbarkeit](high-availability.md)
* [Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

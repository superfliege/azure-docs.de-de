---
title: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB | Microsoft-Dokumentation
description: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
keywords: Konsistenz, Leistung, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 0e4105d6f56a8eb45a83e970c85319cf25041781
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514773"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB

Verteilte Datenbanken, die auf Replikation angewiesen sind, um Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherzustellen, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz sowie Durchsatz. Azure Cosmos DB betrachtet Datenkonsistenz als ein Spektrum von Auswahlmöglichkeiten und reduziert sie nicht auf die beiden Extreme „starke Konsistenz“ und „letztliche Konsistenz“. Mit Cosmos DB können Entwickler zwischen fünf klar definierten Konsistenzmodellen aus dem gesamten Konsistenzspektrum wählen. Diese sind (von der stärksten bis zur schwächsten Option): **starke Konsistenz**, **begrenzte Veraltung**, **Sitzungskonsistenz**, **Präfixkonsistenz** und **letztliche Konsistenz**. Jedes der fünf Konsistenzmodelle bietet Verfügbarkeits- und Leistungskompromisse und wird durch umfassende SLAs abgesichert.

## <a name="consistency-levels-and-latency"></a>Konsistenzebenen und Latenz

- Die für alle Konsistenzebenen garantierte **Leselatenz** liegt jederzeit unter 10 Millisekunden im 99. Perzentil und wird durch das SLA unterstützt. Die durchschnittliche Leselatenz (im 50. Perzentil) beträgt i.d.R. maximal 2 Millisekunden.

- Mit Ausnahme von Cosmos-Konten, die mehrere Regionen umfassen und mit der starken Konsistenz konfiguriert sind, liegt die **Schreiblatenz** für die anderen Konsistenzebenen jederzeit unter 10 Millisekunden im 99. Perzentil. Außerdem wird sie durch die SLA unterstützt. Die durchschnittliche Schreiblatenz (im 50. Perzentil) beträgt i.d.R. maximal 5 Millisekunden.

- Für die Cosmos-Konten mit mehreren Regionen, die mit starker Konsistenz konfiguriert sind (derzeit in der Vorschau), ist die garantierte **Schreiblatenz** < (2 * RTT (Roundtripzeit)) + 10 Millisekunden im 99. Perzentil. Die RTT bezieht sich auf die beiden am weitesten entfernten Regionen, die mit Ihrem Cosmos-Konto verbunden sind. Die exakte RTT-Latenz ist eine Funktion für schnelle Geschwindigkeit und die genaue Azure-Netzwerktopologie. Azure-Netzwerke bieten keine Latenz-SLAs für die RTT zwischen zwei beliebigen Azure-Regionen. Die Cosmos DB-Replikationslatenzen werden im Azure-Portal für Ihr Cosmos-Konto angezeigt, sodass Sie die Replikationslatenzen zwischen verschiedenen Regionen überwachen können, die mit Ihrem Cosmos-Konto verbunden sind.

## <a name="consistency-levels-and-throughput"></a>Konsistenzebenen und Durchsatz

- Für die gleiche Anzahl von Anforderungseinheiten (RUs) bieten die Sitzungskonsistenz, Präfixkonsistenz und letztliche Konsistenz den etwa 2-fachen Lesedurchsatz im Vergleich zur starken Konsistenz und der begrenzten Veraltung.

- Für bestimmte Schreibvorgänge (z.B. Einfügen, Ersetzen, Upsert, Löschen) ist der Schreibdurchsatz für RUs für alle Konsistenzebenen identisch.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zur globalen Verteilung und zu den allgemeinen Konsistenzkompromissen in verteilten Systemen:

* [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Hochverfügbarkeit](high-availability.md)
* [Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

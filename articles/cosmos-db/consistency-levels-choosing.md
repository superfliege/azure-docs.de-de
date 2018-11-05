---
title: Auswählen der richtigen Konsistenzebene für Ihre Anwendung | Microsoft-Dokumentation
description: Auswählen der richtigen Konsistenzebene für Ihre Anwendung in Azure Cosmos DB.
keywords: Konsistenz, Leistung, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 4b438320ac75918d10880a4bcdccadca4eebec32
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243985"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Auswählen der richtigen Konsistenzebene für Ihre Anwendung

Verteilte Datenbanken, die auf Replikation für Hochverfügbarkeit, niedrige Latenzzeiten oder beides angewiesen sind, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz und Durchsatz. Die meisten kommerziell verfügbaren verteilten Datenbanken verlangen von Entwicklern, dass sie zwischen den beiden extremen Konsistenzmodellen wählen: starke Konsistenz und letztliche Konsistenz. Azure Cosmos DB bietet Entwicklern die Auswahl zwischen fünf klar definierten Konsistenzmodellen: starke Konsistenz, begrenzte Veraltung, Sitzungskonsistenz, Präfixkonsistenz und letztliche Konsistenz. Jedes dieser Konsistenzmodelle ist klar definiert, intuitiv und kann für spezifische reale Szenarien verwendet werden. Jedes der fünf Konsistenzmodelle bietet klare Verfügbarkeits- und Leistungskompromisse und wird durch umfassende SLAs unterstützt. Die folgenden einfachen Überlegungen helfen Ihnen, in vielen gängigen Szenarien die richtige Wahl zu treffen.

## <a name="sql-api-or-table-api"></a>SQL-API oder Tabellen-API

- Für viele reale Szenarien ist Sitzungskonsistenz optimal und die empfohlene Option. Weitere Informationen finden Sie unter [Verwalten des Sitzungstokens für Ihre Anwendung](how-to-manage-consistency.md#utilize-session-tokens).
- Wenn Ihre Anwendung eine starke Konsistenz erfordert, wird empfohlen, die Konsistenzebene begrenzte Veraltung zu verwenden.
- Wenn Sie strengere Konsistenzgarantien benötigen als die, die durch Sitzungskonsistenz und Latenz im einstelligen Millisekundenbereich für Schreibvorgänge bereitgestellt werden, wird empfohlen, die Konsistenzebene mit begrenzter Veraltung zu verwenden.  
- Wenn Ihre Anwendung letztliche Konsistenz erfordert, wird empfohlen, die Konsistenzebene Präfixkonsistenz zu verwenden.
- Wenn Sie weniger strenge Konsistenzgarantien als mit Sitzungskonsistenz benötigen, wird empfohlen, die Konsistenzebene Präfixkonsistenz zu verwenden.
- Wenn Sie die höchste Verfügbarkeit und niedrigste Latenz benötigen, verwenden Sie die letztliche Konsistenzebene.

## <a name="cassandra-mongodb-or-gremlin-api"></a>Cassandra, MongoDB oder Gremlin-API

- Informationen zur Zuordnung zwischen der „Lesekonsistenzebene“ der Apache Cassandra- und Cosmos DB-Konsistenzebenen finden Sie unter [Konsistenzebenen und Cosmos DB-APIs](consistency-levels-across-apis.md#cassandra-mapping).
- Informationen zur Zuordnung zwischen dem „Leseinteresse“ der MongoDB- und Azure Cosmos DB-Konsistenzebenen finden Sie unter [Konsistenzebenen und Cosmos DB-APIs](consistency-levels-across-apis.md#mongo-mapping).

## <a name="you-may-get-stronger-consistency-guarantees-in-practice"></a>Sie erhalten möglicherweise in der Praxis stärkere Konsistenzgarantien.

Konsistenzgarantien für einen Lesevorgang entsprechen der Aktualität und Sortierung des Zustands der angeforderten Datenbank. Lesekonsistenz ist an die Sortierung und Verbreitung der Schreibvorgänge (Aktualisierungsvorgänge) gebunden.  

Wenn die Konsistenzebene auf **begrenzte Veraltung** festgelegt ist, garantiert Cosmos DB, dass die Clients immer den Wert eines vorherigen Schreibvorgangs mit einer Verzögerung lesen, die durch das Veraltungsfenster gebunden wird.

Wenn die Konsistenzebene auf **stark** festgelegt ist, ist das Veraltungsfenster gleich Null, und für die Clients wird garantiert, dass sie den letzten committeten Wert des Schreibvorgangs lesen.

Für die verbleibenden drei Konsistenzebenen ist das Veraltungsfenster weitgehend von Ihrer Workload abhängig. Wenn beispielsweise keine Schreibvorgänge für die Datenbank stattfinden, ist es wahrscheinlich, dass ein Lesevorgang mit **letztlicher**, **Sitzungs**- oder **Präfixkkonsistenz**-Konsistenzebene die gleichen Ergebnisse liefert wie ein Lesevorgang mit starker Konsistenzebene.

Wenn Ihr Cosmos DB-Konto mit einer anderen Konsistenzebene als der starken Konsistenz konfiguriert ist, können Sie die Wahrscheinlichkeit herausfinden, mit der Ihre Clients für Ihre Workload(s) stark konsistente (linearisierbare) Lesezugriffe erhalten, indem Sie sich die im Azure-Portal bereitgestellte PBS-Metrik (Probabilistic Bounded Staleness) ansehen. [Hier erfahren Sie, wie Sie die PBS-Metrik verwenden können](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric). PBS (Probabilistic Bounded Staleness) zeigt, wie letztendlich Ihre letztendliche Konsistenz ist. Diese Metrik vermittelt einen Einblick, wie oft Sie eine stärkere Konsistenz erhalten als die Konsistenzebene, die Sie in Ihrem Cosmos DB-Konto konfiguriert haben. Mit anderen Worten: Sie können die Wahrscheinlichkeit (in Millisekunden dargestellt) erkennen, dass stark konsistente Lesezugriffe für eine Kombination aus Schreib- und Lesebereichen auftreten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Konsistenzebenen erhalten Sie in den folgenden Artikeln:

* [Zuordnung der Konsistenzebenen in Cosmos DB-APIs](consistency-levels-across-apis.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Verwalten des Sitzungstokens für Ihre Anwendung](how-to-manage-consistency.md#utilize-session-tokens)
* [Überwachen der PBS-Metrik (Probabilistically Bounded Staleness)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
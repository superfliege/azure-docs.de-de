---
title: Konsistenzebenen in Azure Cosmos DB | Microsoft-Dokumentation
description: Azure Cosmos DB bietet fünf Konsistenzebenen, um für vorhersehbare Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz sorgen zu können.
keywords: Letztliche Konsistenz, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ace11cf3704ddbd503c0202d45874670476198e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624826"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Konsistenzebenen in Azure Cosmos DB

Verteilte Datenbanken, die auf Replikation angewiesen sind, um Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherzustellen, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz sowie Durchsatz. Die meisten kommerziell verfügbaren verteilten Datenbanken verlangen von Entwicklern, dass sie zwischen den beiden extremen Konsistenzmodellen wählen: starke Konsistenz und letztliche Konsistenz. Die  [Linearisierbarkeit](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) bzw. die starke Konsistenz ist zwar das Nonplusultra in Sachen Datenprogrammierbarkeit, sie wird jedoch durch hohe Latenzen (im stabilen Zustand) und durch geringere Verfügbarkeit (bei Ausfällen) teuer erkauft. Auf der anderen Seite bietet die letztliche Konsistenz eine höhere Verfügbarkeit und bessere Leistung, erschwert jedoch die Programmierung von Anwendungen erheblich.

Cosmos DB bietet für die Datenkonsistenz nicht nur diese beiden Extreme, sondern ein ganzes Spektrum von Auswahlmöglichkeiten. Die starke und die letztliche Konsistenz bilden die beiden Enden des Spektrums, und dazwischen gibt es eine Reihe weiterer Konsistenzoptionen. Diese Konsistenzoptionen ermöglichen es Entwicklern, präzise Entscheidungen zu treffen, und bieten differenzierte Kompromisse hinsichtlich Hochverfügbarkeit und Leistung. Mit Cosmos DB können Entwickler zwischen fünf klar definierten Konsistenzmodellen aus dem gesamten Konsistenzspektrum wählen. Diese sind (von der stärksten bis zur schwächsten Option): **starke Konsistenz**, **begrenzte Veraltung**, **Sitzungskonsistenz**, **Präfixkonsistenz** und **letztliche Konsistenz**. Jedes dieser Konsistenzmodelle ist klar definiert, intuitiv und kann für spezifische reale Szenarien verwendet werden. Jedes der fünf Konsistenzmodelle bietet [Verfügbarkeits- und Leistungskompromisse](consistency-levels-tradeoffs.md) und wird durch umfassende SLAs abgesichert. Die folgende Abbildung zeigt verschiedene Konsistenzebenen als Spektrum:

![Konsistenz als Spektrum](./media/consistency-levels/five-consistency-levels.png)

Die Konsistenzebenen sind regionsunabhängig. Die Konsistenzebene Ihres Cosmos DB-Kontos ist für alle Lesevorgänge garantiert – unabhängig von der Region, in der die Lese- und Schreibvorgänge verarbeitet werden, der Anzahl der mit Ihrem Cosmos-Konto verbundenen Regionen und davon, ob Ihr Konto mit einer oder mehreren Schreibregionen konfiguriert ist.

## <a name="scope-of-the-read-consistency"></a>Geltungsbereich der Lesekonsistenz

Die Lesekonsistenz gilt für einen einzelnen Lesevorgang innerhalb eines Partitionsschlüsselbereichs (d.h. einer logischen Partition). Ein Lesevorgang kann durch einen Remoteclient oder eine gespeicherte Prozedur ausgelöst werden.

## <a name="configuring-the-default-consistency-level"></a>Konfigurieren der Standardkonsistenzebene

Sie können die **Standardkonsistenzebene** in Ihrem Cosmos DB-Konto jederzeit konfigurieren. Die in Ihrem Konto konfigurierte Standardkonsistenzebene gilt für alle Cosmos-Datenbanken (und Container) in diesem Konto. Bei allen in einem Container oder einer Datenbank ausgeführten Lesevorgängen und Abfragen wird standardmäßig die angegebene Konsistenzebene verwendet. Weitere Informationen finden Sie unter [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantien in Zusammenhang mit Konsistenzebenen

Die von Azure Cosmos DB bereitgestellten umfassenden SLAs garantieren, dass 100 % aller Leseanforderungen die Konsistenzgarantie für jede von Ihnen ausgewählte Konsistenzebene erfüllen. Bei einer Leseanforderung gilt die Konsistenz-SLA als erfüllt, wenn alle Konsistenzgarantien der Konsistenzebene gegeben sind. Die genauen Definitionen der fünf Konsistenzebenen in Cosmos DB – unter Verwendung der [TLA+-Spezifikationssprache](http://lamport.azurewebsites.net/tla/tla.html) – finden Sie im GitHub-Repository [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla). Im Folgenden wird die Semantik der fünf Konsistenzebenen beschrieben:

- **Konsistenzebene „starke Konsistenz“**: Die starke Konsistenz bietet garantierte [Linearisierbarkeit](https://aphyr.com/posts/313-strong-consistency-models), was heißt, dass Lesevorgänge auf jeden Fall die neueste Version eines Elements zurückgeben, für die ein Commit ausgeführt wurde. Einem Client wird nie ein partieller Schreibvorgang bzw. ein Schreibvorgang, für den kein Commit ausgeführt wurde, angezeigt. Benutzer haben immer die Garantie, dass sie den neuesten Schreibvorgang lesen, für den ein Commit ausgeführt wurde.

- **Konsistenzebene „begrenzte Veraltung“**: Die Lesevorgänge berücksichtigen immer die Garantie der Präfixkonsistenz. Lesevorgänge bleiben höchstens K Versionen (d.h. Updates) eines Elements oder um ein durch „t“ definiertes Zeitintervall hinter Schreibvorgängen zurück. Wenn Sie die begrenzte Veraltung auswählen, haben Sie zwei Optionen, die Veraltung zu konfigurieren: 

  * Anhand der Anzahl von Versionen (K) des Elements
  * Anhand des Zeitintervalls (t) zwischen Lese- und Schreibvorgängen 

  Begrenzte Veraltung bietet eine vollständige globale Reihenfolge außer innerhalb des „Veraltungsfensters“. Die monotonen Lesegarantien bestehen innerhalb einer Region sowohl innerhalb als auch außerhalb des Veraltungszeitfensters. Die starke Konsistenz weist die gleiche Semantik auf wie die begrenzte Veraltung, und das „Zeitfenster der Veraltung“ ist gleich Null (0). Die begrenzte Veraltung wird auch als **Linearisierbarkeit mit Zeitverzögerung** bezeichnet. Wenn ein Client Lesevorgänge in einer Region ausführt, die Schreibvorgänge akzeptiert, bietet die begrenzte Veraltung die gleichen Garantien wie die starke Konsistenz.

- **Konsistenzebene „Sitzungskonsistenz“**: Die Lesevorgänge berücksichtigen immer folgende Garantien: Präfixkonsistenz, monotone Lesevorgänge, monotone Schreibvorgänge, Lesen der eigenen Schreibvorgänge, Schreibvorgänge folgen Lesevorgängen. Die Sitzungskonsistenz gilt immer für eine Clientsitzung.

- **Konsistenzebene „Präfixkonsistenz“**: Zurückgegebene Updates enthalten ein bestimmtes Präfix aller Updates ohne Lücken. Die Präfixkonsistenz stellt sicher, dass Lesevorgängen keine Schreibvorgänge außerhalb der Reihenfolge angezeigt werden.

- **Konsistenzebene „letztliche Konsistenz“**: Es gibt keine Reihenfolgengarantie für Lesevorgänge. Wenn keine weiteren Schreibvorgänge vorhanden sind, konvergieren die Replikate schließlich.

## <a name="consistency-levels-explained-through-baseball"></a>Konsistenzebenen – erläutert am Beispiel Baseball

Betrachten wir das Baseballszenario aus dem Dokument [Replicated Data Consistency Explained Through Baseball](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) (Replizierte Datenkonsistenz – erläutert am Beispiel Baseball): Stellen Sie sich eine Sequenz von Schreibvorgängen vor, die das Ergebnis eines Baseballspiels mit den Punkteständen für jedes Inning darstellen. Das hypothetische Baseballspiel befindet sich gerade mitten im siebten Inning (dem sprichwörtlichen „Seventh-inning stretch“), und die Heimmannschaft gewinnt 2-5.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Runs** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Gäste** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **Heim** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Ein Cosmos DB-Container enthält die Gesamtanzahl von Runs des Gast- und des Heimteams. Während das Spiel läuft, können verschiedene Lesegarantien dazu führen, dass Clients verschiedene Punktestände lesen. Die folgende Tabelle listet sämtliche Punktestände auf, die durch Lesen der Punktestände für Gast- und Heimteam mit jeder der fünf Konsistenzgarantien zurückgegeben werden können. Die Ergebnisse des Gastteams werden zuerst aufgeführt, und die verschiedenen möglichen Rückgabewerte werden durch Trennzeichen getrennt.

| **Konsistenzebene** | **Punktestände** |
| - | - |
| **Starke Konsistenz** | 2-5 |
| **Begrenzte Veraltung** | Punktestände, die mindestens ein Inning „veraltet“ sind: 2-3, 2-4, 2-5 |
| **Sitzungskonsistenz** | <ul><li>Für den Writer: 2-5</li><li> Für alle anderen (außer dem Writer): 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>Nach dem Lesen von 1-3:  1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Präfixkonsistenz** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Letztliche Konsistenz** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Zusätzliche Lektüre

In folgenden Ressourcen erfahren Sie mehr über Konsistenzkonzepte:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Allgemeine TLA+-Spezifikationen für die fünf von Azure Cosmos DB angebotenen Konsistenzebenen)
- [Replicated Data Consistency Explained through Baseball (video) by Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I) (Erläuterung der Konsistenz replizierter Daten anhand von Baseball – Video von Doug Terry)
- [Replicated Data Consistency Explained through Baseball (whitepaper) by Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf) (Replizierte Datenkonsistenz – erläutert am Beispiel Baseball – Whitepaper von Doug Terry)
- [Sitzungsgarantien für schwach konsistente replizierte Daten](https://dl.acm.org/citation.cfm?id=383631)
- [Consistency Tradeoffs in Modern Distributed Database Systems Design: CAP is Only Part of the Story](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html) (Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme: CAP ist nur ein Teil der Wahrheit)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Eventually Consistent – Revisited](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html) (Letztliche Konsistenz – Neuauflage)

## <a name="next-steps"></a>Nächste Schritte

Um mehr über die Konsistenzebenen in Azure Cosmos DB zu erfahren, lesen Sie die folgenden Artikel:

* [Auswählen der richtigen Konsistenzebene für Ihre Anwendung](consistency-levels-choosing.md)
* [Konsistenzebenen und Cosmos DB-APIs](consistency-levels-across-apis.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Außerkraftsetzen der Standardkonsistenzebene](how-to-manage-consistency.md#override-the-default-consistency-level)


---
title: Wichtige Vorteile der globalen Verteilung durch Azure Cosmos DB
description: Hier finden Sie Informationen zu Azure Cosmos DB-Multimaster, die wichtigsten Vorteile der Georeplikation sowie Multimaster und nützliche Anwendungsfälle.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 11d70a648bfc1882753688e5352835b04cee6b9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968344"
---
# <a name="distribute-data-globally-with-azure-cosmos-db"></a>Globale Verteilung von Daten mit Azure Cosmos DB

Dieser Artikel beschreibt die wichtigsten Vorteile der globalen Verteilung von Daten sowie einige Echtzeitszenarien, in denen die globale Datenverteilung erforderlich ist.

## <a name="key-benefits"></a>Hauptvorteile

Im folgenden werden die wichtigsten Vorteile aufgeführt, die für Konten verfügbar sind, die die Azure Cosmos DB-Funktionen zur globalen Datenverteilung nutzen:

* **Einstellige Latenz**: Azure Cosmos DB bietet für Lese- und Schreibvorgänge eine Latenz von < 10 ms im 99. Perzentil, die durch [finanziell abgesicherte SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) garantiert wird.

* **Verfügbarkeit von 99,999 %**: Azure Cosmos DB bietet für Lese- und Schreibvorgänge eine Verfügbarkeit von 99,999 %, die durch [finanziell abgesicherte SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) garantiert wird.

* **Flexible Konfliktlösung**: Für Kunden, die Multimasterfunktionen nutzen, bietet Azure Cosmos DB drei Modi für die Behandlung von Konflikten, um eine globale Datenintegrität und -konsistenz sicherzustellen.

* **Verschiedene Konsistenzeinstellungen**: Azure Cosmos DB unterstützt fünf verschiedene [Konsistenzebenen](consistency-levels.md) für die globale Verteilung. Für Konten mit Multimasterfunktion werden alle Konsistenzebenen außer der starken Konsistenz unterstützt.

* **Implizite Fehlertoleranz** : Wenn Daten regionsübergreifend repliziert werden, profitieren Anwendungen von einer hohen Fehlertoleranz bei regionalen Ausfällen.

## <a name="use-cases"></a>Anwendungsfälle

Hier wird nur ein kleiner Teil der Szenarien, die von den Azure Cosmos DB-Funktionen zur globalen Verteilung profitieren können.

* **Apps für soziale Medien**: Die Anwendungen sozialer Medien erfordern niedrige Latenz sowie hohe Verfügbarkeit und Skalierbarkeit, um für Benutzer rund um den Globus eine überzeugende Umgebung zu bieten.

* **IoT** : In geografisch verteilten Edgebereitstellungen müssen häufig Zeitreihendaten von mehreren Standorten nachverfolgt werden. Jedes Gerät kann in der nächstgelegenen Region verwaltet werden. Wenn Geräte an andere Standorte bewegt werden, erfolgen deren Schreibvorgänge in die nächste verfügbare Region.

* **E-Commerce** : Für E-Commerce sind eine sehr niedrige Latenz und Hochverfügbarkeit erforderlich. Die geografische Verteilung von Daten mit Lese- und Schreibvorgängen werden Daten in die Nähe der Benutzer verlagert, wodurch sich das Reaktionsverhalten von Anwendungen beschleunigt. Durch die Verfügbarkeit sowohl für Lese- als auch Schreibvorgänge in mehreren Regionen wird eine höhere Verfügbarkeit gewährleistet.

* **Betrugs- und Anomalieerkennung** : Häufig sind Anwendungen, die Benutzer- oder Kontoaktivitäten überwachen, global verteilt und müssen mehrere Ereignisse parallel nachverfolgen, um Bewertungen zu aktualisieren und die Risikometriken aufeinander abzustimmen.

* **Messung** : Mithilfe der Multimasterunterstützung in Azure Cosmos DB können Zählung und Regulierung der Verwendung (z.B. API-Aufrufe, Transaktionen pro Sekunde oder in Anspruch genommene Minuten) mühelos global implementiert werden. Die integrierte Konfliktlösung gewährleistet die Genauigkeit der Zählungen sowie die Regulierung in Echtzeit.

## <a name="next-steps"></a>Nächste Schritte  

In diesem Artikel haben Sie die wichtigsten Vorteile und Anwendungsfälle für die globalen Verteilungsfunktionen von Azure Cosmos DB kennengelernt. Sehen Sie sich als Nächstes folgende Ressourcen an:

* [Aktivieren der schlüsselfertigen globalen Verteilung durch Azure Cosmos DB](distribute-data-globally-turnkey.md)

* [Konfigurieren der globalen Azure Cosmos DB-Datenbankreplikation](tutorial-global-distribution-sql-api.md)

* [Aktivieren von Multimaster für Azure Cosmos DB-Konten](enable-multi-master.md)

* [Funktionsweise des automatischen und manuellen Failovers in Azure Cosmos DB](regional-failover.md)

* [Grundlegendes zur Konfliktauflösung in Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Verwenden von Multimaster mit Open Source-NoSQL-Datenbanken](multi-master-oss-nosql.md)

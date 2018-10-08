---
title: Globale Verteilung von Daten mit Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie mehr über weltweite Georeplikation, Multimaster, Failover und Datenwiederherstellung mithilfe der globalen Datenbanken von Azure Cosmos DB, einem global verwalteten Datenbankdienst, der mehrere Modelle unterstützt.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 227c243d82665dc533e3bfa6a1fe3e9bb775a262
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408894"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale Datenverteilung mit Azure Cosmos DB

Azure ist allgegenwärtig: Es ist in über 50 geografischen Regionen auf der ganzen Welt präsent und breitet sich kontinuierlich weiter aus. Die weltweite Präsenz und die Multimasterunterstützung von Azure ermöglicht den Entwicklern unter anderem die problemlose Erstellung, Bereitstellung und Verwaltung global verteilter Anwendungen.

[Azure Cosmos DB](../cosmos-db/introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet sofort einsetzbare globale Verteilung, [elastische Skalierung von Durchsatz und Speicher](../cosmos-db/partition-data.md) weltweit, Lese- und Schreiblatenzen im einstelligen Millisekundenbereich beim 99. Perzentil, [klar definierte Konsistenzmodelle](consistency-levels.md) sowie garantierte Hochverfügbarkeit, gestützt durch [branchenführende, umfassende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch alle Ihre Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- oder Indexverwaltung befassen müssen.

## <a name="global-distribution-with-multi-master"></a>Globale Verteilung mit Multimasterarchitektur

Azure Cosmos DB ist ein Clouddienst, der sorgfältig konzipiert wurde, um Mehrinstanzenfähigkeit, globale Verteilung und Multimasterfunktionen für Dokument-, Schlüssel-Wert-, Graph- und Column-Family-Datenmodelle zu unterstützen.

![Azure Cosmos DB-Container, partitioniert und auf drei Regionen verteilt](./media/distribute-data-globally/global-apps.png)

**Ein einzelner Azure Cosmos DB-Container, partitioniert und auf mehrere Azure-Regionen verteilt**

Bei der Entwicklung von Azure Cosmos DB haben wir gelernt, dass die globale Verteilung nicht als Anhängsel behandelt werden darf. Und sie kann nicht einfach auf ein Datenbanksystem mit einem einzelnen Standort aufgesetzt werden. Die Funktionen einer global verteilten Datenbank gehen über die Funktionen hinaus, die von der herkömmlichen geografischen Notfallwiederherstellung (Geographical Disaster Recovery, Geo-DR) von Datenbanken mit nur einem Standort geboten werden. Bei Datenbanken mit nur einem Standort und Geo-DR-Funktion handelt es sich um eine echte Untermenge weltweit verteilter Datenbanken.

Dank der sofort einsatzbereiten globalen Verteilung von Azure Cosmos DB müssen Entwickler kein eigenes Replikationsgerüst erstellen – weder in Form des Lambda-Musters für das Datenbankprotokoll noch durch doppelte Schreibvorgänge in mehreren Regionen. Diese Verfahren werden *nicht* empfohlen, da ihre Korrektheit nicht gewährleistet werden kann und keine vernünftigen SLAs bereitgestellt werden können.

## <a id="Next Steps"></a>Nächste Schritte

* [Aktivieren der schlüsselfertigen globalen Verteilung durch Azure Cosmos DB](distribute-data-globally-turnkey.md)

* [Wichtige Vorteile der globalen Verteilung durch Azure Cosmos DB](distribute-data-globally-benefits.md)

* [Konfigurieren der globalen Azure Cosmos DB-Datenbankreplikation](tutorial-global-distribution-sql-api.md)

* [Aktivieren von Multimaster für Azure Cosmos DB-Konten](enable-multi-master.md)

* [Funktionsweise des automatischen und manuellen Failovers in Azure Cosmos DB](regional-failover.md)

* [Grundlegendes zur Konfliktauflösung in Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Verwenden von Multimaster mit Open Source-NoSQL-Datenbanken](multi-master-oss-nosql.md)

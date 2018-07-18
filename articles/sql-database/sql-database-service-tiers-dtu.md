---
title: Azure SQL-Datenbank-Dienstebenen – DTU | Microsoft-Dokumentation
description: Erfahren Sie mehr über Dienstebenen für Einzel- und Pooldatenbanken, um verschiedene Leistungsstufen und Speichergrößen bereitzustellen.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: cf17ec616819da94678f2ae4f0f0ca283f99f629
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750423"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Auswählen einer DTU-basierten Dienstebene, von Leistungsstufe und Speicherressourcen 

Diensttarife unterscheiden sich durch eine Reihe von Leistungsstufen mit einer festen Menge an integriertem Speicher, einem festen Aufbewahrungszeitraum für Sicherungen und einem festen Preis. Alle Diensttarife ermöglichen das flexible Wechseln von Leistungsstufen ohne Ausfallzeiten. Einzeldatenbanken und Pools für elastische Datenbanken werden basierend auf dem Diensttarif und der Leistungsstufe pro Stunde abgerechnet.

> [!IMPORTANT]
> Für die verwaltete SQL-Datenbank-Instanz, die sich derzeit in der öffentlichen Vorschauphase befindet, wird das DTU-basierte Kaufmodell nicht unterstützt. Weitere Informationen finden Sie unter [Verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Auswählen einer DTU-basierten Dienstebene

Die Auswahl einer Dienstebene hängt in erster Linie von den Anforderungen an Geschäftskontinuität, Speicher und Leistung ab.
||Basic|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Zielworkload|Entwicklung und Produktion|Entwicklung und Produktion|Entwicklung und Produktion||
|Betriebszeit-SLA|99,99 %|99,99 %|99,99 %|Während der Vorschau nicht zutreffend|
|Sicherungsaufbewahrung|7 Tage|35 Tage|35 Tage|
|CPU|Niedrig|Niedrig, Mittel, Hoch|Mittel, Hoch|
|E/A-Durchsatz (ungefähr) |2,5 IOPS pro DTU| 2,5 IOPS pro DTU | 48 IOPS pro DTU|
|E/A-Wartezeit (ungefähr)|5 ms (Lesen), 10 ms (Schreiben)|5 ms (Lesen), 10 ms (Schreiben)|2 ms (Lesen/Schreiben)|
|Columnstore-Indizierung |N/V|S3 und höher|Unterstützt|
|In-Memory-OLTP|N/V|N/V|Unterstützt|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Begrenzungen für Einzeldatenbank-DTUs und Speicher

Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße | 2 GB | 1 TB | 4 TB  | 
| Maximale DTU-Anzahl | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Begrenzungen für eDTUs in Pools für elastische Datenbanken, Speicher und in einem Pool zusammengefasste Datenbanken

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße pro Datenbank  | 2 GB | 1 TB | 1 TB | 
| Maximale Speichergröße pro Pool | 156 GB | 4 TB | 4 TB | 
| Maximale Anzahl von eDTUs pro Datenbank | 5 | 3000 | 4000 | 
| Maximale Anzahl von eDTUs pro Pool | 1600 | 3000 | 4000 | 
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Mehr als 1 TB Speicher im Premium-Tarif ist derzeit in allen Regionen verfügbar, mit Ausnahme von: „Vereinigtes Königreich, Norden“, „USA, Westen-Mitte“, „Vereinigtes Königreich, Süden2“, „China, Osten“, „US DoD, Mitte“, „Deutschland, Mitte“, „US DoD, Osten“, „US Gov, Südwesten“, „US Gov, Süden-Mitte“, „Deutschland, Nordosten“, „China, Norden“, „US Gov, Osten“. In anderen Regionen ist der Speicher im Premium-Tarif auf 1 TB begrenzt. Siehe [Aktuelle Einschränkungen für P11–P15](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen für Einzeldatenbanken finden Sie unter [Single database: Storage sizes and performance levels](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels) (Einzeldatenbank: Speichergrößen und Leistungsstufen).
- Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Leistungsstufen und Speichergrößen für Pools für elastische Datenbanken finden Sie unter [Elastic pool: Storage sizes and performance levels](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels) (Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen).
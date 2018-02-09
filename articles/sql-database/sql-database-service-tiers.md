---
title: Azure SQL-Datenbank-Dienst | Microsoft-Dokumentation
description: "Erfahren Sie mehr über Dienstebenen für Einzel- und Pooldatenbanken, um verschiedene Leistungsstufen und Speichergrößen bereitzustellen."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/29/2018
ms.author: carlrab
ms.openlocfilehash: af845d62b8e635449ada98cdea23f407815ffeb0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Was sind die Dienstebenen von SQL-Datenbank?

[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet die Dienstebenen **Basic**, **Standard**, **Premium** und **Premium RS** sowohl für [einzelne Datenbanken](sql-database-single-database-resources.md) als auch für [Pools für elastische Datenbanken](sql-database-elastic-pool.md). Dienstebenen unterscheiden sich in erster Linie durch eine Reihe zur Wahl stehender Leistungsstufen und Speichergrößen sowie den Preis.  Alle Dienstebenen bieten Flexibilität beim Wechsel von Leistungsstufe und Speichergröße.  Einzelne Datenbanken und Pools für elastische Datenbanken werden stündlich basierend auf Dienstebene, Leistungsstufe und Speichergröße abgerechnet.   

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene

Die Auswahl einer Dienstebene hängt in erster Linie von den Anforderungen an Geschäftskontinuität, Speicher und Leistung ab.
| | **Basic** | **Standard** |**Premium** |**Premium RS** |
| :-- | --: |--:| --:| --:| 
|Zielworkload|Entwicklung und Produktion|Entwicklung und Produktion|Entwicklung und Produktion|Workload, die Datenverlust aufgrund von Dienstfehlern bis zu 5 Minuten tolerieren kann|
|Betriebszeit-SLA|99,99 %|99,99 %|99,99 %|Während der Vorschau nicht zutreffend|
|Sicherungsaufbewahrung|7 Tage|35 Tage|35 Tage|35 Tage|
|CPU|Niedrig|Niedrig, Mittel, Hoch|Mittel, Hoch|Mittel|
|E/A-Durchsatz|Niedrig  | Mittel | Größenordnung höher als Standard|Identisch mit Premium|
|E/A-Latenz|Höher als Premium|Höher als Premium|Niedriger als Basic und Standard|Identisch mit Premium|
|Columnstore-Indizierung und In-Memory-OLTP|N/V|N/V|Unterstützt|Unterstützt|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Einschränkungen von Leistungsstufe und Speichergröße

Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Einzeldatenbanken

|  | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße* | 2 GB | 1 TB | 4 TB  | 1 TB  |
| Maximale DTU-Anzahl | 5 | 3000 | 4000 | 1000 |
||||||

### <a name="elastic-pools"></a>Pools für elastische Datenbanken

| | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße pro Datenbank*  | 2 GB | 1 TB | 1 TB | 1 TB |
| Maximale Speichergröße pro Pool* | 156 GB | 4 TB | 4 TB | 1 TB |
| Maximale Anzahl von eDTUs pro Datenbank | 5 | 3000 | 4000 | 1000 |
| Maximale Anzahl von eDTUs pro Pool | 1600 | 3000 | 4000 | 1000 |
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 100 |
||||||

> [!IMPORTANT]
> \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* Im Premium-Tarif ist derzeit eine Speicherkapazität von mehr als 1 TB in folgenden Regionen verfügbar: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „USA, Mitte“, „Frankreich, Mitte“, „Deutschland, Mitte“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“ und „Europa, Westen“. Siehe [Aktuelle Einschränkungen für P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Leistungsstufen und Speichergrößen finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](sql-database-resource-limits.md).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-resources.md).
- Informationen zu Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer SQL-Datenbanken](sql-database-elastic-pool.md).
- Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
* Informieren Sie sich über [Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)](sql-database-what-is-a-dtu.md).
* Informationen zur Überwachung der DTU-Nutzung finden Sie unter [Tipps zur Optimierung der SQL-Datenbankleistung](sql-database-troubleshoot-performance.md).


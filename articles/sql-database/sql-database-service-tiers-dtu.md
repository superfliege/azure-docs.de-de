---
title: Azure SQL-Datenbank-Dienst – DTU | Microsoft-Dokumentation
description: Erfahren Sie mehr über Dienstebenen für Einzel- und Pooldatenbanken, um verschiedene Leistungsstufen und Speichergrößen bereitzustellen.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/09/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 6e282291a6e6e219bb275dd4da91f3815590adc1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32196003"
---
# <a name="dtu-based-purchasing-model-for-azure-sql-database"></a>DTU-basiertes Kaufmodell für Azure SQL-Datenbank 


[Azure SQL-Datenbank](sql-database-technical-overview.md) verfügt über zwei Kaufmodelle für Compute-, Speicher- und E/A-Ressourcen: ein DTU-basiertes Kaufmodell und ein V-Kern-basiertes Kaufmodell (Vorschauversion). In der folgenden Tabelle und im Diagramm werden diese Kaufmodelle verglichen und gegenübergestellt.

> [!IMPORTANT]
> Informationen zum auf virtuellen Kernen basierenden Kaufmodell (Vorschau) finden Sie unter [Auf virtuellen Kernen basierendes Kaufmodell für Azure SQL Datenbank (Vorschau)](sql-database-service-tiers-vcore.md).


|**Kaufmodell**|**Beschreibung**|**Am besten geeignet für**|
|---|---|---|
|DTU-basiertes Modell|Dieses Modell basiert auf einem Paket mit Compute-, Speicher- und E/A-Ressourcen. Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).|Eignet sich am besten für Kunden, die einfache, vorkonfigurierte Ressourcenoptionen benötigen.| 
|V-Kern-basiertes Modell|Mit diesem Modell können Sie Compute- und Speicherressourcen einzeln skalieren. Außerdem können Sie den Azure-Hybridvorteil für SQL Server verwenden, um Kosten zu sparen.|Eignet sich am besten für Kunden, für die Flexibilität, Kontrolle und Transparenz im Vordergrund stehen.|
||||  

![Preismodell](./media/sql-database-service-tiers/pricing-model.png)

## <a name="dtu-based-purchasing-model"></a>DTU-basiertes Kaufmodell

Mit einer Datenbankdurchsatzeinheit (Database Throughput Unit, DTU) wird eine Mischung aus den Werten von CPU, Arbeitsspeicher, Lesevorgängen und Schreibvorgängen dargestellt. Das DTU-basierte Kaufmodell verfügt über einen Satz mit vorkonfigurierten Paketen mit Computeressourcen und integriertem Speicher, um die Anwendungsleistung auf verschiedenen Ebenen zu erhöhen. Wenn Kunden die Einfachheit eines vorkonfigurierten Pakets und von festen Monatszahlungen bevorzugen, ist das DTU-basierte Modell unter Umständen besser für ihre Anforderungen geeignet. Beim DTU-basierten Kaufmodell können Kunden zwischen den Diensttarifen **Basic**, **Standard** und **Premium** für [Einzeldatenbanken](sql-database-single-database-resources.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) wählen. Diensttarife unterscheiden sich durch eine Reihe von Leistungsstufen mit einer festen Menge an integriertem Speicher, einem festen Aufbewahrungszeitraum für Sicherungen und einem festen Preis. Alle Diensttarife ermöglichen das flexible Wechseln von Leistungsstufen ohne Ausfallzeiten. Einzeldatenbanken und Pools für elastische Datenbanken werden basierend auf dem Diensttarif und der Leistungsstufe pro Stunde abgerechnet.

> [!IMPORTANT]
> Für die verwaltete SQL-Datenbank-Instanz, die sich derzeit in der öffentlichen Vorschauphase befindet, wird das DTU-basierte Kaufmodell nicht unterstützt. Weitere Informationen finden Sie unter [Verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md). 

## <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>Auswählen eines Diensttarifs im DTU-basierten Kaufmodell

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

## <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>Grenzwerte für die Leistungsstufe und Speichergröße beim DTU-basierten Kaufmodell

Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Einzeldatenbanken

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße* | 2 GB | 1 TB | 4 TB  | 
| Maximale DTU-Anzahl | 5 | 3000 | 4000 | |
||||||

Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen für Einzeldatenbanken finden Sie unter [Single database: Storage sizes and performance levels](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels) (Einzeldatenbank: Speichergrößen und Leistungsstufen).

### <a name="elastic-pools"></a>Pools für elastische Datenbanken

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße pro Datenbank*  | 2 GB | 1 TB | 1 TB | 
| Maximale Speichergröße pro Pool* | 156 GB | 4 TB | 4 TB | 
| Maximale Anzahl von eDTUs pro Datenbank | 5 | 3000 | 4000 | 
| Maximale Anzahl von eDTUs pro Pool | 1600 | 3000 | 4000 | 
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> -  Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
> -  Im Premium-Tarif ist derzeit eine Speicherkapazität von mehr als 1 TB in folgenden Regionen verfügbar: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „USA, Mitte“, „Frankreich, Mitte“, „Deutschland, Mitte“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“ und „Europa, Westen“. Siehe [Aktuelle Einschränkungen für P11–P15](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Leistungsstufen und Speichergrößen für Pools für elastische Datenbanken finden Sie unter [Elastic pool: Storage sizes and performance levels](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) (Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen).



## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen finden Sie unter [Azure SQL Database DTU-based resource model limits](sql-database-dtu-resource-limits.md) (Azure SQL-Datenbank – Grenzwerte für DTU-basiertes Ressourcenmodell) und [Azure SQL Database vCore-based purchasing model limits (preview)](sql-database-vcore-resource-limits.md) (Azure SQL-Datenbank – Grenzwerte für V-Kern-basiertes Kaufmodell (Vorschauversion)).
- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

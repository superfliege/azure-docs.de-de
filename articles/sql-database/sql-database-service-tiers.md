---
title: Azure SQL-Datenbank-Dienst | Microsoft-Dokumentation
description: Erfahren Sie mehr über Diensttarife für Einzel- und Pooldatenbanken, um verschiedene Leistungsstufen und Speichergrößen bereitzustellen.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: a4474aec212084006becd02f317dabae6e731d98
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Wie heißen die Diensttarife von SQL-Datenbank?

[Azure SQL-Datenbank](sql-database-technical-overview.md) verfügt über zwei Kaufmodelle für Compute-, Speicher- und E/A-Ressourcen: ein DTU-basiertes Kaufmodell und ein V-Kern-basiertes Kaufmodell (Vorschauversion). In der folgenden Tabelle und im Diagramm werden diese Kaufmodelle verglichen und gegenübergestellt.

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

### <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>Auswählen eines Diensttarifs im DTU-basierten Kaufmodell

Die Auswahl eines Diensttarifs hängt in erster Linie von den Anforderungen an Geschäftskontinuität, Speicher und Leistung ab.
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

### <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>Grenzwerte für die Leistungsstufe und Speichergröße beim DTU-basierten Kaufmodell

Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).

#### <a name="single-databases"></a>Einzeldatenbanken

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße* | 2 GB | 1 TB | 4 TB  | 
| Maximale DTU-Anzahl | 5 | 3000 | 4000 | |
||||||

Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen für Einzeldatenbanken finden Sie unter [Single database: Storage sizes and performance levels](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels) (Einzeldatenbank: Speichergrößen und Leistungsstufen).

#### <a name="elastic-pools"></a>Pools für elastische Datenbanken

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße pro Datenbank*  | 2 GB | 1 TB | 1 TB | 
| Maximale Speichergröße pro Pool* | 156 GB | 4 TB | 4 TB | 
| Maximale Anzahl von eDTUs pro Datenbank | 5 | 3000 | 4000 | 
| Maximale Anzahl von eDTUs pro Pool | 1600 | 3000 | 4000 | 
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* Im Premium-Tarif ist derzeit eine Speicherkapazität von mehr als 1 TB in folgenden Regionen verfügbar: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „USA, Mitte“, „Frankreich, Mitte“, „Deutschland, Mitte“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“ und „Europa, Westen“. Siehe [Aktuelle Einschränkungen für P11–P15](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Leistungsstufen und Speichergrößen für Pools für elastische Datenbanken finden Sie unter [Elastic pool: Storage sizes and performance levels](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) (Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen).

## <a name="vcore-based-purchasing-model-preview"></a>V-Kern-basiertes Kaufmodell (Vorschauversion)

Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen angeboten. Beim V-Kern-basierten Kaufmodell (Vorschauversion) erhalten Sie Flexibilität, Kontrolle und Transparenz in Bezug auf den individuellen Ressourcenverbrauch. Außerdem können Sie die lokalen Workloadanforderungen leicht auf die Cloud übertragen. Mit diesem Modell können Sie die Bereiche Compute, Arbeitsspeicher und Speicher je nach den jeweiligen Workloadanforderungen skalieren. Beim V-Kern-basierten Kaufmodell können Kunden für [Einzeldatenbanken](sql-database-single-database-resources.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) zwischen den Diensttarifen „Universell“ und „Unternehmenskritisch“ (Vorschauversion) wählen. 

Diensttarife unterscheiden sich in Bezug auf Leistungsstufen, Auslegung auf Hochverfügbarkeit, Fehlerisolation, Speichertypen und E/A-Bereich. Der Kunde muss den erforderlichen Speicher und den Aufbewahrungszeitraum für Sicherungen separat konfigurieren. Bei der Verwendung des V-Kern-Modells ermöglichen Einzeldatenbanken und Pools für elastische Datenbanken Einsparungen von bis zu 30 Prozent mit dem [Azure-Hybridnutzungsvorteil für SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Beim V-Kern-basierten Kaufmodell zahlen Kunden für Folgendes:
- Compute (Diensttarif + Anzahl von V-Kernen + Hardwaregeneration)*
- Typ und Menge von Daten und Protokollspeicher 
- E/A-Anzahl**
- Sicherungsspeicher (RA-GRS)** 

\* In der ersten öffentlichen Vorschauversion basieren die logischen CPUs der Generation 4 auf Prozessoren vom Typ Intel E5-2673 v3 (Haswell) mit 2,4 GHz.

\*\* Während der Vorschauphase sind Sicherungen und E/A-Vorgänge für einen Zeitraum von sieben Tagen kostenlos.

> [!IMPORTANT]
> Compute, E/A-Vorgänge, Daten und Protokollspeicher werden pro Datenbank oder Pool für elastische Datenbanken berechnet. Sicherungsspeicher wird pro Datenbank berechnet. Ausführlichere Informationen zur verwalteten SQL-Datenbank-Instanz finden Sie unter [Was ist eine verwaltete Instanz (Vorschauversion)?](sql-database-managed-instance.md).

### <a name="choosing-service-tier-compute-memory-storage-and-io-resources"></a>Auswählen des Diensttarifs und von Compute-, Arbeitsspeicher-, Speicher- und E/A-Ressourcen

Durch die Umstellung auf das V-Kern-basierte Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren. Wenn Ihre Datenbank oder der Pool für elastische Datenbanken mehr als 300 DTUs verbraucht, können Sie mit der Umstellung auf V-Kerne unter Umständen Ihre Kosten senken. Sie können für die Umstellung die API Ihrer Wahl oder das Azure-Portal verwenden, ohne dass es zu Ausfallzeit kommt. Die Umstellung ist aber nicht zwingend erforderlich. Wenn das DTU-Kaufmodell Ihre Leistungs- und Geschäftsanforderungen erfüllt, sollten Sie es weiter nutzen. Falls Sie sich für die Umstellung vom DTU-Modell auf das V-Kern-Modell entscheiden, sollten Sie zum Auswählen der Leistungsstufe die folgende Faustregel verwenden: Für 100 DTUs im Standard-Tarif ist jeweils mindestens ein V-Kern erforderlich, und für 125 DTUs im Premium-Tarif ist jeweils mindestens ein V-Kern erforderlich.

Die Unterschiede zwischen diesen beiden Tarifen werden in der folgenden Tabelle verdeutlicht:

||**Universell**|**Unternehmenskritisch**|
|---|---|---|
|Am besten geeignet für:|Die meisten geschäftlichen Workloads. Ermöglicht budgetorientierte ausgewogene und skalierbare Compute- und Speicheroptionen.|Geschäftsanwendungen mit hohen E/A-Anforderungen. Ermöglicht höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.|
|Compute|1 bis 16 V-Kerne|1 bis 16 V-Kerne|
|Arbeitsspeicher|7 GB pro Kern |7 GB pro Kern |
|Speicher|Premium-Remotespeicher, 5 GB - 4 TB|Lokaler SSD-Speicher, 5 GB - 1 TB|
|E/A-Durchsatz (ungefähr)|500 IOPS pro V-Kern mit maximal 7.500 IOPS|5.000 IOPS pro Kern|
|Verfügbarkeit|1 Replikat, keine Leseskalierung|3 Replikate, 1 [Leseskalierung](sql-database-read-scale-out.md), zonenredundante Hochverfügbarkeit|
|Sicherungen|RA-GRS, 7 - 35 Tage (standardmäßig 7 Tage)|RA-GRS, 7 - 35 Tage (standardmäßig 7 Tage)*|
|In-Memory|N/V|Unterstützt|
|||

\* Während der Vorschauphase kann der Aufbewahrungszeitraum nicht konfiguriert werden und ist auf sieben Tage festgelegt.

> [!IMPORTANT]
> Verwenden Sie das DTU-basierte Kaufmodell, wenn Sie weniger als einen V-Kern mit Computekapazität benötigen.

Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen für eine Einzeldatenbank finden Sie unter [Single database: Storage sizes and performance levels](sql-database-vcore-resource-limits.md#single-database-storage-sizes-and-performance-levels) (Einzeldatenbank: Speichergrößen und Leistungsstufen) und zu Pools für elastische Datenbanken unter [Elastic pool: Storage sizes and performance levels](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) (Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen).

Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md). 

### <a name="storage-considerations"></a>Speicheraspekt

Beachten Sie Folgendes:
- Der zugeordnete Speicher wird für Datendateien (MDF) und Protokolldateien (LDF) verwendet.
- Jede Leistungsstufe unterstützt eine maximale Datenbankgröße, die standardmäßig bei 32 GB liegt.
- Wenn Sie die erforderliche Datenbankgröße (MDF-Größe) konfigurieren, werden 30% des zusätzlichen Speichers automatisch hinzugefügt, um LDF zu unterstützen.
- Sie können eine beliebige Datenbankgröße zwischen 10 GB und dem unterstützten Maximum auswählen.
 - Für Speicher vom Typ „Standard“ erhöhen bzw. verringern Sie die Größe in 10-GB-Inkrementen.
 - Für Speicher vom Typ „Premium“ erhöhen bzw. verringern Sie die Größe in 250-GB-Inkrementen.
- Im Diensttarif „Universell“ wird für `tempdb` eine angefügte SSD verwendet, und diese Speicherkosten sind im V-Kern-Preis enthalten.
- Im Diensttarif „Unternehmenskritisch“ wird für `tempdb` die angefügte SSD für MDF- und LDF-Dateien gemeinsam genutzt, und die tempDB-Speicherkosten sind im V-Kern-Preis enthalten.

> [!IMPORTANT]
> Ihnen wird der gesamte Speicher berechnet, der für MDF und LDF zugeordnet ist.

Verwenden Sie [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql) zum Überwachen der aktuellen Gesamtgröße für MDF und LDF. Verwenden Sie [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql), um die aktuelle Größe der einzelnen MDF- und LDF-Dateien zu überwachen.

### <a name="backups-and-storage"></a>Sicherungen und Speicher

Der Speicher für Datenbanksicherungen wird zugeordnet, um die Funktionen „Point-in-Time-Wiederherstellung“ (Point in Time Restore, PITR) und „Langfristige Aufbewahrung“ (Long Term Retention, LTR) von SQL-Datenbank zu unterstützen. Dieser Speicher wird für jede Datenbank separat zugeordnet und als zwei Arten von getrennten Datenbankgebühren berechnet. 

- **PITR**: Einzelne Datenbanksicherungen werden automatisch in RA-GRS-Speicher kopiert. Die Speichergröße wird dynamisch erhöht, wenn die neuen Sicherungen erstellt werden.  Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Der Speicherverbrauch richtet sich nach der Änderungsrate der Datenbank und nach der Aufbewahrungsdauer. Sie können für jede Datenbank eine separate Aufbewahrungsdauer konfigurieren, die zwischen 7 und 35 Tagen liegt. Eine Mindestspeichermenge, die der Gesamtgröße der Daten entspricht, wird kostenlos zur Verfügung gestellt. Für die meisten Datenbanken reicht diese Menge aus, um Sicherungen für sieben Tage aufzubewahren.
- **LTR**: SQL-Datenbank verfügt über eine Option zum Konfigurieren der langfristigen Aufbewahrung von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren. Wenn die LTR-Richtlinie aktiviert ist, werden diese Sicherungen automatisch in RA-GRS-Speicher gespeichert, aber Sie können steuern, wie häufig die Sicherungen kopiert werden. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Mit dieser Konfiguration wird definiert, wie viel Speicher für die LTR-Sicherungen verwendet wird. Sie können den LTR-Preisrechner verwenden, um die Kosten für den LTR-Speicher zu schätzen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

### <a name="azure-hybrid-use-benefit"></a>Azure-Vorteil bei Hybridnutzung

Beim V-Kern-basierten Kaufmodell können Sie Ihre vorhandenen Lizenzen gegen Rabattpreise für SQL-Datenbank eintauschen, indem Sie den [Azure-Hybridnutzungsvorteil für SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) verwenden. Bei diesem Azure-Vorteil können Sie Ihre lokalen SQL Server-Lizenzen nutzen, um für SQL-Datenbank bis zu 30% zu sparen, indem Sie Software Assurance für die lokalen SQL Server-Lizenzen verwenden.

![Preise](./media/sql-database-service-tiers/pricing.png)

#### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migration von Einzeldatenbanken mit Georeplikationsverknüpfungen

Die Migration vom DTU-basierten Modell zum V-Kern-basierten Modell ähnelt dem Upgrade bzw. Downgrade der Georeplikationsbeziehungen zwischen Standard- und Premium-Datenbanken. Die Georeplikation muss nicht beendet werden, aber der Benutzer muss die Sequenzierungsregeln beachten. Bei einem Upgrade müssen Sie zuerst das Upgrade für die sekundäre Datenbank und anschließend das Upgrade für die primäre Datenbank durchführen. Drehen Sie bei einem Downgrade die Reihenfolge um: Führen Sie zuerst das Downgrade für die primäre und anschließend das Downgrade für die sekundäre Datenbank durch. 

Bei Verwendung der Georeplikation zwischen zwei Pools für elastische Datenbanken wird dringend empfohlen, einen Pool als primäres Element und den anderen als sekundäres Element festzulegen. In diesem Fall sollte für die Migration von Pools für elastische Datenbanken die gleiche Anleitung verwendet werden.  Allerdings ist es technisch möglich, dass ein Pool für elastische Datenbanken sowohl primäre als auch sekundäre Datenbanken enthält. In diesem Fall sollten Sie den Pool mit der höheren Auslastung als „primär“ einstufen und die entsprechenden Sequenzierungsregeln befolgen, um die Migration richtig durchzuführen.  

Die folgende Tabelle enthält eine Anleitung für die jeweiligen Migrationsszenarien: 

|Aktueller Diensttarif|Zieldiensttarif|Migrationstyp|Benutzeraktionen|
|---|---|---|---|
|Standard|Universell|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Premium|Unternehmenskritisch|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Standard|Unternehmenskritisch|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Standard|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Premium|Universell|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Universell|Premium|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Universell|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Universell|Unternehmenskritisch|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
||||

\* Für 100 DTUs im Standard-Tarif ist jeweils mindestens ein V-Kern erforderlich, und für 125 DTUs im Premium-Tarif ist jeweils mindestens ein V-Kern erforderlich.

#### <a name="migration-of-failover-groups"></a>Migration von Failovergruppen 

Für die Migration von Failovergruppen mit mehreren Datenbanken ist eine individuelle Migration der primären und sekundären Datenbank erforderlich. Während dieses Prozesses gelten die gleichen Aspekte und Sequenzierungsregeln. Nachdem die Datenbanken auf das V-Kern-basierte Modell umgestellt wurden, bleibt die Failovergruppe mit denselben Richtlinieneinstellungen wirksam. 

#### <a name="creation-of-a-geo-replication-secondary"></a>Erstellung einer sekundären Einheit für die Georeplikation

Sie können eine sekundäre Einheit für die Georeplikation nur mit demselben Diensttarif wie für die primäre Einheit erstellen. Für eine Datenbank mit einer hohen Protokollgenerierungsrate ist es ratsam, die sekundäre Einheit mit der gleichen Leistungsstufe wie für die primäre Einheit zu erstellen. Wenn Sie eine sekundäre Einheit für die Georeplikation im Pool für elastische Datenbanken für eine einzelne primäre Datenbank erstellen, wird dringend empfohlen, dass die Einstellung `maxVCore` für den Pool mit der Leistungsstufe der primären Datenbank übereinstimmt. Bei der Erstellung einer sekundären Einheit für die Georeplikation im Pool für elastische Datenbanken einer primären Einheit in einem anderen Pool für elastische Datenbanken wird dringend empfohlen, dass die Pools über die gleichen `maxVCore`-Einstellungen verfügen.

#### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Verwenden einer Datenbankkopie zum Konvertieren einer DTU-basierten Datenbank in eine V-Kern-basierte Datenbank

Sie können eine beliebige Datenbank mit einer DTU-basierten Leistungsstufe in eine Datenbank mit einer V-Kern-basierten Leistungsstufe kopieren, ohne dass hierfür Einschränkungen oder spezielle Sequenzierungen gelten, solange die Zielleistungsstufe die maximale Datenbankgröße der Quelldatenbank unterstützt. Der Grund ist, dass die Datenbankkopie eine Momentaufnahme der Daten zum Startzeitpunkt des Kopiervorgangs erstellt und keine Datensynchronisierung zwischen Quelle und Ziel durchführt. 

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Leistungsstufen und Speichergrößen finden Sie unter [Azure SQL Database DTU-based resource model limits](sql-database-dtu-resource-limits.md) (Azure SQL-Datenbank – Grenzwerte für DTU-basiertes Ressourcenmodell) und [Azure SQL Database vCore-based purchasing model limits (preview)](sql-database-vcore-resource-limits.md) (Azure SQL-Datenbank – Grenzwerte für V-Kern-basiertes Kaufmodell (Vorschauversion)).
- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

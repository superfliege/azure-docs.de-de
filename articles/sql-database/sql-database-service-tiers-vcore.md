---
title: Azure SQL-Datenbank-Dienst – virtueller Kern | Microsoft-Dokumentation
description: Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 09/26/2018
ms.openlocfilehash: 3fb1357b7a70579fa527a896d5bd359749b10ad6
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407654"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Auswählen eines V-Kern-Diensttarifs und von Compute-, Arbeitsspeicher-, Speicher- und E/A-Ressourcen

Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren. Es ermöglicht Ihnen auch die Wahl der Hardwaregeneration:
- Gen 4: bis zu 24 logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) 2,4 GHz, virtueller Kern = 1 PP (physischer Kern), 7 GB pro Kern, angeschlossene SSD
- Gen 5: bis zu 80 logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) 2,3 GHz, V-Kern = 1 LP (Hyperthread), 5,5. GB pro Kern, schnelle eNVM-SSD

Beim virtuellen Kern-Modell können Sie außerdem den [Vorteil der Verwendung von Azure-Hybrid für SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) nutzen, um Kosten zu sparen.

## <a name="service-tier-characteristics"></a>Merkmale des Diensttarifs

Das Modell mit virtuellem Kern bietet zwei Diensttarife: „Universell“und „Unternehmenskritisch“. Diensttarife unterscheiden sich in Bezug auf Computegrößen, Auslegung auf Hochverfügbarkeit, Fehlerisolation, Speichertypen und E/A-Bereich. Der Kunde muss den erforderlichen Speicher und den Aufbewahrungszeitraum für Sicherungen separat konfigurieren.

Die Unterschiede zwischen diesen beiden Tarifen werden in der folgenden Tabelle verdeutlicht:

||**Allgemeiner Zweck**|**Unternehmenskritisch**|**Hyperscale (Vorschau)**|
|---|---|---|---|
|Am besten geeignet für:|Die meisten geschäftlichen Workloads. Ermöglicht budgetorientierte ausgewogene und skalierbare Compute- und Speicheroptionen.|Geschäftsanwendungen mit hohen E/A-Anforderungen. Ermöglicht höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.|Die meisten geschäftlichen Workloads mit hohen Anforderungen an skalierbaren Speicher und Leseskalierung|
|Compute|Gen4: 1 bis 24 virtuelle Kerne<br/>Gen5: 1 bis 80 virtuelle Kerne|Gen4: 1 bis 24 virtuelle Kerne<br/>Gen5: 1 bis 80 virtuelle Kerne|Gen4: 1 bis 24 virtuelle Kerne<br/>Gen5: 1 bis 80 virtuelle Kerne|
|Arbeitsspeicher|Gen4: 7 GB pro Kern<br>Gen5: 5,5 GB pro Kern | Gen4: 7 GB pro Kern<br>Gen5: 5,5 GB pro Kern |Gen4: 7 GB pro Kern<br>Gen5: 5,5 GB pro Kern|
|Speicher|[Storage Premium (Remote)](../virtual-machines/windows/premium-storage.md),<br/>Einzeldatenbank: 5 GB bis 4 TB<br/>Verwaltete Instanz: 32 GB bis 8 TB |Lokaler SSD-Speicher,<br/>Einzeldatenbank: 5 GB bis 4 TB<br/>Verwaltete Instanz: 32 GB bis 4 TB |Flexibel, automatische Speichervergrößerung nach Bedarf. Unterstützt bis zu 100 TB Speicher und mehr. Lokaler SSD-Speicher für den lokalen Pufferpoolcache und den lokalen Datenspeicher. Azure-Remotespeicher als endgültiger langfristiger Datenspeicher. |
|E/A-Durchsatz (ungefähr)|Einzeldatenbank: 500 IOPS pro virtuellem Kern (maximal 7.000 IOPS)</br>Verwaltete Instanz: abhängig von der [Dateigröße](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOPS pro Kern mit maximal 200.000 IOPS|TBD|
|Verfügbarkeit|1 Replikat, keine Leseskalierung|3 Replikate, 1 [Replikat, Leseskalierung](sql-database-read-scale-out.md),<br/>Zonenredundante HA|?|
|Backups|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 Tage (standardmäßig 7 Tage)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 Tage (standardmäßig 7 Tage)|Sicherung auf der Grundlage von Momentaufnahmen im Azure-Remotespeicher. Diese Momentaufnahmen werden für die schnelle Wiederherstellung verwendet. Sicherungen werden sofort ausgeführt und haben keine Auswirkungen auf die E/A-Computeleistung. Wiederherstellungen sind sehr schnell und entsprechen nicht der Größe von Datenvorgängen (in Minuten nicht in Stunden/Tagen).|
|In-Memory|Nicht unterstützt|Unterstützt|Nicht unterstützt|
|||

Weitere Informationen finden Sie unter [V-Kern-Ressourcengrenzwerte in Einzeldatenbank](sql-database-vcore-resource-limits-single-databases.md) und [V-Kern-Ressourcengrenzwerte in einer verwalteten Instanz](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> Verwenden Sie das DTU-basierte Kaufmodell, wenn Sie weniger als einen V-Kern mit Computekapazität benötigen.

Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md). 

## <a name="storage-considerations"></a>Speicheraspekt

### <a name="general-purpose-and-business-critical-service-tiers"></a>Diensttarife „Universell“ und „Unternehmenskritisch“
Beachten Sie Folgendes:
- Der zugeordnete Speicher wird für Datendateien (MDF) und Protokolldateien (LDF) verwendet.
- Jede Einzeldatenbank-Computegröße unterstützt eine maximale Datenbankgröße, die standardmäßig bei 32 GB liegt.
- Wenn Sie die erforderliche Einzeldatenbankgröße (MDF-Größe) konfigurieren, werden automatisch 30 Prozent zusätzlicher Speicher hinzugefügt, um LDF zu unterstützen.
- Die Speichergröße in einer verwalteten Azure SQL-Datenbank-Instanz muss als Vielfaches von 32 GB angegeben werden.
- Sie können eine beliebige Einzeldatenbankgröße zwischen 10 GB und dem unterstützten Maximum auswählen.
 - Für Speicher vom Typ „Standard“ erhöhen bzw. verringern Sie die Größe in Schritten von 10 GB.
 - Für Speicher vom Typ „Premium“ erhöhen bzw. verringern Sie die Größe in Schritten von 250 GB.
- Im Diensttarif „Universell“ wird für `tempdb` eine angefügte SSD verwendet, und diese Speicherkosten sind im V-Kern-Preis enthalten.
- Im Diensttarif „Unternehmenskritisch“ wird für `tempdb` die angefügte SSD für MDF- und LDF-Dateien gemeinsam genutzt, und die tempDB-Speicherkosten sind im V-Kern-Preis enthalten.

> [!IMPORTANT]
> Ihnen wird der gesamte Speicher berechnet, der für MDF und LDF zugeordnet ist.

Verwenden Sie [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql) zum Überwachen der aktuellen Gesamtgröße für MDF und LDF. Verwenden Sie [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql), um die aktuelle Größe der einzelnen MDF- und LDF-Dateien zu überwachen.

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

### <a name="hyperscale-service-tier-preview"></a>Diensttarif „Hyperscale“ (Vorschau)

Der Speicher wird für die Datenbank mit Hyperskalierung automatisch verwaltet. Der Speicher wächst nach Bedarf. „Unendlicher“ Protokollspeicher auf hochwertigen schnellen Azure Speicher-SSDs, wobei keine häufige Protokollkürzung erforderlich ist.

## <a name="backups-and-storage"></a>Sicherungen und Speicher

### <a name="general-purpose-and-business-critical-service-tiers"></a>Diensttarife „Universell“ und „Unternehmenskritisch“

Der Speicher für Datenbanksicherungen wird zugeordnet, um die Funktionen „Point-in-Time-Wiederherstellung“ (Point in Time Restore, PITR) und „[Langfristige Aufbewahrung](sql-database-long-term-retention.md)“ (Long Term Retention, LTR) von SQL-Datenbank zu unterstützen. Dieser Speicher wird für jede Datenbank separat zugeordnet und als zwei Arten von getrennten Datenbankgebühren berechnet. 

- **PITR**: Einzelne Datenbanksicherungen werden automatisch in [RA-GRS-Speicher](../storage/common/storage-designing-ha-apps-with-ragrs.md) kopiert. Die Speichergröße wird dynamisch erhöht, wenn die neuen Sicherungen erstellt werden.  Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Der Speicherverbrauch richtet sich nach der Änderungsrate der Datenbank und nach der Aufbewahrungsdauer. Sie können für jede Datenbank eine separate Aufbewahrungsdauer konfigurieren, die zwischen 7 und 35 Tagen liegt. Eine Mindestspeichermenge, die der Gesamtgröße der Daten entspricht, wird kostenlos zur Verfügung gestellt. Für die meisten Datenbanken reicht diese Menge aus, um Sicherungen für sieben Tage aufzubewahren.
- **LTR**: SQL-Datenbank verfügt über eine Option zum Konfigurieren der langfristigen Aufbewahrung von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren. Wenn die LTR-Richtlinie aktiviert ist, werden diese Sicherungen automatisch in RA-GRS-Speicher gespeichert, aber Sie können steuern, wie häufig die Sicherungen kopiert werden. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Mit dieser Konfiguration wird definiert, wie viel Speicher für die LTR-Sicherungen verwendet wird. Sie können den LTR-Preisrechner verwenden, um die Kosten für den LTR-Speicher zu schätzen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

### <a name="hyperscale-service-tier-preview"></a>Diensttarif „Hyperscale“ (Vorschau)

Im Diensttarif „Hyperscale“ basieren Sicherungen auf Momentaufnahmen und werden im Azure-Remotespeicher gespeichert. Bei Wiederherstellungen werden diese Momentaufnahmen zur schnellen Wiederherstellung verwendet. Sicherungen werden sofort ausgeführt und haben keine Auswirkungen auf die E/A-Computeleistung. Wiederherstellungen sind sehr schnell und entsprechen nicht der Größe von Datenvorgängen (in Minuten nicht in Stunden/Tagen).

## <a name="azure-hybrid-use-benefit"></a>Azure-Vorteil bei Hybridnutzung

Beim V-Kern-basierten Kaufmodell können Sie Ihre vorhandenen Lizenzen gegen Rabattpreise für SQL-Datenbank eintauschen, indem Sie den [Azure-Hybridnutzungsvorteil für SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) verwenden. Bei diesem Azure-Vorteil können Sie Ihre lokalen SQL Server-Lizenzen nutzen, um für SQL-Datenbank bis zu 30% zu sparen, indem Sie Software Assurance für die lokalen SQL Server-Lizenzen verwenden.

![Preise](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migration vom DTU-Modell zum Modell mit virtuellem Kern

### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migration von Einzeldatenbanken mit Georeplikationsverknüpfungen

Die Migration vom DTU-basierten Modell zum V-Kern-basierten Modell ähnelt dem Upgrade bzw. Downgrade der Georeplikationsbeziehungen zwischen Standard- und Premium-Datenbanken. Die Georeplikation muss nicht beendet werden, aber der Benutzer muss die Sequenzierungsregeln beachten. Bei einem Upgrade müssen Sie zuerst das Upgrade für die sekundäre Datenbank und anschließend das Upgrade für die primäre Datenbank durchführen. Drehen Sie bei einem Downgrade die Reihenfolge um: Führen Sie zuerst das Downgrade für die primäre und anschließend das Downgrade für die sekundäre Datenbank durch. 

Bei Verwendung der Georeplikation zwischen zwei Pools für elastische Datenbanken wird empfohlen, einen Pool als primäres Element und den anderen als sekundäres Element festzulegen. In diesem Fall sollte für die Migration von Pools für elastische Datenbanken die gleiche Anleitung verwendet werden.  Allerdings ist es technisch möglich, dass ein Pool für elastische Datenbanken sowohl primäre als auch sekundäre Datenbanken enthält. In diesem Fall sollten Sie den Pool mit der höheren Auslastung als „primär“ einstufen und die entsprechenden Sequenzierungsregeln befolgen, um die Migration richtig durchzuführen.  

Die folgende Tabelle enthält eine Anleitung für die jeweiligen Migrationsszenarien: 

|Aktueller Diensttarif|Zieldiensttarif|Migrationstyp|Benutzeraktionen|
|---|---|---|---|
|Standard|Allgemeiner Zweck|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Premium|Unternehmenskritisch|Seitwärts|Die Migration ist in einer beliebigen Reihenfolge möglich, aber Sie müssen für eine geeignete V-Kern-Größe sorgen*|
|Standard|Unternehmenskritisch|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Standard|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Premium|Allgemeiner Zweck|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Allgemeiner Zweck|Premium|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
|Unternehmenskritisch|Allgemeiner Zweck|Downgrade|Primäre Einheit muss zuerst migriert werden|
|Allgemeiner Zweck|Unternehmenskritisch|Upgrade|Sekundäre Einheit muss zuerst migriert werden|
||||

\* Für 100 DTUs im Standard-Tarif ist jeweils mindestens ein V-Kern erforderlich, und für 125 DTUs im Premium-Tarif ist jeweils mindestens ein V-Kern erforderlich.

### <a name="migration-of-failover-groups"></a>Migration von Failovergruppen 

Für die Migration von Failovergruppen mit mehreren Datenbanken ist eine individuelle Migration der primären und sekundären Datenbank erforderlich. Während dieses Prozesses gelten die gleichen Aspekte und Sequenzierungsregeln. Nachdem die Datenbanken auf das V-Kern-basierte Modell umgestellt wurden, bleibt die Failovergruppe mit denselben Richtlinieneinstellungen wirksam. 

### <a name="creation-of-a-geo-replication-secondary"></a>Erstellung einer sekundären Einheit für die Georeplikation

Sie können eine sekundäre Einheit für die Georeplikation nur mit demselben Diensttarif wie für die primäre Einheit erstellen. Für eine Datenbank mit einer hohen Protokollgenerierungsrate ist es ratsam, die sekundäre Einheit mit der gleichen Computegröße wie für die primäre Einheit zu erstellen. Wenn Sie eine sekundäre Einheit für die Georeplikation im Pool für elastische Datenbanken für eine einzelne primäre Datenbank erstellen, wird empfohlen, dass die Einstellung `maxVCore` für den Pool mit der Computegröße der primären Datenbank übereinstimmt. Bei der Erstellung einer sekundären Einheit für die Georeplikation im Pool für elastische Datenbanken einer primären Einheit in einem anderen Pool für elastische Datenbanken wird empfohlen, dass die Pools über die gleichen `maxVCore`-Einstellungen verfügen.

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Verwenden einer Datenbankkopie zum Konvertieren einer DTU-basierten Datenbank in eine V-Kern-basierte Datenbank

Sie können eine beliebige Datenbank mit einer DTU-basierten Computegröße in eine Datenbank mit einer V-Kern-basierten Computegröße kopieren, ohne dass hierfür Einschränkungen oder spezielle Sequenzierungen gelten, solange die Zielcomputegröße die maximale Datenbankgröße der Quelldatenbank unterstützt. Der Grund ist, dass die Datenbankkopie eine Momentaufnahme der Daten zum Startzeitpunkt des Kopiervorgangs erstellt und keine Datensynchronisierung zwischen Quelle und Ziel durchführt. 

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Einzeldatenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Computegrößen und Speichergrößen für Pools für elastische Datenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).

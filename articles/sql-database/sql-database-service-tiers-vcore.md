---
title: Azure SQL-Datenbank-Dienst – virtueller Kern | Microsoft-Dokumentation
description: Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: edba858f9be3350034ff48ea16d3c9137254bb97
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59357951"
---
# <a name="vcore-service-tiers-azure-hybrid-benefit-and-migration"></a>V-Kern-Diensttarife, Azure-Hybridvorteil und Migration

Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren. Es ermöglicht Ihnen auch die Wahl der Hardwaregeneration:

- Gen 4: bis zu 24 logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) 2,4 GHz, virtueller Kern = 1 PP (physischer Kern), 7 GB pro Kern, angeschlossene SSD
- Gen 5: bis zu 80 logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) 2,3 GHz, V-Kern = 1 LP (Hyperthread), 5,1 GB pro Kern, schnelle eNVM-SSD

Gen4-Hardware verfügt über deutlich mehr Arbeitsspeicher pro V-Kern. Bei Gen5-Hardware können die Computeressourcen aber viel stärker zentral hochskaliert werden.

> [!NOTE]
> Weitere Informationen zu DTU-basierten Diensttarifen finden Sie unter [DTU-basierte Diensttarife](sql-database-service-tiers-dtu.md). Weitere Informationen zu den Unterschieden zwischen DTU-basierten Diensttarifen und V-Kern-basierten Diensttarifen finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Merkmale des Diensttarifs

Das Modell mit virtuellem Kern bietet drei Diensttarife: „Universell“, Hyperscale“ und „Unternehmenskritisch“. Diensttarife unterscheiden sich in Bezug auf Computegrößen, Auslegung auf Hochverfügbarkeit, Fehlerisolation, Speichertypen und -größe sowie E/A-Bereich. Sie müssen den erforderlichen Speicher und den Aufbewahrungszeitraum für Sicherungen separat konfigurieren. Navigieren Sie im Azure-Portal zu „Server“ (nicht zu „Datenbank“) > Verwaltete Sicherungen > Richtlinie konfigurieren > Konfiguration der Point-in-Time-Wiederherstellung > 7–35 Tage.

Die folgende Tabelle zeigt die Unterschiede zwischen diesen drei Tarifen:

||**Allgemeiner Zweck**|**Unternehmenskritisch**|**Hyperscale (Vorschau)**|
|---|---|---|---|
|Am besten geeignet für:|Die meisten geschäftlichen Workloads. Ermöglicht budgetorientierte ausgewogene und skalierbare Compute- und Speicheroptionen.|Geschäftsanwendungen mit hohen E/A-Anforderungen. Ermöglicht höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.|Die meisten geschäftlichen Workloads mit hohen Anforderungen an skalierbaren Speicher und Leseskalierung|
|Compute|Gen4: 1 bis 24 V-Kerne<br/>Gen5: 1 bis 80 V-Kerne|Gen4: 1 bis 24 V-Kerne<br/>Gen5: 1 bis 80 V-Kerne|Gen4: 1 bis 24 V-Kerne<br/>Gen5: 1 bis 80 V-Kerne|
|Arbeitsspeicher|Gen4: 7 GB pro Kern<br>Gen5: 5,1 GB pro Kern | Gen4: 7 GB pro Kern<br>Gen5: 5,1 GB pro Kern |Gen4: 7 GB pro Kern<br>Gen5: 5,1 GB pro Kern|
|Storage|Verwendet Remotespeicher:<br/>Einzeldatenbank: 5 GB – 4 TB<br/>Verwaltete Instanz: 32 GB – 8 TB |Verwendet lokalen SSD-Speicher:<br/>Einzeldatenbank: 5 GB – 4 TB<br/>Verwaltete Instanz: 32 GB – 4 TB |Flexibel, automatische Speichervergrößerung nach Bedarf. Unterstützt bis zu 100 TB Speicher und mehr. Lokaler SSD-Speicher für den lokalen Pufferpoolcache und den lokalen Datenspeicher. Azure-Remotespeicher als endgültiger langfristiger Datenspeicher. |
|E/A-Durchsatz (ungefähr)|Einzeldatenbank: 500 IOPS pro V-Kern mit maximal 7.000 IOPS</br>Verwaltete Instanz: Hängt von der [Größe der Datei](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) ab|5000 IOPS pro Kern mit maximal 200.000 IOPS|TBD|
|Verfügbarkeit|1 Replikat, keine Leseskalierung|3 Replikate, 1 [Replikat, Leseskalierung](sql-database-read-scale-out.md),<br/>Zonenredundante HA|?|
|Backups|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 Tage (standardmäßig 7 Tage)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 Tage (standardmäßig 7 Tage)|Sicherung auf der Grundlage von Momentaufnahmen im Azure-Remotespeicher. Diese Momentaufnahmen werden für die schnelle Wiederherstellung verwendet. Sicherungen werden sofort ausgeführt und haben keine Auswirkungen auf die E/A-Computeleistung. Wiederherstellungen sind sehr schnell und entsprechen nicht der Größe eines Datenvorgangs (benötigen eher Minuten als Stunden oder Tage).|
|In-Memory|Nicht unterstützt|Unterstützt|Nicht unterstützt|
|||

> [!NOTE]
> Sie können eine kostenlose Azure SQL-Datenbank mit dem Diensttarif „Basic“ in Verbindung mit einem kostenlosen Azure-Konto erhalten, um Azure zu erkunden. Weitere Informationen finden Sie unter [Mit dem kostenlosen Azure-Konto eine verwaltete Clouddatenbank erstellen](https://azure.microsoft.com/free/services/sql-database/).

- Weitere Informationen finden Sie unter [V-Kern-Ressourcengrenzwerte in Einzeldatenbank](sql-database-vcore-resource-limits-single-databases.md) und [V-Kern-Ressourcengrenzwerte in einer verwalteten Instanz](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Weitere Informationen zu den Diensttarifen „Universell“ und „Unternehmenskritisch“ finden Sie unter [Diensttarife „Universell“ und „Unternehmenskritisch“](sql-database-service-tiers-general-purpose-business-critical.md).
- Ausführliche Informationen zum Diensttarif „Hyperscale“ im V-Kern-basierten Kaufmodell finden Sie unter [Diensttarif „Hyperscale“](sql-database-service-tier-hyperscale.md).  

> [!IMPORTANT]
> Verwenden Sie das DTU-basierte Kaufmodell, wenn Sie weniger als einen V-Kern mit Computekapazität benötigen.

## <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Beim V-Kern-basierten Kaufmodell können Sie Ihre vorhandenen Lizenzen gegen Rabattpreise für SQL-Datenbank tauschen, indem Sie den [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) verwenden. Bei diesem Azure-Vorteil können Sie Ihre lokalen SQL Server-Lizenzen nutzen, um für SQL-Datenbank bis zu 30% zu sparen, indem Sie Software Assurance für die lokalen SQL Server-Lizenzen verwenden.

![Preise](./media/sql-database-service-tiers/pricing.png)

Mit dem Azure-Hybridvorteil bezahlen Sie wahlweise nur die zugrunde liegende Azure-Infrastruktur und verwenden Ihre vorhandene SQL Server-Lizenz für die eigentliche SQL-Datenbank-Engine (**BasePrice**), oder Sie bezahlen für die zugrunde liegende Infrastruktur und für die SQL Server-Lizenz (**LicenseIncluded**). Sie können Ihr Lizenzierungsmodell im Azure-Portal oder mithilfe einer der folgenden APIs auswählen oder ändern.

- So legen Sie mithilfe von PowerShell den Lizenztyp fest oder aktualisieren ihn

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase):
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql)

- So legen Sie mithilfe der Azure-Befehlszeilenschnittstelle den Lizenztyp fest oder aktualisieren ihn

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- So legen Sie mithilfe der REST-API den Lizenztyp fest oder aktualisieren ihn

  - [Datenbanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Datenbanken – Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Verwaltete Instanzen – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Verwaltete Instanzen – Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migration vom DTU-Modell zum Modell mit virtuellem Kern

### <a name="migration-of-a-database"></a>Migrieren einer Datenbank

Die Migration einer Datenbank vom DTU-basierten Kaufmodell zum V-Kern-basierten Kaufmodell ist vergleichbar mit einem Up- oder Downgrade zwischen den Datenbankebenen „Standard“ und „Premium“ im DTU-basierten Kaufmodell.

### <a name="migration-of-databases-with-geo-replication-links"></a>Migrieren von Datenbanken mit Georeplikationsverknüpfungen

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

Sie können eine beliebige Datenbank mit einer DTU-basierten Computegröße in eine Datenbank mit einer V-Kern-basierten Computegröße kopieren, ohne dass hierfür Einschränkungen oder spezielle Sequenzierungen gelten, solange die Zielcomputegröße die maximale Datenbankgröße der Quelldatenbank unterstützt. Die Datenbankkopie erstellt eine Momentaufnahme der Daten zum Startzeitpunkt des Kopiervorgangs und führt keine Datensynchronisierung zwischen Quelle und Ziel durch.

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Einzeldatenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
- Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Compute- und Speichergrößen für Pools für elastische Datenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).

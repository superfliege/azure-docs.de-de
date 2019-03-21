---
title: Azure SQL-Datenbank – „Universell“ und „Unternehmenskritisch“ | Microsoft-Dokumentation
description: In diesem Artikel werden die Diensttarife „Universell“ und „Unternehmenskritisch“ im V-Kern-Kaufmodell erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 02/23/2019
ms.openlocfilehash: db91e67254e9927caba68a84b38a1ee94da8d3ab
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750122"
---
# <a name="azure-sql-database-service-tiers"></a>Dienstebenen für Azure SQL-Datenbank

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um die Verfügbarkeit von 99,99 % selbst bei Infrastrukturausfällen sicherzustellen. In Azure SQL-Datenbank werden drei Architekturmodelle verwendet:

- [Universell](sql-database-service-tier-general-purpose.md): ist für die meisten generischen Workloads konzipiert.
- [Unternehmenskritisch](sql-database-service-tier-business-critical.md): ist für Workloads mit geringer Latenz mit einem lesbaren Replikat konzipiert.
- [Hochgradig skalierbaren](sql-database-service-tier-hyperscale.md): ist für sehr große Datenbanken (bis zu 100 TB) mit mehreren lesbaren Replikaten konzipiert.

Dieser Artikel beschreibt die Speicher- und Sicherungsüberlegungen für die Diensttarife „Universell“ und „Unternehmenskritisch“ im V-Kern-basierten Kaufmodell.

> [!NOTE]
> Ausführliche Informationen zum Diensttarif „Hyperscale“ im V-Kern-basierten Kaufmodell finden Sie unter [Diensttarif „Hyperscale“](sql-database-service-tier-hyperscale.md). Einen Vergleich zwischen V-Kern-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Daten- und Protokollspeicher

Beachten Sie Folgendes:

- Der zugeordnete Speicher wird für Datendateien (MDF) und Protokolldateien (LDF) verwendet.
- Jede Einzeldatenbank-Computegröße unterstützt eine maximale Datenbankgröße, die standardmäßig bei 32 GB liegt.
- Wenn Sie die erforderliche Einzeldatenbankgröße (MDF-Größe) konfigurieren, werden automatisch 30 Prozent zusätzlicher Speicher hinzugefügt, um LDF zu unterstützen.
- Die Speichergröße in einer verwalteten Azure SQL-Datenbank-Instanz muss als Vielfaches von 32 GB angegeben werden.
- Sie können eine beliebige Einzeldatenbankgröße zwischen 10 GB und dem unterstützten Maximum auswählen.
  - In den Diensttarifen „Standard“ oder „Universell“ erhöhen oder verringern Sie die Speichergröße in Schritten von 10 GB.
  - In den Diensttarifen „Premium“ oder „Unternehmenskritisch“ erhöhen oder verringern Sie die Speichergröße in Schritten von 250 GB
- Im Diensttarif „Universell“ wird für `tempdb` eine angefügte SSD verwendet, und diese Speicherkosten sind im V-Kern-Preis enthalten.
- Im Diensttarif „Unternehmenskritisch“ wird für `tempdb` die angefügte SSD für MDF- und LDF-Dateien gemeinsam genutzt, und die tempDB-Speicherkosten sind im V-Kern-Preis enthalten.

> [!IMPORTANT]
> Ihnen wird der gesamte Speicher berechnet, der für MDF und LDF zugeordnet ist.

Verwenden Sie [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql) zum Überwachen der aktuellen Gesamtgröße für MDF und LDF. Verwenden Sie [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql), um die aktuelle Größe der einzelnen MDF- und LDF-Dateien zu überwachen.

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Sicherungen und Speicher

Der Speicher für Datenbanksicherungen wird zugeordnet, um die Funktionen „Point-in-Time-Wiederherstellung“ (Point in Time Restore, PITR) und „[Langfristige Aufbewahrung](sql-database-long-term-retention.md)“ (Long Term Retention, LTR) von SQL-Datenbank zu unterstützen. Dieser Speicher wird für jede Datenbank separat zugeordnet und als zwei Arten von getrennten Datenbankgebühren berechnet.

- **PITR**: Einzelne Datenbanksicherungen werden automatisch in [RA-GRS-Speicher](../storage/common/storage-designing-ha-apps-with-ragrs.md) kopiert. Die Speichergröße wird dynamisch erhöht, wenn die neuen Sicherungen erstellt werden.  Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Der Speicherverbrauch richtet sich nach der Änderungsrate der Datenbank und nach der Aufbewahrungsdauer. Sie können für jede Datenbank eine separate Aufbewahrungsdauer konfigurieren, die zwischen 7 und 35 Tagen liegt. Eine Mindestspeichermenge, die der Gesamtgröße der Daten entspricht, wird kostenlos zur Verfügung gestellt. Für die meisten Datenbanken reicht diese Menge aus, um Sicherungen für sieben Tage aufzubewahren.
- **LTR**: SQL-Datenbank verfügt über eine Option zum Konfigurieren der langfristigen Aufbewahrung von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren. Wenn die LTR-Richtlinie aktiviert ist, werden diese Sicherungen automatisch in RA-GRS-Speicher gespeichert, aber Sie können steuern, wie häufig die Sicherungen kopiert werden. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Mit dieser Konfiguration wird definiert, wie viel Speicher für die LTR-Sicherungen verwendet wird. Sie können den LTR-Preisrechner verwenden, um die Kosten für den LTR-Speicher zu schätzen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Einzeldatenbanken in den Diensttarifen „Universell“ und „Unternehmenskritisch“ finden Sie unter [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Pools für elastische Datenbanken in den Diensttarifen „Universell“ und „Unternehmenskritisch“ finden Sie unter [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).

---
title: 'Azure SQL-Datenbank: Funktionsvergleich | Microsoft-Dokumentation'
description: Dieser Artikel vergleicht die Features von SQL Server, die in verschiedenen Varianten von Azure SQL-Datenbank verfügbar sind.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, sstein
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: e1c15b78b93c638c8941356319de2c5e17712795
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358273"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Funktionsvergleich: Azure SQL-Datenbank und SQL Server

Azure SQL-Datenbank verwendet eine gemeinsame Codebasis mit SQL Server. Die von Azure SQL-Datenbank unterstützen Features von SQL Server hängen vom Typ der Azure SQL-Datenbank ab, die Sie erstellen. Mit Azure SQL-Datenbank können Sie eine Datenbank als Teil einer [verwalteten Instanz](sql-database-managed-instance.md), als Einzeldatenbank oder als Teil eines Pools für elastische Datenbanken erstellen.

Microsoft fügt Azure SQL-Datenbank ständig weitere Features hinzu. Besuchen Sie die Webseite mit Dienstupdates für Azure, und rufen Sie mithilfe dieser Filter die neuesten Updates ab:

- Gefiltert nach [SQL-Datenbank](https://azure.microsoft.com/updates/?service=sql-database).
- Gefiltert nach allgemeiner Verfügbarkeit [Ankündigungen](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Unterstützung des SQL Server-Features in der Azure SQL-Datenbank

Die folgende Tabelle enthält die wichtigsten Features von SQL Server und gibt Aufschluss darüber, ob die einzelnen Features teilweise oder vollständig unterstützt werden. Darüber hinaus enthält die Tabelle jeweils einen Link zu weiteren Featureinformationen.

| **SQL-Funktionen** | **Unterstützt von Singletons und Pools für elastische Datenbanken** | **Unterstützt von verwalteten Instanzen** |
| --- | --- | --- |
| [Aktive Georeplikation](sql-database-active-geo-replication.md) | Ja – alle Dienstebenen außer Hyperscale | Nein, siehe [Autofailover-Gruppen](sql-database-auto-failover-group.md) |
| [Autofailover-Gruppen](sql-database-auto-failover-group.md) | Ja – alle Dienstebenen außer Hyperscale | Ja, in der [Public Preview](sql-database-auto-failover-group.md)|
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja (siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)) | Ja (siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)) |
| [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [Anfügen einer Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nein  | Nein  |
| [Anwendungsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja | Ja |
|[Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)| [Ja](sql-database-managed-instance-auditing.md) |
| [Automatische Sicherungen](sql-database-automated-backups.md) | Ja | Ja |
| [Automatische Optimierung (Planerzwingung)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| [Ja](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatische Optimierung (Indizes)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| Nein  |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Ja | Ja |
| [BACPAC Datei (exportieren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja: siehe [Export der SQL-Datenbank](sql-database-export.md) | Ja |
| [BACPAC Datei (importieren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja: siehe [Import der SQL-Datenbank](sql-database-import.md) | Ja |
| [BACKUP-Befehl](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nein, nur vom System initiierte automatische Sicherungen – siehe [Automatisierte Sicherungen](sql-database-automated-backups.md) | Vom System initiierte automatisierte Sicherungen und benutzerinitiierte Kopiesicherungen – siehe [Sicherung](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Integrierte Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Erfassung geänderter Daten](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nein  | Ja |
| [Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |Ja |
| [Sortierung – Datenbank](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Ja | Ja |
| [Sortierung – Server/Instanz](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Nein  | Ja, in der [Public Preview](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| [ColumnStore-Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja – [Premium-Tarif, Standard-Tarif – mindestens S3, Tarif „Universell“ und Tarif „Unternehmenskritisch“](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Ja |
| [Common Language Runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nein  | Ja – siehe [CLR](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Eigenständige Datenbanken](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja | Nein [aufgrund eines Fehlers in RESTORE einschließlich Point-in-Time-Wiederherstellung](sql-database-managed-instance-transact-sql-information.md#cannot-restore-contained-database) |
| [Enthaltene Benutzer](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja | Ja |
| [Schlüsselwörter der Sprache zur Ablaufsteuerung](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja | Ja |
| [Datenbankübergreifende Abfragen](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein – siehe [Elastische Abfragen](sql-database-elastic-query-overview.md) | Ja, plus [elastische Abfragen](sql-database-elastic-query-overview.md) |
| [Datenbankübergreifende Transaktionen](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein  | Ja, innerhalb der Instanz. Informationen zu instanzübergreifenden Abfragen finden Sie unter [Verknüpfte Server](sql-database-managed-instance-transact-sql-information.md#linked-servers). |
| [Cursor](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja |Ja |
| [Datenkomprimierung](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |Ja |
| [Datenbank-E-Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nein  | Ja |
| [Database Migration Service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Ja | Ja |
| [Datenbankspiegelung](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nein  | Nein  |
| [Konfigurationseinstellungen für Datenbank](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nein  | Nein  |
| [Datenbank-Momentaufnahmen](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nein  | Nein  |
| [Datentypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |Ja |
| [DBCC-Anweisungen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [DBCC](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/statements) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Nur Datenbank |  Ja |
| [Verteilte Partitionsansichten](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nein  | Ja |
| [Verteilte Transaktionen – MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nein (siehe [Elastische Transaktionen](sql-database-elastic-transactions-overview.md)) |  Nein – siehe [Verknüpfte Server](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [DML-Anweisungen](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja | Ja |
| [DML-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Die meisten (siehe einzelne Anweisungen) |  Ja |
| [DMVs](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Die meisten – siehe einzelne DMVs |  Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
|[Dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)| [Ja](sql-database-dynamic-data-masking-get-started.md) |
| [Pools für elastische Datenbanken](sql-database-elastic-pool.md) | Ja | Integriert: Eine einzelne verwaltete Instanz kann mehrere Datenbanken aufweisen, die denselben Ressourcenpool gemeinsam nutzen. |
| [Ereignisbenachrichtigungen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nein (siehe [Warnungen](sql-database-insights-alerts-portal.md)) | Nein  |
| [Ausdrücke](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja | Ja |
| [Erweiterte Ereignisse](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Siehe [Erweiterte Ereignisse in der SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) | Ja – siehe [Erweiterte Ereignisse](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nein  | Nein  |
[Dateien und Dateigruppen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Nur primäre Dateigruppe | Ja |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nein  | Nein  |
| [Volltextsuche](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Worttrennungen von Drittanbietern werden nicht unterstützt. |Worttrennungen von Drittanbietern werden nicht unterstützt. |
| [Functions](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Geografische Wiederherstellung](sql-database-recovery-using-backups.md#geo-restore) | Ja – alle Dienstebenen außer Hyperscale | Nein – Sie können vollständige Sicherungen vom Typ „COPY_ONLY“ wiederherstellen, die Sie in regelmäßigen Abständen erstellen – siehe [Sicherung](sql-database-managed-instance-transact-sql-information.md#backup) und [Wiederherstellung](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Graph-Verarbeitung](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Ja | Ja |
| [In-Memory-Optimierung](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja – [nur Tarife „Premium“ und „Unternehmenskritisch“](sql-database-in-memory.md) | Ja – [nur Tarif „Unternehmenskritisch“](sql-database-managed-instance.md) |
| [Unterstützung von JSON-Daten](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ja](sql-database-json-features.md) | [Ja](sql-database-json-features.md) |
| [Sprachelemente](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Die meisten (siehe einzelne Elemente) |  Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Verknüpfte Server](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein (siehe [Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md)) | Nur SQL Server und SQL-Datenbank |
| [Protokollversand](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |[Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nein  | Nein  |
| [Minimale Protokollierung bei Massenimport](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nein  | Nein  |
| [Ändern von Systemdaten](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nein  | Ja |
| [OLE-Automatisierung](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | Nein  | Nein  |
| [Online-Indexvorgänge](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja | Ja |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Ja|Ja|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Ja|Ja|
| [Operatoren](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Die meisten (siehe einzelne Operatoren) |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Partitionierung](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja | Ja |
| [Point-in-Time-Wiederherstellung einer Datenbank](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja – alle Dienstebenen außer Hyperscale – siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) | Ja: siehe [Wiederherstellung der SQL-Datenbank](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nein  | Nein  |
| [Richtlinienbasierte Verwaltung](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nein  | Nein  |
| [Prädikate](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja | Ja |
| [Abfragebenachrichtigungen](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Nein  | Ja |
| [Statistik zur Abfrageleistung](sql-database-query-performance.md) | Ja | Nein  |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Ja, in der [Public Preview](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nein  |
| [Ressourcenkontrolle](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nein  | Ja |
| [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nein  | Ja – siehe [Wiederherstellung](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Wiederherstellen einer Datenbank aus der Sicherung](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Nur auf der Grundlage automatisierter Sicherungen (siehe [SQL-Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md)) | Aus automatisierten Sicherungen (siehe [SQL-Datenbankwiederherstellung](sql-database-recovery-using-backups.md)) und vollständigen Sicherungen(siehe [Sicherungsunterschiede](sql-database-managed-instance-transact-sql-information.md#backup)) |
| [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja | Ja |
| [Semantische Suche](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nein  | Nein  |
| [Sequenznummern](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nein  | Ja – siehe [Service Broker](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Konfigurationseinstellungen für Server](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nein  | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [SET-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Ja | Ja |
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja | Ja |
| [Azure SQL-Analyse](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Ja | Ja |
| [SQL-Datensynchronisierung](sql-database-get-started-sql-data-sync.md) | Ja | Nein  |
| [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nein (siehe [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)) | Ja – siehe [SQL Server-Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nein – siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | Nein (siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/)) |
| [SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nein (siehe [SQL-Datenbank-Überwachung](sql-database-auditing.md)) | Ja – siehe [Überwachung](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja | Ja |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja, mit einer verwalteten SSIS in Azure Data Factory-Umgebung (ADF), bei der Pakete in der von Azure SQL-Datenbank gehosteten SSISDB gespeichert und in Azure SSIS Integration Runtime (IR) ausgeführt werden. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Informationen zum Vergleich der SSIS-Features in SQL-Datenbank-Server und einer verwalteten Instanz finden Sie unter [Vergleich zwischen Azure SQL-Datenbank-Einzeldatenbanken/Pools für elastische Datenbanken und einer verwalteten Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). | Ja, mit einer verwalteten SSIS in Azure Data Factory-Umgebung (ADF), bei der Pakete in der von einer verwalteten Instanz gehosteten SSISDB gespeichert und in Azure SSIS Integration Runtime (IR) ausgeführt werden. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Informationen zum Vergleich der SSIS-Features in SQL-Datenbank und einer verwalteten Instanz finden Sie unter [Vergleich zwischen Azure SQL-Datenbank-Einzeldatenbanken/Pools für elastische Datenbanken und einer verwalteten Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Ja | Ja |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja | Ja |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nein (siehe [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)) | Ja |
| [SQL Server-Replikation](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Nur für Abonnentendatenbanken mit Transaktions- und Momentaufnahmenreplikation](sql-database-single-database-migrate.md) | Ja, in der [Public Preview](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nein – siehe [Power BI](https://docs.microsoft.com/power-bi/) | Nein – siehe [Power BI](https://docs.microsoft.com/power-bi/) |
| [Gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja | Ja |
| [Gespeicherte Systemfunktionen](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Gespeicherte Systemprozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Einige (siehe einzelne gespeicherte Prozeduren) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Systemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Einige (siehe einzelne Tabellen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Systemkatalogsichten](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Einige (siehe einzelne Sichten) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Lokale und datenbankbezogene globale temporäre Tabellen | Lokale und instanzbezogene globale temporäre Tabellen |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ja](sql-database-temporal-tables.md) | [Ja](sql-database-temporal-tables.md) |
|Bedrohungserkennung|  [Ja](sql-database-threat-detection.md)|[Ja](sql-database-managed-instance-threat-detection.md)|
| [Ablaufverfolgungsflags](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nein  | Nein  |
| [Variables](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | Ja |
| [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja, nur die Dienstebenen „Universell“ und „Unternehmenskritisch“| [Ja](transparent-data-encryption-azure-sql.md) |
[VNet](../virtual-network/virtual-networks-overview.md) | Teilweise – siehe [VNET-Endpunkte](sql-database-vnet-service-endpoint-rule-overview.md) | Ja, nur Resource Manager-Modell |
| [Windows Server-Failoverclustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [XML-Indizes](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja | Ja |

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)

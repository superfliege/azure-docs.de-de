---
title: 'Azure SQL-Datenbank: Funktionsvergleich | Microsoft-Dokumentation'
description: In diesem Artikel werden die Funktionen von Azure SQL-Datenbank und verwalteten Instanzen miteinander sowie mit SQL Server verglichen.
services: sql-database
author: jovanpop-msft
ms.reviewer: bonova, carlrab
ms.service: sql-database
ms.topic: article
ms.date: 03/30/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 7e3b084f833b6d84e5c5102555eb586e306e9de8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33895570"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Funktionsvergleich: Azure SQL-Datenbank und SQL Server 

Azure SQL-Datenbank verwendet eine gemeinsame Codebasis mit SQL Server. Die von Azure SQL-Datenbank unterstützen Funktionen von SQL Server hängen vom Typ der Azure SQL-Datenbank ab, die Sie erstellen. Mit Azure SQL-Datenbank können Sie eine Datenbank entweder als Teil einer [verwalteten Instanz](sql-database-managed-instance.md) (zurzeit als öffentliche Vorschauversion verfügbar) oder als Einzeldatenbank bzw. als Datenbank in einem Pool für elastische Datenbanken erstellen. 

Microsoft fügt Azure SQL-Datenbank ständig weitere Features hinzu. Besuchen Sie die Webseite mit Dienstupdates für Azure, und rufen Sie mithilfe dieser Filter die neuesten Updates ab:

* Gefiltert nach [SQL-Datenbank](https://azure.microsoft.com/updates/?service=sql-database).
* Gefiltert nach allgemeiner Verfügbarkeit [Ankündigungen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen.

## <a name="sql-server-and-sql-database-feature-support"></a>Unterstützung von SQL Server- und SQL-Datenbank-Features

Die folgende Tabelle enthält die wichtigsten Features von SQL Server und gibt Aufschluss darüber, ob die einzelnen Features teilweise oder vollständig unterstützt werden. Darüber hinaus enthält die Tabelle jeweils einen Link zu weiteren Featureinformationen. 

| **SQL-Feature** | **Unterstützt in Azure SQL-Datenbank** | **Verwaltete Instanz (Vorschauversion)** |
| --- | --- | --- |
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja (siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)) | Ja (siehe [Zertifikatspeicher](sql-database-always-encrypted.md) und [Schlüsseltresor](sql-database-always-encrypted-azure-key-vault.md)) |
| [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [Anfügen einer Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nein  | Nein  |
| [Anwendungsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja | Ja |
|[Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)| [Ja](sql-database-managed-instance-auditing.md) |
| [Automatische Sicherungen](sql-database-automated-backups.md) | Ja | Ja |
| [Automatische Optimierung (Planerzwingung)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| [Ja](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatische Optimierung (Indizes)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| Nein  |
| [BACPAC Datei (Export)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja (siehe [SQL-­Datenbank-Export](sql-database-export.md)) | Ja |
| [BACPAC Datei (Import)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja (siehe [SQL-­Datenbank-Import](sql-database-import.md)) | Ja |
| [Befehl „BACKUP“](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nein, nur vom System initiierte automatische Sicherungen – siehe [Automatisierte Sicherungen](sql-database-automated-backups.md) | Vom System initiierte automatisierte Sicherungen und benutzerinitiierte Kopiesicherungen – siehe [Sicherung](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Integrierte Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Erfassung geänderter Daten](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nein  | Ja |
| [Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |Ja |
| [Sortierungsanweisungen](https://docs.microsoft.com/sql/t-sql/statements/collations) | Ja | Ja |
| [ColumnStore-Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja – [Premium-Tarif, Standard-Tarif – mindestens S3, Tarif „Universell“ und Tarif „Unternehmenskritisch“](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Ja |
| [Common Language Runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nein  | Ja – siehe [CLR](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Eigenständige Datenbanken](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja | Ja |
| [Eigenständige Benutzer](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja | Ja |
| [Schlüsselwörter der Sprache zur Ablaufsteuerung](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja | Ja |
| [Datenbankübergreifende Abfragen](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein – siehe [Elastische Abfragen](sql-database-elastic-query-overview.md) | Ja, plus [elastische Abfragen](sql-database-elastic-query-overview.md) |
| [Datenbankübergreifende Transaktionen](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein  | Ja – siehe [Verknüpfte Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#linked-servers) |
| [Cursor](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja |Ja | 
| [Datenkomprimierung](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |Ja |
| [Datenbank-E-Mails](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nein  | Ja |
| [Data Migration Service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Ja | Ja |
| [Spiegeln von Datenbanken](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nein  | Nein  |
| [Datenbankkonfigurationseinstellungen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nein  | Nein  |
| [Datenbankmomentaufnahmen](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nein  | Nein  |
| [Datentypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |Ja |
| [DBCC-Anweisungen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [DBCC](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/statements) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Nur Datenbank |  Ja |
| [Verteilte Partitionsansichten](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nein  | Ja |
| [Verteilte Transaktionen – MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nein (siehe [Elastische Transaktionen](sql-database-elastic-transactions-overview.md)) |  Nein (siehe [Elastische Transaktionen](sql-database-elastic-transactions-overview.md)) |
| [DML-Anweisungen](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja | Ja |
| [DML-Trigger](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Die meisten (siehe einzelne Anweisungen) |  Ja |
| [DMVs](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Die meisten – siehe einzelne DMVs |  Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
|[Dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)| [Ja](sql-database-dynamic-data-masking-get-started.md) |
| [Pools für elastische Datenbanken](sql-database-elastic-pool.md) | Ja | Integriert: Eine einzelne verwaltete Instanz kann mehrere Datenbanken aufweisen, die denselben Ressourcenpool gemeinsam nutzen. |
| [Ereignisbenachrichtigungen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nein (siehe [Warnungen](sql-database-insights-alerts-portal.md)) | Ja |
| [Ausdrücke](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja | Ja |
| [Erweiterte Ereignisse](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Siehe [Erweiterte Ereignisse in der SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) | Ja – siehe [Erweiterte Ereignisse](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nein  | Nein  |
[Dateien und Dateigruppen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Nur primäre Dateigruppe | Ja |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nein  | Nein  |
| [Volltextsuche](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Worttrennungen von Drittanbietern werden nicht unterstützt. |Worttrennungen von Drittanbietern werden nicht unterstützt. |
| [Funktionen](https://docs.microsoft.com/sql/t-sql/functions/functions) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Geowiederherstellung](sql-database-recovery-using-backups.md#geo-restore) | Ja | Nein – Sie können vollständige Sicherungen vom Typ „COPY_ONLY“ wiederherstellen, die Sie in regelmäßigen Abständen erstellen – siehe [Sicherung](sql-database-managed-instance-transact-sql-information.md#backup) und [Wiederherstellung](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Georeplikation](sql-database-geo-replication-overview.md) | Ja | Nein  |
| [Graph-Verarbeitung](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Ja | Ja |
| [In-Memory-Optimierung](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja – [nur Tarife „Premium“ und „Unternehmenskritisch“](sql-database-in-memory.md) | Nein  |
| [Unterstützung von JSON-Daten](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ja](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) | [Ja](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) |
| [Sprachelemente](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Die meisten (siehe einzelne Elemente) |  Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Verknüpfte Server](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nein (siehe [Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md)) | Nur SQL Server und SQL-Datenbank |
| [Protokollversand](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |[Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nein  | Nein  |
| [Minimale Protokollierung bei Massenimport](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nein  | Nein  |
| [Ändern von Systemdaten](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nein  | Ja |
| [Onlineindexvorgänge](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja | Ja |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Ja|Ja|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Nein |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Ja|Ja|
| [Operatoren](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Die meisten (siehe einzelne Operatoren) |Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Partitionierung](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja | Ja |
| [Point-in-Time-Wiederherstellung einer Datenbank](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja (siehe [SQL-­Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)) | Ja (siehe [SQL-­Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nein  | Nein  |
| [Richtlinienbasierte Verwaltung](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nein  | Nein  |
| [Prädikate](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja | Ja |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Vorschauversion. Siehe [Neuerungen beim Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nein  |
| [Ressourcenkontrolle](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nein  | Ja |
| [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nein  | Ja – siehe [Wiederherstellung](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Wiederherstellen einer Datenbank aus der Sicherung](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Nur auf der Grundlage automatisierter Sicherungen (siehe [SQL-Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md)) | Auf der Grundlage automatisierter Sicherungen – siehe [SQL-­Datenbank-Wiederherstellung](sql-database-recovery-using-backups.md) und auf der Grundlage vollständiger Sicherungen – siehe [Sicherung](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja | Ja |
| [Semantische Suche](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nein  | Nein  |
| [Sequenznummern](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nein  | Ja – siehe [Service Broker](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Serverkonfigurationseinstellungen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nein  | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Set-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Die meisten (siehe einzelne Anweisungen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Ja | Ja |
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja | Ja |
| [SQL-Datensynchronisierung](sql-database-get-started-sql-data-sync.md) | Ja | Nein  |
| [SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is) | Ja | Ja |
| [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nein (siehe [Elastische Aufträge](sql-database-elastic-jobs-getting-started.md)) | Ja – siehe [SQL Server-Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nein – siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | Nein (siehe [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/)) |
| [SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nein (siehe [SQL-Datenbank-Überwachung](sql-database-auditing.md)) | Ja – siehe [Überwachung](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)] (https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja | Ja |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja, mit einer verwalteten SSIS in Azure Data Factory-Umgebung (ADF), bei der Pakete in der von Azure SQL-Datenbank gehosteten SSISDB gespeichert und in [Azure SSIS Integration Runtime](../data-factory/tutorial-deploy-ssis-packages-azure-powershell.md) ausgeführt werden. | Ja, mit einer verwalteten SSIS in Azure Data Factory-Umgebung (ADF), bei der Pakete in der von Azure SQL-Datenbank gehosteten verwalteten Instanz gespeichert und in [Azure SSIS Integration Runtime](../data-factory/tutorial-deploy-ssis-packages-azure-powershell.md) ausgeführt werden. |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Ja | Ja |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja | Ja |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nein (siehe [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)) | Ja |
| [SQL Server-Replikation](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Nur für Transaktions- und Momentaufnahmenreplikationsabonnent](sql-database-cloud-migrate.md) | Nein  |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nein – siehe [Power BI](https://docs.microsoft.com/power-bi/) | Nein – siehe [Power BI](https://docs.microsoft.com/power-bi/) |
| [Gespeicherten Prozeduren](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja | Ja |
| [Gespeicherte Systemfunktionen](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Die meisten (siehe einzelne Funktionen) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Gespeicherte Systemprozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Einige (siehe einzelne gespeicherte Prozeduren) | Ja – siehe [Gespeicherte Prozeduren, Funktionen, Trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Systemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Einige (siehe einzelne Tabellen) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Systemkatalogsichten](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Einige (siehe einzelne Sichten) | Ja – siehe [T-SQL-Unterschiede](sql-database-managed-instance-transact-sql-information.md) |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Lokale und datenbankbezogene globale temporäre Tabellen | Lokale und instanzbezogene globale temporäre Tabellen |
| [Temporäre Tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ja](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) | [Ja](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) |
|Bedrohungserkennung|  [Ja](sql-database-threat-detection.md)|[Ja](sql-database-managed-instance-threat-detection.md)|
| [Ablaufverfolgungsflags](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nein  | Nein  |
| [Variablen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | Ja |
| [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja | Nein, nicht in Public Preview |
[VNET](../virtual-network/virtual-networks-overview.md) | Teilweise – siehe [VNET-Endpunkte](sql-database-vnet-service-endpoint-rule-overview.md) | Ja, nur Resource Manager-Modell |
| [ Windows Server-Failoverclustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). | [Hochverfügbarkeit](sql-database-high-availability.md) ist in jeder Datenbank enthalten. Informationen zur Notfallwiederherstellung finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md). |
| [XML-Indizes](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja | Ja |

## <a name="next-steps"></a>Nächste Schritte

- Informationen über den Azure SQL-Datenbank-Dienst finden Sie unter [Was ist SQL Database? Einführung in SQL-Datenbank](sql-database-technical-overview.md).
- Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)

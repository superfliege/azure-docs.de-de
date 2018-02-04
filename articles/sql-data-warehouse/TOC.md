# Übersicht

## [Informationen zu SQL Data Warehouse](sql-data-warehouse-overview-what-is.md)
## [Cheat Sheet](cheat-sheet.md)

# Schnellstarts

## [Erstellen und Verbinden – Portal](create-data-warehouse-portal.md)
## Anhalten und Fortsetzen von Computevorgängen
### [Portal](pause-and-resume-compute-portal.md)
### [PowerShell](pause-and-resume-compute-powershell.md)
## Skalieren von Computeressourcen
### [Portal](quickstart-scale-compute-portal.md)
### [PowerShell](quickstart-scale-compute-powershell.md)
### [T-SQL](quickstart-scale-compute-tsql.md)


# Tutorials
## [1: Laden von Daten aus einem Blob](load-data-from-azure-blob-storage-using-polybase.md)

# Konzepte
## Features des Diensts
### [MPP-Architektur](massively-parallel-processing-mpp-architecture.md)
### [Leistungsstufen](performance-tiers.md)
### [Data Warehouse-Einheiten](what-is-a-data-warehouse-unit-dwu-cdwu.md)
### [Data Warehouse-Sicherungen](sql-data-warehouse-backups.md)
### [Überwachung](sql-data-warehouse-auditing-overview.md)
### [Kapazitätsgrenzen](sql-data-warehouse-service-capacity-limits.md)
### [HÄUFIG GESTELLTE FRAGEN](sql-data-warehouse-overview-faq.md)

## Sicherheit
### [Übersicht](sql-data-warehouse-overview-manage-security.md)
### [Authentifizierung](sql-data-warehouse-authentication.md)


## Migrieren zu SQL Data Warehouse
### [Übersicht](sql-data-warehouse-overview-migrate.md)
### [Migrieren des Schemas](sql-data-warehouse-migrate-schema.md)
### [Migrieren von Code](sql-data-warehouse-migrate-code.md)
### [Migrieren von Daten](sql-data-warehouse-migrate-data.md)

## Laden und Verschieben von Daten
### [Übersicht](design-elt-data-loading.md)
### [bewährten Methoden](guidance-for-loading-data.md)


## Integrieren
### [Übersicht](sql-data-warehouse-overview-integrate.md)
### [Elastische Abfrage für SQL-Datenbank](how-to-use-elastic-query-with-sql-data-warehouse.md)


## Überwachen & optimieren
### [Richtlinien](resource-classes-for-workload-management.md)
### [Columnstore-Komprimierung](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)
### [Überwachen](sql-data-warehouse-manage-monitor.md)
### [Problembehandlung](sql-data-warehouse-troubleshoot.md)

## Entwickeln von Data Warehouses
### [Übersicht](sql-data-warehouse-overview-develop.md)
### [Data Warehouse-Komponenten](sql-data-warehouse-overview-workload.md)

### Tabellen
#### [Übersicht](sql-data-warehouse-tables-overview.md)
#### [CTAS](sql-data-warehouse-develop-ctas.md)
#### [Datentypen](sql-data-warehouse-tables-data-types.md)
#### [Verteilte Tabellen](sql-data-warehouse-tables-distribute.md)
#### [Indizes](sql-data-warehouse-tables-index.md)
#### [Identität](sql-data-warehouse-tables-identity.md)
#### [Partitionen](sql-data-warehouse-tables-partition.md)
#### [Replizierte Tabellen](design-guidance-for-replicated-tables.md)
#### [Statistiken](sql-data-warehouse-tables-statistics.md)
#### [Temporär](sql-data-warehouse-tables-temporary.md)

### Abfragen
#### [Dynamischer SQL-Code](sql-data-warehouse-develop-dynamic-sql.md)
#### [Gruppierungsoptionen](sql-data-warehouse-develop-group-by-options.md)
#### [Bezeichnungen](sql-data-warehouse-develop-label.md)

### T-SQL-Sprachelemente
#### [Schleifen](sql-data-warehouse-develop-loops.md)
#### [Gespeicherten Prozeduren](sql-data-warehouse-develop-stored-procedures.md)
#### [Transaktionen](sql-data-warehouse-develop-transactions.md)
#### [Bewährte Methoden für Transaktionen](sql-data-warehouse-develop-best-practices-transactions.md)
#### [Benutzerdefinierte Schemas](sql-data-warehouse-develop-user-defined-schemas.md)
#### [Variablenzuweisung](sql-data-warehouse-develop-variable-assignment.md)
#### [Ansichten](sql-data-warehouse-develop-views.md)

# Anleitungen
## Features des Diensts
### [Wiederherstellen eines Data Warehouse – Portal](sql-data-warehouse-restore-database-portal.md)
### [Wiederherstellen eines Data Warehouse – PowerShell](sql-data-warehouse-restore-database-powershell.md)
### [Wiederherstellen eines Data Warehouse – REST-API](sql-data-warehouse-restore-database-rest-api.md)

## Sicherheit
### [Aktivieren der Verschlüsselung – Portal](sql-data-warehouse-encryption-tde.md)
### [Aktivieren der Verschlüsselung – T-SQL](sql-data-warehouse-encryption-tde-tsql.md)
### [Bedrohungserkennung](sql-data-warehouse-security-threat-detection.md)


## Laden und Verschieben von Daten
### [Öffentliche Contoso-Daten](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
### [Azure Data Lake Store](sql-data-warehouse-load-from-azure-data-lake-store.md)
### [BCP](sql-data-warehouse-load-with-bcp.md)
### [Data Factory](sql-data-warehouse-load-with-data-factory.md)
### [AzCopy](sql-data-warehouse-load-from-sql-server-with-polybase.md)
### [RedGate](sql-data-warehouse-load-with-redgate.md)
### [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)


## Integrieren
### [Elastische Abfrage für SQL-Datenbank](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)
### [Hinzufügen eines Azure Stream Analytics-Auftrags](sql-data-warehouse-integrate-azure-stream-analytics.md)
### [Verwenden von Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
### [Visualisieren mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)

## Überwachen & optimieren
### [Analysieren Ihrer Workload](analyze-your-workload.md)

## Horizontales Skalieren
### [Computeverwaltung – REST-API](sql-data-warehouse-manage-compute-rest-api.md)
### [Automatisieren von Computeebenen](manage-compute-with-azure-functions.md)


# Verweis


## T-SQL
### [Vollständige Referenz](https://docs.microsoft.com/sql/t-sql/language-reference/)
### [SQL DW-Sprachelemente](sql-data-warehouse-reference-tsql-language-elements.md)
### [SQL DW-Anweisungen](sql-data-warehouse-reference-tsql-statements.md)
## [Systemsichten](sql-data-warehouse-reference-tsql-system-views.md)
## [PowerShell-Cmdlets](sql-data-warehouse-reference-powershell-cmdlets.md)

# angeben
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=databases)
## [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse)
## [Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Funktionsanfragen](https://feedback.azure.com/forums/307516-sql-data-warehouse/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=sql-data-warehouse)
## [Stapelüberlauf](https://stackoverflow.com/questions/tagged/azure-sqldw/)
## [Unterstützung](sql-data-warehouse-get-started-create-support-ticket.md)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

## Partner
### [Business Intelligence](sql-data-warehouse-partner-business-intelligence.md)
### [Datenintegration](sql-data-warehouse-partner-data-integration.md)
### [Datenverwaltung](sql-data-warehouse-partner-data-management.md)

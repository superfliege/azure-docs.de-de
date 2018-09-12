---
title: Verwaltete Azure SQL-Datenbank-Instanz – T-SQL-Unterschiede | Microsoft-Dokumentation
description: In diesem Artikel werden die T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server beschrieben.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab, bonova
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 0813/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 57c6b52df3e8f6c47eb794cda4b47bfa2d7de374
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051237"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server 

Verwaltete Azure SQL-Datenbank-Instanzen (Vorschau) bieten umfassende Kompatibilität mit einer lokalen SQL Server-Datenbank-Engine. Die meisten Features der SQL Server-Datenbank-Engine werden in verwalteten Instanzen unterstützt. Es bestehen dennoch einige Unterschiede in der Syntax und im Verhalten. Diese Unterschiede werden in diesem Artikel zusammengefasst und erläutert.
 - [T-SQL-Unterschiede und nicht unterstützte Features](#Differences)
 - [Features mit abweichendem Verhalten in verwalteten Instanzen](#Changes)
 - [Temporäre Einschränkungen und bekannte Probleme](#Issues)

## <a name="Differences"></a> T-SQL-Unterschiede zu SQL Server 

In diesem Abschnitt sind die wichtigsten Unterschiede in der T-SQL-Syntax und dem Verhalten zwischen einer verwalteten Instanz und einer lokalen SQL Server-Datenbank-Engine sowie nicht unterstützte Features zusammengefasst.

### <a name="always-on-availability"></a>Always On-Verfügbarkeit

[Hochverfügbarkeit](sql-database-high-availability.md) ist in verwalteten Instanzen integriert und kann von Benutzern nicht gesteuert werden. Folgende Anweisungen werden nicht unterstützt:
 - [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
 - [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
 - [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
 - [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
 - Klausel [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) der Anweisung [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)

### <a name="auditing"></a>Überwachung 
 
Wichtigste Unterschiede bei der SQL-Überwachung in einer verwalteten Instanz, Azure SQL-Datenbank und lokalem SQL Server:
- In der verwalteten Instanz wird die SQL-Überwachung auf Serverebene ausgeführt und speichert Dateien mit der Endung `.xel` in einem Azure Blob Storage-Konto.  
- In Azure SQL-Datenbank wird die SQL-Überwachung auf Datenbankebene ausgeführt.
- Auf einem lokalen oder virtuellen SQL Server-Computer wird die SQL-Überwachung auf Serverebene ausgeführt, Ereignisse werden jedoch in Dateisystemprotokollen bzw. Windows-Ereignisprotokollen gespeichert.  
  
Die XEvent-Überwachung in einer verwalteten Instanz unterstützt Azure Blob Storage-Ziele. Dateiprotokolle und Windows-Protokolle werden nicht unterstützt.    
 
Wichtigste Unterschiede in der Syntax von `CREATE AUDIT` zur Überwachung in Azure Blob Storage:
- Mit der neuen Syntax `TO URL` können Sie die URL des Azure Blob Storage-Containers angeben, in dem Dateien mit der Endung `.xel` gespeichert werden. 
- Die Syntax `TO FILE` wird nicht unterstützt, da die verwaltete Instanz nicht auf Windows-Dateifreigaben zugreifen kann. 
 
Weitere Informationen finden Sie unter  
- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql) 
- [Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)     

### <a name="backup"></a>Backup 

In einer verwalteten Instanz werden automatische Sicherungen durchgeführt, sodass Benutzer vollständige `COPY_ONLY`-Sicherungen für Datenbanken erstellen können. Differenzielle, Protokoll- und Dateimomentaufnahmesicherungen werden nicht unterstützt.  
- In einer verwalteten Instanz kann eine Datenbank nur in einem Azure Blob Storage-Konto gesichert werden: 
 - Nur `BACKUP TO URL` wird unterstützt. 
 - `FILE`, `TAPE` und Sicherungsmedien werden nicht unterstützt.  
- Die meisten allgemeinen `WITH`-Optionen werden unterstützt. 
 - `COPY_ONLY` ist obligatorisch.
 - `FILE_SNAPSHOT` wird nicht unterstützt.  
 - Bandoptionen: `REWIND`, `NOREWIND`, `UNLOAD` und `NOUNLOAD` werden nicht unterstützt. 
 - Protokollspezifische Optionen: `NORECOVERY`, `STANDBY` und `NO_TRUNCATE` werden nicht unterstützt. 
 
Einschränkungen:  
- Eine verwaltete Instanz kann eine Datenbank in einer Sicherung mit bis zu 32 Stripes sichern. Dies ist ausreichend für Datenbanken mit bis zu 4 TB, wenn die Sicherungskomprimierung verwendet wird.
- Die maximale Stripegröße für Sicherungen ist 195 GB (maximale Blobgröße). Erhöhen Sie die Anzahl der Stripes im Sicherungsbefehl, um die einzelne Stripegröße zu verringern und diese Einschränkung einzuhalten. 

> [!TIP]
> Um diese Einschränkung in der lokalen Umgebung zu umgehen, führen Sie anstelle einer Sicherung in `URL` eine Sicherung in `DISK` durch, laden Sie die Sicherungsdatei in das Blob hoch, und führen Sie dann die Wiederherstellung durch. Bei der Wiederherstellung werden größere Dateien unterstützt, da ein anderer Blobtyp verwendet wird.  

Informationen zu Sicherungen mithilfe von T-SQL finden Sie unter [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Pufferpoolerweiterung 
 
- [Pufferpoolerweiterung](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wird nicht unterstützt.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wird nicht unterstützt. Siehe [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql). 
 
### <a name="bulk-insert--openrowset"></a>BULK INSERT/OPENROWSET

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordnern zugreifen. Daher müssen die Dateien aus Azure Blob Storage importiert werden.
- Beim Importieren von Dateien aus Azure Blob Storage ist `DATASOURCE` im Befehl `BULK INSERT` erforderlich. Siehe [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- Beim Lesen von Inhalten einer Datei aus Azure Blob Storage ist `DATASOURCE` in der Funktion `OPENROWSET` erforderlich. Siehe [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
 
### <a name="certificates"></a>Zertifikate 

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen: 
- `CREATE FROM`/`BACKUP TO` für Dateien wird für Zertifikate nicht unterstützt.
- `CREATE`/`BACKUP` für Zertifikate von `FILE`/`ASSEMBLY` wird nicht unterstützt. Dateien mit privaten Schlüsseln können nicht verwendet werden.  
 
Siehe [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) und [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
> [!TIP]
> Problemumgehung: Erstellen Sie ein Skript für das Zertifikat und den privaten Schlüssel, speichern Sie es als SQL-Datei, und erstellen Sie sie aus der Binärdatei: 
> 
> ``` 
CREATE CERTIFICATE  
 FROM BINARY = asn_encoded_certificate    
WITH PRIVATE KEY ( <private_key_options> ) 
>```   
 
### <a name="clr"></a>CLR 

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen: 
- Nur `CREATE ASSEMBLY FROM BINARY` wird unterstützt. Siehe [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` wird nicht unterstützt. Siehe [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kann nicht auf Dateien verweisen. Siehe [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).
 
### <a name="compatibility-levels"></a>Kompatibilitätsgrade 
 
- Folgende Kompatibilitätsgrade werden unterstützt: 100, 110, 120, 130, 140  
- Kompatibilitätsgrade unter 100 werden nicht unterstützt. 
- Der standardmäßige Kompatibilitätsgrad für neue Datenbanken ist 140. Bei wiederhergestellten Datenbanken bleibt der Kompatibilitätsgrad unverändert, wenn er zuvor bei 100 und höher lag.

Siehe [ALTER DATABASE-Kompatibilitätsgrad](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).
 
### <a name="credential"></a>Anmeldeinformation 
 
Nur Azure Key Vault- und `SHARED ACCESS SIGNATURE`-Identitäten werden unterstützt. Windows-Benutzer werden nicht unterstützt.
 
Siehe [CREATE CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) und [ALTER CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql). 
 
### <a name="cryptographic-providers"></a>Kryptografieanbieter

Eine verwaltete Instanz kann nicht auf Dateien zugreifen. Daher können Kryptografieanbieter nicht erstellt werden:
- `CREATE CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [CREATE CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [ALTER CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql). 

### <a name="collation"></a>Collation 
 
Die Serversortierung ist als `SQL_Latin1_General_CP1_CI_AS` festgelegt und kann nicht geändert werden. Siehe [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).
 
### <a name="database-options"></a>Datenbankoptionen 
 
- Mehrere Protokolldateien werden nicht unterstützt. 
- In-Memory-Objekte werden auf der Dienstebene für allgemeine Zwecke nicht unterstützt.  
- Es gilt eine Einschränkung von 280 Dateien pro Instanz, d.h. maximal 280 Dateien pro Datenbank. Für diese Einschränkung werden Datendateien und Protokolldateien berücksichtigt.  
- Eine Datenbank darf keine Dateigruppen mit Filestreamdaten enthalten.  Bei der Wiederherstellung treten Fehler auf, wenn die BAK-Datei `FILESTREAM`-Daten enthält.  
- Jede Datei wird in Azure Storage Premium gespeichert. E/A und Durchsatz pro Datei hängen wie bei Azure Storage Premium-Datenträgern von der Größe der einzelnen Dateien ab. Siehe [Größen von Azure Storage Premium-Datenträgern](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes).  
 
#### <a name="create-database-statement"></a>CREATE DATABASE-Anweisung

Es gelten die folgenden Einschränkungen für `CREATE DATABASE`: 
- Dateien und Dateigruppen können nicht definiert werden.  
- Die Option `CONTAINMENT` wird nicht unterstützt.  
- `WITH`-Optionen werden nicht unterstützt.  
   > [!TIP]
   > Als Problemumgehung können Sie `ALTER DATABASE` nach `CREATE DATABASE` verwenden, um Datenbankoptionen zum Hinzufügen von Dateien oder Festlegen einer Kapselung festzulegen.  

- Die Option `FOR ATTACH` wird nicht unterstützt. 
- Die Option `AS SNAPSHOT OF` wird nicht unterstützt. 

Weitere Informationen finden Sie unter [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE-Anweisung

Einige Dateieigenschaften können nicht festgelegt oder geändert werden:
- Der Dateipfad kann in der T-SQL-Anweisung `ALTER DATABASE ADD FILE (FILENAME='path')` nicht angegeben werden. Entfernen Sie `FILENAME` aus dem Skript, da die verwaltete Instanz die Dateien automatisch speichert.  
- Der Dateiname kann nicht mithilfe der Anweisung `ALTER DATABASE` geändert werden.

Die folgenden Optionen werden standardmäßig festgelegt und können nicht geändert werden: 
- `MULTI_USER` 
- `ENABLE_BROKER ON` 
- `AUTO_CLOSE OFF` 

Folgende Optionen können nicht geändert werden: 
- `AUTO_CLOSE` 
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)` 
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)` 
- `DISABLE_BROKER` 
- `EMERGENCY` 
- `ENABLE_BROKER` 
- `FILESTREAM` 
- `HADR`   
- `NEW_BROKER` 
- `OFFLINE` 
- `PAGE_VERIFY` 
- `PARTNER` 
- `READ_ONLY` 
- `RECOVERY BULK_LOGGED` 
- `RECOVERY_SIMPLE` 
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER` 
- `SINGLE_USER` 
- `WITNESS`

Die Änderung von Namen wird nicht unterstützt.

Weitere Informationen finden Sie unter [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Datenbankspiegelung

Die Datenbankspiegelung wird nicht unterstützt.
 - Die Optionen `ALTER DATABASE SET PARTNER` und `SET WITNESS` werden nicht unterstützt.
 - `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wird nicht unterstützt.

Weitere Informationen finden Sie unter [ALTER DATABASE SET PARTNER und SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) und [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC 
 
Nicht dokumentierte DBCC-Anweisungen, die in SQL Server aktiviert sind, werden in einer verwalteten Instanz nicht unterstützt.
- `Trace Flags` werden nicht unterstützt. Siehe [Ablaufverfolgungsflags](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` wird nicht unterstützt. Siehe [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` wird nicht unterstützt. Siehe [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Verteilte Transaktionen

Weder MSDTC noch [elastische Transaktionen](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) werden derzeit in einer verwalteten Instanz unterstützt.

### <a name="extended-events"></a>Erweiterte Ereignisse 

Einige Windows-spezifische Ziele für XEvents werden nicht unterstützt:
- `etw_classic_sync target` wird nicht unterstützt. Speichern Sie Dateien mit der Endung `.xel` in Azure Blob Storage. Siehe [etw_classic_sync-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target). 
- `event_file target` wird nicht unterstützt. Speichern Sie Dateien mit der Endung `.xel` in Azure Blob Storage. Siehe [event_file-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Externe Bibliotheken

Datenbankinterne R- und externe Python-Bibliotheken werden noch nicht unterstützt. Siehe [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>Filestream und Filetable

- Filestreamdaten werden nicht unterstützt. 
- Die Datenbank darf keine Dateigruppen mit `FILESTREAM`-Daten enthalten.
- `FILETABLE` wird nicht unterstützt.
- Tabellen dürfen keine `FILESTREAM`-Typen enthalten.
- Folgende Funktionen werden nicht unterstützt:
 - `GetPathLocator()` 
 - `GET_FILESTREAM_TRANSACTION_CONTEXT()` 
 - `PathName()` 
 - `GetFileNamespacePath()` 
 - `FileTableRootPath()` 

Weitere Informationen finden Sie unter [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) und [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantische Volltextsuche

Die [semantische Suche](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) wird nicht unterstützt.

### <a name="linked-servers"></a>Verknüpfte Server
 
Verbindungsserver in einer verwalteten Instanz unterstützen eine begrenzte Anzahl von Zielen: 
- Unterstützte Ziele: SQL Server und SQL-Datenbank
- Nicht unterstützte Ziele: Dateien, Analysis Services und andere RDBMS.

Vorgänge

- Instanzübergreifende Schreibtransaktionen werden nicht unterstützt.
- `sp_dropserver` wird zum Löschen eines Verbindungsservers unterstützt. Siehe [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- Die Funktion `OPENROWSET` kann nur auf SQL Server-Instanzen (entweder verwaltet, lokal oder auf virtuellen Computern) zur Ausführung von Abfragen verwendet werden. Siehe [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- Die Funktion `OPENDATASOURCE` kann nur auf SQL Server-Instanzen (entweder verwaltet, lokal oder auf virtuellen Computern) zur Ausführung von Abfragen verwendet werden. Nur die Werte `SQLNCLI`, `SQLNCLI11` und `SQLOLEDB` werden als Anbieter unterstützt. Beispiel: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Siehe [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
 
### <a name="logins--users"></a>Anmeldungen/Benutzer 

- Mithilfe von `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` und `FROM SID` erstellte SQL-Anmeldungen werden unterstützt. Siehe [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Windows-Anmeldungen, die mit der Syntax `CREATE LOGIN ... FROM WINDOWS` erstellt wurden, werden nicht unterstützt.
- Der Azure Active Directory-Benutzer (Azure AD), der die Instanz erstellt hat, verfügt über [uneingeschränkte Administratorrechte](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Azure Active Directory-Benutzer (Azure AD) auf Datenbankebene ohne Administratorrechte können mit der Syntax `CREATE USER ... FROM EXTERNAL PROVIDER` erstellt werden. Siehe [CREATE USER ... FROM EXTERNAL PROVIDER](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users).
 
### <a name="polybase"></a>PolyBase

Externe Tabellen, die auf die Dateien in HDFS oder Azure Blob Storage verweisen, werden nicht unterstützt. Informationen zu PolyBase finden Sie unter [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replikation 
 
Die Replikation wird in der verwalteten Instanz unterstützt. Informationen zur Replikation finden Sie unter [SQL Server-Replikation](http://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).
 
### <a name="restore-statement"></a>RESTORE-Anweisung 
 
- Unterstützte Syntax  
   - `RESTORE DATABASE` 
   - `RESTORE FILELISTONLY ONLY` 
   - `RESTORE HEADER ONLY` 
   - `RESTORE LABELONLY ONLY` 
   - `RESTORE VERIFYONLY ONLY` 
- Nicht unterstützte Syntax 
   - `RESTORE LOG ONLY` 
   - `RESTORE REWINDONLY ONLY`
- Quelle  
 - Als einzige Option wird `FROM URL` (Azure Blob Storage) unterstützt.
 - `FROM DISK`/`TAPE`/Sicherungsmedium wird nicht unterstützt.
 - Sicherungssätze werden nicht unterstützt. 
- `WITH`-Optionen werden nicht unterstützt (`DIFFERENTIAL`, `STATS` usw. nicht möglich).     
- `ASYNC RESTORE`: Die Wiederherstellung wird fortgesetzt, selbst wenn die Clientverbindung unterbrochen wird. Wenn die Verbindung ausfällt, können Sie die Sicht `sys.dm_operation_status` auf den Status eines Wiederherstellungsvorgangs (sowie auf CREATE und DROP DATABASE) prüfen. Siehe [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  
 
Die folgenden Datenbankoptionen werden festgelegt oder überschrieben und können später nicht geändert werden:  
- `NEW_BROKER` (wenn der Broker in der BAK-Datei nicht aktiviert ist)  
- `ENABLE_BROKER` (wenn der Broker in der BAK-Datei nicht aktiviert ist)  
- `AUTO_CLOSE=OFF` (wenn eine Datenbank in der BAK-Datei `AUTO_CLOSE=ON` enthält)  
- `RECOVERY FULL` (wenn eine Datenbank in der BAK-Datei den Wiederherstellungsmodus `SIMPLE` oder `BULK_LOGGED` enthält)
- Eine arbeitsspeicheroptimierte Dateigruppe wird hinzugefügt und erhält den Namen „XTP“, wenn sie in der BAK-Quelldatei nicht vorhanden war.  
- Alle vorhandenen arbeitsspeicheroptimierten Dateigruppen werden in XTP umbenannt.  
- Die Optionen `SINGLE_USER` und `RESTRICTED_USER` werden in `MULTI_USER`  konvertiert.  
Einschränkungen:  
- `.BAK`-Dateien mit mehreren Sicherungssätzen können nicht wiederhergestellt werden. 
- `.BAK`-Dateien mit mehreren Protokolldateien können nicht wiederhergestellt werden. 
- Bei der Wiederherstellung treten Fehler auf, wenn die BAK-Datei `FILESTREAM`-Daten enthält.
- Sicherungen, die Datenbanken mit aktiven In-Memory-Objekten enthalten, können derzeit nicht wiederhergestellt werden.  
- Sicherungen, die Datenbanken enthalten, in denen zu einem bestimmten Zeitpunkt In-Memory-Objekte vorhanden waren, können derzeit nicht wiederhergestellt werden.   
- Sicherungen, die Datenbanken im schreibgeschützten Modus enthalten, können derzeit nicht wiederhergestellt werden. Diese Einschränkung wird in Kürze aufgehoben.   
 
Weitere Informationen zu Restore-Anweisungen finden Sie unter [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service Broker 
 
- Der instanzübergreifende Service Broker wird nicht unterstützt. 
 - `sys.routes` – Voraussetzung: Wählen Sie „address“ für „sys.routes“ aus. „address“ muss für jede Route auf „LOCAL“ festgelegt sein. Siehe [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
 - `CREATE ROUTE`: `CREATE ROUTE` kann ausschließlich mit dem Wert `LOCAL` für `ADDRESS` verwendet werden. Siehe [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
 - `ALTER ROUTE`: `ALTER ROUTE` kann ausschließlich mit dem Wert `LOCAL` für `ADDRESS` verwendet werden. Siehe [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  
 
### <a name="service-key-and-service-master-key"></a>Dienstschlüssel und Diensthauptschlüssel 
 
- Die [Sicherung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet). 
- Die [Wiederherstellung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet). 
- Die [Sicherung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet). 
- Die [Wiederherstellung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet). 
 
### <a name="stored-procedures-functions-triggers"></a>Gespeicherte Prozeduren, Funktionen, Trigger 
 
- `NATIVE_COMPILATION` wird derzeit nicht unterstützt. 
- Die folgenden [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql)-Optionen werden nicht unterstützt: 
 - `allow polybase export` 
 - `allow updates` 
 - `filestream_access_level` 
 - `max text repl size` 
 - `remote data archive` 
 - `remote proc trans` 
- `sp_execute_external_scripts` wird nicht unterstützt. Siehe [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` wird nicht unterstützt. Siehe [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` werden nicht unterstützt, einschließlich `sp_addextendedproc` und `sp_dropextendedproc`. Siehe [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db` und `sp_detach_db` werden nicht unterstützt. Siehe [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql) und [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` wird nicht unterstützt. Siehe [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>SQL Server-Agent 
 
- SQL-Agent-Einstellungen sind schreibgeschützt. Die Prozedur `sp_set_agent_properties` wird in einer verwalteten Instanz nicht unterstützt.  
- Aufträge: Derzeit werden nur T-SQL-Auftragsschritte unterstützt (weitere Schritte werden in der Public Preview-Phase hinzugefügt).
 - SSIS wird noch nicht unterstützt. 
 - Die Replikation wird noch nicht unterstützt.  
  - Der Transaktionsprotokollleser wird noch nicht unterstützt.  
  - Die Momentaufnahme wird noch nicht unterstützt.  
  - Der Verteiler wird noch nicht unterstützt.  
  - Das Zusammenführen wird nicht unterstützt.  
  - Der Warteschlangenleser wird nicht unterstützt.  
 - Die Befehlsshell wird noch nicht unterstützt. 
  - Eine verwaltete Instanz kann nicht auf externe Ressourcen zugreifen (z.B. Netzwerkfreigaben über Robocopy).  
 - PowerShell wird noch nicht unterstützt.
 - Analysis Services wird nicht unterstützt.  
- Benachrichtigungen werden teilweise unterstützt.
 - Die E-Mail-Benachrichtigung wird unterstützt. Dazu muss ein Datenbank-E-Mail-Profil konfiguriert werden. Es kann nur ein Datenbank-E-Mail-Profil festgelegt werden, für das in der Public Preview-Phase der Name `AzureManagedInstance_dbmail_profile` angegeben werden muss (temporäre Einschränkung).  
 - Der Pager wird nicht unterstützt.  
 - NetSend wird nicht unterstützt. 
 - Warnungen werden noch nicht unterstützt.
 - Proxys werden nicht unterstützt.  
- EventLog wird nicht unterstützt. 
 
Die folgenden Features werden derzeit nicht unterstützt, kommen aber später hinzu:  
- Proxys
- Planen von Aufträgen für CPU im Leerlauf 
- Aktivieren/Deaktivieren des Agent
- Alerts

Weitere Informationen zum SQL Server-Agent finden Sie unter [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).
 
### <a name="tables"></a>Tabellen 

Folgende Tabellen werden nicht unterstützt: 
- `FILESTREAM` 
- `FILETABLE` 
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Weitere Informationen zum Erstellen und Ändern von Tabellen finden Sie unter [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) und [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="Changes"></a> Behavior Changes 
 
Die folgenden Variablen, Funktionen und Sichten geben abweichende Ergebnisse zurück:  
- `SERVERPROPERTY('EngineEdition')` gibt den Wert 8 zurück. Durch diese Eigenschaft wird eine verwaltete Instanz eindeutig identifiziert. Siehe [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` gibt NULL zurück, da das für SQL Server bestehende Konzept der Instanz nicht für die verwaltete Instanz gilt. Siehe [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net. Siehe [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. `myinstance.domain.database.windows.net` für die Eigenschaften „name“ und „data_source“. Siehe [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVICENAME` gibt NULL zurück, da das für SQL Server bestehende Konzept des Diensts nicht für die verwaltete Instanz gilt. Siehe [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).   
- `SUSER_ID` wird unterstützt. Gibt NULL zurück, wenn die AAD-Anmeldung in „sys.syslogins“ nicht vorhanden ist. Siehe [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` wird nicht unterstützt. Gibt falsche Daten zurück (temporäres bekanntes Problem). Siehe [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 
- `GETDATE()` und andere integrierte Datum/Uhrzeit-Funktionen geben die Zeit immer in der UTC-Zeitzone zurück. Siehe [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bekannte Probleme und Einschränkungen

### <a name="tempdb-size"></a>TEMPDB-Größe

`tempdb` ist in 12 Dateien mit jeweils einer maximalen Größe von 14 GB unterteilt. Diese maximale Größe pro Datei kann nicht geändert werden. Zudem können `tempdb` keine neuen Dateien hinzugefügt werden. Diese Einschränkung wird in Kürze aufgehoben. Einige Abfragen geben möglicherweise einen Fehler zurück, wenn für sie mehr als 168 GB in `tempdb` erforderlich sind.

### <a name="exceeding-storage-space-with-small-database-files"></a>Überschreiten des Speicherplatzes mit kleinen Datenbankdateien

Jede verwaltete Instanz verfügt über bis zu 35 TB Speicher für Azure Premium-Datenträger, und jede Datenbankdatei wird auf einem separaten physischen Datenträger abgelegt. Mögliche Datenträgergrößen sind 128 GB, 256 GB, 512 GB, 1 TB oder 4 TB. Nicht verwendeter Speicherplatz auf dem Datenträger wird nicht berechnet, aber die Gesamtgröße der Azure Premium-Datenträger darf 35 TB nicht überschreiten. In einigen Fällen kann eine verwaltete Instanz, die nicht insgesamt 8 TB benötigt, aufgrund interner Fragmentierung das Azure-Limit von 35 TB überschreiten. 

Eine verwaltete Instanz könnte beispielsweise über eine Datei mit einer Größe von 1,2 TB verfügen, die auf einer 4-TB-Platte gespeichert ist, und über 248 Dateien mit einer Größe von jeweils 1 GB, die auf separaten 128-GB-Platten gespeichert sind. In diesem Beispiel 
* beträgt die Gesamtgröße des Datenträgerspeichers 1 x 4 TB + 248 x 128 GB = 35 TB. 
* beträgt der reservierte Gesamtspeicherplatz für Datenbanken in der Instanz 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.
Dies verdeutlicht, dass eine verwaltete Instanz unter bestimmten Umständen aufgrund einer sehr spezifischen Verteilung von Dateien das Azure Premium-Datenträgerlimit in Höhe von 35 TB erreichen kann, wo Sie dies möglicherweise nicht erwarten. 

In diesem Beispiel funktionieren vorhandene Datenbanken weiterhin und können ohne Probleme weiter wachsen, solange keine neuen Dateien hinzugefügt werden. Es könnten jedoch keine neuen Datenbanken erstellt oder wiederhergestellt werden, da für neue Plattenlaufwerke nicht genügend Speicherplatz vorhanden ist, selbst nicht dann, wenn die Gesamtgröße aller Datenbanken das Größenlimit der Instanz nicht überschreitet. Der Fehler, der in diesem Fall zurückgegeben wird, ist nicht klar.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Falsche Konfiguration des SAS-Schlüssels bei der Datenbankwiederherstellung

Wenn die Shared Access Signature (SAS) in `CREDENTIAL` falsch ist, wird beim Lesen einer BAK-Datei durch `RESTORE DATABASE` möglicherweise konstant versucht, die BAK-Datei zu lesen, und nach einem längeren Zeitraum ein Fehler zurückgegeben. Führen Sie vor der Wiederherstellung einer Datenbank RESTORE HEADERONLY aus, um sicherzustellen, dass der SAS-Schlüssel korrekt ist.
Entfernen Sie das vorangestellte `?` aus dem über das Azure-Portal erstellten SAS-Schlüssel.

### <a name="tooling"></a>Tools

In SQL Server Management Studio und SQL Server Data Tools treten beim Zugriff auf eine verwaltete Instanz möglicherweise Probleme auf. Alle Probleme mit Tools werden vor der allgemeinen Verfügbarkeit behoben.

### <a name="incorrect-database-names"></a>Falsche Datenbanknamen

In einer verwalteten Instanz wird bei der Wiederherstellung oder in einigen Fehlermeldungen möglicherweise anstelle des Datenbanknamens der GUID-Wert angezeigt. Diese Probleme werden vor der allgemeinen Verfügbarkeit behoben.

### <a name="database-mail-profile"></a>Datenbank-E-Mail-Profil
Es kann nur ein Datenbank-E-Mail-Profil festgelegt werden, für das der Name `AzureManagedInstance_dbmail_profile` angegeben werden muss. Dabei handelt es sich um eine temporäre Einschränkung, die bald entfällt.

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Ein Tutorial, in dem die Erstellung einer neuen verwalteten Instanz gezeigt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md).

---
title: Verwaltete Azure SQL-Datenbank-Instanz – T-SQL-Unterschiede | Microsoft-Dokumentation
description: In diesem Artikel werden die T-SQL-Unterschiede zwischen einer verwalteten Instanz in Azure SQL-Datenbank und SQL Server beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: f1adcca48882ca3a149046cbc0729612666363cc
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734605"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server

Die Bereitstellungsoption „Verwaltete Instanz“ bietet umfassende Kompatibilität mit einer lokalen SQL Server-Datenbank-Engine. Die meisten Features der SQL Server-Datenbank-Engine werden in verwalteten Instanzen unterstützt.

![Migration](./media/sql-database-managed-instance/migration.png)

Es bestehen dennoch einige Unterschiede in der Syntax und im Verhalten. Diese Unterschiede werden in diesem Artikel zusammengefasst und erläutert. <a name="Differences"></a>
- [Verfügbarkeit](#availability), einschließlich der Unterschiede bei [Always On](#always-on-availability) und [Sicherungen](#backup)
- [Sicherheit](#security), einschließlich der Unterschiede bei [Überwachung](#auditing), [Zertifikaten](#certificates), [Anmeldeinformationen](#credentials), [Kryptografieanbietern](#cryptographic-providers), [Anmeldungen/Benutzern](#logins--users), [Dienstschlüssel und Diensthauptschlüssel](#service-key-and-service-master-key)
- [Konfiguration](#configuration), einschließlich der Unterschiede bei [Pufferpoolerweiterung](#buffer-pool-extension), [Sortierung](#collation), [Kompatibilitätsgraden](#compatibility-levels),[Datenbankspiegelung](#database-mirroring), [Datenbankoptionen](#database-options), [SQL Server-Agent](#sql-server-agent), [Tabellenoptionen](#tables)
- [Funktionen](#functionalities), einschließlich [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [verteilter Transaktionen](#distributed-transactions), [erweiterter Ereignisse](#extended-events), [externer Bibliotheken](#external-libraries), [Filestream und Filetable](#filestream-and-filetable), [semantischer Volltextsuche](#full-text-semantic-search), [Verbindungsserver](#linked-servers), [PolyBase](#polybase), [Replikation](#replication), [RESTORE](#restore-statement), [Service Broker](#service-broker), [gespeicherter Prozeduren, Funktionen und Trigger](#stored-procedures-functions-triggers)
- [Features mit abweichendem Verhalten in verwalteten Instanzen](#Changes)
- [Temporäre Einschränkungen und bekannte Probleme](#Issues)

## <a name="availability"></a>Verfügbarkeit

### <a name="always-on-availability"></a>Always On

[Hochverfügbarkeit](sql-database-high-availability.md) ist in verwalteten Instanzen integriert und kann von Benutzern nicht gesteuert werden. Folgende Anweisungen werden nicht unterstützt:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- Klausel [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) der Anweisung [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)

### <a name="backup"></a>Backup

In verwalteten Instanzen werden automatische Sicherungen durchgeführt, sodass Benutzer vollständige `COPY_ONLY`-Sicherungen für Datenbanken erstellen können. Differenzielle, Protokoll- und Dateimomentaufnahmesicherungen werden nicht unterstützt.

- Mit einer verwalteten Instanz können Sie eine Instanzdatenbank nur in einem Azure Blob Storage-Konto sichern:
  - Nur `BACKUP TO URL` wird unterstützt.
  - `FILE`, `TAPE` und Sicherungsmedien werden nicht unterstützt.  
- Die meisten allgemeinen `WITH`-Optionen werden unterstützt.
  - `COPY_ONLY` ist obligatorisch.
  - `FILE_SNAPSHOT` wird nicht unterstützt.
  - Bandoptionen: `REWIND`, `NOREWIND`, `UNLOAD` und `NOUNLOAD` werden nicht unterstützt.
  - Protokollspezifische Optionen: `NORECOVERY`, `STANDBY` und `NO_TRUNCATE` werden nicht unterstützt.

 Einschränkungen:  

- Mit einer verwalteten Instanz können Sie eine Instanzdatenbank in einer Sicherung mit bis zu 32 Stripes sichern. Dies ist ausreichend für Datenbanken mit bis zu 4 TB, wenn die Sicherungskomprimierung verwendet wird.
- Die maximale Stripegröße für Sicherungen ist 195 GB (maximale Blobgröße). Erhöhen Sie die Anzahl der Stripes im Sicherungsbefehl, um die einzelne Stripegröße zu verringern und diese Einschränkung einzuhalten.

> [!TIP]
> Um diese Einschränkung in der lokalen Umgebung zu umgehen, führen Sie anstelle einer Sicherung in `URL` eine Sicherung in `DISK` durch, laden Sie die Sicherungsdatei in das Blob hoch, und führen Sie dann die Wiederherstellung durch. Bei der Wiederherstellung werden größere Dateien unterstützt, da ein anderer Blobtyp verwendet wird.  

Informationen zu Sicherungen mithilfe von T-SQL finden Sie unter [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Sicherheit

### <a name="auditing"></a>Überwachung

Die wichtigsten Unterschiede zwischen der Überwachung in Datenbanken in Azure SQL-Datenbank und SQL Server:

- Bei der Bereitstellungsoption „Verwaltete Instanz“ in Azure SQL-Datenbank wird Überwachung auf Serverebene ausgeführt. Die Dateien mit der Endung `.xel` werden in einem Azure Blob Storage-Konto gespeichert.
- Bei Bereitstellungen als Singleton und in Pools für elastische Datenbanken in Azure SQL-Datenbank erfolgt die Überwachung auf Datenbankebene.
- Auf lokalen oder virtuellen SQL Server-Computern wird die Überwachung auf Serverebene ausgeführt, Ereignisse werden jedoch in Dateisystemprotokollen bzw. Windows-Ereignisprotokollen gespeichert.
  
Die XEvent-Überwachung in einer verwalteten Instanz unterstützt Azure Blob Storage-Ziele. Dateiprotokolle und Windows-Protokolle werden nicht unterstützt.

Wichtigste Unterschiede in der Syntax von `CREATE AUDIT` zur Überwachung in Azure Blob Storage:

- Mit der neuen Syntax `TO URL` können Sie die URL des Azure Blob Storage-Containers angeben, in dem Dateien mit der Endung `.xel` gespeichert werden.
- Die Syntax `TO FILE` wird nicht unterstützt, da verwaltete Instanzen nicht auf Windows-Dateifreigaben zugreifen können.

Weitere Informationen finden Sie unter  

- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Zertifikate

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen:

- `CREATE FROM`/`BACKUP TO` für Dateien wird für Zertifikate nicht unterstützt.
- `CREATE`/`BACKUP` für Zertifikate von `FILE`/`ASSEMBLY` wird nicht unterstützt. Dateien mit privaten Schlüsseln können nicht verwendet werden.  

Siehe [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) und [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Problemumgehung:** Erstellen Sie ein Skript für das Zertifikat und den privaten Schlüssel, speichern Sie es als SQL-Datei, und erstellen Sie sie aus der Binärdatei:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Anmeldeinformation

Nur Azure Key Vault- und `SHARED ACCESS SIGNATURE`-Identitäten werden unterstützt. Windows-Benutzer werden nicht unterstützt.

Siehe [CREATE CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) und [ALTER CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Kryptografieanbieter

Eine verwaltete Instanz kann nicht auf Dateien zugreifen. Daher können Kryptografieanbieter nicht erstellt werden:

- `CREATE CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [CREATE CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [ALTER CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins--users"></a>Anmeldungen/Benutzer

- Mithilfe von `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` und `FROM SID` erstellte SQL-Anmeldungen werden unterstützt. Siehe [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory-Anmeldungen, die mit der [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current)-Syntax oder der [CREATE USER](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current)-Syntax erstellt wurden, werden unterstützt (**öffentliche Vorschau**).
- Windows-Anmeldungen, die mit der Syntax `CREATE LOGIN ... FROM WINDOWS` erstellt wurden, werden nicht unterstützt. Verwenden Sie Azure Active Directory-Anmeldungen und -Benutzer.
- Der Azure Active Directory-Benutzer (Azure AD), der die Instanz erstellt hat, verfügt über [uneingeschränkte Administratorrechte](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Azure Active Directory-Benutzer (Azure AD) auf Datenbankebene ohne Administratorrechte können mit der Syntax `CREATE USER ... FROM EXTERNAL PROVIDER` erstellt werden. Siehe [CREATE USER ... FROM EXTERNAL PROVIDER](sql-database-manage-logins.md#non-administrator-users).

### <a name="service-key-and-service-master-key"></a>Dienstschlüssel und Diensthauptschlüssel

- Die [Sicherung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet).
- Die [Wiederherstellung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet).
- Die [Sicherung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet).
- Die [Wiederherstellung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) wird nicht unterstützt (wird durch SQL-Datenbank-Dienst verwaltet).

## <a name="configuration"></a>Konfiguration

### <a name="buffer-pool-extension"></a>Pufferpoolerweiterung

- [Pufferpoolerweiterung](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wird nicht unterstützt.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wird nicht unterstützt. Siehe [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Collation

Die standardmäßige Instanzsortierung ist `SQL_Latin1_General_CP1_CI_AS`, sie kann als Erstellungsparameter angegeben werden. Siehe [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Kompatibilitätsgrade

- Folgende Kompatibilitätsgrade werden unterstützt: 100, 110, 120, 130, 140  
- Kompatibilitätsgrade unter 100 werden nicht unterstützt.
- Der standardmäßige Kompatibilitätsgrad für neue Datenbanken ist 140. Bei wiederhergestellten Datenbanken bleibt der Kompatibilitätsgrad unverändert, wenn er zuvor bei 100 und höher lag.

Siehe [ALTER DATABASE-Kompatibilitätsgrad](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Datenbankspiegelung

Die Datenbankspiegelung wird nicht unterstützt.

- Die Optionen `ALTER DATABASE SET PARTNER` und `SET WITNESS` werden nicht unterstützt.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wird nicht unterstützt.

Weitere Informationen finden Sie unter [ALTER DATABASE SET PARTNER und SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) und [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

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

### <a name="sql-server-agent"></a>SQL Server-Agent

- SQL-Agent-Einstellungen sind schreibgeschützt. Die Prozedur `sp_set_agent_properties` wird in verwalteten Instanzen nicht unterstützt.  
- Aufträge
  - T-SQL-Auftragsschritte werden unterstützt.
  - Die folgenden Replikationsaufträge werden unterstützt:
    - Transaktionsprotokollleser  
    - Momentaufnahme
    - Verteiler
  - SSIS-Auftragsschritte werden unterstützt.
  - Andere Arten von Auftragsschritten werden derzeit nicht unterstützt, einschließlich:
    - Der Auftragsschritt Mergereplikation wird nicht unterstützt.  
    - Der Warteschlangenleser wird nicht unterstützt.  
    - Die Befehlsshell wird noch nicht unterstützt.
  - Verwaltete Instanzen können nicht auf externe Ressourcen zugreifen (z.B. Netzwerkfreigaben über Robocopy).  
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

## <a name="functionalities"></a>Funktionen

### <a name="bulk-insert--openrowset"></a>BULK INSERT/OPENROWSET

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher müssen die Dateien aus Azure Blob Storage importiert werden:

- Beim Importieren von Dateien aus Azure Blob Storage ist `DATASOURCE` im Befehl `BULK INSERT` erforderlich. Siehe [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- Beim Lesen von Inhalten einer Datei aus Azure Blob Storage ist `DATASOURCE` in der Funktion `OPENROWSET` erforderlich. Siehe [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen:

- Nur `CREATE ASSEMBLY FROM BINARY` wird unterstützt. Siehe [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` wird nicht unterstützt. Siehe [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kann nicht auf Dateien verweisen. Siehe [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).


### <a name="dbcc"></a>DBCC

Nicht dokumentierte DBCC-Anweisungen, die in SQL Server aktiviert sind, werden in verwalteten Instanzen nicht unterstützt.

- `Trace Flags` werden nicht unterstützt. Siehe [Ablaufverfolgungsflags](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` wird nicht unterstützt. Siehe [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` wird nicht unterstützt. Siehe [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Verteilte Transaktionen

Weder MSDTC noch [elastische Transaktionen](sql-database-elastic-transactions-overview.md) werden derzeit in verwalteten Instanzen unterstützt.

### <a name="extended-events"></a>Erweiterte Ereignisse

Einige Windows-spezifische Ziele für XEvents werden nicht unterstützt:

- `etw_classic_sync target` wird nicht unterstützt. Speichern Sie Dateien mit der Endung `.xel` in Azure Blob Storage. Siehe [etw_classic_sync-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target).
- `event_file target` wird nicht unterstützt. Speichern Sie Dateien mit der Endung `.xel` in Azure Blob Storage. Siehe [event_file-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

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
  - `GetFileNamespacePat)`
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

### <a name="polybase"></a>PolyBase

Externe Tabellen, die auf die Dateien in HDFS oder Azure Blob Storage verweisen, werden nicht unterstützt. Informationen zu PolyBase finden Sie unter [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replikation

Die Replikation ist für die öffentliche Vorschauversion der verwalteten Instanz verfügbar. Informationen zur Replikation finden Sie unter [SQL Server-Replikation](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

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
- Die Optionen `SINGLE_USER` und `RESTRICTED_USER` werden in `MULTI_USER` konvertiert.

 Einschränkungen:  

- `.BAK`-Dateien mit mehreren Sicherungssätzen können nicht wiederhergestellt werden.
- `.BAK`-Dateien mit mehreren Protokolldateien können nicht wiederhergestellt werden.
- Bei der Wiederherstellung treten Fehler auf, wenn die BAK-Datei `FILESTREAM`-Daten enthält.
- Sicherungen, die Datenbanken mit aktiven In-Memory-Objekten enthalten, können derzeit nicht wiederhergestellt werden.  
- Sicherungen, die Datenbanken enthalten, in denen zu einem bestimmten Zeitpunkt In-Memory-Objekte vorhanden waren, können derzeit nicht wiederhergestellt werden.
- Sicherungen, die Datenbanken im schreibgeschützten Modus enthalten, können derzeit nicht wiederhergestellt werden. Diese Einschränkung wird in Kürze aufgehoben.

Weitere Informationen zu Restore-Anweisungen finden Sie unter [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service Broker

Der instanzübergreifende Service Broker wird nicht unterstützt:

- `sys.routes` – Voraussetzung: Wählen Sie „address“ für „sys.routes“ aus. „address“ muss für jede Route auf „LOCAL“ festgelegt sein. Siehe [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: `CREATE ROUTE` kann ausschließlich mit dem Wert `LOCAL` für `ADDRESS` verwendet werden. Siehe [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: `ALTER ROUTE` kann ausschließlich mit dem Wert `LOCAL` für `ADDRESS` verwendet werden. Siehe [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

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
- `Extended stored procedures` werden nicht unterstützt, einschließlich `sp_addextendedproc` und `sp_dropextendedproc`. Siehe [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db` und `sp_detach_db` werden nicht unterstützt. Siehe [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql) und [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` wird nicht unterstützt. Siehe [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

## <a name="Changes"></a> Behavior Changes

Die folgenden Variablen, Funktionen und Sichten geben abweichende Ergebnisse zurück:

- `SERVERPROPERTY('EngineEdition')` gibt den Wert 8 zurück. Durch diese Eigenschaft wird eine verwaltete Instanz eindeutig identifiziert. Siehe [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` gibt NULL zurück, da das für SQL Server bestehende Konzept der Instanz nicht für eine verwaltete Instanz gilt. Siehe [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net. Siehe [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. `myinstance.domain.database.windows.net` für die Eigenschaften „name“ und „data_source“. Siehe [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` gibt NULL zurück, da das für SQL Server bestehende Konzept des Diensts nicht für eine verwaltete Instanz gilt. Siehe [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` wird unterstützt. Gibt NULL zurück, wenn die AAD-Anmeldung in „sys.syslogins“ nicht vorhanden ist. Siehe [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` wird nicht unterstützt. Gibt falsche Daten zurück (temporäres bekanntes Problem). Siehe [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` und andere integrierte Datum/Uhrzeit-Funktionen geben die Zeit immer in der UTC-Zeitzone zurück. Siehe [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bekannte Probleme und Einschränkungen

### <a name="tempdb-size"></a>TEMPDB-Größe

`tempdb` ist in 12 Dateien mit jeweils einer maximalen Größe von 14 GB unterteilt. Diese maximale Größe pro Datei kann nicht geändert werden. Zudem können `tempdb` keine neuen Dateien hinzugefügt werden. Diese Einschränkung wird in Kürze aufgehoben. Einige Abfragen geben möglicherweise einen Fehler zurück, wenn für sie mehr als 168 GB in `tempdb` erforderlich sind.

### <a name="exceeding-storage-space-with-small-database-files"></a>Überschreiten des Speicherplatzes mit kleinen Datenbankdateien

Jede verwaltete Instanz verfügt über bis zu 35 TB Speicher für Azure Premium-Datenträger, und jede Datenbankdatei wird auf einem separaten physischen Datenträger abgelegt. Mögliche Datenträgergrößen sind 128 GB, 256 GB, 512 GB, 1 TB oder 4 TB. Nicht verwendeter Speicherplatz auf dem Datenträger wird nicht berechnet, aber die Gesamtgröße der Azure Premium-Datenträger darf 35 TB nicht überschreiten. In einigen Fällen kann eine verwaltete Instanz, die nicht insgesamt 8 TB benötigt, aufgrund interner Fragmentierung das Azure-Limit von 35 TB überschreiten.

Eine verwaltete Instanz könnte beispielsweise über eine Datei mit einer Größe von 1,2 TB verfügen, die auf einem 4-TB-Datenträger gespeichert ist, und über 248 Dateien mit einer Größe von jeweils 1 GB, die auf separaten 128-GB-Datenträgern gespeichert sind. In diesem Beispiel:

- Die Gesamtgröße des zugewiesenen Datenträgerspeichers beträgt 1 x 4 TB + 248 x 128 GB = 35 TB.
- Der reservierte Gesamtspeicherplatz für Datenbanken in der Instanz beträgt 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Dies verdeutlicht, dass eine verwaltete Instanz unter bestimmten Umständen aufgrund einer spezifischen Verteilung von Dateien das Azure Premium-Datenträgerlimit in Höhe von 35 TB erreichen kann, wo Sie dies möglicherweise nicht erwarten.

In diesem Beispiel funktionieren vorhandene Datenbanken weiterhin und können ohne Probleme weiter wachsen, solange keine neuen Dateien hinzugefügt werden. Es könnten jedoch keine neuen Datenbanken erstellt oder wiederhergestellt werden, da für neue Plattenlaufwerke nicht genügend Speicherplatz vorhanden ist, selbst nicht dann, wenn die Gesamtgröße aller Datenbanken das Größenlimit der Instanz nicht überschreitet. Der Fehler, der in diesem Fall zurückgegeben wird, ist nicht klar.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Falsche Konfiguration des SAS-Schlüssels bei der Datenbankwiederherstellung

Wenn die Shared Access Signature (SAS) in `CREDENTIAL` falsch ist, wird beim Lesen einer BAK-Datei durch `RESTORE DATABASE` möglicherweise konstant versucht, die BAK-Datei zu lesen, und nach einem längeren Zeitraum ein Fehler zurückgegeben. Führen Sie vor der Wiederherstellung einer Datenbank RESTORE HEADERONLY aus, um sicherzustellen, dass der SAS-Schlüssel korrekt ist.
Entfernen Sie das vorangestellte `?` aus dem über das Azure-Portal erstellten SAS-Schlüssel.

### <a name="tooling"></a>Tools

In SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) kommt es beim Zugriff auf eine verwaltete Instanz möglicherweise zu Problemen.

- Die Verwendung von Azure AD-Anmeldungen und -Benutzern (**öffentliche Vorschau**) mit SSDT wird aktuell nicht unterstützt.
- Die Skripterstellung für Azure AD-Anmeldungen und -Benutzer (**öffentliche Vorschau**) wird in SSMS nicht unterstützt.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Falsche Datenbanknamen in einigen Ansichten, Protokollen und Meldungen

Mehrere Systemansichten, Leistungsindikatoren, Fehlermeldungen, XEvents und Fehlerprotokolleinträge zeigen GUID-Datenbankbezeichner anstelle der eigentlichen Datenbanknamen an. Verlassen Sie sich nicht auf diese GUID-Bezeichner, da sie in Zukunft durch tatsächliche Datenbanknamen ersetzt werden.

### <a name="database-mail-profile"></a>Datenbank-E-Mail-Profil

Es kann nur ein Datenbank-E-Mail-Profil festgelegt werden, für das der Name `AzureManagedInstance_dbmail_profile` angegeben werden muss.

### <a name="error-logs-are-not-persisted"></a>Fehlerprotokolle sind nicht persistent

Fehlerprotokolle, die in einer verwalteten Instanz verfügbar sind, sind nicht persistent und ihre Größe wird nicht in der maximalen Speicherbegrenzung berücksichtigt. Fehlerprotokolle werden im Falle eines Failovers ggf. automatisch gelöscht.

### <a name="error-logs-are-verbose"></a>Protokolle sind nicht ausführlich

Die verwaltete SQL-Datenbank-Instanz stellt ausführliche Informationen in Fehlerprotokollen zur Verfügung, von denen viele nicht relevant sind. Die Informationsmenge in Fehlerprotokollen wird in Zukunft reduziert.

**Problemumgehung**: Verwenden Sie eine benutzerdefinierte Prozedur zum Lesen von Fehlerprotokollen, mit der einige nicht relevante Einträge herausgefiltert werden. Weitere Informationen finden Sie unter [Verwaltete Instanz – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-is-not-supported"></a>Der gleiche Transaktionsbereich wird auf zwei Datenbanken in derselben Instanz nicht unterstützt

Die `TransactionScope`-Klasse in .Net funktioniert nicht, wenn zwei Abfragen an zwei Datenbanken in derselben Instanz unter dem gleichen Transaktionsbereich gesendet werden:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Obwohl dieser Code mit Daten innerhalb derselben Instanz funktioniert, erforderte er MS DTC.

**Problemumgehung**: Verwenden Sie [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase), um anstelle von zwei Verbindungen eine andere Datenbank im Verbindungskontext zu verwenden.

### <a name="clr-modules-and-linked-servers-sometime-cannot-reference-local-ip-address"></a>CLR-Module und Verbindungsserver können manchmal nicht auf lokale IP-Adressen verweisen

CLR-Module, die in einer verwalteten Instanz bereitgestellt werden, und Verbindungsserver/verteilte Abfragen, die auf die aktuelle Instanz verweisen, können manchmal nicht die IP-Adresse der lokalen Instanz auflösen. Dieser Fehler ist ein vorübergehendes Problem.

**Problemumgehung**: Verwenden Sie Kontextverbindungen nach Möglichkeit im CLR-Modul.

### <a name="tde-encrypted-databases-dont-support-user-initiated-backups"></a>Für Datenbanken mit TDE-Verschlüsselung werden keine vom Benutzer initiierten Sicherungen unterstützt.

Sie können `BACKUP DATABASE ... WITH COPY_ONLY` nicht für eine Datenbank ausführen, die per Transparent Data Encryption (TDE) verschlüsselt ist. Bei TDE wird erzwungen, dass Sicherungen mit internen TDE-Schlüsseln verschlüsselt werden. Da der Schlüssel nicht exportiert werden kann, können Sie die Sicherung nicht wiederherstellen.

**Problemumgehung**: Verwenden Sie automatische Sicherungen und die Point-in-Time-Wiederherstellung, oder deaktivieren Sie die Verschlüsselung für die Datenbank.

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Einen Schnellstart zum Erstellen einer neuen verwalteten Instanz finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md).

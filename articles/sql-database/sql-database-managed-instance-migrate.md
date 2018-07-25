---
title: Migrieren einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz migrieren.
keywords: Datenbankmigration,SQL Server-Datenbankmigration,Datenbankmigrationstools,Migrieren einer Datenbank,Migrieren einer SQL-Datenbank
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: bonova
ms.openlocfilehash: e0de9a1494641fef87d11545b99e5e7275f6b614
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069262"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz

Dieser Artikel enthält Informationen zu den Methoden, mit denen eine SQL Server 2005-Instanz (oder eine höhere Version) zu einer verwalteten Azure SQL-Datenbank-Instanz migriert wird (Vorschau). 

Die verwaltete Azure SQL-Datenbank-Instanz ist eine Erweiterung des vorhandenen SQL-Datenbank-Diensts, die neben einzelnen Datenbanken und Pools für elastische Datenbanken noch eine dritte Bereitstellungsoption bietet.  Sie ist so konzipiert, dass sie eine Datenbankmigration per Lift & Shift zu einem vollständig verwalteten PaaS ermöglicht, ohne dass die Anwendung neu gestaltet werden muss. Verwaltete SQL-Datenbank-Instanzen bieten umfassende Kompatibilität mit dem SQL Server-Programmiermodell sowie integrierte Unterstützung für die überwiegende Mehrheit von SQL Server-Features sowie die zugehörigen Tools und Dienste.

Im Überblick sieht der Prozess der Anwendungsmigration wie im folgenden Diagramm aus:

![Migrationsprozess](./media/sql-database-managed-instance-migration/migration-process.png)

- [Bewerten der Kompatibilität der verwalteten Instanz](sql-database-managed-instance-migrate.md#assess-managed-instance-compatibility)
- [Wählen einer App-Konnektivitätsoption](sql-database-managed-instance-migrate.md#choose-app-connectivity-option)
- [Bereitstellen für eine verwaltete Instanz mit optimaler Größe](sql-database-managed-instance-migrate.md#deploy-to-an-optimally-sized-managed-instance)
- [Auswählen der Migrationsmethode und Migration](sql-database-managed-instance-migrate.md#select-migration-method-and-migrate)
- [Überwachen von Anwendungen](sql-database-managed-instance-migrate.md#monitor-applications)

> [!NOTE]
> Informationen zum Migrieren einer einzelnen Datenbank zu einer einzelnen Datenbank oder eines Pools für elastische Datenbanken finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank in der Cloud](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Bewerten der Kompatibilität der verwalteten Instanz

Stellen Sie zunächst fest, ob die verwaltete Instanz mit den Datenbankanforderungen Ihrer Anwendung kompatibel ist. Verwaltete Instanzen wurden konzipiert, um eine einfache Migration per Lift & Shift für die meisten bestehenden Anwendungen zu ermöglichen, die SQL Server lokal oder auf virtuellen Computern verwenden. Es kann jedoch vorkommen, dass Sie Features oder Funktionen benötigen, die noch nicht unterstützt werden und die Kosten für die Implementierung eines Workarounds zu hoch sind. 

Verwenden Sie den [Datenmigrations-Assistenten (DMA)](https://docs.microsoft.com/sql/dma/dma-overview), um mögliche Kompatibilitätsprobleme zu erkennen, die die Datenbankfunktionalität der Azure SQL-Datenbank beeinträchtigen. Der DMA unterstützt noch keine verwalteten Instanzen als Migrationsziel, aber es wird empfohlen, die Bewertung der Azure SQL-Datenbank durchzuführen und die Liste der gemeldeten Funktionsparitäten und Kompatibilitätsprobleme anhand der Produktdokumentation sorgfältig zu überprüfen. Die meisten Blockierungsprobleme, die eine Migration zu einer Azure SQL-Datenbank verhindern, wurden mit der verwalteten Instanz beseitigt. Beispielsweise stehen Features wie datenbankübergreifende Abfragen, datenbankübergreifende Transaktionen innerhalb derselben Instanz, verknüpfte Server mit anderen SQL-Quellen, CLR, globale temporäre Tabellen, Ansichten auf Instanzebene, Service Broker und ähnliches in verwalteten Instanzen zur Verfügung. 

Es gibt jedoch einige Fälle, in denen Sie über eine Alternative nachdenken müssen, wie in [SQL Server auf virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) beschrieben. Hier einige Beispiele:

- Wenn Sie direkten Zugriff auf das Betriebssystem oder das Dateisystem benötigen, z.B. um Drittanbieteragents oder benutzerdefinierte Agents auf demselben virtuellen SQL Server-Computer zu installieren.
- Wenn Sie unbedingt von Features abhängig sind, die noch nicht unterstützt werden, wie z.B. FileStream / FileTable, PolyBase und instanzübergreifende Transaktionen.
- Wenn Sie unbedingt bei einer bestimmten Version von SQL Server (z.B. 2012) bleiben müssen.
- Wenn Ihre Computeanforderungen wesentlich niedriger sind, als bei einer verwalteten Instanz in der öffentlichen Vorschau (z.B. ein V-Kern) und eine Datenbankkonsolidierung nicht in Frage kommt.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Bereitstellen für eine verwaltete Instanz mit optimaler Größe

Verwaltete Instanzen sind auf lokale Workloads zugeschnitten, die zum Verschieben in die Cloud vorgesehen sind. Sie führen ein neues Erwerbsmodell ein, das eine größere Flexibilität bei der Auswahl der richtigen Ressourcen für Ihre Workloads bietet. In der lokalen Umgebung sind Sie wahrscheinlich daran gewöhnt, diese Workloads mit physischen Kernen zu dimensionieren. Das neue Erwerbsmodell für verwaltete Instanzen basiert auf virtuellen Kernen oder „V-Kernen“, wobei zusätzlicher Speicher und EA separat erhältlich sind. Das V-Kern-Modell ist eine einfachere Methode, um die Computeanforderungen in der Cloud mit dem, was Sie heute lokal verwenden, zu vergleichen. Mit diesem neuen Modell können Sie die Größe Ihrer Zielumgebung in der Cloud anpassen.

Sie haben die Möglichkeit, Compute- und Speicherressourcen zum Zeitpunkt der Bereitstellung auszuwählen und anschließend zu ändern, ohne dass es zu Downtime für Ihre Anwendung kommt.

![Dimensionierung einer verwalteten Instanz](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Informationen zum Erstellen der VNet-Infrastruktur und der verwalteten Instanz finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-create-tutorial-portal.md).

> [!IMPORTANT]
> Es ist wichtig, dass Sie Ihr Ziel-VNet und Subnetz immer in Übereinstimmung mit den [VNet-Anforderungen für verwaltete Instanzen](sql-database-managed-instance-vnet-configuration.md#requirements) halten. Jede Inkompatibilität kann Sie daran hindern, neue Instanzen anzulegen oder bereits erstellte Instanzen zu verwenden.

## <a name="select-migration-method-and-migrate"></a>Auswählen der Migrationsmethode und Migration

Verwaltete Instanzen zielen auf Benutzerszenarien ab, die eine Massenmigration der Datenbank von lokalen oder IaaS-Datenbankimplementierungen erfordern. Sie sind die optimale Wahl, wenn es darum geht, das Back-End der Anwendungen, die regelmäßig Funktionalitäten auf Instanzebene und/oder über Datenbanken hinweg nutzen, per Lift & Shift zu migrieren. Wenn dies Ihr Szenario ist, können Sie eine ganze Instanz in eine entsprechende Umgebung in Azure verschieben, ohne dass Sie Ihre Anwendungen neu strukturieren müssen. 

Zum Verschieben von SQL-Instanzen müssen Sie Folgendes sorgfältig planen:

-   Die Migration aller Datenbanken, die zusammengeführt werden müssen (diejenigen, die auf derselben Instanz ausgeführt werden).
-   Die Migration von Objekten auf Instanzebene, von denen Ihre Anwendung abhängt, wie Anmeldeinformationen, SQL-Agentaufträge und -operatoren sowie Trigger auf Serverebene. 

Managed Instance ist ein vollständig verwalteter Dienst, der es Ihnen ermöglicht, einige der regulären DBA-Aktivitäten während deren Einbindung an die Plattform zu delegieren. Daher müssen einige Daten auf Instanzebene nicht migriert werden, wie z.B. Wartungsaufträge für regelmäßige Sicherungen oder Always On-Konfigurationen, da [Hochverfügbarkeit](sql-database-high-availability.md) integriert ist.

Verwaltete Instanzen unterstützen die folgenden Datenbankmigrationsoptionen (derzeit sind dies die einzigen unterstützten Migrationsmethoden):

- Azure Database Migration Service – Migrationen nahezu ohne Ausfallzeiten
- Native RESTORE from URL – nutzt native Sicherungen von SQL Server, ist mit einer gewissen Ausfallzeit verbunden

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

Der [Azure Database Migration Service (DMS)](../dms/dms-overview.md) ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Dieser Dienst optimiert die Aufgaben, die erforderlich sind, um bestehende Drittanbieter- und SQL Server-Datenbanken nach Azure zu verschieben. Zu den Bereitstellungsoptionen bei der öffentlichen Vorschau gehören Azure SQL-Datenbank, Managed Instance und SQL Server in einem virtuellen Azure-Computer. DMS ist die empfohlene Migrationsmethode für Ihre Unternehmensworkloads. 

Wenn Sie SQL Server Integration Services (SSIS) auf SQL Server lokal verwenden, unterstützt DMS noch nicht die Migration des SSIS-Katalogs (SSISDB), in dem SSIS-Pakete gespeichert werden. Sie können jedoch die Azure SSIS Integration Runtime (IR) in Azure Data Factory (ADF) bereitstellen, mit der eine neue SSISDB in Azure SQL-Datenbank bzw. einer verwalteten Instanz erstellt wird. Anschließend können Sie dort das Paket erneut bereitstellen. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

Weitere Informationen zu diesem Szenario und zu Konfigurationsschritten für DMS finden Sie unter [Migrate your on-premises database to Managed Instance using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migrieren Ihrer lokalen Datenbank zu einer verwalteten Instanz mit DMS).  

### <a name="native-restore-from-url"></a>Native RESTORE FROM URL-Option

Das Wiederherstellen mit der RESTORE-Option von nativen Sicherungen (.bak-Dateien), die von lokalen SQL Server-Instanzen oder von [SQL Servern auf virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) erstellt wurden, die in [Azure Storage](https://azure.microsoft.com/services/storage/) verfügbar sind, ist eine der wichtigsten Funktionen von verwalteten SQL-Datenbank-Instanzen, die eine schnelle und einfache Offlinedatenbankmigration ermöglicht. 

Das folgende Diagramm bietet einen allgemeinen Überblick über den Prozess:

![Migrationsflow](./media/sql-database-managed-instance-migration/migration-flow.png)

In der folgenden Tabelle finden Sie weitere Informationen über die Methoden, die Sie je nach Version der ausgeführten SQL Server-Quellinstanz verwenden können:

|Schritt|SQL-Engine und -Version|Sicherungs- und Wiederherstellungsmethode|
|---|---|---|
|Sicherung auf Azure Storage legen|Vor SQL 2012 SP1 CU2|Direkter Upload der .bak-Datei in Azure Storage|
||2012 SP1 CU2 – 2016|Direkte Sicherung mit veralteter [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql)-Syntax|
||Ab 2016|Direkte Sicherung mit [WITH SAS CREDENTIAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Aus Azure Storage in verwalteter Instanz wiederherstellen|[RESTORE FROM URL mit SAS CREDENTIAL](sql-database-managed-instance-restore-from-backup-tutorial.md)|

> [!IMPORTANT]
> - Beim Migrieren einer Datenbank, die durch [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) geschützt ist, zu einer verwalteten Azure SQL-Instanz mithilfe der nativen Wiederherstellungsoption muss das entsprechende Zertifikat aus dem lokalen oder IaaS-SQL-Server vor der Wiederherstellung der Datenbank migriert werden. Eine ausführliche Schrittbeschreibung finden Sie unter [Migrieren von TDE zur verwalteten Instanz](sql-database-managed-instance-migrate-tde-certificate.md).
> - Das Wiederherstellen von Systemdatenbanken wird nicht unterstützt. Um Objekte auf Instanzebene (gespeichert in Master- oder msdb-Datenbanken) zu migrieren, wird empfohlen, diese zu skripten und T-SQL-Skripts auf der Zielinstanz auszuführen.

Ein vollständiges Tutorial über das Wiederherstellen einer Datenbanksicherung auf einer verwalteten Instanz mithilfe von SAS-Anmeldeinformationen finden Sie unter [Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-restore-from-backup-tutorial.md).

## <a name="monitor-applications"></a>Überwachen von Anwendungen

Verfolgen Sie das Anwendungsverhalten und die Leistung nach der Migration. In der verwalteten Instanz werden einige Änderungen erst dann übernommen, wenn der [Kompatibilitätsgrad der Datenbank geändert wurde](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Die Datenbankmigration zur Azure SQL-Datenbank behält in den meisten Fällen ihren ursprünglichen Kompatibilitätsgrad bei. Wenn der Kompatibilitätsgrad einer Benutzerdatenbank vor der Migration 100 oder höher war, bleibt er auch nach der Migration unverändert. Lag der Kompatibilitätsgrad einer Benutzerdatenbank vor der Migration bei 90, wird er in der aktualisierten Datenbank auf 100 gesetzt, was der niedrigste unterstützte Kompatibilitätsgrad in einer verwalteten Instanz ist. Der Kompatibilitätsgrad von Systemdatenbanken ist 140.

Um Migrationsrisiken zu minimieren, sollten Sie den Kompatibilitätsgrad der Datenbank erst nach der Leistungsüberwachung ändern. Verwenden Sie den Abfragespeicher als optimales Tool, um Informationen über die Workloadleistung vor und nach einer Änderung des Datenbankkompatibilitätsgrads zu erhalten. Siehe [Aufrechterhalten einer stabilen Leistung während des Upgrades auf das neuere SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Sobald Sie sich auf einer vollständig verwalteten Plattform befinden, profitieren Sie von den Vorteilen, die automatisch als Teil des SQL-Datenbankdienstes bereitgestellt werden. Beispielsweise müssen Sie keine Sicherungen für den Managed Instance-Dienst erstellen, da dieser automatische Sicherungen für Sie  durchführt. Sie müssen sich auch nicht mehr darum kümmern, Sicherungen zu planen, zu erstellen und zu verwalten. Verwaltete Instanzen bieten Ihnen die Möglichkeit, mithilfe von [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) eine Wiederherstellung auf jeden beliebigen Zeitpunkt innerhalb der Aufbewahrungsdauer durchzuführen. Bei der öffentlichen Vorschau ist die Aufbewahrungsdauer auf sieben Tage festgelegt.
Darüber hinaus müssen Sie sich keine Gedanken über die Einrichtung von [Hochverfügbarkeit](sql-database-high-availability.md) machen, da diese integriert ist.

Um die Sicherheit zu erhöhen, sollten Sie einige der verfügbaren Features nutzen:
- Azure Active Directory-Authentifizierung auf Datenbankebene
- Überwachung und Bedrohungserkennung für Aktivitäten
- Kontrollierter Zugriff auf sensible und vertrauliche Daten ([Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) und [dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking))

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu verwalteten Instanzen finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial zu einer Wiederherstellung aus einer Sicherung finden Sie unter [Create a Managed Instance](sql-database-managed-instance-create-tutorial-portal.md) (Erstellen einer verwalteten Instanz).
- Ein Tutorial zur Migration mit DMS finden Sie unter [Migrate your on-premises database to Managed Instance using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migrieren Ihrer lokalen Datenbank zu einer verwalteten Instanz mit DMS).  

---
title: Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank | Microsoft Docs
description: Erstellen Sie eine neue Azure SQL-Datenbank, indem Sie eine BACPAC-Datei importieren.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/11/2019
ms.openlocfilehash: 754f2845911307cdd698bff4aa3e891f5c1bcdbd
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234780"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Schnellstart: Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank

Sie können eine SQL Server-Datenbank mithilfe einer [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac)-Datei in eine Datenbank in Azure SQL-Datenbank importieren. Sie können die Daten aus einer `BACPAC`-Datei, die im Azure Blob Storage gespeichert ist (nur Standardspeicher), oder aus dem lokalen Speicher an einem lokalen Speicherort importieren. Um die Importgeschwindigkeit durch die Bereitstellung von mehr und schnelleren Ressourcen zu maximieren, skalieren Sie Ihre Datenbank während des Importvorgangs auf eine höhere Dienstebene und Computegröße. Sie können dann nach erfolgreichem Import zentral herunterskalieren. 

> [!NOTE]
> Der Kompatibilitätsgrad der importierten Datenbank beruht auf dem Kompatibilitätsgrad der Quelldatenbank.
> [!IMPORTANT]
> Nachdem Sie die Datenbank importiert haben, können Sie wählen, ob die Datenbank mit dem aktuellen Kompatibilitätsgrad (Ebene 100 für die Datenbank „AdventureWorks2008R2“) oder mit einem höheren Grad ausgeführt werden soll. Weitere Informationen zu den Auswirkungen und Optionen für das Ausführen einer Datenbank mit einem bestimmten Kompatibilitätsgrad finden Sie unter [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lesen Sie auch [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql). Dort finden Sie Informationen zu weiteren Einstellungen auf Datenbankebene, die sich auf Kompatibilitätsgrade beziehen.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Importieren aus einer BACPAC-Datei im Azure-Portal

In diesem Abschnitt wird gezeigt, wie Sie im [Azure-Portal](https://portal.azure.com) eine Azure SQL-Datenbank aus einer `BACPAC`-Datei erstellen, die im Azure Blob Storage gespeichert ist. Das Portal unterstützt *nur* das Importieren einer BACPAC-Datei aus Azure Blob Storage.

> [!NOTE]
> Bei [einer verwalteten Instanz](sql-database-managed-instance.md) ist es derzeit nicht möglich, eine Datenbank aus einer `BACPAC`-Datei über das Azure-Portal in eine Instanzdatenbank zu migrieren.

Wenn Sie über das Azure-Portal in eine einzelne Datenbank importieren möchten, öffnen Sie die Seite für den Datenbankserver für diese Datenbank, und wählen Sie auf der Symbolleiste **Datenbank importieren** aus.  

   ![Datenbankimport](./media/sql-database-import/import.png)

Wählen Sie das Speicherkonto, den Container und die zu importierende `BACPAC`-Datei aus. Geben Sie die Größe der neuen Datenbank (in der Regel identisch mit dem Ursprung) und die SQL Server-Anmeldeinformationen für das Ziel an. Eine Liste der möglichen Werte für eine neue Azure SQL-Datenbank finden Sie unter [Create Database](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

### <a name="monitor-imports-progress"></a>Überwachen des Fortschritts eines Importvorgangs

Um den Fortschritt eines Importvorgangs zu überwachen, öffnen Sie die Seite für den Server der Datenbank, und wählen Sie unter **Einstellungen** die Option **Import-/Exportverlauf** aus. Bei erfolgreicher Ausführung weist der Import den Status **Abgeschlossen** auf.

Zum Überprüfen, ob die Datenbank auf dem Datenbankserver aktiv ist, wählen Sie **SQL-Datenbanken** aus, und prüfen Sie, ob der Status der neuen Datenbank **Online** lautet.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importieren aus einer BACPAC-Datei mit SqlPackage

Informationen zum Importieren einer SQL Server-Datenbank mit dem Befehlszeilenprogramm [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) finden Sie unter [Importparameter und -eigenschaften](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). Im Lieferumfang von SqlPackage sind die neuesten Versionen von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) enthalten. Sie können die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) auch aus dem Microsoft Download Center herunterladen.

Aus Gründen der Skalierbarkeit und Leistung wird die Verwendung von SqlPackage (statt des Azure-Portals) für die meisten Produktionsumgebungen empfohlen. Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von `BACPAC`-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).

Mit dem folgenden SqlPackage-Befehl wird die Datenbank **AdventureWorks2008R2** aus dem lokalen Speicher in einen Azure SQL-Datenbankserver mit dem Namen **mynewserver20170403** importiert. Er erstellt eine neue Datenbank namens **myMigratedDatabase** mit der **Premium**-Dienstebene und dem Dienstziel **P6**. Ändern Sie diese Werte entsprechend Ihrer Umgebung.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Wenn Sie hinter einer Unternehmensfirewall eine Verbindung mit einem SQL-Datenbankserver herstellen möchten, der eine einzelne Datenbank verwaltet, muss Port 1433 in der Firewall geöffnet sein. Um eine Verbindung mit einer verwalteten Instanz herzustellen, benötigen Sie eine [Point-to-Site-Verbindung](/sql-database-managed-instance-configure-p2s.md) oder eine ExpressRoute-Verbindung.
>

In diesem Beispiel wird gezeigt, wie eine Datenbank mithilfe von SqlPackage mit universeller Active Directory-Authentifizierung importiert wird.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importieren aus einer BACPAC-Datei mit PowerShell

Verwenden Sie das [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport)-Cmdlet, um eine Anforderung für einen Datenbankimport beim Azure SQL-Datenbankdienst einzureichen. Je nach Größe der Datenbank nimmt der Importvorgang einige Zeit in Anspruch.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 Mit dem Cmdlet [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) können Sie den Status des Importvorgangs überprüfen. Wenn Sie das Cmdlet direkt nach der Anforderung ausführen, wird in der Regel **Status: InProgress** zurückgegeben. Der Import ist abgeschlossen, wenn **Status: Succeeded** angezeigt wird.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Ein weiteres Skriptbeispiel finden Sie unter [Importieren einer Datenbank aus einer BACPAC-Datei](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Einschränkungen

Das Importieren in eine Datenbank in einen Pool für elastische Datenbanken wird nicht unterstützt. Sie können jedoch Daten in eine Einzeldatenbank importieren und die Datenbank anschließend in einen Pool für elastische Datenbanken verschieben.

## <a name="import-using-wizards"></a>Importieren mithilfe von Assistenten

Sie können auch die folgenden Assistenten verwenden:

- [Assistent zum Importieren der Datenebenenanwendung in SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server-Import/Export-Assistent](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Herstellen einer Verbindung mit einer importierten SQL-Datenbank und zum Abfragen einer solchen Datenbank finden Sie unter [Schnellstart: Azure SQL-Datenbank: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung und Abfragen von Daten](sql-database-connect-query-ssms.md).
- Einen Blogbeitrag des SQL Server-Kundenberatungsteams zur Migration mithilfe von BACPAC-Dateien finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).
- Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-single-database-migrate.md).
- Informationen zum sicheren Verwalten und Freigeben von Speicherschlüsseln und SAS finden Sie im [Azure Storage-Sicherheitsleitfaden](https://docs.microsoft.com/azure/storage/common/storage-security-guide).

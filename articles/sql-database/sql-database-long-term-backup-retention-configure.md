---
title: 'Azure SQL-Datenbank: Verwalten der langfristigen Aufbewahrung von Sicherungen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie automatisierte Sicherungen im SQL Azure-Speicher speichern und dann wiederherstellen.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: df3d843516bce30253c23080716e606dfb56f25e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211560"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Verwalten der langfristigen Aufbewahrung von Sicherungen in Azure SQL-Datenbank

Sie können Azure SQL-Datenbank mit einer Richtlinie für die [langfristige Aufbewahrung der Sicherung](sql-database-long-term-retention.md) (Long-Term Retention, LTR) konfigurieren, um automatisch Sicherungen in Azure-Blobspeicher für bis zu zehn Jahre aufzubewahren. Sie können dann mit dem Azure-Portal oder mit PowerShell eine Datenbank anhand dieser Sicherungen wiederherstellen.

> [!NOTE]
> Bei der ersten Version der Vorschau dieses Features im Oktober 2016 wurden Sicherungen im Azure Services Recovery Service-Tresor gespeichert. Bei diesem Update wird die Abhängigkeit entfernt, aus Gründen der Abwärtskompatibilität wird die ursprüngliche API jedoch bis zum 31. Mai 2018 unterstützt. Wenn Sie mit Sicherungen im Azure Services Recovery-Tresor interagieren müssen, lesen Sie die Informationen unter [Configure and restore from Azure SQL Database long-term backup retention using Azure Recovery Services Vault](sql-database-long-term-backup-retention-configure-vault.md) (Konfigurieren und Wiederherstellen auf der Grundlage der langfristigen Sicherungsaufbewahrung in Azure SQL-Datenbank mit dem Azure Recovery Services-Tresor). 

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung und Wiederherstellen von Sicherungen mithilfe des Azure-Portals

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe des Azure-Portals die langfristige Aufbewahrung konfigurieren, Sicherungen in der langfristigen Aufbewahrung anzeigen und Sicherungen aus der langfristigen Aufbewahrung wiederherstellen.

### <a name="configure-long-term-retention-policies"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung

Sie können SQL-Datenbank [zur Aufbewahrung von automatisierten Sicherungen](sql-database-long-term-retention.md) für einen längeren Zeitraum konfigurieren, als gemäß der Aufbewahrungsdauer für Ihre Dienstebene vorgesehen ist. 

1. Wählen Sie im Azure-Portal Ihre SQL Server-Instanz aus, und klicken Sie dann auf **Langfristige Sicherungsaufbewahrung**.

   ![Link „Long-term backup retention“ (Langfristige Beibehaltung der Sicherung)](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Wählen Sie auf der Registerkarte **Configure policies** (Richtlinien konfigurieren) die Datenbank aus, für die Sie Richtlinien für die langfristige Sicherungsaufbewahrung festlegen oder ändern möchten.

   ![Auswählen der Datenbank](./media/sql-database-long-term-retention/ltr-configure-select-database.png)

3. Wählen Sie im Bereich **Configure policies** (Richtlinien konfigurieren) aus, ob Sie wöchentliche, monatliche oder jährliche Sicherungen aufbewahren möchten, und geben Sie den jeweiligen Aufbewahrungszeitraum an. 

   ![Konfigurieren von Richtlinien](./media/sql-database-long-term-retention/ltr-configure-policies.png)

4. Klicken Sie auf **Anwenden**, wenn Sie fertig sind.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Anzeigen von Sicherungen und Wiederherstellen einer Sicherung mithilfe des Azure-Portals

Zeigen Sie die Sicherungen an, die für eine bestimmte Datenbank mit einer LTR-Richtlinie aufbewahrt werden, und stellen Sie diese Sicherungen wieder her. 

1. Wählen Sie im Azure-Portal Ihre SQL Server-Instanz aus, und klicken Sie dann auf **Langfristige Sicherungsaufbewahrung**.

   ![Link „Long-term backup retention“ (Langfristige Beibehaltung der Sicherung)](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Wählen Sie auf der Registerkarte **Verfügbare Sicherungen** die Datenbank aus, für die Sie verfügbare Sicherungen anzeigen möchten.

   ![Auswählen der Datenbank](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. Überprüfen Sie im Bereich **Verfügbare Sicherungen** die verfügbaren Sicherungen. 

   ![Anzeigen von Sicherungen](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Wählen Sie die Sicherung aus, die Sie wiederherstellen möchten, und geben Sie dann den neuen Datenbanknamen an.

   ![Wiederherstellen](./media/sql-database-long-term-retention/ltr-restore.png)

5. Klicken Sie auf **OK**, um die Datenbank auf Grundlage der Sicherung im Azure SQL-Speicher in der neuen Datenbank wiederherzustellen.

6. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

   ![Status des Wiederherstellungsauftrags (aus dem Tresor)](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags die Seite **SQL-Datenbanken**, um die neu wiederhergestellte Datenbank anzuzeigen.

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung und Wiederherstellen von Sicherungen mithilfe von PowerShell

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe von PowerShell die langfristige Sicherungsaufbewahrung konfigurieren, Sicherungen in Azure SQL-Speicher anzeigen und eine Sicherung in Azure SQL-Speicher wiederherstellen.

> [!IMPORTANT]
> Sie müssen die aktuelle AzureRM-PowerShell verwenden, um die LTR V2-Richtlinien einzurichten. Die aktuelle Version ist [AzureRM 4.5.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0-preview). Hierbei handelt es sich um eine Vorschauversion, also verwenden Sie diesen Befehl zur Installation: `Install-Module -Name AzureRM.Sql -AllowPrerelease -Force`.
> Anweisungen zum Installieren der Vorabversion finden Sie unter [Installieren von PowerShellGet](https://docs.microsoft.com/en-us/powershell/gallery/installing-psget). Das Release von AzureRM-PowerShell von Mai 2018 erscheint in wenigen Tagen (voraussichtliche am 18.5.2018). Wenn Sie die Releaseversion nach ihrer Verfügbarkeit installieren, können Sie den Schalter -AllowPrelease ignorieren und den folgenden Befehl verwenden: `Install-Module -Name AzureRM.Sql -Force`.

### <a name="create-an-ltr-policy"></a>Erstellen einer LTR-Richtlinie

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Anzeigen von LTR-Richtlinien
In diesem Beispiel wird gezeigt, wie Sie die LTR-Richtlinien auf einem Server auflisten.

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Löschen einer LTR-Richtlinie
In diesem Beispiel wird gezeigt, wie Sie eine LTR-Richtlinie aus einer Datenbank löschen.

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Anzeigen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie die LTR-Sicherungen auf einem Server auflisten. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Löschen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung aus der Liste der Sicherungen löschen.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>Wiederherstellen von LTR-Sicherungen
Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung wiederherstellen. Beachten Sie, dass sich diese Schnittstelle nicht geändert hat, der Parameter für die Ressourcen-ID jetzt jedoch die Ressourcen-ID der LTR-Sicherung erfordert. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).

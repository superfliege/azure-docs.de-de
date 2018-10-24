---
title: 'Azure SQL-Datenbank: Verwalten der langfristigen Aufbewahrung von Sicherungen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie automatisierte Sicherungen im SQL Azure-Speicher speichern und dann wiederherstellen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: 1775e1810a164bfbdd1cddea9360674592cf446c
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857532"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Verwalten der langfristigen Aufbewahrung von Sicherungen in Azure SQL-Datenbank

In Azure SQL-Datenbank können Sie eine einzelne oder eine gruppierte Datenbank mit einer Richtlinie für die [langfristige Aufbewahrung von Sicherungen](sql-database-long-term-retention.md) (Long-Term Retention, LTR) konfigurieren, um automatisch Sicherungen in Azure Blob Storage für bis zu zehn Jahre aufzubewahren. Sie können dann mit dem Azure-Portal oder mit PowerShell eine Datenbank anhand dieser Sicherungen wiederherstellen.

> [!IMPORTANT]
> Die [verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) unterstützt derzeit keine langfristigen Aufbewahrung von Sicherungen.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung und Wiederherstellen von Sicherungen mithilfe des Azure-Portals
In den folgenden Abschnitten wird erläutert, wie Sie mithilfe des Azure-Portals die langfristige Aufbewahrung konfigurieren, Sicherungen in der langfristigen Aufbewahrung anzeigen und Sicherungen aus der langfristigen Aufbewahrung wiederherstellen.

### <a name="configure-long-term-retention-policies"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung

Sie können SQL-Datenbank [zur Aufbewahrung von automatisierten Sicherungen](sql-database-long-term-retention.md) für einen längeren Zeitraum konfigurieren, als gemäß der Aufbewahrungsdauer für Ihre Dienstebene vorgesehen ist. 

1. Wählen Sie im Azure-Portal Ihren SQL Server aus, und klicken Sie dann auf **Sicherungen verwalten**. Aktivieren Sie auf der Registerkarte **Richtlinien konfigurieren** das Kontrollkästchen fürdie Datenbank, für die Sie Richtlinien für die langfristige Sicherungsaufbewahrung festlegen oder ändern möchten.

   ![Link zum Verwalten von Sicherungen](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Wählen Sie im Bereich **Configure policies** (Richtlinien konfigurieren) aus, ob Sie wöchentliche, monatliche oder jährliche Sicherungen aufbewahren möchten, und geben Sie den jeweiligen Aufbewahrungszeitraum an. 

   ![Konfigurieren von Richtlinien](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Klicken Sie auf **Anwenden**, wenn Sie fertig sind.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Anzeigen von Sicherungen und Wiederherstellen einer Sicherung mithilfe des Azure-Portals

Zeigen Sie die Sicherungen an, die für eine bestimmte Datenbank mit einer LTR-Richtlinie aufbewahrt werden, und stellen Sie diese Sicherungen wieder her. 

1. Wählen Sie im Azure-Portal Ihren SQL Server aus, und klicken Sie dann auf **Sicherungen verwalten**. Wählen Sie auf der Registerkarte **Verfügbare Sicherungen** die Datenbank aus, für die Sie verfügbare Sicherungen anzeigen möchten.

   ![Auswählen der Datenbank](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. Überprüfen Sie im Bereich **Verfügbare Sicherungen** die verfügbaren Sicherungen. 

   ![Anzeigen von Sicherungen](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Wählen Sie die Sicherung aus, die Sie wiederherstellen möchten, und geben Sie dann den neuen Datenbanknamen an.

   ![Wiederherstellen](./media/sql-database-long-term-retention/ltr-restore.png)

5. Klicken Sie auf **OK**, um die Datenbank auf Grundlage der Sicherung im Azure SQL-Speicher in der neuen Datenbank wiederherzustellen.

6. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

   ![Status des Wiederherstellungsauftrags](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags die Seite **SQL-Datenbanken**, um die neu wiederhergestellte Datenbank anzuzeigen.

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung und Wiederherstellen von Sicherungen mithilfe von PowerShell

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe von PowerShell die langfristige Sicherungsaufbewahrung konfigurieren, Sicherungen in Azure SQL-Speicher anzeigen und eine Sicherung in Azure SQL-Speicher wiederherstellen.

> [!IMPORTANT]
> Die LTR V2-API wird in den folgenden PowerShell-Versionen unterstützt:
- [AzureRM.Sql-4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) oder höher
- [AzureRM-6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) oder höher
> 

### <a name="rbac-roles-to-manage-long-term-retention"></a>RBAC-Rollen zum Verwalten der langfristigen Aufbewahrung

Zum Verwalten von LTR-Sicherungen müssen Sie über eine der folgenden Rollen verfügen: 
- Besitzer des Abonnements
- SQL Server-Mitwirkender im Bereich **Abonnement**
- SQL-Datenbankmitwirkender im Bereich **Abonnement**

Wenn eine präzisere Steuerung erforderlich ist, können Sie benutzerdefinierte RBAC-Rollen erstellen und diese im Bereich **Abonnement** zuweisen. 

Für **Get-AzureRmSqlDatabaseLongTermRetentionBackup** und **Restore-AzureRmSqlDatabase** muss die Rolle über folgende Berechtigungen verfügen:

Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read
 
Für **Remove-AzureRmSqlDatabaseLongTermRetentionBackup** muss die Rolle über folgende Berechtigungen verfügen:

Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


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
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
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

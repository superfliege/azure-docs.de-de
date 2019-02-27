---
title: Verwalten und Überwachen von SQL Server-Datenbanken auf einer mit Azure Backup gesicherten Azure-VM | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie SQL Server-Datenbanken wiederherstellen, die auf einer Azure-VM ausgeführt und mit Azure Backup gesichert werden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 1c2ce0ba42f0bc3efd1dcc951113b05ab6941b98
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430954"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Verwalten und Überwachen gesicherter SQL Server-Datenbanken 


In diesem Artikel werden allgemeine Aufgaben zur Verwaltung und Überwachung von SQL Server-Datenbanken beschrieben, die auf einer Azure-VM ausgeführt werden, welche mit dem [Azure Backup](backup-overview.md)-Dienst in einem Azure Backup Recovery Services-Tresor gesichert wird. Zu diesen Aufgaben gehören das Überwachen von Aufträgen und Warnungen, das Beenden und Fortsetzen des Datenbankschutzes, das Ausführen von Sicherungsaufträgen und das Aufheben der Registrierung eines virtuellen Computers bei einer Sicherung.


> [!NOTE]
> Die Sicherung von SQL Server-Datenbanken auf einer Azure-VM mit Azure Backup befindet sich derzeit in der öffentlichen Vorschau.


Wenn Sie für die SQL Server-Datenbanken keine Sicherung konfiguriert haben, befolgen Sie die Anweisungen in [diesem Artikel](backup-azure-sql-database.md).

## <a name="monitor-backup-jobs"></a>Überwachen von Sicherungsaufträgen

###  <a name="monitor-ad-hoc-jobs-in-the-portal"></a>Überwachen von Ad-hoc-Aufträgen im Portal

Azure Backup zeigt alle manuell ausgelösten Aufträge im Portal für **Sicherungsaufträge** an, darunter das Ermitteln und Registrieren von Datenbanken sowie Sicherungs- und Wiederherstellungsvorgänge.

![Portal „Sicherungsaufträge“](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Geplante Sicherungsaufträge werden im Portal für **Sicherungsaufträge** nicht angezeigt. Verwenden Sie SQL Server Management Studio, um geplante Sicherungsaufträge zu überwachen, wie im nächsten Abschnitt beschrieben.
>

### <a name="monitor-backup-jobs-with-sql-server-management-studio"></a>Überwachen von Sicherungsaufträgen mit SQL Server Management Studio 

Azure Backup nutzt für alle Sicherungsvorgänge native SQL-APIs.

Mit den nativen APIs können Sie alle Auftragsinformationen aus der [SQL-Sicherungssatztabelle](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in der MSDB-Datenbank abrufen.

Das folgende Beispiel zeigt eine Abfrage zum Abrufen aller Sicherungsaufträge für eine Datenbank mit dem Namen **DB1**. Passen Sie die Abfrage für eine erweiterte Überwachung an.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

## <a name="view-backup-alerts"></a>Anzeigen von Sicherungswarnungen

Da Protokollsicherungen alle 15 Minuten stattfinden, kann die Überwachung von Sicherungsaufträgen mühsam sein. Azure Backup erleichtert die Überwachung mithilfe von E-Mail-Warnungen.

- Für alle Sicherungsfehler werden Warnungen ausgelöst.
- Benachrichtigungen werden auf Datenbankebene nach Fehlercode konsolidiert.
- Eine E-Mail-Benachrichtigung wird nur für den ersten Sicherungsfehler in einer Datenbank gesendet. 

So überwachen Sie Sicherungswarnungen

1. Melden Sie sich bei Ihrem Azure-Abonnement im [Azure-Portal](https://portal.azure.com) an, um Datenbankwarnungen zu überwachen.

2. Wählen Sie im Tresordashboard **Warnungen und Ereignisse** aus.

   ![Auswählen von „Warnungen und Ereignisse“](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Wählen Sie unter **Warnungen und Ereignisse** die Option **Sicherungswarnungen** aus.

   ![Auswählen von „Sicherungswarnungen“](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Beenden des Schutzes für eine SQL Server-Datenbank

Sie können die Sicherung einer SQL Server-Datenbank auf verschiedene Arten beenden:

* Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte
* Beenden aller zukünftigen Sicherungsaufträge und Beibehalten der Wiederherstellungspunkte

Beachten Sie Folgendes:

Wenn Sie die Wiederherstellungspunkte beibehalten, werden die Punkte in Übereinstimmung mit der Sicherungsrichtlinie bereinigt. Es fallen Gebühren für die geschützte Instanz und den genutzten Speicher an, bis alle Wiederherstellungspunkte bereinigt wurden. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/backup/) zu den Preisen.
- Wenn Sie Wiederherstellungspunkte beibehalten, obwohl sie gemäß der Aufbewahrungsrichtlinie ablaufen, speichert Azure Backup immer einen letzten Wiederherstellungspunkt, bis Sie die Sicherungsdaten explizit löschen.
- Wenn Sie eine Datenquelle löschen, ohne die Sicherung zu beenden, treten bei neuen Sicherungen Fehler auf. In diesem Fall laufen alte Wiederherstellungspunkte gemäß der Richtlinie ab, aber ein letzter Wiederherstellungspunkt wird immer beibehalten, bis Sie die Sicherung beenden und die Daten löschen.
- Sie können die Sicherung für eine Datenbank, die für den automatischen Schutz aktiviert ist, erst beenden, wenn der automatische Schutz deaktiviert wurde.

Gehen Sie wie folgt vor, um den Schutz für eine Datenbank zu beenden:

1. Wählen Sie im Dashboard des Tresors unter **Nutzung** die Option **Sicherungselemente** aus.

    ![Öffnen des Menüs „Sicherungselemente“](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

2. Wählen Sie unter **Sicherungsverwaltungstyp** die Option **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“](./media/backup-azure-sql-database/sql-restore-backup-items.png)


3. Wählen Sie die Datenbank aus, für die Sie den Schutz beenden möchten.

    ![Auswählen der Datenbank zum Beenden des Schutzes](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


5. Wählen Sie im Datenbankmenü **Sicherung beenden** aus.

    ![Auswählen von „Sicherung beenden“](./media/backup-azure-sql-database/stop-db-button.png)


6. Wählen Sie im Menü **Sicherung beenden** aus, ob Daten beibehalten oder gelöscht werden sollen. Geben Sie optional einen Grund und einen Kommentar ein.

    ![Menü „Sicherung beenden“](./media/backup-azure-sql-database/stop-backup-button.png)

7. Klicken Sie auf **Sicherung beenden**.

  

### <a name="resume-protection-for-a-sql-database"></a>Fortsetzen des Schutzes für eine SQL-­Datenbank-Instanz

Wenn Sie beim Beenden des Schutzes für die SQL-Datenbank die Option **Sicherungsdaten beibehalten** auswählen, wird der Schutz fortgesetzt. Wenn die Sicherungsdaten nicht beibehalten wurden, kann der Schutz nicht fortgesetzt werden.

1. Um den Schutz für die SQL-Datenbank fortzusetzen, öffnen Sie das Sicherungselement, und wählen Sie **Sicherung fortsetzen** aus.

    ![Auswählen von „Sicherung fortsetzen“, um den Datenbankschutz fortzusetzen](./media/backup-azure-sql-database/resume-backup-button.png)

2. Wählen Sie im Menü **Sicherungsrichtlinie** eine Richtlinie aus, und klicken Sie dann auf **Speichern**.

## <a name="run-an-on-demand-backup"></a>Ausführen einer bedarfsgesteuerten Sicherung

Sie können verschiedene Arten von bedarfsgesteuerten Sicherungen ausführen:

* Vollständige Sicherung
* Vollständige Kopiesicherung
* Differenzielle Sicherung
* Protokollsicherung

[Erfahren Sie mehr](backup-architecture.md#sql-server-backup-types) über SQL Server-Sicherungstypen.

## <a name="unregister-a-sql-server-instance"></a>Registrierung einer SQL Server-Instanz aufheben

Heben Sie die Registrierung einer SQL Server-Instanz auf, nachdem Sie den Schutz deaktiviert haben und bevor Sie den Tresor löschen:

1. Wählen Sie im Dashboard des Tresors unter **Verwalten** die Option **Sicherungsinfrastruktur** aus.  

   ![Auswählen von „Sicherungsinfrastruktur“](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Wählen Sie unter **Verwaltungsserver** **Geschützte Server** aus.

   ![Auswählen von „Geschützte Server“](./media/backup-azure-sql-database/protected-servers.png)


3. Wählen Sie unter **Geschützte Server** den Server aus, dessen Registrierung Sie aufheben möchten. Um den Tresor zu löschen, müssen Sie die Registrierung aller Server aufheben.

4. Klicken Sie mit der rechten Maustaste auf den geschützten Server und dann auf **Löschen**.

   ![Auswählen von „Löschen“](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Nächste Schritte

[Lesen Sie](backup-sql-server-azure-troubleshoot.md) Problembehandlungsinformationen für die SQL Server-Datenbanksicherung.

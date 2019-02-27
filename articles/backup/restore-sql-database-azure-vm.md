---
title: Wiederherstellen gesicherter SQL Server-Datenbanken auf einer Azure-VM mit Azure Backup | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie SQL Server-Datenbanken wiederherstellen, die auf einer Azure-VM ausgeführt und mit Azure Backup gesichert werden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 9b741f8562ae2e81d297357afd3b0e0e3976a248
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56445439"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Wiederherstellen von SQL Server-Datenbanken auf virtuellen Azure-Computern 


In diesem Artikel wird die Wiederherstellung einer SQL Server-Datenbank beschrieben, die auf einer Azure-VM ausgeführt wird, welche mit dem [Azure Backup](backup-overview.md)-Dienst in einem Azure Backup Recovery Services-Tresor gesichert wurde.


> [!NOTE]
> Die Sicherung von SQL Server-Datenbanken auf einer Azure-VM mit Azure Backup befindet sich derzeit in der öffentlichen Vorschau.


Dieser Artikel beschreibt die Wiederherstellung von SQL Server-Datenbanken. Wenn Sie für die Datenbanken keine Sicherung konfiguriert haben, befolgen Sie die Anweisungen in [diesem Artikel](backup-azure-sql-database.md).



## <a name="about-restoring-databases"></a>Informationen zum Wiederherstellen von Datenbanken

Azure Backup kann auf virtuellen Azure-Computern ausgeführte SQL Server-Datenbanken folgendermaßen wiederherstellen:

- Wiederherstellung eines bestimmten Datums oder einer bestimmten Uhrzeit (sekundengenau) mithilfe von Transaktionsprotokollsicherungen. Azure Backup ermittelt automatisch die geeignete vollständige differenzielle Sicherung und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten basierend auf dem ausgewählten Zeitpunkt benötigt werden.
- Wiederherstellung einer bestimmten vollständigen oder differenziellen Sicherung, um die Daten auf einen bestimmten Wiederherstellungspunkt und nicht auf einen bestimmten Zeitpunkt wiederherzustellen.


### <a name="prerequisites"></a>Voraussetzungen

Beachten Sie vor dem Wiederherstellen einer Datenbank Folgendes:

- Sie können die Datenbank auf einer SQL Server-Instanz in derselben Azure-Region wiederherstellen.
- Der Zielserver muss bei demselben Tresor wie die Quelle registriert werden.
- Um eine mit TDE verschlüsselte Datenbank auf einer anderen SQL Server-Instanz wiederherzustellen, [stellen Sie zuerst das Zertifikat auf dem Zielserver wieder her](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Bevor Sie eine Wiederherstellung der Datenbank „master“ auslösen, starten Sie die SQL Server-Instanz mit der Startoption **-m AzureWorkloadBackup** im Einzelbenutzermodus.
    - Der Wert für **-m** ist der Name des Clients.
    - Nur der angegebene Clientname darf die Verbindung öffnen.
- Beenden Sie für alle Systemdatenbanken („model“, „master“, „msdb“) vor dem Auslösen der Wiederherstellung den SQL Agent-Dienst.
- Schließen Sie alle Anwendungen, die ggf. versuchen, eine Verbindung mit einer dieser Datenbanken zu belegen.

## <a name="restore-a-database"></a>Wiederherstellen einer Datenbank

Zum Wiederherstellen benötigen Sie die folgenden Berechtigungen:

* Berechtigungen vom Typ **Sicherungsoperator** in dem Tresor, in dem Sie die Wiederherstellung durchführen.
* Zugriff **Mitwirkender (Schreiben)** auf die gesicherte Quell-VM.
* Zugriff **Mitwirkender (Schreiben)** auf die Ziel-VM:
    - Wenn Sie die Daten auf derselben VM wiederherstellen, entspricht diese der Quell-VM.
    - Wenn Sie sie an einem anderen Speicherort wiederherstellen, ist dieser die neue Ziel-VM. 

Gehen Sie zur Wiederherstellung wie folgt vor:
1. Öffnen Sie den Tresor, in dem die SQL Server-VM registriert ist.
2. Wählen Sie im Dashboard des Tresors unter **Nutzung** die Option **Sicherungselemente** aus.

    ![Öffnen des Menüs „Sicherungselemente“](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Wählen Sie im Menü **Sicherungselemente** unter **Sicherungsverwaltungstyp** die Option **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Wählen Sie die wiederherzustellende Datenbank aus.

    ![Auswählen der wiederherzustellenden Datenbank](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Überprüfen Sie das Datenbankmenü. Es enthält folgende Informationen über die Datenbanksicherung: 

    * Die ältesten und neuesten Wiederherstellungspunkte
    * Den Protokollsicherungsstatus der letzten 24 Stunden (für Datenbanken in einem vollständigen und massenprotokollierten Wiederherstellungsmodell, falls für Transaktionsprotokollsicherungen konfiguriert)

6. Klicken Sie auf **Datenbank wiederherstellen**. 

    ![Auswählen von „Datenbank wiederherstellen“](./media/backup-azure-sql-database/restore-db-button.png)

7. Geben Sie unter **Wiederherstellungskonfiguration** an, wo Sie die Daten wiederherstellen möchten:
    - **Alternativer Standort:** Die Datenbank wird an einem alternativen Speicherort wiederhergestellt und behält die ursprüngliche Quelldatenbank bei.
    - **Datenbank überschreiben:** Die Daten werden auf derselben SQL Server-Instanz wiederhergestellt, auf der sich auch die ursprüngliche Quelle befunden hat. Dadurch wird die ursprüngliche Datenbank überschrieben.

    > [!Important]
    > Wenn die ausgewählte Datenbank zu einer Always On-Verfügbarkeitsgruppe gehört, lässt SQL Server das Überschreiben der Datenbank nicht zu. Nur **Alternativer Speicherort** ist verfügbar.
    >

    ![Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

1. Wählen Sie im Menü **Wiederherstellungskonfiguration** unter **Zielort der Wiederherstellung** die Option **Alternativer Speicherort** aus.
2. Wählen Sie den SQL Server-Namen und die Instanz aus, in der Sie die Datenbank wiederherstellen möchten.
3. Geben Sie im Feld **Name der wiederhergestellten Datenbank** den Namen der Zieldatenbank ein.
4. Aktivieren Sie ggf. **Überschreiben, wenn die gleichnamige Datenbank bereits in der ausgewählten SQL-Instanz vorhanden ist**.
5. Klicken Sie auf **OK**.

    ![Angeben von Werten für das Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Wählen Sie unter **Wiederherstellungspunkt auswählen** aus, ob Sie [einen bestimmten Zeitpunkt](#restore-to-a-specific-point-in-time) oder einen [bestimmten Wiederherstellungspunkt](#restore-to-a-specific-restore-point) wiederherstellen möchten.

    > [!NOTE]
    > Die Point-in-Time-Wiederherstellung ist nur für Transaktionsprotokollsicherungen für Datenbanken mit einem vollständigen und massenprotokollierten Wiederherstellungsmodell verfügbar. 


### <a name="restore-and-overwrite"></a>Wiederherstellen und Überschreiben

1. Wählen Sie im Menü **Wiederherstellungskonfiguration** unter **Zielort der Wiederherstellung** die Option **Datenbank überschreiben** > **OK** aus.

    ![Auswählen von „Datenbank überschreiben“](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. Wählen Sie unter **Wiederherstellungspunkt auswählen** die Option **Protokolle (Zeitpunkt)** aus, um [einen bestimmten Zeitpunkt wiederherzustellen](#restore-to-a-specific-point-in-time) bzw. **Vollständig und differenziell**, um einen [bestimmten Wiederherstellungspunkt](#restore-to-a-specific-restore-point) wiederherzustellen.

    > [!NOTE]
    > Die Point-in-Time-Wiederherstellung ist nur für Transaktionsprotokollsicherungen für Datenbanken mit einem vollständigen und massenprotokollierten Wiederherstellungsmodell verfügbar. 




    
### <a name="restore-to-a-specific-point-in-time"></a>Wiederherstellen eines bestimmten Zeitpunkts

Wenn Sie **Protokolle (Zeitpunkt)** als Wiederherstellungstyp ausgewählt haben, gehen Sie folgendermaßen vor:

1.  Wählen Sie unter **Datum/Uhrzeit für Wiederherstellung** den Minikalender aus. Im **Kalender** enthalten die fett angezeigten Daten Wiederherstellungspunkte. Das aktuelle Datum wird hervorgehoben.
2. Wählen Sie ein Datum mit Wiederherstellungspunkten aus. Daten ohne Wiederherstellungspunkte können nicht ausgewählt werden.

    ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

3. Wenn Sie ein Datum ausgewählt haben, zeigt das Zeitachsendiagramm die verfügbaren Wiederherstellungspunkte in einem fortlaufenden Bereich an.
4. Geben Sie eine Uhrzeit für die Wiederherstellung über das Zeitachsendiagramm an, oder wählen Sie einen Zeitpunkt aus. Klicken Sie dann auf **OK**.

    ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

 
4. Aktivieren Sie im Menü **Erweiterte Konfiguration** die Option **Mit NORECOVERY wiederherstellen**, wenn die Datenbank nach der Wiederherstellung nicht den Betrieb aufnehmen soll.
5. Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Zielpfad ein.
6. Klicken Sie auf **OK**.

    ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)


7. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten.
8. Verfolgen Sie den Wiederherstellungsfortschritt im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

    ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)



### <a name="restore-to-a-specific-restore-point"></a>Wiederherstellen eines bestimmten Wiederherstellungspunkts

Wenn Sie **Vollständig und differenziell** als Wiederherstellungstyp ausgewählt haben, gehen Sie folgendermaßen vor:


1. Wählen Sie aus der Liste einen Wiederherstellungspunkt aus, und klicken Sie auf **OK**, um die Vorgehensweise für Wiederherstellungspunkte abzuschließen.

    ![Auswählen eines vollständigen Wiederherstellungspunkts](./media/backup-azure-sql-database/choose-fd-recovery-point.png)
        
2. Aktivieren Sie im Menü **Erweiterte Konfiguration** die Option **Mit NORECOVERY wiederherstellen**, wenn die Datenbank nach der Wiederherstellung nicht den Betrieb aufnehmen soll.
3. Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Zielpfad ein. 
4. Klicken Sie auf **OK**.

    ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

7. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten.
8. Verfolgen Sie den Wiederherstellungsfortschritt im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

    ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Nächste Schritte

[Verwalten und überwachen](manage-monitor-sql-database-backup.md) Sie SQL Server-Datenbanken, die mit Azure Backup gesichert wurden.

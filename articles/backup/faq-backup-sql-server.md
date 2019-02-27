---
title: Häufig gestellte Fragen zur Sicherung von SQL Server-Datenbanken auf Azure-VMs mit Azure Backup
description: Enthält Antworten auf häufig gestellte Fragen zur Sicherung von SQL Server-Datenbanken auf Azure-VMs mit Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: a14406733ff60d53d4bf7792ff0c9a015c57d9b3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430924"
---
# <a name="faq-on-sql-server-running-on-azure-vm-backup"></a>Häufig gestellte Fragen zur Sicherung von SQL Server auf Azure-VMs

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Sicherung von SQL Server-Datenbanken, die auf Azure-VMs ausgeführt werden, mit dem Dienst [Azure Backup](backup-overview.md).

> [!NOTE]
> Dieses Feature ist zurzeit als öffentliche Preview verfügbar.



## <a name="can-i-throttle-the-backup-speed"></a>Kann ich die Sicherungsrate verringern?

Ja. Sie können die Rate verringern, mit der die Sicherungsrichtlinie ausgeführt wird, um die Auswirkungen auf eine SQL Server-Instanz zu minimieren. So ändern Sie die Einstellung:
1. Erstellen Sie auf der SQL Server-Instanz im Ordner *C:\Programme\Azure Workload Backup\bin* die Datei **ExtensionSettingsOverrides.json**.
2. Ändern Sie in der Datei **ExtensionSettingsOverrides.json** die Einstellung **DefaultBackupTasksThreshold** in einen kleineren Wert (z. B. „5“). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Speichern Sie die Änderungen. Schließen Sie die Datei.
4. Öffnen Sie auf der SQL Server-Instanz **Task-Manager**. Starten Sie den Dienst **AzureWLBackupCoordinatorSvc** neu.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kann ich eine vollständige Sicherung aus einem sekundären Replikat ausführen?
Nein. Diese Funktion wird nicht unterstützt.

## <a name="do-successful-backup-jobs-create-alerts"></a>Erstellen erfolgreiche Sicherungsaufträge Warnungen?

Nein. Erfolgreiche Sicherungsaufträge generieren keine Warnungen. Warnungen werden nur für Sicherungsaufträge gesendet, bei denen ein Fehler aufgetreten ist.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Werden geplanten Sicherungsaufträge im Menü mit den Aufträgen angezeigt?

Nein. Im Menü **Sicherungsaufträge** werden die Details von bedarfsgesteuerten Aufträgen angezeigt, aber keine weiteren Informationen zu geplanten Sicherungsaufträgen. Wenn bei geplanten Sicherungsaufträgen Fehler auftreten, finden Sie die entsprechenden Details in den Warnungen der fehlerhaften Aufträge. Verwenden Sie [SQL Server Management Studio](manage-monitor-sql-database-backup.md), um alle geplanten und Ad-hoc-Sicherungsaufträge zu überwachen.

## <a name="are-future-databases-automatically-added-for-backup"></a>Werden zukünftige Datenbanken für die Durchführung von Sicherungen automatisch hinzugefügt?

Nein. Wenn Sie den Schutz für eine SQL Server-Instanz konfigurieren, werden alle Datenbanken hinzugefügt, wenn Sie die Severebenenoption auswählen. Wenn Sie einer SQL Server-Instanz nach der Schutzkonfiguration Datenbanken hinzufügen, müssen Sie die neuen Datenbanken manuell hinzufügen, um diese zu schützen. Die Datenbanken werden nicht automatisch in den konfigurierten Schutz eingeschlossen.

##  <a name="how-do-i-restart-protection-after-changing-recovery-type"></a>Wie kann ich den Schutz neu starten, nachdem der Wiederherstellungstyp geändert wurde?

Lösen Sie eine vollständige Sicherung aus. Protokollsicherungen starten wie erwartet.

## <a name="can-i-protect-availability-groups-on-premises"></a>Kann ich Verfügbarkeitsgruppen lokal schützen?

Nein. Azure Backup schützt SQL Server-Instanzen, die in Azure ausgeführt werden. Wenn eine Verfügbarkeitsgruppe auf Azure und lokale Computer verteilt ist, kann die Verfügbarkeitsgruppe nur geschützt werden, wenn das primäre Replikat in Azure ausgeführt wird. Außerdem schützt Azure Backup nur die Knoten, die in derselben Azure-Region ausgeführt werden wie der Recovery Services-Tresor.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kann ich Verfügbarkeitsgruppen regionsübergreifend schützen?

Der Azure Backup Recovery Services-Tresor kann alle Knoten erkennen und schützen, die sich in der gleichen Region wie der Recovery Services-Tresor befinden. Wenn sich Ihre SQL Always On-Verfügbarkeitsgruppe über mehrere Azure-Regionen erstreckt, müssen Sie die Sicherung von der Region aus konfigurieren, die über den primären Knoten verfügt. Azure Backup kann alle Datenbanken in der Verfügbarkeitsgruppe gemäß der Sicherungseinstellung erkennen und schützen. Wenn die Sicherung nicht entsprechend der Einstellung erfolgt, tritt bei den Sicherungen ein Fehler auf, und Sie erhalten eine Fehlermeldung.

## <a name="can-i-exclude-databases-with-auto-protection-enabled"></a>Kann ich bei aktiviertem automatischem Schutz Datenbanken ausschließen?

Nein, der [automatische Schutz](backup-azure-sql-database.md#enable-auto-protection) gilt für die gesamte Instanz. Sie können mithilfe des automatischen Schutzes nicht Datenbanken einer Instanz selektiv schützen.

## <a name="can-i-have-different-policies-in-an-auto-protected-instance"></a>Kann ich für eine automatisch geschützte Instanz unterschiedliche Richtlinien verwenden?

Wenn Sie bereits über einige geschützte Datenbanken in einer Instanz verfügen, werden diese auch noch durch ihre jeweiligen Richtlinien geschützt, nachdem Sie den [automatischen Schutz](backup-azure-sql-database.md#enable-auto-protection) **aktiviert** haben. Allerdings gilt für alle nicht geschützten Datenbanken sowie diejenigen, die Sie in der Zukunft hinzufügen würden, nur eine einzige Richtlinie, die Sie unter **Sicherung konfigurieren** nach Auswahl der Datenbanken definieren. Im Gegensatz zu anderen geschützten Datenbanken können Sie sogar nicht einmal die Richtlinie für eine Datenbank unter einer automatisch geschützten Instanz ändern.
Die einzige Möglichkeit, dies zu tun, besteht darin, den automatischen Schutz der Instanz zunächst zu deaktivieren und dann die Richtlinie für diese Datenbank zu ändern. Sie können dann den automatischen Schutz für diese Instanz erneut aktivieren.

## <a name="if-i-delete-a-database-from-auto-protection-will-backups-stop"></a>Wird die Durchführung von Sicherungen gestoppt, wenn ich eine Datenbank aus dem automatischen Schutz lösche?

Nein, wenn eine Datenbank aus einer automatisch geschützten Instanz gelöscht wird, wird weiterhin versucht, die Datenbank zu sichern. Dies bedeutet auch, dass die gelöschte Datenbank fortan unter **Sicherungselemente** als fehlerhaft angezeigt und immer noch als geschützt behandelt wird.

Sie können den Schutz dieser Datenbank nur beenden, indem Sie den [automatischen Schutz](backup-azure-sql-database.md#enable-auto-protection) für die Instanz zunächst deaktivieren und dann die Option **Sicherung beenden** unter **Sicherungselemente** für diese Datenbank auswählen. Sie können dann den automatischen Schutz für diese Instanz erneut aktivieren.

##  <a name="why-cant-i-see-an-added-database-for-an-auto-protected-instance"></a>Warum kann ich eine hinzugefügte Datenbank für eine geschützte Instanz mit automatischem Schutz nicht sehen?

Eine Datenbank, die einer [automatisch geschützten Instanz](backup-azure-sql-database.md#enable-auto-protection) neu hinzugefügt wird, wird unter Umständen nicht sofort unter den geschützten Elementen aufgeführt. Dies liegt daran, dass die Ermittlung in der Regel alle 8 Stunden ausgeführt wird. Allerdings kann der Benutzer eine manuelle Ermittlung mit der Option **DBs wiederherstellen** durchführen, um neue Datenbanken sofort zu ermitteln und zu schützen, wie in der folgenden Abbildung gezeigt:

  ![Neu hinzugefügte Datenbank anzeigen](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr dazu](backup-azure-sql-database.md), wie Sie eine SQL Server-Datenbank sichern, die auf einer Azure-VM ausgeführt wird.

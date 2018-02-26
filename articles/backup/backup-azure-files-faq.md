---
title: "Häufig gestellte Fragen zum Sichern von Azure Files"
description: "Dieser Artikel enthält ausführliche Informationen zum Schützen von Azure Files in Azure. Diese Zusammenfassung wird in den Suchergebnissen angezeigt."
services: backup
keywords: "Vermeiden Sie es, Schlüsselwörter hinzuzufügen oder zu bearbeiten, ohne Ihren SEO-Experten zurate zu ziehen."
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: d37e119709bc9d4643fcaa9512b5209d4139515e
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Fragen zum Sichern von Azure Files
In diesem Artikel werden allgemeine Fragen zum Sichern von Azure Files beantwortet. Einige Antworten enthalten Links zu Artikeln mit umfassenderen Informationen. Außerdem können Sie Fragen zum Azure Backup-Dienst im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)stellen.

Verwenden Sie die Links auf der rechten Seite unter **In diesem Artikel**, um sich einen Überblick über die Abschnitte dieses Artikels zu verschaffen.

## <a name="configuring-the-backup-job-for-azure-files"></a>Konfigurieren des Sicherungsauftrags für Azure Files

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-file-shares-br"></a>Warum werden einige Speicherkonten nicht angezeigt, die geschützt werden sollen und gültige Dateifreigaben enthalten? <br/>
Die Azure Files-Sicherung ist derzeit als Vorschau verfügbar, und nur unterstützte Speicherkonten können für die Sicherung konfiguriert werden. Stellen Sie sicher, dass es sich beim gewünschten Speicherkonto um ein unterstütztes Speicherkonto handelt.

### <a name="why-cant-i-see-some-of-my-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Warum werden beim Konfigurieren der Sicherung einige Dateifreigaben im Speicherkonto nicht angezeigt? <br/>
Überprüfen Sie, ob die Dateifreigabe bereits im gleichen Recovery Services-Tresor geschützt wird. Vergewissern Sie sich, dass die Dateifreigabe, die Sie schützen möchten, nicht vor Kurzem gelöscht wurde.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Warum kann ich keine Dateifreigaben löschen, die mit einer Synchronisierungsgruppe in Azure Files Sync verbunden sind? <br/>
Die Schutzfunktion für Azure-Dateifreigaben, die mit Synchronisierungsgruppen verbunden sind, ist als eingeschränkte Vorschau verfügbar. Schreiben Sie unter Angabe Ihrer Abonnement-ID an [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com), um Zugriff anzufordern. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>An welchen geografischen Standorten kann ich Azure-Dateifreigaben sichern? <br/>
Die Sicherung für Azure-Dateifreigaben befindet sich derzeit in der Vorschauphase und ist nur an folgenden geografischen Standorten verfügbar: 
-   Kanada, Mitte (CNC)
-   Kanada, Osten (CE) 
-   USA, Mitte (CUS)
-   Asien, Osten (EA)
-   Australien, Osten (AE) 
-   Indien, Mitte (INC) 
-   USA, Norden-Mitte (NCUS) 
-   Vereinigtes Königreich, Süden (UKS) 
-   Vereinigtes Königreich, Westen (UKW) 
-   USA, Westen-Mitte (WCUS)
-   USA, Westen 2 (WUS 2)

Die Sicherung für Azure-Dateifreigaben wird ab dem *23. Februar* an den folgenden geografischen Standorten verfügbar sein:
-   Australien, Südosten (ASE) 
-   Brasilien, Süden (BRS) 
-   USA, Osten (EUS) 
-   USA, Osten 2 (EUS2) 
-   Europa, Norden (NE) 
-   USA, Süden-Mitte (SCUS) 
-   Asien, Südosten (SEA)
-   Europa, Westen (WE) 
-   USA, Westen (WUS)  

Schreiben Sie an [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com), wenn Sie die Version an einem bestimmten geografischen Standort verwenden müssen, der oben nicht aufgeführt ist.

### <a name="how-many-file-shares-can-i-protect-in-a-vaultbr"></a>Wie viele Dateifreigaben kann ich in einem Tresor schützen?<br/>
Während der Vorschauphase können Sie Dateifreigaben aus bis zu 25 Speicherkonten pro Tresor schützen. Sie können auch bis zu 200 Dateifreigaben in einem einzelnen Tresor schützen. 

## <a name="backup"></a>Sicherung

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Wie viele bedarfsgesteuerte Sicherungen kann ich pro Dateifreigabe erstellen? <br/>
Sie können jederzeit bis zu 200 Momentaufnahmen für eine Dateifreigabe besitzen, einschließlich der Momentaufnahmen, die von Azure Backup gemäß Ihrer Richtlinie erstellt werden. Wenn bei Ihren Sicherungen ein Fehler auftritt, da dieser Grenzwert erreicht wurde, löschen Sie entsprechend bedarfsgesteuerte Wiederherstellungspunkte.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Nach dem Aktivieren von virtuellen Netzwerken in meinem Speicherkonto ist beim Sichern der Dateifreigaben im Konto ein Fehler aufgetreten. Warum?
Die Azure Files-Sicherung wird derzeit nicht für Speicherkonten unterstützt, für die virtuelle Netzwerke aktiviert sind. Deaktivieren Sie die virtuellen Netzwerke in Speicherkonten, die Sie sichern möchten. 

## <a name="restore"></a>Restore 

### <a name="can-i-recover-from-a-deleted-file-share-br"></a>Kann die Wiederherstellung auf der Grundlage einer gelöschten Dateifreigabe erfolgen? <br/>
Wenn Sie versuchen, eine Dateifreigabe zu löschen, wird eine Liste der Sicherungen angezeigt, die ebenfalls gelöscht werden, wenn Sie den Vorgang fortsetzen. Außerdem müssen Sie den Löschvorgang bestätigen. Sie können keine Wiederherstellung auf der Grundlage einer gelöschten Dateifreigabe durchführen.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-a-file-share-br"></a>Kann ich Sicherungen wiederherstellen, wenn ich den Schutz für eine Dateifreigabe beendet habe? <br/>
Ja. Wenn Sie beim Beenden des Schutzes die Option **Sicherungsdaten beibehalten** ausgewählt haben, können Sie alle vorhandenen Wiederherstellungspunkte wiederherstellen.

## <a name="manage-backup"></a>Verwalten von Sicherungen

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Kann ich auf die von Azure Backup erstellten Momentaufnahmen zugreifen und sie einbinden? <br/>
Auf alle von Azure Backup erstellten Momentaufnahmen kann im Portal, über PowerShell oder mithilfe der CLI über die Funktion zum Anzeigen von Momentaufnahmen zugegriffen werden. Sie können sie mit dem hier erläuterten Verfahren einbinden.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Was ist die maximale Aufbewahrungsdauer, die ich für Sicherungen konfigurieren kann? <br/>
Mit Sicherungen für Azure-Dateifreigaben können Sie Ihre täglichen Sicherungen maximal 120 Tage aufbewahren.

### <a name="what-happens-when-i-change-the-backup-policy-for-a-file-share-br"></a>Was passiert, wenn ich die Sicherungsrichtlinie für eine Dateifreigabe ändere? <br/>
Wenn eine neue Richtlinie auf Dateifreigaben angewendet wird, werden der Zeitplan und die Aufbewahrung der neuen Richtlinie beachtet. Bei einer Ausweitung der Aufbewahrung werden bereits vorhandene Wiederherstellungspunkte markiert, um sie gemäß der neuen Richtlinie aufzubewahren. Bei einer Verkürzung der Aufbewahrung werden sie zum Löschen im Rahmen der nächsten Bereinigung markiert und demgemäß gelöscht.


## <a name="see-also"></a>Weitere Informationen
Diese Informationen beziehen sich nur auf das Sichern von Azure Files. Wenn Sie weitere Informationen zu anderen Bereichen von Azure Backup benötigen, sehen Sie sich die folgenden Artikel mit häufig gestellten Fragen zu Backup an:
-  [Häufig gestellte Fragen zum Recovery Services-Tresor](backup-azure-backup-faq.md)
-  [Häufig gestellte Fragen zur Azure-VM-Sicherung](backup-azure-vm-backup-faq.md)
-  [Häufig gestellte Fragen zur Sicherung von Dateien/Ordnern mit dem Azure Backup-Agent](backup-azure-file-folder-backup-faq.md)

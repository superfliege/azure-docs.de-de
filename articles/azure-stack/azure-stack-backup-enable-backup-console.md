---
title: Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal | Microsoft-Dokumentation
description: Aktivieren Sie den Dienst für die Infrastruktursicherung über das Verwaltungsportal, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: d7d47b61c926c6704a06dacc55f00d77a1266988
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038364"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal
Aktivieren Sie den Dienst für die Infrastruktursicherung über das Verwaltungsportal, sodass Azure Stack Sicherungen generieren kann. Mit diesen Sicherungen können Sie bei einem [schwerwiegenden Fehler](.\azure-stack-backup-recover-data.md) Ihre Umgebung mittels einer Cloudwiederherstellung wiederherstellen. Die Cloudwiederherstellung soll sicherstellen, dass die Anbieter und Benutzer sich nach Abschluss der Wiederherstellung wieder im Portal anmelden können. Auf diese Weise können Benutzer ihre Abonnements wiederherstellen, einschließlich RBAC-Berechtigungen (Role-Based Access Control) und -Rollen, ursprünglichen Plänen, Angeboten und zuvor definierten Compute-, Speicher- und Netzwerkkontingenten.

Allerdings sichert der Infrastructure Backup-Dienst keine IaaS-VMs, Netzwerkkonfigurationen oder Speicherressourcen (z.B. Speicherkonten, Blobs, Tabellen). Daher werden Benutzern zuvor vorhandene Ressourcen nicht angezeigt, wenn sie sich nach einer Cloudwiederherstellung anmelden. PaaS-Ressourcen (Platform as a Service) und -Daten werden ebenfalls nicht vom Dienst gesichert. 

Administratoren und Benutzer sind für die Sicherung und Wiederherstellung von IaaS- und PaaS-Ressourcen verantwortlich, die separat von den Infrastruktursicherungsprozessen erfolgen muss. Informationen zum Sichern von IaaS- und PaaS-Ressourcen finden Sie unter folgenden Links:

- [Virtuelle Computer](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>Sicherung aktivieren oder neu konfigurieren

1. Öffnen Sie das [Azure Stack-Verwaltungsportal](azure-stack-manage-portals.md).
2. Wählen Sie **Alle Dienste** aus, und wählen Sie dann unter der Kategorie **VERWALTUNG** die Option **Infrastruktursicherung** aus. Wählen Sie auf dem Blatt **Infrastructure Backup** die Option **Konfiguration** aus.
3. Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Verwenden Sie für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe eine UNC-Zeichenfolge (Universal Naming Convention). Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Für den Dienst können Sie eine IP-Adresse verwenden. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten nach einem Notfall sollte sich das Gerät an einem gesonderten Speicherort befinden.

    > [!Note]  
    > Anstelle der IP-Adresse können Sie einen vollqualifizierten Domänennamen (FQDN) verwenden, wenn in Ihrer Umgebung die Namensauflösung vom Azure Stack-Infrastrukturnetzwerk in die Unternehmensumgebung unterstützt wird.
    
4. Geben Sie den **Benutzernamen** ein, und verwenden Sie dabei die Domäne und den Benutzernamen mit ausreichendem Zugriff zum Lesen und Schreiben von Dateien. Beispiel: `Contoso\backupshareuser`.
5. Geben Sie das **Kennwort** für den Benutzer ein.
6. Geben Sie das Kennwort erneut ein, um das **Kennwort zu bestätigen**.
7. Die **Häufigkeit in Stunden** bestimmt, wie oft Sicherungen erstellt werden. Der Standardwert ist 12. Scheduler unterstützt maximal 12 und mindestens 4. 
8. Die **Vermerkdauer in Tagen** bestimmt, wie viele Tage Sicherungen am externen Speicherort beibehalten werden. Der Standardwert ist 7. Scheduler unterstützt maximal 14 und mindestens 2. Nach Ablauf ihrer Vermerkdauer werden Sicherungen automatisch aus dem externen Speicherort gelöscht.

    > [!Note]  
    > Wenn Sie Sicherungen nach Ablauf ihrer Vermerkdauer archivieren möchten, achten Sie darauf, die Dateien zu sichern, bevor der Scheduler die Sicherungen löscht. Wenn Sie die Vermerkdauer reduzieren (z.B. von 7 auf 5 Tage) löscht der Scheduler alle Sicherungen, die älter als neue Vermerkdauer sind. Stellen Sie sicher, dass das Löschen von Sicherungen keine Probleme verursacht, bevor Sie diesen Wert aktualisieren. 

9. Geben Sie im Feld **Verschlüsselungsschlüssel** einen vorinstallierten Schlüssel an. Die Sicherungsdateien werden mit diesem Schlüssel verschlüsselt. Stellen Sie sicher, dass Sie diesen Schlüssel an einem sicheren Speicherort speichern. Nachdem Sie diesen Schlüssel zum ersten Mal festgelegt haben oder künftig ändern, kann er über diese Schnittstelle nicht mehr angezeigt werden. Zum Erstellen des Schlüssels führen Sie die folgenden Azure Stack PowerShell-Befehle aus:
    ```powershell
    New-AzsEncryptionKeyBase64
    ```
10. Wählen Sie **OK** aus, um die Backup Controller-Einstellungen zu speichern.

    ![Azure Stack – Backup Controller-Einstellungen](media\azure-stack-backup\backup-controller-settings.png)

## <a name="start-backup"></a>Sicherung starten
Zum Starten einer Sicherung klicken Sie auf **Jetzt sichern**, um eine bedarfsgesteuerte Sicherung zu starten. Eine bedarfsgesteuerte Sicherung ändert die Zeit für die nächste geplante Sicherung nicht. Nachdem die Aufgabe abgeschlossen ist, können Sie die Einstellungen in der **Zusammenfassung** bestätigen:

![Azure Stack: Bedarfsgesteuerte Sicherung](media\azure-stack-backup\scheduled-backup.png)

Sie können auch das PowerShell-Cmdlet **Start-AzsBackup** auf Ihrem Azure Stack-Verwaltungscomputer ausführen. Weitere Informationen finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Aktivieren oder Deaktivieren von automatischen Sicherungen
Wenn Sie die Sicherung aktivieren, werden Sicherungen automatisch geplant. Den Zeitpunkt der nächsten geplanten Sicherung finden Sie in der **Zusammenfassung**. 

![Azure Stack: Bedarfsgesteuerte Sicherung](media\azure-stack-backup\on-demand-backup.png)

Wenn Sie künftige geplante Sicherungen deaktivieren möchten, klicken Sie auf **Automatische Sicherungen deaktivieren**. Beim Deaktivieren automatischer Sicherungen bleiben die Sicherungseinstellungen einschließlich des Sicherungszeitplans konfiguriert. Mit diese Aktion wird der Scheduler lediglich angewiesen, künftige Sicherungen zu überspringen. 

![Azure Stack: Geplante Sicherungen deaktivieren](media\azure-stack-backup\disable-auto-backup.png)

Stellen Sie in der **Zusammenfassung** sicher, dass künftige geplante Sicherungen deaktiviert wurden:

![Azure Stack: Sicherstellen, dass Sicherungen deaktiviert wurden](media\azure-stack-backup\confirm-disable.png)

Klicken Sie auf **Automatische Sicherungen aktivieren**, um den Scheduler anzuweisen, zur geplanten Zeit mit künftigen Sicherungen zu beginnen. 

![Azure Stack: Geplante Sicherungen aktivieren](media\azure-stack-backup\enable-auto-backup.png)


> [!Note]  
> Wenn Sie die Infrastruktursicherung vor dem Update auf 1807 konfiguriert haben, werden automatische Sicherungen deaktiviert. So stehen die von Azure Stack gestarteten Sicherungen nicht mit Sicherungen in Konflikt, die von einem externen Aufgabenplanungsmodul gestartet wurden. Nachdem Sie alle externen Aufgabenplanungen deaktiviert haben, klicken Sie auf **Automatische Sicherungen aktivieren**.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine Sicherung ausführen. Lesen Sie dazu die Informationen unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Erfahren Sie, wie Sie sicherstellen, dass die Sicherung ausgeführt wurde. Lesen Sie dazu die Informationen unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md).

---
title: Behandeln von Problemen mit dem Azure Backup-Agent (MARS-Agent) | Microsoft-Dokumentation
description: Behandeln von Problemen mit der Installation und Registrierung des Azure Backup-Agents
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: da297177ca56822f7d50dbe06ec022640bf5d0cb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Behandeln von Problemen mit der Konfiguration oder Registrierung des Azure Backup-Agents
## <a name="recommended-steps"></a>Empfohlene Schritte
Lesen Sie die unten angegebenen empfohlenen Aktionen, um die entsprechenden Fehler, die bei der Konfiguration oder Registrierung des Azure Backup-Agents aufgetreten sind, zu beheben.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Fehler: Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst.
| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktion(en) |
| ---     | ---     | ---    |
| **Fehler** </br> *Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst. (ID: 34513)* | <ol><li> Die Tresoranmeldeinformationen sind ungültig (d.h., sie wurden mehr als 48 Stunden vor der Registrierung heruntergeladen).<li>   Der Azure Backup-Agent ist nicht in der Lage, eine temporäre Datei in den Temp-Ordner von Windows herunterzuladen. <li>Die Tresoranmeldeinformationen befinden sich an einem Netzwerkspeicherort. <li>TLS 1.0 ist deaktiviert.<li> Ein konfigurierter Proxyserver blockiert die Verbindung. <br> |  <ol><li>Laden Sie die neuen Tresoranmeldeinformationen herunter.<li>Klicken Sie unter „Internetoptionen“ > „Sicherheit“ > „Internet“ auf „Stufe anpassen...“, scrollen Sie zum Abschnitt „Dateidownload“, und aktivieren Sie „Aktivieren“.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/en-us/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass der Proxyserver verwendet wird, und geben Sie die Proxydetails an. <li> Passen Sie Datum und Uhrzeit (UTC) an die Ihres Computers an.<li>Navigieren Sie zu „C:/Windows/Temp“, prüfen Sie, ob es mehr als 60.000 oder 65.000 Dateien (mit der Erweiterung TMP) gibt, und löschen Sie diese Dateien.<li>Sie können dies testen, indem Sie das SDP-Paket auf dem Server ausführen. Wenn Sie eine Fehlermeldung erhalten, die besagt, dass Dateidownloads nicht zulässig sind, können Sie davon ausgehen, dass sich eine große Anzahl von Dateien im Verzeichnis „C:/Windows/Temp“ befindet.<li>Vergewissern Sie sich, dass .NET Framework 4.6.2 installiert ist. <li>Wenn Sie TLS 1.0 aufgrund von PCI-Compliance deaktiviert haben, lesen Sie die Informationen unter diesem [Link zur Problembehandlung](https://support.microsoft.com/help/4022913). <li>Wenn Sie ein Antivirenprogramm auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Virenprüfung aus. <ol><li>CBengine.exe<li>CSC.exe (Diese Datei ist mit dem .NET Framework verknüpft. Auf dem Server ist eine Datei namens „CSC.exe“ pro .NET-Version installiert. Schließen Sie sämtliche mit allen .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus.) <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Speicherort des Caches lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“.*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Fehler: Die Verbindung des Microsoft Azure Recovery Services-Agents mit Microsoft Azure Backup konnte nicht hergestellt werden.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktion(en) |
| ---     | ---     | ---    |
| **Fehler** </br><ol><li>*Die Verbindung des Microsoft Azure Recovery Services-Agents mit Microsoft Azure Backup konnte nicht hergestellt werden. (ID: 100050) Überprüfen Sie Ihre Netzwerkeinstellungen, und stellen Sie sicher, dass Sie eine Verbindung mit dem Internet herstellen können.*<li>*(407) Proxyauthentifizierung erforderlich* |Der Proxyserver blockiert die Verbindung.* |  <ol><li>Klicken Sie unter „Internetoptionen“ > „Sicherheit“ > „Internet“ auf „Stufe anpassen...“, scrollen Sie zum Abschnitt „Dateidownload“, und aktivieren Sie „Aktivieren“.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/en-us/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass der Proxyserver verwendet wird, und geben Sie die Proxydetails an. <li>Wenn Sie ein Antivirenprogramm auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Virenprüfung aus. <ol><li>CBengine.exe<li>(statt dpmra.exe)<li>CSC.exe (Diese Datei ist mit dem .NET Framework verknüpft. Auf dem Server ist eine Datei namens „CSC.exe“ pro .NET-Version installiert. Schließen Sie sämtliche mit allen .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus.) <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Speicherort des Caches lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“.*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Fehler: Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktion(en) |
| ---     | ---     | ---    |      
| **Fehler** </br>*Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen. Der aktuelle Vorgang ist aufgrund eines internen Dienstfehlers „Ungültige Eingabe“ fehlgeschlagen. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.* |Der Server ist bereits bei einem anderen Tresor registriert.| Heben Sie die Registrierung des Servers beim Tresor auf, und registrieren Sie ihn erneut.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Fehler: Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktion(en) |
| ---     | ---     | ---    |          
| **Fehler** </br><ol><li>*Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.* <li>*Fehler 34506. Die für diesen Computer gespeicherte Verschlüsselungspassphrase ist nicht ordnungsgemäß konfiguriert.* | <li> Der Ablageordner befindet sich auf einem Volume, das nicht genügend Speicherplatz aufweist. <li> Der Ablageordner wurde nicht ordnungsgemäß an einen anderen Speicherort verschoben. <li> Die Datei „OnlineBackup.KEK“ fehlt. | <li>Ändern Sie den Speicherort für den Ablageordner oder Cache in ein Volume, auf dem Speicherplatz von 5-10 % der Gesamtgröße der Sicherungsdaten zur Verfügung steht. Führen Sie die in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) erläuterten Schritte durch, um den Speicherort des Caches ordnungsgemäß zu ändern.<li> Stellen Sie sicher, dass die Datei „OnlineBackup.KEK“ vorhanden ist. <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Speicherort des Caches lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“.*

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die Einzelheiten zum [Sichern von Windows Server mit dem Azure Backup-Agent](tutorial-backup-windows-server-to-azure.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

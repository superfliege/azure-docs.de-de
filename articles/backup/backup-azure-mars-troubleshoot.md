---
title: Behandeln von Problemen mit dem Azure Backup-Agent | Microsoft-Dokumentation
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
ms.openlocfilehash: 52a32d61dd69110ace560fd1e52389140f322678
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="troubleshoot-azure-backup-agent-configuration-and-registration-issues"></a>Behandeln von Problemen mit der Konfiguration oder Registrierung des Azure Backup-Agents
## <a name="recommended-steps"></a>Empfohlene Schritte
In den folgenden Tabellen werden die empfohlenen Aktionen aufgeführt, um mögliche Fehler bei der Konfiguration oder Registrierung des Azure Backup-Agents zu beheben.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst.
| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |
| **Fehler** </br> *Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst. (ID: 34513)* | <ul><li> Die Tresoranmeldeinformationen sind ungültig (d.h. sie wurden mehr als 48 Stunden vor der Registrierung heruntergeladen).<li>Der Azure Backup-Agent ist nicht in der Lage, eine temporäre Datei in den temporären Windows -Ordner herunterzuladen. <li>Die Tresoranmeldeinformationen befinden sich an einem Netzwerkspeicherort. <li>TLS 1.0 ist deaktiviert.<li> Ein konfigurierter Proxyserver blockiert die Verbindung. <br> |  <ul><li>Laden Sie die neuen Tresoranmeldeinformationen herunter.<li>Navigieren Sie zu **Internetoptionen** > **Sicherheit** > **Internet**. Wählen Sie als Nächstes **Stufe anpassen** aus, und scrollen Sie zum Abschnitt zum Dateidownload. Wählen Sie anschließend **Aktivieren** aus.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass ein Proxyserver verwendet wird. Geben Sie dann die Proxyserverdetails an. <li> Passen Sie Datum und Uhrzeit an die Ihres Computers an.<li>Navigieren Sie zu „C:/Windows/Temp“, und prüfen Sie, ob es mehr als 60.000 oder 65.000 Dateien mit der Erweiterung TMP gibt. Wenn dies der Fall ist, löschen Sie diese Dateien.<li>Sie können dies testen, indem Sie das SDP-Paket auf dem Server ausführen. Wenn Sie eine Fehlermeldung erhalten, die besagt, dass Dateidownloads unzulässig sind, befindet sich aller Wahrscheinlichkeit nach eine große Anzahl von Dateien im Verzeichnis „C:/Windows/Temp“.<li>Vergewissern Sie sich, dass .NET Framework 4.6.2 installiert ist. <li>Wenn Sie TLS 1.0 aufgrund von PCI-Compliance deaktiviert haben, lesen Sie die Informationen auf dieser [Seite zur Problembehandlung](https://support.microsoft.com/help/4022913). <li>Wenn Sie eine Antivirensoftware auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Antivirenprüfung aus: <ul><li>CBengine.exe<li>CSC.exe (Diese Datei ist mit dem .NET Framework verknüpft. Auf dem Server ist eine Datei namens „CSC.exe“ für jede .NET-Version installiert. Schließen Sie sämtliche mit .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus.) <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Es konnte keine Verbindung zwischen dem Microsoft Azure Recovery Services-Agent und Microsoft Azure Backup hergestellt werden.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |
| **Fehler** </br><ol><li>*Die Verbindung des Microsoft Azure Recovery Services-Agents mit Microsoft Azure Backup konnte nicht hergestellt werden. (ID: 100050) Überprüfen Sie Ihre Netzwerkeinstellungen, und stellen Sie sicher, dass Sie eine Verbindung mit dem Internet herstellen können.*<li>*(407) Proxyauthentifizierung erforderlich* |Der Proxyserver blockiert die Verbindung. |  <ul><li>Navigieren Sie zu **Internetoptionen** > **Sicherheit** > **Internet**. Wählen Sie als Nächstes **Stufe anpassen** aus, und scrollen Sie zum Abschnitt zum Dateidownload. Wählen Sie **Aktivieren** aus.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass ein Proxyserver verwendet wird. Geben Sie dann die Proxyserverdetails an. <li>Wenn Sie eine Antivirensoftware auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Antivirenprüfung aus. <ul><li>CBEngine.exe (statt dpmra.exe)<li>CSC.exe: (Diese Datei ist mit dem .NET Framework verknüpft.) Auf dem Server ist eine Datei namens „CSC.exe“ für jede .NET-Version installiert. Schließen Sie sämtliche mit .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus. <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |      
| **Fehler** </br>*Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen. Der aktuelle Vorgang ist aufgrund eines internen Dienstfehlers „Ungültige Eingabe“ fehlgeschlagen. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support*. |Der Server ist bereits bei einem anderen Tresor registriert.| Heben Sie die Registrierung des Servers beim Tresor auf, und registrieren Sie ihn erneut.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |          
| **Fehler** </br><ol><li>*Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.* <li>*Fehler 34506. Die für diesen Computer gespeicherte Verschlüsselungspassphrase ist nicht ordnungsgemäß konfiguriert*. | <li> Der Ablageordner befindet sich auf einem Volume, das nicht genügend Speicherplatz aufweist. <li> Der Ablageordner wurde nicht ordnungsgemäß an einen anderen Speicherort verschoben. <li> Die Datei „OnlineBackup.KEK“ fehlt. | <li>Ändern Sie den Speicherort für den Ablageordner oder Cache in ein Volume, auf dem Speicherplatz von 5-10 % der Gesamtgröße der Sicherungsdaten zur Verfügung steht. Um den Cachespeicherort ordnungsgemäß zu verschieben, führen Sie die Schritte unter [Fragen zum Azure Backup-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) durch.<li> Stellen Sie sicher, dass die Datei „OnlineBackup.KEK“ vorhanden ist. <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die Einzelheiten zum [Sichern von Windows Server mit dem Azure Backup-Agent](tutorial-backup-windows-server-to-azure.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

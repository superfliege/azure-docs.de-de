---
title: Behandeln von Problemen mit dem Azure Backup-Agent
description: Behandeln von Problemen mit der Installation und Registrierung des Azure Backup-Agents
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: e7a63167285c06fdfe632e7d45d9fddd3cca7842
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248521"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Behandeln von Problemen mit dem Microsoft Azure Recovery Services-Agent (MARS)
## <a name="recommended-steps"></a>Empfohlene Schritte
In diesem Artikel erfahren Sie, wie Sie Fehler bei der Konfiguration, Registrierung, Sicherung und Wiederherstellung mit dem MARS-Agent beheben.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst.
| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |
| **Fehler** </br> *Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst. (ID: 34513)* | <ul><li> Die Tresoranmeldeinformationen sind ungültig (d.h. sie wurden mehr als 48 Stunden vor der Registrierung heruntergeladen).<li>Der MARS-Agent kann keine Dateien in das temporäre Windows-Verzeichnis herunterladen. <li>Die Tresoranmeldeinformationen befinden sich an einem Netzwerkspeicherort. <li>TLS 1.0 ist deaktiviert.<li> Ein konfigurierter Proxyserver blockiert die Verbindung. <br> |  <ul><li>Laden Sie die neuen Tresoranmeldeinformationen herunter.<li>Navigieren Sie zu **Internetoptionen** > **Sicherheit** > **Internet**. Wählen Sie als Nächstes **Stufe anpassen** aus, und scrollen Sie zum Abschnitt zum Dateidownload. Wählen Sie anschließend **Aktivieren** aus.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass ein Proxyserver verwendet wird. Geben Sie dann die Proxyserverdetails an. <li> Passen Sie Datum und Uhrzeit an die Ihres Computers an.<li>Navigieren Sie zu „C:/Windows/Temp“, und prüfen Sie, ob es mehr als 60.000 oder 65.000 Dateien mit der Erweiterung TMP gibt. Wenn dies der Fall ist, löschen Sie diese Dateien.<li>Sie können dies überprüfen, indem Sie das SDP-Paket auf dem Server ausführen. Wenn Sie eine Fehlermeldung erhalten, die darauf hinweist, dass Dateidownloads unzulässig sind, befindet sich wahrscheinlich viele Dateien im Verzeichnis „C:/Windows/Temp“.<li>Vergewissern Sie sich, dass .NET Framework 4.6.2 installiert ist. <li>Wenn Sie TLS 1.0 aufgrund von PCI-Compliance deaktiviert haben, lesen Sie die Informationen auf dieser [Seite zur Problembehandlung](https://support.microsoft.com/help/4022913). <li>Wenn Sie eine Antivirensoftware auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Antivirenprüfung aus: <ul><li>CBengine.exe<li>CSC.exe (Diese Datei ist mit dem .NET Framework verknüpft. Auf dem Server ist eine Datei namens „CSC.exe“ für jede .NET-Version installiert. Schließen Sie sämtliche mit .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus. <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Es konnte keine Verbindung zwischen dem Microsoft Azure Recovery Services-Agent und Microsoft Azure Backup hergestellt werden.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |
| **Fehler** </br><ol><li>*Die Verbindung des Microsoft Azure Recovery Services-Agents mit Microsoft Azure Backup konnte nicht hergestellt werden. (ID: 100050) Überprüfen Sie Ihre Netzwerkeinstellungen, und stellen Sie sicher, dass Sie eine Verbindung mit dem Internet herstellen können.*<li>*(407) Proxyauthentifizierung erforderlich* |Der Proxyserver blockiert die Verbindung. |  <ul><li>Navigieren Sie zu **Internetoptionen** > **Sicherheit** > **Internet**. Wählen Sie als Nächstes **Stufe anpassen** aus, und scrollen Sie zum Abschnitt zum Dateidownload. Wählen Sie **Aktivieren** aus.<li>Möglicherweise müssen Sie die Website auch unter [Vertrauenswürdige Sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements) hinzufügen.<li>Ändern Sie die Einstellungen dahingehend, dass ein Proxyserver verwendet wird. Geben Sie dann die Proxyserverdetails an. <li>Wenn Sie eine Antivirensoftware auf dem Server installiert haben, schließen Sie die folgenden Dateien von der Antivirenprüfung aus. <ul><li>CBEngine.exe (statt dpmra.exe)<li>CSC.exe: (Diese Datei ist mit dem .NET Framework verknüpft.) Auf dem Server ist eine Datei namens „CSC.exe“ für jede .NET-Version installiert. Schließen Sie sämtliche mit .NET Framework-Versionen verknüpfte Dateien mit dem Namen „CSC.exe“ auf dem betroffenen Server aus. <li>Speicherort des Ablageordners oder Caches <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
| ---     | ---     | ---    |      
| **Fehler** </br>*Fehler beim Festlegen des Verschlüsselungsschlüssels für sichere Sicherungen. Die Aktivierung wurde nicht erfolgreich abgeschlossen, doch die Verschlüsselungspassphrase wurde in der folgenden Datei gespeichert.* |<li>Der Server ist bereits bei einem anderen Tresor registriert.<li>Beim Konfigurieren wurde die Passphrase beschädigt.| Heben Sie die Registrierung des Servers beim Tresor auf, und registrieren Sie ihn erneut mit einer neuen Passphrase.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
|---------|---------|---------|
|**Fehler** </br><ol>*Die Aktivierung wurde nicht erfolgreich abgeschlossen. Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x1FC07] ein Fehler aufgetreten. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.*     | <li> Der Ablageordner befindet sich auf einem Volume, das nicht genügend Speicherplatz aufweist. <li> Der Ablageordner wurde nicht ordnungsgemäß an einen anderen Speicherort verschoben. <li> Die Datei „OnlineBackup.KEK“ fehlt.         | <li>Führen Sie ein Upgrade auf die [neueste Version](http://aka.ms/azurebackup_agent) des MARS-Agents durch.<li>Verschieben Sie den Ablageordner oder den Cachespeicherort in ein Volume, auf dem Speicherplatz in Höhe von 5–10 % der Gesamtgröße der Sicherungsdaten verfügbar ist. Um den Cachespeicherort ordnungsgemäß zu verschieben, führen Sie die Schritte unter [Fragen zum Azure Backup-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) durch.<li> Stellen Sie sicher, dass die Datei „OnlineBackup.KEK“ vorhanden ist. <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Fehler 34506. Die auf diesem Computer gespeicherte Verschlüsselungspassphrase ist nicht ordnungsgemäß konfiguriert.

| Fehlerdetails | Mögliche Ursachen | Empfohlene Aktionen |
|---------|---------|---------|
|**Fehler** </br><ol>*Fehler 34506. Die für diesen Computer gespeicherte Verschlüsselungspassphrase ist nicht ordnungsgemäß konfiguriert*.    | <li> Der Ablageordner befindet sich auf einem Volume, das nicht genügend Speicherplatz aufweist. <li> Der Ablageordner wurde nicht ordnungsgemäß an einen anderen Speicherort verschoben. <li> Die Datei „OnlineBackup.KEK“ fehlt.        | <li>Führen Sie ein Upgrade auf die [neueste Version](http://aka.ms/azurebackup_agent) des MARS-Agents durch.<li>Ändern Sie den Speicherort für den Ablageordner oder Cache in ein Volume, auf dem Speicherplatz von 5-10 % der Gesamtgröße der Sicherungsdaten zur Verfügung steht. Um den Cachespeicherort ordnungsgemäß zu verschieben, führen Sie die Schritte unter [Fragen zum Azure Backup-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) durch.<li> Stellen Sie sicher, dass die Datei „OnlineBackup.KEK“ vorhanden ist. <br>*Der Standardspeicherort für den Ablageordner oder der Pfad für den Cachespeicherort lautet „C:\Programme\Microsoft Azure Recovery Services Agent\Scratch“*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>Sicherungen werden nicht nach Zeitplan ausgeführt
Führen Sie die folgenden Schritte aus, wenn geplante Sicherungen nicht automatisch ausgelöst werden, während manuelle Sicherungen problemlos funktionieren. 
 
<li>Überprüfen Sie, ob PowerShell 3.0 oder höher auf dem Server installiert ist. Um die PowerShell-Version zu überprüfen, führen Sie den folgenden Befehl aus, und überprüfen Sie, ob der *Hauptteil* der Versionsnummer mindestens 3 ist.

`$PSVersionTable.PSVersion`
<li>Überprüfen Sie, ob der folgende Pfad Teil der *PSMODULEPATH*-Umgebungsvariablen ist.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Wenn die PowerShell-Ausführungsrichtlinie für die *LocalMachine* auf „Eingeschränkt“ festgelegt ist, schlägt möglicherweise das PowerShell-Cmdlet fehl, das die Sicherungsaufgabe auslöst. Führen Sie die folgenden Befehle im Modus mit erhöhten Rechten aus, um die Ausführungsrichtlinie zu überprüfen und auf *Uneingeschränkt* oder *RemoteSigned* festzulegen.

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Gehen Sie zu Systemsteuerung -> Verwaltung -> Aufgabenplanung -> erweitern Sie „Microsoft“ -> wählen Sie „Onlinesicherung“ aus
<li>Doppelklicken Sie auf die Aufgabe „Microsoft-OnlineBackup“, und wechseln Sie zur Registerkarte „Trigger“.
<li>Überprüfen Sie, ob der „Status“ der Aufgabe „Aktiviert“ lautet. Wenn das nicht der Fall ist, klicken Sie auf „Bearbeiten“, und aktivieren Sie das Kontrollkästchen „Aktiviert“.
<li>Navigieren Sie auf der Registerkarte *Allgemein* zum Abschnitt *Sicherheitsoptionen*.
<li>Überprüfen Sie, ob das Benutzerkonto, das zum Ausführen der Aufgabe ausgewählt wurde, *SYSTEM* oder die lokale Administratorgruppe auf dem Server ist. > [!TIP] Es wird empfohlen, den Server neu zu starten, nachdem die obigen Schritten ausgeführt wurden, um sicherzustellen, dass vorgenommene Änderungen konsistent angewendet werden.


## <a name="troubleshooting-restore-issues"></a>Behandeln von Wiederherstellungsproblemen

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Fehler beim Einbinden des Wiederherstellungsvolumes beim Wiederherstellen von Dateien und Ordnern
Falls Azure Backup das Wiederherstellungsvolume auch mehrere Minuten nach dem Klicken auf **Einbinden** nicht erfolgreich eingebunden hat oder beim Einbinden des Wiederherstellungsvolumes Fehler auftreten, führen Sie die folgenden Schritte für eine normale Wiederherstellung aus.

1.  Brechen Sie den laufenden Einbindungsvorgang ab, falls dieser bereits mehrere Minuten lang ausgeführt wird.

2.  Vergewissern Sie sich, dass Sie über die neueste Version des Azure Backup-Agents verfügen. Klicken Sie zum Ermitteln der Versionsinformationen für den Azure Backup-Agent im Bereich **Aktionen** der Microsoft Azure Backup-Konsole auf **About Microsoft Azure Recovery Services Agent** (Informationen zum Microsoft Azure Recovery Services-Agent), und vergewissern Sie sich, dass die **Versionsummer** mindestens der in [diesem Artikel](https://go.microsoft.com/fwlink/?linkid=229525) angegebenen Version entspricht. Sie können die neueste Version [hier](https://go.microsoft.com/fwLink/?LinkID=288905) herunterladen.

3.  Wechseln Sie zu **Geräte-Manager** -> **Speichercontroller**, und vergewissern Sie sich, dass **Microsoft iSCSI-Initiator** vorhanden ist. Ist dies der Fall, fahren Sie direkt mit Schritt 7 fort. 

4.  Ist der in Schritt 3 erwähnte Microsoft iSCSI-Initiator-Dienst nicht vorhanden, suchen Sie unter **Geräte-Manager** -> **Speichercontroller** nach einem Eintrag vom Typ **Unbekanntes Gerät** mit der Hardware-ID **ROOT\ISCSIPRT**.

5.  Klicken Sie mit der rechten Maustaste auf **Unbekanntes Gerät**, und wählen Sie **Treibersoftware aktualisieren** aus.

6.  Aktualisieren Sie den Treiber durch Auswählen der Option **Automatisch nach aktueller Treibersoftware suchen**. Nach Abschluss des Updates sollte **Unbekanntes Gerät** durch **Microsoft iSCSI-Initiator** ersetzt werden, wie im Anschluss dargestellt. 

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Wechseln Sie zu **Task-Manager** -> **Dienste (lokal)** -> **Microsoft iSCSI-Initiator-Dienst**. 

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Starten Sie den Microsoft iSCSI-Initiator neu, indem Sie mit der rechten Maustaste auf den Dienst und anschließend auf „Beenden“ klicken. Klicken Sie dann mit der rechten Maustaste und anschließend auf **Starten**.

9.  Wiederholen Sie die Wiederherstellung mit der sofortigen Wiederherstellung. 

Ist die Wiederherstellung weiterhin nicht erfolgreich, starten Sie Ihren Server/Client neu. Wenn Sie keinen Neustart möchten oder die Wiederherstellung auch nach einem Serverneustart fehlschlägt, führen Sie die Wiederherstellung über einen anderen Computer aus. Befolgen Sie dafür die Anleitung in [diesem Artikel](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die Einzelheiten zum [Sichern von Windows Server mit dem Azure Backup-Agent](tutorial-backup-windows-server-to-azure.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

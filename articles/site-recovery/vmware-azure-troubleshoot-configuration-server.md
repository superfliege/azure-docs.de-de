---
title: Beheben von Problemen mit dem Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Problembehandlung für die Bereitstellung des Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 597b8f59ef6991f7868d3de481e98ed9a459077b
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050794"
---
# <a name="troubleshoot-configuration-server-issues"></a>Behandeln von Problemen mit dem Konfigurationsserver

Dieser Artikel hilft Ihnen bei der Behandlung von Problemen bei der Bereitstellung und Verwaltung des [Azure Site Recovery](site-recovery-overview.md)-Konfigurationsservers. Der Konfigurationsserver fungiert als Verwaltungsserver. Verwenden Sie den Konfigurationsserver zum Einrichten der Notfallwiederherstellung von lokalen virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Site Recovery. In den folgenden Abschnitten werden die häufigsten Fehler erörtert, die beim Hinzufügen eines neuen Konfigurationsserver und beim Verwalten von Konfigurationsservern auftreten können.

## <a name="registration-failures"></a>Fehler bei der Registrierung

Der Quellcomputer registriert sich während der Installation des Mobilitäts-Agents beim Konfigurationsserver. Sie können alle während dieses Setups auftretenden Fehler durch Befolgen der folgenden Anleitungen debuggen:

1. Öffnen Sie die Datei „C:\Programme\ASR\home\svsystems\var\configurator_register_host_static_info.log“. (Der Ordner „ProgramData“ ist normalerweise ein versteckter Ordner. Wenn der Ordner „ProgramData“ im Datei-Explorer nicht angezeigt wird, aktivieren Sie auf der Registerkarte **Ansicht** im Bereich **Ein-/ausblenden** das Kontrollkästchen **Ausgeblendete Elemente**.) Fehler können durch mehrere Probleme verursacht werden.

2. Suchen Sie nach der Zeichenfolge **No Valid IP Address found** (Keine gültige IP-Adresse gefunden). Wenn die Zeichenfolge gefunden wird:
    1. Überprüfen Sie, ob die angeforderte Host-ID mit der Host-ID des Quellcomputers übereinstimmt.
    2. Überprüfen Sie, ob der Quellcomputer über mindestens eine der physischen Netzwerkkarte zugewiesene IP-Adresse verfügt. Damit der Agent erfolgreich beim Konfigurationsserver registriert werden kann, muss der Quellcomputer über mindestens eine der physischen Netzwerkkarte zugewiesene IP v4-Adresse verfügen.
    3. Führen Sie einen der folgenden Befehle auf dem Quellcomputer aus, um alle IP-Adressen des Quellcomputers abzurufen:
      - Für Windows: `> ipconfig /all`
      - Für Linux: `# ifconfig -a`

3. Wenn die Zeichenfolge **No Valid IP Address found** (Keine gültige IP-Adresse gefunden) nicht gefunden wird, suchen Sie nach der Zeichenfolge **Reason=>NULL**. Dieser Fehler tritt auf, wenn der Quellcomputer einen leeren Host für die Registrierung beim Konfigurationsserver verwendet. Wenn die Zeichenfolge gefunden wird:
    - Nachdem Sie die Probleme behoben haben, befolgen Sie die Anleitungen unter [Registrieren des Quellcomputers beim Konfigurationsserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server), um die Registrierung manuell zu wiederholen.

4. Wenn die Zeichenfolge **Reason=>NULL** nicht gefunden wird, öffnen Sie auf dem Quellcomputer die Datei „C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log“. (Der Ordner „ProgramData“ ist normalerweise ein versteckter Ordner. Wenn der Ordner „ProgramData“ im Datei-Explorer nicht angezeigt wird, aktivieren Sie auf der Registerkarte **Ansicht** im Bereich **Ein-/ausblenden** das Kontrollkästchen **Ausgeblendete Elemente**.) Fehler können durch mehrere Probleme verursacht werden. 

5. Suchen Sie nach der Zeichenfolge **post request: (7) – Couldn't connect to server** (Veröffentlichungsanforderung: (7) – Serververbindung konnte nicht hergestellt werden). Wenn die Zeichenfolge gefunden wird:
    1. Beheben Sie die Netzwerkprobleme zwischen dem Quellcomputer und dem Konfigurationsserver. Überprüfen Sie, ob der Konfigurationsserver vom Quellcomputer aus erreichbar ist, mithilfe von Netzwerktools wie Ping, Traceroute oder einem Webbrowser. Stellen Sie sicher, dass der Quellcomputer den Konfigurationsserver über Port 443 erreichen kann.
    2. Überprüfen Sie, ob eine der Firewallregeln auf dem Quellcomputer die Verbindung zwischen Quellcomputer und Konfigurationsserver blockiert. Arbeiten Sie gemeinsam mit Ihrem Netzwerkadministrator daran, jegliche Verbindungsprobleme zu beheben.
    3. Stellen Sie sicher, dass die unter [Site Recovery-Ordnerausschlüsse von Antivirenprogrammen](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) aufgeführten Ordner in der Antivirensoftware ausgeschlossen sind.
    4. Nachdem die Probleme behoben wurden, versuchen Sie die Registrierung erneut, indem Sie die Anleitungen unter [Registrieren des Quellcomputers beim Konfigurationsserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) befolgen.

6. Wenn die Zeichenfolge **post request: (7) – Couldn't connect to server** (Veröffentlichungsanforderung: (7) – Serververbindung konnte nicht hergestellt werden) nicht gefunden wird, suchen Sie in derselben Protokolldatei nach der Zeichenfolge **request: (60) – Peer certificate cannot be authenticated with given CA certificates** (Anforderung: (60) – Das Peerzertifikat kann mit den vorhandenen Zertifizierungsstellenzertifikaten nicht authentifiziert werden). Dieser Fehler kann auftreten, weil das Zertifikat des Konfigurationsservers abgelaufen ist oder der Quellcomputer weder TLS 1.0 noch höhere SSL-Protokolle unterstützt. Er kann auch auftreten, wenn eine Firewall die SSL-Kommunikation zwischen dem Quellcomputer und dem Konfigurationsserver blockiert. Wenn die Zeichenfolge gefunden wird: 
    1. Stellen Sie zur Behebung eine Verbindung mit der IP-Adresse des Konfigurationsservers mithilfe eines Webbrowsers auf dem Quellcomputer her. Verwenden Sie den URI „https:\/\/<IP-Adresse des Konfigurationsservers\>:443/“. Stellen Sie sicher, dass der Quellcomputer den Konfigurationsserver über Port 443 erreichen kann.
    2. Überprüfen Sie, ob eine der Firewallregeln auf dem Quellcomputer hinzugefügt oder entfernt werden muss, damit der Quellcomputer mit dem Konfigurationsserver kommunizieren kann. Aufgrund der Vielfalt von Firewallsoftware, die im Einsatz sein kann, können wir nicht alle erforderlichen Firewallkonfigurationen auflisten. Arbeiten Sie gemeinsam mit Ihrem Netzwerkadministrator daran, jegliche Verbindungsprobleme zu beheben.
    3. Stellen Sie sicher, dass die unter [Site Recovery-Ordnerausschlüsse von Antivirenprogrammen](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) aufgeführten Ordner in der Antivirensoftware ausgeschlossen sind.  
    4. Nachdem Sie die Probleme behoben haben, versuchen Sie die Registrierung erneut, indem Sie die Anleitungen unter [Registrieren des Quellcomputers beim Konfigurationsserver](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) befolgen.

7. Wenn unter Linux der Wert für die Plattform in „<INSTALLATIONSVERZ\>/etc/drscout.conf“ beschädigt ist, schlägt die Registrierung fehl. Um dieses Problem zu identifizieren, öffnen Sie die Datei „/var/log/ua_install.log“. Suchen Sie nach der Zeichenfolge **Aborting configuration as VM_PLATFORM value is either null or it is not VmWare/Azure** (Die Konfiguration wird abgebrochen, da der Wert von VM_PLATFORM entweder NULL oder nicht VmWare/Azure ist). Die Plattform sollte entweder auf **VmWare** oder **Azure** festgelegt sein. Wenn die Datei „drscout.conf“ beschädigt ist, empfehlen wird, dass Sie [den Mobilitäts-Agent deinstallieren](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) und diesen dann erneut installieren. Schlägt die Deinstallation fehl, führen Sie die folgenden Schritte aus:
    1. Öffnen Sie die Datei „Installation_Directory/uninstall.sh“, und kommentieren Sie den Aufruf der Funktion **StopServices** aus.
    2. Öffnen Sie die Datei „Installation_Directory/Vx/bin/uninstall.sh“, und kommentieren Sie den Aufruf der Funktion **stop_services** aus.
    3. Öffnen Sie die Datei „Installation_Directory/Fx/uninstall.sh“, und kommentieren Sie den gesamten Abschnitt aus, der versucht, den Fx-Dienst zu beenden.
    4. [Deinstallieren](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) Sie den Mobilitäts-Agent. Starten Sie nach der erfolgreichen Deinstallation das System neu, und versuchen Sie, den Mobilitäts-Agent erneut zu installieren.

## <a name="installation-failure-failed-to-load-accounts"></a>Installationsfehler: Fehler beim Laden von Konten.

Dieser Fehler tritt auf, wenn der Dienst beim Installieren des Mobilitäts-Agents und seiner Registrierung beim Konfigurationsserver keine Daten aus der Transportverbindung lesen kann. Stellen Sie zur Behebung des Problems sicher, dass TLS 1.0 auf Ihrem Quellcomputer aktiviert ist.

## <a name="change-the-ip-address-of-the-configuration-server"></a>Ändern der IP-Adresse des Konfigurationsservers

Wir empfehlen dringend, dass Sie die IP-Adresse eines Konfigurationsservers nicht ändern. Stellen Sie sicher, dass alle IP-Adressen, die dem Konfigurationsserver zugewiesen sind, statische IP-Adressen sind. Verwenden Sie keine DHCP-IP-Adressen.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: Das SAML-Token ist ungültig

Um diesen Fehler zu vermeiden, vergewissern Sie sich, dass die Uhrzeit Ihrer Systemuhr nicht um mehr als 15 Minuten von der Ortszeit abweicht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.

## <a name="failed-to-create-a-certificate"></a>Fehler beim Erstellen eines Zertifikats

Ein für die Authentifizierung von Site Recovery erforderliches Zertifikat kann nicht erstellt werden. Führen Sie Setup erneut aus, nachdem Sie sichergestellt haben, dass Sie Setup als lokaler Administrator ausführen.

## <a name="register-the-source-machine-with-the-configuration-server"></a>Registrieren des Quellcomputers beim Konfigurationsserver

### <a name="if-the-source-machine-runs-windows"></a>Wenn der Quellcomputer Windows ausführt

Führen Sie den folgenden Befehl auf dem Quellcomputer aus:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
  ```

Einstellung | Details
--- | ---
Verwendung | UnifiedAgentConfigurator.exe  /CSEndPoint <IP-Adresse des Konfigurationsservers\> /PassphraseFilePath <Dateipfad der Passphrase\>
Agent-Konfigurationsprotokolle | Befindet sich unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log“.
/CSEndPoint | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
/PassphraseFilePath |  Obligatorisch. Der Speicherort der Passphrase. Verwenden Sie einen beliebiger UNC- oder lokalen Dateipfad.

### <a name="if-the-source-machine-runs-linux"></a>Wenn der Quellcomputer Linux ausführt

Führen Sie den folgenden Befehl auf dem Quellcomputer aus:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Einstellung | Details
--- | ---
Verwendung | cd /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i <IP-Adresse des Konfigurationsservers\> -P <Dateipfad der Passphrase\>
-i | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
-P |  Obligatorisch. Der vollständige Dateipfad der Datei, in der die Passphrase gespeichert ist. Verwenden Sie einen beliebigen gültigen Ordner.


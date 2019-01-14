---
title: Beheben von Problemen mit dem Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Problembehandlung für die Bereitstellung des Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: f5c8241907459a06f0a6206ae6865cdf3fe9ab89
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998964"
---
# <a name="troubleshoot-configuration-server-issues"></a>Behandeln von Problemen mit dem Konfigurationsserver

Dieser Artikel hilft Ihnen bei der Behandlung von Problemen bei der Bereitstellung und Verwaltung des [Azure Site Recovery](site-recovery-overview.md)-Konfigurationsservers. Der Konfigurationsserver fungiert als Verwaltungsserver und wird zum Einrichten der Notfallwiederherstellung für lokale virtuelle VMware-Computer und physische Server in Azure mithilfe von Site Recovery verwendet. In den folgenden Abschnitten werden die häufigsten Fehler erörtert, die beim Hinzufügen eines neuen Konfigurationsserver und beim Verwalten von Konfigurationsservern auftreten.

## <a name="registration-failures"></a>Fehler bei der Registrierung

Der Quellcomputer registriert sich während der Installation des Mobilitäts-Agents beim Konfigurationsserver. Alle während dieses Setups auftretenden Fehler können durch Einhaltung der unten genannten Richtlinien debuggt werden:

1. Navigieren Sie zur Datei „C:\Programme\ASR\home\svsystems\var\configurator_register_host_static_info.log“. „Programme“ ist normalerweise ein verborgener Ordner. Wenn Sie ihn nicht finden können, versuchen Sie, seine Ausblendung aufzuheben. Die Fehler können mehrere Ursachen haben.
2. Suchen Sie nach der Zeichenfolge „No Valid IP Address found“ (Keine gültige IP-Adresse gefunden). Wenn die Zeichenfolge gefunden wird:
    - Überprüfen Sie, ob die ID des angeforderten Hosts die gleiche wie die des Quellcomputers ist.
    - Der Quellcomputer sollte über mindestens eine der physischen Netzwerkkarte zugewiesene IP-Adresse verfügen, damit die Registrierung des Agents beim Konfigurationsserver erfolgreich ist.
    - Führen Sie auf dem Quellcomputer den Befehl `> ipconfig /all` (für Windows-Betriebssystem) und `# ifconfig -a` (für Linux-Betriebssystem) aus, um alle IP-Adressen des Quellcomputers abzurufen.
    - Bitte beachten Sie, dass der physischen Netzwerkkarte für die Agent-Registrierung eine gültige IP v4-Adresse zugewiesen sein muss.
3. Wenn die oben genannte Zeichenfolge nicht gefunden wird, suchen Sie nach der Zeichenfolge "Reason"=>"NULL" ("Ursache"=>"NULL"). Wenn sie gefunden wird:
    - Dieser Fehler tritt auf, wenn der Quellcomputer eine leere Host-IP für die Registrierung bei einem Konfigurationsserver verwendet.
    - Befolgen Sie nach der Behebung der Probleme die [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) aufgeführten Richtlinien, um die Registrierung erneut manuell auszuführen.
4. Wenn die Zeichenfolge oben nicht gefunden wird, wechseln Sie zum Quellcomputer, und überprüfen Sie das Protokoll „C:\Programme\ASRSetupLogs\UploadedLogs\*ASRUnifiedAgentInstaller.log“. „Programme“ ist normalerweise ein verborgener Ordner. Wenn Sie ihn nicht finden können, versuchen Sie, seine Ausblendung aufzuheben. Die Fehler können mehrere Ursachen haben. Suchen Sie nach der Zeichenfolge „post request: (7) - Couldn't connect to server“ (Veröffentlichungsanforderung: (7) – Serververbindung konnte nicht hergestellt werden). Wenn sie gefunden wird:
    - Beheben Sie die Netzwerkprobleme zwischen dem Quellcomputer und dem Konfigurationsserver. Überprüfen Sie, ob der Konfigurationsserver vom Quellcomputer aus erreichbar ist, mithilfe von Netzwerktools wie ping, traceroute, Webbrowser usw. Vergewissern Sie sich, dass der Quellcomputer den Konfigurationsserver über Port 443 erreichen kann.
    - Überprüfen Sie, ob eventuell Firewallregeln auf dem Quellcomputer die Verbindung zwischen Quellcomputer und Konfigurationsserver blockieren. Arbeiten Sie mit Ihrem Netzwerkadministrator zusammen, um die Verbindungsprobleme zu beheben.
    - Achten Sie darauf, dass die [hier](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) genannten Ordner vom Schutz durch Antivirussoftware ausgeschlossen sind.
    - Versuchen Sie nach der Behebung der Netzwerkprobleme, erneut die Registrierung durchzuführen. Befolgen Sie dazu die [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) genannten Richtlinien.
5. Falls die Zeichenfolge nicht gefunden wird, suchen Sie im gleichen Protokoll nach „request: (60) - Peer certificate cannot be authenticated with given CA certificates.“ (Anforderung: (60) – Das Peerzertifikat kann mit den vorhandenen Zertifizierungsstellenzertifikaten nicht authentifiziert werden). Wenn sie gefunden wird: 
    - Dieser Fehler kann die Ursache haben, dass das Zertifikat des Konfigurationsservers abgelaufen ist oder der Quellcomputer das TLS 1.0- und höhere SLS-Protokolle nicht unterstützt oder eine Firewall die SSL-Kommunikation zwischen dem Quellcomputer und dem Konfigurationsserver blockiert.
    - Stellen Sie zur Behebung eine Verbindung mit der IP-Adresse des Konfigurationsservers mithilfe eines Webbrowsers auf dem Quellcomputer über URI https://<CSIPADDRESS>:443/ her. Vergewissern Sie sich, dass der Quellcomputer den Konfigurationsserver über Port 443 erreichen kann.
    - Überprüfen Sie, ob eventuell Firewallregeln auf dem Quellcomputer die Verbindung zwischen Quellcomputer und Konfigurationsserver blockieren. Arbeiten Sie mit Ihrem Netzwerkadministrator zusammen, um die Verbindungsprobleme zu beheben.
    - Achten Sie darauf, dass die [hier](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) genannten Ordner vom Schutz durch Antivirussoftware ausgeschlossen sind.  
    - Versuchen Sie nach der Behebung der Probleme erneut, die Registrierung durchzuführen. Befolgen Sie dazu die [hier](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) genannten Richtlinien.
6. Wenn unter Linux der Wert für die Plattform in „<INSTALLATIONSVERZ>/etc/drscout.conf“ beschädigt ist, schlägt die Registrierung fehl. Um den Fehler zu identifizieren, wechseln Sie zum Protokoll „/var/log/ua_install.log“. Sie sollten die Zeichenfolge „Aborting configuration as VM_PLATFORM value is either null or it is not VmWare/Azure“ (Die Konfiguration wird abgebrochen, da der Wert von VM_PLATFORM entweder NULL oder nicht VmWare/Azure ist). Die Plattform muss auf entweder „VmWare“ oder „Azure“ festgelegt sein. Da die drscout.conf-Datei beschädigt ist, empfiehlt es sich, den Mobilitäts-Agent zu [deinstallieren](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) und dann erneut zu installieren. Wenn bei der Deinstallation ein Fehler auftritt, befolgen Sie die Schritte unten:
    - Öffnen Sie die Datei „Installation_Directory/uninstall.sh“, und kommentieren Sie den Aufruf der Funktion *StopServices* aus.
    - Öffnen Sie die Datei „Installation_Directory/Vx/bin/uninstall“, und kommentieren Sie den Aufruf der Funktion `stop_services` aus.
    - Öffnen Sie die Datei „Installation_Directory/Fx/uninstall“, und kommentieren Sie den gesamten Abschnitt aus, der versucht, den Fx-Dienst zu beenden“.
    - Versuchen Sie jetzt, den Mobilitäts-Agent zu [deinstallieren](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service). Starten Sie nach der erfolgreichen Deinstallation das System neu, und versuchen Sie, den Agent erneut zu installieren.

## <a name="installation-failure---failed-to-load-accounts"></a>Installationsfehler: Fehler beim Laden der Konten

Dieser Fehler tritt auf, wenn der Dienst beim Installieren des Mobilitäts-Agents und seiner Registrierung beim Konfigurationsserver keine Daten aus der Transportverbindung lesen kann. Stellen Sie zur Behebung sicher, dass TLS 1.0 auf dem Quellcomputer aktiviert ist.

## <a name="change-ip-address-of-configuration-server"></a>Ändern der IP-Adresse des Konfigurationsservers

Es wird dringend empfohlen, die IP-Adresse eines Konfigurationsservers nicht zu ändern. Stellen Sie sicher, dass es sich bei allen dem Konfigurationsserver zugewiesenen IPs um statische IPs, nicht um DHCP-IPs handelt.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: Das SAML-Token ist ungültig

Um diesen Fehler zu vermeiden, vergewissern Sie sich, dass die Uhrzeit Ihrer Systemuhr nicht mehr als 15 Minuten von der Ortszeit abweicht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.

## <a name="failed-to-create-certificate"></a>Fehler beim Erstellen eines Zertifikats

Ein für die Authentifizierung von Site Recovery erforderliches Zertifikat kann nicht erstellt werden. Führen Sie Setup erneut aus, nachdem Sie sichergestellt haben, dass Sie Setup als lokaler Administrator ausführen.

## <a name="register-source-machine-with-configuration-server"></a>Registrieren des Quellcomputers beim Konfigurationsserver

### <a name="if-source-machine-has-windows-os"></a>Wenn der Quellcomputer das Windows-Betriebssystem aufweist

Führen Sie auf dem Quellcomputer den folgenden Befehl aus

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```
**Einstellung** | **Details**
--- | ---
Verwendung | UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Agent-Konfigurationsprotokolle | Unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log“.
/CSEndPoint | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
/PassphraseFilePath |  Obligatorisch. Speicherort der Passphrase. Verwenden Sie einen beliebiger UNC- oder lokalen Dateipfad.

### <a name="if-source-machine-has-linux-os"></a>Wenn der Quellcomputer ein Linux-Betriebssystem aufweist

Führen Sie auf dem Quellcomputer den folgenden Befehl aus

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```
**Einstellung** | **Details**
--- | ---
Verwendung | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
-i | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
-P |  Obligatorisch. Der vollständige Dateipfad der Datei, in der die Passphrase gespeichert ist. Verwenden Sie einen beliebigen gültigen Ordner.
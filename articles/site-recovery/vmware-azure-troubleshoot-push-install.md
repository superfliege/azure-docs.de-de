---
title: Beheben von Fehlern bei der Mobility Service-Pushinstallation während der Aktivierung der Replikation für die Notfallwiederherstellung | Microsoft-Dokumentation
description: Beheben von Fehlern bei der Mobility Service-Installation während der Aktivierung der Replikation für die Notfallwiederherstellung
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 10/29/2018
ms.openlocfilehash: c12683fee7022b84e097c2b71628776271611429
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210991"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Beheben von Problemen bei Pushinstallationen von Mobility Service

Die Installation von Mobility Service ist ein wichtiger Schritt bei der Aktivierung der Replikation. Der Erfolg dieses Schritts hängt ausschließlich davon ab, ob Sie die Voraussetzungen erfüllen und mit unterstützten Konfigurationen arbeiten. Im Folgenden werden die Gründe für die häufigsten Fehler aufgeführt, die bei der Installation von Mobility Service auftreten.

* Fehler bei Anmeldeinformationen/Berechtigungen
* Verbindungsfehler
* Nicht unterstützte Betriebssysteme

Wenn Sie die Replikation aktivieren, versucht Azure Site Recovery, die Pushinstallation des Mobility Service-Agents auf Ihrem virtuellen Computer auszuführen. Im Rahmen dieses Vorgangs versucht der Konfigurationsserver, eine Verbindung mit dem virtuellen Computer herzustellen und den Agent zu kopieren. Folgen Sie der unten aufgeführten Schritt-für-Schritt-Anleitung zur Problembehandlung, um eine erfolgreiche Installation zu ermöglichen.

## <a name="credentials-check-errorid-95107--95108"></a>Überprüfung der Anmeldeinformationen (Fehler-ID: 95107 & 95108)

* Überprüfen Sie, ob das beim Aktivieren der Replikation ausgewählte Benutzerkonto **gültig und korrekt** ist.
* Azure Site Recovery benötigt **Administratorberechtigungen**, um die Pushinstallation ausführen zu können.
  * Überprüfen Sie unter Windows, ob das Benutzerkonto auf dem Quellcomputer über lokalen oder domänenspezifischen Administratorzugriff verfügt.
  * Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren.
    * Fügen Sie zum Deaktivieren der Remote-Benutzerzugriffssteuerung unter dem Registrierungsschlüssel „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ einen neuen DWORD-Eintrag hinzu: „LocalAccountTokenFilterPolicy“. Legen Sie den Wert auf 1 fest. Führen Sie zur Ausführung dieses Schritts den folgenden Befehl an der Eingabeaufforderung aus:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Unter Linux müssen Sie das Stammkonto auswählen, um die Installation des Mobility Service-Agents erfolgreich ausführen zu können.

Wenn Sie die Anmeldeinformationen des ausgewählten Benutzerkontos ändern möchten, folgen Sie den [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) angegebenen Anweisungen.

## <a name="connectivity-check-errorid-95117--97118"></a>**Konnektivitätsprüfung (Fehler-ID: 95117 & 97118)**

* Stellen Sie sicher, dass Sie Ihren Quellcomputer über den Konfigurationsserver pingen können. Wenn Sie beim Aktivieren der Replikation einen Prozessserver für horizontales Skalieren ausgewählt haben, stellen Sie sicher, dass Sie Ihren Quellcomputer über den Prozessserver pingen können.
  * Verwenden Sie in der Befehlszeile des Quellservercomputers wie unten gezeigt Telnet zum Pingen des Konfigurationsservers/Prozessservers für horizontales Skalieren mit dem HTTPS-Port (Standardport 9443), um festzustellen, ob Netzwerkkonnektivitätsprobleme bestehen oder der Port durch die Firewall blockiert wird.

     `telnet <CS/ scale-out PS IP address> <port>`

  * Wenn Sie keine Verbindung herstellen können, lassen Sie den eingehenden Port 9443 auf dem Konfigurationsserver/Prozessserver für horizontales Skalieren zu.
  * Überprüfen Sie den Status des Diensts **InMage Scout VX Agent – Sentinel/Outpost**. Starten Sie den Dienst, wenn er nicht ausgeführt wird.

* Darüber hinaus gilt für **Linux-VMs** Folgendes:
  * Überprüfen Sie, ob die aktuellen Versionen der Pakete openssh, openssh-server und openssl installiert sind.
  * Stellen Sie unbedingt sicher, dass Secure Shell (SSH) auf Port 22 aktiviert ist und ausgeführt wird.
  * Die SFTP-Dienste müssen ausgeführt werden. Gehen Sie wie folgt vor, um das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd_config“ zu aktivieren:
    * Melden Sie sich als Root-Benutzer an.
    * Wechseln Sie zur Datei „/etc/ssh/sshd_config“, und suchen Sie die Zeile, die mit „PasswordAuthentication“ beginnt.
    * Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert in „yes“.
    * Suchen Sie die Zeile, die mit „Subsystem“ beginnt, und heben Sie die Auskommentierung der Zeile auf.
    * Starten Sie den SSHD-Dienst neu.
* Ein Verbindungsversuch könnte fehlschlagen, wenn nach einer bestimmten Zeit keine ordnungsgemäße Antwort erfolgt ist, oder es ist ein Fehler bei einer hergestellten Verbindung aufgetreten, weil der verbundene Host nicht reagiert hat.
* Es kann sich um ein Verbindungs-, Netzwerk- oder Domänenproblem handeln. Es könnte sich auch um ein Problem beim Auflösen von DNS-Namen oder um ein Auslastungsproblem des TCP-Ports handeln. Überprüfen Sie, ob in Ihrer Domäne derartige bekannte Probleme vorkommen.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Überprüfung der Datei- und Druckerfreigabedienste (Fehler-ID: 95105 & 95106)

Überprüfen Sie nach der Konnektivitätsprüfung, ob der Datei- und Druckerfreigabedienst auf Ihrem virtuellen Computer aktiviert ist.

**Windows 2008 R2 und vorherige Versionen**:

* Gehen Sie wie folgt vor, um die Datei- und Druckfreigabe über die Windows-Firewall zu aktivieren:
  * Öffnen Sie die Systemsteuerung, und klicken Sie auf „System und Sicherheit“ -> „Windows-Firewall“. Klicken Sie im linken Bereich auf „Erweiterte Einstellungen“, und klicken Sie dann in der Konsolenstruktur auf „Eingehende Regeln“.
  * Suchen Sie nach den Regeln „Datei- und Druckerfreigabe (NB-Sitzung eingehend)“ und „Datei- und Druckerfreigabe (SMB eingehend)“. Klicken Sie mit der rechten Maustaste auf jede Regel, und klicken Sie dann auf **Regel aktivieren**.
* Gehen Sie wie folgt vor, um die Dateifreigabe mit der Gruppenrichtlinie zu aktivieren:
  * Wechseln Sie zu „Start“, geben Sie „gpmc.msc“ ein, und starten Sie die Suche.
  * Öffnen Sie im Navigationsbereich die folgenden Ordner: „Richtlinie für ,Lokaler Computer‘“, „Benutzerkonfiguration“, „Administrative Vorlagen“, „Windows-Komponenten“ und „Netzwerkfreigabe“.
  * Doppelklicken Sie im Detailbereich auf **Verhindern, dass Benutzer Dateien in ihrem Profil freigeben**. Wenn Sie die Gruppenrichtlinieneinstellung deaktivieren und die Option zum Freigeben von Dateien für den Benutzer aktivieren möchten, klicken Sie auf „Deaktiviert“. Klicken Sie zum Speichern der Änderungen auf „OK“. Klicken Sie [hier](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)), um weitere Informationen zu erhalten.

Um die Datei- und Druckerfreigabe in **höheren Versionen** zu aktivieren, folgen Sie den [hier](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) angegebenen Anweisungen.

## <a name="windows-management-instrumentation-wmi-configuration-check"></a>Überprüfung der Konfiguration der Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI)

Aktivieren Sie nach der Überprüfung der Datei- und Druckerfreigabedienste den WMI-Dienst über die Firewall.

* Klicken Sie in der Systemsteuerung auf „Sicherheit“, und klicken Sie dann auf „Windows-Firewall“.
* Klicken Sie auf „Einstellungen ändern“, und klicken Sie dann auf die Registerkarte „Ausnahmen“.
* Aktivieren Sie im Fenster „Ausnahmen“ das Kontrollkästchen für die Windows-Verwaltungsinstrumentation (WMI), um den WMI-Datenverkehr durch die Firewall zu aktivieren. 

Sie können den WMI-Datenverkehr durch die Firewall auch an der Eingabeaufforderung aktivieren. Verwenden Sie den folgenden Befehl: `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Weitere Artikel zur WMI-Problembehandlung finden Sie unter den folgenden Links.

* [Grundlegende WMI-Tests](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-Problembehandlung](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Problembehandlung bei WMI-Skripts und WMI-Diensten](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Nicht unterstützte Betriebssysteme

Eine andere häufige Ursache für Fehler ist ein nicht unterstütztes Betriebssystem. Stellen Sie sicher, dass Sie ein unterstütztes Betriebssystem/eine unterstützte Kernel-Version verwenden, um eine erfolgreiche Installation von Mobility Service zu ermöglichen.

Informationen zu den von Azure Site Recovery unterstützten Betriebssystemen finden Sie in unserem [Dokument zur Unterstützungsmatrix](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="next-steps"></a>Nächste Schritte

[Vorgehensweise](vmware-azure-tutorial.md) zum Einrichten der Notfallwiederherstellung für virtuelle VMware-Computer.
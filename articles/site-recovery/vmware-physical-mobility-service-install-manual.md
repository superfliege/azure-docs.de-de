---
title: Manuelles Installieren des Mobilitätsdiensts für die Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Mobilitätsdienst-Agent installieren, um mit dem Azure Site Recovery-Dienst eine Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure auszuführen.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: ramamill
ms.openlocfilehash: b3b6d7a64873d77336b2fcf14e22f4d6ae33e3a2
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976790"
---
ms.author: ramamill

# <a name="install-the-mobility-service-manually-on-vmware-vms-and-physical-servers"></a>Manuelles Installieren des Mobilitätsdiensts auf virtuellen VMware-Computern und physischen Servern

Wenn Sie die Notfallwiederherstellung für VMware-VMs und physischen Servern mithilfe von [Azure Site Recovery](site-recovery-overview.md) einrichten, installieren Sie den [Site Recovery-Mobilitätsdienst](vmware-physical-mobility-service-overview.md) auf jedem lokalen virtuellen VMware-Computer und physischen Server.  Der Mobilitätsdienst erfasst Datenschreibvorgänge auf dem Computer und leitet sie an den Site Recovery-Prozessserver weiter.

Dieser Artikel beschreibt, wie Sie den Mobilitätsdienst manuell auf jedem Computer installieren, den Sie schützen möchten.

## <a name="create-a-passphrase"></a>Erstellen einer Passphrase

Erstellen Sie vor der Installation eine Passphrase, die während der Installation verwendet wird.

1. Melden Sie sich beim Konfigurationsserver an.
2. Öffnen Sie eine Eingabeaufforderung als Administrator.
3. Wechseln Sie in das Verzeichnis „bin“, und erstellen Sie dann eine Passphrasedatei.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Speichern Sie die Passphrase an einem sicheren Speicherort. 


## <a name="install-the-service-from-the-ui"></a>Installieren des Diensts über die Benutzeroberfläche

>[!IMPORTANT]
> Wenn Sie Azure-IaaS-VMs aus einer Azure-Region in eine andere replizieren, verwenden Sie diese Methode nicht. Verwenden Sie stattdessen die befehlszeilenbasierte Installationsmethode.

1. Ermitteln Sie die Version des Installationsprogramms, die Sie für das Betriebssystem des Computers benötigen. Die Installationsprogramme befinden sich im Ordner „%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository“. [Überprüfen Sie](vmware-physical-mobility-service-overview.md#installer-files), welches Installationsprogramm Sie benötigen.
2. Kopieren Sie die Installationsdatei auf den Computer, und führen Sie sie aus.
3. Wählen Sie unter **Installationsoption** den Eintrag **Mobilitätsdienst installieren** aus.
4. Wählen Sie den Installationspfad aus, und klicken Sie auf **Installieren**.

    ![Optionsseite für die Mobility Service-Installation](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

5. Überwachen Sie die Installation im **Installationsfortschritt**. Klicken Sie nach Abschluss der Installation auf **Mit der Konfiguration fortfahren**, um den Dienst beim Konfigurationsserver zu registrieren.

    ![Seite für die Mobility Service-Registrierung](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

6.  Geben Sie unter **Konfigurationsserverdetails** die IP-Adresse und die konfigurierte Passphrase an.  

    ![Seite für die Mobility Service-Registrierung](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

7. Klicken Sie auf **Registrieren**, um die Registrierung abzuschließen.

    ![Letzte Seite der Mobility Service-Registrierung](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-the-service-from-the-command-prompt"></a>Installieren des Diensts über die Eingabeaufforderung

### <a name="on-a-windows-machine"></a>Auf einem Windows-Computer

1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „C:\Temp“) auf dem Server, den Sie schützen möchten. 

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Führen Sie die Installation auf folgende Weise aus:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

3. Registrieren Sie den Agent beim Konfigurationsserver.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Installationseinstellungen
**Einstellung** | **Details**
--- | ---
Verwendung | UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
Setupprotokolle | Unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log“.
/Role | Obligatorischer Installationsparameter. Gibt an, ob der Mobilitätsdienst (Mobility Service, MS) oder das Masterziel (Master Target, MT) installiert werden soll.
/InstallLocation| Dieser Parameter ist optional. Gibt den Installationspfad des Mobilitätsdiensts an (beliebiger Ordner).
/Platform | Obligatorisch. Gibt die Plattform an, auf der Mobility Service installiert wird. **VMware** für VMware-VMs/physische VMware-Server; **Azure** für Azure-VMs. 
/Silent| Optional. Gibt an, ob das Installationsprogramm im unbeaufsichtigten Modus ausgeführt werden soll.

#### <a name="registration-settings"></a>Registrierungseinstellungen
**Einstellung** | **Details**
--- | ---
Verwendung | UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Agent-Konfigurationsprotokolle | Unter „%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log“.
/CSEndPoint | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
/PassphraseFilePath |  Obligatorisch. Speicherort der Passphrase. Verwenden Sie einen beliebiger UNC- oder lokalen Dateipfad.


### <a name="on-a-linux-machine"></a>Auf einem Linux-Computer

1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „/tmp“) auf dem Server, den Sie schützen möchten. Führen Sie an einem Terminal die folgenden Befehle aus:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Führen Sie die Installation auf folgende Weise aus:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Nach Abschluss der Installation muss Mobility Service auf dem Konfigurationsserver registriert werden. Führen Sie den folgenden Befehl aus, um Mobility Service beim Konfigurationsserver zu registrieren:

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```


#### <a name="installation-settings"></a>Installationseinstellungen
**Einstellung** | **Details**
--- | ---
Verwendung | ./install -d <Install Location> -r <MS|MT> -v VmWare -q
-r | Obligatorischer Installationsparameter. Gibt an, ob der Mobilitätsdienst (Mobility Service, MS) oder das Masterziel (Master Target, MT) installiert werden soll.
-d angeben, | Dieser Parameter ist optional. Gibt den Installationspfad des Mobilitätsdiensts an: /usr/local/ASR.
-v | Obligatorisch. Gibt die Plattform an, auf der Mobility Service installiert wird. **VMware** für VMware-VMs/physische VMware-Server; **Azure** für Azure-VMs. 
-q | Optional. Gibt an, ob das Installationsprogramm im unbeaufsichtigten Modus ausgeführt werden soll.

#### <a name="registration-settings"></a>Registrierungseinstellungen
**Einstellung** | **Details**
--- | ---
Verwendung | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
-i | Obligatorischer Parameter. Gibt die IP-Adresse des Konfigurationsservers an. Verwenden Sie eine beliebige gültige IP-Adresse.
-P |  Obligatorisch. Der vollständige Dateipfad der Datei, in der die Passphrase gespeichert ist. Verwenden Sie einen beliebigen gültigen Ordner.

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten der Notfallwiederherstellung für VMware-VMs](vmware-azure-tutorial.md)
- [Einrichten der Notfallwiederherstellung für physische Server](physical-azure-disaster-recovery.md)

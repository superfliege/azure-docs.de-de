---
title: Installieren von Mobility Service (VMware oder physisch in Azure) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Mobility Service-Agent installieren, um lokale VMware-VMs und physische Server mit Azure Site Recovery zu schützen.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: bc0ec09e28c5540eb919ac4e5f970f877ae27e44
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37919086"
---
# <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts 

Azure Site Recovery Mobility Service wird auf VMware-VMs und physischen Servern installiert, die Sie in Azure replizieren möchten. Der Dienst erfasst Datenschreibvorgänge auf einem Computer und leitet sie dann an den Prozessserver weiter. Stellen Sie Mobility Service auf jedem Computer (VMware-VM oder physischer Server) bereit, den Sie in Azure replizieren möchten. Sie können Mobility Service mithilfe der folgenden Methoden auf den Servern und VMware-VMs bereitstellen, die Sie schützen möchten:


* [Installieren mit Softwarebereitstellungstools wie System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)
* [Installieren mit Azure Automation DSC (Desired State Configuration, Konfiguration für den gewünschten Zustand)](vmware-azure-mobility-deploy-automation-dsc.md)
* [Manuelles Installieren über die Benutzeroberfläche](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui)
* [Manuelles Installieren über die Eingabeaufforderung](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt)
* [Installieren mithilfe der Site Recovery-Pushinstallation](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Ab Version 9.7.0.0 wird mit dem Mobility Service-Installationsprogramm auf Windows-VMs auch der jeweils neueste verfügbare [Azure-VM-Agent](../virtual-machines/extensions/features-windows.md#azure-vm-agent) installiert. Wenn ein Computer ein Failover zu Azure ausführt, erfüllt der Computer die Agent-Installationsvoraussetzungen für die Verwendung beliebiger VM-Erweiterungen.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie diese erforderlichen Schritte aus, bevor Sie Mobility Service manuell auf Ihrem Server installieren:
1. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Wechseln Sie in das Verzeichnis „bin“, und erstellen Sie dann eine Passphrasedatei.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Speichern Sie die Passphrase an einem sicheren Speicherort. Sie verwenden die Datei während der Installation von Mobility Service.
4. Die Mobility Service-Installationsprogramme für alle unterstützten Betriebssysteme befinden sich im Ordner „%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository“.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Zuordnung von Mobility Service-Installationsprogrammen zu Betriebssystemen

| Vorlagenname des Installationsprogramms| Betriebssystem |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64 Bit) </br> Windows Server 2012 (64 Bit) </br> Windows Server 2012 R2 (64 Bit) </br> Windows Server 2016 (64 Bit) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8, 6.9 (nur 64 Bit) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8, 6.9 (nur 64 Bit) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2, 7.3 (nur 64 Bit) </br> CentOS 7.0, 7.1, 7.2, 7.3 (nur 64 Bit) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit)|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (nur 64 Bit)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit)|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (nur 64 Bit)|
|Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS Server (nur 64 Bit)|
|Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 (nur 64-Bit)|
|Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8 (nur 64 Bit)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>Manuelles Installieren von Mobility Service über die grafische Benutzeroberfläche (GUI)

>[!IMPORTANT]
> Verwenden Sie die befehlszeilenbasierte Installationsmethode, wenn Sie einen Konfigurationsserver zum Replizieren von Azure-IaaS-VMs von einem Azure-Abonnement bzw. einer Azure-Region zu einem anderen Abonnement bzw. einer anderen Region nutzen.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Manuelles Installieren von Mobility Service über eine Eingabeaufforderung

### <a name="command-line-installation-on-a-windows-computer"></a>Befehlszeileninstallation auf einem Windows-Computer
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Befehlszeileninstallation auf einem Linux-Computer
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Installieren von Mobility Service mithilfe der Pushinstallation in Azure Site Recovery
Sie können eine Pushinstallation von Mobility Service durchführen, indem Sie Site Recovery verwenden. Alle Zielcomputer müssen die folgenden Voraussetzungen erfüllen.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Nachdem Mobility Service installiert wurde, klicken Sie im Azure-Portal auf **+Replizieren**, um den Schutz dieser virtuellen Computer zu starten.

## <a name="update-mobility-service"></a>Dient zum Aktualisieren von Mobility Service.

> [!WARNING]
> Stellen Sie sicher, dass der Konfigurationsserver, der Prozessserver für horizontales Hochskalieren und alle Masterzielserver, die Teil der Bereitstellung sind, aktualisiert werden, bevor Sie mit der Aktualisierung von Mobility Service auf den geschützten Servern beginnen.

1. Navigieren Sie im Azure-Portal zur Ansicht *Name Ihres Tresors* > **Replizierte Elemente**.
2. Wenn der Konfigurationsserver bereits auf die neueste Version aktualisiert wurde, wird eine Benachrichtigung angezeigt, dass ein neues Update für den Site Recovery-Replikations-Agent verfügbar ist. Sie können es per Klick installieren.

     ![Fenster „Replizierte Elemente“](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Klicken Sie auf die Benachrichtigung, um die Auswahlseite für virtuelle Computer zu öffnen.
4. Wählen Sie die virtuellen Computer aus, für die Sie Mobility Service aktualisieren möchten, und wählen Sie anschließend **OK**.

     ![VM-Liste mit replizierten Elementen](.\media\vmware-azure-install-mobility-service\update-okpng.png)

Der Aktualisierungsauftrag für Mobility Service wird für jeden ausgewählten virtuellen Computer gestartet.

> [!NOTE]
> [Hier finden Sie weitere Informationen](vmware-azure-manage-configuration-server.md) zum Aktualisieren des Kennworts für das Konto, das für die Installation von Mobility Service verwendet wird.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Deinstallieren von Mobility Service auf einem Windows Server-Computer
Verwenden Sie eine der folgenden Methoden, um Mobility Service auf einem Windows Server-Computer zu deinstallieren.

### <a name="uninstall-by-using-the-gui"></a>Deinstallieren mithilfe der grafischen Benutzeroberfläche (GUI)
1. Wählen Sie in der Systemsteuerung **Programme** aus.
2. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** und dann **Deinstallieren** aus.

### <a name="uninstall-at-a-command-prompt"></a>Deinstallieren über die Eingabeaufforderung
1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Deinstallieren von Mobility Service auf einem Linux-Computer
1. Melden Sie sich auf dem Linux-Server als ein **Root**-Benutzer an.
2. Navigieren Sie in einem Terminal zu „/user/local/ASR“.
3. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

    ```
    uninstall.sh -Y
    ```

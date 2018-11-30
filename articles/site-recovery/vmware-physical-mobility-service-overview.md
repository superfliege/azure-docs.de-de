---
title: Informationen zum Mobilitätsdienst für die Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren mehr über den Mobilitätsdienst-Agent, um mit dem Azure Site Recovery-Dienst eine Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure auszuführen.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: ramamill
ms.openlocfilehash: efbbe048456c969f0045a0588bc5b64d1138ee15
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976960"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Informationen zum Mobilitätsdienst auf virtuellen VMware-Computern und physischen Servern

Wenn Sie die Notfallwiederherstellung für VMware-VMs und physische Server mithilfe von [Azure Site Recovery](site-recovery-overview.md) einrichten, installieren Sie den Site Recovery-Mobilitätsdienst auf jedem lokalen virtuellen VMware-Computer und physischen Server.  Der Mobilitätsdienst erfasst Datenschreibvorgänge auf dem Computer und leitet sie an den Site Recovery-Prozessserver weiter. Sie können die den Mobilitätsdienst mithilfe der folgenden Methoden bereitstellen:

[Pushinstallation](vmware-azure-install-mobility-service.md): Konfigurieren Sie Site Recovery, um eine Pushinstallation des Mobilitätsdienstes durchzuführen: Dazu richten Sie bei der Einrichtung der Notfallwiederherstellung auch ein Konto ein, mit dem der Site Recovery-Prozessserver für die Installation des Dienstes auf die VM oder den physischen Server zugreifen kann.
[Manuelles Installieren](vmware-physical-mobility-service-install-manual.md): Über die Benutzeroberfläche oder die Eingabeaufforderung können Sie den Mobilitätsdienst manuell auf jedem Computer installieren.
[Automatisierte Bereitstellung](vmware-azure-mobility-install-configuration-mgr.md): Sie können die Installation mit Softwarebereitstellungstools, wie System Center Configuration Manager, automatisieren.

## <a name="azure-virtual-machine-agent"></a>Azure-VM-Agent

- **Windows-VMs**: Ab Version 9.7.0.0 des Mobilitätsdiensts wird der [Azure VM-Agent](../virtual-machines/extensions/features-windows.md#azure-vm-agent) vom Mobilitätsdienst-Installer installiert. Damit wird sichergestellt, dass die Azure-VM die Agent-Installationsvoraussetzungen für die Verwendung beliebiger VM-Erweiterungen erfüllt, wenn ein Computer ein Failover zu Azure ausführt.
- **Linux-VMs**: Der [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) muss nach dem Failover manuell auf der Azure-VM installiert werden.

## <a name="installer-files"></a>Installerdateien

Die Tabelle fasst die Installerdateien für jede VMware-VM und jedes Betriebssystem von physischen Servern zusammen. Sie können sich vor dem Start die [unterstützten Betriebssysteme](vmware-physical-azure-support-matrix.md#replicated-machines) anschauen.


**Installerdatei** | **Betriebssystem (nur 64-Bit)** 
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1 
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1,SP2,SP3 
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS-Server
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="anti-virus-on-replicated-machines"></a>Virenschutz auf replizierten Computern

Wenn auf Computern, die Sie replizieren möchten, aktive Virenschutzsoftware ausgeführt wird, achten Sie darauf, den Installationsordner von Mobility Service von Virenschutzvorgängen auszuschließen (*C:\ProgramData\ASR\agent*). Dadurch wird sichergestellt, dass die Replikation wie erwartet funktioniert.

## <a name="update-the-mobility-service"></a>Aktualisieren des Mobilitätsdiensts

1. Stellen Sie vor dem Start sicher, dass der Konfigurationsserver, der Prozessserver für horizontales Skalieren und alle Masterzielserver, die Teil der Bereitstellung sind, aktualisiert werden, bevor Sie mit der Aktualisierung von Mobility Service auf den geschützten Computern beginnen.
2. Öffnen Sie im Portal den Tresor > **Replizierte Elemente**.
3. Wenn der Konfigurationsserver bereits die neueste Version verwendet, wird eine Benachrichtigung angezeigt, dass ein neues Update für den Site Recovery-Replikations-Agent verfügbar ist. Sie können es per Klick installieren.

     ![Fenster „Replizierte Elemente“](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)

4. Klicken Sie auf die Benachrichtigung, und wählen Sie in **Agent-Update** den Computer, auf dem Sie den Mobilitätsdienst aktualisieren möchten. Klicken Sie dann auf **OK**.

     ![VM-Liste mit replizierten Elementen](.\media\vmware-azure-install-mobility-service\update-okpng.png)

5. Der Aktualisierungsauftrag für Mobility Service wird für jeden ausgewählten Computer gestartet.

## <a name="update-the-acount-used-for-push-installation-of-the-mobility-service"></a>Aktualisieren des für die Pushinstallation des Mobilitätsdiensts verwendeten Kontos

Wenn Sie Site Recovery eingesetzt haben, um die Pushinstallation des Mobilitätsdiensts zu aktivieren, haben Sie ein Konto angegeben, über das der Site Recovery-Prozessserver auf die Computer zugreift und den Dienst installiert, wenn die Replikation für den Computer aktiviert ist. Wenn Sie die Anmeldeinformationen für dieses Konto aktualisieren möchten, führen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md) aus.

## <a name="uninstall-the-mobility-service"></a>Deinstallieren von Mobility Service

### <a name="on-a-windows-machine"></a>Auf einem Windows-Computer

Die Deinstallation erfolgt über die Benutzeroberfläche oder eine Eingabeaufforderung.

- **Über die Benutzeroberfläche**: Wählen Sie in der Systemsteuerung des Computers **Programme**. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** > **Deinstallieren**.
- **Über eine Eingabeaufforderung**: Öffnen Sie ein Eingabeaufforderungsfenster als Administrator auf dem Computer. Führen Sie den folgenden Befehl aus: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="on-a-linux-machine"></a>Auf einem Linux-Computer
1. Melden Sie sich auf dem Linux-Computer als ein **root**-Benutzer an.
2. Navigieren Sie in einem Terminal zu „/user/local/ASR“.
3. Führen Sie den folgenden Befehl aus:

    ```
    uninstall.sh -Y
    ```

## <a name="next-steps"></a>Nächste Schritte

[Einrichten der Pushinstallation für den Mobilitätsdienst](vmware-azure-install-mobility-service.md)

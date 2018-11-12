---
title: Vorbereiten lokaler VMware-Server für die Notfallwiederherstellung von VMware-VMs in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie lokale VMware-Server mit dem Azure Site Recovery-Dienst für die Notfallwiederherstellung in Azure einrichten.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: f6b93f41a85ae3e87965d4e09e8ef7479f72617b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247584"
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Vorbereiten lokaler VMware-Server für die Notfallwiederherstellung in Azure

[Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

- Dies ist das zweite Tutorial in einer Reihe, welche die Einrichtung der Notfallwiederherstellung in Azure für lokale virtuelle VMware-Computer veranschaulicht. Im ersten Tutorial [richten wir die erforderlichen Azure-Komponenten](tutorial-prepare-azure.md) für die VMware-Notfallwiederherstellung ein.


> [!NOTE]
> Tutorials dienen zur Veranschaulichung des einfachsten Bereitstellungspfads für ein Szenario. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Eine ausführliche Anleitung finden Sie jeweils im Abschnitt mit der **Gewusst wie-Anleitung** für das entsprechende Szenario.

In diesem Artikel erfahren Sie, wie Sie Ihre lokale VMware-Umgebung vorbereiten, wenn Sie virtuelle VMware-Computer mithilfe von Azure Site Recovery in Azure replizieren möchten. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Vorbereiten eines Kontos auf dem vCenter-Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung
> * Vorbereiten eines Kontos für die automatische Installation des Mobility Service auf VMware-VMs
> * Überprüfen der Anforderungen für VMware-Server und virtuelle Computer
> * Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover



## <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Erstellen Sie das Konto wie folgt:

1. Erstellen Sie zum Verwenden eines dedizierten Kontos eine Rolle auf vCenter-Ebene. Geben Sie der Rolle einen Namen wie **Azure_Site_Recovery**.
2. Weisen Sie der Rolle die Berechtigungen, die in der folgenden Tabelle aufgeführt sind, zu.
3. Erstellen Sie einen Benutzer auf dem vCenter-Server oder vSphere-Host. Weisen Sie dem Benutzer die Rolle zu.

### <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen

**Aufgabe** | **Rolle/Berechtigungen** | **Details**
--- | --- | ---
**VM-Ermittlung** | Mindestens ein Benutzer mit Lesezugriff<br/><br/> Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.
**Vollständige Replikation, Failover, Failback** |  Erstellen Sie eine Rolle („Azure_Site_Recovery“) mit den erforderlichen Berechtigungen, und weisen Sie die Rolle dann einem VMware-Benutzer oder einer VMware-Gruppe zu.<br/><br/> Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.

## <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobilitätsdienst muss auf Computern installiert sein, die Sie replizieren möchten. Site Recovery kann eine Pushinstallation dieses Diensts ausführen, wenn Sie die Replikation für einen Computer aktivieren. Sie haben auch die Möglichkeit, den Dienst manuell oder mithilfe von Installationstools zu installieren.

- In diesem Tutorial installieren wir den Mobilitätsdienst mit der Pushinstallation.
- Für diese Pushinstallation müssen Sie ein Konto vorbereiten, das Site Recovery verwenden kann, um auf die den virtuellen Computer zuzugreifen. Sie geben dieses Konto an, wenn Sie die Notfallwiederherstellung an der Azure-Konsole einrichten.

Bereiten Sie das Konto wie folgt vor:

Bereiten Sie ein Domänen- oder lokales Konto mit den Berechtigungen zum Installieren auf dem virtuellen Computer vor.

- **Virtuelle Windows-Computer**: Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie für die Installation auf Windows-VMs die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer. Dazu fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
- **Virtuelle Linux-Computer**: Für die Installation auf virtuellen Linux-Computern bereiten Sie einen Superuser auf dem Linux-Quellserver vor.


## <a name="check-vmware-requirements"></a>Überprüfen der VMware-Anforderungen

Stellen Sie sicher, dass VMware-Server und virtuelle Computer die Anforderungen erfüllen.

1. [Überprüfen](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers) Sie die Anforderungen für VMware-Server.
2. [Überprüfen](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) Sie für virtuelle Linux-Computer die Dateisystem- und Speicheranforderungen. 
3. Überprüfen Sie die Unterstützung für das lokale [Netzwerk](vmware-physical-azure-support-matrix.md#network) und den lokalen [Speicher](vmware-physical-azure-support-matrix.md#storage). 
4. Überprüfen Sie, was nach einem Failover für [Azure-Netzwerke](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [Speicher](vmware-physical-azure-support-matrix.md#azure-storage) und [Computer](vmware-physical-azure-support-matrix.md#azure-compute) unterstützt wird.
5. Ihre lokalen virtuellen Computer, die Sie in Azure replizieren, müssen die [Azure-VM-Anforderungen](vmware-physical-azure-support-matrix.md#azure-vm-requirements) einhalten.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach einem Failover sollten Sie von Ihrem lokalen Netzwerk aus eine Verbindung mit den virtuellen Azure-Computern herstellen.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Windows-VMs nach dem Failover per RDP herstellen möchten:

- **Zugriff auf das Internet**. Aktivieren Sie vor dem Failover RDP auf dem lokalen virtuellen Computer. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
- **Standort-zu-Standort-VPN-Zugriff**:
    - Aktivieren Sie vor dem Failover RDP auf dem lokalen Computer.
    - RDP sollte unter **Windows-Firewall** -> **Zugelassene Apps und Feature** für **private und Domänennetzwerke** zugelassen werden.
    - Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135).
- Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls können Sie sich nach Abschluss des Updates nicht mehr auf dem virtuellen Computer anmelden.
- Aktivieren Sie nach dem Failover auf der Windows-Azure-VM die **Startdiagnose**, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Linux-VMs nach dem Failover per SSH herstellen möchten:

- Überprüfen Sie vor dem Failover auf dem lokalen Computer, ob der Secure Shell-Dienst für den automatischen Start beim Systemstart aktiviert ist.
- Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.
- Erlauben Sie nach dem Failover auf der Azure-VM in den Netzwerksicherheitsgruppen-Regeln eingehende Verbindungen am SSH-Port und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist.
- Fügen Sie der VM eine [öffentliche IP-Adresse](site-recovery-monitoring-and-troubleshooting.md) hinzu.
- Sie können die **Startdiagnose** aktivieren, um einen Screenshot des virtuellen Computers anzuzeigen.


## <a name="failback-requirements"></a>Failbackanforderungen
Wenn Sie ein Failback zu Ihrer lokalen Umgebung planen, müssen Sie auch sicherstellen, dass bestimmte [Voraussetzungen erfüllt sind](vmware-azure-reprotect.md##before-you-begin). Dieser Schritt ist aber **nicht erforderlich, um mit der Aktivierung der Notfallwiederherstellung für Ihre VMs beginnen zu können**, sondern kann auch nach dem Failover zu Azure noch ausgeführt werden.

## <a name="useful-links"></a>Nützliche Links

Wenn Sie mehrere virtuelle Computer replizieren, müssen Sie Kapazität und Bereitstellung vorab planen. [Weitere Informationen](site-recovery-deployment-planner.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung in Azure für VMware-VMs](vmware-azure-tutorial.md)

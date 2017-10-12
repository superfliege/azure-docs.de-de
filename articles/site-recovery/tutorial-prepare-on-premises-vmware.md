---
title: "Vorbereiten lokaler VMware-Server für die Notfallwiederherstellung von VMware-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie lokale VMware-Server mit dem Azure Site Recovery-Dienst für die Notfallwiederherstellung in Azure einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 90a4582c-6436-4a54-a8f8-1fee806b8af7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 2e59a78f2c348b581155484d77e272a050da1f1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Vorbereiten lokaler VMware-Server für die Notfallwiederherstellung in Azure

In diesem Tutorial wird gezeigt, wie Sie Ihre lokale VMware-Infrastruktur vorbereiten, wenn Sie VMware-VMs in Azure replizieren möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten eines Kontos auf dem vCenter-Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung
> * Vorbereiten eines Kontos für die automatische Installation des Mobility Service auf VMware-VMs
> * Überprüfen der Anforderungen für VMware-Server
> * Überprüfen der Anforderungen für VMware-VMs

In dieser Tutorialreihe erfahren Sie, wie Sie einen einzelnen virtuellen Computer mithilfe von Azure Site Recovery sichern. Wenn Sie beabsichtigen, mehrere VMware-VMs zu schützen, sollten Sie den [Bereitstellungsplaner](https://aka.ms/asr-deployment-planner) für die VMware-Replikation herunterladen. Mit diesem Tool sammeln Sie Informationen zur VM-Konformität, die Datenträger pro VM und die Datenänderungen pro Datenträger. Zudem behandelt das Tool Anforderungen an Netzwerkbandbreite und die erforderliche Azure-Infrastruktur für eine erfolgreiche Replikation und ein erfolgreiches Testfailover. [Erfahren Sie mehr](site-recovery-deployment-planner.md) über die Ausführung des Tools.

Dies ist das zweite Tutorial in der Reihe. Stellen Sie sicher, dass Sie die [Azure-Komponenten eingerichtet haben](tutorial-prepare-azure.md). Die Vorgehensweise dazu wird im vorhergehenden Tutorial beschrieben.

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

Der Mobility Service muss auf der VM installiert sein, die Sie replizieren möchten. Site Recovery installiert diesen Dienst automatisch, wenn Sie die Replikation für die VM aktivieren. Für die automatische Installation müssen Sie ein Konto vorbereiten, mit dem Site Recovery auf die VM zugreift. Sie geben dieses Konto an, wenn Sie die Notfallwiederherstellung an der Azure-Konsole einrichten.

1. Bereiten Sie ein Domänen- oder lokales Konto mit den Berechtigungen zum Installieren auf dem virtuellen Computer vor.
2. Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie für die Installation auf Windows-VMs die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer.
   - Fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
3. Für die Installation auf virtuellen Linux-Computern bereiten Sie ein root-Konto auf dem Linux-Quellserver vor.


## <a name="check-vmware-server-requirements"></a>Überprüfen der Anforderungen für VMware-Server

Vergewissern Sie sich, dass die VMware-Server die folgenden Anforderungen erfüllen.

**Komponente** | **Anforderung**
--- | ---
**vCenter-Server** | vCenter 6.5, 6.0 oder 5.5
**vSphere-Host** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Überprüfen der Anforderungen für VMware-VMs

Vergewissern Sie sich, dass der virtuelle Computer die Azure-Anforderungen in der folgenden Tabelle erfüllt.

**VM-Anforderung** | **Details**
--- | ---
**Größe des Betriebssystemdatenträgers** | Bis zu 2.048 GB
**Anzahl von Betriebssystemdatenträgern** | 1
**Anzahl von Datenträgern für Daten** | Maximal 64
**Größe des VHD-Datenträgers** | Bis zu 4.095 GB
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt.
**Freigegebene VHD** | Nicht unterstützt
**FC-Datenträger** | Nicht unterstützt
**Festplattenformat** | VHD oder VHDX<br/><br/> Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failback auf lokalen VMs durchführen, verwenden Sie weiter das VHDX-Format.
**BitLocker** | Nicht unterstützt. Deaktivieren Sie die Option vor dem Aktivieren der Replikation für einen virtuellen Computer.
**VM-Name** | Zwischen 1 und 63 Zeichen.<br/><br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Der VM-Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.
**VM-Typ** | Generation 1 – Linux oder Windows<br/><br/>Generation 2 – nur Windows

Auf dem virtuellen Computer muss ein unterstütztes Betriebssystem ausgeführt werden. Eine vollständige Liste der unterstützten Versionen finden Sie in der [Site Recovery-Supportmatrix](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions).

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Bei einem Failoverszenario können Sie von Ihrem lokalen Netzwerk aus eine Verbindung mit Ihren replizierten virtuellen Computern in Azure herstellen.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Windows-VMs nach dem Failover per RDP herstellen möchten:

1. Aktivieren Sie vor dem Failover RDP auf dem lokalen virtuellen Computer, um über das Internet darauf zuzugreifen. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über ein Site-to-Site-VPN aktivieren Sie RDP auf dem lokalen Computer. RDP sollte unter **Windows-Firewall** -> **Zugelassene Apps und Feature** für **private und Domänennetzwerke** zugelassen werden.
   Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135). Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls können Sie sich nach Abschluss des Updates nicht mehr auf dem virtuellen Computer anmelden.
3. Aktivieren Sie nach dem Failover auf der Windows-Azure-VM die **Startdiagnose**, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Linux-VMs nach dem Failover per SSH herstellen möchten:

1. Überprüfen Sie vor dem Failover auf dem lokalen Computer, ob der Secure Shell-Dienst für den automatischen Start beim Systemstart aktiviert ist. Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.

2. Erlauben Sie nach dem Failover auf der Azure-VM in den Netzwerksicherheitsgruppen-Regeln eingehende Verbindungen am SSH-Port und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist.
   Fügen Sie der VM eine [öffentliche IP-Adresse](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) hinzu. Sie können die **Startdiagnose** aktivieren, um einen Screenshot des virtuellen Computers anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung in Azure für VMware-VMs](tutorial-vmware-to-azure.md)

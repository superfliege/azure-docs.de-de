---
title: Unterstützungsmatrix für die Replikation von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die unterstützten Betriebssysteme und Komponenten für die Replikation von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/29/2018
ms.author: raynew
ms.openlocfilehash: 28ddecc45faa213d1fd536b5ad8690e151037505
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Unterstützungsmatrix für die Replikation von VMware-VMs und physischen Servern in Azure

Dieser Artikel enthält eine Übersicht über die unterstützten Komponenten und Einstellungen für die Notfallwiederherstellung von VMware-VMs in Azure mit [Azure Site Recovery](site-recovery-overview.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

**Szenario** | **Details**
--- | ---
Virtuelle VMware-Computer | Sie können die Notfallwiederherstellung für lokale VMware-VMs in Azure ausführen. Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen.
Physische Server | Sie können die Notfallwiederherstellung für lokale physische Windows- und Linux-Server in Azure ausführen. Dieses Szenario können Sie im Azure-Portal bereitstellen.

## <a name="on-premises-virtualization-servers"></a>Lokale Virtualisierungsserver

**Server** | **Anforderungen** | **Details**
--- | --- | ---
VMware | vCenter Server 6.5, 6.0 oder 5.5 bzw. vSphere 6.5, 6.0 oder 5.5 | Es wird die Verwendung eines vCenter-Servers empfohlen.
Physisch | N/V


## <a name="replicated-machines"></a>Replizierte Computer

In der folgenden Tabelle ist die Replikationsunterstützung für VMware-VMs und physische Server zusammengefasst. Site Recovery unterstützt die Replikation aller Workloads, die auf einem Computer mit einem unterstützten Betriebssystem ausgeführt werden.

**Komponente** | **Details**
--- | ---
Computereinstellungen | Computer, die in Azure repliziert werden sollen, müssen die [Azure-Anforderungen](#azure-vm-requirements) erfüllen.
Windows-Betriebssystem | Windows Server 2016, 64-Bit (Server Core, Server mit Desktopdarstellung), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1. Windows 2016 Nano Server wird nicht unterstützt.
Linux-Betriebssystem | Red Hat Enterprise Linux: 5.2 bis 5.11, 6.1 bis 6.9, 7.0 bis 7.4 <br/><br/>CentOS: 5.2 bis 5.11, 6.1 bis 6.9, 7.0 bis 7.4 <br/><br/>Ubuntu 14.04 LTS Server[ (unterstützte Kernel-Versionen)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS Server[ (unterstützte Kernel-Versionen)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (unterstützte Kernel-Versionen)](#debian-kernel-versions)<br/><br/>Oracle Enterprise Linux 6.4, 6.5, unter dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird. <br/><br/>SUSE Linux Enterprise Server 11 SP3 oder SUSE Linux Enterprise Server 11 SP4 <br/><br/>Das Upgrade replizierter Computer von SP3 auf SP4 wird nicht unterstützt. Um ein Upgrade auszuführen, deaktivieren Sie die Replikation und aktivieren Sie sie nach dem Upgrade erneut.

>[!NOTE]
>
> - Bei Linux-Distributionen werden nur die vordefinierten Kernel, die bei Veröffentlichungen/Updates von Nebenversionen der Distribution enthalten sind, unterstützt.
>
> - Das Aktualisieren geschützter Computer über die wichtigsten Linux-Distributionsversionen hinweg wird nicht unterstützt. Um ein Upgrade auszuführen, deaktivieren Sie die Replikation, aktualisieren das Betriebssystem und aktivieren die Replikation erneut.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu-Kernelversionen


**Unterstütztes Release** | **Azure Site Recovery Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.11 | 3.13.0-24-generic bis 3.13.0-128-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-generic to 3.13.0-132-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic to 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-generic bis 3.13.0-137-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-104-generic |
14.04 LTS | 9.14 | 3.13.0-24-generic bis 3.13.0-142-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-116-generic |
16.04 LTS | 9.11 | 4.4.0-21-generic bis 4.4.0-91-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-generic to 4.4.0-96-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic to 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-generic bis 4.4.0-104-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic |
16.04 LTS | 9.14 | 4.4.0-21-generic bis 4.4.0-116-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-36-generic,<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1011-azure |


### <a name="debian-kernel-versions"></a>Debian-Kernelversionen


**Unterstütztes Release** | **Azure Site Recovery Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
Debian 7 | 9.14 | 3.2.0-4-amd64 bis 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 8 | 9.14 | 3.16.0-4-amd64 bis 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.5-amd64 |


## <a name="linux-file-systemsguest-storage"></a>Linux-Dateisysteme/-Gastspeicher

**Komponente** | **Unterstützt**
--- | ---
Dateisysteme | ext3, ext4, XFS.
Volume-Manager | LVM2.
Multipfadsoftware | Gerätezuordnung.
Paravirtualisierte Speichergeräte | Von paravirtualisierten Treibern exportierte Geräte werden nicht unterstützt.
E/A-Geräte mit Blöcken mit mehreren Warteschlangen | Nicht unterstützt.
Physische Server mit HP CCISS-Speichercontroller | Nicht unterstützt.
Verzeichnisse | Diese Verzeichnisse (sofern als separate Partitionen/Dateisysteme eingerichtet) müssen sich alle auf demselben Betriebssystem-Datenträger auf dem Quellserver befinden: /(root), /boot, /usr, /usr/local, /var, /etc.</br></br> /boot muss sich auf einer Datenträgerpartition und nicht auf einem LVM-Volume befinden.<br/><br/>
Erforderlicher Speicherbedarf| 2 GB auf der /root-Partition <br/><br/> 250 MB im Installationsordner
XFSv5 | XFSv5-Features auf XFS-Dateisystemen wie Metadatenprüfsummen werden ab Mobility Service-Version 9.10 unterstützt. Verwenden Sie das Hilfsprogramm „xfs_info“, um den XFS-Superblock für die Partition zu überprüfen. Wenn „ftype“ auf 1 festgelegt ist, werden XFSv5-Features verwendet.



## <a name="network"></a>Netzwerk

**Komponente** | **Unterstützt**
--- | ---
NIC-Teaming im Hostnetzwerk | Unterstützt für VMware-VMs. <br/><br/>Nicht unterstützt für die Replikation physischer Computer
VLAN im Hostnetzwerk | Ja.
IPv4 im Hostnetzwerk | Ja.
IPv6 im Hostnetzwerk | Nein.
NIC-Teaming im Gast-/Servernetzwerk | Nein.
IPv4 im Gast-/Servernetzwerk | Ja.
IPv6 im Gast-/Servernetzwerk | Nein.
Statische IP im Gast-/Servernetzwerk (Windows) | Ja.
Statische IP im Gast-/Servernetzwerk (Linux) | Ja. <br/><br/>VMs werden für die Verwendung von DHCP bei Failback konfiguriert.
Mehrere NICs im Gast-/Servernetzwerk | Ja.


## <a name="azure-vm-network-after-failover"></a>Azure-VM-Netzwerk (nach Failover)

**Komponente** | **Unterstützt**
--- | ---
Azure ExpressRoute | Ja
ILB | Ja
ELB | Ja
Azure Traffic Manager | Ja
Multi-NIC | Ja
Reservierte IP-Adresse | Ja
IPv4 | Ja
Quell-IP-Adresse beibehalten | Ja
Azure Virtual Network-Dienstendpunkte<br/><br/> (Firewalls und virtuelle Netzwerke in Azure Storage) | Nein 

## <a name="storage"></a>Speicher
**Komponente** | **Unterstützt**
--- | ---
Host-NFS | Ja für VMware<br/><br/> Nein für physische Server
Host-SAN (iSCSI) | Ja
Multipfad auf dem Host (MPIO) | Ja, getestet mit: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM für CLARiiON
Gast-/Server-VMDK | Ja
Gast-/Server-EFI/UEFI| Teilweise (nur bei Migration zu Azure für virtuelle VMware-Computer mit Windows Server 2012 und höher) </br></br> Siehe Hinweis am Ende der Tabelle
Freigegebener Gast-/Server-Clusterdatenträger | Nein 
Verschlüsselter Gast-/Serverdatenträger | Nein 
Gast-/Server-NFS | Nein 
Gast/Server-SMB 3.0 | Nein 
Gast-/Server-RDM | Ja<br/><br/> Nicht verfügbar für physische Server
Gast-/Serverdatenträger > 1 TB | Ja<br/><br/>Bis zu 4.095 GB
Gast-/Serverdatenträger mit einer logischen Sektorgröße von 4K und einer physischen Sektorgröße von 4k | Ja
Gast-/Serverdatenträger mit einer logischen Sektorgröße von 4K und einer physischen Sektorgröße von 512 Bytes | Ja
Gast-/Servervolume mit Stripesetdatenträgern > 4 TB <br><br/>Logische Volumeverwaltung (Logical Volume Management, LVM)| Ja
Gast/Server – Speicherplätze | Nein 
Gast/Server – Datenträger bei laufendem Systembetrieb hinzufügen/entfernen | Nein 
Gast/Server – Datenträger ausschließen | Ja
Gast-/Servermultipfad (MPIO) | N/V

> [!NOTE]
> Virtuelle VMware-Computer unter Windows Server 2012 oder höher mit UEFI Boot können zu Azure migriert werden. Es gelten folgende Einschränkungen:

> - Es wird nur die Migration zu Azure unterstützt. Ein Failback zu einem lokalen VMware-Standort wird nicht unterstützt.
> - Der Server sollte über höchstens vier Partitionen auf dem Betriebssystem-Datenträger verfügen.
> - Erfordert Mobility Service-Version 9.13 oder höher.
> - Nicht bei physischen Servern unterstützt.

## <a name="azure-storage"></a>Azure-Speicher

**Komponente** | **Unterstützt**
--- | ---
Lokal redundanter Speicher | Ja
Georedundanter Speicher | Ja
Georedundanter Speicher mit Lesezugriff | Ja
Speicherebene „Kalt“ | Nein 
Speicherebene „Heiß“| Nein 
Blockblobs | Nein 
Verschlüsselung ruhender Daten (Speicherdienstverschlüsselung)| Ja
Storage Premium | Ja
Import-/Exportdienst | Nein 
Virtual Network-Dienstendpunkte<br/><br/> Storage-Firewalls und virtuelle Netzwerke in Zielspeicher-/Cachespeicherkonto (zum Speichern von Replikationsdaten) konfiguriert | Nein 
Universelle v2-Speicherkonten (heiße und kalte Ebene) | Nein 

## <a name="azure-compute"></a>Azure Compute

**Feature** | **Unterstützt**
--- | ---
Verfügbarkeitsgruppen | Ja
HUB | Ja
Verwaltete Datenträger | Ja

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Lokale virtuelle Computer, die Sie in Azure replizieren, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen. Wenn Site Recovery eine Voraussetzungsprüfung durchführt, ist diese nicht erfolgreich, wenn einige der Anforderungen nicht erfüllt sind.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Gastbetriebssystem | Überprüfung der [unterstützten Betriebssysteme](#replicated machines) | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
Architektur des Gastbetriebssystems | 64 Bit. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
Größe des Betriebssystem-Datenträgers | Bis zu 2.048 GB. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
Anzahl von Betriebssystem-Datenträgern | 1 | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.  
Anzahl von Datenträgern für Daten | Maximal 64. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.  
Datenträgergröße | Bis zu 4.095 GB | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
Netzwerkadapter | Es werden mehrere Adapter unterstützt. | 
Freigegebene VHD | Nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
Fiber-Channel-Datenträger | Nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist. 
BitLocker | Nicht unterstützt. | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen Computer aktivieren. | 
Name des virtuellen Computers | 1 bis 63 Zeichen.<br/><br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt.<br/><br/> Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. |  Aktualisieren Sie den Wert in den Computereigenschaften in Site Recovery.


## <a name="vault-tasks"></a>Tresortasks

**Aktion** | **Unterstützt**
--- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein 
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein 


## <a name="mobility-service"></a>Mobility Service

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
Einheitliches Setup von Azure Site Recovery | Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure  <br/><br/> Installiert auf lokalen VMware-Servern | 9.12.4653.1 (über das Portal verfügbar) | [Neueste Features und Fixes](https://aka.ms/latest_asr_updates)
Mobility Service | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort<br/><br/> Installiert auf einem virtuellen VMware-Computer oder auf physischen Servern, die Sie replizieren möchten | 9.12.4653.1 (über das Portal verfügbar) | [Neueste Features und Fixes](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie](tutorial-prepare-azure.md), wie Sie Azure für die Notfallwiederherstellung von VMware-VMs vorbereiten.

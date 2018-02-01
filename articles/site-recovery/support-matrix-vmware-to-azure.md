---
title: "Azure Site Recovery-Unterstützungsmatrix zum Replizieren von VMware-VMs und physischen Server in Azure | Microsoft-Dokumentation"
description: "Dieser Artikel zeigt eine Übersicht über die unterstützten Betriebssysteme und Komponenten für die Replikation von VMware-VMs in Azure mit dem Azure Site Recovery-Dienst."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: raynew
ms.openlocfilehash: ead133318d8660e8b8f4b3e9c5dddb6d75878b19
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Unterstützungsmatrix für die Replikation von VMware-VMs und physischen Servern in Azure


Dieser Artikel zeigt eine Übersicht über die unterstützten Komponenten und Einstellungen für die Notfallwiederherstellung von VMware-VMs in Azure mit dem Dienst [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Unterstützte Szenarien

**Szenario** | **Details** 
--- | --- 
**Virtuelle VMware-Computer** | Sie können die Notfallwiederherstellung für lokale VMware-VMs in Azure ausführen. Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen.
**Physische Server** | Sie können die Notfallwiederherstellung für lokale physische Windows- und Linux-Server in Azure ausführen. Dieses Szenario können Sie im Azure-Portal bereitstellen.

## <a name="on-premises-virtualization-servers"></a>Lokale Virtualisierungsserver

**Server** | **Anforderungen** | **Details**
--- | --- | ---
**VMware** | vCenter Server 6.5, 6.0 oder 5.5 oder vSphere 6.5, 6.0, 5.5 | Es wird die Verwendung eines vCenter-Servers empfohlen.
**Physische Server** | Nicht verfügbar


## <a name="replicated-machines"></a>Replizierte Computer

In der folgenden Tabelle ist die Replikationsunterstützung für Computer zusammengefasst. Site Recovery unterstützt die Replikation aller Workloads, die auf einem Computer mit einem unterstützten Betriebssystem ausgeführt werden.

**Komponente** | **Details**
--- | ---
Computerkonfiguration | Computer, die in Azure repliziert werden sollen, müssen die [Azure-Anforderungen](#failed-over-azure-vm-requirements) erfüllen.
Betriebssystem des Computers (Windows) | Windows Server 2016, 64-Bit (Server Core, Server mit Desktopdarstellung)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1
Betriebssystem des Computers (Linux) | Red Hat Enterprise Linux : 5.2 bis 5.11, 6.1 bis 6.9, 7.0 bis 7.4 <br/><br/>CentOS : 5.2 bis 5.11, 6.1 bis 6.9, 7.0 bis 7.4 <br/><br/>Ubuntu 14.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Ein Upgrade von replizierenden Computern von SLES 11 SP3 auf SLES 11 SP4 wird nicht unterstützt. Wenn für einen replizierten Computer ein Upgrade von SLES 11 SP3 auf SLES 11 SP4 durchgeführt wurde, müssen Sie die Replikation deaktivieren und den Computer nach dem Upgrade erneut schützen.)

>[!NOTE]
>
> - Bei Linux-Distributionen werden nur die vordefinierten Kernel, die bei Veröffentlichungen/Updates von Nebenversionen der Distribution enthalten sind, unterstützt.
>
> - Upgrades von Hauptversionen einer Linux-Distribution auf einem mit Azure Site Recovery geschützten virtuellen VMware-Computer oder einem physischen Server werden nicht unterstützt. Deaktivieren Sie während des Upgrades des Betriebssystems von einer Hauptversion auf eine andere (z.B. CentOS 6.* nach CentOS 7.*) die Replikation für den Computer, aktualisieren Sie das Betriebssystem auf dem Computer, und aktivieren Sie dann die Replikation erneut.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu-Kernelversionen


**Unterstütztes Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-generic bis 3.13.0-121-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-generic bis 3.13.0-128-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-generic to 3.13.0-132-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic to 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-generic bis 3.13.0-137-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-generic bis 4.4.0-81-generic,<br/>4.8.0-34-generic bis 4.8.0-56-generic,<br/>4.10.0-14-generic bis 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-generic bis 4.4.0-91-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-generic to 4.4.0-96-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic to 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-generic bis 4.4.0-104-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage-configurations"></a>Linux-Dateisysteme/-Gastspeicherkonfigurationen

**Komponente** | **Unterstützt**
--- | ---
Dateisysteme | ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS
Volume-Manager | LVM2
Multipfadsoftware | Gerätezuordnung
Paravirtualisierte Speichergeräte | Von paravirtualisierten Treibern exportierte Geräte werden nicht unterstützt.
E/A-Geräte mit Blöcken mit mehreren Warteschlangen | Nicht unterstützt.
Physische Server mit HP CCISS-Speichercontroller | Nicht unterstützt.
Verzeichnisse | Diese Verzeichnisse (sofern als separate Partitionen/Dateisysteme eingerichtet) müssen sich auf demselben Betriebssystem-Datenträger auf dem Quellserver befinden: /(root), /boot, /usr, /usr/local, /var, /etc. </br></br>  Wenn „/“ (das Stammvolume) ein LVM-Volume ist, muss sich „/boot“ auf einer separaten Partition auf dem gleichen Datenträger befinden und es darf sich nicht um ein LVM-Volume handeln.<br/><br/>
XFSv5 | XFSv5-Features auf XFS-Dateisystemen wie Metadaten-Prüfsummen werden ab Version 9.10 von Mobility Service unterstützt. Verwenden Sie das Hilfsprogramm „xfs_info“, um den XFS-Superblock für die Partition zu überprüfen. Wenn „ftype“ auf 1 festgelegt ist, werden XFSv5-Features verwendet.



## <a name="network"></a>Netzwerk

**Komponente** | **Unterstützt** 
--- | --- 
NIC-Teaming im Hostnetzwerk | Unterstützt für VMware-VMs <br/><br/>Nicht unterstützt für die Replikation physischer Computer
VLAN im Hostnetzwerk | Ja 
IPv4 im Hostnetzwerk | Ja 
IPv6 im Hostnetzwerk | Nein  
NIC-Teaming im Gast-/Servernetzwerk | Nein  
IPv4 im Gast-/Servernetzwerk | Ja 
IPv6 im Gast-/Servernetzwerk | Nein  
Statische IP im Gast-/Servernetzwerk (Windows) | Ja 
Statische IP im Gast-/Servernetzwerk (Linux) | Ja <br/><br/>VMs werden für die Verwendung von DHCP bei Failback konfiguriert  
Mehrere NICs im Gast-/Servernetzwerk | Ja 


## <a name="azure-vm-network-after-failover"></a>Azure-VM-Netzwerk (nach Failover)

**Komponente** | **Unterstützt** 
--- | --- 
ExpressRoute | Ja 
ILB | Ja 
ELB | Ja 
Traffic Manager | Ja 
Multi-NIC | Ja 
Reservierte IP-Adresse | Ja 
IPv4 | Ja 
Quell-IP-Adresse beibehalten | Ja 
Dienstendpunkte im virtuellen Netzwerk<br/><br/> (Azure Storage-Firewalls und -VNETs) | Nein  


## <a name="storage"></a>Speicher


**Komponente** | **Unterstützt** 
--- | --- 
Host-NFS | Ja für VMware<br/><br/> Nein für physische Server 
Host-SAN (iSCSI) | Ja
Multipfad auf dem Host (MPIO) | Ja – getestet mit: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM für CLARiiON
Gast-/Server-VMDK | Ja 
Gast-/Server-EFI/UEFI| Teilweise (nur bei Migration zu Azure für Windows Server 2012 und höher.) </br></br> ** Siehe Hinweis am Ende der Tabelle.
Freigegebener Gast-/Server-Clusterdatenträger | Nein  
Verschlüsselter Gast-/Serverdatenträger | Nein  
Gast-/Server-NFS | Nein  
Gast/Server-SMB 3.0 | Nein 
Gast-/Server-RDM | Ja<br/><br/> Nicht verfügbar für physische Server 
Gast-/Serverdatenträger > 1 TB | Ja<br/><br/>Bis zu 4.095 GB 
Gast-/Serverdatenträger mit einer logischen Sektorgröße von 4K und einer physischen Sektorgröße von 4k | Ja
Gast-/Serverdatenträger mit einer logischen Sektorgröße von 4K und einer physischen Sektorgröße von 512 Bytes | Ja 
Gast-/Servervolume mit Stripesetdatenträgern > 1 TB<br/><br/> Verwaltung von LVM- und logischen Volumes auf Gast/Server – Speicherplätze: nein | Hinzufügen/Entfernen von Datenträgern im laufenden Betrieb auf Gast/Server: nein | Datenträgerausschluss auf Gast/Server: ja | Multipfadfunktionen (MPIO) auf Gast/Server: keine Angabe

> [!NOTE]
> ** Virtuelle VMware-Computer oder physische Server unter Windows Server 2012 oder höher mit UEFI Boot können zu Azure migriert werden. Es gelten folgende Einschränkungen.
> - Es wird nur die Migration zu Azure unterstützt. Ein Failback zu einem lokalen VMware-Standort wird nicht unterstützt.
> - Der Server sollte über höchstens 4 Partitionen auf dem Betriebssystem-Datenträger verfügen.
> - Erfordert Azure Site Recovery Mobility Service-Version 9.13 oder höher.


## <a name="azure-storage"></a>Azure-Speicher

**Komponente** | **Unterstützt** 
--- | --- 
LRS | Ja 
GRS | Ja 
RA-GRS | Ja 
Speicherebene „Kalt“ | Nein  
Speicherebene „Heiß“| Nein  
Blockblobs | Nein  
Verschlüsselung ruhender Daten (SSE)| Ja 
Storage Premium | Ja 
Import-/Exportdienst | Nein  
Dienstendpunkte im virtuellen Netzwerk<br/><br/> Azure Storage-Firewalls und VNET-Dienstendpunkte in Zielspeicher-/Cachespeicherkonto (zum Speichern von Replikationsdaten) konfiguriert | Nein  
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Nein  


## <a name="azure-compute"></a>Azure Compute

**Feature** | **Unterstützt** 
--- | --- 
Verfügbarkeitsgruppen | Ja 
HUB | Ja   
Verwaltete Datenträger | Ja 

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Lokale virtuelle Computer, die Sie in Azure replizieren, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
**Gastbetriebssystem** | Überprüfung der [unterstützten Betriebssysteme](#replicated machines) | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Architektur des Gastbetriebssystems** | 64 Bit | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des Betriebssystemdatenträgers** | Bis zu 2048 GB | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Betriebssystemdatenträgern** | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Datenträgern für Daten** | Maximal 64, wenn Sie **VMware-VMs in Azure** replizieren, maximal 16, wenn Sie **Hyper-V-VMs in Azure** replizieren | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des VHD-Datenträgers** | Bis zu 4.095 GB | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt. | 
**Freigegebene VHD** | Nicht unterstützt | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**FC-Datenträger** | Nicht unterstützt | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Festplattenformat** | VHD  <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
**BitLocker** | Nicht unterstützt | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen Computer aktivieren.
**VM-Name** | 1 – 63 Zeichen.<br/><br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt.<br/><br/> Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. | Aktualisieren Sie den Wert in den Computereigenschaften in Site Recovery.
**VM-Typ** | Generation 1<br/><br/> Generation 2: Windows | Zwei virtuelle Computer der Generation 2 mit einem Betriebssystem-Datenträger des Typs „Basic“ (mit einem oder zwei als VHDX formatierten Datenvolumes) und weniger als 300 GB Speicherplatz werden unterstützt.<br></br>Linux-VMs der Generation 2 werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="vault-tasks"></a>Tresortasks

**Aktion** | **Unterstützt** 
--- | --- 
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  


## <a name="mobility-service"></a>Mobilitätsdienst

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
**Einheitliches Setup von Azure Site Recovery** | Koordiniert die Kommunikation zwischen lokalen VMware-Servern und Azure  <br/><br/> Installiert auf lokalen VMware-Servern | 9.12.4653.1 (über das Portal verfügbar) | [Neueste Features und Fixes](https://aka.ms/latest_asr_updates)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physischen Servern und Azure/sekundärem Standort<br/><br/> Installiert auf einem virtuellen VMware-Computer oder auf physischen Servern, die Sie replizieren möchten  | 9.12.4653.1 (über das Portal verfügbar) | [Neueste Features und Fixes](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie](tutorial-prepare-azure.md), wie Sie Azure für die Notfallwiederherstellung von VMware-VMs vorbereiten.

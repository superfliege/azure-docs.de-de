---
title: "Unterstützungsmatrix für Hyper-V-Replikation in Azure | Microsoft-Dokumentation"
description: "Fasst die unterstützten Komponenten und Anforderungen für die Hyper-V-Replikation in Azure mit Azure Site Recovery zusammen"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 81983b9287a6b8073724f0cd973929f4b4677d4a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Unterstützungsmatrix für die Hyper-V-Replikation in Azure


Dieser Artikel beschreibt die unterstützten Komponenten und Einstellungen für die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure mit dem Dienst [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Unterstützte Szenarien

**Szenario** | **Details**
--- | --- 
**Hyper-V mit VMM** | Sie können die Notfallwiederherstellung in Azure für virtuelle Computer auf Hyper-V-Hosts durchführen, die im System Center Virtual Machine Manager-Fabric (VMM) ausgeführt werden.<br/><br/> Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen.<br/><br/> Wenn Hyper-V-Hosts von VMM verwaltet werden, können Sie die Notfallwiederherstellung auch auf einem sekundären lokalen Standort ausführen. Weitere Informationen zu diesem Szenario finden Sie in [diesem Artikel](tutorial-vmm-to-vmm.md).
**Hyper-V ohne VMM** | Sie können die Notfallwiederherstellung für Azure für virtuelle Computer auf Hyper-V-Hosts, die nicht von VMM verwaltet werden, ausführen.<br/><br/> Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen. 


## <a name="on-premises-servers"></a>Lokale Server

**Server** | **Anforderungen** | **Details**
--- | --- | ---
**Hyper-V (ohne VMM)** | Windows Server 2016, Windows Server 2012 R2 mit den neuesten Updates. | Für die Konfiguration eines Hyper-V-Standorts in Site Recovery wird das Kombinieren von Hosts mit Windows Server 2016 und 2012 R2 nicht unterstützt.<br/><br/> Für virtuelle Computer auf einem Host unter Windows Server 2016 wird die Wiederherstellung an einem alternativen Standort nicht unterstützt.
**Hyper-V (mit VMM)** | VMM 2016, VMM 2012 R2 | Wenn VMM verwendet wird, müssen Windows Server 2016-Hosts mit VMM 2016 verwaltet werden.<br/><br/> VMM-Clouds mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts werden derzeit nicht unterstützt.<br/><br/> Umgebungen, die ein Upgrade eines vorhandenen VMM 2012 R2-Servers auf 2016 enthalten, werden nicht unterstützt.


## <a name="replicated-vms"></a>Replizierte VMs


In der folgenden Tabelle wird die VM-Unterstützung zusammengefasst. Site Recovery unterstützt alle Workloads, die auf einem unterstützten Betriebssystem ausgeführt werden. 

 **Komponente** | **Details**
--- | ---
Konfiguration des virtuellen Computers | Virtuelle Computer, die nach Azure repliziert werden sollen, müssen die [Azure-Anforderungen](#failed-over-azure-vm-requirements) erfüllen.
Gastbetriebssystem | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme.<br/><br/> Windows Server 2016 Nano Server wird nicht unterstützt.




## <a name="hyper-v-network-configuration"></a>Hyper-V-Netzwerkkonfiguration

**Komponente** | **Hyper-V mit VMM** | **Hyper-V ohne VMM**
--- | --- | ---
Hostnetzwerk: NIC-Teambildung | Ja
Hostnetzwerk: VLAN | Ja
Hostnetzwerk: IPv4 | Ja
Hostnetzwerk: IPv6 | Nein 
Gast-VM-Netzwerk: NIC-Teambildung | Nein 
Gast-VM-Netzwerk: IPv4 | Ja
Gast-VM-Netzwerk: IPv6 | Nein 
Gast-VM-Netzwerk: statische IP-Adresse (Windows) | Ja
Gast-VM-Netzwerk: statische IP-Adresse (Linux) | Nein 
Gast-VM-Netzwerk: Multi-NIC | Ja



## <a name="azure-vm-network-configuration-after-failover"></a>Netzwerkkonfiguration für virtuellen Azure-Computer (nach Failover)

**Komponente** | **Hyper-V mit VMM** | **Hyper-V ohne VMM**
--- | --- | ---
ExpressRoute | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Multi-NIC | Ja | Ja
Reservierte IP | Ja | Ja
IPv4 | Ja | Ja
Quell-IP-Adresse beibehalten | Ja | Ja
VNET-Dienstendpunkte<br/><br/> (Firewalls und virtuelle Netzwerke in Azure Storage) | Nein  | Nein 


## <a name="hyper-v-host-storage"></a>Hyper-V-Hostspeicher

**Speicher** | **Hyper-V mit VMM** | Hyper-V ohne VMM
--- | --- | --- | ---
NFS | Nicht verfügbar | Nicht verfügbar
SMB 3.0 | Ja | Ja
SAN (ISCSI) | Ja | Ja
Multipfad (MPIO). Getestet mit:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM für CLARiiON | Ja | Ja

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V-VM-Gastspeicher

**Speicher** | **Hyper-V mit VMM** | Hyper-V ohne VMM
--- | --- | ---
VMDK | Nicht verfügbar | Nicht verfügbar
VHD/VHDX | Ja | Ja
VM der 2. Generation | Ja | Ja
EFI/UEFI| Ja | Ja
Freigegebener Clusterdatenträger | Nein  | Nein 
Verschlüsselter Datenträger | Nein  | Nein 
NFS | Nicht verfügbar | Nicht verfügbar
SMB 3.0 | Nein  | Nein 
RDM | Nicht verfügbar | Nicht verfügbar
Datenträger > 1 TB | Ja, bis zu 4.095 GB | Ja, bis zu 4.095 GB
Datenträger: 4K für logischen und physischen Sektor | Nicht unterstützt: 1./2. Generation | Nicht unterstützt: 1./2. Generation
Datenträger: 4K für logischen, 512 Byte für physischen Sektor | Ja |  Ja
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM (logische Volumeverwaltung) | Ja | Ja
Speicherplätze | Ja | Ja
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein  | Nein 
Ausschließen von Datenträgern | Ja | Ja
Multipfad (MPIO) | Ja | Ja

## <a name="azure-storage"></a>Azure-Speicher

**Komponente** | **Hyper-V mit VMM** | **Hyper-V ohne VMM**
--- | --- | ---
LRS | Ja | Ja
GRS | Ja | Ja
RA-GRS | Ja | Ja
Speicherebene „Kalt“ | Nein  | Nein 
Speicherebene „Heiß“| Nein  | Nein 
Blockblobs | Nein  | Nein 
Verschlüsselung ruhender Daten (SSE)| Ja | Ja
Storage Premium | Ja | Ja
Import-/Exportdienst | Nein  | Nein 
VNET-Dienstendpunkte (Azure Storage-Firewalls und -VNETs) auf Ziel für Cachespeicherkonto für Replikationsdaten | Nein  | Nein 


## <a name="azure-compute-features"></a>Azure-Computefeatures

**Feature** | **Hyper-V mit VMM** | **Hyper-V ohne VMM**
--- | --- | ---
Verfügbarkeitsgruppen | Ja | Ja
HUB | Ja | Ja  
Verwaltete Datenträger | Ja, für Failover<br/><br/> Failback von verwalteten Datenträgern wird nicht unterstützt. | Ja, für Failover<br/><br/> Failback von verwalteten Datenträgern wird nicht unterstützt.

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Lokale virtuelle Computer, die Sie in Azure replizieren, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
**Gastbetriebssystem** | Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Architektur des Gastbetriebssystems** | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des Betriebssystemdatenträgers** | Bis zu 2.048 GB für VMs der 1. Generation.<br/><br/> Bis zu 300 GB für VMs der 2. Generation.  | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Betriebssystemdatenträgern** | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Datenträgern für Daten** | Maximal 16  | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des VHD-Datenträgers** | Bis zu 4.095 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt. |
**Freigegebene VHD** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**FC-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Festplattenformat** | VHD  <br/><br/> VHDX | Site Recovery konvertiert VHDX automatisch in VHD, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
**BitLocker** | Nicht unterstützt | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen virtuellen Computer aktivieren.
**VM-Name** | Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Der VM-Name muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den VM-Eigenschaften in Site Recovery.
**VM-Typ** | Generation 1<br/><br/> Generation 2: Windows | Zwei virtuelle Computer der Generation 2 mit einem Betriebssystem-Datenträger des Typs „Basic“ (mit einem oder zwei als VHDX formatierten Datenvolumes) und weniger als 300 GB Speicherplatz werden unterstützt.<br></br>Linux-VMs der Generation 2 werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Aktionen mit dem Recovery Services-Tresor

**Aktion** |  **Hyper-V mit VMM** | **Hyper-V ohne VMM**
--- | --- | --- 
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  | Nein  
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  | Nein  


## <a name="provider-and-agent"></a>Anbieter und Agent

Damit Ihre Bereitstellung mit den Einstellungen in diesem Artikel kompatibel ist, stellen Sie sicher, dass Sie die neuesten Anbieter- und Agent-Versionen ausführen.

**Name** | **Beschreibung** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure <br/><br/> Hyper-V mit VMM: auf VMM-Servern installiert<br/><br/> Hyper-V ohne VMM: auf Hyper-V-Hosts installiert| Neueste Version: 5.1.2700.1 (über das Portal verfügbar)<br/><br/> [Neueste Features und Fixes](https://aka.ms/latest_asr_updates)
**Microsoft Azure Recovery Services-Agent (MARS)** | Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/> Installiert auf lokalen Hyper-V-Servern (mit oder ohne VMM) | Aktueller Agent über das Portal verfügbar






## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Azure für die Notfallwiederherstellung von lokalen Hyper-V-VMs [vorbereiten](tutorial-prepare-azure.md). 

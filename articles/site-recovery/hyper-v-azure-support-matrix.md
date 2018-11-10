---
title: Unterstützungsmatrix für die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure | Microsoft-Dokumentation
description: Dieser Artikel fasst die unterstützten Komponenten und Anforderungen für die Hyper-V-Notfallwiederherstellung in Azure mit Azure Site Recovery zusammen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: e389f37448211afc35fb98572161be4fcaea7556
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210719"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Unterstützungsmatrix für die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure


In diesem Artikel werden die unterstützten Komponenten und Einstellungen für die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure mit [Azure Site Recovery](site-recovery-overview.md) beschrieben.


## <a name="supported-scenarios"></a>Unterstützte Szenarien

**Szenario** | **Details**
--- | ---
Hyper-V mit Virtual Machine Manager | Sie können die Notfallwiederherstellung in Azure für virtuelle Computer auf Hyper-V-Hosts durchführen, die im System Center Virtual Machine Manager-Fabric verwaltet werden.<br/><br/> Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen.<br/><br/> Wenn Hyper-V-Hosts mit Virtual Machine Manager verwaltet werden, können Sie auch die Notfallwiederherstellung an einem sekundären lokalen Standort durchführen. Weitere Informationen zu diesem Szenario finden Sie in [diesem Tutorial](hyper-v-vmm-disaster-recovery.md).
Hyper-V ohne Virtual Machine Manager | Sie können die Notfallwiederherstellung in Azure für virtuelle Computer auf Hyper-V-Hosts ausführen, die nicht von Virtual Machine Manager verwaltet werden.<br/><br/> Dieses Szenario können Sie über das Azure-Portal oder mit PowerShell bereitstellen.


## <a name="on-premises-servers"></a>Lokale Server

**Server** | **Anforderungen** | **Details**
--- | --- | ---
Hyper-V (Ausführung ohne Virtual Machine Manager) | Windows Server 2016 (einschließlich Server Core-Installation), Windows Server 2012 R2 mit den neuesten Updates | Für die Konfiguration eines Hyper-V-Standorts in Site Recovery wird das Kombinieren von Hosts mit Windows Server 2016 und 2012 R2 nicht unterstützt.<br/><br/> Für virtuelle Computer auf einem Host unter Windows Server 2016 wird die Wiederherstellung an einem alternativen Standort nicht unterstützt.
Hyper-V (Ausführung mit Virtual Machine Manager) | Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Wenn Virtual Machine Manager verwendet wird, sollten Windows Server 2016-Hosts in Virtual Machine Manager 2016 verwaltet werden.<br/><br/> Eine Virtual Machine Manager-Cloud mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts wird derzeit nicht unterstützt.<br/><br/> Umgebungen, die ein Upgrade eines vorhandenen Virtual Machine Manager 2012 R2-Servers auf 2016 enthalten, werden nicht unterstützt.


## <a name="replicated-vms"></a>Replizierte VMs


In der folgenden Tabelle wird die VM-Unterstützung zusammengefasst. Site Recovery unterstützt alle Workloads, die auf einem unterstützten Betriebssystem ausgeführt werden.

 **Komponente** | **Details**
--- | ---
Konfiguration des virtuellen Computers | Virtuelle Computer, die nach Azure repliziert werden sollen, müssen die [Azure-Anforderungen](#azure-vm-requirements) erfüllen.
Gastbetriebssystem | Alle [für Azure unterstützten](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases) Gastbetriebssysteme.<br/><br/> Windows Server 2016 Nano Server wird nicht unterstützt.


## <a name="vmdisk-management"></a>VM-/Datenträgerverwaltung

**Aktion** | **Details**
--- | ---
Größe des Datenträgers auf einer replizierten Hyper-V-VM ändern | Nicht unterstützt. Deaktivieren Sie die Replikation, nehmen Sie die Änderung vor, und aktivieren Sie anschließend erneut die Replikation für die VM.
Datenträger auf einer replizierten Hyper-V-VM hinzufügen | Nicht unterstützt. Deaktivieren Sie die Replikation, nehmen Sie die Änderung vor, und aktivieren Sie anschließend erneut die Replikation für die VM.

## <a name="hyper-v-network-configuration"></a>Hyper-V-Netzwerkkonfiguration

**Komponente** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
Hostnetzwerk: NIC-Teaming | JA | JA
Hostnetzwerk: VLAN | JA | JA
Hostnetzwerk: IPv4 | JA | JA
Hostnetzwerk: IPv6 | Nein  | Nein 
Gast-VM-Netzwerk: NIC-Teaming | Nein  | Nein 
Gast-VM-Netzwerk: IPv4 | JA | JA
Gast-VM-Netzwerk: IPv6 | Nein  | JA
Gast-VM-Netzwerk: statische IP-Adresse (Windows) | JA | JA
Gast-VM-Netzwerk: statische IP-Adresse (Linux) | Nein  | Nein 
Gast-VM-Netzwerk: Multi-NIC | JA | JA



## <a name="azure-vm-network-configuration-after-failover"></a>Netzwerkkonfiguration für virtuellen Azure-Computer (nach Failover)

**Komponente** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | JA | JA
ILB | JA | JA
ELB | JA | JA
Azure Traffic Manager | JA | JA
Multi-NIC | JA | JA
Reservierte IP | JA | JA
IPv4 | JA | JA
Quell-IP-Adresse beibehalten | JA | JA
Azure Virtual Network-Dienstendpunkte<br/> (ohne Azure Storage-Firewalls) | JA | JA
Beschleunigter Netzwerkbetrieb | Nein  | Nein 


## <a name="hyper-v-host-storage"></a>Hyper-V-Hostspeicher

**Speicher** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | --- | ---
NFS | Nicht verfügbar | Nicht verfügbar
SMB 3.0 | JA | JA
SAN (ISCSI) | JA | JA
Multipfad (MPIO). Getestet mit:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM für CLARiiON | JA | JA

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V-VM-Gastspeicher

**Speicher** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
VMDK | Nicht verfügbar | Nicht verfügbar
VHD/VHDX | JA | JA
VM der 2. Generation | JA | JA
EFI/UEFI| JA | JA
Freigegebener Clusterdatenträger | Nein  | Nein 
Verschlüsselter Datenträger | Nein  | Nein 
NFS | Nicht verfügbar | Nicht verfügbar
SMB 3.0 | Nein  | Nein 
RDM | Nicht verfügbar | Nicht verfügbar
Datenträger > 1 TB | Ja, bis zu 4.095 GB | Ja, bis zu 4.095 GB
Datenträger: 4K für logischen und physischen Sektor | Nicht unterstützt: 1./2. Generation | Nicht unterstützt: 1./2. Generation
Datenträger: 4K für logischen, 512 Byte für physischen Sektor | JA |  JA
Logische Volumeverwaltung (Logical Volume Management, LVM). LVM wird nur für Datenträger unterstützt. Azure stellt nur einen einzelnen Betriebssystemdatenträger bereit. | JA | JA
Volume mit Stripesetdatenträger > 1 TB | JA | JA
Speicherplätze | JA | JA
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein  | Nein 
Ausschließen von Datenträgern | JA | JA
Multipfad (MPIO) | JA | JA

## <a name="azure-storage"></a>Azure Storage

**Komponente** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
Lokal redundanter Speicher | JA | JA
Georedundanter Speicher | JA | JA
Georedundanter Speicher mit Lesezugriff | JA | JA
Speicherebene „Kalt“ | Nein  | Nein 
Speicherebene „Heiß“| Nein  | Nein 
Blockblobs | Nein  | Nein 
Verschlüsselung ruhender Daten (SSE)| JA | JA
Storage Premium | JA | JA
Import-/Exportdienst | Nein  | Nein 
Azure Storage-Firewalls für virtuelle Netzwerke, konfiguriert im Zielspeicher-/Cachespeicherkonto (zum Speichern von Replikationsdaten) | Nein  | Nein 


## <a name="azure-compute-features"></a>Azure-Computefeatures

**Feature** | **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
Verfügbarkeitsgruppen | JA | JA
HUB | JA | JA  
Verwaltete Datenträger | Ja, für Failover.<br/><br/> Failback von verwalteten Datenträgern wird nicht unterstützt. | Ja, für Failover.<br/><br/> Failback von verwalteten Datenträgern wird nicht unterstützt.

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Lokale virtuelle Computer, die Sie in Azure replizieren, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Gastbetriebssystem | Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Architektur des Gastbetriebssystems | 64 Bit | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des Betriebssystem-Datenträgers | Bis zu 2.048 GB für VMs der 1. Generation.<br/><br/> Bis zu 300 GB für VMs der 2. Generation.  | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 16  | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des VHD-Datenträgers | Bis zu 4.095 GB | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Freigegebene VHD | Nicht unterstützt | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Nicht unterstützt | Beim Überprüfen der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Festplattenformat | VHD  <br/><br/> VHDX | Site Recovery konvertiert VHDX automatisch in VHD, wenn Sie das Failover auf Azure anstoßen. Wenn Sie ein Failback zum lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
BitLocker | Nicht unterstützt | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen virtuellen Computer aktivieren.
Name des virtuellen Computers | Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Der VM-Name muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den VM-Eigenschaften in Site Recovery.
VM-Typ | Generation 1<br/><br/> Generation 2 – Windows | Zwei virtuelle Computer der Generation 2 mit einem Betriebssystem-Datenträger des Typs „Basic“ (mit einem oder zwei als VHDX formatierten Datenvolumes) und weniger als 300 GB Speicherplatz werden unterstützt.<br></br>Linux-VMs der Generation 2 werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Aktionen mit dem Recovery Services-Tresor

**Aktion** |  **Hyper-V mit Virtual Machine Manager** | **Hyper-V ohne Virtual Machine Manager**
--- | --- | ---
Tresor über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  | Nein 
Speicher, Netzwerk, Azure-VMs über Ressourcengruppen hinweg verschieben<br/><br/> Innerhalb von und über Abonnements hinweg | Nein  | Nein 


## <a name="provider-and-agent"></a>Anbieter und Agent

Damit Ihre Bereitstellung mit den Einstellungen in diesem Artikel kompatibel ist, stellen Sie sicher, dass Sie die neuesten Anbieter- und Agent-Versionen ausführen.

**Name** | **Beschreibung** | **Details**
--- | --- | --- | --- | ---
Azure Site Recovery-Anbieter | Koordiniert die Kommunikation zwischen lokalen Servern und Azure <br/><br/> Hyper-V mit Virtual Machine Manager: Installation auf Virtual Machine Manager-Servern<br/><br/> Hyper-V ohne Virtual Machine Manager: Installation auf Hyper-V-Hosts| Aktuelle Version: 5.1.2700.1 (über das Azure-Portal verfügbar)<br/><br/> [Neueste Features und Fixes](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure Recovery Services-Agent | Koordiniert die Replikation zwischen Hyper-V-VMs und Azure<br/><br/> Installation auf lokalen Hyper-V-Servern (mit oder ohne Virtual Machine Manager) | Aktueller Agent über das Portal verfügbar






## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Azure für die Notfallwiederherstellung von lokalen Hyper-V-VMs [vorbereiten](tutorial-prepare-azure.md).

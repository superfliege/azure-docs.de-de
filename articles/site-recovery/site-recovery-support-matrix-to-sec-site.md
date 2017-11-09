---
title: "Unterstützungsmatrix für die Replikation an einen sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Enthält eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: c0f86e13e21f2af323e0a306b381054b6eb76755
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>Unterstützungsmatrix für die Replikation an einen sekundären Standort mit Azure Site Recovery

Dieser Artikel beschreibt, was unterstützt wird, wenn Sie den [Azure Site Recovery](site-recovery-overview.md)-Dienst für die Replikation an einen sekundären lokalen Standort verwenden.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

**Bereitstellung** | **Details** 
--- | ---
**VMware zu VMware** | Notfallwiederherstellung von lokalen VMware-VMs zu einem sekundären VMware-Standort.<br/><br/> Herunterladen des [InMage Scout-Benutzerhandbuchs](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf).
**Hyper-V zu Hyper-V** | Notfallwiederherstellung von lokalen Hyper-V-VMs in VMM-Clouds zu einer sekundären VMM-Cloud.<br></br> Ohne VMM nicht unterstützt.



  

## <a name="host-servers"></a>Hostserver

**Bereitstellung** | **Unterstützung**
--- | ---
**VMware-VM/physische Server** | vCenter 5.5 oder 6.0 (Unterstützung nur für 5.5-Features) 
**Hyper-V mit VMM** | Windows Server 2016 und Windows Server 2012 R2 mit den neuesten Updates.<br/><br/> Windows Server 2016-Hosts müssen mit VMM 2016 verwaltet werden.<br/><br/> VMM 2016-Clouds mit einer Mischung aus Windows Server 2016- und Windows Server 2012 R2-Hosts werden derzeit nicht unterstützt.<br/><br/> Bereitstellungen, die ein Upgrade für eine vorhandene VMM 2012 R2-Instanz auf System Center 2016 umfassen, werden derzeit nicht unterstützt.


## <a name="support-for-replicated-machine-os-versions"></a>Unterstützung für replizierte Computer-Betriebssystemversionen

In der folgenden Tabelle werden die unterstützten Betriebssysteme für mit Site Recovery replizierte Computer aufgeführt. Unter dem unterstützten Betriebssystem können beliebige Workloads ausgeführt werden.

**VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | ---
64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4 oder 6.5, unter dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird <br/><br/> SUSE Linux Enterprise Server 11 SP3 | Alle [von Hyper-V unterstützten](https://technet.microsoft.com/library/mt126277.aspx) Gastbetriebssysteme

## <a name="linux-machine-storage"></a>Speicher eines Linux-Computers

Nur Linux-Computer mit dem folgenden Speicher können repliziert werden:

- Dateisystem (EXT3, ETX4, ReiserFS, XFS)
- Multipfadsoftware (Geräte-Mapper)
- Volume-Manager (LVM2)
- Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt.
- Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.

## <a name="network-configuration"></a>Network Configuration

### <a name="hosts"></a>Host

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Ja | Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nein | Nein

### <a name="guest-vms"></a>Gast-VMs

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NIC-Teaming | Nein | Nein
IPv4 | Ja | Ja
IPv6 | Nein | Nein
Statische IP-Adresse (Windows) | Ja | Ja
Statische IP-Adresse (Linux) | Ja | Ja
Multi-NIC | Ja | Ja


## <a name="storage"></a>Speicher

### <a name="host-storage"></a>Hostspeicher

**Speicher (Host)** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
NFS | Ja | –
SMB 3.0 | N/V | Ja
SAN (ISCSI) | Ja | Ja
Multipfad (MPIO) | Ja | Ja

### <a name="guest-or-physical-server-storage"></a>Gast- oder physischer Serverspeicher

**Konfiguration** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
VMDK | Ja | –
VHD/VHDX | N/V | Ja (bis zu 16 Datenträger)
Gen 2-VM | – | Ja
Freigegebener Clusterdatenträger | Ja  | Nein
Verschlüsselter Datenträger | Nein | Nein
UEFI| Nein | –
NFS | Nein | Nein
SMB 3.0 | Nein | Nein
RDM | Ja | N/V
Datenträger > 1 TB | Nein | Ja
Volume mit Stripesetdatenträgern > 1 TB<br/><br/> LVM | Ja | Ja
Speicherplätze | Nein | Ja
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nein | Nein
Ausschließen von Datenträgern | Nein | Ja
Multipfad (MPIO) | N/V | Ja

## <a name="vaults"></a>Tresore

**Aktion** | **VMware-/physische Server** | **Hyper-V (mit VMM)**
--- | --- | ---
Verschieben von Tresoren zwischen Ressourcengruppen (innerhalb oder zwischen Abonnements) | Nein | Nein
Verschieben von Speicher, Netzwerk, Azure-VMs zwischen Ressourcengruppen (innerhalb oder zwischen Abonnements) | Nein | Nein

## <a name="provider-and-agent"></a>Anbieter und Agent

**Name** | **Beschreibung** | **Aktuelle Version** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery-Anbieter** | Koordiniert die Kommunikation zwischen lokalen Servern und Azure <br/><br/> Installiert auf lokalen VMM-Servern oder auf Hyper-V-Servern, falls kein VMM-Server vorhanden ist | 5.1.19 ([über das Portal verfügbar](http://aka.ms/downloaddra)) | [Neueste Features und Fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsdienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern oder physischen Servern und dem sekundärem Standort<br/><br/> Wird auf einer VMware-VM oder physischen Servern installiert, die Sie replizieren möchten  | Nicht verfügbar (über das Portal verfügbar) | N/V


## <a name="next-steps"></a>Nächste Schritte

- [Replizieren von Hyper-V-VMs in VMM-Clouds in einer sekundären Cloud](tutorial-vmm-to-vmm.md)
- [Replizieren von VMware-VMs und physischen Servern an einem sekundären Standort](tutorial-vmware-to-vmware.md)

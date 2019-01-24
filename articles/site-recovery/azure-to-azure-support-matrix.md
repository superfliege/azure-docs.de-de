---
title: Azure Site Recovery-Unterstützungsmatrix für die Notfallwiederherstellung von Azure IaaS-VMs zwischen Azure-Regionen mit Azure Site Recovery | Microsoft-Dokumentation
description: Übersicht über die unterstützten Betriebssysteme und Konfigurationen für die Azure Site Recovery-Replikation von virtuellen Azure-Computern (VMs) aus einer Region in eine andere für die Notfallwiederherstellung.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/21/2019
ms.author: raynew
ms.openlocfilehash: fa25c144737c4d070b2dbd08700c68e617950e0d
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452257"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Unterstützungsmatrix für die Replikation von einer Azure-Region in eine andere

In diesem Artikel sind unterstützte Konfigurationen und Komponenten zusammengefasst, wenn Sie die Notfallwiederherstellung mit Replikation, Failover und Wiederherstellung von Azure-VMs aus einer Azure-Region in eine andere bereitstellen, indem Sie den [Azure Site Recovery](site-recovery-overview.md)-Dienst verwenden.


## <a name="deployment-method-support"></a>Unterstützung für die Bereitstellungsmethode

**Bereitstellungsmethode** |  **Unterstützt/Nicht unterstützt**
--- | ---
**Azure-Portal** | Unterstützt
**PowerShell** | [Replikation von Azure zu Azure mit PowerShell](azure-to-azure-powershell.md)
**REST-API** | Unterstützt
**BEFEHLSZEILENSCHNITTSTELLE (CLI)** | Derzeit nicht unterstützt


## <a name="resource-support"></a>Ressourcenunterstützung

**Ressourcenaktion** | **Details**
--- | --- | ---
**Tresor über Ressourcengruppen hinweg verschieben** | Nicht unterstützt
**Verschieben von Compute-, Speicher- und Netzwerkressourcen über Ressourcengruppen hinweg** | Nicht unterstützt.<br/><br/> Wenn Sie eine VM oder zugehörige Komponenten wie Speicher bzw. Netzwerke verschieben, nachdem die VM repliziert wurde, müssen Sie die Replikation für die VM deaktivieren und dann wieder aktivieren.
**Replizieren von Azure-VMs von einem Abonnement in ein anderes zur Notfallwiederherstellung** | Innerhalb des gleichen Azure Active Directory-Mandanten unterstützt.
**Migrieren von VMs zwischen Regionen innerhalb von unterstützten geografischen Clustern (innerhalb von Abonnements und übergreifend)** | Innerhalb des gleichen Azure Active Directory-Mandanten unterstützt.
**Migrieren von VMs in derselben Region** | Nicht unterstützt.

## <a name="region-support"></a>Unterstützung für Regionen

Sie können virtuelle Computer zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren. Geographische Cluster werden unter Berücksichtigung von Datenlatenz und Souveränität definiert.


**Geografischer Cluster** | **Azure-Regionen**
-- | --
Amerika | Kanada, Osten; Kanada, Mitte; USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2; USA, Mitte; USA, Norden-Mitte
Europa | „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“, „Europa, Norden“, „Europa, Westen“, „Frankreich, Mitte“, „Frankreich, Süden“
Asien | Indien, Süden; Indien, Mitte; Asien, Südosten; Asien, Osten; Japan, Osten; Japan, Westen; Korea, Mitte; Korea, Süden
Australien   | Australien, Osten; Australien, Südosten; Australien, Mitte; Australien, Mitte 2
Azure Government    | „US GOV Virginia“; „US GOV Iowa“; „US GOV Arizona“; „US GOV Texas“; „US DoD, Osten“; „US DoD, Mitte“
Deutschland | „Deutschland, Mitte“; „Deutschland, Nordosten“
China | China, Osten; China, Norden; China, Norden 2; China, Osten 2

>[!NOTE]
>
> - Für die Region **Brasilien, Süden** können Sie eine Replikation und ein Failover auf eine der folgenden Regionen durchführen: USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2 und USA, Norden-Mitte. Es sei darauf hingewiesen, dass in Site Recovery „Brasilien, Süden“ nur als Quellregion ausgewählt werden kann, aus der VMs geschützt werden können. Die Region **kann nicht als Zielregion für die Notfallwiederherstellung** anderer Azure-Regionen wie „USA, Süden-Mitte“ verwendet werden. Da aufgrund der geografischen Entfernung eine Latenz auftritt, wird empfohlen, eine andere amerikanische Region als „Brasilien, Süden“ auszuwählen.
> 
> - Wenn Sie **eine Region nicht sehen können**, in der Sie **einen Tresor erstellen** möchten, dann stellen Sie sicher, dass Ihr Abonnement in dieser Region Zugriff auf das Erstellen von Ressourcen hat. Beispiel:  Wenn Sie keinen Tresor in „Frankreich, Süden“ erstellen können, hat Ihr Abonnement keinen Zugriff auf die Region „Frankreich, Süden“. Bitte erstellen Sie ein Supportticket unter dem Problemtyp „Abonnementverwaltung“ und dem Problemtyp „andere allgemeine Fragen“, Betreff „Abonnement für die Azure-Region XXX auf Whitlelist setzen“.
> 
> - Wenn Sie **während der Aktivierung der Replikation** **keine Region innerhalb eines geografischen Clusters sehen können**, stellen Sie sicher, dass Ihr Abonnement über Zugriff verfügt, um virtuelle Computer in dieser Region zu erstellen. Beispiel:  Wenn Sie versuchen, virtuelle Computer aus „Frankreich, Mitte“ in „Frankreich, Süden“ zu schützen, und „Frankreich, Süden“ nicht in der Regionendropdownliste angezeigt wird, hat Ihr Abonnement in dieser Region keinen Zugriff zum Bereitstellen virtueller Computer. Bitte erstellen Sie ein Supportticket unter dem Problemtyp „Abonnementverwaltung“ und dem Problemtyp „andere allgemeine Fragen“, Betreff „Abonnement für die Azure-Region XXX auf Whitlelist setzen“.
> - Sie können Regionen nicht oben genannte geografische Cluster übergreifend auswählen.


## <a name="cache-storage"></a>Cachespeicher

In dieser Tabelle ist die Unterstützung für das Cachespeicherkonto zusammengefasst, das von Site Recovery während der Replikation verwendet wird.

**Einstellung** | **Unterstützung** | **Details**
--- | --- | ---
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Nicht unterstützt. | Die Einschränkung ist für den Cachespeicher vorhanden, da die Transaktionskosten für V2 erheblich höher als für V1-Speicherkonten sind.
Azure Storage-Firewalls für virtuelle Netzwerke  | Unterstützt | Wenn Sie ein Cache- oder Zielspeicherkonto mit aktivierter Firewall verwenden, müssen Sie [vertrauenswürdigen Microsoft-Diensten den Zugriff erlauben](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions), indem Sie die entsprechende Option auswählen.


## <a name="replicated-machine-operating-systems"></a>Replizierte Computerbetriebssysteme

Site Recovery unterstützt die Replikation von Azure-VMs, auf denen die in diesem Abschnitt angegebenen Betriebssysteme ausgeführt werden.

### <a name="windows"></a>Windows

**Betriebssystem** | **Details**
--- | ---
Windows Server 2016  | Server Core, Server mit Desktopdarstellung
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | Mit SP1 oder höher

#### <a name="linux"></a>Linux

**Betriebssystem** | **Details**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6  
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6
Ubuntu 14.04 LTS Server | [Unterstützte Kernel-Versionen](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [Unterstützte Kernel-Version](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Bei Ubuntu-Servern mit kennwortbasierter Authentifizierung und Anmeldung und mit dem Paket „cloud-init“ zum Konfigurieren von Cloud-VMs kann bei einem Failover die kennwortbasierte Anmeldung deaktiviert sein (je nach cloud-init-Konfiguration). Die kennwortbasierte Anmeldung kann auf der VM wieder aktiviert werden, indem (für die VM, für die das Failover ausgeführt wurde) das Kennwort im Azure-Portal unter „Support“ > „Problembehandlung“ im Menü „Einstellungen“ zurückgesetzt wird.
Debian 7 | [Unterstützte Kernel-Versionen](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Unterstützte Kernel-Versionen](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3. [(Unterstützte Kernel-Versionen)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Ein Upgrade von replizierenden Computern von SP3 auf SP4 wird nicht unterstützt. Wenn ein replizierter Computer aktualisiert wurde, müssen Sie die Replikation deaktivieren und nach dem Upgrade dann wieder aktivieren.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 <br/><br/> Ausführung des Red Hat-kompatiblen Kernels oder von UEK3 (Unbreakable Enterprise Kernel Release 3).


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Ubuntu-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.21 | 3.13.0-24-generic bis 3.13.0-163-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-140-generic,<br/>4.15.0-1023-azure bis 4.15.0-1035-azure |
14.04 LTS | 9.20 | 3.13.0-24-generic bis 3.13.0-161-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-138-generic,<br/>4.15.0-1023-azure bis 4.15.0-1030-azure |
14.04 LTS | 9.19 | 3.13.0-24-generic bis 3.13.0-153-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-131-generic, |
14.04 LTS | 9.18 | 3.13.0-24-generic bis 3.13.0-151-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-128-generic |
|||
16.04 LTS | 9.21 | 4.4.0-21-generic bis 4.4.0-140-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-42-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1035-azure|
16.04 LTS | 9.20 | 4.4.0-21-generic bis 4.4.0-138-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-38-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1030-azure|
16.04 LTS | 9.19 | 4.4.0-21-generic bis 4.4.0-131-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-30-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-generic bis 4.4.0-128-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure |


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Debian-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
Debian 7 | 9.18, 9.19, 9.20, 9.21 | 3.2.0-4-amd64 bis 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.20, 9.21 | 3.16.0-4-amd64 bis 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.8-amd64 |
Debian 8 | 9.19 | 3.16.0-4-amd64 bis 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.18 | 3.16.0-4-amd64 bis 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.6-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Unterstützte SUSE Linux Enterprise Server 12-Kernelversionen für Azure-VMs

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.21 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default bis 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.20 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default bis 4.4.162-94.69-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.19 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default bis 4.4.140-94.42-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.18 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default bis 4.4.138-94.39-default |


## <a name="replicated-machines---linux-file-systemguest-storage"></a>Replizierte Computer – Linux-Dateisystem/Gastspeicher

* Dateisysteme: ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS, BTRFS
* Volume-Manager: LVM2
* Multipfadsoftware: Gerätezuordnung


## <a name="replicated-machines---compute-settings"></a>Replizierte Computer – Compute-Einstellungen

**Einstellung** | **Unterstützung** | **Details**
--- | --- | ---
Größe | Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM | Überprüfen Sie die [Größen der virtuellen Azure-Computer](../virtual-machines/windows/sizes.md).
Verfügbarkeitsgruppen | Unterstützt | Wenn Sie die Replikation für eine Azure-VM mit den Standardoptionen aktivieren, wird basierend auf den Einstellungen der Quellregion automatisch eine Verfügbarkeitsgruppe erstellt. Sie können diese Einstellungen ändern.
Verfügbarkeitszonen | Unterstützt |
Hybridnutzungsvorteil (Hybrid Use Benefit, HUB) | Unterstützt | Wenn für den virtuellen Quellcomputer eine HUB-Lizenz aktiviert wurde, verwendet auch die Testfailover- oder Failover-VM die HUB-Lizenz.
VM-Skalierungsgruppen | Nicht unterstützt |
Azure-Katalogimages – von Microsoft veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Azure-Katalogimages – von Drittanbietern veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Benutzerdefinierte Images – von Drittanbietern veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Mit Site Recovery migrierte virtuelle Computer | Unterstützt | Wenn eine VMware-VM oder ein physischer Computer mit Site Recovery zu Azure migriert wurde, müssen Sie die ältere Version des Mobility Service deinstallieren, die auf dem Computer ausgeführt wird, und den Computer neu starten, bevor er in einer anderen Azure-Region repliziert werden kann.

## <a name="replicated-machines---disk-actions"></a>Replizierte Computer – Datenträgeraktionen

**Aktion** | **Details**
-- | ---
Größe des Datenträgers auf einer replizierten VM ändern | Unterstützt
Hinzufügen eines Datenträgers zu einem replizierten virtuellen Computer | Nicht unterstützt.<br/><br/> Sie müssen die Replikation für die VM deaktivieren, den Datenträger hinzufügen und die Replikation dann erneut aktivieren.

## <a name="replicated-machines---storage"></a>Replizierte Computer – Speicher

In dieser Tabelle ist die Unterstützung für den Betriebssystemdatenträger, Datenträger und temporären Datenträger der Azure-VM zusammengefasst.

- Es ist wichtig, dass die VM-Datenträgergrenzwerte und -Ziele für [Linux](../virtual-machines/linux/disk-scalability-targets.md)- und [Windows](../virtual-machines/windows/disk-scalability-targets.md)-VMs eingehalten werden, um Leistungsprobleme zu vermeiden.
- Wenn Sie für die Bereitstellung die Standardeinstellungen verwenden, erstellt Site Recovery Datenträger und Speicherkonten automatisch basierend auf den Einstellungen für die Datenquelle.
- Achten Sie darauf, dass Sie die Richtlinien befolgen, wenn Sie Anpassungen vornehmen möchten.

**Komponente** | **Unterstützung** | **Details**
--- | --- | ---
Maximale Größe des Betriebssystemdatenträgers | 2.048 GB | [Erfahren Sie mehr](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms) zu VM-Datenträgern.
Temporärer Datenträger | Nicht unterstützt | Der temporäre Datenträger ist immer von der Replikation ausgeschlossen.<br/><br/> Speichern Sie auf dem temporären Datenträger keine persistenten Daten. [Weitere Informationen](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk)
Maximale Größe des Datenträgers | 4095 GB |
Maximale Anzahl von Datenträgern | Bis zu 64, gemäß der Unterstützung für eine bestimmte Azure-VM-Größe | [Erfahren Sie mehr](../virtual-machines/windows/sizes.md) zu VM-Größen.
Änderungsrate für Datenträger | Maximal 10 MBit/s pro Datenträger für Storage Premium. Maximal 2 MBit/s pro Datenträger für Standardspeicher. | Wenn die durchschnittliche Datenänderungsrate auf dem Datenträger dauerhaft über dem Maximalwert liegt, kann dies durch die Replikation nicht aufgeholt werden.<br/><br/>  Falls der Maximalwert aber nur sporadisch überschritten wird, kann die Replikation aufholen, aber es kommt ggf. zu einer leichten Verzögerung bei den Wiederherstellungspunkten.
Datenträger – Standard-Speicherkonto | Unterstützt |
Datenträger – Storage Premium-Konto | Unterstützt | Wenn ein virtueller Computer Datenträger in Premium- und Standard-Speicherkonten aufweist, können Sie für jeden Datenträger ein eigenes Zielspeicherkonto auswählen, um sicherzustellen, dass die gleiche Speicherkonfiguration in der Zielregion vorhanden ist.
Verwalteter Datenträger – Standard | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |
Verwalteter Datenträger – Premium | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |
SSD Standard | Nicht unterstützt |
Redundanz | LRS und GRS werden unterstützt.<br/><br/> ZRS wird nicht unterstützt.
Kalter und heißer Speicher | Nicht unterstützt | VM-Datenträger werden für kalten und heißen Speicher nicht unterstützt
Speicherplätze | Unterstützt |
Verschlüsselung ruhender Daten (SSE) | Unterstützt | SSE ist die Standardeinstellung für Speicherkonten.   
Azure Disk Encryption (ADE) für Windows | VMs, die für die [Verschlüsselung mit der Azure AD-App aktiviert sind](https://aka.ms/ade-aad-app), werden unterstützt. |
Azure Disk Encryption (ADE) für Linux | Nicht unterstützt |
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nicht unterstützt | Wenn Sie Datenträger auf dem virtuellen Computer hinzufügen oder entfernen, müssen Sie die Replikation deaktivieren und dann für den virtuellen Computer wieder aktivieren.
Ausschließen von Datenträgern | Nicht unterstützt|   Temporäre Datenträger sind standardmäßig ausgeschlossen.
Speicherplätze direkt  | Nicht unterstützt|
Dateiserver mit horizontaler Skalierung  | Nicht unterstützt|
LRS | Unterstützt |
GRS | Unterstützt |
RA-GRS | Unterstützt |
ZRS | Nicht unterstützt |
Kalter und heißer Speicher | Nicht unterstützt | Datenträger für virtuelle Computer werden auf kaltem und heißem Speicher nicht unterstützt.
Azure Storage-Firewalls für virtuelle Netzwerke  | Unterstützt | Wenn Sie den Zugriff auf virtuelle Netzwerke in Speicherkonten einschränken, stellen Sie sicher, dass den [vertrauenswürdigen Microsoft-Diensten Zugriff gewährt wird](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Nein  | Die Transaktionskosten steigen gegenüber allgemeinen V1-Speicherkonten erheblich an.

>[!IMPORTANT]
> Stellen Sie sicher, dass Sie die Skalierbarkeits- und Leistungsziele für VM-Datenträger für virtuelle [Linux](../virtual-machines/linux/disk-scalability-targets.md)- oder [Windows](../virtual-machines/windows/disk-scalability-targets.md)-Computer beachten, um Leistungsprobleme zu vermeiden. Wenn Sie die Standardeinstellungen übernehmen, erstellt Site Recovery die erforderlichen Datenträger und Speicherkonten auf Basis der Quellkonfiguration. Wenn Sie Ihre eigenen Einstellungen anpassen und verwenden möchten, achten Sie darauf, die Skalierbarkeits- und Leistungsziele für Datenträger für Ihre virtuellen Quellcomputer einzuhalten.

## <a name="azure-site-recovery-limits-to-replicate-data-change-rates"></a>Azure Site Recovery-Grenzwerte zur Replikation von Datenänderungsraten
Die folgende Tabelle enthält die Azure Site Recovery-Grenzwerte. Diese Grenzwerte basieren auf unseren Tests, können aber nicht alle möglichen E/A-Kombinationen für Anwendungen abdecken. Die tatsächlichen Ergebnisse können je nach Ihrer E/A-Mischung für die Anwendungen variieren. Außerdem sollte erwähnt werden, dass es zwei Grenzwerte zu beachten gilt, nämlich jene für Datenänderungen bei Datenträgern und jene für Datenänderungen bei virtuellen Computern.
Bei dem Premium P20-Datenträger beispielsweise in der folgenden Tabelle können Sie sehen, dass Site Recovery Datenänderungen pro Datenträger von 5 MB/s mit maximal fünf solcher Datenträger pro VM verarbeiten kann. Denn der Grenzwert für die Gesamtänderungsrate liegt bei 25 MB/s pro VM.

**Replikationsspeicherziel** | **Durchschnittliche E/A-Größe des Quelldatenträgers** |**Durchschnittliche Datenänderungsrate des Quelldatenträgers** | **Gesamte Datenänderungsrate des Quelldatenträgers pro Tag**
---|---|---|---
Standardspeicher | 8 KB | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 8 KB  | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 16 KB | 4 MB/s |  336 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 32 KB oder höher | 8 MB/s | 672 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 8 KB    | 5 MB/s | 421 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 16 KB oder höher |10 MB/s | 842 GB pro Datenträger
## <a name="replicated-machines---networking"></a>Replizierte Computer – Netzwerk
**Konfiguration** | **Unterstützung** | **Details**
--- | --- | ---
NIC | Unterstützte maximale Anzahl für eine bestimmte Azure-VM-Größe | Netzwerkkarten werden erstellt, wenn die VM während des Failovers erstellt wird.<br/><br/> Die Anzahl von Netzwerkkarten auf dem virtuellen Failovercomputer ist abhängig von der Anzahl von Netzwerkkarten, die auf dem virtuellen Quellcomputer vorhanden waren, als die Replikation aktiviert wurde. Falls Sie eine Netzwerkkarte nach dem Aktivieren der Replikation hinzufügen oder entfernen, wirkt sich dies nicht auf die Anzahl von Netzwerkkarten auf der replizierten VM nach dem Failover aus.
Internetlastenausgleich | Unterstützt | Ordnen Sie den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zu.
Interner Lastenausgleich | Unterstützt | Ordnen Sie den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zu.
Öffentliche IP-Adresse | Unterstützt | Ordnen Sie der Netzwerkkarte eine vorhandene öffentliche IP-Adresse zu. Oder erstellen Sie eine öffentliche IP-Adresse, und ordnen Sie diese der Netzwerkkarte zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
NSG auf Netzwerkkarte | Unterstützt | Ordnen Sie die NSG der Netzwerkkarte zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
NSG im Subnetz | Unterstützt | Ordnen Sie die NSG dem Subnetz zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
Reservierte (statische) IP-Adresse | Unterstützt | Wenn die Netzwerkkarte auf dem virtuellen Quellcomputer über eine statische IP-Adresse verfügt und für das Zielsubnetz die gleiche IP-Adresse verfügbar ist, wird sie der VM zugewiesen, für die das Failover ausgeführt wurde.<br/><br/> Falls das Zielsubnetz nicht über die gleiche IP-Adresse verfügt, wird eine der verfügbaren IP-Adressen im Subnetz für den virtuellen Computer reserviert.<br/><br/> Sie können eine feste IP-Adresse und ein Subnetz auch unter **Replizierte Elemente** > **Einstellungen** > **Compute und Netzwerk** > **Netzwerkschnittstellen** angeben.
Dynamische IP-Adresse | Unterstützt | Wenn die Netzwerkkarte auf der Quelle über eine dynamische IP-Adressierung verfügt, ist die Netzwerkkarte auf dem virtuellen Computer, für den das Failover ausgeführt wurde, standardmäßig ebenfalls dynamisch.<br/><br/> Sie können dies in eine feste IP-Adresse ändern, falls es erforderlich ist.
Traffic Manager     | Unterstützt | Sie können Traffic Manager so vorkonfigurieren, dass Datenverkehr in regelmäßigen Abständen an den Endpunkt in der Quellregion und bei einem Failover an den Endpunkt in der Zielregion weitergeleitet wird.
Azure DNS | Unterstützt |
Benutzerdefinierter DNS  | Unterstützt |
Nicht authentifizierter Proxy | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).    
Authentifizierter Proxy | Nicht unterstützt | Wenn der virtuelle Computer einen authentifizierten Proxy für ausgehende Verbindungen verwendet, kann er nicht mit Azure Site Recovery repliziert werden.    
Standort-zu-Standort-VPN mit lokalem Netzwerk (mit oder ohne ExpressRoute)| Unterstützt | Stellen Sie sicher, dass die UDRs und NSGs so konfiguriert sind, dass der Datenverkehr für die Sitewiederherstellung nicht lokal weitergeleitet wird. Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  
VNet-zu-VNet-Verbindung | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  
Dienstendpunkte im virtuellen Netzwerk | Unterstützt | Wenn Sie den Zugriff auf virtuelle Netzwerke in Speicherkonten einschränken, stellen Sie sicher, dass den vertrauenswürdigen Microsoft-Diensten Zugriff auf das Speicherkonto gewährt wird.
Beschleunigter Netzwerkbetrieb | Unterstützt | Auf dem virtuellen Quellcomputer muss der beschleunigte Netzwerkbetrieb aktiviert sein. [Weitere Informationen](azure-vm-disaster-recovery-with-accelerated-networking.md)



## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md).
- Stellen Sie die Notfallwiederherstellung bereit, indem Sie [Azure-VMs replizieren](site-recovery-azure-to-azure.md).

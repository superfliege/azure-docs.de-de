---
title: Neue Funktionen in Azure Site Recovery | Microsoft-Dokumentation
description: Bietet eine Zusammenfassung der neuen Funktionen von Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: raynew
ms.openlocfilehash: 61e66a19b625141c69a9473373d3d5d808e18fde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211116"
---
# <a name="whats-new-in-site-recovery"></a>Neuerungen in Site Recovery

Der [Azure Site Recovery](site-recovery-overview.md)-Dienst wird laufend aktualisiert und verbessert. Um Sie auf dem Laufenden zu halten, bietet Ihnen dieser Artikel Informationen zu den neuesten Releases, neuen Features und neuen Inhalten. Diese Seite wird regelmäßig aktualisiert.

Wenn Sie Vorschläge für Site Recovery-Funktionen haben, freuen wir uns über Ihr [Feedback](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>Q1 2019

### <a name="linux-support"></a>Linux-Unterstützung

[Updaterollup 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) stellt ein Update für Site Recovery-Agents und -Anbieter bereit. Das Update fügt Unterstützung für Linux wie folgt hinzu:

- **Notfallwiederherstellung von Azure-VMs**: Red Hat Workstation 6/7, neue Kernelversionen für Ubuntu, Debian und SUSE.
- **Notfallwiederherstellung von virtuellen VMware-Computern/physischen Servern in Azure**: Red Hat Enterprise Linux 7.6, Red Hat Workstation 6/7, Oracle Linux 6.10/7.6. Neue Kernelversionen für Ubuntu, Debian und SUSE.



## <a name="q4-2018"></a>Q4 2018

## <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Preisrechner für die Notfallwiederherstellung von Azure-VMs

Notfallwiederherstellung für Azure-VMs verursacht VM-Lizenzierungskosten sowie Netzwerk- und Speicherkosten. Azure bietet einen [Preisrechner](https://aka.ms/a2a-cost-estimator), mit dem Sie diese Kosten herausfinden können. Site Recovery bietet nun eine [Beispielpreisschätzung](https://aka.ms/a2a-cost-estimator), die eine Beispielbereitstellung basierend auf einer Drei-Schichten-App mit sechs VMs mit 12 HDD Standard-Datenträgern und 6 SSD Premium-Datenträgern bewertet. Das Beispiel geht von einer Datenänderung von 10 GB pro Tag für Standard und 20 GB für Premium aus. Für Ihre individuelle Bereitstellung können Sie die Variablen ändern, um die Kosten zu schätzen. Sie können die Anzahl der VMs, die Anzahl und den Typ der verwalteten Datenträger und die erwartete Datenänderungs-Gesamtrate angeben, die für die VMs erwartet wird. Darüber hinaus können Sie einen Komprimierungsfaktor anwenden, um die Kosten für Bandbreite zu schätzen. Ankündigung [lesen](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).

### <a name="support-for-azure-vms-in-zones"></a>Unterstützung für virtuelle Azure-Computer in Zonen

Azure-Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Sie können nun Replikation für eine Azure-VM aktivieren und das Ziel für das Failover auf eine einzelne VM-Instanz, eine VM in einer Verfügbarkeitsgruppe oder eine VM in einer Verfügbarkeitszone festlegen. Die Einstellung besitzt keine Auswirkungen auf die Replikation. Ankündigung [lesen](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/).
 
### <a name="disaster-recovery-for-encrypted-vms"></a>Notfallwiederherstellung für verschlüsselte virtuelle Computer

Site Recovery unterstützt Azure-VMs, die mit Azure Disk Encryption (ADE) mit der Azure AD-App verschlüsselt wurden. [Weitere Informationen](azure-to-azure-how-to-enable-replication-ade-vms.md)

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Notfallwiederherstellung für VMs mit beschleunigtem Netzwerkbetrieb

Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) auf einem virtuellen Computer und somit eine Steigerung der Netzwerkleistung. Wenn Sie die Replikation für eine Azure-VM aktivieren, erkennt Site Recovery, ob beschleunigter Netzwerkbetrieb aktiviert ist. Wenn dies der Fall ist, konfiguriert Site Recovery nach dem Failover automatisch beschleunigten Netzwerkbetrieb auf der Azure-Zielreplikat-VM, und zwar sowohl für [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) als auch für [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Weitere Informationen](azure-vm-disaster-recovery-with-accelerated-networking.md)

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Automatische Updates für die Mobility Service-Erweiterung

Site Recovery hat eine Option für automatische Updates der Mobility Service-Erweiterung hinzugefügt. Die Mobility Service-Erweiterung wird auf jeder Azure-VM installiert, die von Site Recovery repliziert wird. Wenn Sie die Replikation aktivieren, wählen Sie aus, ob Site Recovery Updates für die Erweiterung verwalten soll. Updates erfordern keinen Neustart der VM und wirken sich nicht auf die Replikation aus. [Weitere Informationen](azure-to-azure-autoupdate.md)

### <a name="support-for-standard-ssd-disks"></a>Unterstützung für SSD Standard-Datenträger

Azure hat [SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)-Datenträger (Standard Solid State Drives) eingeführt, um eine kostengünstige Speicherlösung für Apps wie Webserver bereitzustellen, die eine konsistente Leistung benötigen, aber keine hohe Datenträger-IOPS aufweisen. Sie kombinieren Elemente von SSD Premium- und HDD Standard-Datenträgern. Site Recovery bietet Notfallwiederherstellung für Azure-VMs, die einen SSD Standard-Datenträger verwenden. Standardmäßig wird der Datenträgertyp nach einem Failover in die Zielregion beibehalten.

### <a name="support-for-azure-storage-firewall"></a>Unterstützung für Azure Storage-Firewall

Sie können Azure Storage-Konten in einer bestimmten Sammlung von Netzwerken schützen, indem Sie Firewallregeln für das Konto aktivieren. Sie konfigurieren Speicherkonten, um den Datenverkehr aus internen Netzwerken und dem Internet standardmäßig zu verweigern, und gewähren dann Zugriff auf den Datenverkehr aus bestimmten VNETs. Site Recovery unterstützt die Replikation für VMs mit nicht verwalteten Datenträgern für firewallaktivierte Speicherkonten in eine sekundäre Region. In der Zielregion können Sie für nicht verwaltete Datenträger Speicherkonten mit aktivierten Firewalls auswählen. Sie können auch den Zugriff auf das Cachespeicherkonto einschränken, indem Sie den Netzwerkzugriff auf das Netzwerk beschränken, in dem sich die Quell-VMs befinden. Beachten Sie, dass Sie Zugriff für vertrauenswürdige Microsoft-Dienste [zulassen](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) müssen.

## <a name="q3-2018"></a>Q3 2018 

### <a name="linux-support"></a>Linux-Unterstützung

#### <a name="update-rollup-28"></a>Updaterollup 28

[Updaterollup 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) stellt ein Update für Site Recovery-Agents und -Anbieter bereit. Das Update fügt Unterstützung für Linux wie folgt hinzu:

- **Notfallwiederherstellung von Azure-VMs**: Red Hat Enterprise Linux 6.10, CentOS 6.10. Linux-basierte VMs, die den Partitionsstil der GUID-Partitionstabelle (GPT) im BIOS-Legacykompatibilitätsmodus verwenden, werden jetzt unterstützt.
- **Notfallwiederherstellung von virtuellen VMware-Computern/physischen Servern in Azure**: Red Hat Enterprise Linux 6.10, CentOS 6.10. Linux-basierte VMs, die den Partitionsstil der GUID-Partitionstabelle (GPT) im BIOS-Legacykompatibilitätsmodus verwenden, werden jetzt unterstützt.

#### <a name="update-rollup-27"></a>Updaterollup 27

[Updaterollup 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) stellt ein Update für Site Recovery-Agents und -Anbieter bereit. Das Update fügt Unterstützung für Linux wie folgt hinzu:

- **Notfallwiederherstellung von Azure-VMs**: Red Hat Enterprise Linux 7.5
- **Notfallwiederherstellung von virtuellen VMware-Computern/physischen Servern in Azure**: SUSE Linux Enterprise Server 12, Red Hat Enterprise Linux 7.5

### <a name="support-for-azure-vms-running-on-windows-server-2016"></a>Unterstützung für Azure-VMs unter Windows Server 2016

Azure-VMs unter Windows Server 2016 können in Azure-Regionen mit Azure Site Recovery repliziert werden.

### <a name="support-for-azure-vms-running-storage-spaces-direct"></a>Unterstützung für Azure-VMs, die direkte Speicherplätze ausführen

[Direkte Speicherplätze](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) (verfügbar ab Windows Server 2016) gruppieren Laufwerke in einem Speicherpool und verwenden dann Kapazität aus dem Pool, um Speicherplätze zu erstellen. Speicherplätze können auf einem eigenständigen virtuellen Computer, einem [Gastcluster von virtuellen Azure-Computern](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) mit lokalem Speicher auf jedem Clusterknoten oder für freigegebenen Speicher im Cluster verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Auf dem neuesten Stand mit unseren Updates auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?product=site-recovery) bleiben.



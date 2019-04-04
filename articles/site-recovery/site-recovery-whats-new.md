---
title: Neue Funktionen in Azure Site Recovery | Microsoft-Dokumentation
description: Bietet eine Zusammenfassung der neuen Funktionen von Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: raynew
ms.openlocfilehash: fece1adc9ad2f5844a7d6cc1e0e9cc92d44de6d8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57892930"
---
# <a name="whats-new-in-site-recovery"></a>Neuerungen in Site Recovery

Der [Azure Site Recovery](site-recovery-overview.md)-Dienst wird laufend aktualisiert und verbessert. Um Sie auf dem Laufenden zu halten, bietet Ihnen dieser Artikel Informationen zu den neuesten Releases, neuen Features und neuen Inhalten. Diese Seite wird regelmäßig aktualisiert.

Wenn Sie Vorschläge für Site Recovery-Funktionen haben, freuen wir uns über Ihr [Feedback](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>Q1 2019 

### <a name="update-rollup-34-february-2019"></a>Updaterollup 34 (Februar 2019)

Mit dem [Updaterollup 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)



### <a name="update-rollup-33-february-2019"></a>Updaterollup 33 (Februar 2019)

Mit dem [Updaterollup 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)
**Netzwerkzuordnung** | Sie können nun ein beliebiges Zielnetzwerk beim Aktivieren der Replikation für die Azure-VM-Notfallwiederherstellung verwenden. 
**SSD Standard** | Sie können nun die Notfallwiederherstellung für Azure-VMs mit [SSD Standard-Datenträgern](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd) einrichten.
**Direkte Speicherplätze** | Sie können nun die Notfallwiederherstellung für Apps auf Azure-VMs mit [direkten Speicherplätzen](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) für Hochverfügbarkeit einrichten.
**BRTFS-Dateisystem** | Wird nun zusätzlich zu Azure-VMs auch für VMware-VMs unterstützt.<br/><br/> Wird nicht unterstützt, wenn: das untergeordnete Volume des BTRFS-Dateisystems nach Aktivierung der Replikation geändert wird, das Dateisystem auf mehrere Datenträger verteilt wird oder das BTRFS-Dateisystem RAID unterstützt.



### <a name="update-rollup-32-january-2019"></a>Updaterollup 32 (Januar 2019)

Mit dem [Updaterollup 31](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)
**Notfallwiederherstellung für Linux** | **Virtuelle Azure-Computer**: Red Hat Workstation 6/7, Unterstützung für neue Kernelversionen für Ubuntu, Debian und SUSE.<br/><br/> **VMware-VMs/physische Server:** Red Hat Enterprise Linux 7.6, Red Hat Workstation 6/7, Oracle Linux 6.10/7.6, Unterstützung für neue Kernelversionen für Ubuntu, Debian und SUSE.


### <a name="update-rollup-31-january-2019"></a>Updaterollup 31 (Januar 2019)

Mit dem [Updaterollup 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)
**Notfallwiederherstellung für Linux** | **Virtuelle Azure-Computer**: Oracle Linux 6.8 und 6.9/7.0, Unterstützung für UEK5-Kernel.<br/><br/> **VMware-VMs/physische Server:** Oracle Linux 6.8 und 6.9/7.0, Unterstützung für UEK5-Kernel.
**BRTFS-Dateisystem** | Unterstützung für Azure-VMs
**LVM** | Unterstützung für LVM- und LVM2-Volumes wurde hinzugefügt.<br/><br/> Das Verzeichnis „/boot“ wird auf Datenträgerpartitionen und LVM-Volumes unterstützt.
**Verzeichnisse** | Die Unterstützung der folgenden Verzeichnisse wurde für separate Partitionen oder Dateisysteme eingerichtet, die sich nicht auf dem gleichen Systemdatenträger befinden: „/(root)“, „/boot“, „/usr“, „/usr“, „/local“, „/var“ usw.
**Windows Server 2008** | Dynamische Datenträger werden nun unterstützt.
**Failover für VMware-VMs** | Verbesserte Failoverzeit für VMware-VMs, bei denen „storvsc“ und „vsbus“ keine Starttreiber sind.
**UEFI-Unterstützung** | Azure-VMs unterstützen den Starttyp UEFI nicht. Sie können lokale physische Server mit UEFI nun mithilfe von Site Recovery zu Azure migrieren. Site Recovery migriert den Server, indem der Starttyp vor der Migration in BIOS konvertiert wird. Site Recovery hat diese Konvertierung zuvor nur für VMs unterstützt. Physische Server mit Windows Server 2012 oder höher werden nun unterstützt.
**Azure-VMs in Verfügbarkeitszonen** | Sie können die Replikation in anderen Regionen für Azure-VMs aktivieren, die in Verfügbarkeitszonen bereitgestellt wurden. Sie können nun Replikation für eine Azure-VM aktivieren und das Ziel für das Failover auf eine einzelne VM-Instanz, eine VM in einer Verfügbarkeitsgruppe oder eine VM in einer Verfügbarkeitszone festlegen. Die Einstellung besitzt keine Auswirkungen auf die Replikation. Ankündigung [lesen](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/).


## <a name="q4-2018"></a>Q4 2018

### <a name="update-rollup-30-october-2018"></a>Updaterollup 30 (Oktober 2018)

Mit dem [Updaterollup 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)
**Unterstützung für Regionen** | Die Site Recovery-Unterstützung wurde für Australien, Mitte und Australien, Mitte 2 hinzugefügt.
**Unterstützung für die Datenträgerverschlüsselung** | Die Unterstützung der Notfallwiederherstellung für Azure-VMs, die mit Azure Disk Encryption (ADE) mit der Azure AD-App verschlüsselt wurden, wurde hinzugefügt. [Weitere Informationen](azure-to-azure-how-to-enable-replication-ade-vms.md)
**Ausschluss von Datenträgern** | Nicht initialisierte Datenträger werden während der Replikation von Azure-VMs automatisch ausgeschlossen.
**Speicher mit aktivierter Firewall** | [Speicherkonten mit aktivierter Firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security) werden nun unterstützt.<br/><br/> Im Falle einer Notfallwiederherstellung können Sie Azure-VMs mit nicht verwalteten Datenträgern in Speicherkonten mit aktivierter Firewall in eine andere Azure-Region replizieren.<br/><br/> Sie können Speicherkonten mit aktivierter Firewall als Zielspeicherkonten für nicht verwaltete Datenträger verwenden.<br/><br/> Dies wird nur mit PowerShell unterstützt.


### <a name="update-rollup-29-october-2018"></a>Updaterollup 29 (Oktober 2018)

Mit dem [Updaterollup 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Problembehebungen** | Mehrere Fehlerbehebungen und Verbesserungen (wie im Rollup beschrieben)

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Automatische Updates für die Mobility Service-Erweiterung

Site Recovery hat eine Option für automatische Updates der Mobility Service-Erweiterung hinzugefügt. Die Mobility Service-Erweiterung wird auf jeder Azure-VM installiert, die von Site Recovery repliziert wird. Wenn Sie die Replikation aktivieren, wählen Sie aus, ob Site Recovery Updates für die Erweiterung verwalten soll. Updates erfordern keinen Neustart der VM und wirken sich nicht auf die Replikation aus. [Weitere Informationen](azure-to-azure-autoupdate.md)

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Notfallwiederherstellung für VMs mit beschleunigtem Netzwerkbetrieb

Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) auf einem virtuellen Computer und somit eine Steigerung der Netzwerkleistung. Wenn Sie die Replikation für eine Azure-VM aktivieren, erkennt Site Recovery, ob beschleunigter Netzwerkbetrieb aktiviert ist. Wenn dies der Fall ist, konfiguriert Site Recovery nach dem Failover automatisch beschleunigten Netzwerkbetrieb auf der Azure-Zielreplikat-VM, und zwar sowohl für [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) als auch für [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Weitere Informationen](azure-vm-disaster-recovery-with-accelerated-networking.md)

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Preisrechner für die Notfallwiederherstellung von Azure-VMs

Notfallwiederherstellung für Azure-VMs verursacht VM-Lizenzierungskosten sowie Netzwerk- und Speicherkosten. Azure bietet einen [Preisrechner](https://aka.ms/a2a-cost-estimator), mit dem Sie diese Kosten herausfinden können. Site Recovery bietet nun eine [Beispielpreisschätzung](https://aka.ms/a2a-cost-estimator), die eine Beispielbereitstellung basierend auf einer Drei-Schichten-App mit sechs VMs mit 12 HDD Standard-Datenträgern und 6 SSD Premium-Datenträgern bewertet. Das Beispiel geht von einer Datenänderung von 10 GB pro Tag für Standard und 20 GB für Premium aus. Für Ihre individuelle Bereitstellung können Sie die Variablen ändern, um die Kosten zu schätzen. Sie können die Anzahl der VMs, die Anzahl und den Typ der verwalteten Datenträger und die erwartete Datenänderungs-Gesamtrate angeben, die für die VMs erwartet wird. Darüber hinaus können Sie einen Komprimierungsfaktor anwenden, um die Kosten für Bandbreite zu schätzen. Ankündigung [lesen](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).



## <a name="q3-2018"></a>Q3 2018 


### <a name="update-rollup-28-august-2018"></a>Updaterollup 28 (August 2018)

Mit dem [Updaterollup 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Notfallwiederherstellung für Linux** | **Virtuelle Azure-Computer**: Die Unterstützung für Red Hat Enterprise Linux 6.10 und CentOS 6.10 wurde hinzugefügt.<br/><br/> **VMware-VMs:** Red Hat Enterprise Linux 6.10 und CentOS 6.10.<br/><br/> Linux-basierte VMs, die den Partitionsstil der GUID-Partitionstabelle (GPT) im BIOS-Legacykompatibilitätsmodus verwenden, werden jetzt unterstützt.
**Unterstützung für Clouds** | Die Notfallwiederherstellung für Azure-VMs in der Azure Deutschland-Cloud wird nun unterstützt.
**Abonnementübergreifende Notfallwiederherstellung** | Die Replikation von Azure-VMs von einer Region zu einer anderen Region in einem anderen Abonnement innerhalb desselben Azure Active Directory-Mandanten wird nun unterstützt. [Weitere Informationen](https://aka.ms/cross-sub-blog)
**Windows Server 2008** | Die Migration von Computern mit Windows Server 2008 R2/2008 64-Bit und 32-Bit wird nun unterstützt.<br/><br/> Es wird lediglich die Migration unterstützt (Replikation und Failover). Failbacks werden nicht unterstützt.

### <a name="update-rollup-27-july-2018"></a>Updaterollup 27 (Juli 2018)

Mit dem [Updaterollup 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) werden die folgenden Updates bereitgestellt.

**Aktualisieren** | **Details**
--- | ---
**Anbieter und Agents** | Ein Update für Site Recovery-Agents und -Anbieter (wie im Rollup beschrieben)
**Notfallwiederherstellung für Linux** | **Virtuelle Azure-Computer**: Red Hat Enterprise Linux 7.5<br/><br/> **VMware-VMs/physische Server:** Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12




## <a name="next-steps"></a>Nächste Schritte

Auf dem neuesten Stand mit unseren Updates auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?product=site-recovery) bleiben.




 










---
title: "Vorbereiten lokaler Hyper-V-Server für die Notfallwiederherstellung von Hyper-V-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie lokale Hyper-V-VMs, die nicht von System Center VMM verwaltet werden, für die Notfallwiederherstellung mit dem Azure Site Recovery-Dienst vorbereiten."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6e2837341e16f5077cfff18d4a34c097c165ef09
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Vorbereiten lokaler Hyper-V-Server für die Notfallwiederherstellung in Azure

In diesem Tutorial wird gezeigt, wie Sie Ihre lokale Hyper-V-Infrastruktur vorbereiten, wenn Sie Hyper-V-VMs für die Notfallwiederherstellung in Azure replizieren möchten. Hyper-V-Hosts können von System Center Virtual Machine Manager (VMM) verwaltet werden, dies ist jedoch nicht erforderlich.  In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Hyper-V-Anforderungen und der Anforderungen für VMM, falls erforderlich
> * Vorbereiten von VMM, falls erforderlich
> * Überprüfen des Internetzugriffs auf Azure-Standorte
> * Vorbereiten von VMs, damit nach einem Failover in Azure auf sie zugegriffen werden kann

Dies ist das zweite Tutorial in der Reihe. Stellen Sie sicher, dass Sie die [Azure-Komponenten eingerichtet haben](tutorial-prepare-azure.md). Die Vorgehensweise dazu wird im vorhergehenden Tutorial beschrieben.



## <a name="review-server-requirements"></a>Überprüfen der Serveranforderungen

Vergewissern Sie sich, dass die Hyper-V-Hosts die folgenden Anforderungen erfüllen. Wenn Sie Hosts in System Center Virtual Machine Manager-Clouds (VMM) verwalten, überprüfen Sie die VMM-Anforderungen.


**Komponente** | **Mit VMM verwaltetes Hyper-V** | **Hyper-V ohne VMM**
--- | --- | ---
**Hyper-V-Hostbetriebssystem** | Windows Server 2016, 2012 R2 | Nicht verfügbar
**VMM** | VMM 2012, VMM 2012 R2 | Nicht verfügbar


## <a name="review-hyper-v-vm-requirements"></a>Überprüfen der Hyper-V-VM-Anforderungen

Vergewissern Sie sich, dass der virtuelle Computer die in der Tabelle zusammengefassten Anforderungen erfüllt.

**VM-Anforderung** | **Details**
--- | ---
**Gastbetriebssystem** | Alle [von Azure unterstützten](https://technet.microsoft.com/library/cc794868.aspx) Gastbetriebssysteme.
**Anforderungen für Azure** | Lokale Hyper-V-VMs müssen die Azure-VM-Anforderungen erfüllen (site-recovery-support-matrix-to-azure.md).

## <a name="prepare-vmm-optional"></a>Vorbereiten von VMM (optional)

Wenn Hyper-V-Hosts von VMM verwaltet werden, müssen Sie den lokalen VMM-Server vorbereiten. 

- Stellen Sie sicher, dass der VMM-Server über mindestens eine Cloud mit mindestens einer Hostgruppe verfügt. Der Hyper-V-Host, auf dem virtuelle Computer ausgeführt werden, sollte sich in der Cloud befinden.
- Bereiten Sie den VMM-Server für die Netzwerkzuordnung vor.

### <a name="prepare-vmm-for-network-mapping"></a>Vorbereiten von VMM für die Netzwerkzuordnung

Bei Verwendung von VMM gilt die [Netzwerkzuordnung](site-recovery-network-mapping.md) zwischen lokalen VMM-VM-Netzwerken und virtuellen Azure-Netzwerken. Die Zuordnung stellt sicher, dass Azure-VMs mit dem richtigen Netzwerk verbunden sind, wenn sie nach einem Failover erstellt werden.

Bereiten Sie VMM wie folgt für die Netzwerkzuordnung vor:

1. Bereiten Sie ein [logisches VMM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-logical) vor, das der Cloud zugeordnet ist, in der sich die Hyper-V-Hosts befinden.
2. Stellen Sie sicher, dass Sie über ein [VM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-virtual) verfügen, das mit dem logischen Netzwerk verknüpft ist.
3. Verbinden Sie in VMM die virtuellen Computer mit dem VM-Netzwerk.

## <a name="verify-internet-access"></a>Überprüfen des Internetzugriffs

1. Im Rahmen des Tutorials besteht die einfachste Konfiguration darin, dass die Hyper-V-Hosts und ggf. die VMM-Server direkten Zugriff auf das Internet haben, ohne einen Proxy zu verwenden. 
2. Stellen Sie sicher, dass Hyper-V-Hosts und ggf. der VMM-Server auf diese URLs zugreifen können: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Stellen Sie Folgendes sicher:
    - Bei Verwendung von auf IP-Adressen basierenden Firewallregeln muss die Kommunikation mit Azure möglich sein.
    - Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

In einem Failoverszenario sollten Sie eine Verbindung mit Ihrem replizierten lokalen Netzwerk herstellen können.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Windows-VMs nach dem Failover per RDP herstellen möchten:

1. Aktivieren Sie vor dem Failover RDP auf dem lokalen virtuellen Computer, um über das Internet darauf zuzugreifen. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über ein Site-to-Site-VPN aktivieren Sie RDP auf dem lokalen Computer. RDP sollte unter **Windows-Firewall** -> **Zugelassene Apps und Feature** für **private und Domänennetzwerke** zugelassen werden.
   Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135) Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls können Sie sich nach Abschluss des Updates nicht mehr auf dem virtuellen Computer anmelden.
3. Aktivieren Sie nach dem Failover auf der Windows-Azure-VM die **Startdiagnose**, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs](tutorial-hyper-v-to-azure.md)
> [Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs in VMM-Clouds](tutorial-hyper-v-vmm-to-azure.md)

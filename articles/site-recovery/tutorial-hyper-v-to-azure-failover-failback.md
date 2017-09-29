---
title: Failover und Failback von Hyper-V-VMs, die mit Site Recovery in Azure repliziert werden | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie ein Failover für Hyper-V-VMs zu Azure und ein Failback zum lokalen Standort mit Azure Site Recovery durchführen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72065d01-ed0f-430e-b117-a5885cecd1db
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/15/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e1cc21661450a983c25b24fe2a6228e26ceecec6
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Failover und Failback von Hyper-V-VMs, die in Azure repliziert werden

Der [Azure Site Recovery](site-recovery-overview.md)-Dienst verwaltet und koordiniert Replikation, Failover und Failback von lokalen Computern sowie virtuellen Azure-Computern (VMs).

In diesem Tutorial wird das Ausführen eines Failovers einer Hyper-V-VM zu Azure beschrieben. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Failover von lokalen Hyper-V-VMs zu Azure
> * Failback von Azure zu lokalen Standorten und erneutes Starten der Replikation in Azure

## <a name="overview"></a>Übersicht
Failover und Failback weisen zwei Phasen auf:

1. **Failover zu Azure:** Failover von Computern vom lokalen Standort zu Azure
2. **Failback von Azure zum lokalen Standort:** Ausführen eines geplanten Failovers von Azure zum lokalen Standort. Nach dem geplanten Failover aktivieren Sie die umgekehrte Replikation, um die Replikation vom lokalen Standort in Azure erneut zu starten. 


## <a name="fail-over-to-azure"></a>Failover in Azure

### <a name="failover-prerequisites"></a>Failovervoraussetzungen

So führen Sie das Failover durch

- Führen Sie unbedingt eine [Notfallwiederherstellungs-Übung](tutorial-dr-drill-azure.md) durch, um zu überprüfen, ob alles wie erwartet funktioniert.
- Bereiten Sie vor dem Failover optional eine Verbindung mit Azure-VMs vor.
- Überprüfen Sie die VM-Eigenschaften, bevor Sie das Failover ausführen.

#### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten:

##### <a name="azure-vms-running-windows"></a>Azure-VMs unter Windows

1. Aktivieren Sie vor dem Failover RDP auf dem lokalen virtuellen Computer, um über das Internet auf Azure-VMs zuzugreifen. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über ein Site-to-Site-VPN aktivieren Sie RDP auf dem lokalen Computer. RDP sollte unter **Windows-Firewall** -> **Zugelassene Apps und Feature** für **private und Domänennetzwerke** zugelassen werden. Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135) Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls können Sie sich nach Abschluss des Updates nicht mehr auf dem virtuellen Computer anmelden. 
3. Aktivieren Sie nach dem Failover auf der Windows-Azure-VM die **Startdiagnose**, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.


##### <a name="azure-vms-running-linux"></a>Azure-VMs unter Linux

1. Überprüfen Sie vor dem Failover auf dem lokalen Computer, ob der Secure Shell-Dienst für den automatischen Start beim Systemstart aktiviert ist. Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.
2. Erlauben Sie nach dem Failover auf der Azure-VM in den Netzwerksicherheitsgruppen-Regeln eingehende Verbindungen am SSH-Port und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist.  Fügen Sie der VM eine [öffentliche IP-Adresse](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) hinzu. Sie können die **Startdiagnose** aktivieren, um einen Screenshot des virtuellen Computers anzuzeigen.


#### <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle Computer die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > VM.
2. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
3. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, Zielgröße, [Verfügbarkeitsgruppe](../virtual-machines/windows/tutorial-availability-sets.md) und [Einstellungen verwalteter Datenträger](#managed-disk-considerations) ändern.
4. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.
5. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.


### <a name="run-a-failover-from-on-premises-to-azure"></a>Ausführen eines Failovers vom lokalen Standort nach Azure

Sie können ein reguläres oder geplantes Failover für virtuelle Hyper-V-Computer ausführen.

- Verwenden Sie ein reguläres Failover für unerwartete Ausfälle. Beim Ausführen dieses Failovers erstellt Site Recovery eine Azure-VM und fährt sie hoch. Sie führen das Failover für einen bestimmten Wiederherstellungspunkt aus. Je nachdem, welchen Wiederherstellungspunkt Sie verwenden, können Datenverluste auftreten.
- Ein geplantes Failover kann zu Wartungszwecken oder bei erwarteten Ausfällen verwendet werden. Mit dieser Option vermeiden Sie jeglichen Datenverlust. Wenn ein geplantes Failover ausgelöst wird, werden die Quell-VMs heruntergefahren. Nicht synchronisierte Daten werden synchronisiert, und das Failover wird ausgelöst.

In diesem Verfahren erfahren Sie, wie Sie ein reguläres Failover durchführen.


1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Sie können eine der folgenden Optionen auswählen:
    - **Neueste** (Standard): Mit dieser Option werden zuerst alle an Site Recovery gesendeten Daten verarbeitet. Sie bietet die niedrigste RPO (Recovery Point Objective), da die nach dem Failover erstellte Azure-VM über alle Daten verfügt, die bei Auslösung des Failovers zu Site Recovery repliziert wurden.
    - **Letzte Verarbeitung:** Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.
    - **Letzte App-Konsistenz:** Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten App-konsistenten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. 
    - **Benutzerdefiniert:** Geben Sie einen Wiederherstellungspunkt an.
3. Wenn Sie ein Failover von Hyper-V-VMs in System Center-VMM-Clouds zu Azure durchführen und die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen des Anbietersetups auf dem VMM-Server ausgestellt wurde.
4. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Site Recovery versucht vor dem Auslösen des Failovers auch, lokale Daten zu synchronisieren, die noch nicht an Azure gesendet wurden. Beachten Sie, dass das Failover auch dann fortgesetzt wird, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
5. Wenn Sie das Herstellen einer Verbindung mit der Azure-VM vorbereitet haben, stellen Sie eine Verbindung her, um sie nach dem Failover zu überprüfen.
6. **Committen** Sie das Failover nach der Überprüfung. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.  


## <a name="fail-back-from-azure-to-on-premises"></a>Failback von Azure zum lokalen Standort

### <a name="prerequisites-for-failback"></a>Voraussetzungen für ein Failback

Um das Failback abzuschließen, stellen Sie sicher, dass die primären VMM-/Hyper-V-Server am Standort mit Site Recovery verbunden sind.


### <a name="run-a-failback-and-reprotect-on-premises-vms"></a>Durchführen eines Failbacks und Schützen von lokalen virtuellen Computern

Führen Sie ein Failover von Azure zum lokalen Standort durch, und starten Sie die Replikation von virtuellen Computern vom lokalen Standort in Azure.

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Geplantes Failover**.
2. Überprüfen Sie in **Geplantes Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie Quell- und Zielspeicherort aus. 
3. Geben Sie in **Datensynchronisierung** an, wie Sie synchronisieren möchten:
    - **Daten vor dem Failover synchronisieren (nur Deltaänderungen synchronisieren):** Diese Option minimiert die VM-Ausfallzeit, da die Synchronisierung ohne Herunterfahren der VM ausgeführt wird. Dies funktioniert folgendermaßen:
        1. Eine Momentaufnahme der Azure-VM wird erstellt, und sie wird in den lokalen Hyper-V-Host kopiert. Der virtuelle Computer wird in Azure weiter ausgeführt.
        2. Die Azure-VM wird heruntergefahren, sodass dort keine neuen Änderungen auftreten. Die letzten Deltaänderungen werden an den lokalen Server übertragen, und der lokale virtuelle Computer wird gestartet.
    - **Daten nur während Failover synchronisieren (vollständiger Download):** Verwenden Sie diese Option, wenn Sie über einen längeren Zeitraum Azure verwendet haben. Diese Option ist schneller, da mehrere Datenträgeränderungen erwartet werden, und keine Zeit für die Berechnung von Prüfsummen aufgewendet wird. Diese Option führt einen Datenträgerdownload durch. Sie ist auch nützlich, wenn der lokale virtuelle Computer gelöscht wurde.
4. Wenn Sie ein Failover von Hyper-V-VMs in System Center-VMM-Clouds zu Azure durchführen und die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen des Anbietersetups auf dem VMM-Server ausgestellt wurde.
5. Initiieren Sie das Failover. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
6. Wenn Sie das Synchronisieren der Daten vor dem Failover aktiviert haben, klicken Sie auf **Aufträge** > Auftragsname des geplanten Failovers > **Failover abschließen**, sobald die erste Datensynchronisierung abgeschlossen ist und Sie zum Herunterfahren der Azure-VMs bereit sind. Dadurch wird die Azure-VM heruntergefahren, die neuesten Änderungen werden an den lokalen Standort übertragen, und der lokale virtuelle Computer wird gestartet.
7. Melden Sie sich auf dem lokalen virtuellen Computer an, um zu überprüfen, ob er wie erwartet verfügbar ist.
8. Der lokale virtuelle Computer befindet sich nun im Status „Commit ausstehend“. Klicken Sie auf **Commit ausführen**, um das Failover zu committen. Dies löscht die Azure-VMs und die zugehörigen Datenträger und bereitet den lokalen virtuellen Computer auf die umgekehrte Replikation vor.
9. Aktivieren Sie zum Starten der Replikation des lokalen virtuellen Computers in Azure **Umgekehrt replizieren**.


> [!NOTE]
> Die umgekehrte Replikation repliziert nur Änderungen, die seit dem Ausschalten der Azure-VM aufgetreten sind, und es werden nur Deltaänderungen gesendet.



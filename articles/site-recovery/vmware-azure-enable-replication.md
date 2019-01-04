---
title: Aktivieren der Replikation von virtuellen VMware-Computern für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von Azure Site Recovery die Replikation von virtuellen VMware-Computern für die VMware-Notfallwiederherstellung in Azure aktivieren.
author: asgang
ms.service: site-recovery
ms.date: 11/27/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 51470e9f8e0bffe18d1dc4007433246d084a5cb2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846657"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Aktivieren der Replikation in Azure für VMware-VMs


Dieser Artikel beschreibt die Aktivierung der Replikation von lokalen VMware-VMs in Azure.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie Folgendes abgeschlossen haben:

1.  [Einrichten der lokalen Quellumgebung](vmware-azure-set-up-source.md)
2.  [Einrichten der Zielumgebung in Azure](vmware-azure-set-up-target.md)


## <a name="before-you-start"></a>Vorbereitung
Wenn Sie VMware-VMs replizieren, beachten Sie Folgendes:

* Ihr Azure-Benutzerkonto benötigt bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines neuen virtuellen Computers in Azure.
* VMware-VMs werden alle 15 Minuten ermittelt. Es kann 15 Minuten oder länger dauern, bis sie nach der Ermittlung im Azure-Portal angezeigt werden. Ebenso kann die Ermittlung 15 Minuten oder länger dauern, wenn Sie einen neuen vCenter-Server oder vSphere-Host hinzufügen.
* Es kann auch 15 Minuten oder länger dauern, bis Umgebungsänderungen auf dem virtuellen Computer (z.B. die Installation von VMware-Tools) im Portal aktualisiert werden.
* Sie können den Zeitpunkt der letzten Ermittlung für die VMware-VMs auf der Seite **Konfigurationsserver** im Feld **Letzter Kontakt um** für den vCenter-Server oder vSphere-Host überprüfen.
* Wenn Sie Computer für die Replikation hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken), und klicken Sie auf die Schaltfläche **Refresh** (Aktualisieren).
* Wenn Sie die Replikation aktivieren, installiert der Prozessserver den Mobility Service automatisch auf dem Computer, falls dieser vorbereitet ist.


## <a name="enable-replication"></a>Aktivieren der Replikation

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.
2. Wählen Sie auf der Seite **Quelle** unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer** oder **Physische Computer** aus.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus. Wenn Sie physische Computer replizieren, ist diese Einstellung nicht relevant.
5. Wählen Sie den Prozessserver, dessen Name dem des Konfigurationsservers entspricht, wenn Sie keine zusätzlichen Prozessserver erstellt haben. Klicken Sie dann auf **OK**.

    ![Aktivieren der Replikationsquelle](./media/vmware-azure-enable-replication/enable-replication2.png)

6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die virtuellen Computer, für die ein Failover ausgeführt wurde, verwendet werden soll.

7. Wählen Sie das Azure Storage-Konto aus, das Sie für die Replikation von Daten verwenden möchten. 

    > [!NOTE]

    >   * Sie können ein Premium- oder Standardspeicherkonto auswählen. Wenn Sie ein Premium-Konto auswählen, müssen Sie für fortlaufende Replikationsprotokolle ein zusätzliches Standardspeicherkonto angeben. Die Konten müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
    >   * Falls Sie ein anderes Speicherkonto verwenden möchten, können Sie ein [Speicherkonto erstellen](../storage/common/storage-create-storage-account.md). Klicken Sie zum Erstellen eines Speicherkontos mit dem Resource Manager auf **Neu erstellen**. 

8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Wenn Sie über kein Netzwerk verfügen, müssen Sie ein [Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem Resource Manager auf **Neu erstellen**. Wählen Sie, falls zutreffend, ein Subnetz aus, und klicken Sie auf **OK**.

    ![Aktivieren der Replikationszieleinstellung](./media/vmware-azure-enable-replication/enable-rep3.png)
9. Wählen Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen** die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Aktivieren der Replikation: VMs auswählen](./media/vmware-azure-enable-replication/enable-replication5.png)
10. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren des Mobility Service auf dem Computer verwenden soll.  
11. Standardmäßig werden alle Datenträger repliziert. Klicken Sie zum Ausschließen von Datenträgern aus der Replikation auf **Alle Datenträger**, und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten.  Klicken Sie dann auf **OK**. Sie können später weitere Eigenschaften festlegen. [Weitere Informationen](vmware-azure-exclude-disk.md) zum Ausschließen von Datenträgern.

    ![Aktivieren der Replikation: Eigenschaften konfigurieren](./media/vmware-azure-enable-replication/enable-replication6.png)

12. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Sie können die Replikationsrichtlinieneinstellungen unter **Einstellungen** > **Replikationsrichtlinien** > (Richtlinienname) > **Einstellungen bearbeiten** ändern. Die Änderungen, die Sie an einer Richtlinie vornehmen, werden auch auf die Replikation und neue Computer angewendet.
13. Aktivieren Sie **Multi-VM-Konsistenz**, wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten. Geben Sie einen Namen für die Gruppe ein, und klicken Sie dann auf **OK**. 

    > [!NOTE]

    >    * Die Computer in einer Replikationsgruppe werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und app-konsistente Wiederherstellungspunkte.
    >    * Fassen Sie VMs und physische Server zusammen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann die Workloadleistung beeinträchtigen. Nutzen Sie sie nur, wenn mehrere Computer dieselbe Workload ausführen und Konsistenz erforderlich ist.

    ![Aktivieren der Replikation](./media/vmware-azure-enable-replication/enable-replication7.png)
14. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.



## <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Überprüfen Sie als Nächstes die Eigenschaften des Quellcomputers. Beachten Sie, dass der Name der Azure-VM die [Anforderungen für virtuelle Azure-Computer](vmware-physical-azure-support-matrix.md#replicated-machines) erfüllen muss.

1. Klicken Sie auf **Einstellungen** > **Replizierte Elemente**, und wählen Sie dann den Computer aus. Die Seite **Zusammenfassung** enthält Informationen zu Computereinstellungen und -status.
2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.
3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt.

    ![Eigenschaften für Compute und Netzwerk](./media/vmware-azure-enable-replication/vmproperties.png)

4.  Sie können eine [Ressourcengruppe](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) auswählen, der ein Computer nach einem Failover angehört. Sie können diese Einstellung jederzeit vor dem Failover ändern. Wenn Sie den Computer nach dem Failover zu einer anderen Ressourcengruppe migrieren, funktionieren die Schutzeinstellungen des Computers nicht mehr.
5. Sie können eine [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) auswählen, wenn der Computer nach dem Failover einer Verfügbarkeitsgruppe angehören muss. Beachten Sie bei der Auswahl einer Verfügbarkeitsgruppe folgende Punkte:

    * Es werden nur Verfügbarkeitsgruppen aufgelistet, die der angegebenen Ressourcengruppe angehören.  
    * Computer mit unterschiedlichen virtuellen Netzwerken dürfen nicht der gleichen Verfügbarkeitsgruppe angehören.
    * Einer Verfügbarkeitsgruppe dürfen nur virtuelle Computer gleicher Größe angehören.
5. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zu der IP-Adresse, die der Azure-VM zugewiesen ist, anzeigen und hinzufügen.
6. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert werden.

### <a name="configure-networks-and-ip-addresses"></a>Konfigurieren von Netzwerken und IP-Adressen

- Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht. Wenn die Adresse im Testfailover-Netzwerk verfügbar ist, kann dieselbe Ziel-IP-Adresse für das Testfailover verwendet werden.
- Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:
    - Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer dieselbe Anzahl von Netzwerkkarten wie der Quellcomputer.
    - Wenn die Anzahl von Netzwerkadaptern für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
    Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Falls der Quellcomputer dagegen zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe nur eine Karte unterstützt, erhält der Zielcomputer nur eine Karte.
    - Wenn der virtuelle Computer über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden. Außerdem wird die erste, die in der Liste angezeigt wird, zur *Standardnetzwerkkarte* des virtuellen Azure-Computers.

### <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Microsoft Software Assurance-Kunden können den Azure-Hybridvorteil nutzen, um Lizenzierungskosten für Windows Server-Computer zu sparen, die nach Azure migriert werden, oder um Azure für die Notfallwiederherstellung zu verwenden. Wenn Sie zur Nutzung des Azure-Hybridvorteils berechtigt sind, können Sie angeben, dass dieser Vorteil dem virtuellen Computer zugewiesen werden soll, der bei einem Failover von Azure Site Recovery erstellt wird. Gehen Sie dazu folgendermaßen vor:
- Navigieren Sie zum Abschnitt „Compute- und Netzwerkeigenschaften“ des replizierten virtuellen Computers.
- Beantworten Sie die Frage, ob Sie eine Windows Server-Lizenz besitzen, die Sie zur Nutzung des Azure-Hybridvorteils berechtigt.
- Bestätigen Sie durch Aktivieren des entsprechenden Kontrollkästchens für den Computer, der bei einem Failover erstellt wird, dass Sie über eine berechtigte Windows Server-Lizenz mit Software Assurance für die Nutzung des Azure-Hybridvorteils verfügen.
- Speichern Sie die Einstellungen für den replizierten Computer.

Weitere Informationen zum Azure-Hybridvorteil finden Sie [hier](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="common-issues"></a>Häufige Probleme

* Datenträger dürfen nicht größer sein als 1 TB.
* Bei dem Betriebssystem-Datenträger sollte es sich um einen Basisdatenträger anstelle eines dynamischen Datenträgers handeln.
* Für Computer der zweiten Generation/virtuelle UEFI-Computer muss es sich bei der Betriebssystemfamilie um Windows handeln, und der Startdatenträger muss kleiner als 300 GB sein.

## <a name="next-steps"></a>Nächste Schritte

Sobald der Schutz abgeschlossen ist und der Computer den Zustand „Geschützt“ aufweist, können Sie ein [Failover](site-recovery-failover.md) durchführen, um zu überprüfen, ob Ihre Anwendung in Azure verfügbar gemacht wird.

Wenn Sie den Schutz deaktivieren möchten, informieren Sie sich über das [Bereinigen von Registrierungs- und Schutzeinstellungen](site-recovery-manage-registration-and-protection.md).

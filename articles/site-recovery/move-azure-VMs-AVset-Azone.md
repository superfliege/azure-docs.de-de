---
title: Verschieben von virtuellen Azure IaaS-Computern als an Zonen angeheftete virtuelle Computer zu einer anderen Azure-Region mithilfe des Azure Site Recovery-Diensts | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure IaaS-Computer als an Zonen angeheftete virtuelle Computer in eine andere Azure-Region zu verschieben.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5597f3c017ccf2dbb58b7b6b046720c8f49803c5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312285"
---
# <a name="move-azure-vms-into-availability-zones"></a>Verschieben virtueller Azure-Computer in Verfügbarkeitszonen
Verfügbarkeitszonen in Azure schützen Ihre Anwendungen und Daten vor Rechenzentrumsausfällen. Jede Verfügbarkeitszone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Ausfällen von Rechenzentren. Mit Verfügbarkeitszonen bietet Azure die branchenweit beste Betriebszeit-SLA von 99,99 % für VMs. Verfügbarkeitszonen werden wie [hier](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) erwähnt in ausgewählten Regionen unterstützt. 

Wenn Sie Ihre virtuellen Computer als „Einzelinstanz“ in einer bestimmten Region bereitgestellt haben und Ihre Verfügbarkeit verbessern möchten, indem Sie diese in eine Verfügbarkeitszone verschieben, können Sie dazu Azure Site Recovery verwenden. Dies kann weiter kategorisiert werden in:

- Verschieben von Einzelinstanz-VMs in Verfügbarkeitszonen in einer Zielregion
- Verschieben von virtuellen Computern in einer Verfügbarkeitsgruppe in Verfügbarkeitszonen in einer Zielregion

> [!IMPORTANT]
> Azure Site Recovery unterstützt aktuell das Verschieben virtueller Computer zwischen Regionen, aber keine Verschiebung innerhalb einer Region. 

## <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen

- Überprüfen Sie, ob die Zielregion [Verfügbarkeitszonen unterstützt](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones): Überprüfen Sie, ob die von Ihnen ausgewählte [Kombination von Quell- und Zielregion unterstützt wird](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), und treffen Sie eine fundierte Entscheidung bezüglich der Zielregion.
- Stellen Sie sicher, dass Sie die [Architektur und die Komponenten des Szenarios](azure-to-azure-architecture.md) verstehen.
- Lesen Sie die [Einschränkungen und Anforderungen der Unterstützung](azure-to-azure-support-matrix.md).
- Überprüfen der Kontoberechtigungen: Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator dieses Abonnements. Wenn Sie nicht der Abonnementadministrator sind, wenden Sie sich an den Administrator, damit dieser die erforderlichen Berechtigungen zuweist. Um die Replikation für einen virtuellen Computer zu aktivieren und Daten schließlich mithilfe von Azure Site Recovery in die Zielregion zu kopieren, benötigen Sie Folgendes:

    1. Berechtigungen zum Erstellen einer VM in Azure-Ressourcen. Die integrierte Rolle „Mitwirkender für virtuelle Computer“ verfügt über diese Berechtigungen, zu den die folgenden gehören:
        - Berechtigung zum Erstellen einer VM in der ausgewählten Ressourcengruppe
        - Berechtigung zum Erstellen einer VM im ausgewählten virtuellen Netzwerk
        - Berechtigung zum Schreiben in das ausgewählte Speicherkonto

    2. Sie benötigen außerdem die Berechtigung zum Verwalten von Azure Site Recovery-Vorgängen. Die Rolle „Site Recovery-Mitwirkender“ verfügt über alle erforderlichen Berechtigungen zum Verwalten von Site Recovery-Vorgängen in einem Recovery Services-Tresor.

## <a name="prepare-the-source-vms"></a>Vorbereiten der virtuellen Quellcomputer

1. Ihre virtuellen Computer müssen verwaltete Datenträger verwenden, wenn Sie sie mithilfe von Site Recovery in eine Verfügbarkeitszone verschieben möchten. Sie können vorhandene virtuelle Windows-Computer (VMs) konvertieren, die nicht verwaltete Datenträger verwenden. Sie können die virtuellen Computer mit den [hier](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks) angegebenen Schritte konvertieren, sodass verwaltete Datenträger verwendet werden. Stellen Sie dazu sicher, dass die Verfügbarkeitsgruppe auf „Verwaltet“ festgelegt ist. 
2. Stellen Sie sicher, dass alle aktuellen Stammzertifikate auf den virtuellen Azure-Computern vorhanden sind, die Sie verschieben möchten. Wenn die aktuellen Stammzertifikate nicht vorhanden sind, kann das Kopieren von Daten in die Zielregion aufgrund von Sicherheitseinschränkungen nicht aktiviert werden.

3. Installieren Sie alle aktuellen Windows-Updates auf Windows-VMs, damit alle vertrauenswürdigen Stammzertifikate auf den Computern vorhanden sind. Führen Sie in einer nicht verbundenen Umgebung den Standardprozess für Windows Update und Zertifikatupdates in Ihrer Organisation durch.

4. Befolgen Sie bei Linux-VMs die Anleitung Ihres Linux-Händlers, um die aktuellen vertrauenswürdigen Stammzertifikate und die Zertifikatssperrliste auf der VM abzurufen.
5. Stellen Sie sicher, dass Sie keinen Authentifizierungsproxy verwenden, um die Netzwerkkonnektivität für virtuelle Computer zu steuern, die Sie migrieren möchten.

6. Wenn der virtuelle Computer, die Sie verschieben möchten, keinen Zugriff auf das Internet hat und einen Firewallproxy zum Steuern des ausgehenden Zugriffs verwendet, überprüfen Sie die [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) aufgeführten Anforderungen.

7. Identifizieren Sie das Quellnetzwerklayout und alle Ressourcen, die Sie derzeit verwenden (einschließlich Lastenausgleichsmodule, Netzwerksicherheitsgruppen, öffentliche IP-Adressen usw. für Ihre Überprüfung).

## <a name="prepare-the-target-region"></a>Vorbereiten der Zielregion

1. Vergewissern Sie sich, dass Ihr Azure-Abonnement das Erstellen von VMs in der für die Notfallwiederherstellung verwendeten Zielregion zulässt. Wenden Sie sich ggf. an den Support, um das erforderliche Kontingent zu aktivieren.

2. Stellen Sie sicher, dass Ihr Abonnement über ausreichend Ressourcen verfügt, um VMs zu unterstützen, die so groß wie die Quell-VMs sind. Wenn Sie Site Recovery verwenden, um Daten in die Zielregion zu kopieren, wird dieselbe oder eine möglichst ähnliche Größe für den virtuellen Zielcomputer ausgewählt.

3. Achten Sie darauf, für jede im Quellnetzwerklayout identifizierte Komponente eine Ressource zu erstellen. Dadurch wird sichergestellt, dass Ihre virtuellen Computer nach dem Verschieben in die Zielregion über alle Funktionen und Features verfügen, die Sie in der Quellregion genutzt haben.

    > [!NOTE]
    > Azure Site Recovery ermittelt und erstellt automatisch ein virtuelles Netzwerk und ein Speicherkonto, wenn Sie die Replikation für den virtuellen Quellcomputer aktivieren. Alternativ können Sie diese Ressourcen vorab erstellen und dem virtuellen Computer beim Aktivieren der Replikation zuweisen. Alle anderen Ressourcen (siehe unten) müssen jedoch manuell in der Zielregion erstellt werden.

     Informationen zum Erstellen der für Sie relevanten am häufigsten verwendeten Netzwerkressourcen auf Grundlage der Quell-VM-Konfiguration finden Sie in den folgenden Dokumenten.

    - [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Lastenausgleichsmodule](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials
        
     - [Öffentliche IP-Adresse](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Informationen zu anderen Netzwerkkomponenten finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/#pivot=products&panel=network) zum Netzwerk. 

> [!IMPORTANT]
> Achten Sie darauf, am Ziel einen zonenredundanten Lastenausgleich zu verwenden. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).

4. [Erstellen Sie manuell ein nicht für die Produktion vorgesehenes Netzwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in der Zielregion, wenn Sie die Konfiguration vor der endgültigen Verschiebung in die Zielregion testen möchten. Diese Vorgehensweise wird empfohlen, da es so nur zu minimalen Beeinträchtigungen in der Produktionsumgebung kommt.


> [!NOTE]
> Die folgenden Schritte gelten für einen einzelnen virtuellen Computer. Sie können auf mehrere virtuelle Computer angewendet werden, indem Sie zum Recovery Services-Tresor navigieren, auf „+ Replizieren“ klicken und die gewünschten virtuellen Computer zusammen auswählen.

## <a name="enable-replication"></a>Aktivieren der Replikation
Im Folgenden wird erläutert, wie Sie die Replikation von Daten mithilfe von Azure Site Recovery aktivieren, bevor Sie sie schließlich in Verfügbarkeitszonen verschieben.

1. Klicken Sie im Azure-Portal auf **Virtuelle Computer**, und wählen Sie den virtuellen Computer aus, den Sie in Verfügbarkeitszonen verschieben möchten.
2. Klicken Sie in den **Vorgängen** auf **Notfallwiederherstellung**.
3. Wählen Sie unter **Configure disaster recovery** (Notfallwiederherstellung konfigurieren) > **Zielregion** die Zielregion aus, in die Sie replizieren möchten. Vergewissern Sie sich, dass diese Region Verfügbarkeitszonen [unterstützt](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones).
![enable-rep-1.PNG](media/azure-vms-to-zones/enable-rep-1.PNG)

1. Klicken Sie auf **Weiter: Erweiterte Einstellungen**
2. Wählen Sie die richtigen Werte für Zielabonnement, Ziel-VM-Ressourcengruppe und virtuelles Netzwerk aus.
3. Wählen Sie im Abschnitt **Verfügbarkeit** die Verfügbarkeitszone aus, in die Sie den virtuellen Computer verschieben möchten. 
> [!NOTE]
> Wenn die Option für Verfügbarkeitsgruppe oder -zone nicht angezeigt wird, vergewissern Sie sich, dass die [Voraussetzungen](#prepare-the-source-vms) erfüllt sind und die [Vorbereitung](#prepare-the-source-vms) der virtuellen Quellcomputer abgeschlossen ist.

   ![enable-rep-2.PNG](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Klicken Sie auf „Replikation aktivieren“. Dadurch wird ein Auftrag gestartet, um die Replikation der VM zu aktivieren.

## <a name="verify-settings"></a>Überprüfen der Einstellungen

Nach Abschluss des Replikationsauftrags können Sie den Replikationsstatus überprüfen, die Replikationseinstellungen ändern und die Bereitstellung testen.

1. Klicken Sie im VM-Menü auf **Notfallwiederherstellung**.
2. Sie können die Replikationsintegrität, die erstellten Wiederherstellungspunkte und die Quell- und Zielregionen auf der Karte überprüfen.

   ![Replikationsstatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Testen der Konfiguration

1. Klicken Sie im Menü des virtuellen Computers auf **Notfallwiederherstellung**.
2. Klicken Sie auf das Symbol **Testfailover**.
3. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:

   - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der vom Site Recovery-Dienst verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten verwendet und die Recovery Time Objective (RTO) niedrig gehalten.
   - **Letzter anwendungskonsistenter Zeitpunkt**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
   - **Benutzerdefiniert**: Wählen Sie einen beliebigen Wiederherstellungspunkt aus.

3. Wählen Sie das virtuelle Azure-Testzielnetzwerk aus, in das die virtuellen Azure-Computer zum Testen der Konfiguration verschoben werden sollen. 

> [!IMPORTANT]
> Wir empfehlen, für das Testfailover ein separates Azure-VM-Netzwerk zu verwenden und nicht das Produktionsnetzwerk in der Zielregion, in das Sie Ihre virtuellen Computer letztlich verschieben möchten.

4. Klicken Sie zum Testen der Verschiebung auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM ausgeführt wird, die passende Größe hat und mit dem entsprechenden Netzwerk verbunden ist.
6. Wenn Sie den beim Testen der Verschiebung erstellten virtuellen Computer löschen möchten, klicken Sie im replizierten Element auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Durchführen und Bestätigen der Verschiebung in die Zielregion

1.  Klicken Sie im Menü des virtuellen Computers auf **Notfallwiederherstellung**.
2. Klicken Sie auf das Symbol **Failover**.
3. Wählen Sie unter **Failover** die Option **Neueste** aus. 
4. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VM herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt. 
5. Überprüfen Sie nach Abschluss des Auftrags, ob der virtuelle Computer wie erwartet in der Azure-Zielregion angezeigt wird.
6. Klicken Sie unter **Replizierte Elemente** mit der rechten Maustaste auf die VM und dann auf **Commit**. Dadurch wird die Verschiebung in die Zielregion abgeschlossen. Warten Sie, bis der Commitauftrag abgeschlossen ist.

## <a name="discard-the-resource-in-the-source-region"></a>Verwerfen der Ressourcen in der Quellregion 

1. Navigieren Sie zum virtuellen Computer.  Klicken Sie auf **Replikation deaktivieren**.  Dadurch wird der Prozess zum Kopieren der Daten für den virtuellen Computer angehalten.  

> [!IMPORTANT]
> Dieser Schritt ist wichtig, da Ihnen andernfalls nach der Verschiebung Gebühren für die Site Recovery-Replikation in Rechnung gestellt werden. Die Quellreplikationseinstellungen werden automatisch bereinigt. Beachten Sie, dass die im Rahmen der Replikation installierte Site Recovery-Erweiterung nicht entfernt wird und manuell gelöscht werden muss. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Verfügbarkeit eines virtuellen Azure-Computers durch Verschieben in eine Verfügbarkeitsgruppe oder -zone erhöht. Nun können Sie die Notfallwiederherstellung für den verschobenen virtuellen Azure-Computer konfigurieren.

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung nach der Migration](azure-to-azure-quickstart.md)



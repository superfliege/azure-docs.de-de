---
title: Migrieren von virtuellen Azure IaaS-Computern zu einer anderen Azure-Region mithilfe des Azure Site Recovery-Diensts | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure IaaS-Computer von einer Azure-Region zu einer anderen zu migrieren.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: efa8f4fc604440b8c1396aa654834ce83a41844e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875810"
---
# <a name="move-azure-vms-to-another-region"></a>Verschieben virtueller Azure-Computer in eine andere Region

Es gibt verschiedene Szenarien, in denen Sie sich dafür entscheiden sollten, Ihre vorhandenen Azure IaaS-VMs von einer Region in eine andere zu verschieben: zum Verbessern der Zuverlässigkeit und Verfügbarkeit Ihrer vorhandenen VMs, zum Verbessern der Verwaltbarkeit, aus Gründen der Governance usw. 

In diesem Tutorial erfahren Sie, wie Sie Azure-VMs mithilfe von Azure Site Recovery in eine andere Region verschieben. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * [Überprüfen der Voraussetzungen](#verify-prerequisites)
> * [Vorbereiten der Quell-VMs](#prepare-the-source-vms)
> * [Vorbereiten der Zielregion](#prepare-the-target-region)
> * [Kopieren von Daten in die Zielregion](#copy-data-to-the-target-region)
> * [Testen der Konfiguration](#test-the-configuration)
> * [Durchführen der Verschiebung](#perform-the-move-to-the-target-region-and-confirm)
> * [Verwerfen der Ressourcen in der Quellregion](#discard-the-resource-in-the-source-region)

> [!IMPORTANT]
> In diesem Dokument wird Schritt für Schritt erläutert, wie Sie Azure-VMs von einer Region in eine andere verschieben. Falls Sie die Verfügbarkeit Ihrer Infrastruktur durch das Verschieben von VMs in Verfügbarkeitszonen verbessern möchten, finden Sie in diesem Tutorial weitere Informationen.

## <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen

- Stellen Sie sicher, dass in der Azure-Quellregion, aus der Sie VMs verschieben möchten, Azure-VMs vorhanden sind.
- Überprüfen Sie, ob die von Ihnen ausgewählte [Kombination von Quell- und Zielregion unterstützt wird](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), und treffen Sie eine fundierte Entscheidung bezüglich der Zielregion.
- Stellen Sie sicher, dass Sie die [Architektur und die Komponenten des Szenarios](azure-to-azure-architecture.md) verstehen.
- Lesen Sie die [Einschränkungen und Anforderungen der Unterstützung](azure-to-azure-support-matrix.md).
- Überprüfen der Kontoberechtigungen: Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator dieses Abonnements. Wenn Sie nicht der Abonnementadministrator sind, wenden Sie sich an den Administrator, damit dieser die erforderlichen Berechtigungen zuweist. Um die Replikation für einen virtuellen Computer zu aktivieren und Daten schließlich mithilfe von Azure Site Recovery in die Zielregion zu kopieren, benötigen Sie Folgendes:

    1. Berechtigungen zum Erstellen einer VM in Azure-Ressourcen. Die integrierte Rolle „Mitwirkender für virtuelle Computer“ verfügt über diese Berechtigungen, zu den die folgenden gehören:
        - Berechtigung zum Erstellen einer VM in der ausgewählten Ressourcengruppe
        - Berechtigung zum Erstellen einer VM im ausgewählten virtuellen Netzwerk
        - Berechtigung zum Schreiben in das ausgewählte Speicherkonto

    2. Sie benötigen außerdem die Berechtigung zum Verwalten von Azure Site Recovery-Vorgängen. Die Rolle „Site Recovery-Mitwirkender“ verfügt über alle erforderlichen Berechtigungen zum Verwalten von Site Recovery-Vorgängen in einem Recovery Services-Tresor.

## <a name="prepare-the-source-vms"></a>Vorbereiten der Quell-VMs

1. Stellen Sie sicher, dass alle aktuellen Stammzertifikate auf den Azure-VMs vorhanden sind, die Sie verschieben möchten. Wenn die aktuellen Stammzertifikate nicht vorhanden sind, kann das Kopieren von Daten in die Zielregion aufgrund von Sicherheitseinschränkungen nicht aktiviert werden.

    - Installieren Sie alle aktuellen Windows-Updates auf Windows-VMs, damit alle vertrauenswürdigen Stammzertifikate auf den Computern vorhanden sind. Führen Sie in einer nicht verbundenen Umgebung den Standardprozess für Windows Update und Zertifikatupdates in Ihrer Organisation durch.
    - Befolgen Sie bei Linux-VMs die Anleitung Ihres Linux-Händlers, um die aktuellen vertrauenswürdigen Stammzertifikate und die Zertifikatssperrliste auf der VM abzurufen.
2. Stellen Sie sicher, dass Sie keinen Authentifizierungsproxy verwenden, um die Netzwerkkonnektivität für virtuelle Computer zu steuern, die Sie migrieren möchten.
3. Wenn die VM, die Sie verschieben möchten, keinen Zugriff auf das Internet hat und einen Firewallproxy zum Steuern des ausgehenden Zugriffs verwendet, überprüfen Sie die [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) aufgeführten Anforderungen.
4. Identifizieren Sie das Quellnetzwerklayout und alle Ressourcen, die Sie derzeit verwenden (einschließlich, aber nicht beschränkt auf Lastenausgleichsmodule, Netzwerksicherheitsgruppen, öffentliche IP-Adressen usw.), und listen Sie sie zur Überprüfung auf.

## <a name="prepare-the-target-region"></a>Vorbereiten der Zielregion

1. Vergewissern Sie sich, dass Ihr Azure-Abonnement das Erstellen von VMs in der für die Notfallwiederherstellung verwendeten Zielregion zulässt. Wenden Sie sich ggf. an den Support, um das erforderliche Kontingent zu aktivieren.

2. Stellen Sie sicher, dass Ihr Abonnement über ausreichend Ressourcen verfügt, um VMs zu unterstützen, die so groß wie die Quell-VMs sind. Wenn Sie Site Recovery verwenden, um Daten in die Zielregion zu kopieren, wird dieselbe oder eine möglichst ähnliche Größe für den virtuellen Zielcomputer ausgewählt.

3. Achten Sie darauf, für jede im Quellnetzwerklayout identifizierte Komponente eine Ressource zu erstellen. Dadurch wird sichergestellt, dass Ihre virtuellen Computer nach dem Verschieben in die Zielregion über alle Funktionen und Features verfügen, die Sie in der Quellregion genutzt haben.

    > [!NOTE]
    > Azure Site Recovery ermittelt und erstellt automatisch ein virtuelles Netzwerk und ein Speicherkonto, wenn Sie die Replikation für den virtuellen Quellcomputer aktivieren. Alternativ können Sie diese Ressourcen vorab erstellen und dem virtuellen Computer beim Aktivieren der Replikation zuweisen. Alle anderen Ressourcen (siehe unten) müssen jedoch manuell in der Zielregion erstellt werden.

     Informationen zum Erstellen der für Sie relevanten am häufigsten verwendeten Netzwerkressourcen auf Grundlage der Quell-VM-Konfiguration finden Sie in den folgenden Dokumenten.

    - [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Load Balancer](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Öffentliche IP-Adresse](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Informationen zu anderen Netzwerkkomponenten finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/#pivot=products&panel=network) zum Netzwerk. 

4. [Erstellen Sie manuell ein nicht für die Produktion vorgesehenes Netzwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in der Zielregion, wenn Sie die Konfiguration vor der endgültigen Verschiebung in die Zielregion testen möchten. Diese Vorgehensweise wird empfohlen, da es so nur zu minimalen Beeinträchtigungen in der Produktionsumgebung kommt.
    
## <a name="copy-data-to-the-target-region"></a>Kopieren von Daten in die Zielregion
Im Folgenden wird erläutert, wie Sie Daten mithilfe von Azure Site Recovery in die Zielregion kopieren.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Erstellen Sie den Tresor in einer beliebigen Region außer der Quellregion.

1. Melden Sie sich unter [Azure-Portal](https://portal.azure.com) > **Recovery Services** an.
2. Klicken Sie auf **Ressource erstellen** > **Verwaltungstools** > **Backup & Site Recovery**.
3. Geben Sie in **Name** den Anzeigenamen **ContosoVMVault** ein. Wenn Sie mehrere Abonnements haben, wählen Sie das entsprechende Abonnement aus.
4. Erstellen Sie die Ressourcengruppe **ContosoRG**.
5. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Klicken Sie unter „Recovery Services-Tresore“ auf **Übersicht** > **ConsotoVMVault** > **+ Replizieren**.
7. Wählen Sie unter **Quelle** die Option **Azure**.
8. Wählen Sie unter **Quellstandort** die Azure-Quellregion aus, in der Ihre VMs derzeit ausgeführt werden.
9. Wählen Sie das Ressourcen-Manager-Bereitstellungsmodell. Wählen Sie dann das **Quellabonnement** und die **Quellressourcengruppe** aus.
10. Klicken Sie auf **OK** , um die Einstellungen zu speichern.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Aktivieren Sie die Replikation für Azure-VMs, und starten Sie das Kopieren der Daten.

Site Recovery ruft eine Liste der VMs ab, die dem Abonnement und der Ressourcengruppe zugeordnet sind.

1. Gehen Sie im nächsten Schritt wie folgt vor: Wählen Sie den virtuellen Computer aus, den Sie migrieren möchten. Klicken Sie dann auf **OK**.
3. Klicken Sie in den **Einstellungen** auf **Notfallwiederherstellung**.
4. Wählen Sie unter **Configure disaster recovery** (Notfallwiederherstellung konfigurieren) > **Zielregion** die Zielregion aus, in die Sie replizieren möchten.
5. Sie können entweder die Standardzielressourcen oder die vorab erstellten Ressourcen verwenden.
6. Klicken Sie auf **Replikation aktivieren**. Dadurch wird ein Auftrag gestartet, um die Replikation der VM zu aktivieren.

    ![Replikation aktivieren](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>Testen der Konfiguration


1. Navigieren Sie zum Tresor, klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf die VM, die Sie in die Zielregion verschieben möchten, und klicken Sie auf das Symbol **+ Testfailover**.
2. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:

   - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der vom Site Recovery-Dienst verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten verwendet und die Recovery Time Objective (RTO) niedrig gehalten.
   - **Letzter anwendungskonsistenter Zeitpunkt**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
   - **Benutzerdefiniert**: Wählen Sie einen beliebigen Wiederherstellungspunkt aus.

3. Wählen Sie das virtuelle Azure-Zielnetzwerk aus, in das die Azure-VMs zum Testen der Konfiguration verschoben werden sollen. 

> [!IMPORTANT]
> Wir empfehlen, für das Testfailover ein separates Azure-VM-Netzwerk zu verwenden und nicht das Produktionsnetzwerk in der Zielregion, in das Sie Ihre VMs letztlich verschieben möchten.

4. Klicken Sie zum Testen der Verschiebung auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM ausgeführt wird, die passende Größe hat und mit dem entsprechenden Netzwerk verbunden ist.
6. Wenn Sie den beim Testen der Verschiebung erstellten virtuellen Computer löschen möchten, klicken Sie im replizierten Element auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Durchführen und Bestätigen der Verschiebung in die Zielregion

1.  Navigieren Sie zum Tresor, klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf die VM und dann auf **Failover**.
2. Wählen Sie unter **Failover** die Option **Neueste** aus. 
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VM herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt. 
4. Überprüfen Sie nach Abschluss des Auftrags, ob der virtuelle Computer wie erwartet in der Azure-Zielregion angezeigt wird.
5. Klicken Sie unter **Replizierte Elemente** mit der rechten Maustaste auf die VM und dann auf **Commit**. Dadurch wird die Verschiebung in die Zielregion abgeschlossen. Warten Sie, bis der Commitauftrag abgeschlossen ist.

## <a name="discard-the-resource-in-the-source-region"></a>Verwerfen der Ressourcen in der Quellregion 

1. Navigieren Sie zum virtuellen Computer.  Klicken Sie auf **Replikation deaktivieren**.  Dadurch wird der Prozess zum Kopieren der Daten für den virtuellen Computer angehalten.  

> [!IMPORTANT]
> Führen Sie diesen Schritt unbedingt aus. Andernfalls werden Ihnen nach der Verschiebung Gebühren für die Site Recovery-Replikation in Rechnung gestellt.

Falls Sie nicht vorhaben, die Quellressourcen wiederzuverwenden, fahren Sie mit den nächsten Schritten fort.

1. Löschen Sie alle relevanten Netzwerkressourcen in der Quellregion, die Sie in Schritt 4 [Vorbereiten der Quell-VMs](#prepare-the-source-vms) aufgelistet haben. 
2. Löschen Sie das entsprechende Speicherkonto in der Quellregion.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen virtuellen Azure-Computer in eine andere Azure-Region verschoben. Nun können Sie die Notfallwiederherstellung für den verschobenen virtuellen Azure-Computer konfigurieren.

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung nach der Migration](azure-to-azure-quickstart.md)


---
title: Migrieren von Azure-VMs zwischen Azure-Regionen mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um Azure-IaaS-VMs aus einer Azure-Region in eine andere zu migrieren.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Migrieren virtueller Azure-Computer zu einer anderen Region

Zusätzlich zur Verwendung des Diensts [Azure Site Recovery](site-recovery-overview.md) zur Verwaltung und Orchestrierung der Notfallwiederherstellung von lokalen Computern und Azure-VMs für die BCDR-Zwecke (Business Continuity/Disaster Recovery, Geschäftskontinuität und Notfallwiederherstellung) können Sie Site Recovery auch zur Migration von Azure-VMs in eine sekundäre Region verwenden. Um Azure-VMs zu migrieren, aktivieren Sie die Replikation für diese VMs und führen für sie ein Failover aus der primären Region in die sekundäre Region Ihrer Wahl durch.

Dieses Tutorial zeigt Ihnen, wie Sie Azure-VMs in eine andere Region migrieren können. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Recovery Services-Tresors
> * Aktivieren der Replikation für eine VM
> * Ausführen eines Failovers, um die VM zu migrieren

Dieses Tutorial setzt voraus, dass Sie bereits über ein Azure-Abonnement verfügen. Wenn Sie keines haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

>[!NOTE]
>
> Die Site Recovery-Replikation für Azure-VMs ist derzeit in der Vorschauphase.



## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie Azure-VMs in einer Azure-Region, aus der Sie migrieren möchten. Darüber hinaus gibt es eine Reihe von Einstellungen, die Sie überprüfen sollten, bevor Sie beginnen.


### <a name="verify-target-resources"></a>Überprüfen der Zielressourcen

1. Vergewissern Sie sich, dass Ihr Azure-Abonnement das Erstellen von VMs in der für die Notfallwiederherstellung verwendeten Zielregion zulässt. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

2. Stellen Sie sicher, dass Ihr Abonnement über ausreichend Ressourcen verfügt, um VMs zu unterstützen, die so groß wie die Quell-VMs sind. Site Recovery wählt dieselbe oder eine möglichst ähnliche Größe für die Ziel-VM aus.


### <a name="verify-account-permissions"></a>Überprüfen von Kontoberechtigungen

Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator dieses Abonnements. Wenn Sie nicht der Abonnementadministrator sind, wenden Sie sich an den Administrator, damit dieser die erforderlichen Berechtigungen zuweist. Zum Aktivieren der Replikation für eine neue VM benötigen Sie Folgendes:

1. Berechtigungen zum Erstellen einer VM in Azure-Ressourcen. Die integrierte Rolle „Mitwirkender für virtuelle Computer“ verfügt über diese Berechtigungen, zu den die folgenden gehören:
    - Berechtigung zum Erstellen einer VM in der ausgewählten Ressourcengruppe
    - Berechtigung zum Erstellen einer VM im ausgewählten virtuellen Netzwerk
    - Berechtigung zum Schreiben in das ausgewählte Speicherkonto

2. Sie benötigen außerdem die Berechtigung zum Verwalten von Azure Site Recovery-Vorgängen. Die Rolle „Site Recovery-Mitwirkender“ verfügt über alle erforderlichen Berechtigungen zum Verwalten von Site Recovery-Vorgängen in einem Recovery Services-Tresor.


### <a name="verify-vm-outbound-access"></a>Überprüfen des ausgehenden Zugriffs der VM

1. Stellen Sie sicher, dass Sie keinen Authentifizierungsproxy verwenden, um die Netzwerkkonnektivität für VMs zu steuern, die Sie migrieren möchten. 
2. Für die Zwecke dieses Tutorials gehen wir davon aus, dass die VMs, die Sie migrieren möchten, auf das Internet zugreifen können und keinen Firewallproxy zur Steuerung des ausgehenden Zugriffs verwenden. Falls doch, überprüfen Sie die Anforderungen [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Überprüfen von VM-Zertifikaten

Stellen Sie sicher, dass alle neuesten Stammzertifikate auf den Azure-VMs vorhanden sind, die Sie migrieren möchten. Wenn die neuesten Stammzertifikate nicht vorhanden sind, kann die VM aufgrund von Sicherheitseinschränkungen nicht bei Site Recovery registriert werden.

- Installieren Sie alle aktuellen Windows-Updates auf Windows-VMs, damit alle vertrauenswürdigen Stammzertifikate auf den Computern vorhanden sind. Führen Sie in einer nicht verbundenen Umgebung den Standardprozess für Windows Update und Zertifikatupdates in Ihrer Organisation durch.
- Befolgen Sie bei Linux-VMs die Anleitung Ihres Linux-Händlers, um die aktuellen vertrauenswürdigen Stammzertifikate und die Zertifikatssperrliste auf der VM abzurufen.



## <a name="create-a-vault"></a>Erstellen eines Tresors

Erstellen Sie den Tresor in einer beliebigen Region außer der Quellregion.

1. Melden Sie sich unter [Azure-Portal](https://portal.azure.com) > **Recovery Services** an.
2. Klicken Sie auf **Neu** > **Überwachung und Verwaltung** > **Backup und Site Recovery**.
3. Geben Sie in **Name** den Anzeigenamen **ContosoVMVault** ein. Wenn Sie mehrere Abonnements haben, wählen Sie das gewünschte aus.
4. Erstellen Sie die Ressourcengruppe **ContosoRG**.
5. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Um schnell über das Dashboard auf den Tresor zuzugreifen, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

   ![Neuer Tresor](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

Der neue Tresor wird dem **Dashboard** unter **Alle Ressourcen** und der Hauptseite **Recovery Services-Tresore** hinzugefügt.






## <a name="select-the-source"></a>Auswählen der Quelle

1. Klicken Sie unter „Recovery Services-Tresore“ auf **ConsotoVMVault** > **+Replizieren**.
2. Wählen Sie unter **Quelle** die Option **Azure – VORSCHAU**.
3. Wählen Sie unter **Quellstandort** die Azure-Quellregion aus, in der Ihre VMs derzeit ausgeführt werden.
4. Wählen Sie das Ressourcen-Manager-Bereitstellungsmodell. Wählen Sie dann die **Quellressourcengruppe** aus.
5. Klicken Sie auf **OK** , um die Einstellungen zu speichern.


## <a name="enable-replication-for-azure-vms"></a>Aktivieren der Replikation für Azure-VMs

Site Recovery ruft eine Liste der VMs ab, die dem Abonnement und der Ressourcengruppe zugeordnet sind.


1. Klicken Sie im Azure-Portal auf **Virtuelle Computer**.
2. Wählen Sie die VM aus, die Sie migrieren möchten. Klicken Sie dann auf **OK**.
3. Klicken Sie in den **Einstellungen** auf **Notfallwiederherstellung (Vorschau)**.
4. Wählen Sie unter **Configure disaster recovery** (Notfallwiederherstellung konfigurieren) > **Zielregion** die Zielregion aus, in die Sie replizieren möchten.
5. Übernehmen Sie für dieses Tutorial die anderen Standardwerte.
6. Klicken Sie auf **Replikation aktivieren**. Dadurch wird ein Auftrag gestartet, um die Replikation der VM zu aktivieren.

    ![Replikation aktivieren](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Derzeit wird die Replikation von Azure-VMs mit verwalteten Datenträgern nicht unterstützt. 

## <a name="run-a-failover"></a>Ausführen eines Failovers

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf die VM und dann auf **Failover**.
2. Wählen Sie unter **Failover** die Option **Neueste** aus. Die Einstellung für den Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VM herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.
5. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen und die Replikation für die VM beendet.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-VM in eine andere Azure-Region migriert. Nun können Sie die Notfallwiederherstellung für die migrierte VM konfigurieren.

> [!div class="nextstepaction"]
> [Einrichten der Notfallwiederherstellung nach der Migration](azure-to-azure-quickstart.md)


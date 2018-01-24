---
title: "Einrichten der Notfallwiederherstellung für lokale Hyper-V-VMs (ohne VMM) in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Notfallwiederherstellung für lokale Hyper-V-VMs (ohne VMM) in Azure mit dem Azure Site Recovery-Dienst einrichten."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Einrichten der Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie zur Notfallwiederherstellung, indem Replikation, Failover und Failback von lokalen Computern und virtuellen Azure-Computern (Virtual Machines, VMs) verwaltet und orchestriert werden.

In diesem Tutorial erfahren Sie, wie Sie die Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure einrichten. Das Tutorial gilt für Hyper-V-VMs, die nicht mit dem System Center Virtual Machine Manager (VMM) verwaltet werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auswählen von Replikationsquelle und -ziel
> * Einrichten der Quellreplikationsumgebung – einschließlich der lokalen Site Recovery-Komponenten – und der Zielreplikationsumgebung
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Prepare Azure](tutorial-prepare-azure.md) (Vorbereiten von Azure)
2. [Vorbereiten lokaler Hyper-V-Instanzen](tutorial-prepare-on-premises-hyper-v.md)

Bevor Sie beginnen, empfiehlt sich eine [Überprüfung der Architektur](concepts-hyper-v-to-azure-architecture.md) für dieses Notfallwiederherstellungsszenario.

## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels


1. Klicken Sie in **Alle Dienste** > **Recovery Services-Tresore** auf den Namen des Tresors, der im vorherigen Tutorial vorbereitet wurde: **ContosoVMVault**.
2. Klicken Sie in **Erste Schritte** auf **Site Recovery**. Klicken Sie dann auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V** aus.
6. Wählen Sie in **Verwenden Sie System Center VMM?** die Option **Nein** aus. Klicken Sie dann auf **OK**.

    ![Replikationsziel](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Um die Quellumgebung einzurichten, fügen Sie Hyper-V-Hosts zu einem Hyper-V-Standort hinzu, laden den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent herunter und installieren sie. Anschließend registrieren Sie den Hyper-V-Standort im Tresor. 

1. Klicken Sie unter **Bereiten Sie die Infrastruktur vor** auf **Quelle**.
2. Klicken Sie auf **-Hyper-V-Standort**, und geben Sie den Namen des Standorts ein, der im vorherigen Tutorial erstellt wurde, **ContosoHyperVSite**.
3. Klicken Sie auf **+Hyper-V-Server**.
4. Laden Sie die Setupdatei für den Anbieter herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des Anbietersetups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Anbieter herunterladen](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>Installieren des Anbieters

Führen Sie die Setupdatei für den Anbieter (AzureSiteRecoveryProvider.exe) auf jedem Hyper-V-Host aus, den Sie dem Standort **ContosoHyperVSite** hinzugefügt haben. Setup installiert Azure Site Recovery-Anbieter und den Recovery Services-Agent auf jedem Hyper-V-Host.

1. Wählen Sie im Setup-Assistenten für den Azure Site Recovery-Anbieter **Microsoft Update** aus, um über Microsoft Update nach Anbieterupdates zu suchen.
2. Akzeptieren Sie unter **Installation** den standardmäßigen Installationsspeicherort für den Anbieter und den Agent, und klicken Sie auf **Installieren**.
3. Klicken Sie nach der Installation im Assistenten für die Microsoft Azure Site Recovery-Registrierung unter **Tresoreinstellungen** auf **Durchsuchen**, und wählen Sie in **Schlüsseldatei** die Tresorschlüsseldatei aus, die Sie heruntergeladen haben. 
4. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen (**ContosoVMVault**) und den Hyper-V-Standort (**ContosoHyperVSite**) an, zu dem der Hyper-V-Server gehört.
5. Wählen Sie unter **Proxyeinstellungen** die Option **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
6. Klicken Sie nach der Registrierung des Servers im Tresor unter **Registrierung** auf**Fertig stellen**.

Azure Site Recovery ruft die Metadaten vom Hyper-V-Server ab, und der Server wird in **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt. Dies kann bis zu 30 Minuten dauern.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie. 

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe (**ContosoRG**) aus, in denen Sie die Azure-VMs nach dem Failover erstellen möchten.
3. Wählen Sie das Bereitstellungsmodell **Resource Manager** aus.

Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.


## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.
2. Geben Sie unter **Richtlinie erstellen und zuordnen** den Richtliniennamen **ContosoReplicationPolicy** an.
3. Übernehmen Sie die Standardeinstellungen, und klicken Sie auf **OK**.
    - **Kopierhäufigkeit** gibt an, dass Deltadaten (nach der ersten Replikation) alle fünf Minuten repliziert werden sollen.
    - **Aufbewahrungszeitraum des Wiederherstellungspunkts** gibt an, dass das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte zwei Stunden beträgt.
    - **Häufigkeit der Momentaufnahmen für App-Konsistenz** gibt an, dass Wiederherstellungspunkte mit App-konsistenten Momentaufnahmen jede Stunde erstellt werden sollen.
    - **Startzeit der ersten Replikation** gibt an, dass die erste Replikation sofort beginnen soll.
4. Nachdem die Richtlinie erstellt wurde, klicken Sie auf **OK**. Wenn Sie eine neue Richtlinie erstellen, wird sie automatisch dem angegebenen Hyper-V-Standort (**ContosoHyperVSite**) zugeordnet.

    ![Replikationsrichtlinie](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Aktivieren der Replikation


1. Klicken Sie unter **Anwendung replizieren** auf **Quelle**. 
2. Wählen Sie unter **Quelle** den Standort **ContosoHyperVSite** aus. Klicken Sie dann auf **OK**.
3. Vergewissern Sie sich, dass unter **Ziel** Azure, das Tresorabonnement und das Bereitstellungsmodell **Resource Manager** angegeben ist.
4. Wählen Sie das im vorherigen Tutorial für die replizierten Daten erstellte Speicherkonto **contosovmsacct1910171607** sowie das Netzwerk **ContosoASRnet** aus, in dem die Azure-VMs nach dem Failover platziert werden.
5. Wählen Sie unter **Virtuelle Computer** > **Auswählen** die VM aus, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

 Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach der Ausführung des Auftrags **Schutz abschließen** ist die erste Replikation abgeschlossen und der virtuelle Computer bereit zum Failover.

## <a name="next-steps"></a>Nächste Schritte
[Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

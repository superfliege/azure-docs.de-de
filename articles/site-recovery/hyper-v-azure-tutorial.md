---
title: Einrichten der Notfallwiederherstellung für lokale Hyper-V-VMs (ohne VMM) in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Notfallwiederherstellung für lokale Hyper-V-VMs (ohne VMM) in Azure mit dem Azure Site Recovery-Dienst einrichten.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: da643a4d7a1dc74385b3854c1952af5ba93bd241
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358103"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Einrichten der Notfallwiederherstellung von lokalen Hyper-V-VMs in Azure

c

> [!div class="checklist"]
> * Wählen Sie die Replikationsquelle und das Replikationsziel aus.
> * Einrichten der Quellreplikationsumgebung – einschließlich der lokalen Site Recovery-Komponenten – und der Zielreplikationsumgebung
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in dem Artikel, der im Anleitungsabschnitt des Site Recovery-Inhaltsverzeichnisses angegeben ist.

## <a name="before-you-begin"></a>Voraussetzungen
Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Vorbereiten lokaler Hyper-V-Instanzen](tutorial-prepare-on-premises-hyper-v.md)



## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Wählen Sie unter **Recovery Services-Tresore** den Tresor aus. Im vorherigen Tutorial wurde der Tresor **ContosoVMVault** vorbereitet.
2. Klicken Sie in **Erste Schritte** auf **Site Recovery**. Klicken Sie dann auf **Infrastruktur vorbereiten**.
3. Wählen Sie unter **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie unter **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie unter **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V** aus.
6. Wählen Sie unter **Are you using System Center VMM to manage your Hyper-V hosts** (Verwenden Sie System Center VMM für die Verwaltung Ihrer Hyper-V-Hosts?) die Option **Nein** aus. Klicken Sie dann auf **OK**.

    ![Replikationsziel](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

1. Falls Sie eine umfangreiche Bereitstellung planen, laden Sie unter **Bereitstellungsplanung** über den auf der Seite bereitgestellten Link den Bereitstellungsplaner für Hyper-V herunter. Weitere Informationen zur Hyper-V-Bereitstellungsplanung finden Sie [hier](hyper-v-deployment-planner-overview.md).
2. In diesem Tutorial wird der Bereitstellungsplaner nicht benötigt. Wählen Sie unter **Haben Sie die Bereitstellungsplanung abgeschlossen?** die Option **Wird später durchgeführt** aus. Klicken Sie dann auf **OK**.

    ![Bereitstellungsplanung](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Zum Einrichten der Quellumgebung erstellen Sie eine Hyper-V-Site und fügen ihr Hyper-V-Hosts mit virtuellen Computern hinzu, die Sie dort replizieren möchten. Anschließend müssen Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent herunterladen und auf den einzelnen Hosts installieren sowie die Hyper-V-Site im Tresor registrieren. 

1. Klicken Sie unter **Infrastruktur vorbereiten** auf **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Hyper-V-Site**.
3. Geben Sie unter **Hyper-V-Site erstellen** einen Namen für die Site an. Hier wird **ContosoHyperVSite** verwendet.

    ![Hyper-V-Standort](./media/hyper-v-azure-tutorial/hyperv-site.png)

3. Wählen Sie nach Erstellung der Site unter **Quelle vorbereiten** > **Schritt 1: Hyper-V-Site auswählen** die Site aus, die Sie erstellt haben.
4. Klicken Sie auf **+Hyper-V-Server**.

    ![Hyper-V-Server](./media/hyper-v-azure-tutorial/hyperv-server.png)

4. Laden Sie das Installationsprogramm für den Microsoft Azure Site Recovery-Anbieter herunter.
6. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen Schlüssel für die Anbieterinstallation. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Anbieter herunterladen](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Installieren des Anbieters

Installieren Sie die heruntergeladene Setupdatei (AzureSiteRecoveryProvider.exe) auf jedem Hyper-V-Host, den Sie der Hyper-V-Site hinzufügen möchten. Setup installiert Azure Site Recovery-Anbieter und den Recovery Services-Agent auf jedem Hyper-V-Host.

1. Führen Sie die Setupdatei aus.
2. Wählen Sie unter Setup-Assistent für den Azure Site Recovery-Anbieter > **Microsoft Update** aus, um über Microsoft Update nach Anbieterupdates zu suchen.
3. Akzeptieren Sie unter **Installation** den standardmäßigen Installationsspeicherort für den Anbieter und den Agent, und klicken Sie auf **Installieren**.
4. Klicken Sie nach der Installation im Assistenten für die Microsoft Azure Site Recovery-Registrierung unter **Tresoreinstellungen** auf **Durchsuchen**, und wählen Sie in **Schlüsseldatei** die Tresorschlüsseldatei aus, die Sie heruntergeladen haben. 
5. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen (**ContosoVMVault**) und den Hyper-V-Standort (**ContosoHyperVSite**) an, zu dem der Hyper-V-Server gehört.
6. Wählen Sie unter **Proxyeinstellungen** die Option **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
7. Klicken Sie nach der Registrierung des Servers im Tresor unter **Registrierung** auf**Fertig stellen**.

Azure Site Recovery ruft die Metadaten vom Hyper-V-Server ab, und der Server wird in **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt. Dieser Vorgang kann bis zu 30 Minuten dauern.        

#### <a name="install-on-hyper-v-core-server"></a>Installieren auf dem Hyper-V Core Server

Wenn Sie einen Hyper-V Core Server ausführen, laden Sie die Setupdatei herunter, und gehen Sie wie folgt vor:

1. Extrahieren Sie die Dateien aus „AzureSiteRecoveryProvider.exe“ in einem lokalen Verzeichnis, indem Sie Folgendes ausführen:

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2.  Run `.\setupdr.exe /i. Die Ergebnisse werden in „%Programdata%\ASRLogs\DRASetupWizard.log“ protokolliert.

3.  Registrieren Sie den Server mithilfe des folgenden Befehls:

    ```
    cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```
 

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie. 

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe (**ContosoRG**) aus, in denen Sie die Azure-VMs nach dem Failover erstellen möchten.
3. Wählen Sie das **Resource Manager**-Bereitstellungsmodell aus.

Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.


## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an. Hier wird **ContosoReplicationPolicy** verwendet.
3. In diesem Tutorial behalten wir die Standardwerte bei.
    - **Kopierhäufigkeit** gibt an, wie oft Deltadaten nach der ersten Replikation repliziert werden (standardmäßig alle fünf Minuten).
    - **Aufbewahrungszeitraum des Wiederherstellungspunkts** gibt an, dass Wiederherstellungspunkte zwei Stunden lang aufbewahrt werden.
    - **App-konsistente Momentaufnahmehäufigkeit** gibt an, dass Wiederherstellungspunkte mit anwendungskonsistenten Momentaufnahmen jede Stunde erstellt werden sollen.
    - **Startzeit der ersten Replikation** gibt an, dass die erste Replikation sofort beginnen soll.
4. Nachdem die Richtlinie erstellt wurde, klicken Sie auf **OK**. Wenn Sie eine neue Richtlinie erstellen, wird sie automatisch der angegebenen Hyper-V-Site (in diesem Tutorial: **ContosoHyperVSite**) zugeordnet.

    ![Replikationsrichtlinie](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Aktivieren der Replikation


1. Klicken Sie unter **Anwendung replizieren** auf **Quelle**. 
2. Wählen Sie unter **Quelle** den Standort **ContosoHyperVSite** aus. Klicken Sie dann auf **OK**.
3. Vergewissern Sie sich, dass unter **Ziel** Azure, das Tresorabonnement und das Bereitstellungsmodell **Resource Manager** angegeben ist.
4. Wählen Sie bei Verwendung der Tutorialeinstellungen das Speicherkonto **contosovmsacct1910171607**, das im vorherigen Tutorial für replizierte Daten erstellt wurde, sowie das Netzwerk **ContosoASRnet** aus, in dem sich die virtuellen Azure-Computer nach dem Failover befinden.
5. Wählen Sie unter **Virtuelle Computer** > **Auswählen** die VM aus, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

   Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach der Ausführung des Auftrags **Schutz abschließen** ist die erste Replikation abgeschlossen und der virtuelle Computer bereit zum Failover.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

---
title: Einrichten der Notfallwiederherstellung von lokalen Hyper-V-VMs in VMM-Clouds nach Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Notfallwiederherstellung für lokale Hyper-V-VMs in System Center VMM-Clouds nach Azure mit dem Azure Site Recovery-Dienst einrichten.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 64559f653ba8a466de7bec10db34383b508e3e4b
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361297"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Einrichten der Notfallwiederherstellung von lokalen Hyper-V-VMs in VMM-Clouds nach Azure

In diesem Artikel wird beschrieben, wie Sie mithilfe des Diensts [Azure Site Recovery](site-recovery-overview.md) die Replikation für lokale Hyper-V-VMs, die in System Center Virtual Machine Manager (VMM) verwaltet werden, für die Notfallwiederherstellung in Azure aktivieren. Wenn Sie VMM nicht verwenden, finden Sie entsprechende Informationen in [diesem Tutorial](hyper-v-azure-tutorial.md).

Dies ist das dritte in einer Reihe von Tutorials zur Einrichtung der Notfallwiederherstellung in Azure für lokale VMware-VMs. Im vorherigen Tutorial haben Sie sich mit der [Vorbereitung der lokalen Hyper-V-Umgebung](hyper-v-prepare-on-premises-tutorial.md) für die Notfallwiederherstellung in Azure befasst. 

In diesem Tutorial lernen Sie Folgendes:


> [!div class="checklist"]
> * Wählen Sie die Replikationsquelle und das Replikationsziel aus.
> * Richten Sie die Quellreplikationsumgebung, einschließlich der lokalen Komponenten zur Sitewiederherstellung, und die Zielreplikationsumgebung ein.
> * Richten Sie Netzwerkzuordnungen zwischen VMM-VM-Netzwerken und virtuellen Netzwerken in Azure ein.
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM


> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in dem Artikel, der im Anleitungsabschnitt des Site Recovery-Inhaltsverzeichnisses angegeben ist.

## <a name="before-you-begin"></a>Voraussetzungen

Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Vorbereiten lokaler Hyper-V-Instanzen](tutorial-prepare-on-premises-hyper-v.md) Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:


## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Wählen Sie unter **Recovery Services-Tresore** den Tresor aus. Im vorherigen Tutorial wurde der Tresor **ContosoVMVault** vorbereitet.
2. Klicken Sie in **Erste Schritte** auf **Site Recovery**. Klicken Sie dann auf **Infrastruktur vorbereiten**.
3. Wählen Sie unter **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie unter **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie unter **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V** aus.
6. Wählen Sie in **Are you using System Center VMM** (Verwenden Sie System Center VMM?) die Option **Ja** aus. Klicken Sie dann auf **OK**.

    ![Replikationsziel](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)


## <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

1. Falls Sie eine umfangreiche Bereitstellung planen, laden Sie unter **Bereitstellungsplanung** über den auf der Seite bereitgestellten Link den Bereitstellungsplaner für Hyper-V herunter. Weitere Informationen zur Hyper-V-Bereitstellungsplanung finden Sie [hier](hyper-v-deployment-planner-overview.md).
2. In diesem Tutorial wird der Bereitstellungsplaner nicht benötigt. Wählen Sie unter **Haben Sie die Bereitstellungsplanung abgeschlossen?** die Option **Wird später durchgeführt** aus. Klicken Sie dann auf **OK**.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Beim Einrichten der Quellumgebung installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server und registrieren den Server im Tresor. Sie installieren den Azure Recovery Services-Agent auf den einzelnen Hyper-V-Hosts. 

1. Klicken Sie unter **Bereiten Sie die Infrastruktur vor** auf **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **System Center VMM-Server** angezeigt wird.
3. Laden Sie das Installationsprogramm für den Microsoft Azure Site Recovery-Anbieter herunter.
4. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des Anbietersetups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.
5. Laden Sie das Installationsprogramm für den Microsoft Azure Recovery Services-Agent herunter.

    ![Download](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Installieren des Anbieters auf dem VMM-Server

1. Wählen Sie unter Setup-Assistent für den Azure Site Recovery-Anbieter > **Microsoft Update** aus, um über Microsoft Update nach Anbieterupdates zu suchen.
2. Akzeptieren Sie unter **Installation** den Standard-Installationsspeicherort für den Anbieter, und klicken Sie auf **Installieren**. 
3. Klicken Sie nach der Installation unter Microsoft Azure Site Recovery-Registrierungs-Assistent > **Tresoreinstellungen** auf **Durchsuchen**, und wählen Sie in **Schlüsseldatei** die Tresorschlüsseldatei aus, die Sie heruntergeladen haben.
4. Geben Sie das Azure Site Recovery-Abonnement und den Tresornamen (**ContosoVMVault**) an. Geben Sie einen Anzeigenamen für den VMM-Server an, mit dem er im Tresor identifiziert werden soll.
5. Wählen Sie unter **Proxyeinstellungen** die Option **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
6. Akzeptieren Sie den Standardspeicherort für das Zertifikat, mit dem Daten verschlüsselt werden. Wenn Sie ein Failover ausführen, werden verschlüsselte Daten entschlüsselt.
7. Wählen Sie unter **Cloudmetadaten synchronisieren** die Option **Cloudmetadaten mit dem Site Recovery Portal synchronisieren** aus. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Klicken Sie dann auf **Registrieren**.
8. Nachdem der Server im Tresor registriert wurde, klicken Sie auf **Fertig stellen**.

Nach der Registrierung ruft Azure Site Recovery Metadaten vom Server ab, und der VMM-Server wird in **Site Recovery-Infrastruktur** angezeigt.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Installieren des Recovery Services-Agent auf den Hyper-V-Hosts

Installieren Sie den Agent auf jedem Hyper-V-Host, auf dem sich virtuelle Computer befinden, die Sie replizieren möchten.

1. Klicken Sie unter Setup-Assistent für den Microsoft Azure Recovery Services-Agent > **Voraussetzungsprüfung** auf **Weiter**. Alle fehlenden Komponenten, die Voraussetzung sind, werden automatisch installiert.
2. Übernehmen Sie unter **Installationseinstellungen** den Installationsspeicherort und den Cachespeicherort. Das Cachelaufwerk benötigt mindestens 5 GB Speicherplatz. Es wird empfohlen, ein Laufwerk mit mindestens 600 GB freiem Speicherplatz zu verwenden. Klicken Sie dann auf **Weiter**.
3. Klicken Sie nach Abschluss der Installation in **Installation** auf **Schließen**, um den Assistenten zu beenden.

    ![Installations-Agent](./media/hyper-v-vmm-azure-tutorial/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe (**ContosoRG**) aus, in denen Sie die Azure-VMs nach dem Failover erstellen möchten.
3. Wählen Sie das Bereitstellungsmodell **Resource Manager** aus.

Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.


## <a name="configure-network-mapping"></a>Konfigurieren der Netzwerkzuordnung

1. Klicken Sie unter **Site Recovery-Infrastruktur** > **Netzwerkzuordnungen** > **Netzwerkzuordnung** auf das Symbol **+Netzwerkzuordnung**.
2. Wählen Sie unter **Netzwerkzuordnung hinzufügen** den VMM-Quellserver aus. Wählen Sie **Azure** als Ziel aus.
3. Überprüfen Sie das Abonnement und das Bereitstellungsmodell nach einem Failover.
4. Wählen Sie unter **Quellnetzwerk** das lokale VM-Quellnetzwerk aus.
5. Wählen Sie unter **Zielnetzwerk** das Azure-Netzwerk aus, in dem Replikate von Azure-VMs nach der Erstellung im Anschluss an ein Failover platziert werden. Klicken Sie dann auf **OK**.

    ![Netzwerkzuordnung](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an. Hier wird **ContosoReplicationPolicy** verwendet.
3. Übernehmen Sie die Standardeinstellungen, und klicken Sie auf **OK**.
    - **Kopierhäufigkeit** gibt an, dass Deltadaten (nach der ersten Replikation) alle fünf Minuten repliziert werden sollen.
    - **Aufbewahrungszeitraum des Wiederherstellungspunkts** gibt an, dass alle Wiederherstellungspunkte zwei Stunden lang aufbewahrt werden.
    - **App-konsistente Momentaufnahmehäufigkeit** gibt an, dass Wiederherstellungspunkte mit anwendungskonsistenten Momentaufnahmen jede Stunde erstellt werden sollen.
    - **Startzeit der ersten Replikation** gibt an, dass die erste Replikation sofort beginnen soll.
    - **In Azure gespeicherte Daten verschlüsseln:** Die Standardeinstellung **Deaktiviert** gibt an, dass ruhende Daten in Azure nicht verschlüsselt werden.
4. Nachdem die Richtlinie erstellt wurde, klicken Sie auf **OK**. Wenn Sie eine neue Richtlinie erstellen, wird sie der VMM-Cloud automatisch zugeordnet.

## <a name="enable-replication"></a>Aktivieren der Replikation

1. Klicken Sie unter **Anwendung replizieren** auf **Quelle**. 
2. Wählen Sie unter **Quelle** die VMM-Cloud aus. Klicken Sie dann auf **OK**.
3. Vergewissern Sie sich unter **Ziel**, dass Azure das Ziel ist, überprüfen Sie das Tresorabonnement, und wählen Sie das **Resource Manager**-Modell aus.
4. Wählen Sie das Speicherkonto **contosovmsacct1910171607** und das Azure-Netzwerk **ContosoASRnet** aus.
5. Wählen Sie unter **Virtuelle Computer** > **Auswählen** die VM aus, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

   Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach der Ausführung des Auftrags **Schutz abschließen** ist die erste Replikation abgeschlossen und der virtuelle Computer bereit zum Failover.



## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

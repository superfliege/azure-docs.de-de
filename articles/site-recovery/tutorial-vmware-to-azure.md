---
title: "Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die Notfallwiederherstellung in Azure für lokale VMware-VMs mit dem Azure Site Recovery-Dienst einrichten."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3d9248d2501c7fea0492bad2687b6bdfb0b903e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs

In diesem Tutorial wird veranschaulicht, wie die Notfallwiederherstellung in Azure für lokale VMware-VMs unter Windows eingerichtet wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Geben Sie die Replikationsquelle und das Replikationsziel an.
> * Richten Sie die Quellreplikationsumgebung, einschließlich der lokalen Komponenten zur Sitewiederherstellung, und die Zielreplikationsumgebung ein.
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Prepare Azure](tutorial-prepare-azure.md) (Vorbereiten von Azure)
2. [Prepare on-premises VMware](tutorial-prepare-on-premises-vmware.md) (Lokales Vorbereiten von VMware)

Bevor Sie beginnen, empfiehlt sich eine [Überprüfung der Architektur](concepts-vmware-to-azure-architecture.md) für ein Notfallwiederherstellungsszenario.


## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Klicken Sie unter **Recovery Services-Tresore** auf den Tresornamen, **ContosoVMVault**.
2. Klicken Sie in **Erste Schritte** auf „Site Recovery“. Klicken Sie dann auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Klicken Sie dann auf **OK**.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Zum Einrichten der Quellumgebung benötigen Sie einen einzelnen, hochverfügbaren, lokalen Computer, um lokale Site Recovery-Komponenten zu hosten. Die Komponenten umfassen den Konfigurationsserver, Prozessserver und Masterzielserver.

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- Der Prozessserver fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver installiert auch den Mobility Service auf VMs, die Sie replizieren möchten, und führt auf lokalen VMware VMs eine automatische Ermittlung durch.
- Der Masterzielserver verarbeitet die Replikationsdaten während des Failbacks von Azure.

Wenn Sie den Konfigurationsserver als hochverfügbaren virtuellen VMware-Computer einrichten möchten, laden Sie eine vorbereitete OVF-Vorlage herunter und importieren die Vorlage zum Erstellen des virtuellen Computers in VMware. Nach dem Einrichten des Konfigurationsservers registrieren Sie ihn im Tresor. Nach der Registrierung ermittelt Site Recovery lokale virtuelle VMware-Computer.

### <a name="download-the-vm-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie in **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVF-Vorlage (Open Virtualization Format) für den Konfigurationsserver herunter.

  > [!TIP]
  Die neueste Version der Konfigurationsservervorlage kann direkt im [Microsoft Download Center](https://aka.ms/asrconfigurationserver) heruntergeladen werden.

## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich auf dem VMware vCenter-Server oder dem vSphere ESXi-Host mit dem VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten.  

     ![OVF-Vorlage](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. Geben Sie in **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage an.
4. Klicken Sie in **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie in **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie in **Speicher auswählen** unter **Select virtual disk format** (Format für virtuellen Datenträger auswählen) für eine optimale Leistung **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus.
4. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
5. Unter **Bereit für den Abschluss**:
  - Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.
  - Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten), und wählen Sie dann **Fertig stellen** aus. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt, Sie können aber nach der Bereitstellung noch weitere NICs hinzufügen.

  
## <a name="add-an-additional-adapter"></a>Hinzufügen zusätzlicher Adapter

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie dies durch, bevor Sie den Server im Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie auf **Weiter**.
3. Wählen Sie den Adaptertyp und ein Netzwerk aus. 
4. Um die virtuelle NIC zu verbinden, wenn der virtuelle Computer eingeschaltet wird, wählen Sie **Connect at power on** (Beim Einschalten verbinden) aus. Klicken Sie auf **Weiter** > **Fertig stellen** und dann auf **OK**.


## <a name="register-the-configuration-server"></a>Registrieren des Konfigurationsservers 

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Nehmen den Lizenzvertrag an, und geben Sie ein Administratorkennwort an.
3. Melden Sie sich nach Abschluss der Installation als Administrator auf dem virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool gestartet.
5. Geben Sie einen Namen an, der für die Registrierung des Konfigurationsservers mit Site Recovery verwendet wird. Klicken Sie auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie, nachdem die Verbindung hergestellt wurde, auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut auf dem Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.

### <a name="configure-settings-and-connect-to-vmware"></a>Konfigurieren von Einstellungen und Herstellen einer Verbindung mit VMware

1. Wählen Sie im Assistenten für die Konfigurationsserververwaltung unter **Konnektivität einrichten** die NIC aus, die den Replikationsdatenverkehr empfängt. Klicken Sie anschließend auf **Speichern**. Sie können diese Einstellung nach der Konfiguration nicht mehr ändern.
2. Wählen Sie unter **Recovery Services-Tresor auswählen** Ihr Azure-Abonnement, die zugehörige Ressourcengruppe und den Tresor aus.
3. Akzeptieren Sie unter **Drittanbieter-Software installieren** den Lizenzvertrag, und klicken Sie auf **Herunterladen und installieren**, um MySQL Server zu installieren.
4. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt durchführen. Klicken Sie anschließend auf **Weiter**.
5. In **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor Sie fortfahren.
6. Geben Sie in **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts an, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port, an dem der Server lauscht, sowie einen Anzeigenamen, der für den VMware-Server im Tresor verwendet werden soll, an.
7. Geben Sie die Anmeldeinformationen an, die der Konfigurationsserver zum Verbinden mit dem VMware-Server verwendet. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**.
8. Geben Sie in **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort an, die für die automatische Installation des Mobility Service auf Computern verwendet werden, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
9. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen. 
10. Überprüfen Sie nach Abschluss der Registrierung im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Klicken Sie dann auf **OK**, um die Einstellungen für das Ziel zu konfigurieren.


Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

> [!NOTE]
> Es kann 15 Minuten oder länger dauern, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > ***Servername*** > **Server aktualisieren**.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell vom Typ „Resource Manager“ oder „klassisch“ ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Alle Ressourcen**.
2. Klicken Sie auf den Recovery Services-Tresor **ContosoVMVault**.
3. Klicken Sie zum Erstellen einer Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
4. Geben Sie unter **Replikationsrichtlinie erstellen** den Richtliniennamen **VMwareRepPolicy** an.
5. Übernehmen Sie in **RPO-Schwellenwert** den Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
6. Übernehmen Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** den Standardwert von 24 Stunden für die Dauer des Aufbewahrungszeitfensters für jeden Wiederherstellungspunkt. Für dieses Tutorial wählen wir 72 Stunden aus. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
7. Übernehmen Sie unter **Häufigkeit der Momentaufnahmen für App-Konsistenz** den Standardwert von 60 Minuten für die Häufigkeit, mit der Momentaufnahmen für die App-Konsistenz erstellt werden. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

   ![Richtlinie](./media/tutorial-vmware-to-azure/replication-policy.png)

Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. Standardmäßig wird für das Failback automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="enable-replication"></a>Aktivieren der Replikation

Site Recovery installiert den Mobility Service, wenn die Replikation für eine VM aktiviert wird. Es kann länger als 15 Minuten dauern, bis die Änderungen wirksam und im Portal angezeigt werden.

Aktivieren Sie die Replikation:

1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus.
5. Wählen Sie den Prozessserver (Konfigurationsserver) aus. Klicken Sie anschließend auf **OK**.
6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).
7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten.
8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen.
9. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
10. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.
11. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren des Mobility Service auf dem Computer verwenden soll.
12. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist.
13. Klicken Sie auf **Replikation aktivieren**.

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

Zum Überwachen der hinzugefügten VMs können Sie den Zeitpunkt der letzten Ermittlung für VMs unter **Konfigurationsserver**
> **Letzter Kontakt um** überprüfen. Wenn Sie VMs hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken) und klicken auf **Refresh** (Aktualisieren).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](site-recovery-test-failover-to-azure.md)

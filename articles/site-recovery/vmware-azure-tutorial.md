---
title: Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit Azure Site Recovery für lokale VMware-VMs die Notfallwiederherstellung in Azure einrichten.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6c86a98dd819b91608be04f1466dc1e6764ee4b9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs

In diesem Tutorial wird veranschaulicht, wie die Notfallwiederherstellung in Azure für lokale VMware-VMs unter Windows eingerichtet wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Eingeben von Replikationsquelle und -ziel
> * Einrichten der Quellreplikationsumgebung (einschließlich der lokalen Azure Site Recovery-Komponenten) und der Zielreplikationsumgebung
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

Dies ist das dritte Tutorial einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie die Aufgaben aus den vorherigen Tutorials durchgeführt haben:

* [Vorbereiten von Azure:](tutorial-prepare-azure.md) In diesem Tutorial erfahren Sie, wie Sie ein Azure-Speicherkonto und ein Netzwerk einrichten, wie Sie sicherstellen, dass Ihr Azure-Konto über die richtigen Berechtigungen verfügt, und wie Sie einen Recovery Services-Tresor erstellen.
* [Lokales Vorbereiten von VMware:](vmware-azure-tutorial-prepare-on-premises.md) In diesem Tutorial bereiten Sie Konten vor, damit Site Recovery zur Ermittlung von virtuellen Computern auf VMware-Server zugreifen kann, und führen optional eine Pushinstallation der Site Recovery Mobility-Dienstkomponente durch, wenn Sie die Replikation für einen virtuellen Computer aktivieren. Außerdem stellen Sie sicher, dass Ihre VMware-Server und virtuellen Computer die Site Recovery-Anforderungen erfüllen.

Bevor Sie beginnen, empfiehlt sich eine [Überprüfung der Architektur](vmware-azure-architecture.md) für ein Notfallwiederherstellungsszenario.


## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Wählen Sie unter **Recovery Services-Tresore** den Tresornamen **ContosoVMVault** aus.
2. Wählen Sie unter **Erste Schritte** die Option „Site Recovery“ aus. Klicken Sie anschließend auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Wählen Sie dann **OK**aus.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung


Zum Einrichten der Quellumgebung benötigen Sie einen einzelnen, hochverfügbaren, lokalen Computer, um lokale Site Recovery-Komponenten zu hosten. Die Komponenten umfassen den Konfigurationsserver, den Prozessserver und den Masterzielserver:

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.
- Der Masterzielserver verarbeitet die Replikationsdaten während des Failbacks von Azure.

Um den Konfigurationsserver als hochverfügbaren virtuellen VMware-Computer einzurichten, laden Sie eine vorbereitete OVF-Vorlage (Open Virtualization Format) herunter und importieren sie in VMware, um den virtuellen Computer zu erstellen. Registrieren Sie den eingerichteten Konfigurationsserver anschließend beim Tresor. Nach der Registrierung ermittelt Site Recovery lokale virtuelle VMware-Computer.

> [!TIP]
> In diesem Tutorial wird eine OVF-Vorlage verwendet, um den virtuellen Computer für den VMware-Konfigurationsserver zu erstellen. Sollte das nicht möglich sein, verwenden Sie die [manuelle Einrichtung](physical-manage-configuration-server.md). 


### <a name="download-the-vm-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVF-Vorlage für den Konfigurationsserver herunter.

  > [!TIP]
  Die neueste Version der Konfigurationsservervorlage können Sie direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.

## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich beim VMware vCenter-Server oder beim vSphere ESXi-Host über den VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten. 

     ![OVF-Vorlage](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie unter **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie unter **Speicher auswählen** für **Select virtual disk format** (Format für virtuellen Datenträger auswählen) die Option **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus, um eine optimale Leistung zu erzielen.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Gehen Sie unter **Bereit für den Abschluss** wie folgt vor:

    * Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

    * Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten). Klicken Sie dann auf **Fertig stellen**. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt. Weitere NICs können nach der Bereitstellung hinzugefügt werden.

## <a name="add-an-additional-adapter"></a>Hinzufügen zusätzlicher Adapter

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie diesen Schritt aus, bevor Sie den Server beim Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie anschließend auf **Weiter**.
3. Wählen Sie einen Adaptertyp und ein Netzwerk aus. 
4. Um die virtuelle NIC zu verbinden, wenn der virtuelle Computer eingeschaltet wird, wählen Sie **Connect at power on** (Beim Einschalten verbinden) aus. Klicken Sie auf **Weiter** > **Fertig stellen**. Wählen Sie dann **OK**aus.


## <a name="register-the-configuration-server"></a>Registrieren des Konfigurationsservers 

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.

### <a name="configure-settings-and-add-the-vmware-server"></a>Konfigurieren der Einstellungen und Hinzufügen des VMware-Servers

1. Klicken Sie im Assistenten für die Konfigurationsserververwaltung auf **Konnektivität einrichten**, und wählen Sie anschließend die NIC aus, die den Replikationsdatenverkehr empfangen soll. Klicken Sie dann auf **Speichern**. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
2. Wählen Sie unter **Recovery Services-Tresor auswählen** Ihr Azure-Abonnement, die entsprechende Ressourcengruppe und den entsprechenden Tresor aus.
3. Akzeptieren Sie unter **Drittanbietersoftware installieren** den Lizenzvertrag. Klicken Sie auf **Herunterladen und installieren**, um MySQL Server zu installieren.
4. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt durchführen. Klicken Sie anschließend auf **Weiter**.
5. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
6. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
7. Geben Sie die Anmeldeinformationen ein, die der Konfigurationsserver beim Herstellen der Verbindung mit dem VMware-Server verwenden soll. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**.
8. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort an, die für die automatische Installation von Mobility Service auf Computern verwendet werden sollen, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
9. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen. 
10. Überprüfen Sie nach Abschluss der Registrierung im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Klicken Sie dann auf **OK**, um die Einstellungen für das Ziel zu konfigurieren.


Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt virtuelle Computer.

> [!NOTE]
> Es kann 15 Minuten oder länger dauern, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > ***Servername*** > **Server aktualisieren**.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**. Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell auf Azure Resource Manager basiert oder klassisch ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Registerkarte „Ziel“](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Alle Ressourcen**.
2. Wählen Sie den Recovery Services-Tresor **ContosoVMVault** aus.
3. Klicken Sie zum Erstellen einer Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
4. Geben Sie unter **Replikationsrichtlinie erstellen** den Richtliniennamen **VMwareRepPolicy** ein.
5. Übernehmen Sie in **RPO-Schwellenwert** den Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
6. Übernehmen Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** den Standardwert von 24 Stunden für die Dauer des Aufbewahrungszeitfensters für jeden Wiederherstellungspunkt. Verwenden Sie für dieses Tutorial 72 Stunden. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
7. Übernehmen Sie unter **Häufigkeit der Momentaufnahmen für App-Konsistenz** den Standardwert von 60 Minuten für die Häufigkeit, mit der Momentaufnahmen für die App-Konsistenz erstellt werden. Wählen Sie **OK** aus, um die Richtlinie zu erstellen.

   ![Erstellen einer Replikationsrichtlinie](./media/vmware-azure-tutorial/replication-policy.png)

Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. Für das Failback wird standardmäßig automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="enable-replication"></a>Aktivieren der Replikation

Site Recovery installiert Mobility Service, wenn die Replikation für einen virtuellen Computer aktiviert wird. Es kann länger als 15 Minuten dauern, bis die Änderungen wirksam und im Portal angezeigt werden.

Aktivieren Sie die Replikation:

1. Klicken Sie auf **Anwendung replizieren** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus.
5. Wählen Sie den Prozessserver (Konfigurationsserver) aus. Wählen Sie dann **OK**aus.
6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. in der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).
7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten.
8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen sollen.
9. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
10. Wählen Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen** die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Wählen Sie dann **OK**aus.
11. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren von Mobility Service auf dem Computer verwenden soll.
12. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist.
13. Klicken Sie auf **Replikation aktivieren**.

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

Überprüfen Sie zur Überwachung der hinzugefügten virtuellen Computer den Zeitpunkt der letzten Ermittlung für virtuelle Computer unter **Konfigurationsserver** > **Letzter Kontakt um**. Wenn Sie virtuelle Computer hinzufügen möchten, ohne auf die geplante Ermittlung zu warten, markieren Sie den Konfigurationsserver (wählen Sie ihn aber nicht aus), und klicken Sie auf **Aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](site-recovery-test-failover-to-azure.md)

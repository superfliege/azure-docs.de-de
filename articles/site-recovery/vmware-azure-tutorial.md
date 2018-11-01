---
title: Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit Azure Site Recovery für lokale VMware-VMs die Notfallwiederherstellung in Azure einrichten.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8d7bc9b5deda946dabdcfbb8eccd25d028acfbcd
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50213065"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs

[Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.


In diesem Tutorial wird gezeigt, wie Sie mithilfe von Azure Site Recovery eine Replikation eines virtuellen VMware-Computers in Azure einrichten und aktivieren. Tutorials dienen zur Veranschaulichung der Bereitstellung von Site Recovery mit grundlegenden Einstellungen. Darin wird der einfachste Pfad verwendet, und es werden nicht alle Optionen angezeigt. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Eingeben von Replikationsquelle und -ziel
> * Einrichten der Quellreplikationsumgebung (einschließlich der lokalen Azure Site Recovery-Komponenten) und der Zielreplikationsumgebung
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

## <a name="before-you-start"></a>Vorbereitung

Bevor Sie beginnen, sind die folgenden Schritte hilfreich:

- [Überprüfen Sie die Architektur](vmware-azure-architecture.md) für dieses Notfallwiederherstellungsszenario.
- Wenn Sie ausführlichere Informationen zum Einrichten der Notfallwiederherstellung für virtuelle VMware-Computer benötigen, sehen Sie sich die folgenden Ressourcen an:
    - [Allgemeine Fragen](vmware-azure-common-questions.md) zur Notfallwiederherstellung für VMware
    - [Informationen](vmware-physical-azure-support-matrix.md) zu den unterstützten und erforderlichen Komponenten für VMware
-  In unseren **Anleitungen** finden Sie ausführliche Anweisungen zu allen Bereitstellungsoptionen für VMware:
    - Einrichten von [Replikationsquelle](vmware-azure-set-up-source.md) und [Konfigurationsserver](vmware-azure-deploy-configuration-server.md)
    - Einrichten des [Replikationsziels](vmware-azure-set-up-target.md)
    - Einrichten einer [Replikationsrichtlinie](vmware-azure-set-up-replication.md) und [Aktivieren der Replikation](vmware-azure-enable-replication.md)


## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

1. Wählen Sie unter **Recovery Services-Tresore** den Tresornamen aus. In diesem Szenario verwenden wir **ContosoVMVault**.
2. Wählen Sie unter **Erste Schritte** die Option „Site Recovery“ aus. Klicken Sie anschließend auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Wählen Sie dann **OK**aus.


## <a name="plan-your-deployment"></a>Planen der Bereitstellung

In diesem Tutorial wird veranschaulicht, wie Sie einen einzelnen virtuellen Computer replizieren. Unter **Bereitstellungsplanung** wird **Ja, ist abgeschlossen** ausgewählt. Wenn Sie mehrere virtuelle Computer bereitstellen, wird empfohlen, diesen Schritt nicht zu überspringen. Zu Ihrer Unterstützung wird das [Bereitstellungsplanertool](https://aka.ms/asr-deployment-planner) bereitgestellt. [Hier](site-recovery-deployment-planner.md) finden Sie weitere Informationen zu diesem Tool.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Im ersten Bereitstellungsschritt richten Sie Ihre Quellumgebung ein. Sie benötigen eine einzelne, hoch verfügbare, lokale VMware-VM, um lokale Site Recovery-Komponenten zu hosten. Die Komponenten umfassen den Konfigurationsserver, den Prozessserver und den Masterzielserver:

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.
- Der Masterzielserver verarbeitet die Replikationsdaten während des Failbacks von Azure.

Zum Einrichten des Konfigurationsservers als hoch verfügbaren virtuellen VMware-Computer laden Sie eine vorbereitete OVA-Vorlage (Open Virtualization Application) herunter und importieren sie in VMware, um den virtuellen Computer zu erstellen. Registrieren Sie den eingerichteten Konfigurationsserver anschließend beim Tresor. Nach der Registrierung ermittelt Site Recovery lokale virtuelle VMware-Computer.

> [!TIP]
> In diesem Tutorial wird eine OVA-Vorlage verwendet, um den virtuellen Computer für den VMware-Konfigurationsserver zu erstellen. Kann sie nicht verwendet werden, können Sie [den Konfigurationsserver manuell einrichten](physical-manage-configuration-server.md).

> [!TIP]
> In diesem Tutorial lädt Site Recovery MySQL herunter und installiert MySQL auf dem Konfigurationsserver. Falls Site Recovery diese Schritte nicht ausführen soll, können Sie die Einrichtung manuell vornehmen. [Weitere Informationen](vmware-azure-deploy-configuration-server.md#configure-settings).


### <a name="download-the-vm-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVF-Vorlage für den Konfigurationsserver herunter.

 > [!TIP]
 >Die neueste Version der Konfigurationsservervorlage können Sie direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.

>[!NOTE]
Die mit der OVF-Vorlage bereitgestellte Vorlage ist eine Evaluierungslizenz mit einer Gültigkeit von 180 Tagen. Der Kunde muss die Fenster mit einer bezogenen Lizenz aktivieren.

## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich mit dem VMWare vSphere-Client beim VMware vCenter-Server oder beim vSphere ESXi-Host an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den **Assistenten zum Bereitstellen von OVF-Vorlagen** zu starten. 

     ![OVF-Vorlage](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie unter **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie unter **Speicher auswählen** für **Select virtual disk format** (Format für virtuellen Datenträger auswählen) die Option **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus, um eine optimale Leistung zu erzielen.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Wählen Sie unter **Zum Abschluss bereit** zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

    > [!TIP]
  Wenn Sie einen weiteren Netzwerkadapter hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen**. Standardmäßig enthält die Vorlage einen einzelnen Netzwerkadapter. Weitere NICs können nach der Bereitstellung hinzugefügt werden.

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
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool innerhalb weniger Sekunden gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird innerhalb weniger Sekunden automatisch gestartet.

### <a name="configure-settings-and-add-the-vmware-server"></a>Konfigurieren der Einstellungen und Hinzufügen des VMware-Servers

1. Klicken Sie im Assistenten für die Konfigurationsserververwaltung auf **Konnektivität einrichten**, und wählen Sie anschließend die NIC aus, die der Verarbeitungsserver nutzt, um den Replikationsdatenverkehr zu empfangen. Klicken Sie dann auf **Speichern**. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
2. Wählen Sie unter **Recovery Services-Tresor auswählen** Ihr Azure-Abonnement, die entsprechende Ressourcengruppe und den entsprechenden Tresor aus.
3. Akzeptieren Sie unter **Drittanbietersoftware installieren** den Lizenzvertrag. Klicken Sie auf **Herunterladen und installieren**, um MySQL-Server zu installieren. Wenn Sie MySQL im Pfad platzieren, wird dieser Schritt übersprungen.
4. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt durchführen. Klicken Sie anschließend auf **Weiter**.
5. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
6. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
7. Geben Sie die Benutzeranmeldeinformationen ein, die der Konfigurationsserver zum Herstellen der Verbindung mit dem VMware-Server verwenden soll. Stellen Sie sicher, dass der Benutzername und das Kennwort korrekt sind und dieser Benutzer der Gruppe „Administratoren“ der zu schützenden VM angehört. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**.
8. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort an, die für die automatische Installation von Mobility Service auf virtuellen Computern verwendet werden sollen, wenn die Replikation aktiviert ist.
    - Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten.
    - Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
9. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen.
10. Überprüfen Sie nach Abschluss der Registrierung im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Klicken Sie dann auf **OK**, um die Einstellungen für das Ziel zu konfigurieren.


Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt virtuelle Computer.

> [!NOTE]
> Es kann 15 Minuten oder länger dauern, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > ***Servername*** > **Server aktualisieren**.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**. Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten. Hier wird ein Resource Manager-Modell verwendet.
2. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen. Diese sollten vorhanden sein, wenn Sie die Azure-Komponenten im [ersten Tutorial](tutorial-prepare-azure.md) in dieser Tutorialreihe eingerichtet haben.

   ![Registerkarte „Ziel“](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Alle Ressourcen**.
2. Wählen Sie den Recovery Services-Tresor (in diesem Tutorial: **ContosoVMVault**) aus.
3. Klicken Sie zum Erstellen einer Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
4. Geben Sie unter **Replikationsrichtlinie erstellen** den Richtliniennamen ein. Hier wird **VMwareRepPolicy** verwendet.
5. Übernehmen Sie in **RPO-Schwellenwert** den Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
6. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** an, wie lange die einzelnen Wiederherstellungspunkte aufbewahrt werden. Für dieses Tutorial legen wir 72 Stunden fest. Replizierte virtuelle Computer können für jeden Punkt im Aufbewahrungszeitfenster wiederhergestellt werden.
7. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig anwendungskonsistente Momentaufnahmen erstellt werden. Wir verwenden den Standardwert von 60 Minuten. Wählen Sie **OK** aus, um die Richtlinie zu erstellen.

   ![Erstellen einer Replikationsrichtlinie](./media/vmware-azure-tutorial/replication-policy.png)

- Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet.
- Für das Failback wird standardmäßig automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="enable-replication"></a>Aktivieren der Replikation

Sie können die Replikation wie folgt durchführen:

1. Klicken Sie auf **Anwendung replizieren** > **Quelle**.
2. Wählen Sie unter **Quelle** die Option **Lokal** und dann unter **Quellpfad** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vSphere-Host oder den vCenter-Server aus, der den Host verwaltet.
5. Wählen Sie den Prozessserver aus (wird standardmäßig auf dem virtuellen Computer mit dem Konfigurationsserver installiert). Wählen Sie dann **OK**aus.
6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. in der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Hier wird das Resource Manager-Bereitstellungsmodell verwendet. 
7. Wählen Sie das Azure-Speicherkonto, das zum Replizieren von Daten verwendet werden soll, sowie das Azure-Netzwerk und -Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen sollen.
8. Wählen Sie **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellungen auf alle virtuellen Computer anzuwenden, auf denen Sie die Replikation aktivieren. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
9. Wählen Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen** die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Wählen Sie dann **OK**aus.
10. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren von Mobility Service auf dem Computer verwenden soll.
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist.
12. Klicken Sie auf **Replikation aktivieren**. Site Recovery installiert den Mobility Service, wenn die Replikation für einen virtuellen Computer aktiviert wird.
13. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.
- Es kann länger als 15 Minuten dauern, bis die Änderungen wirksam und im Portal angezeigt werden.
- Überprüfen Sie zur Überwachung der hinzugefügten virtuellen Computer den Zeitpunkt der letzten Ermittlung für virtuelle Computer unter **Konfigurationsserver** > **Letzter Kontakt um**. Wenn Sie virtuelle Computer hinzufügen möchten, ohne auf die geplante Ermittlung zu warten, markieren Sie den Konfigurationsserver (wählen Sie ihn aber nicht aus), und klicken Sie auf **Aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](site-recovery-test-failover-to-azure.md)

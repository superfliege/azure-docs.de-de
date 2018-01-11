---
title: "Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die Notfallwiederherstellung in Azure für lokale VMware-VMs mit dem Azure Site Recovery-Dienst einrichten."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5810ff908d48fc4ff742d734e7c2457fdfe8cb03
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs

In diesem Tutorial wird veranschaulicht, wie die Notfallwiederherstellung in Azure für lokale VMware-VMs unter Windows eingerichtet wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Geben Sie die Replikationsquelle und das Replikationsziel an.
> * Richten Sie die Quellreplikationsumgebung, einschließlich der lokalen Komponenten zur Sitewiederherstellung, und die Zielreplikationsumgebung ein.
> * Erstellen einer Replikationsrichtlinie
> * Aktivieren der Replikation für eine VM

Dies ist das dritte Tutorial in einer Reihe. In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben:

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Lokales Vorbereiten von VMware](tutorial-prepare-on-premises-vmware.md)

Bevor Sie beginnen, empfiehlt sich eine [Überprüfung der Architektur](concepts-vmware-to-azure-architecture.md) für ein Notfallwiederherstellungsszenario.


## <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Klicken Sie unter **Recovery Services-Tresore** auf den Tresornamen, **ContosoVMVault**.
2. Klicken Sie in **Erste Schritte** auf „Site Recovery“. Klicken Sie dann auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren? die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Klicken Sie dann auf **OK**.

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Laden Sie zum Einrichten der Quellumgebung die Datei für das einheitliche Setup von Site Recovery herunter. Sie führen Setup aus, um lokale Site Recovery-Komponenten zu installieren, VMware-Server im Tresor zu registrieren und lokale VMs zu ermitteln.

### <a name="verify-on-premises-site-recovery-requirements"></a>Überprüfen lokaler Site Recovery-Anforderungen

Sie benötigen eine einzelne, hochverfügbare, lokale VMware VM, um lokale Site Recovery-Komponenten zu hosten. Die Komponenten umfassen den Konfigurationsserver, Prozessserver und Masterzielserver.

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- Der Prozessserver fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver installiert auch den Mobility Service auf VMs, die Sie replizieren möchten, und führt auf lokalen VMware VMs eine automatische Ermittlung durch.
- Der Masterzielserver verarbeitet die Replikationsdaten während des Failbacks von Azure.

Die VM muss die folgenden Anforderungen erfüllen.

| **Anforderung** | **Details** |
|-----------------|-------------|
| Anzahl von CPU-Kernen| 8 |
| RAM | 12 GB |
| Anzahl der Datenträger | 3 - Betriebssystem-Datenträger, Prozessservercache-Datenträger, Aufbewahrungslaufwerk (für Failback) |
| Freier Speicherplatz (Prozessservercache) | 600 GB |
| Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB |
| Betriebssystemversion | Windows Server 2012 R2 |
| Gebietsschema des Betriebssystems | Englisch (en-us) |
| VMware vSphere PowerCLI-Version | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Windows Server-Rollen | Aktivieren Sie nicht diese Rollen: Active Directory Domain Services, Internetinformationsdienste, Hyper-V |
| NIC-Typ | VMXNET3 |
| Art der IP-Adresse | statischen |
| Ports | 443 (Steuerkanalorchestrierung)<br/>9443 (Datentransport)|

Außerdem haben Sie folgende Möglichkeiten: 
- Stellen Sie sicher, dass die Systemuhr auf der VM mit einem Zeitserver synchronisiert ist. Die Zeit muss mit einer Toleranz von höchstens 15 Minuten synchronisiert werden. Wenn sie größer ist, tritt beim Setup ein Fehler auf.
setup fails.
- Stellen Sie sicher, dass die VM des Konfigurationsservers auf die folgenden URLs zugreifen kann:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Stellen Sie sicher, dass auf IP-Adressen basierende Firewallregeln die Kommunikation mit Azure gestatten.
    - Lassen Sie die [IP-Bereiche für Azure-Rechenzentrum](https://www.microsoft.com/download/confirmation.aspx?id=41653), Port 443 (HTTPS) und Port 9443 (Datenreplikation) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.


### <a name="download-the-site-recovery-unified-setup-file"></a>Herunterladen der Datei für das einheitliche Setup von Site Recovery

1. Klicken Sie im Tresor > **Infrastruktur vorbereiten** auf **Quelle**.
1. Klicken Sie in **Quelle vorbereiten** auf **+Konfigurationsserver**.
2. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
3. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
4. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>Einrichten des Konfigurationsservers

1. Führen Sie die Installationsdatei für das einheitliche Setup aus.
2. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie unter **Third Party Software License** (Drittanbietersoftwarelizenz) auf **I Accept** (Ich stimme zu), um MySQL herunterzuladen und zu installieren, und klicken Sie auf **Weiter**.

4. Wählen Sie unter **Registration** (Registrierung) den Registrierungsschlüssel aus, den Sie aus dem Tresor heruntergeladen haben.

5. Geben Sie unter **Internet Settings** (Interneteinstellungen) an, wie der auf dem Konfigurationsserver ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

   - Wenn die Verbindung über den derzeit auf dem Computer eingerichteten Proxy hergestellt werden soll, wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden**.
   - Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)**.
   - Falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, müssen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden** auswählen und Adresse, Port und Anmeldeinformationen angeben.

   ![Firewall](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. Bei der **Voraussetzungsüberprüfung** führt das Setup eine Überprüfung durch, um sicherzustellen, dass die Installation ausgeführt werden kann. Falls beim **Überprüfen der Synchronisierung der globalen Zeit** eine Warnung angezeigt wird, stellen Sie sicher, dass die Zeit der Systemuhr (Einstellungen für **Datum und Uhrzeit**) mit der Zeitzone übereinstimmt.

   ![Voraussetzungen](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. Erstellen Sie unter **MySQL Configuration** (MySQL-Konfiguration) Anmeldeinformationen für die Anmeldung bei der MySQL-Serverinstanz, die installiert wird.

8. Wählen Sie unter **Umgebungsdetails** **Ja** aus, um VMware-VMs zu schützen. Das Setup überprüft, ob PowerCLI 6.0 installiert ist.

9. Wählen Sie unter **Install Location** (Installationsspeicherort) aus, wo die Binärdateien installiert werden sollen und wo der Cache gespeichert werden soll. Auf dem ausgewählten Laufwerk müssen mindestens 5 GB an Speicherplatz verfügbar sind, wir raten Ihnen jedoch zu einem Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz.

10. Geben Sie unter **Network Selection** (Netzwerkauswahl) den Listener (Netzwerkadapter und SSL-Port) an, über den der Konfigurationsserver Replikationsdaten sendet und empfängt. Port 9443 ist der Standardport zum Senden und Empfangen von Replikationsdatenverkehr. Sie können diese Portnummer jedoch ändern, um sie an die Anforderungen Ihrer Umgebung anzupassen. Darüber hinaus öffnen wir Port 443, über den Replikationsvorgänge koordiniert werden. Verwenden Sie den Port 443 nicht, um Replikationsdatenverkehr zu senden oder zu empfangen.

11. Lesen Sie die unter **Summary** (Zusammenfassung) angezeigten Informationen und klicken Sie auf **Install** (Installieren). Das Setup installiert den Konfigurationsserver und registriert ihn beim Dienst Azure Site Recovery.

    ![Zusammenfassung](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Nach Abschluss der Installation wird eine Passphrase generiert. Diese benötigen Sie bei der Aktivierung der Replikation. Kopieren Sie sie deshalb, und bewahren Sie sie an einem sicheren Ort auf. Der Server wird im Bereich **Einstellungen** > **Server** im Tresor angezeigt.

### <a name="configure-automatic-discovery"></a>Konfigurieren der automatischen Ermittlung

Zum Ermitteln von VMs muss der Konfigurationsserver eine Verbindung mit lokalen VMware-Servern herstellen. Fügen Sie für die Zwecke dieses Tutorials den vCenter-Server oder die vSphere-Hosts hinzu; verwenden Sie dabei ein Konto mit Administratorrechten auf dem Server. Sie haben dieses Konto im [vorherigen Tutorial](tutorial-prepare-on-premises-vmware.md) erstellt. 

So fügen Sie das Konto hinzu

1. Starten Sie auf der VM des Konfigurationsservers die Datei **CSPSConfigtool.exe**. Diese finden Sie als Verknüpfung auf dem Desktop sowie unter „*Installationsort*\home\svsystems\bin“.

2. Klicken Sie auf **Konten verwalten** > **Konto hinzufügen**.

   ![Konto hinzufügen](./media/tutorial-vmware-to-azure/credentials1.png)

3. Fügen Sie unter **Account Details** (Kontodetails) das Konto hinzu, das für die automatische Ermittlung verwendet werden wird.

   ![Details](./media/tutorial-vmware-to-azure/credentials2.png)

So fügen Sie den VMware-Server hinzu

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Alle Ressourcen**.
2. Klicken Sie auf den Recovery Services-Tresor **ContosoVMVault**.
3. Klicken Sie auf **Site Recovery** > **Infrastruktur vorbereiten** > **Quelle**.
4. Wählen Sie **+vCenter** aus, um eine Verbindung mit einem vCenter-Server oder vSphere ESXi-Host herzustellen.
5. Geben Sie unter **vCenter-Server hinzufügen** einen Anzeigenamen für den Server an. Geben Sie anschließend die IP-Adresse oder den FQDN ein.
6. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie Anforderungen an einem anderen Port überwachen können.
7. Wählen Sie das Konto aus, mit dem Verbindungen mit dem Server hergestellt werden sollen. Klicken Sie auf **OK**.

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

## <a name="enable-replication"></a>Replikation aktivieren

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

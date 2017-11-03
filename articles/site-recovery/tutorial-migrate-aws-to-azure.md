---
title: Migrieren von VMs aus AWS zu Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie in Amazon Web Services (AWS) ausgeführte VMs mithilfe von Azure Site Recovery zu Azure migrieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6a10cbf1d3c8043ddcac9a725373fc30a3ecee20
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrieren von AWS-VMs (Amazon Web Services) zu Azure

In diesem Tutorial erfahren Sie, wie Sie virtuelle AWS-Computer (Amazon Web Services-VMs) mithilfe von Site Recovery zu Azure-VMs migrieren. Beim Migrieren von EC2-Instanzen zu Azure werden die VMs wie physische lokale Computer behandelt. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten der Azure-Ressourcen
> * Vorbereiten der AWS EC2-Instanzen für die Migration
> * Bereitstellen eines Konfigurationsservers
> * Aktivieren der Replikation für VMs
> * Ausführen eines Testfailover aus, um die ordnungsgemäße Funktion sicherzustellen
> * Ausführen eines einmaligen Failovers in Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure-resources"></a>Vorbereiten der Azure-Ressourcen 

Sie müssen einige wenige Ressourcen Azure vorbereitet haben, um die migrierten EC2-Instanzen verwenden zu können. Hierzu zählen ein Speicherkonto, ein Tresor und ein virtuelles Netzwerk.

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Images der replizierten Computer sind in Azure Storage gespeichert. Azure-VMs werden aus dem Speicher erstellt, wenn Sie von Failover von einem lokalen Standort nach Azure ausführen.

1. Klicken Sie im Menü des [Azure-Portals](https://portal.azure.com) auf **Neu** -> **Storage** -> **Speicherkonto**.
2. Geben Sie einen Namen für Ihr Speicherkonto ein. Für diese Tutorials verwenden wir den Namen **awsmigrated2017**. Der Name muss in Azure eindeutig und zwischen 3 und 24 Zeichen lang sein, wobei nur Ziffern und Kleinbuchstaben zulässig sind.
3. Behalten Sie die Standardwerte für **Bereitstellungsmodell**, **Kontoart**, **Leistung** und **Sichere Übertragung erforderlich** bei.
5. Wählen Sie den **RA-GRS**-Standardwert für **Replikation** aus.
6. Wählen Sie das Abonnement aus, das Sie für dieses Tutorial verwenden möchten.
7. Wählen Sie für **Ressourcengruppe** die Option **Neu erstellen** aus. In diesem Beispiel geben wir den Namen **migrationRG** an.
8. Wählen Sie **Europa, Westen** als Ort aus. 
9. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

### <a name="create-a-vault"></a>Erstellen eines Tresors

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Weitere Dienste**, suchen Sie nach dem Eintrag **Recovery Services-Tresore**, und wählen Sie ihn aus.
2. Klicken Sie auf der Seite „Recovery Services-Tresore“ oben links auf **+ Hinzufügen**.
3. Geben Sie in **Name** den Namen *myVault* ein. 
4. Wählen Sie für **Abonnement** das entsprechende Abonnement aus.
4. Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** aus, und wählen Sie *migrationRG* aus. 
5. Wählen Sie als **Ort** die Option *Europa, Westen* aus. 
5. Klicken Sie zum schnellen Zugreifen auf den neuen Tresor über das Dashboard auf **An Dashboard anheften**.
7. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf der Hauptseite **Recovery Services-Tresore** angezeigt.

### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Wenn die Azure-VMs nach der Migration (Failover) erstellt werden, werden sie mit diesem Netzwerk verknüpft.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Neu** > **Netzwerk** >
   **Virtuelles Netzwerk**
3. Geben Sie für **Name** den Namen *myMigrationNetwork* ein.
4. Übernehmen Sie den Standardwert für **Adressbereich**.
5. Wählen Sie für **Abonnement** das entsprechende Abonnement aus.
6. Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** aus, und wählen Sie in der Dropdownliste *migrationRG* aus.
7. Wählen Sie für **Ort** die Option **Europa, Westen** aus. 
8. Behalten Sie die Standardwerte für **Subnetz** bei (**Name** und **IP-Bereich**).
9. Lassen Sie **Dienstendpunkte** deaktiviert.
10. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.


## <a name="prepare-the-ec2-instances"></a>Vorbereiten der EC2-Instanzen

Sie benötigen mindestens eine VM, die migriert werden soll. Auf dieser EC2-Instanz muss die 64-Bit-Version von Windows Server 2008 R2 SP1 oder höher, Windows Server 2012, Windows Server 2012 R2 oder Red Hat Enterprise Linux 6.7 (nur virtualisierte HVM-Instanzen) ausgeführt werden. Der Server darf nur über Citrix PV- oder AWS PV-Treiber verfügen. Instanzen, auf denen RedHat PV-Treiber ausgeführt werden, werden nicht unterstützt.

Der Mobilitätsdienst muss auf jeder VMs installiert sein, den Sie replizieren möchten. Site Recovery installiert diesen Dienst automatisch, wenn Sie die Replikation für die VM aktivieren. Für die automatische Installation müssen Sie ein Konto für die EC2-Instanzen vorbereiten, mit dem Site Recovery auf die VM zugreift.

Sie können ein Domänenkonto oder ein lokales Konto verwenden. Bei Linux-VMs muss auf dem Linux-Quellserver das root-Konto verwendet werden. Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie für Windows-VMs die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer:

  - Fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** hinzu, und legen Sie den Wert auf 1 fest.
    
Zudem benötigen Sie eine separate EC2-Instanz, die Sie als Site Recovery-Konfigurationsserver verwenden können. Auf dieser Instanz muss Windows Server 2012 R2 ausgeführt werden. 
    

## <a name="prepare-the-infrastructure"></a>Vorbereiten der Infrastruktur 

Wählen Sie auf der Portalseite für den Tresor im Abschnitt **Erste Schritte** die Option **Site Recovery** aus, und klicken Sie dann auf **Infrastruktur vorbereiten**.

### <a name="1-protection-goal"></a>1 Schutzziel

Wählen Sie auf der Seite **Schutzziel** die folgenden Werte aus:
   
|    |  | 
|---------|-----------|
| Wo befinden sich die Computer? | **Lokal**|
| Wohin möchten Sie Ihre Computer replizieren? |**In Azure**|
| Sind Ihre Computer virtualisiert? | **Nicht virtualisiert / Andere**|

Klicken Sie anschließend auf **OK** , um zum nächsten Abschnitt zu wechseln.

### <a name="2-source-prepare"></a>2 Quelle, Vorbereiten 

Klicken Sie auf der Seite **Quelle vorbereiten** auf **+ Konfigurationsserver**. 

1. Verwenden Sie eine EC2-Instanz unter Windows Server 2012 R2, um einen Konfigurationsserver zu erstellen, und registrieren Sie ihn bei Ihrem Recovery-Tresor.

2. Konfigurieren Sie den Proxy auf der VM mit der EC2-Instanz, die Sie als Konfigurationsserver verwenden, sodass der Zugriff auf die [Dienst-URLs](site-recovery-support-matrix-to-azure.md) möglich ist.

3. Laden Sie das Programm [Microsoft Azure Site Recovery Unified Setup](http://aka.ms/unifiedinstaller_wus) (Einheitliches Setup von Microsoft Azure Site Recovery) herunter. Sie können es auf den lokalen Computer herunterladen und anschließend auf die VM kopieren, die als Konfigurationsserver verwendet wird.

4. Klicken Sie auf die Schaltfläche **Herunterladen**, um den Schlüssel für die Tresorregistrierung herunterzuladen. Kopieren Sie die heruntergeladene Datei auf die VM, die Sie als Konfigurationsserver verwenden.

5. Klicken Sie auf der VM mit der rechten Maustaste auf das heruntergeladene Installationsprogramm für das **einheitliche Setup von Microsoft Azure Site Recovery**, und wählen Sie **Als Administrator ausführen** aus. 

    1. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus, und klicken Sie auf **Weiter**.
    2. Wählen Sie unter **Third-Party Software License** (Drittanbietersoftware-Lizenz) **I accept the third-party license agreement** (Ich akzeptiere die Drittanbieter-Lizenzvereinbarung) aus. Klicken Sie anschließend auf **Weiter**.
    3. Klicken Sie unter **Registrierung** auf die Schaltfläche zum Durchsuchen, und navigieren Sie zum Speicherort für die Tresorregistrierungsschlüssel-Datei, und klicken Sie auf **Weiter**.
    4. Wählen Sie in **Interneteinstellungen** die Option **Connect to Azure Site Recovery without a proxy server** (Ohne Proxyserver mit Azure Site Recovery verbinden) aus. Klicken Sie anschließend auf **Weiter**.
    5. Auf der Seite **Prüfung der erforderlichen Komponenten** werden verschiedene Komponenten überprüft. Klicken Sie nach Abschluss des Vorgangs auf **Weiter**. 
    6. Geben Sie in **MySQL Configuration** (MySQL-Konfiguration) die erforderlichen Kennwörter ein, und klicken Sie anschließend auf **Weiter**.
    7. Wählen Sie in **Umgebungsdetails** die Option **No, you don't need to protect VMware machines** (Nein, VMware-Computer müssen nicht geschützt werden) aus, und klicken Sie auf **Weiter**.
    8. Klicken Sie unter **Installationspfad** auf **Weiter**, um den Standardwert zu übernehmen.
    9. Klicken Sie unter **Netzwerkauswahl** auf **Weiter**, um den Standardwert zu übernehmen.
    10. Klicken Sie auf der Seite **Zusammenfassung** auf **Installieren**.
    11. In **Installationsfortschritt** werden Informationen zum aktuellen Stand des Installationsvorgangs angezeigt. Klicken Sie nach Abschluss des Vorgangs auf **Fertig stellen**. Ein Popup wird geöffnet, in dem auf darauf hingewiesen wird, dass möglicherweise ein Neustart erforderlich ist. Klicken Sie auf **OK**. Zudem wird ein Popup zur Passphrase für die Verbindung mit dem Konfigurationsserver geöffnet. Konfigurieren Sie die Passphrase in die Zwischenablage, und speichern Sie sie an einem sicheren Ort.
    
6. Führen Sie auf der VM die Datei **cspsconfigtool.exe** aus, um mindestens ein Verwaltungskonto auf dem Konfigurationsserver zu erstellen. Vergewissern Sie sich, dass die Verwaltungskonten über Administratorrechte auf den EC2-Instanzen verfügen, die migriert werden sollen. 

Wenn Sie die Einrichtung des Konfigurationsservers abgeschlossen haben, wechseln Sie zurück zum Portal. Wählen Sie für **Konfigurationsserver** den soeben erstellten Server aus, und klicken Sie auf *OK**, um mit „Schritt 3: Ziel, Vorbereiten“ fortzufahren.

### <a name="3-target-prepare"></a>3 Ziel, Vorbereiten 

In diesem Abschnitt geben Sie Informationen zu den Ressourcen ein, die Sie beim Durcharbeiten des Abschnitts [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) weiter oben in diesem Tutorial erstellt haben.

1. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie für das Tutorial [Vorbereiten von Azure](tutorial-prepare-azure.md) verwendet haben.
2. Wählen Sie als Bereitstellungsmodell die Option **Resource Manager** aus.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen. Hierbei muss es sich um die Ressourcen handeln, die Sie beim Durcharbeiten des Abschnitts [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) weiter oben in diesem Tutorial erstellt haben.
4. Klicken Sie anschließend auf **OK**.


### <a name="4-replication-settings-prepare"></a>4 Replikationseinstellungen, Vorbereiten 

Vor dem Aktivieren der Replikation müssen Sie eine Replikationsrichtlinie erstellen.

1. Klicken Sie auf **+ Replicate and Associate** (+ Replizieren und Zuordnen).
2. Geben Sie im Feld **Name** den Namen **myReplicationPolicy** ein.
3. Übernehmen Sie die übrigen Standardeinstellungen, und klicken Sie auf **OK**, um die Richtlinie zu erstellen. Die neue Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

### <a name="5-deployment-planning-select"></a>5 Bereitstellungsplanung, Auswählen 

Wählen Sie in **Haben Sie die Bereitstellungsplanung abgeschlossen?** in der Dropdownliste die Option **Wird später durchgeführt** aus, und klicken Sie auf **OK**.

Wenn Sie alle fünf Abschnitte von **Infrastruktur vorbereiten** durchgearbeitet haben, klicken Sie auf **OK**.


## <a name="enable-replication"></a>Replikation aktivieren

Aktivieren Sie die Replikation für jede VM, die migriert werden soll. Wenn die Replikation aktiviert ist, wird der Mobility Service von Site Recovery automatisch installiert. 

1. Öffnen Sie das [Azure-Portal](htts://portal.azure.com).
1. Klicken Sie auf der Seite für Ihren Tresor unter **Erste Schritte** auf **Site Recovery**.
2. Klicken Sie unter **Für lokale Computer und Azure-VMs** auf **Schritt 1: Replizieren Sie die Anwendung**. Geben Sie auf den Seiten des Assistenten die folgenden Informationen, und klicken Sie anschließend auf jeder Seite auf **OK**:
    - 1 Quelle, Konfigurieren:
      
    |  |  |
    |-----|-----|
    | Quelle: | **Lokal**|
    | Quellpfad:| Der Name der Konfigurationsserver-EC2-Instanz.|
    |Computertyp: | **Physische Computer**|
    | Prozessserver: | Wählen Sie den Konfigurationsserver in der Dropdownliste aus.|
    
    - 2 Ziel, Konfigurieren
        
    |  |  |
    |-----|-----|
    | Ziel: | Übernehmen Sie den Standardwert.|
    | Abonnement: | Wählen Sie das Abonnement aus, das Sie verwendet haben.|
    | Ressourcengruppe nach Failover:| Verwenden Sie die Ressourcengruppe, die Sie im Abschnitt [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) erstellt haben.|
    | Bereitstellungsmodell nach dem Failover: | Wählen Sie **Ressourcen-Manager** aus.|
    | Speicherkonto: | Wählen Sie das Speicherkonto aus, das Sie im Abschnitt [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) erstellt haben.|
    | Azure-Netzwerk: | Wählen Sie **Jetzt für die ausgewählten Computer konfigurieren** aus.|
    | Azure-Netzwerk nach Failover: | Wählen Sie das Netzwerk aus, das Sie im Abschnitt [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) erstellt haben.|
    | Subnetz: | Wählen Sie in der Dropdownliste den **Standardwert** aus.|
    
    - 3 Physische Computer, Auswählen
        
        Klicken Sie auf **+ Physischer Computer**, geben Sie anschließend **Name**, **IP-Adresse** und **Betriebssystemtyp** der zu migrierenden EC2-Instanz ein, und klicken Sie dann auf **OK**.
        
    - 4 Eigenschaften, Eigenschaften konfigurieren
        
        Wählen Sie in der Dropdownliste das Konto aus, das Sie für den Konfigurationsserver erstellt haben, und klicken Sie auf **OK**.
        
    - 5 Replikationseinstellungen, Replikationseinstellungen konfigurieren
    
        Vergewissern Sie sich, dass in der Dropdownliste die Replikationsrichtlinie **myReplicationPolicy** ausgewählt ist, und klicken Sie auf **OK**.
        
3. Klicken Sie nach Abschluss des Assistenten auf **Replikation aktivieren**.
        

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Überwachung und Berichte** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.        
        
Wenn Sie die Replikation für eine VM aktivieren, kann es länger als 15 Minuten dauern, bis die Änderungen wirksam und im Portal angezeigt werden.

## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

Beim Durchführen eines Testfailovers geschieht Folgendes:

1. Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für ein Failover erfüllt sind.
2. Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
3. Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

Führen Sie das Testfailover im Portal wie folgt aus:

1. Wechseln Sie auf der Seite für Ihren Tresor zu **Geschützte Elemente** > **Replizierte Elemente**> klicken Sie auf die VM > **+ Testfailover**.

2. Wählen Sie einen Wiederherstellungspunkt für das Failover aus:
    - **Letzte Verarbeitung**: Führt ein Failover der VM auf den letzten Wiederherstellungspunkt aus, der von Site Recovery verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten aufgewendet, und der RTO-Wert (Recovery Time Objective) wird niedrig gehalten.
    - **Letzte App-Konsistenz**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
    - **Benutzerdefiniert**: Wählen Sie einen beliebigen Wiederherstellungspunkt aus.
3. Wählen Sie unter **Testfailover** das Azure-Zielnetzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden. Dies sollte das Netzwerk sein, das Sie im Abschnitt [Vorbereiten der Azure-Ressourcen](#prepare-azure-resources) erstellt haben.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt des Vorgangs verfolgen, indem Sie auf die VM klicken, um ihre Eigenschaften zu öffnen. Sie können auch auf den Auftrag **Testfailover** für den Tresor in **Überwachung und Berichte** > **Aufträge** >
   **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Vergewissern Sie sich, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Sie sollten nun eine Verbindung mit der replizierten VM in Azure herstellen können.
7. Klicken Sie zum Löschen der während des Testfailovers erstellten Azure-VMs im Wiederherstellungsplan auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. 


## <a name="migrate-to-azure"></a>Migrieren zu Azure

Führen Sie ein tatsächliches Failover für die EC2-Instanzen durch, um sie zu Azure-VMs zu migrieren. 

1. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** auf die AWS-Instanzen > **Failover**.
2. Wählen Sie unter **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Wählen Sie den letzten Wiederherstellungspunkt aus.
3. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Überprüfen Sie, ob die VM in **Replizierte Elemente** angezeigt wird. 
5. Klicken Sie mit der rechten Maustaste auf jede VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen, die Replikation für die AWS-VM wird beendet, und die Site Recovery-Abrechnung für die VM wird eingestellt.

    ![Abschließen der Migration](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.  


    

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema wurde erläutert, wie AWS EC2-Instanzen zu Azure-VMs migriert werden. Wenn Sie mehr über Azure-VMs erfahren möchten, fahren Sie mit den Tutorials für Windows-VMs fort.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](../virtual-machines/windows/tutorial-manage-vm.md)

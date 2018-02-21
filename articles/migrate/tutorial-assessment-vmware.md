---
title: "Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie lokale virtuelle VMware-Computer für die Migration zu Azure mithilfe des Azure Migrate-Diensts ermittelt und bewertet werden."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bbcfe95f5427681f8d55d647b102d35fc37f15ee
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure

Mit dem [Azure Migrate](migrate-overview.md)-Dienst werden lokale Workloads für die Migration zu Azure bewertet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Migrate-Projekts
> * Einrichten eines lokalen virtuellen Collectorcomputers zum Ermitteln lokaler virtueller VMware-Computer für die Bewertung
> * Gruppieren virtueller Computer und Erstellen einer Bewertung


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- **VMware**: Die virtuellen Computer, die Sie migrieren möchten, müssen von einem vCenter Server mit der Version 5.5, 6.0 oder 6.5 verwaltet werden. Zusätzlich benötigen Sie einen ESXi-Host mit der Version 5.0 oder höher, um die Collector-VM bereitzustellen. 
 
> [!NOTE]
> Für die Unterstützung für Hyper-V, die bald verfügbar sein wird, wurde eine Roadmap erstellt. 

- **vCenter Server-Konto**: Sie benötigen ein schreibgeschütztes Konto, um auf den vCenter Server zugreifen zu können. Dieses Konto wird in Azure Migrate zum Ermitteln der lokalen virtuellen Computer verwendet.
- **Berechtigungen:** Auf dem vCenter Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format. 
- **Statistikeinstellungen:** Die Statistikeinstellungen für den vCenter Server müssen vor der Bereitstellung auf Ebene 3 festgelegt werden. Bei einer niedrigeren Ebene als Ebene 3 wird die Bewertung zwar ausgeführt, die Leistungsdaten für den Speicher und das Netzwerk werden jedoch nicht erfasst. Die Größenempfehlungen in diesem Fall werden auf der Grundlage von Leistungsdaten für CPU und Arbeitsspeicher und Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt. 

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie den Dienst **Azure Migrate (Vorschau)** in den Suchergebnissen aus. Klicken Sie dann auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie die Region an, in der das Projekt erstellt werden soll, und klicken Sie dann auf **Erstellen**. In dieser Vorschauversion können Sie ein Azure Migrate-Projekt nur in der Region „USA, Westen-Mitte“ erstellen. Sie können Ihre Migration jedoch trotzdem für jeden beliebigen Zielort von Azure planen. Der für das Projekt angegebene Standort wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf den lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie in **Computer ermitteln** auf **Herunterladen**, um die OVA-Datei herunterzuladen.
3. Kopieren Sie in **Projektanmeldeinformationen kopieren** die Projekt-ID und den Projektschlüssel. Diese benötigen Sie beim Konfigurieren des Collectors.

    ![Herunterladen der OVA-Datei](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss folgenden Einstellungen entsprechen.
    
    Für OVA-Version 1.0.8.49:
    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Für OVA-Version 1.0.8.40:

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Erstellen der Collector-VM

Importieren Sie die heruntergeladene Datei auf den vCenter Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. Geben Sie im Assistenten „Deploy OVF Template“ (OVF-Vorlage bereitstellen) > **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Speicherort** einen Anzeigenamen für den virtuellen Collectorcomputer und das Inventarobjekt an, in dem der virtuelle Computer gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf dem der virtuelle Collectorcomputer ausgeführt wird.
7. Geben Sie im Speicher das Speicherziel für den virtuellen Collectorcomputer an.
8. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
9. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können. 
10. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.

## <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Einstellungen für die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.
4. Öffnen Sie im Azure Migrate-Collector die Option **Erforderliche Komponenten einrichten**.
    - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.
    - Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.

    > [!NOTE]
    > Die Proxyadresse muss im Format http://ProxyIPAddress oder http://ProxyFQDN eingegeben werden. Es werden nur HTTP-Proxys unterstützt.

    - Der Collector überprüft, ob der Collector-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.
    - Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.

5. Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Sammlungsbereich** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1000 virtuelle Computer umfassen. 

6. Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
7. Überwachen Sie in **Sammlungsfortschritt anzeigen** die Ermittlung, und vergewissern Sie sich, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.

> [!NOTE]
> Der Collector unterstützt nur „Englisch (USA)“ als Sprache des Betriebssystems und die Sprache der Collectorschnittstelle. Die Unterstützung für weitere Sprachen ist in Kürze verfügbar.


### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Ermittlungszeit hängt von der Anzahl der ermittelten virtuellen Computer ab. Normalerweise dauert der Abschluss der Ermittlung bei 100 virtuellen Computern ungefähr eine Stunde, nachdem der Collector ausgeführt wurde. 

1. Klicken Sie im Migration Planner-Projekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer im Portal angezeigt werden.


## <a name="create-and-view-an-assessment"></a>Erstellen und Anzeigen einer Bewertung

Nach der Ermittlung der virtuellen Computer gruppieren Sie sie und erstellen eine Bewertung. 

1. Klicken Sie auf der Seite **Übersicht** des Projekts auf **+ Bewertung erstellen**.
2. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.
3. Erstellen Sie die Gruppe, und geben Sie einen Gruppennamen an.
4. Wählen Sie die Computer aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe und die Bewertung zu erstellen.
6. Zeigen Sie die Bewertung nach der Erstellung in **Übersicht** > **Dashboard** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.

### <a name="sample-assessment"></a>Beispielbewertung

Hier ist ein Beispiel für einen Bewertungsbericht. Er enthält Informationen über die Kompatibilität der virtuellen Computer mit Azure sowie über die geschätzten monatlichen Kosten. 

![Bewertungsbericht](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-Bereitschaft

In dieser Ansicht wird der Bereitschaftsstatus für jeden Computer angezeigt.

- Für virtuelle Computer, die bereit sind, empfiehlt Azure Migrate eine VM-Größe in Azure.
- Für virtuelle Computer, die nicht bereit sind, wird in Azure Migrate der entsprechende Grund erläutert, und es werden Schritte zur Behebung angegeben.
- Azure Migrate schlägt Tools vor, die für die Migration verwendet werden können. Wenn der Computer für eine Lift & Shift-Migration geeignet ist, wird der Azure Site Recovery-Dienst empfohlen. Wenn es sich um einen Datenbankcomputer handelt, wird der Azure Database Migration Service empfohlen.

  ![Bewertungsbereitschaft](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Geschätzte monatliche Kosten

Diese Ansicht zeigt die gesamten Compute- und Speicherkosten für den Betrieb der virtuellen Computer in Azure zusammen mit den Details für die einzelnen Computer. Kostenschätzungen werden anhand der Empfehlungen für die leistungsbasierte Größe eines Computers und der zugehörigen Datenträger und anhand der Bewertungseigenschaften berechnet. 

> [!NOTE]
> Die Kostenschätzung von Azure Migrate bezieht sich auf die Ausführung der lokalen virtuellen Computer als virtuelle Azure IaaS-Computer (Infrastructure-as-a-Service). Darin werden keine PaaS- (Platform-as-a-Service) oder SaaS-Kosten (Software-as-a-Service) berücksichtigt. 

Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert. 

![Bewertung der Kosten für die virtuellen Computer](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Sie können einen Drilldown ausführen, um die Details für einen bestimmten Computer anzuzeigen.

![Bewertung der Kosten für die virtuellen Computer](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen](how-to-scale-assessment.md) zum Ermitteln und Bewerten einer umfangreichen VMware-Umgebung
- Informationen zum Erstellen vertrauenswürdiger Bewertungsgruppen mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md)
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen

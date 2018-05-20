---
title: Bewerten von lokalen Workloads für die Migration zu Azure mit DMA und Azure Migrate | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure auf die Migration von lokalen Computern vorbereiten, indem Sie den Datenmigrations-Assistenten (DMA) und den Azure Migrate-Dienst verwenden.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: fb102cc43c6e1d17afaa78a2833ae447600a96af
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="scenario-1-assess-on-premises-workloads-for-migration-to-azure"></a>Szenario 1: Bewerten von lokalen Workloads für die Migration zu Azure

Das Unternehmen Contoso erwägt die Migration zu Azure und möchte anhand einer technischen und finanziellen Bewertung ermitteln, ob seine lokalen Workloads für die Migration zur Cloud geeignet sind. Insbesondere soll die Kompatibilität von Computern und Datenbanken für die Migration bewertet werden, und es sollen die Kapazität und die Kosten zum Ausführen der Ressourcen in Azure geschätzt werden.

Als Einstieg und zum besseren Verständnis der beteiligten Technologie bewertet und migriert das Unternehmen eine kleine lokale Reise-App. Es handelt sich um eine App mit zwei Ebenen, wobei eine Web-App auf der einen VM und eine SQL Server-Datenbank auf der zweiten VM ausgeführt wird. Die Anwendung wird in VMware bereitgestellt, und die Umgebung wird mit einem vCenter Server verwaltet. Die Bewertung wird mit dem Datenmigrations-Assistenten (DMA) und dem Azure Migrate-Dienst durchgeführt. Wenn Sie dieses Szenario mit dieser anschaulichen Reise-App ausprobieren möchten, können Sie es von [GitHub](https://github.com/Microsoft/SmartHotel360) herunterladen.

**Technologie** | **Beschreibung** | **Kosten**
--- | --- | ---
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Der DMA bewertet und erkennt Kompatibilitätsprobleme, die sich auf die Datenbankfunktionalität in Azure auswirken können. Außerdem wird die Featureparität zwischen Ihrer SQL Server-Quelle und dem dazugehörigen Ziel bewertet, und für die Zielumgebung werden Verbesserungen in Bezug auf die Leistung und Zuverlässigkeit empfohlen. | Sie können das Tool kostenlos herunterladen. 
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Der Dienst ermöglicht es Ihnen, lokale Computer für die Migration zu Azure zu bewerten. Die Migrationseignung der Computer wird bewertet, und es werden Größenanpassungs- und Kostenschätzungen für die Ausführung in Azure bereitgestellt. Derzeit kann der Azure Migrate-Dienst lokale VMware-VMs für die Migration zu Azure bewerten. | Für die Nutzung dieses Diensts werden derzeit (April 2018) keine Gebühren berechnet.
[Dienstzuordnung](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Für Azure Migrate wird eine Dienstzuordnung verwendet, um Abhängigkeiten zwischen Computern anzuzeigen, die Sie migrieren möchten. |  Die Dienstzuordnung ist Teil von Azure Log Analytics. Sie kann derzeit 180 Tage lang genutzt werden, ohne dass Kosten berechnet werden. 

Für dieses Szenario laden wir den DMA herunter und führen ihn aus, um die lokale SQL Server-Datenbank für die Reise-App zu bewerten. Wir verwenden Azure Migrate mit Zuordnung von Abhängigkeiten, um die App-VMs zu bewerten, bevor wir sie zu Azure migrieren.

> [!NOTE]
> Für dieses Szenario lautet unser Bewertungsziel für die Datenbank „SQL Server auf einer Azure-VM“. Im nächsten Szenarioartikel führen wir dann die Migration zu einer verwalteten Azure SQL-Instanz durch. Wir nutzen diesen Ansatz, da die Bewertung eines Ziels vom Typ „Verwaltete Azure SQL-Instanz“ vom DMA derzeit nicht unterstützt wird.

## <a name="architecture"></a>Architecture


![Architektur für Migrationsbewertung](./media/migrate-scenarios-assessment/migration-assessment-architecture.png)

Szenario:
- Contoso ist ein fiktiver Name für ein typisches Unternehmen. Contoso möchte seine zweistufige lokale Reise-App untersuchen und migrieren.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) mit einem lokalen Domänencontroller (**contosodc1**).
- Die interne Reise-App ist auf zwei VMs (**WEBVM** und **SQLVM**) verteilt und befindet sich auf dem VMware ESXi-Host **contosohost1.contoso.com**.
- Die VMware-Umgebung wird mit einem vCenter Server (**vcenter.contoso.com**) verwaltet, der auf einer VM ausgeführt wird.




## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung dieses Szenarios benötigen Sie Folgendes:

- Einen lokalen vCenter Server mit Version 5.5, 6.0 oder 6.5.
- Ein Konto mit Lesezugriff für den vCenter Server bzw. Berechtigungen für die Kontoerstellung. 
- Berechtigungen zum Erstellen einer VM auf dem vCenter Server mit einer .OVA-Vorlage.
- Mindestens einen ESXi-Host mit Version 5.0 oder höher.
- Mindestens zwei lokale VMware-VMs (mit Ausführung einer SQL Server-Datenbank auf einer VM).
- Sie sollten auf jeder VM über die Berechtigungen zum Installieren von Azure Migrate-Agents verfügen.
- Die VMs sollten über eine direkte Internetverbindung verfügen.
        - Sie können den Internetzugriff auf die [erforderlichen URLs](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) beschränken.
        - Falls Sie über Computer ohne Internetverbindung verfügen, ist es erforderlich, dafür das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) herunterzuladen und zu installieren.
- FQDN der VM, auf der die SQL Server-Instanz ausgeführt wird, für die Datenbankbewertung.
- Für die Windows Firewall auf der SQL Server-VM sollten externe Verbindungen über TCP-Port 1433 (Standard) zugelassen sein, damit der DMA eine Verbindung herstellen kann.


## <a name="scenario-overview"></a>Übersicht über das Szenario

Wir gehen wie folgt vor:


> [!div class="checklist"]
> * **Schritt 1: Vorbereiten von Azure** Hierfür benötigen wir nur ein Azure-Abonnement.
> * **Schritt 2: Herunterladen und Installieren des DMA**: Bereiten Sie den DMA für die Bewertung der lokalen SQL Server-Datenbank vor.
> * **Schritt 3: Bewerten der Datenbank**: Führen Sie die Datenbankbewertung durch, und analysieren Sie sie.
> * **Schritt 4: Vorbereiten der VM-Bewertung mit Azure Migrate**: Richten Sie lokale Konten ein, und optimieren Sie die VMware-Einstellungen.
> * **Schritt 5: Ermitteln von lokalen VMs**: Erstellen Sie eine Azure Migrate-Collector-VM. Führen Sie den Collector anschließend aus, um die VMs für die Bewertung zu ermitteln.
> * **Schritt 6: Vorbereiten der Abhängigkeitsanalyse**: Installieren Sie Azure Migrate-Agents auf den VMs, damit die Zuordnung der Abhängigkeiten zwischen VMs angezeigt wird.
> * **Schritt 7: Bewerten der VMs**: Überprüfen Sie die Abhängigkeiten, gruppieren Sie die VMs, und führen Sie die Bewertung durch. Nachdem die Bewertung abgeschlossen wurde, können Sie sie als Vorbereitung für die Migration analysieren.


## <a name="step-1-prepare-azure"></a>Schritt 1: Vorbereiten von Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.

- Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.
- Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ für das Abonnement zuweist (bzw. für die Ressourcengruppe, die Sie für dieses Szenario verwenden).


## <a name="step-2-download-and-install-the-dma"></a>Schritt 2: Herunterladen und Installieren des DMA

1. Laden Sie den DMA aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) herunter.
    - Sie können den Assistenten auf jedem Computer installieren, mit dem eine Verbindung mit der SQL-Instanz hergestellt werden kann. Es ist nicht erforderlich, dass Sie ihn auf dem SQL Server-Computer ausführen.
    - Sie sollten ihn aber nicht auf dem SQL Server-Hostcomputer ausführen.
2. Doppelklicken Sie auf die heruntergeladene Setupdatei (DownloadMigrationAssistant.msi), um die Installation zu starten.
3. Stellen Sie auf der Seite **Fertig stellen** sicher, dass **Launch Microsoft Data Migration Assistant** (Datenmigrations-Assistent starten) ausgewählt ist, und klicken Sie auf **Fertig stellen**.

## <a name="step-3-run-and-analyze-the-database-assessment"></a>Schritt 3: Ausführen und Analysieren der Datenbankbewertung

Führen Sie eine Bewertung durch, um Ihre SQL Server-Quellinstanz für ein angegebenes Ziel zu analysieren.

1. Wählen Sie unter **Neu** die Option **Bewertung**, und geben Sie der Bewertung einen Projektnamen.
2. Wählen Sie unter **Typ des Quellservers** die Option **SQL Server**. Wählen Sie unter **Typ des Zielservers** die Option **SQL Server on Azure Virtual Machines** (SQL Server auf virtuellen Azure-Computern).

    ![Quelle auswählen](./media/migrate-scenarios-assessment/dma-assessment-1.png)

    > [!NOTE]
      Derzeit wird die Bewertung für die Migration zu einer verwalteten SQL-Instanz vom DMA nicht unterstützt. Als Problemumgehung verwenden wir SQL Server auf einer Azure-VM als unser angenommenes Ziel für die Bewertung.

1.  Geben Sie unter **Select Target Version** (Zielversion auswählen) die SQL Server-Zielversion an, die sie in Azure ausführen möchten, und was bei der Bewertung ermittelt werden soll:
    - Unter **Compatibility Issues** (Kompatibilitätsprobleme) werden Informationen zu Änderungen angezeigt, die die Migration ggf. verhindern oder für die vor der Migration eine kleinere Anpassung erforderlich ist. Außerdem erhalten Sie hier Informationen zu allen derzeit verwendeten Features, die als veraltet eingestuft wurden. Die Probleme sind nach Kompatibilitätsgrad angegeben. 
    - Unter **Empfehlung zu neuen Features** werden Informationen zu neuen Features auf der SQL Server-Zielplattform angezeigt, die nach der Migration für Ihre Datenbank verwendet werden können. Diese sind nach Leistung, Sicherheit und Speicher angegeben. 

    ![Ziel auswählen](./media/migrate-scenarios-assessment/dma-assessment-2.png)

2. Geben Sie unter **Verbindung mit einem Server herstellen** den Namen des Computers, auf dem die SQL Server-Instanz ausgeführt wird, den Authentifizierungstyp und die Verbindungsdetails an. Klicken Sie auf **Verbinden**.

    ![Ziel auswählen](./media/migrate-scenarios-assessment/dma-assessment-3.png)
    
3. Wählen Sie unter **Quelle hinzufügen** die Datenbank aus, die Sie bewerten möchten, und klicken Sie auf **Hinzufügen**.
4. Es wird eine Bewertung mit dem von Ihnen angegebenen Namen erstellt.

    ![Bewertung erstellen](./media/migrate-scenarios-assessment/dma-assessment-4.png)

5.  Wählen Sie **Weiter**, um die Bewertung zu starten.
6. Unter **Ergebnisse überprüfen** werden die Ergebnisse für die Bewertungstests angezeigt, die Sie aktiviert haben.


### <a name="analyze-the-database-assessment"></a>Analysieren der Datenbankbewertung

Die Ergebnisse werden im Assistenten angezeigt, sobald sie verfügbar sind. 

1. Überprüfen Sie im Bericht **Compatibility Issues** (Kompatibilitätsprobleme), ob für Ihre Datenbank für die einzelnen Kompatibilitätsgrade Probleme vorliegen, und lesen Sie ggf. die Informationen zur Behebung. Kompatibilitätsgrade sind den SQL Server-Versionen wie folgt zugeordnet:
    - 100: SQL Server 2008/Azure SQL-Datenbank
    - 110: SQL Server 2012/Azure SQL-Datenbank
    - 120: SQL Server 2014/Azure SQL-Datenbank
    - 130: SQL Server 2016/Azure SQL-Datenbank
    - 140: SQL Server 2017/Azure SQL-Datenbank

    ![Kompatibilitätsprobleme](./media/migrate-scenarios-assessment/dma-assessment-5.png)

2. Zeigen Sie im Bericht **Featureempfehlungen** die Leistungs-, Sicherheits- und Speicherfeatures an, für die von der Bewertung nach der Migration eine Konfiguration empfohlen wird. Es werden verschiedene Features empfohlen, z.B. In-Memory-OLTP und Columnstore, Stretch Database, Always Encrypted, dynamische Datenmaskierung und Transparent Data Encryption.

    ![Featureempfehlungen](./media/migrate-scenarios-assessment/dma-assessment-6.png)

3. Nachdem Sie die Probleme behoben haben, können Sie auf **Bewertung neu starten** klicken, um die Bewertung erneut durchzuführen. 
4. Klicken Sie auf **Bericht exportieren**, um den Bewertungsbericht im JSON- oder CSV-Format abzurufen.

Bei Durchführung einer umfangreicheren Bewertung:

- Sie können mehrere Bewertungen gleichzeitig durchführen und den Status der Bewertungen anzeigen, indem Sie die Seite **Alle Bewertungen** öffnen.
- Sie können [Bewertungen in einer SQL Server-Datenbank zusammenfassen](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- Sie können [Bewertungen in einem Power BI-Bericht zusammenfassen](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-4-prepare-for-vm-assessment-with-azure-migrate"></a>Schritt 4: Vorbereiten der VM-Bewertung mit Azure Migrate

Erstellen Sie ein VMware-Konto, das von Azure Migrate zum automatischen Ermitteln von VMs für die Bewertung verwendet wird. Überprüfen Sie, ob Sie über Berechtigungen zum Erstellen einer VM verfügen, notieren Sie sich die Ports, die geöffnet sein müssen, und legen Sie die Einstellungsebene für Statistiken fest.

### <a name="set-up-a-vmware-account"></a>Einrichten eines VMware-Kontos

 Sie benötigen ein Konto mit Lesezugriff in vCenter. Erstellen Sie ein VMware-Konto mit den folgenden Eigenschaften, falls Sie dies noch nicht durchgeführt haben:

- Benutzertyp: Mindestens ein Benutzer mit Lesezugriff.
- Berechtigungen: Data Center object (Rechenzentrumsobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle: schreibgeschützt).
- Details: Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.

### <a name="verify-permissions-to-create-a-vm"></a>Überprüfen der Berechtigungen für die Erstellung einer VM

Vergewissern Sie sich, dass Sie über Berechtigungen zum Erstellen einer VM per Import einer Datei im OVA-Format verfügen. [Weitere Informationen](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1)

### <a name="verify-ports"></a>Überprüfen der Ports

In diesem Szenario konfigurieren wir die Zuordnung der Abhängigkeiten. Für dieses Feature muss auf den VMs, die Sie bewerten, ein Agent installiert sein. Dieser Agent muss über den TCP-Port 443 jeder VM jeweils eine Verbindung mit Azure herstellen können. [Erfahren Sie mehr](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) zu den Verbindungsanforderungen.


### <a name="set-statistics-settings"></a>Festlegen der Statistikeinstellungen

Bevor Sie die Bereitstellung starten, sollten die Statistikeinstellungen für vCenter Server auf Ebene 3 festgelegt werden. Beachten Sie Folgendes:
- Nachdem Sie die Ebene festgelegt haben, müssen Sie mindestens einen Tag warten, bis Sie die Bewertung durchführen können. Andernfalls verläuft der Vorgang unter Umständen nicht erwartungsgemäß.
- Bei einer höheren Ebene als 3 funktioniert die Bewertung zwar, aber es sollte Folgendes beachtet werden:
    - Die Leistungsdaten für Datenträger und das Netzwerk werden nicht erfasst.
    - Für den Speicher wird für Azure Migrate ein Standarddatenträger in Azure empfohlen, der die gleiche Größe wie der lokale Datenträger hat.
    - In Bezug auf das Netzwerk wird für jeden lokalen Netzwerkadapter die Verwendung eines entsprechenden Netzwerkadapters in Azure empfohlen.
    - Für den Computebereich untersucht Azure Migrate die VM-Kerne und die Arbeitsspeichergröße und empfiehlt eine Azure-VM mit der gleichen Konfiguration. Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.
   
    
[Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) zur Größenanpassung für Ebene 3.

Legen Sie die Ebene wie folgt fest:

1. Öffnen Sie auf dem vSphere-Web-Client die vCenter Server-Instanz.
2. Wählen Sie die Registerkarte **Verwalten**, und klicken Sie unter **Einstellungen** auf **Allgemein**.
3. Klicken Sie auf **Bearbeiten**, und legen Sie unter **Statistik** die Einstellungen für die Statistikebene auf **Ebene 3** fest.

    ![vCenter-Statistikebene](./media/migrate-scenarios-assessment/vcenter-statistics-level.png)



## <a name="step-5-discover-vms"></a>Schritt 5: Ermitteln von VMs

Erstellen Sie ein Azure Migrate-Projekt, und führen Sie das Herunterladen und Einrichten der Collector-VM durch. Führen Sie den Collector anschließend aus, um Ihre VMs zu ermitteln.

### <a name="create-a-project"></a>Erstellen eines Projekts

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**. Wählen Sie in den Suchergebnissen den Eintrag **Azure Migrate** aus, und klicken Sie auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie den Standort für das Projekt an, und klicken Sie dann auf **Erstellen**.

    - Azure Migrate-Projekte können nur in der Region „USA, Westen-Mitte“ oder „USA, Osten“ erstellt werden.
    - Sie können eine Migration für einen beliebigen Zielstandort planen.
    - Der Projektstandort wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

    ![Azure Migrate](./media/migrate-scenarios-assessment/project-1.png)


    

### <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf den lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie in **Computer ermitteln** auf **Herunterladen**, um die OVA-Datei herunterzuladen.
3. Kopieren Sie in **Projektanmeldeinformationen kopieren** die Projekt-ID und den Projektschlüssel. Diese benötigen Sie beim Konfigurieren des Collectors.

    ![Herunterladen der OVA-Datei](./media/migrate-scenarios-assessment/download-ova.png) 

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Für den generierten Hash müssen diese Einstellungen vorhanden sein (Version 1.0.9.7).
    
    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c
    

### <a name="create-the-collector-appliance"></a>Erstellen der Collectorappliance

Importieren Sie die heruntergeladene Datei auf den vCenter Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/migrate-scenarios-assessment/vcenter-wizard.png) 

2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an, und klicken Sie auf **Weiter**.
3. Klicken Sie unter **OVF Template Details** (Details zur OVF-Vorlage) auf **Weiter**. Klicken Sie unter **Endbenutzer-Lizenzvertrag** auf **Akzeptieren**, um die Vereinbarung zu akzeptieren, und klicken Sie dann auf **Weiter**.
4. Geben Sie unter **Name und Speicherort** einen Anzeigenamen für die Collector-VM und den Inventarspeicherort an, unter dem die VM gehostet wird, und klicken Sie auf **Weiter**. Geben Sie den Host oder Cluster an, auf bzw. in dem die Collectorappliance ausgeführt werden soll.
5. Geben Sie unter **Speicher** an, wo die Dateien für die Appliance gespeichert werden sollen, und klicken Sie auf **Weiter**.
6. Geben Sie unter **Datenträgerformat** an, wie Sie den Speicher bereitstellen möchten.
7. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können. 
8. Überprüfen Sie unter **Ready to Complete** (Für Fertigstellung bereit) die Einstellungen, wählen Sie **Power on after deployment** (Nach Bereitstellung einschalten), und klicken Sie dann auf **Fertig stellen**.

Nachdem die Appliance erstellt wurde, wird eine Meldung mit dem Hinweis angezeigt, dass der Vorgang erfolgreich abgeschlossen wurde.

### <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

Beachten Sie Folgendes, bevor Sie beginnen: Der Collector unterstützt derzeit nur „Englisch (USA)“ als Sprache des Betriebssystems und der Collectoroberfläche.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie für die Appliance die Einstellungen für Sprache, Zeitzone und Kennwort an.
3. Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.

    ![Collectorverknüpfung](./media/migrate-scenarios-assessment/collector-shortcut.png) 
    
4. Öffnen Sie im Azure Migrate-Collector die Option **Erforderliche Komponenten einrichten**.
    - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - Der Collector überprüft, ob die VM über Internetzugriff verfügt, die Uhrzeit synchronisiert ist und der Collectordienst ausgeführt wird (standardmäßig auf der VM installiert). Außerdem wird überprüft, ob VMWare PowerCLI installiert ist. 
    
    > [!NOTE]
    > Wir setzen voraus, dass die VM über direkten Internetzugriff ohne Proxy verfügt.

    ![Überprüfen der Voraussetzungen](./media/migrate-scenarios-assessment/collector-verify-prereqs.png)
    

5. Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Bereich auswählen** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1500 VMs umfassen. 

    ![Herstellen einer Verbindung mit vCenter](./media/migrate-scenarios-assessment/collector-connect-vcenter.png)

6. Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  

    ![Herstellen einer Verbindung mit Azure](./media/migrate-scenarios-assessment/collector-connect-azure.png)

7. Überwachen Sie in **Sammlungsfortschritt anzeigen** die Ermittlung, und vergewissern Sie sich, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.

    ![Collector-Vorgang wird durchgeführt](./media/migrate-scenarios-assessment/collector-collection-process.png)
   


### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nachdem der Collector-Vorgang abgeschlossen ist, sollten Sie überprüfen, ob die VMs im Portal angezeigt werden.

1. Klicken Sie im Azure Migrate-Projekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer auf der Seite angezeigt werden.

    ![Ermittelte Computer](./media/migrate-scenarios-assessment/discovery-complete.png)

3. Beachten Sie, dass auf den Computern derzeit keine Azure Migrate-Agents installiert sind. Wir müssen diese installieren, damit wir die Abhängigkeiten anzeigen können.
    
    ![Ermittelte Computer](./media/migrate-scenarios-assessment/machines-no-agent.png)



## <a name="step-6-prepare-for-dependency-analysis"></a>Schritt 6: Vorbereiten der Abhängigkeitsanalyse

Wir laden die Agents herunter und installieren sie auf den Web-App-VMs (WEBVM und SQLVM), um die Abhängigkeiten zwischen den VMs anzuzeigen, die bewertet werden sollen.

### <a name="take-a-snapshot"></a>Erstellen einer Momentaufnahme

Falls Sie vor dem Vornehmen von Änderungen eine Kopie Ihrer VM erstellen möchten, können Sie vor dem Installieren der Agents eine Momentaufnahme erstellen.

![Momentaufnahme eines Computers](./media/migrate-scenarios-assessment/snapshot-vm.png) 


### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents

1.  Wählen Sie im Azure Migrate-Projekt auf der Seite **Computer** den Computer aus, und klicken Sie in der Spalte **Abhängigkeiten** auf **Installation erforderlich**.
2.  Führen Sie auf der Seite **Computer ermitteln** für jede VM den Download und die Installation des Microsoft Monitoring Agent (MMA) und des Abhängigkeits-Agents durch.
3.  Kopieren Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel. Sie benötigen diese Angaben für die MMA-Installation.

    ![Agent-Download](./media/migrate-scenarios-assessment/download-agents.png) 



#### <a name="install-the-mma"></a>Installieren des MMA

1. Doppelklicken Sie auf den heruntergeladenen Agent.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, und klicken Sie anschließend auf **Weiter**. 
4. Wählen Sie unter **Agent-Setupoptionen** die Option **Connect the agent to Azure Log Analytics (Agent mit Azure Log Analytics verbinden)** > **Weiter**. 

    ![MMA-Installation](./media/migrate-scenarios-assessment/mma-install.png) 
5. Fügen Sie in **Azure Log Analytics** die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den Sie im Portal kopiert haben. Klicken Sie auf **Weiter**.
    ![MMA-Installation](./media/migrate-scenarios-assessment/mma-install2.png) 

6. Installieren Sie unter **Bereit für die Installation** den MMA.



#### <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents

1.  Doppelklicken Sie auf den heruntergeladenen Abhängigkeits-Agent.
2.  Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3.  Warten Sie unter **Installieren**, bis die Installation abgeschlossen ist. Klicken Sie auf **Weiter**.

    ![Abhängigkeits-Agent](./media/migrate-scenarios-assessment/dependency-agent.png) 


       
## <a name="step-7-run-and-analyze-the-vm-assessment"></a>Schritt 7: Durchführen und Analysieren der VM-Bewertung

Überprüfen Sie die Abhängigkeiten der Computer, und erstellen Sie eine Gruppe. Führen Sie anschließend die Bewertung durch.

### <a name="verify-dependencies-and-create-a-group"></a>Überprüfen Sie die Abhängigkeiten, und erstellen Sie eine Gruppe.

1.  Klicken Sie auf der Seite **Computer** für die VMs, die Sie analysieren möchten, auf **Abhängigkeiten anzeigen**.

    ![Anzeigen von Computerabhängigkeiten](./media/migrate-scenarios-assessment/view-machine-dependencies.png) 

2. Für die SQLVM werden in der Abhängigkeitsübersicht die folgenden Informationen angezeigt:

    - Prozessgruppen bzw. Prozesse mit aktiven Netzwerkverbindungen auf der SQLVM für den angegebenen Zeitraum (standardmäßig eine Stunde)
    - Eingehende (Client) und ausgehende TCP-Verbindungen (Server) zu bzw. von allen abhängigen Computern.
    - Abhängige Computer mit installierten Azure Migrate-Agents werden als separate Felder angezeigt
    - Für Computer ohne installierte Agents werden Informationen zum Port und zur IP-Adresse angezeigt.
    
 3. Klicken Sie für Computer mit installiertem Agent (WEBVM) auf das Computerfeld, um weitere Informationen anzuzeigen, z.B. FQDN, Betriebssystem und MAC-Adresse. 

    ![Anzeigen der Gruppenabhängigkeiten](./media/migrate-scenarios-assessment/sqlvm-dependencies.png)

4. Wählen Sie nun die VMs aus, die Sie der Gruppe hinzufügen möchten (SQLVM und WEBVM).  Wenn Sie beim Klicken die STRG-TASTE gedrückt halten, können Sie mehrere VMs auswählen.
5. Klicken Sie auf **Gruppe erstellen**, und geben Sie einen Namen an (smarthotelapp).

> [!NOTE]
    > Sie können den Zeitbereich erweitern, um detailliertere Abhängigkeiten anzuzeigen. Sie können eine bestimmte Dauer oder das Start- und Enddatum auswählen. 


### <a name="run-an-assessment"></a>Durchführen einer Bewertung


1. Öffnen Sie auf der Seite **Gruppen**  die Gruppe (smarthotelapp).
2. Klicken Sie auf **Bewertung erstellen**.

    ![Erstellen einer Bewertung](./media/migrate-scenarios-assessment/run-vm-assessment.png)

3. Die Bewertung wird auf der Seite **Verwalten** > **Bewertungen** angezeigt.


### <a name="modify-assessment-settings"></a>Ändern der Bewertungseinstellungen

Für dieses Tutorial haben wir die Standardeinstellungen für Bewertungen verwendet, aber Sie können die Einstellungen auch wie folgt anpassen:

1. Wählen Sie auf der Seite **Bewertungen** des Migrationsprojekts die Bewertung aus, und klicken Sie auf **Eigenschaften bearbeiten**.
2. Ändern Sie die Eigenschaften entsprechend der folgenden Tabelle:

    **Einstellung** | **Details** | **Standard**
    --- | --- | ---
    **Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll. | Keine Standardeinstellung.
    **Speicherredundanz** | Die Speicherredundanz, die von den virtuellen Azure-Computern nach der Migration verwendet wird. | Der Standardwert ist [Lokal redundanter Speicher (LRS)](../storage/common/storage-redundancy-lrs.md). Azure Migrate unterstützt nur Bewertungen, die auf verwalteten Datenträgern basieren. Verwaltete Datenträger wiederum unterstützen nur LRS, sodass nur die LRS-Option verfügbar ist. 
    **Größenkriterium** | Das Kriterium, das in Azure Migrate zur Größenanpassung virtueller Computer für Azure verwendet werden soll. Sie können eine *leistungsbasierte* Größenanpassung vornehmen oder die Größe der virtuellen Computer *lokal* ändern, ohne den Leistungsverlauf zu berücksichtigen. | Die leistungsbasierte Größenanpassung ist die Standardoption.
    **Leistungsverlauf** | Die zur Bewertung der Leistung der virtuellen Computer zu berücksichtigende Dauer. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium die *leistungsbasierte Größenanpassung* festgelegt ist. | Standardmäßig ist ein Tag festgelegt.
    **Perzentilwert der Nutzung** | Der für die Größenanpassung zu berücksichtigende Perzentilwert des festgelegten Leistungsbeispiels. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium die *leistungsbasierte Größenanpassung* festgelegt ist.  | Der Standardwert ist das 95. Perzentil.
    **Preisstufe** | Sie können den [Tarif (Basic/Standard)](../virtual-machines/windows/sizes-general.md) für die virtuellen Azure-Zielcomputer angeben. Wenn Sie beispielsweise eine Produktionsumgebung migrieren möchten, sollten Sie den Standard-Tarif angeben, der virtuelle Computer mit geringer Latenz umfasst, jedoch auch kostenintensiver sein kann. Bei einer Entwicklungs- und Testumgebung sollten Sie dagegen den Basic-Tarif angeben, der virtuelle Computer mit einer höheren Latenz und geringeren Kosten umfasst. | Standardmäßig wird der Tarif [Standard](../virtual-machines/windows/sizes-general.md) verwendet.
    **Komfortfaktor** | Bei Azure Migrate wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für eine VM mit zehn Kernen und 20% Auslastung ergibt sich normalerweise eine VM mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen. | Die Standardeinstellung ist 1.3x.
    **Angebot** | Das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/), für das Sie registriert sind. | [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) ist die Standardeinstellung.
    **Währung** | Rechnungswährung. | Der Standardwert ist US-Dollar.
    **Rabatt (%)** | Alle abonnementspezifischen Rabatte, die Sie zusätzlich zum Azure-Angebot erhalten. | Die Standardeinstellung ist 0 %.
    **Azure-Hybridvorteil** | Geben Sie an, ob Sie über die Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt. | Die Standardeinstellung ist „Ja“.

3. Klicken Sie auf **Speichern**, um die Bewertungseinstellungen zu aktualisieren.


### <a name="analyze-the-vm-assessment"></a>Analysieren der VM-Bewertung

Eine Azure Migrate-Bewertung enthält Informationen dazu, ob die lokalen VMs mit Azure kompatibel sind, welche Größenanpassung für die Azure-VM richtig ist und welche geschätzten monatlichen Azure-Kosten anfallen. 

![Bewertungsbericht](./media/migrate-scenarios-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

![Bewertungsanzeige](./media/migrate-scenarios-assessment/assessment-display.png)

Für Ihre Bewertung wird eine Zuverlässigkeitsstufe vergeben, die zwischen 1 Stern und 5 Sternen liegt (1 Stern = niedrigste Stufe, 5 Sterne = höchste Stufe).
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand dieses Werts können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe ist hilfreich, wenn Sie eine *leistungsbasierte Größenanpassung* durchführen, da Azure Migrate möglicherweise nicht über genügend Datenpunkte für eine nutzungsbasierte Größenanpassung verfügt. Bei der Größenanpassung vom Typ *Wie lokal* wird die Zuverlässigkeit immer mit fünf Sternen bewertet, da Azure Migrate über alle Datenpunkte verfügt, die zum Bestimmen der VM-Größe erforderlich sind.
- Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent):

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

#### <a name="verify-readiness"></a>Überprüfen der Bereitschaft

![Bewertungsbereitschaft](./media/migrate-scenarios-assessment/azure-readiness.png)  

Im Bewertungsbericht sind die Informationen in einer Tabelle zusammengefasst. Beachten Sie, dass Azure Migrate die folgenden Informationen benötigt, um die leistungsbasierte Größenanpassung anzeigen zu können. Falls diese Informationen nicht erfasst werden können, ist die Bewertung der Größenanpassung ggf. nicht genau.

- Nutzungsdaten für CPU und Arbeitsspeicher.
- Lese/Schreib-IOPS-Wert und Durchsatz für jeden Datenträger, der an die VM angefügt ist.
- Netzwerk-E/A-Informationen für jeden Netzwerkadapter, der an die VM angefügt ist.


**Einstellung** | **Anzeige** | **Details**
--- | --- | ---
**Azure-VM-Bereitschaft** | Gibt an, ob die VM für die Migration bereit ist. | Mögliche Status:</br><br/>- Bereit für Azure<br/><br/>- Bereit mit Bedingungen <br/><br/>- Nicht bereit für Azure<br/><br/>- Bereitschaft unbekannt<br/><br/> Wenn eine VM nicht bereit ist, werden Schritte zur Behebung des Problems angegeben.
**Azure-VM-Größe** | Für VMs, die bereit sind, wird eine Azure-VM-Größe empfohlen. | Die Empfehlung für die Größenanpassung richtet sich nach den Bewertungseigenschaften:<br/><br/>- Wenn Sie die leistungsbasierte Größenanpassung verwendet haben, wird bei der Größenanpassung der Leistungsverlauf der VMs berücksichtigt.<br/><br/>- Wenn Sie die Größenanpassung vom Typ „Wie lokal“ verwendet haben, basiert die Größenanpassung auf der Größe der lokalen VM, und es werden keine Nutzungsdaten verwendet.
**Vorgeschlagenes Tool** | Da die Agents auf unseren Computern ausgeführt werden, untersucht Azure Migrate die auf dem Computer ausgeführten Prozesse und ermittelt, ob es sich um einen Datenbankcomputer handelt.
**VM-Informationen** | Im Bericht werden die Einstellungen für die lokale VM angezeigt, z.B. Informationen zu Betriebssystem, Starttyp, Datenträger und Speicher.




#### <a name="review-monthly-cost-estimates"></a>Überprüfen der geschätzten monatlichen Kosten

In dieser Ansicht sind die gesamten Compute- und Speicherkosten für den Betrieb der virtuellen Computer in Azure zusammen mit den Details für die einzelnen Computer angegeben. 

![Bewertungsbereitschaft](./media/migrate-scenarios-assessment/azure-costs.png) 

- Kostenschätzungen werden anhand der Größenempfehlungen für einen Computer berechnet.
- Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert. 


## <a name="conclusion"></a>Zusammenfassung

In diesem Szenario haben wir Folgendes durchgeführt:

> [!div class="checklist"]
> * Bewerten der lokalen Datenbank mit dem DMA-Tool
> * Bewerten der lokalen VMs per Abhängigkeitszuordnung mit dem Azure Migrate-Dienst
> * Überprüfen der Bewertungen, um sicherzustellen, dass die lokalen Ressourcen für die Migration zu Azure bereit sind

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Szenario fort, um für die lokalen VMs und die Datenbank eine [Migration per Lift & Shift](migrate-scenarios-lift-and-shift.md) zu Azure durchzuführen.




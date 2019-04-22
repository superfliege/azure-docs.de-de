---
title: Rehost-Migrate und Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Zuweisen eines neuen Hosts für eine lokale Linux-App durch Migration zu Azure-VMs.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 200f9c5df0d4165341e38ca9d4dd85ad75c8403c
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010361"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs

Dieser Artikel enthält Informationen zum Zuweisen eines neuen Hosts für eine lokale, auf Linux basierende Service Desk-App (**osTicket**) zu Azure-IaaS-VMs.

Dieses Dokument ist das erste in einer Reihe von Artikeln, die dokumentieren, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und einige Szenarien, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird und wie verschiedene Migrationstypen ausgeführt werden. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Wir fügen im Laufe der Zeit weitere Artikel hinzu.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Migrationsinfrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Contoso führt eine Bewertung seiner lokalen App SmartHotel360 durch, die in VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten. | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert den virtuellen Front-End-Computer der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank mit dem [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten Azure SQL-Datenbank-Instanz. | Verfügbar   
[Artikel 5: Rehosten einer App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der App SmartHotel360 mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die App SmartHotel360. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar 
Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs | Contoso führt mithilfe von Azure Site Recovery per Lift & Shift eine Migration der Linux-App „osTicket“ zu virtuellen Azure-Computern durch. | Dieser Artikel
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Site Recovery zu virtuellen Azure-Computern und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz. | Verfügbar
[Artikel 9: Umgestalten einer App in Azure-Web-Apps und in der Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert die App SmartHotel360 zu einer Azure-Web-App und die App-Datenbank mithilfe des Datenbankmigrations-Assistenten zu einer Azure SQL Server-Instanz. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in Azure-Web-Apps und Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Traffic Manager zu einer Azure-Web-App in mehreren Azure-Regionen. Zur Sicherstellung der Continuous Delivery erfolgt eine Integration in GitHub. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar 
[Artikel 11: Umgestalten einer Team Foundation Server-Bereitstellung zu Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer lokalen App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Contoso migriert seine SmartHotel-App zu Azure. Anschließend wird die App-Webschicht zu einem Windows-Container umstrukturiert, der in Azure Service Fabric ausgeführt wird, und die Datenbank wird zu einer Azure SQL-Datenbank umstrukturiert. | Verfügbar
[Artikel 13: Neuerstellen einer lokalen App in Azure](contoso-migration-rebuild.md) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z.B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar
[Artikel 14: Skalieren einer Migration zu Azure](contoso-migration-scale.md) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor. | Verfügbar



In diesem Artikel migriert Contoso die zweischichtige App **osTicket** zu Azure, die unter Linux Apache MySQL PHP (LAMP) ausgeführt wird. Die App-VMs werden mithilfe des Diensts Azure Site Recovery migriert. Wenn Sie diese Open Source-App verwenden möchten, können Sie sie von [GitHub](https://github.com/osTicket/osTicket) herunterladen.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was das Unternehmen mit dieser Migration erreichen möchte:

- **Unternehmenswachstum**: Contoso wächst, und daher geraten die lokalen Systeme und Infrastrukturen unter Druck.
- **Risikobegrenzung**: Die Service Desk-App ist wichtig für das Geschäft von Contoso. Contoso möchte sie ohne jedes Risiko nach Azure verlagern.
- **Erweitern**:  Contoso möchte die App derzeit nicht ändern. Es soll lediglich sichergestellt werden, dass die App stabil ist.


## <a name="migration-goals"></a>Migrationsziele

Für die Bestimmung der besten Migrationsmethode hat das Contoso-Cloudteam Ziele für diese Migration festgelegt:

- Nach der Migration sollte die App in Azure die gleichen Leistungsmöglichkeiten aufweisen wie die lokale VMWare-Umgebung.  Die App bleibt lokal und in der Cloud gleichermaßen wichtig. 
- Contoso möchte nicht in diese App investieren.  Die Investition ist wichtig für das Geschäft, in ihrer derzeitigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Contoso möchte das Vorgangsmodell für diese App nicht ändern. Das Unternehmen möchte genau wie jetzt in der Cloud mit der App interagieren können.
- Contoso möchte die App-Funktionalität nicht ändern. Nur der Speicherort der App ändert sich.
- Nach Abschluss einer Reihe von Windows-App-Migrationen möchte Contoso mehr über die Verwendung einer Linux-basierten Infrastruktur in Azure erfahren.


## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess sowie die für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die App „OSTicket“ ist auf zwei virtuelle Computer aufgeteilt (**OSTICKETWEB** und **OSTICKETMYSQL**).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) mit einem lokalen Domänencontroller (**contosodc1**).

### <a name="proposed-architecture"></a>Vorgeschlagene Architektur

- Da es sich bei der App um eine Produktionsworkload handelt, befinden sich die virtuellen Computer in Azure in der Produktionsressourcengruppe **ContosoRG**.
- Die VMs werden zur primären Region (USA, Osten 2) migriert und im Produktionsnetzwerk (VNET-PROD-EUS2) platziert:
    - Die Web-VM befindet sich im Front-End-Subnetz (PROD-FE-EUS2).
    - Die Datenbank-VM befindet sich im Subnetz der Datenbank (PROD-DB-EUS2).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.

![Szenarioarchitektur](./media/contoso-migration-rehost-linux-vm/architecture.png) 

### <a name="solution-review"></a>Überprüfung der Lösung

Contoso bewertet den vorgeschlagen Entwurf anhand einer Liste mit Vor- und Nachteilen.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Beide virtuellen Computer der App werden ohne Änderungen nach Azure verlagert, was die Migration vereinfacht.<br/><br/> Da Contoso beide virtuellen Computer der App per Lift & Shift migriert, sind für die App-Datenbank keine besonderen Konfigurations- oder Migrationstools erforderlich.<br/><br/> Contoso behält die vollständige Kontrolle über die virtuellen Computer der App in Azure. </br>/br> Die virtuellen Computer der App führen Ubuntu 16.04-TLS aus. Dabei handelt es sich um eine unterstützte Linux-Distribution. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)
**Nachteile** | Die Web- und Datenschicht der App bleiben ein Single Point of Failover. <br/><br/> Contoso muss die App weiterhin als virtuelle Azure-Computer unterstützen, anstatt auf einen verwalteten Dienst wie Azure App Service oder Azure Database for MySQL umzustellen.<br/><br/> Contoso ist sich bewusst, dass bei der Vereinfachung durch eine Migration von virtuellen Computern per Lift & Shift die von [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) bereitgestellten Funktionen (integrierte Hochverfügbarkeit, vorhersagbare Leistung, einfache Skalierung, automatische Sicherungen und integrierte Sicherheit) nicht in vollem Umfang genutzt werden können.

### <a name="migration-process"></a>Migrationsprozess

Contoso geht bei der Migration wie folgt vor:

1. In einem ersten Schritt richtet Contoso die Azure-Infrastruktur und die lokale Infrastruktur ein, die für die Bereitstellung von Site Recovery erforderlich sind.
2. Nach der Vorbereitung der Azure-Komponenten und der lokalen Komponenten richtet das Unternehmen die Replikation für die VMs ein und aktiviert diese.
3. Wenn die Replikation funktioniert, migriert Contoso die virtuellen Computer via Failover zu Azure.

![Migrationsprozess](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-Dienste

**Dienst** | **BESCHREIBUNG** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.

 
## <a name="prerequisites"></a>Voraussetzungen

Für dieses Szenario benötigt Contoso Folgendes.

**Requirements (Anforderungen)** | **Details**
--- | ---
**Azure-Abonnement** | Contoso hat in einem der ersten Artikel dieser Reihe Abonnements erstellt. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-Infrastruktur** | Contoso richtet die Azure-Infrastruktur ein, wie in [Azure infrastructure for migration (Azure-Infrastruktur für die Migration)](contoso-migration-infrastructure.md) beschrieben.<br/><br/> Erfahren Sie mehr zu spezifischen [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network)- und [Speicheranforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) für Site Recovery.
**Lokale Server** | Der lokale vCenter-Server muss mit Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.
**Lokale VMs** | [Überprüfen Sie die Linux-Computer](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines), die für die Migration mit Site Recovery unterstützt werden.<br/><br/> Überprüfen Sie unterstützte [Linux-Datei- und -Speichersysteme](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.


## <a name="scenario-steps"></a>Szenarioschritte

Contoso geht bei der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Vorbereiten von Azure für Site Recovery**: Contoso erstellt ein Azure-Speicherkonto zum Speichern replizierter Daten sowie einen Recovery Services-Tresor.
> * **Schritt 2: Vorbereiten einer lokalen VMware-Instanz für Site Recovery**: Contoso bereitet Konten für die Ermittlung von virtuellen Computern und die Agent-Installation sowie eine Verbindung mit virtuellen Azure-Computern nach dem Failover vor.
> * **Schritt 3: Replizieren von VMs**: Contoso richtet die Quell- und Zielmigrationsumgebung ein, erstellt eine Replikationsrichtlinie und startet die Replikation von virtuellen Computern im Azure-Speicher.
> * **Schritt 4: Migrieren von VMs mit Site Recovery**: Contoso führt ein Testfailover durch, um sicherzustellen, dass alles funktioniert, und führt anschließend ein vollständiges Failover für die Migration der virtuellen Computer zu Azure aus.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Schritt 1: Vorbereiten von Azure für den Site Recovery-Dienst

Contoso benötigt eine Reihe von Azure-Komponenten für Site Recovery:

- Ein neues Azure-Speicherkonto für die Speicherung replizierter Daten. 
- Ein Recovery Services-Tresor in Azure.
- Ein VNET, in dem sich Ressourcen befinden, für die ein Failover ausgeführt wurde. Contoso hat das VNet bereits während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md) erstellt und muss daher nur ein Speicherkonto und einen Tresor erstellen.

1. Contoso-Administratoren erstellen ein Azure-Speicherkonto (contosovmsacc20180528) in der primären Region „USA, Osten 2“.

    - Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
    - Contoso verwendet ein universelles Konto mit Standardspeicher und LRS-Replikation.

      ![Site Recovery-Speicher](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Wenn das Netzwerk und das Speicherkonto vorhanden sind, erstellen die Administratoren einen Tresor (ContosoMigrationVault) und platzieren diesen in der Ressourcengruppe **ContosoFailoverRG** in der primären Region „USA, Osten 2“.

    ![Recovery Services-Tresor](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Benötigen Sie weitere Hilfe?**

[Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) zum Einrichten von Azure für Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 2: Vorbereiten einer lokalen VMware-Instanz für Site Recovery

Contoso-Administratoren bereiten die lokale VMware-Infrastruktur wie folgt vor:

- Sie erstellen ein Konto auf dem Server von vCenter Server oder dem vSphere ESXi-Host zum Automatisieren der Ermittlung virtueller Computer.
- Sie erstellen ein Konto für die automatische Installation des Mobilitätsdiensts auf virtuellen VMware-Computern, die repliziert werden sollen.
- Sie bereiten lokale virtuelle Computer vor, damit diese mit virtuellen Azure-Computern verbunden werden können, wenn diese nach der Migration erstellt werden.


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso-Administratoren richten das Konto wie folgt ein:

1. Es wird eine Rolle auf vCenter-Ebene erstellt.
2. Anschließend werden dieser Rolle die erforderlichen Berechtigungen zugewiesen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobilitätsdienst muss auf den virtuellen Linux-Computern, die migriert werden, installiert sein.

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn die Replikation für virtuelle Computer aktiviert ist.
- Für die automatische Pushinstallation müssen sie ein Konto vorbereiten, mit dem Site Recovery auf die virtuellen Computer zugreift.
- Während der Einrichtung der Replikation werden Kontodetails eingegeben. 
- Bei dem Konto kann es sich um ein Domänenkonto oder ein lokales Konto mit Berechtigungen für die Installation auf VMs handeln.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover in Azure möchte Contoso eine Verbindung mit den replizierten VMs in Azure herstellen können. Dazu müssen die Contoso-Administratoren einige Schritte durchführen:

- Für den Zugriff auf virtuelle Azure-Computer über das Internet müssen sie vor der Migration auf dem lokalen Linux-Computer SSH aktivieren.  Bei Ubuntu kann dieser Vorgang mithilfe des folgenden Befehls durchgeführt werden: **Sudo apt-get ssh install -y**.
- Nach der Durchführung der Migration (Failover) können sie die **Startdiagnose** überprüfen, um einen Screenshot des virtuellen Computers anzuzeigen.
- Falls dies nicht funktioniert, müssen sie überprüfen, ob der virtuelle Computer ausgeführt wird, und sollten folgenden [Tipps zur Problembehandlung](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) lesen.


**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) zum Erstellen und Zuweisen von Rollen für die automatische Ermittlung.
- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) zum Erstellen eines Kontos für die Pushinstallation von Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Schritt 3: Replizieren der lokalen VMs

Bevor sie den virtuellen Web-Computer zu Azure migrieren können, müssen die Contoso-Administratoren die Replikation einrichten und aktivieren.

### <a name="set-a-protection-goal"></a>Festlegen eines Schutzziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel festgelegt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass sich seine Computer in einer lokalen Umgebung befinden, dass es sich um VMware-VMs handelt und diese in Azure repliziert werden sollen.
    ![Replikationsziel](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, bestätigt es durch Auswahl von **Ja, ist abgeschlossen**, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Szenario nur eine VM. Daher ist keine Bereitstellungsplanung erforderlich.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso-Administratoren müssen nun die Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage heruntergeladen, die für die Bereitstellung des Site Recovery-Konfigurationsservers als hoch verfügbare, lokale VMware-VM verwendet wird. Nachdem der Konfigurationsserver eingerichtet wurde und ausgeführt wird, wird er im Tresor registriert.

Auf dem Konfigurationsserver werden einige Komponenten ausgeführt:

- Die Konfigurationsserverkomponente, die die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.

Contoso-Administratoren führen diese Schritte wie folgt aus:

1. Das Unternehmen lädt die OVF-Vorlage von **Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver** herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Es importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert den Lizenzvertrag und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet es sich als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Nachdem die Verbindung hergestellt wurde, meldet sich das Unternehmen bei seinem Azure-Abonnement an. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.

    ![Konfigurationsserver registrieren](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
9. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
10. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.

    ![Tresor](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Anschließend werden MySQL Server und VMWare PowerCLI heruntergeladen und installiert. 
12. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des Servers von vCenter Server oder des vSphere-Hosts an. Der Standardport wird beibehalten, und für den Server von vCenter Server wird ein Anzeigename angegeben.
13. Contoso gibt das für die automatische Ermittlung erstellte Konto und die Anmeldeinformationen für die automatische Installation von Mobility Service an.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Nach Abschluss der Registrierung überprüfen die Administratoren im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern. 
15. Anschließend stellt Site Recovery eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Contoso-Administratoren konfigurieren nun die Zielreplikationseinstellungen.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Nach der Einrichtung der Quelle und des Ziels können sie jetzt eine Replikationsrichtlinie erstellen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) zum Azure-Gast-Agent für Linux.

**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) zum Azure-Gast-Agent für Linux.



### <a name="enable-replication-for-osticketweb"></a>Aktivieren der Replikation für OSTICKETWEB

Jetzt können die Contoso-Administratoren mit der Replikation des virtuellen Computers **OSTICKETWEB** beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Es wählt aus, dass virtuelle Computer aktiviert werden sollen. Darüber hinaus wählt es Quelleneinstellungen einschließlich der Einstellungen für den Server von vCenter Server und den Konfigurationsserver aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Contoso gibt die Zieleinstellungen, einschließlich der Ressourcengruppe und des VNet als Speicherort für die Azure-VM nach dem Failover, und das Speicherkonto an, in dem die replizierten Daten gespeichert werden.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Sie wählen den virtuellen Computer **OSTICKETWEB** für die Replikation aus. 

   - In dieser Phase wählen sie nur **OSTICKETWEB** aus, da das VNET und das Subnetz ausgewählt werden müssen und sich die virtuellen Computer nicht im selben Subnetz befinden.
   - Wenn die Replikation für die VM aktiviert ist, wird Mobility Service von Site Recovery automatisch installiert.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. In den Eigenschaften des virtuellen Computers wählen sie das Konto aus, das der Prozessserver zur automatischen Installation von Mobility Service auf dem Computer verwendet.

     ![Mobilitätsdienst](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. Unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** überprüft das Unternehmen, ob die richtige Replikationsrichtlinie angewendet wird, und wählt **Replikation aktivieren** aus.
6. Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.



### <a name="enable-replication-for-osticketmysql"></a>Aktivieren der Replikation für OSTICKETMYSQL

Contoso-Administratoren können nun mit der Replikation von **OSTICKETMYSQL** beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quellen- und Zieleinstellungen aus.
2. Sie wählen den virtuellen Computer **OSTICKETMYSQL** für die Replikation und das für die Installation von Mobility Service zu verwendende Konto aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Sie wenden die gleiche Replikationsrichtlinie wie für OSTICKETWEB an und aktivieren die Replikation.  

**Benötigen Sie weitere Hilfe?**

Unter [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication) finden Sie eine vollständige exemplarische Vorgehensweise mit all diesen Schritten.


## <a name="step-4-migrate-the-vms"></a>Schritt 4: Migrieren der VMs 

Contoso-Administratoren führen ein schnelles Testfailover aus und migrieren anschließend die virtuellen Computer.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Das Ausführen eines Testfailovers dient zur Sicherstellung, dass vor der Migration alles wie erwartet funktioniert. 

1. Sie führen ein Testfailover auf den letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Testfailover durchführen: 
    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn der letzte Wiederherstellungspunkt ausgewählt wird, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.
3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Das Unternehmen überprüft, ob die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. 
4. Nach der Überprüfung bereinigt es das Failover. Darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert.

### <a name="create-and-customize-a-recovery-plan"></a>Erstellen und Anpassen eines Wiederherstellungsplans

 Nachdem sie überprüft haben, ob das Testfailover wie erwartet funktioniert, erstellen Contoso-Administratoren einen Wiederherstellungsplan für die Migration. 

- Ein Wiederherstellungsplan gibt die Reihenfolge an, in der ein Failover auftritt, wie Azure-VMs in Azure hervorgebracht werden.
- Da das Unternehmen eine zweischichtige App migrieren möchte, passt es den Wiederherstellungsplan so an, dass die Daten-VM (SQLVM) vor der Front-End-VM (WEBVM) gestartet wird.


1. Unter **Wiederherstellungspläne (Site Recovery)** > **+Wiederherstellungsplan** erstellt das Unternehmen einen Plan und fügt die VMs dem Plan hinzu.

    ![Wiederherstellungsplan](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Nachdem der Plan erstellt wurde, wählt das Unternehmen diesen zur Anpassung aus (**Wiederherstellungspläne** > **OsTicketMigrationPlan** > **Anpassen**).
3.  Es entfernt **OSTICKETWEB** aus **Gruppe 1: Start**.  Dadurch wird sichergestellt, dass die erste Startaktion sich nur auf **OSTICKETMYSQL** auswirkt.

    ![Wiederherstellungsgruppe](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  Unter **+ Gruppe** > **Geschützte Elemente hinzufügen** fügt Contoso **OSTICKETWEB** zu **Gruppe 2: Start** hinzu.  Sie benötigen diese in zwei unterschiedlichen Gruppen.

    ![Wiederherstellungsgruppe](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Migrieren der VMs


Contoso-Administratoren können nun ein Failover für den Wiederherstellungsplan ausführen, um die virtuellen Computer zu migrieren.

1. Es wählt den Plan > **Failover** aus.
2.  Es entscheidet, ein Failover für den letzten Wiederherstellungspunkt auszuführen. Zudem gibt das Unternehmen an, dass Site Recovery versuchen sollte, die lokale VM vor dem Auslösen des Failovers herunterzufahren. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)

3. Während des Failovers gibt der vCenter Server Befehle zum Beenden der beiden VMs aus, die auf dem ESXi-Host ausgeführt werden.

    ![Failover](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Nach der Überprüfung der VM in Azure schließt das Unternehmen die Migration ab, um den Migrationsprozess für die einzelnen VMs zu beenden. Dadurch werden die Replikation für die VM und die Site Recovery-Abrechnung für die VM beendet.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>Verbinden der VM mit der Datenbank

Der letzte Schritt im Migrationsprozess besteht in der Aktualisierung der Verbindungszeichenfolge der Anwendung durch die Contoso-Administratoren, um auf die App-Datenbank zu verweisen, die auf dem virtuellen Computer **OSTICKETMYSQL** ausgeführt wird. 

1. Das Unternehmen stellt über Putty oder einen anderen SSH-Client eine SSH-Verbindung mit der VM **OSTICKETWEB** her. Die VM ist privat, daher stellt Contoso eine Verbindung über die private IP-Adresse her.

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Das Unternehmen muss sicherstellen, dass die VM **OSTICKETWEB** mit der VM **OSTICKETMYSQL** kommunizieren kann. Die Konfiguration ist mit der lokalen IP-Adresse 172.16.0.43 derzeit hartcodiert.

    **Vor dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Nach dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Das Unternehmen startet den Dienst mit dem Befehl **systemctl restart apache2** neu.

    ![Neu starten](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Schließlich aktualisiert Contoso die DNS-Datensätze für **OSTICKETWEB** und **OSTICKETMYSQL** auf einem der Contoso-Domänencontroller.

    ![DNS-Aktualisierung](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS-Aktualisierung](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers. 
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach Abschluss der Migration werden die Schichten der App osTicket jetzt auf Azure-VMs ausgeführt.

Contoso muss nun folgende Bereinigungsmaßnahmen vornehmen: 

- Entfernen der lokalen VMs aus dem vCenter-Bestand.
- Es entfernt die lokalen VMs aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation, damit der neue Speicherort und die IP-Adressen für OSTICKETWEB und OSTICKETMYSQL angezeigt werden.
- Überprüfen sämtlicher Ressourcen, die mit den VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.
- Contoso hat mithilfe des Azure Migrate-Diensts durch die Zuordnung von Abhängigkeiten die VMs für die Migration bewertet. Administratoren müssen Microsoft Monitoring Agent und den Dependency-Agent, die für diesen Zweck installiert wurden, vom virtuellen Computer entfernen.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die App jetzt ausgeführt wird, muss Contoso seine neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüft die VMs OSTICKETWEB und OSTICKETMYSQL, um mögliche Sicherheitsprobleme zu ermitteln.

- Das Team überprüft die Netzwerksicherheitsgruppen (NSGs) für die virtuellen Computer zur Steuerung des Zugriffs. Mithilfe von NSGs wird sichergestellt, dass nur für die App zulässiger Datenverkehr übergeben werden kann.
- Das Team zieht darüber hinaus in Betracht, die Daten über Datenträgerverschlüsselung und Azure KeyVault auf den Datenträgern des virtuellen Computers zu sichern.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) über Sicherheitsverfahren für VMs.

### <a name="bcdr"></a>BCDR

Zur Sicherstellung der Geschäftskontinuität und Notfallwiederherstellung führt Contoso die folgenden Aktionen durch:

- **Schützen von Daten**: Contoso sichert die Daten auf den VMs mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- **Sicherstellen eines unterbrechungsfreien App-Betriebs**: Contoso repliziert die App-VMs in Azure mithilfe von Site Recovery in einer sekundären Region. [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nach der Bereitstellung von Ressourcen, weist der Contoso Azure Tags zu, die während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md#set-up-tagging) definiert wurden.
- Contoso hat mit den Ubuntu-Servern keine Lizenzierungsprobleme.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Contoso eine lokale mehrschichtige Service Desk-App mit zwei Linux-VMs mithilfe von Azure Site Recovery zu Azure-IaaS-VMs migriert hat.

Im nächsten Artikel der Reihe erfahren Sie, wie Contoso die gleiche Service Desk-App zu Azure migriert. Dieses Mal migriert Contoso den virtuellen Front-End-Computer für die App mithilfe von Site Recovery. Die App-Datenbank wird mit dem Tool „MySQL Workbench“ über die Sicherung und Wiederherstellung in Azure Database for MySQL migriert. [Erste Schritte](contoso-migration-rehost-linux-vm-mysql.md).

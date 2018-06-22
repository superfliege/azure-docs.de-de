---
title: Zuweisen eines neuen Hosts für eine Service Desk-App für Contoso unter Linux zu Azure und Azure MySQL | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Zuweisen eines neuen Hosts für eine lokale Linux-App durch Migration zu Azure-VMs und Azure MySQL.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 15a429c033cfd1598dd01b5c8cd2743c397dacdb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225516"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs und Azure MySQL

In diesem Artikel wird gezeigt, wie Contoso einen neuen Host für ihre lokale Linux-Service Desk-App (osTicket) auf zwei Ebenen zuweist, indem sie sie zu Azure und Azure MySQL migrieren.

Dieses Dokument ist das achte in einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird, und wie verschiedene Migrationstypen ausgeführt werden. Die Szenarios werden an Komplexität zunehmen, und mit der Zeit werden zusätzliche Artikel hinzugefügt.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für sämtliche Contoso-Migrationsszenarien wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen, unter VMware ausgeführten App SmartHotel durchführt. Sie bewerten App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Datenbankmigrations-Assistent von Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso die SmartHotel-App zu Azure migriert. Sie migrieren die App-Web-VM mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und verwenden die App-Datenbank und den Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview), um zu einer verwalteten SQL-Instanz zu migrieren. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die SmartHotel-App nur mit Site Recovery zu Azure IaaS-VMs migriert.
[Artikel 6: Zuweisen eines neuen Hosts zu Azure-VMs und SQL Server-Verfügbarkeitsgruppen](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Das Unternehmen verwendet Site Recovery zum Migrieren der App-VMs und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-Verfügbarkeitsgruppe. | Verfügbar
[Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs](contoso-migration-rehost-linux-vm.md) | Zeigt, wie Contoso die osTicket-Linux-App mithilfe von Azure Site Recovery zu Azure IaaS-VMs migriert.
Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs und Azure MySQL Server (dieser Artikel) | Veranschaulicht, wie Contoso die osTicket-Linux-App migriert. Das Unternehmen verwendet Site Recovery für die VM-Migration und MySQL-Workbench, um zu einer Azure MySQL Server-Instanz zu migrieren. | Verfügbar

In diesem Artikel migriert Contoso eine Service Desk-App (osTicket) auf zwei Ebenen von Linux Apache MySQL PHP (LAMP) zu Azure. Wenn Sie diese Open Source-App verwenden möchten, können Sie sie von [GitHub](https://github.com/osTicket/osTicket) herunterladen.



## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit seinen Geschäftspartnern zusammengearbeitet, um zu verstehen, was sie erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und daher geraten ihre lokalen Systeme und Infrastrukturen unter Druck.
- **Risikobegrenzung**: Die Service Desk-App ist wichtig für das Geschäft von Contoso. Sie soll ohne jedes Risiko in Azure verschoben werden.
- **Erweitern**: Contoso will die App derzeit nicht ändern. Sie soll einfach nur stabil bleiben.


## <a name="migration-goals"></a>Migrationsziele

Für die Bestimmung der besten Migrationsmethode hat das Contoso-Cloudteam Ziele für diese Migration festgelegt:

- Nach der Migration sollte die App in Azure über dieselben Leistungsmöglichkeiten verfügen, wie sie sie heute in ihrer lokalen VMWare-Umgebung hat.  Die App bleibt lokal und in der Cloud gleichermaßen wichtig. 
- Contoso möchte nicht in diese App investieren.  Sie ist wichtig für das Geschäft, in ihrer derzeitigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Nach Abschluss einer Reihe von Windows-App-Migrationen möchte Contoso mehr über die Verwendung einer Linux-basierten Infrastruktur in Azure erfahren.
- Contoso möchte die Administratoraufgaben für die Datenbank minimieren, nachdem die Anwendung in die Cloud verschoben wird.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Szenario:

- Die App wird über zwei VM-Schichten (OSTICKETWEB und OSTICKETMYSQL) zur Verfügung gestellt.
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die Web-App OSTICKETWEB auf zwei Anwendungsebenen wird zu einer Azure-IaaS-VM migriert werden.
- Die App-Datenbank wird zum Dienst Azure Database for MySQL PaaS migriert.
- Da es sich um die Migration einer Produktionsworkload handelt, befinden sich die Ressourcen in der Ressourcengruppe für die Produktion **ContosoRG**.
- Die Ressourcen werden in der primären Region (USA, Osten 2) repliziert und im Produktionsnetzwerk (VNET-PROD-EUS2) abgelegt:
    - Die Web-VM befindet sich im Front-End-Subnetz (PROD-FE-EUS2).
    - Die Datenbank-Instanz befindet sich im Subnetz der Datenbank (PROD-DB-EUS2).
- Die App-Datenbank wird mit MySQL-Tools zu Azure MySQL migriert.
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


![Szenarioarchitektur](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>Migrationsprozess

Contoso wird den Migrationsprozess wie folgt abschließen:

Zur Migration der Web-VM:

1. In einem ersten Schritt richtet Contoso die Azure-Infrastruktur und die lokale Infrastruktur ein, die für die Bereitstellung von Site Recovery erforderlich sind.
2. Nach der Vorbereitung der Azure-Komponenten und der lokalen Komponenten richtet das Unternehmen die Replikation für die Web-VMs ein und aktiviert diese.
3. Wenn die Replikation funktioniert, migriert Contoso die VM via Failover zu Azure.

Zur Migration der Datenbank:

1. Contoso stellt eine MySQL-Instanz in Azure bereit.
2. Sie richten die MySQL-Workbench ein und sichern die Datenbank lokal.
3. Dann stellen sie die Datenbank aus der lokalen Sicherung in Azure wieder her.

![Migrationsprozess](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | Die Datenbank basiert auf der Open Source-MySQL-Server-Engine. Sie stellt eine vollständig verwaltete, unternehmensgerechte MySQL Community-Edition als Database as a Service für die Entwicklung und Bereitstellung von Apps bereit. 

 
## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie (und Contoso) dieses Szenario ausführen möchten, sind hier die entsprechenden Anforderungen aufgeführt.

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie sollten bereits im Zuge früherer Artikel dieser Reihe ein Abonnement erstellt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-Infrastruktur** | Contoso richtet die Azure-Infrastruktur ein, wie in [Azure infrastructure for migration (Azure-Infrastruktur für die Migration)](contoso-migration-infrastructure.md) beschrieben.<br/><br/> Erfahren Sie mehr zu spezifischen [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network)- und [Speicheranforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) für Site Recovery.
**Lokale Server** | Ihr lokaler Server von vCenter Server sollte als Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.
**Lokale VMs** | [Überprüfen Sie die Linux-VM-Anforderungen](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines), die für die Migration mit Site Recovery unterstützt werden.<br/><br/> Überprüfen Sie unterstützte [Linux-Datei- und -Speichersysteme](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.


## <a name="scenario-steps"></a>Szenarioschritte

Azure geht bei der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Vorbereiten von Azure für Site Recovery**: Sie erstellen ein Azure-Speicherkonto zum Speichern replizierter Daten und erstellen einen Recovery Services-Tresor.
> * **Schritt 2: Vorbereiten der lokalen VMware für Site Recovery**: Sie bereiten Konten für die VM-Ermittlung und Agentinstallation sowie eine Verbindung mit Azure-VMs nach dem Failover vor.
 * **Schritt 3: Bereitstellen der Datenbank**: Sie stellen in Azure eine Instanz der Azure MySQL-Datenbank bereit.
> * **Schritt 4: Replizieren von VMs**: Sie konfigurieren die Quell- und Zielumgebung für Site Recovery, richten eine Replikationsrichtlinie ein und starten die Replikation von VMs zu Azure Storage.
> * **Schritt 5: Migration der Datenbank**: Sie richten die Migration mit MySQL-Tools ein.
> * **Schritt 6: Migrieren der VMs mit Site Recovery**: Führe Sie ein Testfailover durch, um sicherzustellen, dass alles funktioniert, und führen Sie anschließend ein vollständiges Failover für die Migration der VMs zu Azure aus.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Schritt 1: Vorbereiten von Azure für den Site Recovery-Dienst

Contoso benötigt eine Reihe von Azure-Komponenten für Site Recovery:

- Ein VNet, in dem sich Ressourcen befinden, für die ein Failover ausgeführt wurde (Contoso verwendet das bereits bereitgestellte Produktions-VNet).
- Ein neues Azure-Speicherkonto für die Speicherung replizierter Daten. 
- Ein Recovery Services-Tresor in Azure.

Contoso hat das VNet bereits während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md) erstellt und muss daher nur ein Speicherkonto und einen Tresor erstellen.


1. Contoso erstellt ein Azure-Speicherkonto (**contosovmsacc20180528**) in der Region „USA, Osten 2“.

    - Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
    - Contoso verwendet ein universelles Konto mit Standardspeicher und LRS-Replikation.

    ![Site Recovery-Speicher](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Wenn das Netzwerk und das Speicherkonto vorhanden sind, erstellt Contoso einen Tresor (ContosoMigrationVault) und platziert diesen in der Ressourcengruppe **ContosoFailoverRG**, in der primären Region „USA, Osten 2“.

    ![Recovery Services-Tresor](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Benötigen Sie weitere Hilfe?**

[Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) zum Einrichten von Azure für Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 2: Vorbereiten der lokalen VMware für Site Recovery

Contoso bereitet die lokale VMware-Infrastruktur wie folgt vor:

- Erstellt ein Konto auf dem Server von vCenter Server zum Automatisieren der VM-Ermittlung.
- Erstellt ein Konto für die automatische Installation von Mobility Service auf VMware-VMs, die Sie replizieren möchten.
- Bereitet lokale VMs vor, damit diese mit Azure-VMs verbunden werden können, wenn diese nach der Migration erstellt werden.


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso richtet das Konto wie folgt ein:

1. Contoso erstellt eine Rolle auf der vCenter-Ebene.
2. Anschließend weist Contoso dieser Rolle die erforderlichen Berechtigungen zu.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobilitätsdienst muss auf jeder VM installiert sein, die Contoso replizieren möchte.

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn Sie die Replikation für die VMs aktivieren.
- Für die automatische Installation. Site Recovery benötigt ein Konto mit Zugriffsberechtigungen auf die VM. 
- Kontodetails werden während der Einrichtung der Replikation eingegeben. 
- Das Konto kann ein Domänenkonto oder ein lokales Konto sein, solange es über Installationsberechtigungen verfügt.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover in Azure möchte Contoso eine Verbindung mit den VMs in Azure herstellen können. Hierzu muss das Unternehmen mehrere Schritte durchführen: 

- Für den Zugriff über das Internet muss Contoso vor der Migration SSH auf der lokalen Linux-VM aktivieren.  Bei Ubuntu kann dieser Vorgang mithilfe des folgenden Befehls durchgeführt werden: **Sudo apt-get ssh install -y**.
- Nach dem Failover sollte das Unternehmen die **Startdiagnose** überprüfen, um einen Screenshot der VM anzuzeigen.
- Falls dies nicht funktioniert, müssen die Verantwortlichen überprüfen, ob die VM ausgeführt wird, und folgende [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) lesen.

**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) zum Erstellen und Zuweisen von Rollen für die automatische Ermittlung.
- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) zum Erstellen eines Kontos für die Pushinstallation von Mobility Service.


## <a name="step-3-provision-azure-database-for-mysql"></a>Schritt 3: Bereitstellen von Azure Database for MySQL

Contoso stellt eine MySQL-Datenbankinstanz in der primären Region für die USA, Osten 2, zur Verfügung.

1. Im Azure-Portal erstellen die Verantwortlichen von Contoso eine Azure Database for MySQL-Ressource. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Sie fügen den Namen **contosoosticket** für die Azure-Datenbank hinzu. Sie fügen die Datenbank zur Ressourcengruppe für die Produktion **ContosoRG** hinzu, und geben Anmeldeinformationen für sie an.
3. Die lokale MySQL-Datenbank ist in Version 5.7 vorhanden. Deshalb wählen sie diese Version für Kompatibilitätszwecke. Sie verwenden die Standardgrößen, die ihren Datenbankanforderungen entsprechen.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. Für **Optionen für Sicherungsredundanz** wählt Contoso die Verwendung von **Georedundanz** aus. Mit dieser Option können sie ihre Datenbank in der sekundären Region „USA Mitte“ wiederherstellen, wenn ein Ausfall auftritt. Sie können diese Option nur konfigurieren, wenn sie die Datenbank zur Verfügung stellen.

     ![Redundanz](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. Im Netzwerk **VNET-PROD-EUS2** > **Dienstendpunkte** fügen sie einen Dienstendpunkt (ein Datenbanksubnetz) für den SQL-Dienst hinzu.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Nach dem Hinzufügen des Subnetzes erstellen sie eine virtuelle Netzwerkregel, die Zugriff vom Datenbanksubnetz im Produktionsnetzwerk zulässt.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>Schritt 4: Replizieren der lokalen VMs

Bevor Contoso die Web-VM zu Azure migrieren kann, muss das Unternehmen die Replikation einrichten und aktivieren.

### <a name="set-a-protection-goal"></a>Festlegen eines Schutzziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel festgelegt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass sich seine Computer in einer lokalen Umgebung befinden, dass es sich um VMware-VMs handelt und diese in Azure repliziert werden sollen.

    ![Replikationsziel](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, bestätigt es durch Auswahl von **Ja, ist abgeschlossen**, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Szenario nur eine VM. Daher ist keine Bereitstellungsplanung erforderlich.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso muss seine Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage verwendet, die für die Bereitstellung des Site Recovery-Konfigurationsservers als hoch verfügbare, lokale VMware-VM verwendet wird. Nachdem der Konfigurationsserver eingerichtet wurde und ausgeführt wird, wird er im Tresor registriert.

Auf dem Konfigurationsserver werden einige Komponenten ausgeführt:

- Die Konfigurationsserverkomponente, die die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.

Contoso führt diese Schritte wie folgt aus:


1. Das Unternehmen lädt die OVF-Vorlage von **Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver** herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Es importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert die Lizenzvereinbarung und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet sich Contoso als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann.
7. Nachdem die Verbindung hergestellt wurde, meldet sich das Unternehmen bei seinem Azure-Abonnement an. Mit den Anmeldeinformationen muss der Zugriff auf den Tresor möglich sein, in dem der Konfigurationsserver registriert werden soll.

    ![Konfigurationsserver registrieren](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
9. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
10. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
11. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.

    ![Tresor](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Anschließend werden MySQL Server und VMWare PowerCLI heruntergeladen und installiert. 
13. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des Servers von vCenter Server oder des vSphere-Hosts an. Der Standardport wird beibehalten, und für den Server von vCenter Server wird ein Anzeigename angegeben.
14. Contoso gibt das für die automatische Ermittlung erstellte Konto und die Anmeldeinformationen für die automatische Installation von Mobility Service an. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Nach Abschluss der Registrierung im Azure-Portal überprüft Contoso, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern. 
15. Nun stellt Site Recovery eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Contoso gibt nun die Zielreplikationseinstellungen ein.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.


### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Nach der Einrichtung der Quelle und des Ziels kann Contoso nun eine Replikationsrichtlinie erstellen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) zum Azure-Gast-Agent für Linux.

### <a name="enable-replication-for-the-web-vm"></a>Aktivieren einer Replikation für die Web-VM

Contoso kann nun mit der Replikation der VM **OSTICKETWEB** beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Die Verantwortlichen im Unternehmen wählen aus, dass VMs aktiviert werden sollen. Darüber hinaus wählen sie Quelleneinstellungen einschließlich der Einstellungen für den Server von vCenter Server und den Konfigurationsserver aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Nun legen sie die Zieleinstellungen fest. Diese beinhalten die Ressourcengruppe und das Netzwerk, die als Speicherort für die Azure-VM nach dem Failover dienen, und das Speicherkonto, in dem die replizierten Daten gespeichert werden. 

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Contoso wählt **OSTICKETWEB** für die Replikation aus. 

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. In den VM-Eigenschaften wählen Sie das Konto aus, das zur automatischen Installation von Mobility Service auf der VM verwendet werden sollte.

     ![Mobilitätsdienst](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. Unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** überprüft das Unternehmen, ob die richtige Replikationsrichtlinie angewendet wird, und wählt **Replikation aktivieren** aus. Der Mobilitätsdienst wird automatisch installiert.
6.  Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.


**Benötigen Sie weitere Hilfe?**

Unter [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication) finden Sie eine vollständige exemplarische Vorgehensweise mit all diesen Schritten.


## <a name="step-5-migrate-the-database"></a>Schritt 5: Migrieren der Datenbank

Contoso migriert die Datenbank durch Sicherung und Wiederherstellung mit MySQL-Tools. Das Unternehmen installiert MySQL Workbench, speichert die Datenbank aus OSTICKETMYSQL und stellt sie dann wieder in der Azure Database for MySQL Server her.

### <a name="install-mysql-workbench"></a>Installieren von MySQL Workbench

1. Contoso überprüft die [Voraussetzungen und lädt MySQL Workbench herunter](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Die Verantwortlichen bei Contoso installieren MySQL Workbench for Windows entsprechend den [Installationsanweisungen](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. In MySQL Workbench erstellen sie eine MySQL-Verbindung mit OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Sie exportieren die Datenbank als **Osticket** in eine lokale, eigenständige Datei.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Nachdem die Datenbank lokal gesichert wurde, erstellen sie eine Verbindung mit der Azure Database for MySQL-Instanz.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Jetzt kann Contoso die Datenbank aus der eigenständige Datei in die Azure MySQL-Instanz importieren (wiederherstellen). Ein neues Schema (Osticket) wird für die Instanz erstellt.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>Schritt 6: Migrieren von VMs mit Site Recovery

Contoso führt ein schnelles Testfailover aus und migriert anschließend die VMs.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Das Ausführen eines Testfailovers dient zur Sicherstellung, dass vor der Migration alles wie erwartet funktioniert. 

1. Contoso führt ein Testfailover zum letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Testfailover durchführen: 

    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Contoso vergewissert sich, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. 
4. Nach der Überprüfung bereinigt es das Failover. Darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert.

### <a name="migrate-the-vm"></a>Migrieren der VM

Um die VM zu migrieren, erstellt Contoso einen Wiederherstellungsplan, der die VM enthält, und führt ein Failover für den Plan für Azure aus.

1. Contoso erstellt einen Plan und fügt **OSTICKETWEB** hinzu.

    ![Wiederherstellungsplan](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Das Unternehmen führt ein Failover für den Plan aus. Die Verantwortlichen bei Contoso entscheiden, ein Failover für den letzten Wiederherstellungspunkt auszuführen. Zudem gibt das Unternehmen an, dass Site Recovery versuchen sollte, die lokale VM vor dem Auslösen des Failovers herunterzufahren. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Während des Failovers gibt der vCenter Server Befehle zum Beenden der beiden VMs aus, die auf dem ESXi-Host ausgeführt werden.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. Nach der VM-Überprüfung wird die Migration abgeschlossen. Dadurch werden die Replikation für die VM und die Site Recovery-Abrechnung für die VM beendet.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers. 
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.


### <a name="connect-the-vm-to-the-database"></a>Verbinden der VM mit der Datenbank

Im letzten Schritt im Migrationsprozess aktualisiert Contoso die Verbindungszeichenfolge der App, um auf die Azure Database for MySQL zu verweisen. 

1. Das Unternehmen stellt über Putty oder einen anderen SSH-Client eine SSH-Verbindung mit der OSTICKETWEB-VM her. Die VM ist privat, daher stellt Contoso eine Verbindung über die private IP-Adresse her.

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Sie aktualisieren die Einstellungen, dass die VM **OSTICKETWEB** mit der Datenbank **OSTICKETMYSQL** kommunizieren kann. Die Konfiguration ist mit der lokalen IP-Adresse 172.16.0.43 derzeit hartcodiert.

    **Vor dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Nach dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Das Unternehmen startet den Dienst mit dem Befehl **systemctl restart apache2** neu.

    ![Neu starten](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Schließlich aktualisiert Contoso die DNS-Datensätze für **OSTICKETWEB** auf einem der Contoso-Domänencontroller.

    ![DNS-Aktualisierung](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach Abschluss der Migration werden die Stufen der App „osTicket“ auf Azure-VMs ausgeführt.

Contoso muss nun wie folgt vorgehen: 
- Es entfernt die lokalen VMWare-VMs aus dem vCenter-Bestand.
- Es entfernt die lokalen VMs aus lokalen Sicherungsaufträgen.
- Es aktualisiert die interne Dokumentation und zeigt neue Speicherorte und IP-Adressen an. 
- Das Unternehmen überprüft sämtliche Ressourcen, die mit den lokalen VMs interagieren, und aktualisiert sämtliche relevanten Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.
- Contoso hat mithilfe des Azure Migrate-Diensts durch die Zuordnung von Abhängigkeiten die VM **OSTICKETWEB** für die Migration bewertet. Die Contoso-Mitarbeiter sollten nun die Agents (Microsoft Monitoring Agent und Dependency-Agent), die für diesen Zweck installiert wurden, von der VM entfernen.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die App jetzt ausgeführt wird, muss Contoso seine neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüft die VM und die Datenbank, um mögliche Sicherheitsprobleme zu ermitteln.

- Es überprüft die Netzwerksicherheitsgruppen (NSGs) der VMs zur Steuerung des Zugriffs. Mithilfe von NSGs wird sichergestellt, dass nur für die App zulässiger Datenverkehr übergeben werden kann.
- Das Sicherheitsteam zieht darüber hinaus in Erwägung, die Daten auf den VM-Datenträgern über Datenträgerverschlüsselung und Azure KeyVault zu sichern.
- Die Kommunikation zwischen der VM und der Datenbankinstanz ist nicht für SSL konfiguriert. Das Team muss dies tun, um sicherzustellen, dass der Datenbankverkehr nicht gehackt werden kann.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) über Sicherheitsverfahren für VMs.

### <a name="backups"></a>Backups

- Contoso sichert die Daten auf den VMs mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- Das Unternehmen muss die Sicherung für die Datenbank nicht konfigurieren. Azure Database for MySQL erstellt automatisch Serversicherungen und -speicher. Es hat sich für die Georedundanz für die Datenbank entschieden, damit sie robust und einsatzbereit ist.

### <a name="licensing-and-cost-optimization"></a>Lizenzierungs- und Kostenoptimierung

- Nach der Bereitstellung von Ressourcen, weist Contoso Azure-Tags in Übereinstimmung mit Entscheidungen zu, die während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md#set-up-tagging) gemacht wurden.
- Es gibt keine Lizenzierungsprobleme für die Contoso Ubuntu-Server.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management.


## <a name="next-steps"></a>Nächste Schritte

In diesem Szenario haben wir das letzte Szenario für eine Zuweisung eines neuen Hosts gezeigt, das Contoso ausprobiert hat. Das Contoso-Team hat die Front-End-VM der lokalen Linux-OsTicket-App zu einer Azure-VM und die App-Datenbank zu einer Azure MySQL-Instanz migriert.

In der nächsten Tutorial-Reihe der Migrationsserie werden wir Ihnen zeigen, wie Contoso eine komplexere Reihe an Migrationen durchgeführt hat, einschließlich einer App-Umgestaltung anstelle von einfachen Lift-and-Shift-Migrationen.

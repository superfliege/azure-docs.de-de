---
title: Zuweisen eines neues Hosts für eine Contoso-App in Azure durch Migration zu einer Azure-VM und verwalteten Azure SQL-Instanz | Microsoft-Dokumentation
description: Erfahren Sie, wie Contoso eine lokale App erneut auf Azure-VMs und verwalteten Azure SQL-Instanzen hostet.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: c7dc9e8406494739aa5d8f21397a606e0b74a617
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301251"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-azure-sql-managed-instance"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale App zu Azure-VMs und einer verwalteten Azure SQL-Instanz

In diesem Artikel erfahren Sie, wie Contoso die Frontend-VM seiner SmartHotel-App mit dem Azure Site Recovery-Diensts zu Azure VMs und die App-Datenbank zu einer verwalteten Azure SQL-Instanz migriert.

> [!NOTE]
> Verwaltete Azure SQL-Instanz befindet sich derzeit in der Vorschauphase.

Dieses Dokument ist das vierte in einer Reihe von Artikeln, die dokumentieren, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und einige Szenarios, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird und wie verschiedene Migrationstypen ausgeführt werden. Die Szenarien werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Contoso-Migrationsszenarien wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen SmartHotel-App auf einer VMware ausführt. Sie bewerten App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz (dieser Artikel) | Veranschaulicht, wie Contoso die SmartHotel-App zu Azure migriert. Sie migrieren die App-Frontend-VM mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), und verwenden die App-Datenbank und den Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview), um zu einer verwalteten SQL-Instanz zu migrieren. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der SmartHotel-App nur mit Site Recovery migriert.
[Artikel 6: Zuweisen eines neuen Hosts zu Azure-VMs und SQL Server-Verfügbarkeitsgruppen](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die SmartHotel-App migriert. Sie verwenden Site Recovery zum Migrieren der App-VMs und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-Verfügbarkeitsgruppe. | Verfügbar
[Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure VMs migriert. | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs und Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery und MySQL Workbench zu Azure-VMs und zu einer Azure MySQL Server-Instanz migriert. | Verfügbar

Wenn Sie die in diesem Artikel verwendete SmartHotel-Beispiel-App verwenden möchten, können Sie diese in [Github](https://github.com/Microsoft/SmartHotel360) herunterladen.

## <a name="on-premises-architecture"></a>Lokale Architektur

Hier ist eine Darstellung der aktuellen lokalen Infrastruktur von Contoso.

![Contoso-Architektur](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso verfügt über ein Hauptrechenzentrum in New York City im Osten der USA.
- Zudem hat das Unternehmen drei weitere Standorte in den USA.
- Das Hauptrechenzentrum ist über eine Metro-Ethernet-Glasfaserverbindung (500 Mbit/s) mit dem Internet verbunden.
- Jeder Standort ist lokal über Verbindungen der Unternehmensklasse mit dem Internet verbunden, wobei IPSec-VPN-Tunnel zurück zum Hauptrechenzentrum führen. Dies ermöglicht eine dauerhafte Verbindung des gesamten Netzwerks und optimiert die Internetkonnektivität.
- Das Hauptrechenzentrum ist vollständig mit VMware virtualisiert. Contoso verfügt über zwei ESXi 6.5-Virtualisierungshosts, die von vCenter Server 6.5 verwaltet werden.
- Für die Identitätsverwaltung und DNS-Server im internen Netzwerk wird Active Directory verwendet.
- Die Domänencontroller im Rechenzentrum werden auf VMware-VMs ausgeführt. Die Domänencontroller an den lokalen Standorten werden auf physischen Servern ausgeführt.



## <a name="business-drivers"></a>Geschäftsfördernde Faktoren

Das IT-Führungsteam hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was die Unternehmen mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und gerät mit seinen lokalen Systemen und Infrastrukturen an seine Grenzen.
- **Effizienzsteigerung**: Contoso muss unnötige Prozeduren minimieren und Prozesse für seine Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schneller sein, den Kosten- und Zeitaufwand reduzieren und Kundenanforderungen schneller erfüllen.
- **Steigerung der Flexibilität**: Die IT-Verantwortlichen müssen schneller auf die Unternehmensanforderungen reagieren. Sie müssen schneller auf Marktveränderungen reagieren, um den Erfolg in einer globalen Wirtschaft zu garantieren.  IT darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Contoso muss IT-Systeme bereitstellen, die mit dem schnellen Wachstum des Unternehmens mithalten können.

## <a name="migration-goals"></a>Migrationsziele

Das Cloud-Team von Contoso hat sich folgende Ziele für die Migration gesetzt. Diese Ziele werden verwendet, um die beste Migrationsmethode zu bestimmen:

- Nach der Migration sollte die App in Azure die gleichen Leistungsmerkmale aufweisen wie die lokale VMware-Umgebung heute.  Die Umstellung auf die Cloud bedeutet nicht, dass die Leistung der App weniger wichtig ist.
- Contoso möchte nicht in diese App investieren.  Sie ist zwar wichtig für das Geschäft, in ihrer jetzigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Datenbankverwaltungsaufgaben sollten nach der Migration der App auf ein Minimum reduziert sein.
- Contoso möchte für diese App keine Azure SQL-Datenbank verwenden und sucht nach Alternativen.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Szenario:

- Contoso möchte seine lokale Reise-App mit zwei Ebenen migrieren.
- Die App ist auf zwei VMs (WEBVM und SQLVM) verteilt und befindet sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von einem vCenter Server 6.5 (**vcenter.contoso.com**) verwaltet, der auf einer VM ausgeführt wird.
- Das Unternehmen migriert die App-Datenbank (SmartHotelDB) zu einer verwalteten Azure SQL-Instanz.
- Die lokalen VMware-VMs werden zu einer Azure-VM migriert.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach der Migration stillgelegt.

![Szenarioarchitektur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenbankverwaltungsdienst](https://docs.microsoft.com/azure/dms/dms-overview) | DMS ermöglicht die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen bei minimaler Ausfallzeit. | Erfahren Sie mehr über die [unterstützten Regionen](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) für DMS und erhalten Sie eine [Preisübersicht](https://azure.microsoft.com/pricing/details/database-migration/).
[Verwaltete Azure SQL-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Die verwaltete Instanz ist ein verwalteter Datenbankdienst, der eine vollständig verwaltete SQL Server-Instanz in der Azure-Cloud darstellt. Sie basiert auf dem gleichen Code wie die neueste Version der SQL Server-Datenbank-Engine und verfügt über die neuesten Features, Leistungsverbesserungen und Sicherheitspatches. | Bei Verwendung verwalteter Azure SQL-Datenbank-Instanzen, die in Azure ausgeführt werden, fallen Gebühren je nach Kapazität an. [Weitere Informationen](https://azure.microsoft.com/pricing/details/sql-database/managed/) 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.

 

## <a name="migration-process"></a>Migrationsprozess

Contoso wird sowohl die Web- als auch die Datenschichten der SmartHotel-App zu Azure migrieren.

1. Contoso hat bereits eine Azure-Infrastruktur eingerichtet, sodass für dieses Szenario nur noch ein paar bestimmte Azure-Komponenten hinzugefügt werden müssen.
2. Die Datenschicht wird mit Database Migration Service (DMS) migriert.  DMS verbindet sich mit der lokalen SQL Server-VM über eine Standort-zu-Standort-VPN-Verbindung zwischen dem Contoso-Rechenzentrum und Azure und migriert dann die Datenbank.
3. Die Webschicht wird per Lift & Shift mit Azure Site Recovery migriert. Dies beinhaltet die Vorbereitung der lokalen VMware-Umgebung, das Einrichten und Aktivieren der Replikation und die Migration der VMs durch deren Ausfall zu Azure.

     ![Migrationsarchitektur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Voraussetzungen

Contoso (und Sie) benötigen folgendes für dieses Szenario:

**Anforderungen** | **Details**
--- | ---
**Registrierung für die Vorschau** | Sie müssen für die eingeschränkte öffentliche Vorschau der verwalteten SQL-Instanz registriert sein. Sie benötigen ein Azure-Abonnement, um sich [registrieren](https://portal.azure.com#create/Microsoft.SQLManagedInstance) zu können. Da sich die Registrierung über einige Tage erstrecken kann, stellen Sie sicher, dass Sie diesen Vorgang durchführen, bevor Sie mit der Bereitstellung dieses Szenarios beginnen.
**Azure-Abonnement** | Sie müssten bereits ein Abonnement angelegt haben, als Sie die Beurteilung im ersten Artikel dieser Serie durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Recovery (lokal)** | Ihr lokaler vCenter-Server sollte als Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.<br/><br/> Unterstützte Konfiguration für [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) und [Speicher](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage).
**DMS** | Für DMS benötigen Sie ein [kompatibles lokales VPN-Gerät](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Sie müssen das lokale VPN-Gerät konfigurieren können. Es muss über eine externe öffentliche IPv4-Adresse verfügen, und die Adresse darf sich nicht hinter einem NAT-Gerät befinden.<br/><br/> Stellen Sie sicher, dass Sie Zugriff auf Ihre lokale SQL Server-Datenbank haben.<br/><br/> Die Windows-Firewall sollte auf die Quelldatenbank-Engine zugreifen können. [Weitere Informationen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)<br/><br/> Wenn sich eine Firewall vor dem Datenbankcomputer befindet, fügen Sie Regeln hinzu, um den Zugriff auf die Datenbank und auf Dateien über SMB-Port 445 zuzulassen.<br/><br/> Die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldeinformationen müssen Mitglieder der Serverrolle „sysadmin“ sein.<br/><br/> Sie benötigen eine Netzwerkfreigabe in der lokalen Datenbank, die von DMS zum Sichern der Quelldatenbank verwenden werden kann.<br/><br/> Stellen Sie sicher, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt.<br/><br/> Notieren Sie sich einen Windows-Benutzer (mit dem Kennwort), der eine Berechtigung für Vollzugriff auf die Netzwerkfreigabe besitzt. Azure Database Migration Service verwendet diese Benutzeranmeldeinformationen, um Sicherungsdateien in den Azure-Speichercontainer hochzuladen.<br/><br/> Bei der SQL Server Express-Installation wird das TCP/IP-Protokoll standardmäßig auf **Deaktiviert** gesetzt. Stellen Sie sicher, dass es aktiviert ist.


## <a name="scenario-steps"></a>Szenarioschritte

Contoso plant die Bereitstellung wie folgt:

> [!div class="checklist"]
> * **Schritt 1: Einrichten einer verwalteten Azure SQL-Instanz**: Es wird eine bereits erstellte verwaltete Instanz benötigt, zu der die lokale SQL Server-Datenbank migriert wird.
> * **Schritt 2: Vorbereiten von DMS**: Der Database Migration-Anbieter muss registriert, eine Instanz erstellt und dann ein DMS-Projekt angelegt werden. Zudem muss ein SA-URI für die DMS eingerichtet werden. Ein SAS-URI (Shared Access Signature Uniform Resource Identifier) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto, damit Sie eingeschränkte Berechtigungen für Speicherobjekte erteilen können. Der SAS-URI wird so eingerichtet, dass DMS auf den Speicherkontocontainer zugreifen kann, in den der Dienst die SQL Server-Sicherungsdateien hochlädt.
> * **Schritt 3: Vorbereiten von Azure für Site Recovery**: Sie erstellen ein Azure-Speicherkonto zum Speichern replizierter Daten und einen Recovery Services-Tresor.
> * **Schritt 4: Vorbereiten der lokalen VMware für Site Recovery**: Contoso bereitet Konten für die VM-Ermittlung und Agent-Installation sowie eine Verbindung mit Azure-VMs nach dem Failover vor.
> * **Schritt 5: Replizieren von VMs**: Für die Replikation werden die Quell- und Zielumgebung für Site Recovery konfiguriert, eine Replikationsrichtlinie eingerichtet und die Replikation von VMs im Azure-Speicher gestartet.
> * **Schritt 6: Migrieren der Datenbank mit DMS**: Die Datenbankmigration kann jetzt ausgeführt werden.
> * **Schritt 7: Migrieren der VMs mit Site Recovery**: Sie führen einen Test-Failover durch, um sicherzustellen, dass alles funktioniert, und führen anschließend einen vollständigen Failover für die Migration der VMs zu Azure aus.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Schritt 1: Vorbereiten einer verwalteten Azure SQL-Instanz

Um eine verwaltete Azure SQL-Instanz einzurichten, benötigt Contoso ein Subnetz:

- Das Subnetz muss dediziert sein. Es muss leer sein, darf keinen anderen Clouddienst enthalten und darf kein Gatewaysubnetz sein.
- Nachdem die verwaltete Instanz erstellt wurde, dürfen Sie dem Subnetz keine Ressourcen hinzufügen.
- Dem Subnetz darf keine NSG zugeordnet sein.
- Das Subnetz muss eine benutzerdefinierte Routingtabelle (User Route Table, UDR) aufweisen, der die Route vom Typ 0.0.0.0/0, nächster Hop „Internet“ als einzige Route zugewiesen ist. 
- Optionales benutzerdefiniertes DNS: Wenn ein benutzerdefiniertes DNS für das VNET angegeben ist, muss die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) zur Liste hinzugefügt werden. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns)
- Dem Subnetz darf kein Dienstendpunkt (Speicher oder SQL) zugeordnet sein. Dienstendpunkte sollten im virtuellen Netzwerk deaktiviert sein.
- Das Subnetz muss mindestens 16 IP-Adressen aufweisen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) zum Festlegen der Größe des Subnetzes für die verwaltete Instanz.
- In der Hybridumgebung werden benutzerdefinierte DNS-Einstellungen benötigt. Contoso konfiguriert DNS-Einstellungen, um einen oder mehrere seiner Azure DNS-Server zu verwenden. [Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) zur DNS-Anpassung.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Einrichten eines virtuellen Netzwerks für die verwaltete Instanz

Contoso richtet das VNET wie folgt ein: 

1. Contoso erstellt ein neues VNET (VNET-SQLMI-EU2) in der primären Region „USA, Osten 2“ in der Ressourcengruppe ContosoNetworkingRG.
2. Contoso weist einen Adressraum von 10.235.0.0/24 zu, sodass sich der Bereich nicht mit anderen Netzwerken im Contoso Unternehmen überschneidet.
2. Dem Netzwerk werden zwei Subnetze hinzugefügt:
    - SQLMI-DS-EUS2 (10.235.0.0.25)
    - SQLMI-SAW-EUS2 (10.235.0.128/29). Dieses Subnetz wird verwendet, um ein Verzeichnis an die verwaltete Instanz (SQLMI) anzufügen.

    ![Netzwerk für verwaltete Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Nach der Installation von VNET und der Subnetze wird ein Peering für die Netzwerke wie folgt ausgeführt:

    - Peering von VNET-SQLMI-EUS2 mit VNET-HUB-EUS2 (dem Hub VNET für „USA, Osten 2“).
    - Peering von VNET-SQLMI-EUS2 mit VNET-FA-EUS2 (dem Produktionsnetzwerk).

    ![Peering in Netzwerken](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso legt benutzerdefinierte DNS-Einstellungen fest. DNS verweist zunächst auf die Azure-Rechenzentren. Azure DNS ist sekundär. Die Azure-Rechenzentren von Contoso befinden sich an folgenden Orten:

    - Befindet sich im Subnetz PROD-DC-EUS2 im Produktionsnetzwerk „USA, Osten 2“ (VNET-FA-EUS2).
    - CONTOSODC3-Adresse: 10.245.42.4
    - CONTOSODC4-Adresse: 10.245.42.5
    - Azure DNS-Auflösung: 168.63.129.16

     ![Netzwerk-DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Benötigen Sie weitere Hilfe?**

- [Verschaffen Sie sich einen Überblick](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) über verwaltete Azure SQL-Instanzen.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) über das Erstellen eines VNETs für verwaltete SQL-Instanzen.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) über das Einrichten von Peerings.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) über das Aktualisieren von Azure AD-DNS-Einstellungen.



### <a name="set-up-routing"></a>Einrichten des Routings

Die verwaltete Instanz wird in einem privaten VNET platziert, damit Contoso eine Routingtabelle für die Kommunikation mit dem Azure-Verwaltungsdienst benötigt. Wenn keine Kommunikation mit dem Dienst möglich ist, kann nicht auf ihn zugegriffen werden.

- Die Routingtabelle enthält einen Satz von Regeln (Routen), der angibt, wie von der verwalteten Instanz gesendete Pakete im VNET weitergeleitet werden sollen.
- Die Routingtabelle bezieht sich auf Subnetze, in denen verwalteten Instanzen bereitgestellt werden. Jedes Paket, das ein Subnetz verlässt, wird auf Grundlage der entsprechenden Routingtabelle verarbeitet.
- Subnetze können jeweils nur einer Routingtabelle zugeordnet werden.
- Für die Erstellung von Routingtabellen in Microsoft Azure fallen keine zusätzlichen Gebühren an.

1. Contoso erstellt eine benutzerdefinierte Routingtabelle. Die Routingtabelle wird in der Ressourcengruppe ContosoNetworkingRG erstellt.

    ![Routingtabelle](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Contoso fügt nach der Bereitstellung der Routingtabelle (MIRouteTable) eine Route mit einem 0.0.0.0/0-Adresspräfix entsprechend den Anforderungen für die verwaltete Instanz hinzu und legt als **Typ des nächsten Hops** **Internet** fest.

    ![Routingtabellenpräfix](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso ordnet die Routingtabelle dem Subnetz SQLMI-DB-EUS2 (im Netzwerk VNET-SQLMI-EUS2) zu. 

    ![Routingtabellensubnetz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Benötigen Sie weitere Hilfe?**

[Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) über das Einrichten von Routen für eine verwaltete Instanz.

### <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

Contoso kann jetzt eine verwaltete SQL-Datenbank-Instanz bereitstellen.

1. Da die verwaltete Instanz als Geschäfts-App dient, setzt Contoso sie in der primären Region „USA, Osten 2“ in der Ressourcengruppe ContosoRG ein. 
2. Es wird eine Preisstufe ausgewählt und die Größe der Computeleistung sowie der Speicherplatz für die Instanz festgelegt. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/sql-database/managed/) zu den Preisen.

    ![Verwaltete Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nachdem der Bereitstellung der verwaltete Instanz werden zwei neue Ressourcen in der Ressourcengruppe ContosoRG angezeigt:

    - Ein virtueller Cluster bei Vorhandensein mehrerer verwalteter Instanzen
    - Die verwaltete SQL Server-Instanz. 

    ![Verwaltete Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Benötigen Sie weitere Hilfe?**

[Erfahren Sie mehr über](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) das Bereitstellen einer verwalteten Instanz.

## <a name="step-2-prepare-dms"></a>Schritt 2: Vorbereiten von DMS

Zur Vorbereitung von DMS muss Contoso folgende Aufgaben durchführen:

- Registrieren des DMS-Anbieters in Azure
- Bereitstellen von DMS mit Zugriff auf Azure-Speicher für das Hochladen der zum Migrieren einer Datenbank verwendeten Sicherungsdateien Zu diesem Zweck muss ein Blob Storage-Container erstellt werden und ein SAS-URI (Shared Access Signature) dafür generiert werden. 
- Erstellen eines DMS-Projekts


Es müssen folgende Schritte ausgeführt werden:

1. Registrieren des Database Migration-Anbieters im Abonnement
    ![DMS-Registrierung](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Erstellen eines Blob Storage-Containers und Generieren eines SAS-URI für den DMS-Zugriff

    ![SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Erstellen einer DMS-Instanz 

    ![DMS-Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso platziert die DMS-Instanz im Subnetz PROD-DC-EUS2 im VNET VNET-FA-DC-EUS2.
    - Sie wird dort platziert, da sie sich in einem VNET befinden muss, das über ein VPN-Gateway auf die lokale SQL Server-VM zugreifen kann.
    - Für VNET-FA-EUS2 wird ein Peering mit VNET-HUB-EUS2 ausgeführt und die Verwendung von Remotegateways gestattet.  Dadurch wird sichergestellt, dass DMS bei Bedarf kommunizieren kann.

        ![DMS-Netzwerk](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Benötigen Sie weitere Hilfe?**
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) über das Einrichten von DMS.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) über das Erstellen und das Verwenden von SAS.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Schritt 3: Vorbereiten von Azure für den Site Recovery-Dienst

Es gibt diverse Azure-Elemente, die Contoso zum Einrichten von Site Recovery für die Migration der Webschicht-VM (WEBMV) benötigt.

- Ein VNET, in dem sich Ressourcen befinden, für die ein Failover ausgeführt wurde.
- Ein neues Azure-Speicherkonto für die Speicherung replizierter Daten. 
- Einen Recovery Services-Tresor in Azure.

Site Recovery wird wie folgt eingerichtet:

1. Da die VM ein Web-Front-End der SmartHotel-App ist, wird für die VM ein Failover in ihrem vorhandenen Produktionsnetzwerk (VNET-PROD-EUS2) und Subnetz (PROD-FE-EUS2) in der primären Region „USA, Osten 2“ ausgeführt.  Die Einrichtung dieses Netzwerks erfolgte mit der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md).
2. Es wird ein Azure-Speicherkonto (contosovmsacc20180528) erstellt. Das Unternehmen verwendet hierzu ein Konto für allgemeine Zwecke mit Standardspeicher und LRS-Replikation.

    ![Site Recovery-Speicher](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Nach dem Einrichten des Netzwerks und des Speicherkontos kann Contoso nun einen Tresor (ContosoMigrationVault) erstellen und diesen in der Ressourcengruppe ContosoFailoverRG in der primären Region „USA, Osten 2“ platzieren.

    ![Recovery Services-Tresor](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Benötigen Sie weitere Hilfe?**

[Erfahren Sie mehr](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) über das Einrichten von Azure für Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 4: Vorbereiten der lokalen VMware für Site Recovery

Zum Vorbereiten der VMware für Site Recovery muss Contoso folgende Schritte ausführen:

- Vorbereiten eines Kontos auf dem vCenter-Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung
- Erstellen eines Kontos für die automatische Installation des Mobility Service auf VMware-VMs, die repliziert werden sollen
- Vorbereiten lokaler VMs, damit diese mit Azure-VMs verbunden werden können, wenn diese nach der Migration erstellt werden


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso richtet das Konto wie folgt ein:

1. Es wird eine Rolle auf der vCenter-Ebene erstellt.
2. Anschließend werden dieser Rolle die erforderlichen Berechtigungen zugewiesen.

**Benötigen Sie weitere Hilfe?**

[Erfahren Sie mehr](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) über das Erstellen und Zuweisen von Rollen für die automatische Ermittlung.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobility Service muss auf der VM installiert sein, die Sie replizieren möchten.

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn Sie die Replikation für die VM aktivieren.
- Für die automatische Pushinstallation müssen Sie ein Konto vorbereiten, mit dem Site Recovery auf die VM zugreift.
- Sie geben dieses Konto an, wenn Sie die Replikation in der Azure-Konsole einrichten.
- Sie benötigen ein Domänen- oder lokales Konto mit Berechtigungen zum Installieren auf der VM.

**Benötigen Sie weitere Hilfe?**

[Erfahren Sie mehr](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) über das Erstellen eines Kontos für die Pushinstallation des Mobility Service.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover in Azure möchte Contoso eine Verbindung mit den replizierten VMs in Azure herstellen können. Zu diesem Zweck müssen einige Schritte auf der lokalen VM ausgeführt werden, bevor die Migration durchgeführt werden kann: 

1. Für den Zugriff über das Internet muss RDP auf der lokalen VM aktiviert werden, bevor das Failover ausgeführt wird. Zudem muss sichergestellt werden, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden, und dass RDP unter **Windows-Firewall** > **Zulässige Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über eine Standort-zu-Standort-Verbindung muss RDP auf dem lokalen Computer aktiviert werden, und sichergestellt werden, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für die Netzwerke der Typen **Domäne** und Privat zugelassen ist.
3. Die SAN-Richtlinie des Betriebssystems auf dem lokalen virtuellen Computer muss auf **OnlineAll** festgelegt werden.

Bei der Ausführung eines Failovers sind folgende Aspekte zu überprüfen:

- Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn ein Failover ausgelöst wird. Andernfalls ist nach Abschluss des Updates die Anmeldung auf dem virtuellen Computer nicht mehr möglich.
- Nach dem Failover sollte die **Startdiagnose** überprüft werden, um einen Screenshot des virtuellen Computers anzuzeigen. Falls dies nicht funktioniert, sollte überprüft werden, ob die VM ausgeführt wird, und die folgenden [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gelesen werden.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Schritt 5: Replizieren der lokalen VMs in Azure mit Site Recovery

Vor dem Ausführen einer Migration zu Azure muss Contoso die Replikation einrichten und die Replikation für die lokalen VMs aktivieren.

### <a name="set-a-replication-goal"></a>Festlegen eines Replikationsziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel festgelegt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass sich seine Computer in einer lokalen Umgebung befinden, dass es sich um VMware-VMs handelt und diese in Azure repliziert werden sollen.

    ![Replikationsziel](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, muss es durch Auswahl von **Ja, ist abgeschlossen** bestätigen, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Bereitstellungsszenario nur eine VM. Daher ist keine Bereitstellungsplanung erforderlich.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso muss nun seine Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage heruntergeladen, die für die Bereitstellung des Konfigurationsservers und der zugehörigen Komponenten als hochverfügbare, lokale VMware-VM verwendet wird. Zu den Komponenten auf dem Server gehören:

- Der Konfigurationsserver, der die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.
- Nachdem die Konfigurationsserver-VM erstellt und gestartet wurde, kann Contoso sie im Tresor registrieren.


Contoso führt diese Schritte wie folgt aus:

1. Das Unternehmen lädt die OVF-Vorlage im Azure-Portal (**Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver**) herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Das Unternehmen importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert die Lizenzvereinbarung und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet sich Contoso als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich beim Azure-Abonnement anzumelden. Mit den Anmeldeinformationen muss der Zugriff auf den Tresor möglich sein, in dem der Konfigurationsserver registriert werden soll. 

    [Konfigurationsserver registrieren](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
8. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
9. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.
        ![Tresor](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Anschließend werden MySQL Server und VMWare PowerCLI heruntergeladen und installiert. Dann werden die Servereinstellungen validiert.
11. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des vCenter-Servers oder des vSphere-Hosts an. Der Standardport wird beibehalten, und für den vCenter-Server in Azure wird ein Anzeigename angegeben.
12. Es muss das Konto angegeben werden, das zuvor erstellt wurde, damit Site Recovery automatisch VMware-VMs erkennen kann, die für die Replikation verfügbar sind. 
13. Contoso gibt die Anmeldedaten für die automatische Installation des Mobility Service an, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den VMs. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Contoso überprüft nach Abschluss der Registrierung im Azure-Portal erneut, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern. 
8. Site Recovery stellt anschließend mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Nun muss Contoso die Zielumgebung für die Replikation konfigurieren.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Nach der Einrichtung der Quelle und des Ziels kann Contoso nun eine Replikationsrichtlinie erstellen und diese dem Konfigurationsserver zuordnen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.

### <a name="enable-replication"></a>Aktivieren der Replikation

Contoso kann nun mit der Replikation der WebVM beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Das Unternehmen legt fest, dass virtuelle Computer aktiviert werden sollen. Darüber hinaus wählt es den vCenter-Server und den Konfigurationsserver aus.

 ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Contoso gibt nun die Zieleinstellungen, einschließlich der Ressourcengruppe und des Netzwerks als Speicherort für die Azure-VM nach dem Failover, und das Speicherkonto an, in dem die replizierten Daten gespeichert werden.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso wählt die WebVM für die Replikation aus. Site Recovery installiert Mobility Service auf jeder VM, wenn Sie die Replikation dafür aktivieren. 

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso prüft, ob die richtige Replikationsrichtlinie ausgewählt ist, und aktiviert die Replikation für WEBVM. Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.
6. Unter **Zusammenfassung** im Azure-Portal kann Contoso die Struktur für die VMs anzeigen, die in Azure repliziert werden.

    ![Infrastrukturansicht](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Benötigen Sie weitere Hilfe?**

Unter [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication) finden Sie eine vollständige exemplarische Vorgehensweise mit all diesen Schritten.

## <a name="step-6-migrate-the-database-with-dms"></a>Schritt 6: Migrieren der Datenbank mit DMS

Contoso muss ein DMS-Projekt erstellen und die Datenbank migrieren.

### <a name="create-a-dms-project"></a>Erstellen eines DMS-Projekts
1. Das Unternehmen erstellt ein DMS-Projekt. Der Quellservertyp wird als SQL Server und das Ziel als verwaltete Azure SQL-Datenbank-Instanz angegeben.

     ![DMS-Projekt](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Nach dem Erstellen des Projekts wird der Migrations-Assistent geöffnet.

### <a name="migrate-the-database"></a>Migrieren der Datenbank 

1. Im Migrations-Assistenten gibt Contoso die Quell-VM an, auf der sich die lokale Datenbank befindet, sowie die Anmeldeinformationen für den Zugriff darauf.

    ![DMS-Quelle](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Die Datenbank (SmartHotel.Registration) für die Migration wird ausgewählt.

    ![DMS-Quelldatenbank](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Als Ziel werden der Namen der verwalteten Instanz in Azure und die Zugangsdaten angegeben.

    ![DMS-Ziel](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Unter **+ neue Aktivität** > **Migration ausführen** werden die Einstellungen für die Ausführung der Migration angegeben:
    - Quell- und Zielanmeldeinformationen
    - Die zu migrierende Datenbank
    - Die Netzwerkfreigabe, die auf dem lokalen virtuellen Computer erstellt wurde DMS speichert Quellsicherungen in dieser Freigabe.
        - Das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, muss über Schreibberechtigungen für diese Freigabe verfügen.
        - Der FQDN-Pfad zur Freigabe muss angegeben werden.
    - Der SAS-URI, der DMS Zugriff auf den Speicherkontocontainer gewährt, in den der Dienst die Sicherungsdateien für die Migration hochlädt.

        ![DMS-Einstellungen](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Die Migration wird gespeichert und ausgeführt.
6. Der Migrationsstatus wird unter **Übersicht** überwacht.

    ![DMS-Überwachung](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Nach Abschluss der Migration stellt das Unternehmen sicher, dass die Zieldatenbanken in der verwalteten Instanz vorhanden sind.

    ![DMS-Überwachung](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Schritt 7: Migrieren der VM mit Site Recovery

Contoso führt einen schnellen Testfailover aus und migriert anschließend die VMs.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Das Ausführen eines Testfailovers dient zur Sicherstellung, dass vor der Migration von WEBVM alles wie erwartet funktioniert. 

1. Contoso führt ein Testfailover zum letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Das Testfailover wird durchgeführt: 

    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.
3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Contoso überprüft, ob alles ordnungsgemäß funktioniert. Das Unternehmen überprüft, ob die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. 
4. Nach der Überprüfung wird das Failover bereinigt, und darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert. 

### <a name="migrate-the-vm"></a>Migrieren der VM

1. Nachdem Contoso überprüft hat, ob das Testfailover wie erwartet funktioniert, erstellt das Unternehmen einen Wiederherstellungsplan für die Migration. Dem Plan wird WEBVM hinzugefügt.

     ![Wiederherstellungsplan](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Das Unternehmen führt einen Failover für den Plan aus. Es entscheidet, einen Failover für den letzten Wiederherstellungspunkt auszuführen. Zudem gibt das Unternehmen an, dass Site Recovery versuchen soll, die lokale VM vor dem Auslösen des Failovers herunterzufahren.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

   ![Wiederherstellungsplan](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Nach dieser Überprüfung führt das Unternehmen die Migration durch, um den Migrationsprozess zu beenden. Darüber hinaus beendet es die Replikation für die VM und die Site Recovery-Abrechnung für die VM.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Der letzte Schritt im Migrationsprozess besteht in der Aktualisierung der Verbindungszeichenfolge der Anwendung, um auf die migrierte Datenbank zu verweisen, die auf der verwalteten SQL-Instanz ausgeführt wird.

1. Um nach der neuen Verbindungszeichenfolge im Azure-Portal zu suchen, klickt das Unternehmen auf **Einstellungen** > **Verbindungszeichenfolgen**.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Die Zeichenfolge mit dem Benutzernamen und Kennwort der verwalteten SQL-Instanz wird aktualisiert.
3. Nachdem die Zeichenfolge konfiguriert wurde, wird die aktuelle Verbindungszeichenfolge in der Datei „web.config“ der Anwendung ersetzt.
4. Nachdem Contoso die Datei aktualisiert und gespeichert hat, startet das Unternehmen IIS auf WEBVM neu. Hierzu wird **IISRESET/RESTART** aus einer Eingabeaufforderung verwendet.
5. Nachdem IIS neu gestartet wurde, verwendet die App nun die Datenbank, die auf der verwalteten SQL-Instanz ausgeführt wird.
6. An diesem Punkt kann der lokale SQLVM-Computer heruntergefahren werden, und die Migration ist abgeschlossen.

**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers. 
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach der Migration wird die SmartHotel-App auf einer Azure-VM ausgeführt, und die SmartHotel-Datenbank befindet sich in der verwalteten Azure SQL-Instanz.  

Contoso muss nun folgende Bereinigungsmaßnahmen vornehmen:  

- Entfernen des WEBVM-Computers aus dem vCenter-Bestand
- Entfernen des SQLVM-Computers aus dem vCenter-Bestand
- Entfernen von WEBVM und SQLVM aus lokalen Sicherungsaufträgen
- Aktualisieren der internen Dokumentation und Anzeigen der neuen Speicherorte und IP-Adressen für WEBVM
- Entfernen von SQLVM aus der Dokumentation. Alternativ kann diese auch als gelöscht markiert werden, um sie nicht mehr im VM-Bestand anzuzeigen.
- Überprüfen sämtlicher Ressourcen, die mit den außer Betrieb genommenen VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Contoso-Sicherheitsteam überprüft die Azure-VMs und die verwaltete SQL-Instanz, um eventuelle Sicherheitsprobleme bei der Implementierung festzustellen.

- Es werden die Netzwerksicherheitsgruppen (NSGs) der VMs zur Steuerung des Zugriffs überprüft. Mithilfe von Netzwerksicherheitsgruppen wird sichergestellt, dass nur für die App zulässiger Datenverkehr an die App weitergeleitet wird.
- Das Sicherheitsteam zieht darüber hinaus in Erwägung, die Daten auf den Datenträgern über Azure Disk Encryption und Azure KeyVault zu sichern.
- Die Bedrohungserkennung für die verwaltete SQL-Instance (SQLMI) wird aktiviert. Es werden Benachrichtigungen an das Sicherheitsteam/Service Desk-System gesendet, um bei Erkennen einer Bedrohung ein Ticket zu öffnen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection)

     ![Sicherheit bei verwalteten Instanzen](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) über Sicherheitsverfahren für VMs.

### <a name="backups"></a>Backups
Contoso sichert die Daten auf der WEBVM mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

1. Contoso verfügt über eine Lizenzierung für die WEBVM und nutzt den Azure-Hybridvorteil.  Die vorhandenen Azure-VMs werden konvertiert, um von diesen Preisen zu profitieren.
2. Contoso aktiviert das durch Cloudyn, einem Tochterunternehmen von Microsoft, lizenzierte Azure Cost Management. Dabei handelt es sich um eine Multicloud-Kostenverwaltungslösung, die das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 


## <a name="conclusion"></a>Zusammenfassung

Im vorliegenden Artikel hat Contoso der SmartHotel-App in Azure einen neuen Host zugewiesen, indem das Unternehmen die Front-End-VM der App mithilfe des Site Recovery-Diensts zu Azure migriert hat. Contoso migrierte die lokale Datenbank zu einer verwalteten Azure SQL-Instanz mit DMS.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel der Reihe zeigen wir Ihnen, wie Contoso der Azure-VM der SmartHotel-App mithilfe des Azure Site Recovery-Diensts einen neuen Host zuweist.


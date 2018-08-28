---Datentitel: Zuweisen eines neuen Hosts für eine lokale Contoso-App durch Migration auf Azure-VMs und eine verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation, Beschreibung: Erfahren Sie, wie Contoso eine lokale App auf Azure-VMs und mithilfe einer verwalteten Azure SQL-Datenbank-Instanz erneut hostet.
services: site-recovery author: rayne-wiselman manager: carmonm ms.service: site-recovery ms.topic: conceptual ms.date: 08/13/2018 ms.author: raynew

---

# <a name="contoso-migration-rehost-an-on-premises-app-on-an-azure-vm-and-sql-database-managed-instance"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz

In diesem Artikel migriert Contoso seine Front-End-VM für die SmartHotel-App auf eine Azure-VM, indem der Azure Site Recovery-Dienst genutzt wird. Außerdem migriert Contoso die App-Datenbank zu einer verwalteten Azure SQL-Datenbank-Instanz.

> [!NOTE]
> Die verwaltete Azure SQL-Datenbank-Instanz befindet sich derzeit in der Vorschauphase.

Dieser Artikel ist der erste einer Artikelreihe, mit der dokumentiert wird, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und einige Szenarien, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird und verschiedene Arten von Migrationen durchgeführt werden. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Der Reihe werden im Laufe der Zeit weitere Artikel hinzugefügt.


Artikel | Details | Status
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Enthält eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die darin verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md) | Contoso führt eine Bewertung seiner lokalen zweischichtigen SmartHotel-App durch, die per VMware ausgeführt wird. Contoso bewertet die App-VMs mithilfe des Diensts [Azure Migrate](migrate-overview.md). Contoso bewertet die SQL Server-Datenbank der App mit dem [Datenmigrations-Assistenten](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
Artikel 4: Zuweisen eines neuen Hosts für eine App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz | Contoso führt für seine lokale SmartHotel-App per Lift & Shift-Vorgang eine Migration zu Azure durch. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank zu einer verwalteten Azure SQL-Datenbank-Instanz, indem der [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) verwendet wird. | Dieser Artikel
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der SmartHotel-App mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die SmartHotel-App. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe von Site Recovery per Lift & Shift-Vorgang eine Migration seiner Linux-App „osTicket“ zu Azure-VMs durch. | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert seine Linux-App „osTicket“ mithilfe von Site Recovery zu Azure-VMs. Die App-Datenbank wird zu Azure Database for MySQL migriert, indem MySQL Workbench verwendet wird. | Verfügbar
[Artikel 9: Umgestalten einer App in eine Azure-Web-App und in Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert seine SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in eine Azure-Web-App und in Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert seine Linux-App „osTicket“ an mehreren Standorten zu einer Azure-Web-App. Die Web-App wird in GitHub integriert, um die kontinuierliche Bereitstellung (Continuous Delivery) zu ermöglichen. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von Team Foundation Server in Visual Studio Team Services](contoso-migration-tfs-vsts.md) | Contoso migriert seine lokale Team Foundation Server-Bereitstellung zu Visual Studio Team Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer App in einen Azure-Container und Azure SQL-Datenbank](contoso-migration-rearchitect-container-sql.md) | Contoso migriert seine SmartHotel-App zu Azure und strukturiert die App anschließend um. Contoso strukturiert die App-Webschicht in einen Windows-Container und die App-Datenbank mithilfe von Azure SQL-Datenbank um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt seine SmartHotel-App neu, indem verschiedene Azure-Funktionen und -Dienste verwendet werden, z.B. Azure App Service, Azure Kubernetes Service, Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar

Sie können die SmartHotel-Beispiel-App, die in diesem Artikel verwendet wird, von [GitHub](https://github.com/Microsoft/SmartHotel360) herunterladen.

## <a name="on-premises-architecture"></a>Lokale Architektur


In diesem Diagramm ist die aktuelle lokale Infrastruktur von Contoso dargestellt:

![Aktuelle Contoso-Architektur](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso verfügt über ein zentrales Rechenzentrum. Das Rechenzentrum befindet sich in New York City, im Osten der USA.
- Contoso verfügt in den USA über drei weitere Niederlassungen.
- Das zentrale Rechenzentrum ist über eine auf Glasfaser basierende Metro-Ethernet-Verbindung (500 MBit/s) mit dem Internet verbunden.
- Jede Niederlassung ist lokal über Business-Class-Verbindungen mit dem Internet verbunden, und IPsec-VPN-Tunnel führen zurück zum zentralen Rechenzentrum. Aufgrund dieser Anordnung ist das gesamte Netzwerk von Contoso dauerhaft verbunden, und die Internetverbindung wird optimiert.
- Das Hauptrechenzentrum ist vollständig mit VMware virtualisiert. Contoso verfügt über zwei ESXi 6.5-Virtualisierungshosts, die mit vCenter Server 6.5 verwaltet werden.
- Contoso nutzt Active Directory für die Identitätsverwaltung. Contoso verwendet im internen Netzwerk DNS-Server.
- Die Domänencontroller im Rechenzentrum werden auf VMware-VMs ausgeführt. Die Domänencontroller an lokalen Standorten werden auf physischen Servern ausgeführt.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team von Contoso hat eng mit den Geschäftspartnern des Unternehmens zusammengearbeitet, um zu verstehen, was mit dieser Migration erreicht werden soll:

- **Steigerung des Unternehmenswachstums**: Contoso wächst. Aus diesem Grund hat der Druck auf die lokalen Systeme und die Infrastruktur des Unternehmens zugenommen.
- **Effizienzsteigerung**: Contoso muss unnötige Verfahren beseitigen und die Prozesse für seine Entwickler und Benutzer optimieren. Die IT-Abteilung muss schnell sein und darf weder Geld noch Zeit verschwenden, damit die Kundenanforderungen in kürzerer Zeit erfüllt werden können.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Sie muss schneller reagieren können, als sich die Gegebenheiten des Markts ändern, damit das Unternehmen auf dem globalen Markt Erfolg hat. Die IT-Abteilung von Contoso darf keine Hindernisse in den Weg stellen und keine Geschäftsabläufe blockieren.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die IT-Abteilung von Contoso Systeme bereitstellen, die mit der gleichen Geschwindigkeit mitwachsen können.

## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat sich Ziele für die Migration gesetzt. Das Unternehmen nutzt die Migrationsziele, um die beste Migrationsmethode zu ermitteln.

- Nach der Migration sollte die App in Azure die gleichen Leistungsmerkmale aufweisen, über die sie derzeit in der lokalen VMware-Umgebung von Contoso verfügt. Die Umstellung auf die Cloud bedeutet nicht, dass die Leistung der App weniger wichtig ist.
- Contoso möchte nicht in die App investieren. Die App hat eine entscheidende Bedeutung und ist wichtig für das Geschäft, aber Contoso möchte die App einfach in ihrer jetzigen Form in die Cloud verlagern.
- Datenbankverwaltungsaufgaben sollten nach der Migration der App auf ein Minimum reduziert sein.
- Contoso möchte für diese App keine Azure SQL-Datenbank-Instanz verwenden. Das Unternehmen ist auf der Suche nach Alternativen.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Szenario:

- Contoso möchte seine lokale zweischichtige Reise-App migrieren.
- Die App ist auf zwei VMs (**WEBVM** und **SQLVM**) verteilt und befindet sich auf einem VMware ESXi-Host der Version 6.5 (**contosohost1.contoso.com**). 
- Die VMware-Umgebung wird per vCenter Server 6.5-Software (**vcenter.contoso.com**) verwaltet, die auf einer VM ausgeführt wird.
- Contoso migriert die App-Datenbank (**SmartHotelDB**) zu einer verwalteten Azure SQL-Datenbank-Instanz.
- Contoso migriert die lokalen VMware-VMs zu einer Azure-VM.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) und einen lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.

![Szenarioarchitektur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure-Dienste

Dienst | BESCHREIBUNG | Kosten
--- | --- | ---
[Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Der Database Migration Service ermöglicht die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen bei minimaler Ausfallzeit. | Informieren Sie sich über die [unterstützten Regionen](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) und die [Preise für den Database Migration Service](https://azure.microsoft.com/pricing/details/database-migration/).
[Verwaltete Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Die verwaltete Azure SQL-Datenbank-Instanz ist ein verwalteter Datenbankdienst, der eine vollständig verwaltete SQL Server-Instanz in der Azure-Cloud darstellt. Sie nutzt den gleichen Code wie die neueste Version der Microsoft SQL Server-Datenbank-Engine und verfügt über die neuesten Features, Leistungsverbesserungen und Sicherheitspatches. | Bei Verwendung einer verwalteten Azure SQL-Datenbank-Instanz, die in Azure ausgeführt wird, fallen Gebühren je nach Kapazität an. Erfahren Sie mehr zu den [Preisen für verwaltete SQL-Datenbank-Instanzen](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Site Recovery-Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. Erfahren Sie mehr zu [Site Recovery-Gebühren und -Preisen](https://azure.microsoft.com/pricing/details/site-recovery/).

 ## <a name="migration-process"></a>Migrationsprozess

Contoso migriert die Web- und Datenschichten seiner SmartHotel-App mit den folgenden Schritten zu Azure:

1. Contoso hat bereits eine Azure-Infrastruktur eingerichtet, sodass für dieses Szenario nur noch einige bestimmte Azure-Komponenten hinzugefügt werden müssen.
2. Die Datenschicht wird per Database Migration Service migriert. Der Database Migration Service stellt über eine Site-to-Site-VPN-Verbindung zwischen dem Contoso-Rechenzentrum und Azure eine Verbindung mit der lokalen SQL Server-VM her. Anschließend migriert der Database Migration Service die Datenbank.
3. Die Webschicht wird per Lift & Shift-Vorgang mit Site Recovery migriert. Dieser Prozess umfasst das Vorbereiten der lokalen VMware-Umgebung, das Einrichten und Aktivieren der Replikation und das Migrieren der VMs per Failover zu Azure.

     ![Migrationsarchitektur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 

## <a name="prerequisites"></a>Voraussetzungen

Contoso und andere Benutzer müssen für dieses Szenario die folgenden Voraussetzungen erfüllen:

Anforderungen | Details
--- | ---
**Registrieren für die verwaltete Instanz (Vorschauversion)** | Sie müssen für die eingeschränkte öffentliche Vorschauversion der verwalteten SQL-Datenbank-Instanz registriert sein. Sie benötigen ein Azure-Abonnement, um sich [registrieren](https://portal.azure.com#create/Microsoft.SQLManagedInstance) zu können. Da sich die Registrierung über einige Tage erstrecken kann, sollten Sie diesen Vorgang durchführen, bevor Sie mit der Bereitstellung dieses Szenarios beginnen.
**Azure-Abonnement** | Sie müssten bereits ein Abonnement erstellt haben, als Sie die Bewertung im ersten Artikel dieser Reihe durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator des Abonnements zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Falls Sie präzisere Berechtigungen benötigen, helfen Ihnen die Informationen unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Site Recovery-Zugriffs](../site-recovery/site-recovery-role-based-linked-access-control.md) weiter. 
**Site Recovery (lokal)** | Auf Ihrer lokalen vCenter Server-Instanz sollte Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Ein ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.<br/><br/> Unterstützte Konfiguration für [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) und [Speicher](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage).
**Database Migration Service** | Für den Database Migration Service benötigen Sie ein [kompatibles lokales VPN-Gerät](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Sie müssen das lokale VPN-Gerät konfigurieren können. Es muss über eine externe öffentliche IPv4-Adresse verfügen. Diese Adresse darf sich nicht hinter einem NAT-Gerät befinden.<br/><br/> Stellen Sie sicher, dass Sie Zugriff auf Ihre lokale SQL Server-Datenbank haben.<br/><br/> Windows-Firewall sollte auf die Quelldatenbank-Engine zugreifen können. Informieren Sie sich, wie Sie [Windows-Firewall für den Datenbank-Engine-Zugriff konfigurieren](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Wenn sich eine Firewall vor dem Datenbankcomputer befindet, sollten Sie Regeln hinzufügen, um den Zugriff auf die Datenbank und auf Dateien über SMB-Port 445 zuzulassen.<br/><br/> Für die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der jeweiligen verwalteten Zielinstanz verwendeten Anmeldeinformationen muss sichergestellt sein, dass es sich jeweils um Mitglieder der Serverrolle „sysadmin“ handelt.<br/><br/> Sie benötigen eine Netzwerkfreigabe in der lokalen Datenbank, die vom Database Migration Service zum Sichern der Quelldatenbank verwendet werden kann.<br/><br/> Vergewissern Sie sich, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt.<br/><br/> Notieren Sie sich einen Windows-Benutzernamen, der über eine Berechtigung für Vollzugriff auf die Netzwerkfreigabe verfügt, und das zugehörige Kennwort. Der Database Migration Service verwendet diese Benutzeranmeldeinformationen, um Sicherungsdateien in den Azure Storage-Container hochzuladen.<br/><br/> Bei der SQL Server Express-Installation wird das TCP/IP-Protokoll standardmäßig auf **Deaktiviert** gesetzt. Stellen Sie sicher, dass es aktiviert ist.

## <a name="scenario-steps"></a>Szenarioschritte

Contoso plant die Einrichtung der Bereitstellung wie folgt:

> [!div class="checklist"]
> * **Schritt 1: Einrichten einer verwalteten SQL-Datenbank-Instanz**: Contoso benötigt eine bereits erstellte verwaltete Instanz, zu der die lokale SQL Server-Datenbank-Instanz migriert wird.
> * **Schritt 2: Vorbereiten des Database Migration Service**: Contoso muss den Datenbankmigrationsanbieter registrieren, eine Instanz erstellen und dann ein Database Migration Service-Projekt erstellen. Außerdem muss Contoso einen SAS-URI (Shared Access Signature Uniform Resource Identifier) für den Database Migration Service einrichten. Ein SAS-URI ermöglicht den delegierten Zugriff auf Ressourcen im Speicherkonto von Contoso, damit Contoso eingeschränkte Berechtigungen für Speicherobjekte gewähren kann. Contoso richtet einen SAS-URI ein, damit der Database Migration Service auf den Speicherkontocontainer zugreifen kann, in den der Dienst die SQL Server-Sicherungsdateien hochlädt.
> * **Schritt 3: Vorbereiten von Azure für Site Recovery**: Contoso muss ein Speicherkonto erstellen, das die replizierten Daten für Site Recovery enthält. Außerdem muss das Unternehmen einen Azure Recovery Services-Tresor erstellen.
> * **Schritt 4: Vorbereiten der lokalen VMware für Site Recovery**: Contoso bereitet Konten für die VM-Ermittlung und die Agent-Installation vor, um nach dem Failover eine Verbindung mit Azure-VMs herstellen zu können.
> * **Schritt 5: Replizieren von VMs**: Für die Replikation konfiguriert Contoso die Quell- und Zielumgebung für Site Recovery, richtet eine Replikationsrichtlinie ein und beginnt mit der Replikation von VMs in Azure Storage.
> * **Schritt 6: Migrieren der Datenbank mit dem Database Migration Service**: Contoso migriert die Datenbank.
> * **Schritt 7: Migrieren der VMs mit Site Recovery**: Contoso führt ein Testfailover durch, um sicherzustellen, dass alles richtig funktioniert. Anschließend führt Contoso ein vollständiges Failover aus, um die VMs zu Azure zu migrieren.

## <a name="step-1-prepare-a-sql-database-managed-instance"></a>Schritt 1: Vorbereiten einer verwalteten Azure SQL-Datenbank-Instanz

Contoso benötigt ein Subnetz, das die folgenden Anforderungen erfüllt, um eine verwaltete Azure SQL-Datenbank-Instanz einzurichten:

- Das Subnetz muss dediziert sein. Es muss leer sein und darf keinen anderen Clouddienst enthalten. Das Subnetz darf kein Gatewaysubnetz sein.
- Nachdem die verwaltete Instanz erstellt wurde, sollte Contoso dem Subnetz keine Ressourcen hinzufügen.
- Dem Subnetz darf keine Netzwerksicherheitsgruppe zugeordnet sein.
- Das Subnetz muss über eine Routingtabelle für benutzerdefiniertes Routing (User-Defined Routing, UDR) verfügen. Die folgende Route sollte die einzige zugewiesene Route sein: 0.0.0.0/0, nächster Hop „Internet“. 
- Optionales benutzerdefiniertes DNS: Wenn ein benutzerdefiniertes DNS für das virtuelle Azure-Netzwerk angegeben ist, muss die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) der Liste hinzugefügt werden. Informieren Sie sich, wie Sie [ein benutzerdefiniertes DNS für eine verwaltete Azure SQL-Datenbank-Instanz konfigurieren](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Dem Subnetz darf kein Dienstendpunkt (Speicher oder SQL) zugeordnet sein. Dienstendpunkte sollten im virtuellen Netzwerk deaktiviert sein.
- Das Subnetz muss mindestens 16 IP-Adressen aufweisen. Informieren Sie sich, wie Sie [die Größe des Subnetzes für verwaltete Instanzen ermitteln](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances).
- In der Hybridumgebung von Contoso sind benutzerdefinierte DNS-Einstellungen erforderlich. Contoso konfiguriert die DNS-Einstellungen so, dass mindestens ein Azure DNS-Server des Unternehmens genutzt wird. Informieren Sie sich über die [DNS-Anpassung](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Einrichten eines virtuellen Netzwerks für die verwaltete Instanz

Contoso richtet das virtuelle Netzwerk wie folgt ein: 

1. Contoso erstellt ein neues virtuelles Netzwerk (**VNET-SQLMI-EU2**) in der primären Region „East US 2“ (USA, Osten 2). Das virtuelle Netzwerk wird der Ressourcengruppe **ContosoNetworkingRG** hinzugefügt.
2. Contoso weist den Adressraum 10.235.0.0/24 zu. Contoso stellt sicher, dass sich der Bereich nicht mit anderen Netzwerken des Unternehmens überlappt.
2. Contoso fügt dem Netzwerk zwei Subnetze hinzu:
    - **SQLMI-DS-EUS2** (10.235.0.0.25)
    - **SQLMI-SAW-EUS2** (10.235.0.128/29). Dieses Subnetz wird verwendet, um ein Verzeichnis an die verwaltete Instanz anzufügen.

    ![Verwaltete Instanz – Erstellen eines virtuellen Netzwerks](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Nachdem das virtuelle Netzwerk und die Subnetze bereitgestellt wurden, führt Contoso für die Netzwerke wie folgt ein Peering durch:

    - Peering von **VNET-SQLMI-EUS2** mit **VNET-HUB-EUS2** (dem virtuellen Hubnetzwerk für „USA, Osten 2“).
    - Peering von **VNET-SQLMI-EUS2** mit **VNET-PROD-EUS2** (Produktionsnetzwerk).

    ![Netzwerkpeering](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso legt benutzerdefinierte DNS-Einstellungen fest. Das DNS verweist zuerst auf den Azure-Domänencontroller von Contoso. Das Azure DNS ist der sekundäre Verweis. Die Azure-Domänencontroller von Contoso befinden sich an den folgenden Orten:

    - Im Subnetz **PROD-DC-EUS2** im Produktionsnetzwerk „USA, Osten 2“ (**VNET-PROD-EUS2**)
    - **CONTOSODC3**-Adresse: 10.245.42.4
    - **CONTOSODC4**-Adresse: 10.245.42.5
    - Azure DNS-Auflösung: 168.63.129.16

     ![Netzwerk-DNS-Server](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

*Benötigen Sie weitere Hilfe?*

- Sehen Sie sich eine Übersicht über die [verwaltete Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) an.
- Informieren Sie sich über das [Erstellen eines virtuellen Netzwerks für eine verwaltete Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances).
- Erfahren Sie, wie Sie das [Peering einrichten](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering).
- Erfahren Sie, wie Sie die [DNS-Einstellungen für Azure Active Directory aktualisieren](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

### <a name="set-up-routing"></a>Einrichten des Routings

Die verwaltete Instanz wird in einem privaten virtuellen Netzwerk angeordnet. Contoso benötigt eine Routingtabelle für das virtuelle Netzwerk, um mit dem Azure-Verwaltungsdienst zu kommunizieren. Wenn das virtuelle Netzwerk nicht mit dem Dienst kommunizieren kann, von dem es verwaltet wird, kann nicht mehr auf das virtuelle Netzwerk zugegriffen werden.

Contoso berücksichtigt die folgenden Faktoren:

- Die Routingtabelle enthält einen Satz mit Regeln (Routen), mit denen angegeben wird, wie von der verwalteten Instanz gesendete Pakete im virtuellen Netzwerk weitergeleitet werden sollen.
- Die Routingtabelle bezieht sich auf Subnetze, in denen verwalteten Instanzen bereitgestellt werden. Jedes Paket, das ein Subnetz verlässt, wird auf Grundlage der entsprechenden Routingtabelle verarbeitet.
- Ein Subnetz kann nur einer Routingtabelle zugeordnet sein.
- Für die Erstellung von Routingtabellen in Microsoft Azure fallen keine zusätzlichen Gebühren an.

 Richten Sie das Routing wie folgt ein:

1. Contoso erstellt eine UDR-Tabelle. Contoso erstellt die Routingtabelle in der Ressourcengruppe **ContosoNetworkingRG**.

    ![Routingtabelle](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Contoso fügt nach der Bereitstellung der Routingtabelle (**MIRouteTable**) eine Route mit einem 0.0.0.0/0-Adresspräfix entsprechend den Anforderungen für die verwaltete Instanz hinzu. Die Option **Typ des nächsten Hops** wird auf **Internet** festgelegt.

    ![Routingtabellenpräfix](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso ordnet die Routingtabelle dem Subnetz **SQLMI-DB-EUS2** (im Netzwerk **VNET-SQLMI-EUS2**) zu. 

    ![Routingtabellensubnetz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
*Benötigen Sie weitere Hilfe?*

Informieren Sie sich, wie Sie [Routen für eine verwaltete Instanz einrichten](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route).

### <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

Contoso kann jetzt eine verwaltete SQL-Datenbank-Instanz bereitstellen:

1. Da über die verwaltete Instanz eine Geschäfts-App bereitgestellt wird, stellt Contoso die verwaltete Instanz in der primären Region „USA, Osten 2“ des Unternehmens bereit. Contoso fügt die verwaltete Instanz der Ressourcengruppe **ContosoRG** hinzu.
2. Contoso wählt einen Tarif, die Größe der Computeleistung und den Speicher für die Instanz aus. Erfahren Sie mehr zu den [Preisen für verwaltete SQL-Datenbank-Instanzen](https://azure.microsoft.com/pricing/details/sql-database/managed/).

    ![Verwaltete Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nach der Bereitstellung der verwalteten Instanz werden zwei neue Ressourcen in der Ressourcengruppe **ContosoRG** angezeigt:

    - Ein virtueller Cluster, falls Contoso über mehrere verwaltete Instanzen verfügt.
    - Die verwaltete SQL-Datenbank-Instanz. 

    ![Verwaltete Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

*Benötigen Sie weitere Hilfe?*

Informieren Sie sich, wie Sie [eine verwaltete Instanz bereitstellen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

## <a name="step-2-prepare-the-database-migration-service"></a>Schritt 2: Vorbereiten des Database Migration Service

Zum Vorbereiten des Database Migration Service muss Contoso einige Schritte ausführen:

- Registrieren Sie den Database Migration Service-Anbieter in Azure.
- Stellen Sie den Database Migration Service mit Zugriff auf Azure Storage zum Hochladen der Sicherungsdateien bereit, die zum Migrieren einer Datenbank verwendet werden. Contoso erstellt einen Azure Blob Storage-Container, um den Zugriff auf Azure Storage zu ermöglichen. Contoso generiert einen SAS-URI für den Blob Storage-Container. 
- Erstellen Sie ein Database Migration Service-Projekt.

Anschließend führt Contoso die folgenden Schritte aus:

1. Contoso registriert den Datenbankmigrationsanbieter unter seinem Abonnement.
    ![Database Migration Service – Registrieren](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Contoso erstellt einen Blob Storage-Container. Contoso generiert einen SAS-URI, damit der Database Migration Service darauf zugreifen kann.

    ![Database Migration Service – Generieren eines SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Contoso erstellt eine Database Migration Service-Instanz. 

    ![Database Migration Service – Erstellen einer Instanz](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso ordnet die Database Migration Service-Instanz im Subnetz **PROD-DC-EUS2** des virtuellen Netzwerks **VNET-PROD-DC-EUS2** an.
    - Der Database Migration Service wird von Contoso so angeordnet, weil sich der Dienst in einem virtuellen Netzwerk befinden muss, das über ein VPN-Gateway auf die lokale SQL Server-VM zugreifen kann.
    - Für **VNET-FA-EUS2** wird ein Peering mit **VNET-HUB-EUS2** durchgeführt und die Verwendung von Remotegateways zugelassen. Mit der Option **Use remote gateways** (Remotegateways verwenden) wird sichergestellt, dass der Database Migration Service so kommunizieren kann, wie dies erforderlich ist.

        ![Database Migration Service – Konfigurieren des Netzwerks](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

*Benötigen Sie weitere Hilfe?*

- Informieren Sie sich, wie Sie [den Database Migration Service einrichten](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal).
- Erfahren Sie, wie Sie [eine SAS erstellen und verwenden](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Schritt 3: Vorbereiten von Azure für den Site Recovery-Dienst

Es sind mehrere Azure-Elemente erforderlich, damit Contoso Site Recovery für die Migration seiner Webschicht-VM (**WEBMV**) einrichten kann:

- Ein virtuelles Netzwerk, in dem sich Ressourcen befinden, für die ein Failover ausgeführt wurde.
- Ein Speicherkonto für die Speicherung replizierter Daten. 
- Ein Recovery Services-Tresor in Azure.

Contoso richtet Site Recovery wie folgt ein:

1. Da die VM ein Web-Front-End der SmartHotel-App ist, führt Contoso das Failover über die VM in das vorhandene Produktionsnetzwerk (**VNET-PROD-EUS2**) und Subnetz (**PROD-FE-EUS2**) aus. Das Netzwerk und das Subnetz befinden sich in der primären Region „USA, Osten 2“. Das Netzwerk wurde von Contoso eingerichtet, als [die Azure-Infrastruktur](contoso-migration-infrastructure.md) bereitgestellt wurde.
2. Contoso erstellt ein Speicherkonto (**contosovmsacc20180528**). Contoso verwendet ein universelles Konto. Contoso wählt den Standardspeicher und die Replikation in lokal redundantem Speicher aus.

    ![Site Recovery – Erstellen eines Speicherkontos](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Nachdem das Netzwerk und das Speicherkonto vorhanden sind, erstellt Contoso einen Tresor (**ContosoMigrationVault**). Contoso ordnet den Tresor in der Ressourcengruppe **ContosoFailoverRG** in der primären Region „USA, Osten 2“ an.

    ![Recovery Services – Erstellen des Tresors](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

*Benötigen Sie weitere Hilfe?*

Informieren Sie sich, wie Sie [Azure für Site Recovery einrichten](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure).


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 4: Vorbereiten der lokalen VMware für Site Recovery

Contoso muss diese Aufgaben durchführen, um VMware für Site Recovery vorzubereiten:

- Vorbereiten eines Kontos auf der vCenter Server-Instanz oder dem vSphere ESXi-Host (Mit dem Konto wird die VM-Ermittlung automatisiert.)
- Erstellen eines Kontos für die automatische Installation des Mobility Service auf VMware-VMs, die Contoso replizieren möchte
- Vorbereiten lokaler VMs für die Verbindung mit Azure-VMs, wenn diese nach dem Failover erstellt werden


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Hierfür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Contoso benötigt ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso richtet das Konto ein, indem diese Aufgaben durchgeführt werden:

1. Es wird eine Rolle auf der vCenter-Ebene erstellt.
2. Anschließend werden dieser Rolle die erforderlichen Berechtigungen zugewiesen.

*Benötigen Sie weitere Hilfe?*

Informieren Sie sich, wie Sie [eine Rolle für die automatische Ermittlung erstellen und zuweisen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Mobility Service-Installation

Der Mobility Service muss auf der VM installiert sein, die Contoso replizieren möchte. Contoso berücksichtigt die folgenden Faktoren zum Mobility Service:

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn Contoso die Replikation für die VM aktiviert.
- Für die automatische Pushinstallation muss Contoso ein Konto vorbereiten, mit dem Site Recovery auf die VM zugreifen kann.
- Contoso gibt dieses Konto beim Einrichten der Replikation in der Azure-Konsole an.
- Contoso muss über ein Domänenkonto oder lokales Konto mit den Berechtigungen für die Installation auf dem virtuellen Computer verfügen.

*Benötigen Sie weitere Hilfe?*

Informieren Sie sich über das [Erstellen eines Kontos für die Pushinstallation von Mobility Service](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation).

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover in Azure möchte Contoso eine Verbindung mit den replizierten VMs in Azure herstellen können. Um eine Verbindung mit den replizierten VMs in Azure herzustellen, muss Contoso vor der Migration einige Aufgaben auf der lokalen VM durchführen: 

1. Contoso aktiviert vor dem Failover RDP auf dem lokalen virtuellen Computer, um über das Internet darauf zuzugreifen. Contoso stellt sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über die Site-to-Site-VPN-Verbindung von Contoso aktiviert Contoso RDP auf dem lokalen Computer. Contoso lässt RDP unter **Windows-Firewall** > **Zugelassene Apps und Features** für **Domänennetzwerke** und private Netzwerke zu.
3. Contoso legt die SAN-Richtlinie des Betriebssystems auf dem lokalen virtuellen Computer auf **OnlineAll** fest.

Contoso muss auch die folgenden Punkte überprüfen, wenn ein Failover ausgeführt wird:

- Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn ein Failover ausgelöst wird. Falls Windows-Updates ausstehen, kann sich Contoso erst am virtuellen Computer anmelden, nachdem das Update abgeschlossen ist.
- Nach dem Failover sollte Contoso die **Startdiagnose** überprüfen, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Contoso die Startdiagnose nicht anzeigen kann, sollte überprüft werden, ob die VM ausgeführt wird. Anschließend sollten die [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) beachtet werden.

## <a name="step-5-replicate-the-on-premises-vms-to-azure-by-using-site-recovery"></a>Schritt 5: Replizieren der lokalen VMs in Azure mit Site Recovery

Vor dem Ausführen einer Migration zu Azure muss Contoso die Replikation einrichten und die Replikation für seine lokalen VMs aktivieren.

### <a name="set-a-replication-goal"></a>Festlegen eines Replikationsziels

1. Im Tresor legt Contoso unter dem Tresornamen (**ContosoVMVault**) ein Replikationsziel fest (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Contoso gibt an, dass seine Computer lokal angeordnet sind, dass es sich um VMware-VMs handelt und dass Contoso die Replikation in Azure durchführen möchte.

    ![Vorbereiten der Infrastruktur – Schutzziel](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Um fortfahren zu können, muss Contoso bestätigen, dass die Bereitstellungsplanung abgeschlossen wurde. Contoso wählt **Ja, ist abgeschlossen**, um dies zu bestätigen. Bei dieser Bereitstellung migriert Contoso nur eine einzelne VM, sodass keine Bereitstellungsplanung erforderlich ist.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Als Nächstes muss Contoso seine Quellumgebung konfigurieren. Zum Einrichten seiner Quellumgebung lädt Contoso eine OVF-Vorlage herunter. Contoso verwendet die Vorlage für die Bereitstellung des Konfigurationsservers und der zugehörigen Komponenten als hoch verfügbare, lokale VMware-VM. Zu den Komponenten auf dem Server gehören:

- Der Konfigurationsserver, der die Kommunikation zwischen der lokalen Infrastruktur und Azure koordiniert. Der Konfigurationsserver verwaltet die Datenreplikation.
- Der Prozessserver, der als Replikationsgateway fungiert. Mit dem Prozessserver wird Folgendes durchgeführt:
    - Empfängt Replikationsdaten.
    - Optimiert das Replikationsdatum, indem die Zwischenspeicherung, Komprimierung und Verschlüsselung verwendet wird.
    - Sendet das Replikationsdatum an Azure Storage.
- Der Prozessserver installiert auch den Mobility Service auf den VMs, die Contoso replizieren möchte. Die Prozessserver führt die automatische Ermittlung von lokalen VMware-VMs durch.
- Nachdem die Konfigurationsserver-VM erstellt und gestartet wurde, kann Contoso den Server im Tresor registrieren.

Richten Sie die Quellumgebung wie folgt ein:

1. Contoso lädt die OVF-Vorlage im Azure-Portal herunter (**Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver**).
    
    ![Hinzufügen eines Konfigurationsservers](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Contoso importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![Bereitstellen der OVF-Vorlage](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wenn Contoso die VM zum ersten Mal aktiviert, wird sie mit einer Windows Server 2016-Installationsoberfläche gestartet. Contoso akzeptiert die Lizenzvereinbarung und gibt ein Administratorkennwort ein.
4. Nachdem die Installation abgeschlossen ist, meldet sich Contoso als Administrator an der VM an. Bei der ersten Anmeldung an der VM durch Contoso wird automatisch das Azure Site Recovery-Konfigurationstool ausgeführt.
5. Im Site Recovery-Konfigurationstool gibt Contoso einen Namen ein, der zum Registrieren des Konfigurationsservers im Tresor verwendet wird.
6. Das Tool überprüft die Verbindung der VM mit Azure. Nach der Verbindungsherstellung wählt Contoso **Anmelden**, um sich am Azure-Abonnement anzumelden. Mit den Anmeldeinformationen muss der Zugriff auf den Tresor möglich sein, in dem der Konfigurationsserver registriert ist. 

    ![Registrieren des Konfigurationsservers](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
8. Im Assistenten wählt Contoso die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
9. Contoso wählt das Abonnement, die Ressourcengruppe und den Recovery Services-Tresor aus, in dem der Konfigurationsserver registriert werden soll.

    ![Auswählen des Recovery Services-Tresors](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png)

10. Contoso lädt MySQL Server und VMWare PowerCLI herunter und führt die Installation durch. Anschließend überprüft Contoso die Servereinstellungen.
11. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse der vCenter Server-Instanz oder des vSphere-Hosts ein. Contoso behält den Standardport bei und gibt einen Anzeigenamen für die vCenter Server-Instanz in Azure ein.
12. Contoso muss das Konto angeben, das zuvor erstellt wurde, damit Site Recovery automatisch VMware-VMs erkennen kann, die für die Replikation verfügbar sind. 
13. Contoso gibt Anmeldeinformationen ein, damit der Mobility Service automatisch installiert wird, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorberechtigungen auf den VMs. 

    ![Konfigurieren von vCenter Server](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Contoso überprüft nach Abschluss der Registrierung im Azure-Portal erneut, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern. 
8. Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her. Site Recovery ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Nun muss Contoso die Zielumgebung für die Replikation konfigurieren:

1. Contoso wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Speicherkonto und ein Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Nach der Einrichtung der Quelle und des Ziels erstellt Contoso eine Replikationsrichtlinie und ordnet sie dem Konfigurationsserver zu:

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt Contoso die Richtlinie **ContosoMigrationPolicy**.
2. Contoso verwendet die Standardeinstellungen:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrungszeitraum des Wiederherstellungspunkts**: Der Standardwert ist „24 Stunden“. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**: Der Standardwert ist „1 Stunde“. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
    ![Replikationsrichtlinie – Erstellen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

3. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Replikationsrichtlinie – Zuordnen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)

*Benötigen Sie weitere Hilfe?*

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.

### <a name="enable-replication"></a>Aktivieren der Replikation

Contoso kann nun mit der Replikation der WebVM beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **Replizieren** wählt Contoso die Quelleinstellungen aus.
2. Contoso gibt an, dass virtuelle Computer aktiviert werden sollen, wählt die vCenter Server-Instanz aus und legt den Konfigurationsserver fest.

    ![Aktivieren der Replikation– Quelle](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Contoso gibt die Zieleinstellungen an, z.B. die Ressourcengruppe und das Netzwerk, in der bzw. dem sich die Azure-VM nach dem Failover befindet. Contoso gibt das Speicherkonto an, unter dem die replizierten Daten gespeichert werden.

    ![Aktivieren der Replikation – Ziel](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso wählt **WebVM** für die Replikation aus. Site Recovery installiert den Mobility Service auf jeder VM, wenn die Replikation aktiviert ist. 

    ![Aktivieren der Replikation – Auswählen der VM](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso überprüft, ob die richtige Replikationsrichtlinie ausgewählt ist. Anschließend wird die Replikation für **WEBVM** aktiviert. Contoso kann den Status der Replikation unter **Aufträge** nachverfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.
6. Unter **Zusammenfassung** im Azure-Portal kann Contoso die Struktur für die VMs anzeigen, die in Azure repliziert werden:

    ![Infrastrukturansicht](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)

*Benötigen Sie weitere Hilfe?*

Unter [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication) finden Sie eine vollständige exemplarische Vorgehensweise mit diesen Schritten.

## <a name="step-6-migrate-the-database-by-using-the-database-migration-service"></a>Schritt 6: Migrieren der Datenbank mit dem Database Migration Service

Contoso muss ein Database Migration Service-Projekt erstellen und anschließend die Datenbank migrieren.

### <a name="create-a-database-migration-service-project"></a>Erstellen eines Database Migration Service-Projekts

1. Contoso erstellt ein Database Migration Service-Projekt. Contoso wählt den Quellservertyp **SQL Server** aus. Contoso wählt **Verwaltete Azure SQL-Datenbank-Instanz** als Ziel aus.

     ![Database Migration Service – Neues Migrationsprojekt](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Der Migrations-Assistent wird geöffnet.

### <a name="migrate-the-database"></a>Migrieren der Datenbank 

1. Im Migrations-Assistenten gibt Contoso die Quell-VM an, auf der sich die lokale Datenbank befindet. Contoso gibt die Anmeldeinformationen für den Zugriff auf die Datenbank ein.

    ![Database Migration Service – Quelldetails](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Contoso wählt die Datenbank (**SmartHotel.Registration**) für die Migration aus:

    ![Database Migration Service – Auswählen der Quelldatenbanken](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Für das Ziel gibt Contoso den Namen der verwalteten Instanz in Azure ein. Contoso gibt die Anmeldeinformationen für den Zugriff auf die verwaltete Instanz ein.

    ![Database Migration Service – Zieldetails](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Unter **Neue Aktivität** > **Migration ausführen** gibt Contoso die Einstellungen für die Ausführung der Migration an:
    - Quell- und Zielanmeldeinformationen
    - Zu migrierende Datenbank
    - Netzwerkfreigabe, die von Contoso auf dem lokalen virtuellen Computer erstellt wurde Der Database Migration Service ordnet Quellsicherungen auf dieser Freigabe an. 
        - Das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, muss über Schreibberechtigungen für diese Freigabe verfügen.
        - Der FQDN-Pfad zur Freigabe muss verwendet werden.
    - Der SAS-URI, mit dem für den Database Migration Service der Zugriff auf den Speicherkontocontainer gewährt wird, in den der Dienst die Sicherungsdateien für die Migration hochlädt.

        ![Database Migration Service – Konfigurieren der Migrationseinstellungen](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Contoso speichert die Migration und führt sie dann aus.
6. In der **Übersicht** überwacht Contoso den Migrationsstatus.

    ![Database Migration Service – Überwachen](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Nach Abschluss der Migration überprüft Contoso, ob die Zieldatenbanken auf der verwalteten Instanz vorhanden sind.

    ![Database Migration Service – Überprüfen der Datenbankmigration](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-by-using-site-recovery"></a>Schritt 7: Migrieren des virtuellen Computers mithilfe von Site Recovery

Contoso führt ein schnelles Testfailover aus und migriert anschließend die VM.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Mit der Ausführung eines Testfailovers wird sichergestellt, dass vor der Migration von WEBVM alles wie erwartet funktioniert. Contoso führt die folgenden Schritte aus:

1. Contoso führt ein Testfailover zum letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. Contoso wählt die Option **Computer vor Beginn des Failovers herunterfahren** aus. Wenn diese Option ausgewählt wird, versucht Site Recovery, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Testfailover durchführen: 
    1. Eine Überprüfung der erforderlichen Komponenten wird durchgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    2. Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn der letzte Wiederherstellungspunkt ausgewählt wird, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    3.  Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.
3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Contoso vergewissert sich, dass alles wie erwartet funktioniert: Die VM hat die richtige Größe, ist mit dem richtigen Netzwerk verbunden und wird ausgeführt. 
4. Nach der Überprüfung des Testfailovers bereinigt Contoso das Failover. Anschließend werden alle Beobachtungen aufgezeichnet und gespeichert. 

### <a name="migrate-the-vm"></a>Migrieren der VM

1. Nachdem Contoso überprüft hat, ob das Testfailover wie erwartet funktioniert, erstellt das Unternehmen einen Wiederherstellungsplan für die Migration. Contoso fügt WEBVM dem Plan hinzu:

     ![Erstellen eines Wiederherstellungsplans – Auswählen von Elementen](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Contoso führt ein Failover für den Plan aus. Contoso wählt den letzten Wiederherstellungspunkt aus. Contoso gibt an, dass Site Recovery versuchen soll, die lokale VM vor dem Auslösen des Failovers herunterzufahren.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

   ![Details eines Wiederherstellungsplans](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Nach dieser Überprüfung führt Contoso die Migration durch, um den Migrationsprozess zu beenden. Darüber hinaus beendet das Unternehmen die Replikation für die VM und die Site Recovery-Abrechnung für die VM.

    ![Failover – Abschließen der Migration](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Der letzte Schritt im Migrationsprozess besteht darin, die Verbindungszeichenfolge der Anwendung zu aktualisieren, um auf die migrierte Datenbank zu verweisen, die auf der verwalteten Instanz von Contoso ausgeführt wird.

1. Im Azure-Portal ermittelt Contoso die Verbindungszeichenfolge unter **Einstellungen** > **Verbindungszeichenfolgen**.

    ![Verbindungszeichenfolgen](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Contoso aktualisiert die Zeichenfolge mit dem Benutzernamen und Kennwort der verwalteten SQL-Datenbank-Instanz.
3. Nachdem die Zeichenfolge konfiguriert wurde, ersetzt Contoso die aktuelle Verbindungszeichenfolge in der Datei „web.config“ seiner Anwendung.
4. Nachdem die Datei aktualisiert und gespeichert wurde, startet Contoso IIS auf WEBVM neu. Contoso führt `IISRESET /RESTART` in einem Eingabeaufforderungsfenster aus, um IIS neu zu starten.
5. Nach dem Neustart von IIS nutzt die App die Datenbank, die auf der verwalteten Azure SQL-Datenbank-Instanz ausgeführt wird.
6. An diesem Punkt kann Contoso den SQLVM-Computer lokal herunterfahren. Die Migration wurde abgeschlossen.

*Benötigen Sie weitere Hilfe?*

- Informieren Sie sich über die [Durchführung eines Testfailovers](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure). 
- Informieren Sie sich über das [Erstellen eines Wiederherstellungsplans](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).
- Erfahren Sie, wie Sie [das Failover in Azure ausführen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover).

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach Abschluss der Migration wird die SmartHotel-App auf einer Azure-VM ausgeführt, und die SmartHotel-Datenbank befindet sich auf der verwalteten Azure SQL-Datenbank-Instanz.  

Contoso muss nun die folgenden Bereinigungsschritte ausführen:  

- Entfernen des WEBVM-Computers aus dem vCenter Server-Bestand
- Entfernen des SQLVM-Computers aus dem vCenter Server-Bestand
- Entfernen von WEBVM und SQLVM aus lokalen Sicherungsaufträgen
- Aktualisieren der internen Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse für WEBVM
- Entfernen von SQLVM aus der internen Dokumentation. Alternativ hierzu kann Contoso die Dokumentation auch überarbeiten, um anzugeben, dass SQLVM gelöscht wurde und nicht mehr im VM-Bestand enthalten ist.
- Überprüfen aller Ressourcen, die mit den außer Betrieb genommenen VMs interagieren. Alle relevanten Einstellungen bzw. Dokumentationsinhalte müssen aktualisiert werden, um die neue Konfiguration widerzuspiegeln.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso seine neue Infrastruktur vollständig operationalisieren und schützen.

### <a name="security"></a>Sicherheit

Das Contoso-Sicherheitsteam überprüft die Azure-VMs und die verwaltete SQL-Datenbank-Instanz, um eine Überprüfung auf eventuelle Sicherheitsprobleme bei der Implementierung durchzuführen:

- Das Sicherheitsteam überprüft die Netzwerksicherheitsgruppen, die zum Steuern des Zugriffs für den virtuellen Computer verwendet werden. Mit Netzwerksicherheitsgruppen kann sichergestellt werden, dass nur Datenverkehr, der für die App zulässig ist, passieren kann.
- Das Sicherheitsteam von Contoso erwägt auch, die Daten auf dem Datenträger per Azure Disk Encryption und Azure Key Vault zu schützen.
- Das Sicherheitsteam aktiviert die Bedrohungserkennung auf der verwalteten Instanz. Bei der Bedrohungserkennung wird eine Benachrichtigung mit dem Hinweis, dass ein Ticket erstellt werden soll, an das Sicherheitsteam von Contoso bzw. das Service Desk-System gesendet. Erfahren Sie mehr zur [Bedrohungserkennung für verwaltete Instanzen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Sicherheit der verwalteten Instanz – Bedrohungserkennung](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

Weitere Informationen zu den Sicherheitsmethoden für VMs finden Sie unter [Bewährte Sicherheitsmethoden für IaaS-Workloads in Azure](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control).

### <a name="backups"></a>Backups

Contoso sichert die Daten auf WEBVM mithilfe des Azure Backup-Diensts. Informieren Sie sich über [Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Contoso verfügt über eine vorhandene Lizenzierung für WEBVM. Contoso konvertiert die vorhandene Azure-VM, um die günstigeren Preise über den Azure-Hybridvorteil zu nutzen.
- Contoso aktiviert Azure Cost Management. Dies wird von Cloudyn, einem Tochterunternehmen von Microsoft, lizenziert. Cost Management ist eine Multicloud-Lösung zur Kostenverwaltung, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert. Erfahren Sie mehr über [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview). 


## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel weist Contoso der SmartHotel-App in Azure einen neuen Host zu, indem das Unternehmen die Front-End-VM der App mithilfe des Site Recovery-Diensts zu Azure migriert. Contoso migriert die lokale Datenbank zu einer verwalteten Azure SQL-Datenbank-Instanz, indem der Database Migration Service von Azure verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel der Reihe [weist Contoso der SmartHotel-App auf Azure-VMs mithilfe des Azure Site Recovery-Diensts einen neuen Host zu](contoso-migration-rehost-vm.md).


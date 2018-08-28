---
title: Zuweisen eines neuen Hosts für eine Contoso-App durch Migration zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen darüber, wie Contoso einer lokalen App einen neuen Host zuweist, indem diese zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe migriert wird
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: cd7a5832faf0fbb15349edee8ed504c1f94d1aa9
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "42145592"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-sql-server-alwayson-availability-group"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale App zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe

In diesem Artikel wird erläutert, wie Contoso der App SmartHotel in Azure einen neuen Host zuweist. Das Unternehmen migriert die Front-End-VM der App zu einer Azure-VM und die App-Datenbank zu einer Azure SQL Server-VM, die in einem Windows Server-Failovercluster mit SQL Server Always On-Verfügbarkeitsgruppen ausgeführt werden.

Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso lokale Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und einer verwalteten SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und die App-Datenbank mithilfe des [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert. | Verfügbar
Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe | Veranschaulicht, wie Contoso die App SmartHotel migriert. Contoso verwendet Site Recovery, um die App-VMs zu migrieren, und den Database Migration Service, um die App-Datenbank zu einem SQL Servercluster zu migrieren, das durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Dieser Artikel
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 9: Umgestalten einer App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz migriert. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App zu einer Azure-Web-App und einer Azure Database for MySQL-Instanz](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App zu Azure-Web-Apps auf mehreren Websites migriert, die in GitHub für Continuous Delivery integriert sind. Es migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Service, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar



In diesem Artikel migriert Contoso die auf VMware-VMs ausgeführte zweistufige Windows .NET SmartHotel-App zu Azure. Falls Sie diese App verwenden möchten: Sie wird als Open Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was die Unternehmen mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und daher geraten die lokalen Systeme und Infrastrukturen unter Druck.
- **Effizienzsteigerung**: Contoso muss unnötige Verfahren entfernen und Prozesse für Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schnell sein, weder Geld noch Zeit verschwenden, und Kundenanforderungen schneller bearbeiten.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Die IT-Experten müssen schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren.  Es darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT-Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.

## <a name="migration-goals"></a>Migrationsziele

Das Cloud-Team von Contoso hat sich Ziele für diese Migration gesetzt. Anhand dieser Ziele wird die beste Migrationsmethode bestimmt:

- Nach der Migration sollte die App in Azure die gleichen Leistungsmöglichkeiten aufweisen wie die lokale VMWare-Umgebung.  Die App bleibt lokal und in der Cloud gleichermaßen wichtig.
- Contoso möchte nicht in diese App investieren.  Die Investition ist wichtig für das Geschäft, in ihrer derzeitigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Bei der lokalen App-Datenbank gab es Verfügbarkeitsprobleme. Der Cluster soll in Azure als Hochverfügbarkeitscluster mit Failoverfunktionen bereitgestellt werden.
- Contoso möchte ein Upgrade von der aktuellen SQL Server 2008 R2-Plattform auf SQL Server 2017 durchführen.
- Contoso möchte für diese App keine Azure SQL-Datenbank verwenden und sucht nach Alternativen.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Szenario:

- Die App ist auf zwei VMs aufgeteilt (WEBVM und SQLVM).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5)
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.

![Szenarioarchitektur](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | DMS ermöglicht die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen bei minimaler Ausfallzeit. | Erfahren Sie mehr über die [unterstützten Regionen](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) für DMS und erhalten Sie eine [Preisübersicht](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Site Recovery orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.

 

## <a name="migration-process"></a>Migrationsprozess

- Contoso migriert die App-VMs zu Azure.
- Es migriert die Front-End-VM mithilfe von Site Recovery zur Azure-VM:
    - Im ersten Schritt bereitet das Unternehmen Azure-Komponenten vor und richtet diese ein, und es bereitet die lokale VMware-Infrastruktur vor.
    - Wenn alle Vorbereitungen getroffen sind, kann Contoso mit dem Replizieren der VM beginnen.
    - Wenn die Replikation aktiviert wurde und funktioniert, migriert das Unternehmen die VM via Failover zu Azure.
- Die Datenbank wird mithilfe des Data Migration Service (DMS) zu einem SQL Server-Cluster in Azure migriert.
    - Im ersten Schritt müssen SQL Server-VMs in Azure bereitgestellt, der Cluster und ein interner Lastenausgleich eingerichtet und Always On-Verfügbarkeitsgruppen konfiguriert werden.
    - Anschließend kann die Datenbank migriert werden
- Nach der Migration wird der Always On-Schutz für die Datenbank aktiviert.

![Migrationsprozess](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Voraussetzungen

Für die Ausführung dieses Szenarios benötigen Sie (und Contoso) Folgendes:

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie müssten bereits ein Abonnement erstellt haben, als Sie die Beurteilung im ersten Artikel dieser Reihe durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-Infrastruktur** | [Weitere Informationen](contoso-migration-infrastructure.md) zur Vorgehensweise von Contoso beim Einrichten einer Azure-Infrastruktur.<br/><br/> Erfahren Sie mehr zu spezifischen [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network)- und [Speicheranforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) für Site Recovery.
**Site Recovery (lokal)** | Ihr lokaler Server von vCenter Server sollte als Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.<br/><br/> Unterstützte Konfiguration für [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) und [Speicher](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage).<br/><br/> VMs, die repliziert werden sollen, müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.
**DMS** | Für DMS benötigen Sie ein [kompatibles lokales VPN-Gerät](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Sie müssen das lokale VPN-Gerät konfigurieren können. Es muss über eine externe öffentliche IPv4-Adresse verfügen, und die Adresse darf sich nicht hinter einem NAT-Gerät befinden.<br/><br/> Stellen Sie sicher, dass Sie Zugriff auf Ihre lokale SQL Server-Datenbank haben.<br/><br/> Die Windows-Firewall sollte auf die Quelldatenbank-Engine zugreifen können. [Weitere Informationen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)<br/><br/> Wenn sich eine Firewall vor dem Datenbankcomputer befindet, fügen Sie Regeln hinzu, um den Zugriff auf die Datenbank und auf Dateien über SMB-Port 445 zuzulassen.<br/><br/> Die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldeinformationen müssen Mitglieder der Serverrolle „sysadmin“ sein.<br/><br/> Sie benötigen eine Netzwerkfreigabe in der lokalen Datenbank, die von DMS zum Sichern der Quelldatenbank verwenden werden kann.<br/><br/> Stellen Sie sicher, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt.<br/><br/> Notieren Sie sich einen Windows-Benutzer (mit dem Kennwort), der eine Berechtigung für Vollzugriff auf die Netzwerkfreigabe besitzt. Azure Database Migration Service verwendet diese Benutzeranmeldeinformationen, um Sicherungsdateien in den Azure-Speichercontainer hochzuladen.<br/><br/> Bei der SQL Server Express-Installation wird das TCP/IP-Protokoll standardmäßig auf **Deaktiviert** gesetzt. Stellen Sie sicher, dass es aktiviert ist.


## <a name="scenario-steps"></a>Szenarioschritte

Contoso geht bei der Ausführung der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Erstellen der SQL Server-VMs in Azure**: Zur Gewährleistung von Hochverfügbarkeit möchte Contoso eine Clusterdatenbank in Azure bereitstellen. Das Unternehmen stellt zwei SQL Server-VMs und ein internes Azure-Lastenausgleichsmodul bereit.
> * **Schritt 2: Bereitstellen des Clusters**: Nach der Bereitstellung der SQL Server-VMs bereitet das Unternehmen einen Azure SQL Server-Cluster vor.  Es migriert seine Datenbank in diesen vorab erstellten Cluster.
> * **Schritt 3: Vorbereiten von DMS**: Zur Vorbereitung registriert das Unternehmen den Database Migration-Anbieter, erstellt eine DMS-Instanz und ein Projekt. Contoso richtet einen SAS-URI (SAS = Shared Access Signature, URI = Uniform Resource Identifier) ein. DMS verwendet den SAS-URI, um auf den Speicherkontocontainer zuzugreifen, in den der Dienst die SQL Server-Sicherungsdateien hochlädt.
> * **Schritt 4: Vorbereiten von Azure für Site Recovery**: Das Unternehmen erstellt ein Azure-Speicherkonto zum Speichern replizierter Daten und einen Recovery Services-Tresor.
> * **Schritt 5: Vorbereiten der lokalen VMware für Site Recovery**: Das Unternehmen bereitet Konten für die VM-Ermittlung und Agentinstallation sowie lokale VMs vor, damit nach dem Failover eine Verbindung Azure-VMs hergestellt werden kann.
> * **Schritt 6: Replizieren von VMs**: Contoso konfiguriert die Replikationseinstellungen und aktiviert die VM-Replikation.
> * **Schritt 7: Migrieren der Datenbank mit DMS**: Das Unternehmen migriert die Datenbank.
> * **Schritt 8: Migrieren der VMs mit Site Recovery**: Das Unternehmen führt ein Testfailover durch, um sicherzustellen, dass alles funktioniert, und führt anschließend ein vollständiges Failover für die Migration der VMs zu Azure aus.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Schritt 1: Vorbereiten eines Clusters für SQL Server Always On-Verfügbarkeitsgruppen

Contoso möchte vorausschauend planen und die Datenbank in einem Cluster in Azure bereitstellen. Dieser Cluster kann anschließend für andere Datenbanken verwendet werden. 

- Die SQL Server-VMs werden in der Ressourcengruppe ContosoRG bereitgestellt (wird für Produktionsressourcen verwendet).
- Abgesehen von eindeutigen Namen verwenden beide VMs die gleichen Einstellungen.
- Der interne Lastenausgleich wird in der Ressourcengruppe ContosoNetworkingRG bereitgestellt (wird für Netzwerkressourcen verwendet).
- Auf VMs wird Windows Server 2016 mit der SQL Server 2017 Enterprise Edition ausgeführt. Contoso verfügt über keine Lizenzen für dieses Betriebssystem. Daher verwendet das Unternehmen im Azure Marketplace ein Image, das die Lizenz als Gebühr für das Azure EA-Commitment bereitstellt.

Contoso geht bei der Einrichtung des Clusters wie folgt vor:

1. Das Unternehmen erstellt zwei SQL Server-VMs, indem es das Image SQL Server 2017 Enterprise Windows Server 2016 im Azure Marketplace auswählt. 

    ![SQL-VM-SKU](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. Unter **Assistent für die Erstellung virtueller Computer** > **Grundlagen** konfiguriert das Unternehmen Folgendes:

    - Namen für die VMs: **SQLAOG1** und **SQLAOG2**.
    - Da Computer unternehmenskritisch sind, aktiviert Contoso SSD für den VM-Datenträgertyp.
    - Es gibt Anmeldeinformationen für Computer an.
    - Das Unternehmen stellt die VMs in der primären Region „USA, Osten 2“ in der Ressourcengruppe ContosoRG bereit.

3. Unter **Größe** beginnt das Unternehmen bei beiden VMs mit der SKU „D2s_V3“. Nach Bedarf kann die Skalierung zu einem späteren Zeitpunkt durchgeführt werden.
4. Das Unternehmen führt unter **Einstellungen** die folgenden Schritte aus:

    - Da es sich bei diesen VMs um wichtige Datenbanken für die App handelt, verwenden sie verwaltete Datenträger.
    - Das Unternehmen ordnet die Computer im Produktionsnetzwerk der primären Region „USA, Osten 2“ (**VNET-PROD-EUS2**) im Subnetz der Datenbank (**PROD-DB-EUS2**) an.
    - Es erstellt eine neue Verfügbarkeitsgruppe: **SQLAOGAVSET** mit zwei Fehlerdomänen und fünf Updatedomänen.

    ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. Contoso begrenzt über die **SQL Server-Einstellungen** die SQL-Konnektivität mit dem virtuellen Netzwerk (privat) am Standardport 1433. Für die Authentifizierung verwendet es die gleichen Anmeldeinformationen wie vor Ort (**contosoadmin**).

    ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Benötigen Sie weitere Hilfe?**

- [Hilfe](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) zur Bereitstellung einer SQL Server-VM.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) zum Konfigurieren von VMs für verschiedene SQL Server-SKUs.

## <a name="step-2-deploy-the-failover-cluster"></a>Schritt 2: Bereitstellen des Failoverclusters

Contoso geht bei der Einrichtung des Clusters wie folgt vor:

1. Das Unternehmen richtet ein Azure-Speicherkonto ein, um als Cloudzeuge zu fungieren.
2. Es fügt die SQL Server-VMs zur Active Directory-Domäne im lokalen Contoso-Rechenzentrum hinzu.
3. Es erstellt den Cluster in Azure.
4. Es konfiguriert den Cloudzeugen.
5. Schließlich aktiviert das Unternehmen die Always On-Verfügbarkeitsgruppen von SQL.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Einrichten eines Speicherkontos als Cloudzeuge

Für die Einrichtung eines Cloudzeugen benötigt Contoso ein Azure-Speicherkonto, das die Blob-Datei für die Clustervermittlung enthält. Dasselbe Speicherkonto kann für die Einrichtung eines Cloudzeugen für mehrere Cluster verwendet werden. 

Contoso geht bei der Erstellung eines Speicherkontos wie folgt vor:

1. Das Unternehmen gibt einen erkennbaren Namen für das Konto an (**contosocloudwitness**).
2. Es stellt ein allgemein einsetzbares Konto mit LRS bereit.
3. Contoso platziert das Konto in einer dritten Region: USA, Süden-Mitte. Es platziert das Konto außerhalb der primären und sekundären Region, damit es im Falle es regionalen Ausfalls verfügbar bleibt.
4. Das Unternehmen platziert das Konto in seiner Ressourcengruppe mit Infrastrukturressourcen: **ContosoInfraRG**.

    ![Cloudzeuge](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Wenn das Unternehmen das Speicherkonto erstellt, werden für dieses Konto primäre und sekundäre Zugriffsschlüssel generiert. Der primäre Zugriffsschlüssel wird für die Erstellung des Cloudzeugen benötigt. Der Schlüssel wird unter dem Namen des Speicherkontos > **Zugriffstasten** angezeigt.

    ![Zugriffsschüssel](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>Hinzufügen von SQL Server-VMs zur Contoso-Domäne

1. Contoso fügt die VMs SQLAOG1 und SQLAOG2 zur Domäne contoso.com hinzu.
2. Anschließend werden auf jeder VM das Feature und die Tools des Windows-Failoverclusters installiert.

## <a name="set-up-the-cluster"></a>Einrichten des Clusters

Vor dem Einrichten des Clusters erstellt Contoso auf jedem Computer eine Momentaufnahme des Betriebssystemdatenträgers.

![Momentaufnahme](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Anschließend führt das Unternehmen ein Skript aus, mit dem der Windows-Failovercluster erstellt werden soll.

    ![Cluster erstellen](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Nach der Erstellung des Clusters überprüft das Unternehmen, ob die VMs als Clusterknoten angezeigt werden.

     ![Cluster erstellen](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>Konfigurieren des Cloudzeugen

1. Contoso konfiguriert den Cloudzeugen mithilfe des **Assistenten für die Quorumkonfiguration** im Failovercluster-Manager.
2. Im Assistenten erstellt das Unternehmen einen Cloudzeugen mit dem Speicherkonto.
3. Nach der Konfiguration des Cloudzeugen wird dieser im Failovercluster-Manager-Snap-In angezeigt.

    ![Cloudzeuge](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>Aktivieren von SQL Server Always On-Verfügbarkeitsgruppen

Contoso kann jetzt Always On aktivieren:

1. Das Unternehmen aktiviert im SQL Server-Konfigurations-Manager die **Always On-Verfügbarkeitsgruppen** für den Dienst **SQL Server (MSSQLSERVER)**.

    ![Aktivieren von Always On](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Das Unternehmen startet den Dienst neu, damit die Änderungen wirksam werden.

Nach der Aktivierung von Always On kann Contoso die Always On-Verfügbarkeitsgruppe für den Schutz der SmartHotel-Datenbank einrichten.

**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) zum Cloudzeugen und zum Einrichten eines Speicherkontos für diesen Cloudzeugen.
- [Anweisungen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) zum Einrichten eines Clusters und zum Erstellen einer Verfügbarkeitsgruppe.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Schritt 3: Bereitstellen von Azure Load Balancer

Contoso möchte nun ein internes Lastenausgleichsmodul bereitstellen, das sich vor den Clusterknoten befindet. Das Lastenausgleichsmodul ist für Datenverkehr empfangsbereit und leitet diesen an den entsprechenden Knoten weiter.

![Lastenausgleich](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Contoso geht bei der Erstellung des Lastenausgleichs wie folgt vor:

1. Das Unternehmen richtet im Azure-Portal unter **Netzwerk** > **Lastenausgleich** einen neuen internen Lastenausgleich ein: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Das Unternehmen ordnet den Lastenausgleich im Produktionsnetzwerk **VNET-PROD-EUS2** im Subnetz der Datenbank **PROD-DB-EUS2** an.
3. Dem Lastenausgleich wird eine statische IP-Adresse zugewiesen: 10.245.40.100.
4. Der Lastenausgleich wird als Netzwerkelement in der Netzwerkressourcengruppe **ContosoNetworkingRG** bereitgestellt.

    ![Lastenausgleich](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Nach der Bereitstellung des internen Lastenausgleichs muss Contoso diesen einrichten. Das Unternehmen erstellt einen Back-End-Adresspool, richtet einen Integritätstest ein und konfiguriert eine Regel für den Lastenausgleich.

### <a name="add-a-backend-pool"></a>Hinzufügen eines Back-End-Pools

Zum Verteilen von Datenverkehr an die VMs im Cluster richtet Contoso einen Back-End-Adresspool ein, der die IP-Adressen der NICs für VMs enthält, die den Netzwerkdatenverkehr über den Lastenausgleich empfangen.

1. Contoso fügt im Portal in den Einstellungen für den Lastenausgleich einen Back-End-Pool hinzu: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Dem Pool wird die Verfügbarkeitsgruppe SQLAOGAVSET zugeordnet. Die VMs in der Gruppe (**SQLAOG1** und **SQLAOG2**) werden zum Pool hinzugefügt.

    ![Back-End-Pool](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Contoso erstellt einen Integritätstest, damit der Lastenausgleich den Status der App überwachen kann. Abhängig von der Reaktion auf Integritätstests werden durch den Integritätstest dynamisch VMs zur Lastenausgleichsrotation hinzugefügt oder daraus entfernt.

Das Unternehmen geht bei der Erstellung des Tests wie folgt vor: 

1. Contoso erstellt im Portal in den Einstellungen für den Lastenausgleich einen Integritätstest: **SQLAlwaysOnEndPointProbe**.
2. Das Unternehmen legt fest, dass bei dem Test VMs an TCP-Port 59999 überwacht werden sollen.
3. Es legt ein Intervall von 5 Sekunden zwischen den Tests und einen Schwellenwert von 2 fest. Wenn zwei Tests fehlschlagen, gilt eine VM als fehlerhaft.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Konfigurieren des Lastenausgleichs für den Empfang von Datenverkehr


Contoso benötigt nun eine Regel für den Lastenausgleich, um zu definieren, wie Datenverkehr auf die VMs verteilt wird.

- Die Front-End-IP-Adresse verarbeitet eingehenden Datenverkehr.
- Der Back-End-IP-Pool empfängt den Datenverkehr.

Das Unternehmen geht bei der Erstellung der Regel wie folgt vor:

1. Es fügt im Portal in den Einstellungen für den Lastenausgleich eine neue Regel für den Lastenausgleich hinzu: **SQLAlwaysOnEndPointListener**.
2. Contoso legt für den Empfang von eingehendem Datenverkehr des SQL-Clients an TCP-Port 1433 einen Front-End-Listener fest.
3. Es gibt den Back-End-Pool an, an den Datenverkehr weitergeleitet werden soll, und den Port, an dem die VMs für den Datenverkehr empfangsbereit sind.
4. Contoso aktiviert Floating IP (Direct Server Return). Dies ist für SQL Always On immer erforderlich.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Benötigen Sie weitere Hilfe?**

- [Übersicht](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) über Azure Load Balancer.
- [Weitere Informationen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) zum Erstellen eines Lastenausgleichs.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Schritt 4: Vorbereiten von Azure für den Site Recovery-Dienst

Im Folgenden werden die Azure-Komponenten aufgeführt, die Contoso für die Bereitstellung von Site Recovery benötigt:

- Ein VNet, in dem sich VMs befinden, wenn diese während eines Failovers erstellt werden.
- Ein Azure-Speicherkonto für die Speicherung replizierter Daten. 
- Ein Recovery Services-Tresor in Azure.

Das Unternehmen geht bei der Einrichtung dieser Komponenten wie folgt vor:

1.  Contoso hat bereits ein Netzwerk/Subnetz erstellt, das für Site Recovery verwendet werden kann, wenn das Unternehmen die [Azure-Infrastruktur bereitgestellt](contoso-migration-rehost-vm-sql-ag.md) hat.

    - Bei der SmartHotel-App handelt es sich um eine Produktions-App. WEBVM wird zum Azure-Produktionsnetzwerk (VNET-PROD-EUS2) in der primären Region „USA, Osten 2“ migriert.
    - WEBVM wird in der Ressourcengruppe ContosoRG, die für Produktionsressourcen verwendet wird, und im Produktionssubnetz (PROD-FE EUS2) platziert.

2. Contoso erstellt ein Azure-Speicherkonto (contosovmsacc20180528) in der primären Region „USA, Osten 2“.

    - Contoso verwendet ein allgemeines Konto mit Standardspeicher und LRS-Replikation.
    - Das Konto muss sich in der gleichen Region wie der Tresor befinden.

    ![Site Recovery-Speicher](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Nach dem Einrichten des Netzwerks und des Speicherkontos kann Contoso nun einen Recovery Services-Tresor (**ContosoMigrationVault**) erstellen und diesen in der Ressourcengruppe **ContosoFailoverRG** in der primären Region „USA, Osten 2“ platzieren.

    ![Recovery Services-Tresor](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Benötigen Sie weitere Hilfe?**

[Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) zum Einrichten von Azure für Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 4: Vorbereiten der lokalen VMware für Site Recovery

Contoso bereit lokal Folgendes vor:

- Ein Konto auf dem Server von vCenter Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung.
- Ein Konto für die automatische Installation von Mobility Service auf VMware-VMs, die Sie replizieren möchten.
- Lokale VM-Einstellungen, damit Contoso nach einem Failover eine Verbindung mit der replizierten Azure-VM herstellen kann.


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. 
- Orchestrieren von Replikation, Failover und Failback.
- Dafür ist mindestens ein Konto mit Lesezugriff erforderlich. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso richtet das Konto wie folgt ein:

1. Contoso erstellt eine Rolle auf der vCenter-Ebene.
2. Anschließend weist Contoso dieser Rolle die erforderlichen Berechtigungen zu.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Mobility Service muss auf jeder VM installiert sein.

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn die Replikation für die VM aktiviert ist.
- Sie benötigen ein Konto, über das Site Recovery für die Pushinstallation auf die VM zugreifen kann. Sie geben dieses Konto an, wenn Sie die Replikation in der Azure-Konsole einrichten.
- Sie benötigen ein Domänenkonto oder ein lokales Konto mit Berechtigungen für die Installation auf der VM.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover möchte Contoso eine Verbindung mit Azure-VMs herstellen. Hierzu führt das Unternehmen vor der Migration folgende Schritte aus:

1. Für den Zugriff über das Internet:

 - Es aktiviert vor dem Failover RDP auf der lokalen VM.
 - Sicherstellen, dass TCP- und UDP-Regeln zum **öffentlichen** Profil hinzugefügt wurden.
 - Bei allen Profilen unter **Windows-Firewall** > **Zugelassene Apps** überprüfen, ob RDP zulässig ist.
 
2. Für den Zugriff über Site-to-Site-VPN:

 - RDP auf dem lokalen Computer aktivieren.
 - RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private und Domänennetzwerke** zulassen.
 - SAN-Richtlinie des Betriebssystems auf der lokalen VM auf **OnlineAll** festlegen.

Bei der Ausführung eines Failovers sind darüber hinaus folgende Aspekte zu überprüfen:

- Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls ist nach Abschluss des Updates die Anmeldung bei der VM nicht mehr möglich.
- Nach dem Failover kann **Startdiagnose** aktiviert werden, um einen Screenshot der VM anzuzeigen. Falls dies nicht funktioniert, sollte überprüft werden, ob die VM ausgeführt wird, und die folgenden [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gelesen werden.


**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) zum Erstellen und Zuweisen von Rollen für die automatische Ermittlung.
- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) zum Erstellen eines Kontos für die Pushinstallation von Mobility Service.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Schritt 5: Replizieren der lokalen VMs in Azure mit Site Recovery

Bevor Contoso eine Migration zu Azure durchführen kann, muss das Unternehmen die Replikation einrichten und aktivieren.

### <a name="set-a-replication-goal"></a>Festlegen eines Replikationsziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel ausgewählt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass sich seine Computer in einer lokalen Umgebung befinden, unter VMware ausgeführt und in Azure repliziert werden.

    ![Replikationsziel](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, muss es durch Auswahl von **Ja, ist abgeschlossen** bestätigen, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Szenario nur eine VM. Daher ist keine Bereitstellungsplanung erforderlich.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso muss seine Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage heruntergeladen, die für die Bereitstellung des Site Recovery-Konfigurationsservers als hoch verfügbare, lokale VMware-VM verwendet wird. Nachdem der Konfigurationsserver eingerichtet wurde und ausgeführt wird, wird er im Tresor registriert.

Auf dem Konfigurationsserver werden einige Komponenten ausgeführt:

- Die Konfigurationsserverkomponente, die die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.

Contoso führt diese Schritte wie folgt aus:


1. Das Unternehmen lädt die OVF-Vorlage im Tresor von **Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver** herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Das Unternehmen importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert die Lizenzvereinbarung und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet sich Contoso als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Nachdem die Verbindung hergestellt wurde, meldet sich das Unternehmen bei seinem Azure-Abonnement an. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.

    ![Konfigurationsserver registrieren](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
9. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
10. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.
        ![Tresor](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Anschließend werden MySQL Server und VMWare PowerCLI heruntergeladen und installiert. 
11. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des Servers von vCenter Server oder des vSphere-Hosts an. Der Standardport wird beibehalten, und für den Server von vCenter Server wird ein Anzeigename angegeben.
12. Contoso gibt das für die automatische Ermittlung erstellte Konto und die Anmeldeinformationen für die automatische Installation von Mobility Service an. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den VMs.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Contoso überprüft nach Abschluss der Registrierung im Azure-Portal erneut, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung dauert mindestens 15 Minuten. 
8. Site Recovery stellt anschließend mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Contoso gibt nun die Zielreplikationseinstellungen an.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Jetzt kann Contoso eine Replikationsrichtlinie erstellen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Aktivieren der Replikation

Contoso kann nun mit der Replikation der WebVM beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Das Unternehmen wählt aus, dass VMs aktiviert werden sollen. Darüber hinaus wählt es den Server von vCenter Server und den Konfigurationsserver aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Contoso gibt die Zieleinstellungen, einschließlich der Ressourcengruppe und des VNet, und das Speicherkonto an, in dem die replizierten Daten gespeichert werden.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso wählt die WebVM für die Replikation aus, überprüft die Replikationsrichtlinie und aktiviert die Replikation. Site Recovery installiert Mobility Service auf der VM, wenn die Replikation aktiviert ist.
 
    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.
5. Unter **Zusammenfassung** im Azure-Portal kann Contoso die Struktur für die VMs anzeigen, die in Azure repliziert werden.

    ![Infrastrukturansicht](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- Erfahren Sie mehr über das [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>Schritt 5: Installieren des Datenmigrations-Assistenten (DMA)

Contoso migriert die SmartHotel-Datenbank mithilfe des DMA zur Azure-VM **SQLAOG1**. Das Unternehmen geht bei der Einrichtung des DMA wie folgt vor:

1. Es lädt das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) auf die lokale SQL Server-VM (**SQLVM**) herunter.
2. Es führt das Setup (DownloadMigrationAssistant.msi) auf der VM aus.
3. Auf der Seite **Fertigstellen** wählt das Unternehmen **Microsoft-Datenmigrations-Assistent starten** aus, bevor es den Assistenten beendet.

## <a name="step-6-migrate-the-database-with-dma"></a>Schritt 6: Migrieren der Datenbank mit DMA

1. Im DMA führt das Unternehmen eine neue Migration aus: **SmartHotel**.
2. Es wählt **SQL Server auf virtuellen Azure-Computern**als **Typ des Zielservers** aus. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. In den Migrationsdetails fügt das Unternehmen **SQLVM** als Quellserver und **SQLAOG1** als Ziel hinzu. Es gibt für jeden Computer Anmeldeinformationen an.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Das Unternehmen erstellt für die Datenbank- und Konfigurationsinformationen eine lokale Freigabe. Das SQL-Dienstkonto muss auf der VM SQLVM und der VM SQLAOG1 mit Schreibzugriff darauf zugreifen können.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso wählt die zu migrierenden Anmeldedaten aus und beginnt mit der Migration. Nach Abschluss der Migration zeigt der DMA an, dass die Migration erfolgreich ausgeführt wurde.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Das Unternehmen überprüft, ob die Datenbank auf der VM **SQLAOG1** ausgeführt wird.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS verbindet sich über eine Site-to-Site-VPN-Verbindung zwischen dem Contoso-Rechenzentrum und Azure mit der lokalen SQL Server-VM und migriert anschließend die Datenbank.

## <a name="step-7-protect-the-database"></a>Schritt 7: Schutz der Datenbank

Wenn die App-Datenbank auf **SQLAOG1** ausgeführt wird, kann Contoso diese jetzt mithilfe von Always On-Verfügbarkeitsgruppen schützen. Das Unternehmen konfiguriert Always On mit SQL Server Management Studio und weist anschließend über das Windows-Clustering einen Listener zu. 

### <a name="create-an-alwayson-availability-group"></a>Erstellen einer Always On-Verfügbarkeitsgruppe

1. Contoso klickt in SQL Server Management Studio mit der rechten Maustaste auf **Hochverfügbarkeit mit Always On**, um den **Assistenten für neue Verfügbarkeitsgruppen** zu starten.
2. Unter **Optionen angeben** gibt das Unternehmen der Verfügbarkeitsgruppe den Namen **SHAOG**. Unter **Datenbanken auswählen** wählt es die SmartHotel-Datenbank aus.

    ![Always On-Verfügbarkeitsgruppe](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. Unter **Specify Replicas** (Replikate angeben) fügt das Unternehmen die zwei SQL-Knoten als Verfügbarkeitsreplikate hinzu und konfiguriert diese so, dass ein automatisches Failover mit synchronem Commit bereitgestellt wird.

     ![Always On-Verfügbarkeitsgruppe](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Contoso konfiguriert einen Listener für die Gruppe (**SHAOG**) und den Port. Die IP-Adresse des internen Lastenausgleichs wird als statische IP-Adresse (10.245.40.100) hinzugefügt.

    ![Always On-Verfügbarkeitsgruppe](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. Unter **Datensynchronisierung auswählen** aktiviert das Unternehmen das automatische Seeding. Mit dieser Option erstellt SQL Server für jede Datenbank in der Gruppe automatisch die sekundären Replikate, sodass Contoso diese nicht manuell sichern und wiederherstellen muss. Nach der Überprüfung wird die Verfügbarkeitsgruppe erstellt.

    ![Always On-Verfügbarkeitsgruppe](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Bei der Erstellung der Gruppe ist ein Problem aufgetreten. Contoso verwendet in Active Directory nicht die integrierte Sicherheit von Windows. Folglich müssen Berechtigungen für die SQL-Anmeldung erteilt werden, damit die Rollen für den Windows-Failovercluster erstellt werden können.

    ![Always On-Verfügbarkeitsgruppe](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Nach der Erstellung der Gruppe kann Contoso diese in SQL Server Management Studio anzeigen.

### <a name="configure-a-listener-on-the-cluster"></a>Konfigurieren eines Listeners für den Cluster

Im letzten Schritt der Einrichtung der SQL-Bereitstellung konfiguriert Contoso den internen Lastenausgleich als Listener für den Cluster und schaltet den Listener online. Hierzu verwendet das Unternehmen ein Skript.

![Listener für Cluster](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Überprüfen der Konfiguration

Nachdem Contoso die Einrichtung abgeschlossen hat, kann das Unternehmen nun über eine funktionale Verfügbarkeitsgruppe in Azure verfügen, die die migrierte Datenbank verwendet. Das Unternehmen überprüft dies, indem es eine Verbindung mit dem internen Lastenausgleich in SQL Server Management Studio herstellt.

![Verbinden mit dem internen Lastenausgleich](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Benötigen Sie weitere Hilfe?**
- Weitere Informationen zum Erstellen einer [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) und eines [Listeners](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Sie können den Vorgang [Einrichten des Clusters für die Verwendung der IP-Adresse des Lastenausgleichs](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address) manuell durchführen.
- [Weitere Informationen](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) zum Erstellen und Verwenden von SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Schritt 8: Migrieren der VM mit Site Recovery

Contoso führt ein schnelles Testfailover aus und migriert anschließend die VMs.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Das Ausführen eines Testfailovers dient zur Sicherstellung, dass vor der Migration alles wie erwartet funktioniert. 

1. Contoso führt ein Testfailover zum letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Testfailover durchführen: 

    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Contoso prüft, ob die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. 
4. Nach der Überprüfung bereinigt Contoso das Failover. Darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert. 

### <a name="run-a-failover"></a>Ausführen eines Failovers

1. Nachdem Contoso überprüft hat, ob das Testfailover wie erwartet funktioniert, erstellt das Unternehmen einen Wiederherstellungsplan für die Migration und fügt WEBVM zum Plan hinzu.

     ![Wiederherstellungsplan](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Das Unternehmen führt ein Failover für den Plan aus. Es entscheidet, ein Failover für den letzten Wiederherstellungspunkt auszuführen. Zudem gibt das Unternehmen an, dass Site Recovery versuchen sollte, die lokale VM vor dem Auslösen des Failovers herunterzufahren.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

    ![Wiederherstellungsplan](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Nach dieser Überprüfung führt das Unternehmen die Migration durch, um den Migrationsprozess zu beenden. Darüber hinaus beendet es die Replikation für die VM und die Site Recovery-Abrechnung für die VM.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Der letzte Schritt im Migrationsprozess besteht in der Aktualisierung der Verbindungszeichenfolge der Anwendung, um auf die migrierte Datenbank zu verweisen, die auf dem SHAOG-Listener ausgeführt wird. Diese Konfiguration wird auf der WEBVM geändert, die jetzt in Azure ausgeführt wird.  Diese Konfiguration befindet sich in der Datei „web.config“ der ASP-Anwendung. 

1. Suchen Sie die Datei unter C:\inetpub\SmartHotelWeb\web.config.  Ändern Sie den Namen des Servers, damit der FQDN der Always On-Verfügbarkeitsgruppe widergespiegelt wird: shaog.contoso.com.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Nachdem Contoso die Datei aktualisiert und gespeichert hat, startet das Unternehmen IIS auf WEBVM neu. Hierzu verwendet es IISRESET/RESTART aus einer Eingabeaufforderung.
2. Nachdem IIS neu gestartet wurde, verwendet die Anwendung nun die Datenbank, die auf der verwalteten SQL-Instanz ausgeführt wird.


**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers. 
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach der Migration wird die App SmartHotel auf einer Azure-VM ausgeführt. Die SmartHotel-Datenbank befindet sich im Azure SQL-Cluster.

Contoso muss jetzt folgende Schritte für die Bereinigung durchführen:  

- Entfernen der lokalen VMs aus dem vCenter-Bestand.
- Entfernen der VMs aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation und Anzeigen der neuen Speicherorte und IP-Adressen für VMs.
- Überprüfen sämtlicher Ressourcen, die mit den außer Betrieb genommenen VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen zur Widerspiegelung der neuen Konfiguration.
- Hinzufügen der beiden neuen VMs (SQLAOG1 und SQLAOG2) zu Systemen für die Produktionsüberwachung.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüft die Azure-VMs WEBVM, SQLAOG1 und SQLAOG2 auf eventuell vorhandene Sicherheitsprobleme. 

- Das Unternehmen überprüft die Netzwerksicherheitsgruppen (NSGs) der VM zur Steuerung des Zugriffs. Mithilfe von Netzwerksicherheitsgruppen wird sichergestellt, dass nur für die App zulässiger Datenverkehr übergeben werden kann.
- Das Unternehmen zieht darüber hinaus in Erwägung, die Daten über Azure Disk Encryption und Azure KeyVault zu sichern.
- Es sollte die Transparent Data Encryption (TDE) auswerten und diese anschließend in der SmartHotel-Datenbank aktivieren, die in der neuen SQL Always On-Verfügbarkeitsgruppe ausgeführt wird. [Weitere Informationen](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017)

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) über Sicherheitsverfahren für VMs.

### <a name="backups"></a>Backups

Contoso sichert die Daten in WEBVM, SQLAOG1 und SQLAOG2 mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

1. Contoso verfügt über eine Lizenzierung für die WEBVM und nutzt den Azure-Hybridvorteil.  Es konvertiert die vorhandenen Azure-VMs, um von diesen Preisen zu profitieren.
2. Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 

## <a name="conclusion"></a>Zusammenfassung

Im vorliegenden Artikel hat Contoso der SmartHotel-App in Azure einen neuen Host zugewiesen, indem das Unternehmen die Front-End-VM der App mithilfe des Site Recovery-Diensts zu Azure migriert hat. Das Unternehmen hat die App-Datenbank zu einem in Azure bereitgestellten SQL Server-Cluster migriert und dieses in einer SQL Server Always On-Verfügbarkeitsgruppe geschützt.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel dieser Reihe wird erläutert, wie Contoso für die Service Desk-App osTicket einen neuen Host zuweist, die unter Linux ausgeführt wird und mit einer MySQL-Datenbank bereitgestellt wurde.



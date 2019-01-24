---
title: Zuweisen eines neuen Hosts zu einer Contoso-App mit Migration zu Azure-VMs mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einer lokalen App mit einer Lift & Shift-Migration von lokalen Computern zu Azure mithilfe des Azure Site Recovery-Diensts einen neuen Host zuweisen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 36a84618a998dd2e06f5563fb8706a1dc8400e59
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823350"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Contoso-Migration: Rehosten einer lokalen App auf Azure-VMs


In diesem Artikel wird erläutert, wie Contoso der lokalen SmartHotel360-App in Azure durch Migrieren der App-VMs zu Azure-VMs einen neuen Host zuweist.


Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso lokale Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Wir fügen im Laufe der Zeit weitere Artikel hinzu.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Contoso führt eine Bewertung seiner lokalen App SmartHotel360 durch, die in VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten. | Verfügbar
[Artikel 4: Rehosten einer App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert den virtuellen Front-End-Computer der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank mit dem [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten Azure SQL-Datenbank-Instanz. | Verfügbar
Artikel 5: Rehosten einer App auf Azure-VMs | Contoso migriert die VMs der App SmartHotel360 mithilfe des Site Recovery-Diensts zu Azure-VMs. | Dieser Artikel
[Artikel 6: Rehosten einer App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die App SmartHotel360. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar
[Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe von Azure Site Recovery per Lift & Shift eine Migration der Linux-App „osTicket“ zu virtuellen Azure-Computern durch. | Verfügbar
[Artikel 8: Rehosten einer Linux-App auf Azure-VMs und in Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Site Recovery zu virtuellen Azure-Computern und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz. | Verfügbar
[Artikel 9: Umgestalten einer App in Azure-Web-Apps und Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert die App SmartHotel360 zu einer Azure-Web-App und die App-Datenbank mithilfe des Datenbankmigrations-Assistenten zu einer Azure SQL Server-Instanz. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in Azure-Web-Apps und Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Traffic Manager zu einer Azure-Web-App in mehreren Azure-Regionen. Zur Sicherstellung der Continuous Delivery erfolgt eine Integration in GitHub. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten einer TFS-Bereitstellung für Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer App in Azure-Containern und Azure SQL-Datenbank](contoso-migration-rearchitect-container-sql.md) | Contoso migriert seine SmartHotel-App zu Azure. Anschließend wird die App-Webschicht zu einem Windows-Container umstrukturiert, der in Azure Service Fabric ausgeführt wird, und die Datenbank wird zu einer Azure SQL-Datenbank umstrukturiert. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z.B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar
[Artikel 14: Skalieren einer Migration zu Azure](contoso-migration-scale.md) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor. | Verfügbar


Im vorliegenden Artikel migriert Contoso das zweistufige Windows. Die .NET-App SmartHotel360 wird auf VMware-VMs in Azure ausgeführt. Falls Sie diese App verwenden möchten: Sie wird als Open-Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.



## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was das Unternehmen mit dieser Migration erreichen möchte:

- **Unternehmenswachstum**: Contoso wächst, und daher geraten die lokalen Systeme und Infrastrukturen unter Druck.
- **Risikobegrenzung**: Die SmartHotel360-App ist wichtig für das Geschäft von Contoso. Das Unternehmen möchte die App ohne jedes Risiko in Azure verschieben.
- **Erweitern**: Contoso möchte die App nicht ändern, möchte aber sicherstellen, dass sie stabil ist.


## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat sich folgende Ziele für die Migration gesetzt. Diese Ziele werden verwendet, um die beste Migrationsmethode zu bestimmen:

- Nach der Migration sollte die App in Azure das gleiche Leistungsvermögen aufweisen wie derzeit in der VMware-Umgebung. Die App bleibt lokal und in der Cloud gleichermaßen wichtig.
- Contoso möchte nicht in diese App investieren. Die Investition ist wichtig für das Geschäft, in ihrer derzeitigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Contoso möchte das Vorgangsmodell für diese App nicht ändern. Contoso möchte in der Cloud wie derzeit damit interagieren können.
- Contos möchte die App-Funktionalität nicht im Geringsten ändern. Nur der Speicherort der App ändert sich.


## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess sowie die für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die App ist auf zwei VMs aufgeteilt (**WEBVM** und **SQLVM**).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).

### <a name="proposed-architecture"></a>Vorgeschlagene Architektur

- Da es sich bei der App um eine Produktionsworkload handelt, befinden sich die virtuellen Computer der App in Azure in der Ressourcengruppe „ContosoRG“ für die Produktion.
- Die virtuellen Computer der App werden zur primären Azure-Region (USA, Osten 2) migriert und im Produktionsnetzwerk (VNET-PROD-EUS2) platziert.
- Der virtuelle Front-End-Computer wird im Front-End-Subnetz (PROD-FE-EUS2) im Produktionsnetzwerk platziert.
- Der virtuelle Datenbankcomputer wird im Datenbanksubnetz (PROD-DB-EUS2) im Produktionsnetzwerk platziert.
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.

![Szenarioarchitektur](./media/contoso-migration-rehost-vm/architecture.png)

### <a name="database-considerations"></a>Überlegungen zu Datenbanken

Im Rahmen des Lösungsentwurfs hat Contoso einen Featurevergleich zwischen Azure SQL-Datenbank und SQL Server durchgeführt. Hierbei haben die folgenden Überlegungen Contoso dabei unterstützt, sich für einen virtuellen Azure-IaaS-Computer zu entscheiden, auf dem SQL Server ausgeführt wird:

- Der Einsatz eines virtuellen Azure-Computers mit SQL Server erscheint als optimale Lösung, wenn Contoso das Betriebssystem oder den Datenbankserver anpassen muss, oder wenn Drittanbieter-Apps auf demselben virtuellen Computer installiert und ausgeführt werden sollen.
- Dank Software Assurance kann Contoso vorhandene Lizenzen künftig mit dem Azure-Hybridvorteil für SQL Server zu ermäßigten Preisen gegen eine verwaltete SQL-Datenbank-Instanz austauschen. So können 30% bei der verwalteten Instanz eingespart werden.



### <a name="solution-review"></a>Überprüfung der Lösung

Contoso bewertet den vorgeschlagen Entwurf anhand einer Liste mit Vor- und Nachteilen.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Beide virtuellen Computer der App werden ohne Änderungen nach Azure verlagert, was die Migration vereinfacht.<br/><br/> Da Contoso beide virtuellen Computer der App per Lift & Shift migriert, sind für die App-Datenbank keine besonderen Konfigurations- oder Migrationstools erforderlich.<br/><br/> Contoso kann seine Investition in die Software Assurance mit dem Azure-Hybridvorteil nutzen.<br/><br/> Contoso behält die vollständige Kontrolle über die virtuellen Computer der App in Azure.
**Nachteile** | Auf WEBVM und SQLVM wird Windows Server 2008 R2 ausgeführt. Das Betriebssystem wird für bestimmte Rollen von Azure unterstützt (Juli 2018). [Weitere Informationen](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)<br/><br/> Die Web- und Datenschichten der App bleiben ein Single Point of Failover.</br><br/> SQLVM wird unter SQL Server 2008 R2 ausgeführt. Dieses Betriebssystem ist nicht im grundlegenden Support enthalten. Für virtuelle Azure-Computer wird es jedoch unterstützt (Juli 2018). [Weitere Informationen](https://support.microsoft.com/en-us/help/956893)<br/><br/> Contoso muss die App weiterhin als virtuelle Azure-Computer unterstützen, anstatt auf einen verwalteten Dienst wie Azure App Service oder Azure SQL-Datenbank umzustellen.



### <a name="migration-process"></a>Migrationsprozess

Contoso wird die virtuellen Front-End- und Datenbank-Computer der App mithilfe von Site Recovery zu virtuellen Azure-Computern migrieren:

- Im ersten Schritt bereitet Contoso Azure-Komponenten für Site Recovery vor, richtet diese ein und bereitet die lokale VMware-Infrastruktur vor.
- Eine [Azure-Infrastruktur](contoso-migration-infrastructure.md) wurde bereits eingerichtet, sodass Contoso nur noch einige Azure-Komponenten speziell für Site Recovery hinzufügen muss.
- Wenn alle Vorbereitungen getroffen sind, kann Contoso mit dem Replizieren der virtuellen Computer beginnen.
- Wenn die Replikation aktiviert wurde und funktioniert, migriert Contoso den virtuellen Computer via Failover zu Azure.

![Migrationsprozess](./media/contoso-migration-rehost-vm/migraton-process.png)



### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an. Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.


## <a name="prerequisites"></a>Voraussetzungen

Für die Ausführung dieses Szenarios benötigt Contoso Folgendes.

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Contoso hat in einem früheren Artikel dieser Reihe Abonnements erstellt. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md).
**Azure-Infrastruktur** | [Weitere Informationen](contoso-migration-infrastructure.md) zur Vorgehensweise von Contoso beim Einrichten einer Azure-Infrastruktur.<br/><br/> Erfahren Sie mehr zu spezifischen [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network)- und [Speicheranforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) für Site Recovery.
**Lokale Server** | Lokale vCenter-Server sollten als Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> ESXi-Hosts sollten Version 5.5, 6.0 oder 6.5 ausführen.<br/><br/> Mindestens eine VMware-VM sollte auf dem ESXi-Host ausgeführt werden.
**Lokale VMs** | VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.


## <a name="scenario-steps"></a>Szenarioschritte

Contoso-Administratoren gehen bei der Ausführung der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Vorbereiten von Azure für Site Recovery**: Das Unternehmen erstellt ein Azure-Speicherkonto zum Speichern replizierter Daten sowie einen Recovery Services-Tresor.
> * **Schritt 2: Vorbereiten einer lokalen VMware-Instanz für Site Recovery**: Das Unternehmen bereitet Konten für die VM-Ermittlung und Agent-Installation sowie das Herstellen einer Verbindung mit Azure-VMs nach dem Failover vor.
> * **Schritt 3: Replizieren von VMs**: Das Unternehmen richtet die Replikation ein und startet das Replizieren von VMs in Azure-Storage.
> * **Schritt 4: Migrieren von VMs mit Site Recovery**: Es wird ein Testfailover durchgeführt, um sicherzustellen, dass alles funktioniert, und anschließend wird ein vollständiges Failover für die Migration der VMs zu Azure aus.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Schritt 1: Vorbereiten von Azure für den Site Recovery-Dienst

Im Folgenden werden die Azure-Komponenten aufgeführt, die Contoso für die Migration der VMs zu Azure benötigt:

- Ein VNET, in dem sich Azure-VMs befinden, wenn diese während eines Failovers erstellt werden.
- Ein Azure-Speicherkonto für die Speicherung replizierter Daten.
- Ein Recovery Services-Tresor in Azure.

Das Unternehmen geht bei der Einrichtung dieser Komponenten wie folgt vor:

1. Einrichten eines Netzwerks: Im Rahmen der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md) hat Contoso bereits ein Netzwerk eingerichtet, das für Site Recovery verwendet werden kann.

    - Bei der SmartHotel360-App handelt es sich um eine Produktions-App. Die VMs werden zum Azure-Produktionsnetzwerk (VNET-PROD-EUS2) in der primären Region „USA, Osten 2“ migriert.
    - Beide VMs werden in der Ressourcengruppe ContosoRG bereitgestellt, die für Produktionsressourcen verwendet wird.
    - Die App-Front-End-VM (WEBVM) wird zum Front-End-Subnetz (PROD-FE-EUS2) im Produktionsnetzwerk migriert.
    - Die App-Datenbank-VM (SQLVM) wird zum Datenbanksubnetz (PROD-DB-EUS2) im Produktionsnetzwerk migriert.

2. Einrichten eines Speicherkonto: Contoso erstellt ein Azure-Speicherkonto (contosovmsacc20180528) in der primären Region.
    - Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
    - Contoso verwendet ein allgemeines Konto mit Standardspeicher und LRS-Replikation.

    ![Site Recovery-Speicher](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Erstellen eines Tresor: Nach dem Einrichten des Netzwerks und des Speicherkontos kann Contoso nun einen Recovery Services-Tresor (ContosoMigrationVault) erstellen und diesen in der Ressourcengruppe „ContosoFailoverRG“ in der primären Region „USA, Osten 2“ platzieren.

    ![Recovery Services-Tresor](./media/contoso-migration-rehost-vm/asr-vault.png)

**Benötigen Sie weitere Hilfe?**

[Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) zum Einrichten von Azure für Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 2: Vorbereiten einer lokalen VMware-Instanz für Site Recovery

Contoso bereit lokal Folgendes vor:

- Ein Konto auf dem vCenter-Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung.
- Ein Konto, das die automatische Installation des Mobility Service auf VMware-VMs ermöglicht.
- Lokale VM-Einstellungen, damit Contoso nach einem Failover eine Verbindung mit den replizierten Azure-VMs herstellen kann.


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs.
- Orchestrieren von Replikation, Failover und Failback für VMs.
- Dafür ist mindestens ein Konto mit Lesezugriff erforderlich. Auf dem Konto sollten Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer möglich sein.

Contoso-Administratoren richten das Konto wie folgt ein:

1. Es wird eine Rolle auf vCenter-Ebene erstellt.
2. Anschließend werden dieser Rolle die erforderlichen Berechtigungen zugewiesen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Mobility Service muss auf jeder VM installiert sein.

- Site Recovery kann eine automatische Pushinstallation des Mobility Service durchführen, wenn die VM-Replikation aktiviert ist.
- Ein Konto ist erforderlich, über das Site Recovery für die Pushinstallation auf die VM zugreifen kann. Sie geben dieses Konto an, wenn Sie die Replikation einrichten.
- Sie benötigen ein Domänenkonto oder ein lokales Konto mit Berechtigungen für die Installation auf den VMs.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover möchte Contoso eine Verbindung mit den Azure-VMs herstellen. Dazu führen Contoso-Administratoren vor der Migration folgende Schritte aus:

1. Für den Zugriff über das Internet:

 - Aktivieren von RDP auf dem lokalen virtuellen Computer vor dem Failover.
 - Sicherstellen, dass TCP- und UDP-Regeln zum **öffentlichen** Profil hinzugefügt wurden.
 - Bei allen Profilen unter **Windows-Firewall** > **Zugelassene Apps** überprüfen, ob RDP zulässig ist.

2. Für den Zugriff über Site-to-Site-VPN:

 - RDP auf dem lokalen Computer aktivieren.
 - RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private und Domänennetzwerke** zulassen.
 - SAN-Richtlinie des Betriebssystems auf der lokalen VM auf **OnlineAll** festlegen.

Bei der Ausführung eines Failovers sind darüber hinaus folgende Aspekte zu überprüfen:

- Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls ist nach Abschluss des Updates die Anmeldung bei der VM nicht mehr möglich.
- Nach dem Failover kann **Startdiagnose** aktiviert werden, um einen Screenshot der VM anzuzeigen. Falls dies nicht funktioniert, sollte überprüft werden, ob die VM ausgeführt wird, und die folgenden [Tipps zur Problembehandlung](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gelesen werden.


**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) zum Erstellen und Zuweisen von Rollen für die automatische Ermittlung.
- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) zum Erstellen eines Kontos für die Pushinstallation von Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Schritt 3: Replizieren der lokalen VMs

Bevor Contoso-Administratoren eine Migration zu Azure durchführen können, müssen sie die Replikation einrichten und aktivieren.

### <a name="set-a-replication-goal"></a>Festlegen eines Replikationsziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel ausgewählt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass sich seine Computer in einer lokalen Umgebung befinden, unter VMware ausgeführt und in Azure repliziert werden.

    ![Replikationsziel](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, bestätigt es durch Auswahl von **Ja, ist abgeschlossen**, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Szenario nur zwei VMs. Daher ist keine Bereitstellungsplanung erforderlich.


### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso-Administratoren müssen die Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage heruntergeladen, die für die Bereitstellung des Site Recovery-Konfigurationsservers als hoch verfügbare, lokale VMware-VM verwendet wird. Nachdem der Konfigurationsserver eingerichtet wurde und ausgeführt wird, wird er im Tresor registriert.

Auf dem Konfigurationsserver werden einige Komponenten ausgeführt:

- Die Konfigurationsserverkomponente, die die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.



Contoso-Administratoren führen diese Schritte wie folgt aus:

1. Das Unternehmen lädt die OVF-Vorlage im Tresor von **Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver** herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-vm/add-cs.png)

2. Das Unternehmen importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3. Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert die Lizenzvereinbarung und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet sich Contoso als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Nachdem die Verbindung hergestellt wurde, meldet sich das Unternehmen bei seinem Azure-Abonnement an. Mit den Anmeldeinformationen muss der Zugriff auf den Tresor möglich sein, in dem der Konfigurationsserver registriert werden soll.

    ![Konfigurationsserver registrieren](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
9. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
10. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.
        ![Tresor](./media/contoso-migration-rehost-vm/cswiz1.png)

10. MySQL Server und VMWare PowerCLI werden heruntergeladen und installiert.
11. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des vCenter-Servers oder des vSphere-Hosts an. Der Standardport wird beibehalten und für den Server in Azure ein Anzeigename angegeben.
12. Contoso gibt das für die automatische Ermittlung erstellte Konto und die Anmeldeinformationen für die automatische Installation von Mobility Service an. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den VMs.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Nach Abschluss der Registrierung im Azure-Portal überprüfen die Administratoren erneut, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern.
8. Site Recovery stellt anschließend mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Contoso-Administratoren geben nun die Zielreplikationseinstellungen an.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Jetzt können Contoso-Administratoren eine Replikationsrichtlinie erstellen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.

        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-vm/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet.

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Aktivieren der Replikation für WEBVM

Wenn alles ordnungsgemäß ist, können Contoso-Administratoren die Replikation für die virtuellen Computer aktivieren. Es beginnt mit WebVM.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Das Unternehmen wählt aus, dass VMs aktiviert werden sollen. Darüber hinaus wählt es den Server von vCenter Server und den Konfigurationsserver aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Die Zieleinstellungen einschließlich der Ressourcengruppe und des Azure-Netzwerks sowie das Speicherkonto werden ausgewählt.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Sie wählen **WebVM** für die Replikation aus, überprüfen die Replikationsrichtlinie und aktivieren die Replikation.

    - In dieser Phase wählen sie nur WEBVM aus, da VNET und Subnetz ausgewählt werden müssen, und die virtuellen Computer der App in unterschiedlichen Subnetzen platziert werden.
    - Wenn die Replikation aktiviert ist, wird Mobility Service von Site Recovery automatisch auf der VM installiert.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.
6. Unter **Zusammenfassung** im Azure-Portal können sie die Struktur für die virtuellen Computer anzeigen, die in Azure repliziert werden.


### <a name="enable-replication-for-sqlvm"></a>Aktivieren der Replikation für die SQLVM

Contoso-Administratoren können jetzt mit dem oben beschriebenen Prozess mit dem Replizieren des SQLVM-Computers beginnen.

1. Die Quelleinstellungen werden ausgewählt.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Dann werden die Zieleinstellungen festgelegt.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Die SQLVM wird für die Replikation ausgewählt.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Es wird die gleiche Replikationsrichtlinie wie für die WEBVM angewandt und die Replikation aktiviert.

    ![Infrastrukturansicht](./media/contoso-migration-rehost-vm/essentials.png)

**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- Erfahren Sie mehr über das [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Schritt 4: Migrieren der VMs

Contoso-Administratoren führen ein schnelles Testfailover und dann ein vollständiges Failover aus, um die virtuellen Computer zu migrieren.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Mit einem Testfailover lässt sich sicherstellen, dass alles wie erwartet funktioniert.

1. Sie führen ein Testfailover auf den letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist.
3. Testfailover durchführen:

    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Sie prüfen, ob der virtuelle Computer die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
4. Nach der Überprüfung wird das Failover bereinigt, und darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert.

### <a name="create-and-customize-a-recovery-plan"></a>Erstellen und Anpassen eines Wiederherstellungsplans

 Nachdem sie überprüft haben, ob das Testfailover wie erwartet funktioniert, erstellen Contoso-Administratoren einen Wiederherstellungsplan für die Migration.

- Ein Wiederherstellungsplan gibt die Reihenfolge an, in der ein Failover auftritt, und wie Azure-VMs in Azure online geschaltet werden.
- Da die App zweischichtig ist, wird der Wiederherstellungsplan so angepasst, dass die Daten-VM (SQLVM) vor der Front-End-VM (WEBVM) gestartet wird.

1. Unter **Wiederherstellungspläne (Site Recovery)** > **+Wiederherstellungsplan** erstellt das Unternehmen einen Plan und fügt die VMs dem Plan hinzu.

    ![Wiederherstellungsplan](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Nach dem Erstellen kann der Plan angepasst werden (**Wiederherstellungspläne** > **SmartHotelMigrationPlan** > **Anpassen**).
2.  WEBVM wird aus **Gruppe 1: Start** entfernt. Dadurch wird sichergestellt, dass die erste Startaktion sich nur auf SQLVM auswirkt.
3.  Unter **+ Gruppe** > **Geschützte Elemente hinzufügen** wird WEBVM „Gruppe 2: Start“ hinzugefügt. Die virtuellen Computer müssen sich in zwei unterschiedlichen Gruppen befinden.


### <a name="migrate-the-vms"></a>Migrieren der VMs


Contoso-Administratoren führen jetzt ein vollständiges Failover aus, um die Migration abzuschließen.

1. Der Wiederherstellungsplan wird ausgewählt und dann **Failover**.
2. Ein Failover zum letzten Wiederherstellungspunkt wird ausgewählt. Außerdem soll Site Recovery versuchen, die lokale VM vor dem Auslösen des Failovers herunterzufahren. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)


3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

    ![Failover](./media/contoso-migration-rehost-vm/failover2.png)

3. Nach der Überprüfung wird die Migration für jeden virtuellen Computer abgeschlossen. Dadurch werden die Replikation für die VM und die Site Recovery-Abrechnung hierfür beendet.

    ![Failover](./media/contoso-migration-rehost-vm/failover3.png)

**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach Abschluss der Migration werden die Schichten der SmartHotel360-App jetzt auf Azure-VMs ausgeführt.

Contoso muss jetzt folgende Schritte für die Bereinigung durchführen:

- Entfernen des WEBVM-Computers aus dem vCenter-Bestand.
- Entfernen des SQLVM-Computers aus dem vCenter-Bestand
- Entfernen von WEBVM und SQLVM aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adressen für die VMs.
- Überprüfen sämtlicher Ressourcen, die mit den VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die App jetzt ausgeführt wird, muss Contoso sie nun vollständig in Azure operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüft die Azure-VMs auf eventuell vorhandene Sicherheitsprobleme.

- Zur Steuerung des Zugriffs überprüft das Team die Netzwerksicherheitsgruppen (NSGs) für die virtuellen Computer. Mithilfe von Netzwerksicherheitsgruppen wird sichergestellt, dass nur für die App zulässiger Datenverkehr die App erreichen kann.
- Das Team zieht darüber hinaus in Erwägung, die Daten mit Azure Disk Encryption und KeyVault zu sichern.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) über Sicherheitsverfahren für VMs.

## <a name="bcdr"></a>BCDR

Für die Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) führt Contoso die folgenden Aktionen durch:

- Schützen von Daten: Contoso sichert die Daten auf den VMs mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- Sicherstellen eines unterbrechungsfreien Betriebs der Apps: Contoso repliziert die App-VMs in Azure mithilfe von Site Recovery in einer sekundären Region. [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)



### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

1. Contoso verfügt über eine Lizenzierung für die VMs und nutzt den Azure-Hybridvorteil. Contoso wird die vorhandenen virtuellen Azure-Computer konvertieren, um von diesen Preisen zu profitieren.
2. Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Multicloud-Kostenverwaltungslösung, die das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert. [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management.

## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel hat Contoso der SmartHotel360-App in Azure einen neuen Host zugewiesen, indem die VMs der App mithilfe des Site Recovery-Diensts zu Azure-VMs migriert wurden.


## <a name="next-steps"></a>Nächste Schritte

Im [nächsten Artikel](contoso-migration-rehost-vm-sql-ag.md) der Reihe zeigen wir Ihnen, wie Contoso dem Front-End-VM der SmartHotel360-App auf einem Azure-VM einen neuen Host zuweist und die Datenbank zu einer SQL Server-AlwaysOn-Verfügbarkeitsgruppe in Azure migriert.


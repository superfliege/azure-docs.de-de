---
title: Umstrukturieren einer Contoso-App in einen Azure-Container und eine Azure SQL-Datenbank-Instanz| Microsoft-Dokumentation
description: Erfahren Sie, wie Contoso eine App in Azure-Windows-Container und eine Azure SQL-Datenbank-Instanz umstrukturieren.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 243b19d19fbce11d77f60bc2eccc4d1c58c85b80
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892183"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Contoso-Migration: Umstrukturieren einer lokalen App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz

In diesem Artikel wird erläutert, wie Contoso die SmartHotel360-App migriert und in Azure umstrukturiert. Contoso migriert die Front-End-VM der App zu einem Azure-Windows-Container und die App-Datenbank zu einer Azure SQL-Datenbank-Instanz.

Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso lokale Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Im Laufe der Zeit werden weitere Artikel hinzugefügt.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Contoso führt eine Bewertung seiner lokalen App SmartHotel360 durch, die in VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten. | Verfügbar
[Artikel 4: Rehosten einer App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert den virtuellen Front-End-Computer der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank mit dem [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten Azure SQL-Datenbank-Instanz. | Verfügbar   
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der App SmartHotel360 mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die App SmartHotel360. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar 
[Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe von Azure Site Recovery per Lift & Shift eine Migration der Linux-App „osTicket“ zu virtuellen Azure-Computern durch. | Verfügbar
[Artikel 8: Rehosten einer Linux-App auf Azure-VMs und in Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Site Recovery zu virtuellen Azure-Computern und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz. | Verfügbar
[Artikel 9: Umgestalten einer App in Azure-Web-Apps und Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert die App SmartHotel360 zu einer Azure-Web-App und die App-Datenbank mithilfe des Datenbankmigrations-Assistenten zu einer Azure SQL Server-Instanz. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in Azure-Web-Apps und Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Traffic Manager zu einer Azure-Web-App in mehreren Azure-Regionen. Zur Sicherstellung der Continuous Delivery erfolgt eine Integration in GitHub. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar 
[Artikel 11: Umgestalten einer TFS-Bereitstellung für Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure. | Verfügbar
Artikel 12: Umstrukturieren einer App in Azure-Containern und Azure SQL-Datenbank | Contoso migriert seine SmartHotel-App zu Azure. Anschließend wird die App-Webschicht zu einem Windows-Container umstrukturiert, der in Azure Service Fabric ausgeführt wird, und die Datenbank wird zu einer Azure SQL-Datenbank umstrukturiert. | Dieser Artikel
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z.B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar 
[Artikel 14: Skalieren einer Migration zu Azure](contoso-migration-scale.md) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor. | Verfügbar

In diesem Artikel wird erläutert, wie Contoso die auf virtuellen VMware-Computern ausgeführte zweistufige Windows WPF- und XAML.Forms-App „SmartHotel360“ zu Azure migriert. Falls Sie diese App verwenden möchten: Sie wird als Open Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.

## <a name="business-drivers"></a>Business-Treiber

Das IT-Führungsteam von Contoso hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was sie mit dieser Migration erreichen möchten:

- **Unternehmenswachstum**: Contoso wächst, und daher geraten seine lokalen Systeme und Infrastrukturen unter Druck.
- **Effizienzsteigerung:** Contoso muss unnötige Verfahren beseitigen und Prozesse für Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schnell sein, weder Geld noch Zeit verschwenden, und Kundenanforderungen schneller bearbeiten.
- **Steigerung der Flexibilität:**  Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Die IT-Experten müssen schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren.  Es darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.
- **Kosten:** Contoso möchte die Lizenzierungskosten minimieren.

## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat sich folgende Ziele für die Migration gesetzt. Anhand dieser Ziele wird die beste Migrationsmethode bestimmt.

**Ziele** | **Details**
--- | --- 
**App-Anforderungen** | Die App wird in Azure so wichtig bleiben wie heute.<br/><br/> Sie sollte die gleichen Leistungsmerkmale wie aktuell in VMware aufweisen.<br/><br/> Contoso möchte Windows Server 2008 R2, unter dem die App derzeit läuft, nicht mehr unterstützen und ist bereit, in die App zu investieren.<br/><br/> Contoso möchte weg von SQL Server 2008 R2 und hin zu einer modernen PaaS-Datenbankplattform, die den Verwaltungsaufwand minimiert.<br/><br/> Seine Investitionen in die SQL Server-Lizenzierung und in die Software Assurance möchte Contoso nutzen, wo immer dies möglich ist.<br/><br/> Contoso möchte in der Lage sein, die Webebene der App zentral hochzuskalieren.
**Einschränkungen** | Die App besteht aus einer ASP.NET-App und einem WCF-Dienst, die auf derselben VM ausgeführt werden. Contoso möchte diese über Azure App Service auf zwei Web-Apps aufteilen. 
**Azure-Anforderungen** | Contoso möchte die App in Azure umziehen und in einem Container ausführen, um die Lebensdauer der App zu verlängern. Das Unternehmen möchte nicht bei Null beginnen, um die App in Azure zu implementieren. 
**DevOps** | Contoso möchte den Betrieb auf ein DevOps-Modell mit Azure DevOps Services für Builds und die Releasepipeline umstellen.

## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess sowie die für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die lokale App SmartHotel360 ist auf zwei VMs aufgeteilt (WEBVM und SQLVM).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5)
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


### <a name="proposed-architecture"></a>Vorgeschlagene Architektur

- Für die Datenbankebene der App verglich Contoso mithilfe [dieses Artikels](https://docs.microsoft.com/azure/sql-database/sql-database-features) Azure SQL-Datenbank mit SQL Server. Die Entscheidung für Azure SQL-Datenbank hat verschiedene Gründe:
    - Azure SQL-Datenbank ist ein verwalteter Dienst für relationale Datenbanken. Er bietet eine vorhersagbare Leistung auf mehreren Serviceleveln bei nahezu keinem Administrationsaufwand. Zu den Vorteilen gehören dynamische Skalierbarkeit ohne Ausfallzeiten, integrierte intelligente Optimierung sowie globale Skalierbarkeit und Verfügbarkeit.
    - Contoso nutzt den einfachen Datenmigrations-Assistenten (DMA), um die lokale Datenbank zu bewerten und zu Azure SQL-Datenbank zu migrieren.
    - Mit der Software Assurance kann Contoso seine vorhandenen Lizenzen mit dem Azure-Hybridvorteil für SQL Server zu ermäßigten Preisen für eine SQL-Datenbank-Instanz austauschen. So sind Einsparungen von bis zu 30 % möglich.
    - SQL-Datenbank bietet eine Reihe von Sicherheitsfeatures, darunter Always Encrypted, dynamische Datenmaskierung und Sicherheit bzw. Bedrohungserkennung auf Zeilenebene.
- Zudem hat Contoso entschieden, die App-Webebene mit Azure DevOps Services in den Windows-Container zu konvertieren.
    - Die App wird mit Azure Service Fabric bereitgestellt und das Windows-Containerimage aus der Azure Container Registry (ACR) gepullt.
    - Ein Prototyp für die Erweiterung der App um die Standpunktanalyse wird als weiterer Dienst in Service Fabric, verbunden mit Cosmos DB, implementiert.  Dieser liest Informationen aus Tweets und zeigt sie in der App an.
- Zur Implementierung einer DevOps-Pipeline verwendet Contoso Azure DevOps für die Quellcodeverwaltung mit Git-Repositorys.  Automatisierte Builds und Releases werden verwendet, um Code zu erstellen und in Azure Container Registry sowie in Azure Service Fabric bereitzustellen.

    ![Szenarioarchitektur](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Überprüfung der Lösung
Contoso bewertet den vorgeschlagen Entwurf anhand einer Liste mit Vor- und Nachteilen.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Der Code der SmartHotel360-App muss für die Migration zu Azure Service Fabric geändert werden. Der Aufwand ist jedoch minimal, da die Service Fabric-SDK Tools für die Änderungen verwendet werden.<br/><br/> Durch den Umstieg auf Service Fabric kann Contoso mit der Entwicklung von Microservices beginnen, um die App im Laufe der Zeit schnell und ohne Gefahr für die ursprüngliche Codebasis zu erweitern.<br/><br/> Windows-Container bieten die gleichen Vorteile wie Container im Allgemeinen. Sie verbessern die Flexibilität, die Portabilität und die Steuerungsmöglichkeiten.<br/><br/> Contoso kann seine Investition in die Software Assurance mit dem Azure-Hybridvorteil für SQL Server und Windows Server nutzen.<br/><br/> Nach der Migration ist die Unterstützung von Windows Server 2008 R2 nicht mehr erforderlich. [Weitere Informationen](https://support.microsoft.com/lifecycle)<br/><br/> Contoso kann die Webebene der App mit mehreren Instanzen konfigurieren, sodass sie kein Single Point of Failure mehr ist.<br/><br/> Das Unternehmen ist nicht mehr auf das veraltete SQL Server 2008 R2-System angewiesen.<br/><br/> SQL-Datenbank unterstützt die technischen Anforderungen von Contoso. Die Administratoren von Contoso haben die lokale Datenbank mithilfe des Datenbankmigrations-Assistenten bewertet und festgestellt, dass sie kompatibel ist.<br/><br/> SQL-Datenbank verfügt über eine integrierte Fehlertoleranz, die Contoso nicht einrichten muss. Dadurch wird sichergestellt, dass die Datenschicht kein Single Point of Failover mehr ist.
**Nachteile** | Container sind komplexer als andere Migrationsoptionen. Die Lernkurve bei Containern könnte für Contoso ein Problem darstellen.  Der Grad der Komplexität steigt und bietet trotz der Kurve einen hohen Nutzen.<br/><br/> Das Betriebsteam von Contoso muss eingearbeitet werden, um Azure, Container und Microservices für die App verstehen und unterstützen zu können.<br/><br/> Wenn Contoso anstelle des Datenmigrationsdiensts den Datenmigrations-Assistenten zur Migration seiner Datenbank verwendet, verfügt das Unternehmen nicht über die entsprechende Infrastruktur für die Migration umfangreicher Datenbanken.



### <a name="migration-process"></a>Migrationsprozess

1. Contoso stellt den Azure Service Fabric-Cluster für Windows bereit.
2. Das Unternehmen stellt eine Azure SQL-Instanz bereit und migriert die SmartHotel360-Datenbank zu dieser Instanz.
3. Contoso konvertiert die VM auf Webebene mithilfe der Service Fabric SDK-Tools in einen Docker-Container.
4. Anschließend verknüpft das Unternehmen den Service Fabric-Cluster mit der ACR-Instanz und stellt die App über Azure Service Fabric bereit.

    ![Migrationsprozess](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenmigrations-Assistent (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Bewertet und erkennt Kompatibilitätsprobleme, die sich auf die Funktion der Datenbank in Azure auswirken können. Der DMA bewertet die Featureparität zwischen SQL-Quellen und -Zielen, und empfiehlt Verbesserungen der Leistung und Zuverlässigkeit. | Sie können das Tool kostenlos herunterladen.
[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) | Bietet einen intelligenten, vollständig verwalteten Dienst für relationale Clouddatenbanken. | Die Kosten ergeben sich durch Features, Durchsatz und Größe. [Weitere Informationen](https://azure.microsoft.com/pricing/details/sql-database/managed/)
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Speichert Images für alle Arten von Containerbereitstellungen. | Die Kosten ergeben sich aus Features, Speicher und Nutzungsdauer. [Weitere Informationen](https://azure.microsoft.com/pricing/details/container-registry/)
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Dient zum Erstellen und Betreiben skalierbarer, verteilter Always On-Apps. | Die Kosten ergeben sich aus Größe, Standort und Dauer der Computeknoten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/service-fabric/)
[Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | Stellt die Pipeline für Continuous Integration und Continuous Deployment (CI/CD) für die App-Entwicklung bereit. Die Pipeline beginnt bei einem Git-Repository für die Verwaltung von App-Code, einem Buildsystem zum Erstellen von Paketen und anderen Buildartefakten sowie einem Releaseverwaltungssystem zum Bereitstellen von Änderungen in Entwicklungs-, Test- und Produktionsumgebungen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Szenarios benötigt Contoso Folgendes:

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Abonnements wurden in einem früheren Artikel dieser Reihe erstellt. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.
**Azure-Infrastruktur** | Contoso hat bereits eine Azure-Infrastruktur eingerichtet. Weitere Informationen finden Sie [hier](contoso-migration-infrastructure.md).
**Voraussetzungen für Entwickler** | Contoso benötigt die folgenden Tools auf einer Entwicklerarbeitsstation:<br/><br/> - [Visual Studio 2017 Community Edition: Version 15.5](https://www.visualstudio.com/)<br/><br/> – .NET-Workload aktiviert.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [Service Fabric-SDK-Version 3.0 oder höher](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) oder Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) festgelegt für die Verwendung von Windows-Containern.



## <a name="scenario-steps"></a>Szenarioschritte

So führt Contoso die Migration durch:

> [!div class="checklist"]
> * **Schritt 1: Bereitstellen einer SQL-Datenbankinstanz in Azure:** Contoso stellt eine SQL-Instanz in Azure bereit. Nach der Migration der Web-Front-End-VM zu einem Azure-Container wird die Containerinstanz mit dem Web-Front-End der App auf diese Datenbank zeigen.
> * **Schritt 2: Erstellen einer ACR-Instanz (Azure Container Registry)**: Contoso stellt eine professionelle Containerregistrierung für die Docker-Containerimages bereit.
> * **Schritt 3: Bereitstellen von Azure Service Fabric**: ES wird ein Service Fabric-Cluster bereitgestellt.
> * **Schritt 4: Verwalten von Service Fabric-Zertifikaten**: Contoso richtet Zertifikate für den Azure DevOps Services-Zugriff auf den Cluster ein.
> * **Schritt 5: Migrieren der Datenbank mit dem DMA:** Contoso migriert die App-Datenbank mit dem Datenmigrations-Assistenten.
> * **Schritt 6: Einrichten von Azure DevOps Services**: Contoso richtet ein neues Projekt in Azure DevOps Services ein und importiert den Code in das Git-Repository.
> * **Schritt 7: Konvertieren der App**: Contoso konvertiert die App mithilfe von Azure DevOps und SDK-Tools in einen Container.
> * **Schritt 8: Einrichten von Build und Release:** Contoso richtet die Build- und Releasepipelines ein, um die App zu erstellen und in ACR sowie im Service Fabric-Cluster zu veröffentlichen.
> * **Schritt 9: Erweitern der App**: Die veröffentlichte App wird von Contoso erweitert, um die Vorteile von Azure-Funktionen zu nutzen, und anschließend mithilfe der Pipeline erneut in Azure veröffentlicht.



## <a name="step-1-provision-an-azure-sql-database"></a>Schritt 1: Bereitstellen einer Azure SQL-Datenbank-Instanz

Die Contoso-Administratoren stellen eine Azure SQL-Datenbank bereit.

1. Sie entscheiden sich dafür, eine **SQL-Datenbank**-Instanz in Azure zu erstellen. 

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Das Unternehmen gibt einen Datenbanknamen an, der mit der Datenbank übereinstimmt, die auf der lokalen VM (**SmartHotel.Registration**) ausgeführt wird. Die Datenbank wird dann in der ContosoRG-Ressourcengruppe platziert. Dies ist die Ressourcengruppe, die das Unternehmen für die Produktionsressourcen in Azure verwendet.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Das fiktive Unternehmen richtet eine neue SQL Server-Instanz (**sql-smarthotel-eus2**) in der primären Region ein.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Den Tarif legt das Unternehmen gemäß seiner Server- und Datenbankanforderungen fest. Zudem entscheidet es sich, Geld mit dem Azure-Hybridvorteil zu sparen, da es bereits über eine SQL Server-Lizenz verfügen.
5. Zur Größenanpassung nutzt es das auf virtuellen Kernen basierende Kaufmodell und legt die Grenzwerte für die erwarteten Anforderungen fest.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Dann erstellt Contoso die Datenbankinstanz.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Nach dem Erstellen der Instanz öffnet das Unternehmen die Datenbank und notiert sich Details, die es für die Migration mit dem Datenbankmigrations-Assistenten benötigt.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Benötigen Sie weitere Hilfe?**

- [Erhalten Sie Hilfe](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bei der Bereitstellung einer SQL-Datenbank-Instanz.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) zu Ressourcengrenzwerten des auf virtuellen Kernen basierenden Kaufmodells.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>Schritt 2: Erstellen einer ACR und Bereitstellen eines Azure-Containers

Der Azure-Container wird mithilfe der exportierten Dateien aus der Web-VM erstellt. Der Container wird in der Azure Container Registry (ACR) gespeichert.


1. Die Administratoren von Contoso erstellen im Azure-Portal eine Container Registry-Instanz.

     ![Containerregistrierung](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Das Unternehmen gibt einen Namen für die Registrierung (**contosoacreus2**) an und platziert sie in der primären Region, in der für seine Infrastrukturressourcen verwendeten Ressourcengruppe. Dann aktiviert es den Zugriff für Administratorbenutzer, und es legt sie als Premium-SKU fest, um die Georeplikation nutzen zu können.

    ![Containerregistrierung](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>Schritt 3: Bereitstellen von Azure Service Fabric

Der SmartHotel360-Container wird im Azure Service Fabric-Cluster ausgeführt. Die Administratoren von Contoso gehen wie folgt vor, um den Service Fabric-Cluster zu erstellen:

1. Erstellen einer Service Fabric-Ressource aus dem Azure Marketplace

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. Unter **Grundlagen** gibt Contoso einen eindeutigen DS-Namen für den Cluster sowie Anmeldeinformationen für den Zugriff auf die lokale VM an. Es platziert die Ressource in der Produktionsressourcengruppe (**ContosoRG**) in der primären Region „USA, Osten 2“.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. In **Knotentypkonfiguration** gibt das Unternehmen einen Knotentypnamen, Einstellungen zur Dauerhaftigkeit, VM-Größe und App-Endpunkte ein.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. In **Schlüsseltresor erstellen** wird ein neuer Schlüsseltresor in der Infrastrukturressourcengruppe erstellt, in der das Zertifikat gespeichert wird.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. In **Zugriffsrichtlinien** aktiviert das Unternehmen den Zugriff auf virtuelle Computer für die Bereitstellung des Schlüsseltresors.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png) 

6. Es gibt einen Namen für das Zertifikat an.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png) 

7. Auf der Seite mit der Zusammenfassung wird der Link kopiert, der zum Herunterladen des Zertifikats benötigt wird. Dies ist erforderlich, um eine Verbindung mit dem Service Fabric-Cluster herzustellen.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png) 

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png) 

8. Nach erfolgreicher Validierung stellt Contoso den Cluster bereit.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png) 

9. Mit dem Assistenten zum Importieren von Zertifikaten importiert das fiktive Unternehmen das heruntergeladene Zertifikat in die Entwicklercomputer. Das Zertifikat wird zum Authentifizieren beim Cluster verwendet.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png) 

10. Nachdem der Cluster bereitgestellt ist, wird eine Verbindung mit dem Service Fabric-Cluster-Explorer hergestellt.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png) 

11. Contoso muss das richtige Zertifikat auswählen.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png) 

12. Der Service Fabric Explorer wird geladen, und der Contoso-Administrator kann den Cluster verwalten.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png) 


## <a name="step-4-manage-service-fabric-certificates"></a>Schritt 4: Verwalten von Service Fabric-Zertifikaten

Contoso benötigt Clusterzertifikate, um Azure DevOps Services den Zugriff auf den Cluster zu ermöglichen. Diese werden von den Contoso-Administratoren eingerichtet.

1. Dazu navigieren sie im Azure-Portal zu „KeyVault“.
2. Sie öffnen die Zertifikate und kopieren den Fingerabdruck des Zertifikats, das im Rahmen des Bereitstellungsprozesses erstellt wurde.

    ![Kopieren des Fingerabdrucks](./media/contoso-migration-rearchitect-container-sql/cert1.png)
 
3. Der Fingerabdruck wird für später in eine Textdatei kopiert.
4. Anschließend fügen sie ein Clientzertifikat hinzu, das als Administratorclientzertifikat für den Cluster konfiguriert wird. Dadurch kann Azure DevOps Services eine Verbindung mit dem Cluster für die App-Bereitstellung in der Releasepipeline herstellen. Hierzu öffnen Sie „KeyVault“ im Portal und klicken auf **Zertifikate** > **Generieren/importieren**.

    ![Generieren des Clientzertifikats](./media/contoso-migration-rearchitect-container-sql/cert2.png)

5. Sie geben den Namen des Zertifikats ein und geben unter **Antragsteller** einen Distinguished Name (X.509) an.

     ![Zertifikatname](./media/contoso-migration-rearchitect-container-sql/cert3.png)

6. Das erstellte Zertifikat laden sie lokal im PFX-Format herunter.

     ![Herunterladen des Zertifikats](./media/contoso-migration-rearchitect-container-sql/cert4.png)

7. Danach navigieren Sie wieder zur Zertifikatliste in „KeyVault“ und kopieren den Fingerabdruck des soeben erstellen Clientzertifikats. Diesen speichern Sie in der Textdatei.

     ![Fingerabdruck des Clientzertifikats](./media/contoso-migration-rearchitect-container-sql/cert5.png)

8. Für die Azure DevOps Services-Bereitstellung muss der Base64-Wert des Zertifikats ermittelt werden. Diesen Schritt führen sie mithilfe von PowerShell auf der lokalen Entwicklerarbeitsstation aus. Die Ausgabe fügen sie für später in eine Textdatei ein.

    ```powershell
        [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\path\to\certificate.pfx")) 
    ```

     ![Base64-Wert](./media/contoso-migration-rearchitect-container-sql/cert6.png)

9. Abschließend fügen sie das neue Zertifikat dem Service Fabric-Cluster hinzu. Hierzu öffnen sie den Cluster im Portal und klicken auf **Sicherheit**.

     ![Hinzufügen des Clientzertifikats](./media/contoso-migration-rearchitect-container-sql/cert7.png)

10. Sie klicken auf **Hinzufügen** > **Administratorclient** und fügen den Fingerabdruck des neuen Clientzertifikats ein. Anschließend klicken sie auf **Hinzufügen**. Der Vorgang kann bis zu 15 Minuten dauern.

     ![Hinzufügen des Clientzertifikats](./media/contoso-migration-rearchitect-container-sql/cert8.png)

## <a name="step-5-migrate-the-database-with-dma"></a>Schritt 5: Migrieren der Datenbank mit dem DMA

Jetzt können die Administratoren von Contoso die SmartHotel360-Datenbank mit dem DMA migrieren.

### <a name="install-dma"></a>Installieren des DMA

1. Es lädt das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) auf die lokale SQL Server-VM (**SQLVM**) herunter.
2. Es führt das Setup (DownloadMigrationAssistant.msi) auf der VM aus.
3. Auf der Seite **Fertigstellen** wählen sie **Launch Microsoft Data Migration Assistant** (Microsoft-Datenmigrations-Assistenten starten) aus, bevor sie den Assistenten beenden.

### <a name="configure-the-firewall"></a>Konfigurieren der Firewall

Für die Verbindungsherstellung mit Azure SQL-Datenbank richten die Administratoren von Contoso eine Firewallregel ein, um den Zugriff zuzulassen.

1. In den Eigenschaften für **Firewall und virtuelle Netzwerke** für die Datenbank gewährt Contoso Zugriff auf Azure-Dienste, und es fügt eine Regel für die Client-IP-Adresse der lokalen SQL Server-VM hinzu.
2. Es wird eine Firewallregel auf Serverebene erstellt.

    ![Firewall](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Benötigen Sie weitere Hilfe?

[Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) über das Erstellen und Verwalten von Firewallregeln für Azure SQL-Datenbank.

### <a name="migrate"></a>Migrieren

Als Nächstes migrieren die Administratoren von Contoso die Datenbank.

1. Im DMA erstellt das Unternehmen ein neues Projekt (**SmartHotelDB**) und wählt **Migration** aus. 
2. Als Quellservertyp wird **SQL Server** und als Ziel **Azure SQL-Datenbank** ausgewählt. 

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

3. In den Migrationsdetails fügt das Unternehmen **SQLVM** als Quellserver und die **SmartHotel.Registration**-Datenbank als Ziel hinzu. 

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

4. Ein Fehler wird angezeigt, der anscheinend mit der Authentifizierung zusammenhängt. Beim Ermitteln der Ursache wird jedoch festgestellt, dass das Problem durch den Punkt (.) im Datenbanknamen verursacht wird. Daher wurde beschlossen, eine neue SQL-Datenbank-Instanz mit dem Namen **SmartHotel-Registration** bereitzustellen, um das Problem zu beheben. Beim erneuten Ausführen des DMA lässt sich **SmartHotel-Registration** jetzt auswählen, und der Assistent kann fortgesetzt werden.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

5. Unter **Objekte auswählen** wählt das Unternehmen die Datenbanktabellen aus und generiert ein SQL-Skript.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

6. Nachdem das Skript durch den DMS erstellt wurde, klickt Contoso auf **Schema bereitstellen**.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

7. Der DMA bestätigt, dass die Bereitstellung erfolgreich war.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

8. Jetzt kann mit der Migration begonnen werden.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

9. Nach Abschluss der Migration kann Contoso überprüfen, ob die Datenbank auf der Azure SQL-Datenbank-Instanz ausgeführt wird.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. Die zusätzliche SQL-Datenbank-Instanz **SmartHotel.Registration** wird von Contoso im Azure-Portal gelöscht.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)


## <a name="step-6-set-up-azure-devops-services"></a>Schritt 6: Einrichten von Azure DevOps Services

Contoso muss die DevOps-Infrastruktur und die Pipelines für die Anwendung erstellen.  Hierzu erstellen die Administratoren von Contoso ein neues Azure DevOps-Projekt, importieren ihren Code und erstellen anschließend die Build- und Releasepipelines.

1.   Im Azure DevOps-Konto von Contoso erstellen sie ein neues Projekt (**ContosoSmartHotelRearchitect**) und verwenden **Git** für die Versionskontrolle.
![Neues Projekt](./media/contoso-migration-rearchitect-container-sql/vsts1.png)

2. Sie importieren das Git-Repository, das derzeit ihren App-Code enthält. Der Code befindet sich in einem [öffentlichen Repository](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) und kann heruntergeladen werden.

    ![Herunterladen des App-Codes](./media/contoso-migration-rearchitect-container-sql/vsts2.png)

3. Nach dem Importieren des Codes verknüpfen sie Visual Studio mit dem Repository und klonen den Code mithilfe von Team Explorer.

4. Nachdem das Repository auf dem Entwicklercomputer geklont wurde, öffnen sie die Projektmappendatei für die App. Die Datei enthält jeweils ein eigenes Projekt für die Web-App und den WCF-Dienst.

    ![Projektmappendatei](./media/contoso-migration-rearchitect-container-sql/vsts4.png)

## <a name="step-7-convert-the-app-to-a-container"></a>Schritt 7: Konvertieren der App in einen Container

Bei der lokalen App handelt es sich um eine herkömmliche dreischichtige App:

- Sie enthält WebForms und einen WCF-Dienst zur Verbindung mit SQL Server.
- Sie verwendet Entity Framework zur Integration mit den Daten in der SQL-Datenbank-Instanz, die über einen WCF-Dienst verfügbar gemacht wird.
- Die WebForms-Anwendung interagiert mit dem WCF-Dienst.

Die Administratoren von Contoso konvertieren die App mit Visual Studio und den SDK-Tools wie folgt in einen Container:


1. Sie prüfen unter Verwendung von Visual Studio die geöffnete Projektmappendatei (SmartHotel.Registration.sln) im Verzeichnis **SmartHotel360-interne-Buchung-Apps\src\Registration** des lokalen Repositorys.  Es werden zwei Apps angezeigt. „SmartHotel.Registration.Web“ (Web-Front-End) und „SmartHotel.Registration.WCF“ (WCF-Dienst).

    ![Container](./media/contoso-migration-rearchitect-container-sql/container2.png)


2. Contoso klickt mit der rechten Maustaste auf die Web-App > **Hinzufügen** > **Unterstützung für Containerorchestrator**.
3. In **Unterstützung für Containerorchestrator hinzufügen** wird **Service Fabric** ausgewählt.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container3.png)
    
4. Sie wiederholen den Vorgang für die App „SmartHotel.Registration.WCF“.
5. Als Nächstes überprüfen sie, wie sich die Lösung geändert hat.

    - Die neue App heißt nun **SmartHotel.RegistrationApplication/**.
    - Sie umfasst zwei Dienste: **SmartHotel.Registration.WCF** und **SmartHotel.Registration.Web**.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container4.png)

6. Die Docker-Datei wurde von Visual Studio erstellt, und die erforderlichen Bilder wurden lokal auf den Entwicklercomputer gezogen.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container5.png)

7. Es wird eine Manifestdatei (**ServiceManifest.xml**) von Visual Studio erstellt und geöffnet. Diese Datei informiert Service Fabric darüber, wie der Container bei der Bereitstellung in Azure konfiguriert werden soll.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container6.png)

8. Eine weitere Manifestdatei (**ApplicationManifest.xml) enthält die Konfigurationsanwendungen für die Container.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container7.png)

9. Sie öffnen die Datei **ApplicationParameters/Cloud.xml** und aktualisieren die Verbindungszeichenfolge, um die App mit der Azure SQL-Datenbank zu verknüpfen. Die Verbindungszeichenfolge kann in der Datenbank über das Azure-Portal ermittelt werden.

    ![Verbindungszeichenfolge](./media/contoso-migration-rearchitect-container-sql/container8.png)

10. Sie committen den aktualisierten Code und pushen ihn an Azure DevOps Services.

    ![Commit](./media/contoso-migration-rearchitect-container-sql/container9.png)

## <a name="step-8-build-and-release-pipelines-in-azure-devops-services"></a>Schritt 8: Build- und Releasepipelines in Azure DevOps Services

Als Nächstes konfigurieren die Contoso-Administratoren Azure DevOps Services für die Durchführung des Build- und Releasevorgangs, um die DevOps-Praktiken umzusetzen.

1. Dazu klicken sie in Azure DevOps Services auf **Build und Release** > **Neue Pipeline**.

    ![Neue Pipeline](./media/contoso-migration-rearchitect-container-sql/pipeline1.png)

2. Sie wählen **Azure DevOps Services Git** und das entsprechende Repository aus.

    ![Git und Repository](./media/contoso-migration-rearchitect-container-sql/pipeline2.png)

3. Unter **Vorlage auswählen** wählen sie Fabric mit Docker-Unterstützung aus.

     ![Fabric und Docker](./media/contoso-migration-rearchitect-container-sql/pipeline3.png)
    
4. Sie ändern das Aktionstag von Images in **Build an image** (Imageerstellung) und konfigurieren die Aufgabe für die Verwendung der bereitgestellten ACR-Instanz.

     ![Registrierung](./media/contoso-migration-rearchitect-container-sql/pipeline4.png)

5. In der Aufgabe zum **Pushen von Images** konfigurieren sie das Image, das an die ACR-Instanz gepusht werden soll, und wählen aus, dass das neueste Tag eingeschlossen werden soll.
6. Unter **Trigger** aktivieren sie Continuous Integration und fügen den Masterbranch hinzu.

    ![Trigger](./media/contoso-migration-rearchitect-container-sql/pipeline5.png)

7. Sie klicken auf **Speichern und in Warteschlange einreihen**, um einen Buildvorgang zu starten.
8. Nach erfolgreicher Ausführung des Buildvorgangs wenden sie sich der Releasepipeline zu. Dazu klicken sie in Azure DevOps Services auf **Releases** > **Neue Pipeline**.

    ![Releasepipeline](./media/contoso-migration-rearchitect-container-sql/pipeline6.png)    

9. Sie wählen die Vorlage **Azure Service Fabric-Bereitstellung** aus und benennen die Phase (**SmartHotelSF**).

    ![Environment](./media/contoso-migration-rearchitect-container-sql/pipeline7.png)

10. Sie geben einen Pipelinenamen (**ContosoSmartHotel360Rearchitect**) an. Für die Phase klicken sie auf **1 job, 1 task** (1 Auftrag, 1 Aufgabe), um die Service Fabric-Bereitstellung zu konfigurieren.

    ![Phase und Aufgabe](./media/contoso-migration-rearchitect-container-sql/pipeline8.png)

11. Als Nächstes klicken sie auf **Neu**, um eine neue Clusterverbindung hinzuzufügen.

    ![Neue Verbindung](./media/contoso-migration-rearchitect-container-sql/pipeline9.png)

12. Unter **Add Service Fabric service connection** (Service Fabric-Dienstverbindung hinzufügen) konfigurieren sie die Verbindung und die Authentifizierungseinstellungen, die von Azure DevOps Services zum Bereitstellen der App verwendet werden. Der Clusterendpunkt kann im Azure-Portal ermittelt werden, und sie fügen **tcp://** als Präfix hinzu.
13. Die gesammelten Zertifikatinformationen werden unter **Fingerabdruck des Serverzertifikats** und **Clientzertifikat** eingegeben.

    ![Zertifikat](./media/contoso-migration-rearchitect-container-sql/pipeline10.png)

13. Sie klicken auf die Pipeline und anschließend auf die Option zum **Hinzufügen eines Artefakts**.

     ![Artefakt](./media/contoso-migration-rearchitect-container-sql/pipeline11.png)

14. Sie wählen das Projekt und die Buildpipeline (neueste Version) aus.

     ![Entwickeln](./media/contoso-migration-rearchitect-container-sql/pipeline12.png)

15. Beachten Sie das Blitzsymbol des Artefakts.

     ![Artefaktstatus](./media/contoso-migration-rearchitect-container-sql/pipeline13.png)

16. Beachten Sie außerdem, dass der Continuous Deployment-Trigger aktiviert ist.

   ![Continuous Deployment aktiviert](./media/contoso-migration-rearchitect-container-sql/pipeline14.png) 

17. Sie klicken auf **Speichern** > **Create a release** (Release erstellen).

    ![Release](./media/contoso-migration-rearchitect-container-sql/pipeline15.png)

18. Nach Abschluss der Bereitstellung wird SmartHotel360 in Service Fabric ausgeführt.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish4.png)

19. Um eine Verbindung mit der App herzustellen, leiten sie den Datenverkehr an die öffentliche IP-Adresse des Azure-Lastenausgleichs weiter, der den Service Fabric-Knoten vorgelagert ist.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-9-extend-the-app-and-republish"></a>Schritt 9: Erweitern der App und erneutes Veröffentlichen

Nachdem die SmartHotel360-App und die SmartHotel360-Datenbank in Azure ausgeführt werden, möchte Contoso die App erweitern.

- Die Contoso-Entwickler erstellen gerade den Prototypen einer neuen .NET Core-Anwendung, die auf dem Service Fabric-Cluster ausgeführt werden soll.
- Die App wird dann zum Pullen von Standpuntkdaten aus Cosmos DB verwendet.
- Diese Daten werden in Form von Tweets vorliegen, die mit einer serverlosen Azure-Funktion und der Cognitive Services-Textanalyse-API verarbeitet werden.

### <a name="provision-azure-cosmos-db"></a>Bereitstellen von Azure Cosmos DB

Im ersten Schritt erstellen die Administratoren von Contoso eine Azure-Cosmos-Datenbank.

1. Dabei wird eine Azure Cosmos DB-Ressource aus dem Azure Marketplace erstellt.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Contoso gibt einen Datenbanknamen (**contosmarthotel**) an, wählt die SQL-API aus und platziert die Ressource in der Produktionsressourcengruppe in der primären Region „USA, Osten 2“.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. In **Erste Schritte** wählt das Unternehmen **Daten-Explorer** aus und fügt eine neue Sammlung hinzu.
4. In **Sammlung hinzufügen** gibt esIDs an und legt die Speicherkapazität und den Durchsatz fest.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. Im Portal öffnet Contoso die neue Datenbank > **Sammlung** > **Dokumente** und klickt auf **Neues Dokument**.
6. Der folgende JSON-Code wird in das Dokumentfenster eingefügt. Dies sind Beispieldaten in Form eines einzelnen Tweets.

    ```json
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel360",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500, 
            "hashtags": [
                ""
            ]
    }
    ```

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend4.png)

7. Contoso lokalisiert den Cosmos DB-Endpunkt und den Authentifizierungsschlüssel. Diese werden in der App verwendet, um sich mit der Sammlung zu verbinden. In der Datenbank klickt Contoso auf **Schlüssel** und kopiert die URI und den Primärschlüssel nach Notepad.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>Aktualisieren der Standpunkt-App

Nach der Bereitstellung der Cosmos DB-Instanz können die Administratoren von Contoso die App konfigurieren, um eine Verbindung damit herzustellen.

1. In Visual Studio öffnet das Unternehmen im Projektmappen-Explorer die Datei ApplicationModern\ApplicationParameters\cloud.xml.

    ![Standpunkt-App](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Die folgenden beiden Parameter werden angegeben:

   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Standpunkt-App](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Erneutes Veröffentlichen der App

Nach dem Erweitern der App veröffentlichen die Administratoren von Contoso die App mithilfe der Pipeline erneut in Azure.

1. Sie committen ihren Code und pushen ihn an Azure DevOps Services. Dadurch werden die Build- und Releasepipelines initiiert.

2. Nach Abschluss der Erstellung und Bereitstellung wird SmartHotel360 in Service Fabric ausgeführt. Die drei Dienste werden nun in der Service Fabric-Verwaltungskonsole angezeigt.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish3.png)

3. Sie können sich nun durch die Dienste klicken und sich vergewissern, dass die SentimentIntegration-App aktiv ist und ausgeführt wird.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach der Migration muss Contoso die folgenden Schritte zur Bereinigung ausführen:  

- Entfernen der lokalen VMs aus dem vCenter-Bestand.
- Entfernen der VMs aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation zur Anzeige der neuen Speicherorte für die App SmartHotel360 Anzeigen, dass die Datenbank in Azure SQL-Datenbank und das Front-End in Service Fabric ausgeführt wird.
- Überprüfen sämtlicher Ressourcen, die mit den außer Betrieb genommenen VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.


## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

- Die Administratoren von Contoso müssen die Sicherheit der neuen Datenbank **SmartHotel-Registration** gewährleisten. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)
- Insbesondere sollte es den Container aktualisieren, sodass dieser SSL mit Zertifikaten verwendet.
- Zum Schutz von Geheimnissen für die Service Fabric-Apps sollte die Verwendung von Key Vault in Erwägung gezogen werden. [Weitere Informationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management)

### <a name="backups"></a>Backups

- Contoso muss die Sicherungsanforderungen für Azure SQL-Datenbank überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)
- Die Administratoren von Contoso sollten die Implementierung von Failovergruppen in Betracht ziehen, um ein regionales Failover für die Datenbank bereitzustellen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)
- Die geografische Replikation für die Premium-SKU von ACR kann genutzt werden. [Weitere Informationen](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)
- Contoso muss den Einsatz der Web-App in der Hauptregion „USA, Osten 2“ und „USA, Mitte“ in Erwägung ziehen, wenn die Web-App für Container verfügbar wird. Die Administratoren von Contoso können Traffic Manager konfigurieren, um ein Failover bei regionalen Ausfällen sicherzustellen.
- Cosmos DB führt automatisch Sicherungen durch. Weitere Informationen zu diesem Prozess finden Sie [hier](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore).

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nachdem alle Ressourcen bereitgestellt wurden, sollte Contoso Azure-Tags basierend auf der [Infrastrukturplanung](contoso-migration-infrastructure.md#set-up-tagging) zuweisen.
- Die gesamte Lizenzierung ist in die Kosten für die PaaS-Dienste integriert, die Contoso verwendet. Dies wird über EA verrechnet.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management.

## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel hat Contoso die SmartHotel360-App in Azure umgestaltet, indem der Front-End-VM der App zu Service Fabric migriert wurde. Die App-Datenbank wurde zu einer Azure SQL-Datenbank-Instanz migriert.






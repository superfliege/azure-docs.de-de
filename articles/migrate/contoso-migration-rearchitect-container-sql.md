---
title: Umstrukturieren einer Contoso-App in einen Azure-Container und eine Azure SQL-Datenbank-Instanz| Microsoft-Dokumentation
description: Erfahren Sie, wie Contoso eine App in Azure-Windows-Container und eine Azure SQL-Datenbank-Instanz umstrukturieren.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: raynew
ms.openlocfilehash: 7146865270accb73981b09be6409180c4ef1440f
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003196"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Contoso-Migration: Umstrukturieren einer lokalen App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz

In diesem Artikel wird erläutert, wie Contoso die App SmartHotel in Azure migriert und umstrukturiert. Contoso migriert die App-Front-End-VM zu einem Azure-Windows-Container und die App-Datenbank zu einer Azure SQL-Datenbank-Instanz.

Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und die App-Datenbank mithilfe des [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und zur Always On-Verfügbarkeitsgruppe von SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Contoso verwendet Site Recovery, um die App-VMs zu migrieren, und den Database Migration Service, um die App-Datenbank zu einem SQL Servercluster zu migrieren, das durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen von Azure-VMs und Azure MySQL Server als neue Hosts für eine Linux-App](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 9: Umgestalten einer App in Azure-Web-App und Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zur Azure SQL Server-Instanz migriert. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in Azure-Web-Apps und Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App auf Azure-Web-Apps zu mehreren Websites migriert, die in GitHub für Continuous Delivery integriert sind. Das fiktive Unternehmen migriert die App-Datenbank zu einer Azure-MySQL-Instanz. | Verfügbar
Artikel 11: Umstrukturieren einer Contoso-App in einen Azure-Container und eine Azure SQL-Datenbank-Instanz | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Dieser Artikel.
[Artikel 12: Umstrukturieren einer Contoso-App in einen Azure-Container und eine Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Services, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar

In diesem Artikel migriert Contoso das zweistufige Windows. Die NET-App SmartHotel wird auf VMware-VMs in Azure ausgeführt. Falls Sie diese App verwenden möchten: Sie wird als Open Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was die Unternehmen mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und daher geraten die lokalen Systeme und Infrastrukturen unter Druck.
- **Effizienzsteigerung**: Contoso muss unnötige Verfahren entfernen und Prozesse für Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schnell sein, weder Geld noch Zeit verschwenden, und Kundenanforderungen schneller bearbeiten.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Die IT-Experten müssen schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren.  Es darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT-Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.
- **Kosten**: Contoso möchte die Lizenzierungskosten minimieren.

## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat sich folgende Ziele für die Migration gesetzt. Anhand dieser Ziele wird die beste Migrationsmethode bestimmt.

**Ziele** | **Details**
--- | --- 
**App-Anforderungen** | Die App wird in Azure so wichtig bleiben wie heute.<br/><br/> Sie sollte die gleichen Leistungsmerkmale wie aktuell in VMware aufweisen.<br/><br/> Contoso möchte Windows Server 2008 R2, unter dem die App derzeit läuft, nicht mehr unterstützen und ist bereit, in die App zu investieren.<br/><br/> Das fiktive Unternehmen möchte weg von SQL Server 2008 R2 und hin zu einer modernen PaaS-Datenbankplattform, die den Verwaltungsaufwand minimiert.<br/><br/> Seine Investitionen in die SQL Server-Lizenzierung und in Software Assurance möchte Contoso nutzen, wo immer dies möglich ist.<br/><br/> Contoso möchte in der Lage sein, die App-Webebene zentral hochzuskalieren.
**Einschränkungen** | Die App besteht aus einer ASP.NET-App und einem WCF-Dienst, die auf derselben VM ausgeführt werden. Über den Azure App Service soll dies auf zwei Web-Apps aufgeteilt werden. 
**Azure-Anforderungen** | Contoso möchte die App nach Azure verschieben und in einem Container ausführen, um die Lebensdauer der App zu verlängern. Bei der Implementierung der App in Azure soll nicht von Grund auf neu begonnen werden. 

## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert sind, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess, einschließlich der für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die lokale SmartHotel-App ist auf zwei VMs aufgeteilt (WEBVM und SQLVM).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5)
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


### <a name="proposed-architecture"></a>Vorgeschlagene Architektur

- Für die Datenbankebene der App verglich Contoso mithilfe [dieses Artikels](https://docs.microsoft.com/azure/sql-database/sql-database-features) Azure SQL-Datenbank mit SQL Server. Aus verschiedenen Gründen hat sich das Unternehmen für Azure SQL-Datenbank entschieden:
    - Azure SQL-Datenbank ist ein verwalteter Dienst für relationale Datenbanken. Er bietet eine vorhersagbare Leistung auf mehreren Serviceleveln bei nahezu keinem Administrationsaufwand. Zu den Vorteilen gehören dynamische Skalierbarkeit ohne Ausfallzeiten, integrierte intelligente Optimierung sowie globale Skalierbarkeit und Verfügbarkeit.
    - Contoso kann den einfachen Datenmigrations-Assistenten (DMA) nutzen, um die lokale Datenbank zu bewerten und zu Azure SQL-Datenbank zu migrieren.
    - Mit der Software Assurance kann das Unternehmen seine vorhandenen Lizenzen mit dem Azure-Hybridvorteil für SQL Server zu ermäßigten Preisen gegen eine SQL-Datenbank-Instanz austauschen. So sind Einsparungen von bis zu 30% möglich.
    - SQL-Datenbank bietet eine Reihe von Sicherheitsfunktionen, darunter immer verschlüsselte, dynamische Datenmaskierung und Sicherheit bzw. Bedrohungserkennung auf Zeilenebene.
- Zudem hat Contoso entschieden, die App-Webebene mit Visual Studio in den Windows-Container zu konvertieren.
    - Die App wird mit Azure Service Fabric bereitgestellt und das Windows-Containerimage aus der Azure Container Registry (ACR) gepullt.
    - Ein Prototyp für die Erweiterung der App um die Standpunktanalyse wird als weiterer Dienst in Service Fabric, verbunden mit Cosmos DB, implementiert.  Dieser liest Informationen aus Tweets und zeigt sie in der App an.

    ![Szenarioarchitektur](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Bewertung der Lösung
Contoso wertet den vorgeschlagen Entwurf durch Erstellen einer Liste mit Vor- und Nachteilen aus.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Der SmartHotel-App-Code muss für die Migration zu Azure Service Fabric geändert werden. Der Aufwand ist jedoch minimal, da die Service Fabric-SDK Tools für die Änderungen verwendet werden.<br/><br/> Mit dem Umstieg auf Service Fabric kann Contoso mit der Entwicklung von Microservices beginnen, um die App im Laufe der Zeit schnell und ohne Risiko für die ursprüngliche Codebasis zu erweitern.<br/><br/> Windows-Container bieten die gleichen Vorteile wie Container im Allgemeinen. Sie verbessern die Flexibilität, die Portabilität und die Steuerungsmöglichkeiten.<br/><br/> Das Unternehmen kann seine Investition in die Software Assurance mit dem Azure-Hybridvorteil für SQL Server und Windows Server nutzen.<br/><br/> Nach der Migration ist die Unterstützung für Windows Server 2008 R2 nicht mehr erforderlich. [Weitere Informationen](https://support.microsoft.com/lifecycle)<br/><br/> Contoso kann die Webebene der App mit mehreren Instanzen konfigurieren, sodass diese kein Single Point of Failure mehr ist.<br/><br/> Das Unternehmen ist nicht mehr auf das veraltete SQL Server 2008 R2-System angewiesen.<br/><br/> SQL-Datenbank unterstützt die technischen Anforderungen von Contoso. Das Unternehmen bewertete die lokale Datenbank mithilfe des Datenbankmigrations-Assistenten und stellte fest, dass sie kompatibel ist.<br/><br/> SQL-Datenbank verfügt über eine integrierte Fehlertoleranz, die Contoso nicht einrichten muss. Dadurch wird sichergestellt, dass die Datenebene kein Single Point of Failover mehr ist.
**Nachteile** | Container sind komplexer als andere Migrationsmöglichkeiten. Die Lernkurve bei Containern könnte für Contoso ein Problem darstellen.  Der Grad der Komplexität steigt und bietet trotz der Kurve einen hohen Nutzen.<br/><br/> Das Betriebsteam von Contoso muss eingearbeitet werden, um Azure, Container und Microservices für die App verstehen und unterstützen zu können.<br/><br/> Wenn Contoso den Datenmigrations-Assistenten anstelle des Datenmigrationsdiensts zur Migration seiner Datenbank verwendet, verfügt es nicht über die entsprechende Infrastruktur für die Migration von Datenbanken in größerem Umfang.



### <a name="migration-process"></a>Migrationsprozess

1. Contoso stellt den Azure Service Fabric-Cluster für Windows bereit.
2. Es wird eine Azure SQL-Instanz bereitgestellt, zu der die SmartHotel-Datenbank migriert wird.
3. Das Unternehmen konvertiert die VM auf Webebene mithilfe der Service Fabric-SDK Tools in einen Docker-Container.
4. Dann wird der Service Fabric-Cluster mit der ACR verbunden und die App über Azure Service Fabric bereitgestellt.

    ![Migrationsprozess](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenmigrations-Assistent (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Sie verwenden den DMA zum Bewerten und Erkennen von Kompatibilitätsproblemen, die sich auf die Funktionalität ihrer Datenbank in Azure auswirken könnten. Der DMA bewertet die Featureparität zwischen SQL-Quellen und -Zielen, und empfiehlt Verbesserungen der Leistung und Zuverlässigkeit. | Sie können das Tool kostenlos herunterladen.
[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) | Ein intelligenter, vollständig verwalteter Dienst für relationale Clouddatenbanken. | Die Kosten ergeben sich durch Features, Durchsatz und Größe. [Weitere Informationen](https://azure.microsoft.com/pricing/details/sql-database/managed/)
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Speichert Images für alle Arten von Containerbereitstellungen. | Die Kosten ergeben sich aus Features, Speicher und Nutzungsdauer. [Weitere Informationen](https://azure.microsoft.com/pricing/details/container-registry/)
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Ermöglicht das Erstellen und Betreiben von skalierbaren, verteilten Always On-Apps. | Die Kosten ergeben sich aus Größe, Standort und Dauer der Computeknoten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/service-fabric/)

## <a name="prerequisites"></a>Voraussetzungen

Für die Ausführung dieses Szenarios benötigen Sie (und Contoso) Folgendes:

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie müssten bereits ein Abonnement erstellt haben, als Sie die Beurteilung im ersten Artikel dieser Reihe durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.
**Azure-Infrastruktur** | [Weitere Informationen](contoso-migration-infrastructure.md) zur Vorgehensweise von Contoso beim Einrichten einer Azure-Infrastruktur.
**Entwicklervoraussetzungen** | Contoso benötigt die folgenden Tools auf einer Entwicklerarbeitsstation:<br/><br/> - [Visual Studio 2017 Community Edition: Version 15.5](https://www.visualstudio.com/)<br/><br/> – .NET-Workload aktiviert.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [Service Fabric-SDK-Version 3.0 oder höher](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) oder Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) festgelegt für die Verwendung von Windows-Containern.



## <a name="scenario-steps"></a>Szenarioschritte

Contoso geht bei der Ausführung der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Bereitstellen einer SQL-Datenbank-Instanz in Azure**: Contoso stellt eine SQL-Instanz in Azure bereit. Nach der Migration der Web-Front-End-VM zu einem Azure-Container wird die Containerinstanz mit dem Web-Front-End der App auf diese Datenbank zeigen.
> * **Schritt 2: Bereitstellen von Azure Service Fabric**: Contoso stellt einen Service Fabric-Cluster bereit.
> * **Schritt 4: Migrieren der Datenbank mit dem DMA**: Das Unternehmen migriert die App-Datenbank mit dem Datenmigrations-Assistenten.
> * **Schritt 5: Konvertieren der App in einen Container**: Es konvertiert die App mithilfe von Visual Studio und SDK Tools in einen Container.
> * **Schritt 6: Veröffentlichen der App**: Contoso veröffentlicht die App in der ACR und im Service Fabric-Cluster.
> * **Schritt 7: Erweitern der App**: Nach dem Veröffentlichen der App erweitert Contoso diese, um die Vorteile von Azure-Funktionen zu nutzen, und veröffentlicht sie erneut in Azure.



## <a name="step-1-provision-an-azure-sql-database"></a>Schritt 1: Bereitstellen einer Azure SQL-Datenbank-Instanz

1. Contoso entscheidet sich, eine SQL-Datenbank-Instanz in Azure zu erstellen. 

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Das Unternehmen gibt einen Datenbanknamen an, der mit der Datenbank übereinstimmt, die auf der lokalen VM (**SmartHotel.Registration**) ausgeführt wird. Die Datenbank wird dann in der ContosoRG-Ressourcengruppe platziert. Dies ist die Ressourcengruppe, die das Unternehmen für die Produktionsressourcen in Azure verwendet.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Das fiktive Unternehmen richtet eine neue SQL Server-Instanz (**sql-smarthotel-eus2**) in der primären Region ein.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Den Tarif legt das Unternehmen entsprechend seiner Server- und Datenbankanforderungen fest. Zudem entscheidet es sich, Geld mit dem Azure-Hybridvorteil zu sparen, da es bereits über eine SQL Server-Lizenz verfügen.
5. Zur Größenanpassung nutzt es das auf virtuellen Kernen basierende Kaufmodell und legt die Grenzwerte für seine erwarteten Anforderungen fest.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Dann erstellt es die Datenbankinstanz.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Nach dem Erstellen der Instanz öffnet das Unternehmen die Datenbank und notiert sich Details, die es für die Migration mit dem Datenbankmigrations-Assistenten benötigt.

    ![Bereitstellen von SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Benötigen Sie weitere Hilfe?**

- [Erhalten Sie Hilfe](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bei der Bereitstellung einer SQL-Datenbank-Instanz.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) zu Ressourcengrenzwerten des auf virtuellen Kernen basierenden Kaufmodells.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>Schritt 2: Erstellen einer ACR und Bereitstellen eines Azure-Containers

Der Azure-Container wird mithilfe der exportierten Dateien aus der Web-VM erstellt. Der Container wird in der Azure Container Registry (ACR) gespeichert.


1. Contoso erstellt eine Containerregistrierung im Azure-Portal.

     ![Containerregistrierung](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Das Unternehmen gibt einen Namen für die Registrierung (**contosoacreus2**) an und platziert sie in der primären Region, in der für seine Infrastrukturressourcen verwendeten Ressourcengruppe. Dann aktiviert es den Zugriff für Administratorbenutzer, und es legt sie als Premium-SKU fest, um die Georeplikation nutzen zu können.

    ![Containerregistrierung](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>Schritt 3: Bereitstellen von Azure Service Fabric

Der SmartHotel-Container wird im Azure Service Fabric-Cluster ausgeführt. Der Service Fabric-Cluster wird von Contoso wie folgt erstellt:

1. Erstellen einer Service Fabric-Ressource aus dem Azure Marketplace

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. In **Basic** gibt Contoso einen eindeutigen DS-Namen für den Cluster sowie Anmeldeinformationen für den Zugriff auf die lokale VM an. Es platziert die Ressource in der Produktionsressourcengruppe (**ContosoRG**) in der primären Region „USA, Osten 2“.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. In **Knotentypkonfiguration** gibt das Unternehmen einen Knotentypnamen, Einstellungen zur Dauerhaftigkeit, VM-Größe und App-Endpunkte ein.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. In **Schlüsseltresor erstellen** wird ein neuer Schlüsseltresor in der Infrastrukturressourcengruppe erstellt, in der das Zertifikat gespeichert wird.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. In **Zugriffsrichtlinien** aktiviert das Unternehmen den Zugriff auf virtuelle Computer zum Bereitstellen des Schlüsseltresors.

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


## <a name="step-3-migrate-the-database-with-dma"></a>Schritt 3: Migrieren der Datenbank mit dem DMA

Contoso wird die SmartHotel-Datenbank mit dem DMA migrieren.

### <a name="install-dma"></a>Installieren des DMA

1. Es lädt das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) auf die lokale SQL Server-VM (**SQLVM**) herunter.
2. Es führt das Setup (DownloadMigrationAssistant.msi) auf der VM aus.
3. Auf der Seite **Fertigstellen** wählen sie **Launch Microsoft Data Migration Assistant** (Microsoft-Datenmigrations-Assistenten starten) aus, bevor sie den Assistenten beenden.

### <a name="configure-the-firewall"></a>Konfigurieren der Firewall

Zum Verbinden mit der Azure SQL-Datenbank-Instanz wird eine Firewallregel benötigt.

1. In den Eigenschaften für **Firewall und virtuelle Netzwerke** für die Datenbank gewährt Contoso Zugriff auf Azure-Dienste, und es fügt eine Regel für die Client-IP-Adresse der lokalen SQL Server-VM hinzu.
2. Es wird eine Firewallregel auf Serverebene erstellt.

    ![Firewall](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Benötigen Sie weitere Hilfe?

[Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#creating-and-managing-firewall-rules) über das Erstellen und Verwalten von Firewallregeln für Azure SQL-Datenbank.

### <a name="migrate"></a>Migrieren

1. Im DMA erstellt das Unternehmen ein neues Projekt (**SmartHotelDB**) und wählt **Migration** aus. 
2. Der Quellservertyp wird als **SQL Server** und das Ziel als **Azure SQL-Datenbank** ausgewählt. 

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

9. Nach Abschluss der Migration kann Contoso überprüfen, ob die Datenbank auf der Azure SQL-Instanz ausgeführt wird.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. Die zusätzliche SQL-Datenbank-Instanz **SmartHotel.Registration** wird von Contoso im Azure-Portal gelöscht.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)



## <a name="step-4-convert-the-app-to-a-container"></a>Schritt 4: Konvertieren der App in einen Container

Bei der lokalen App handelt es sich um eine herkömmliche dreischichtige App:

- Sie enthält WebForms und einen WCF-Dienst zur Verbindung mit SQL Server.
- Sie verwendet Entity Framework zur Integration mit den Daten in der SQL-Datenbank-Instanz, die über einen WCF-Dienst verfügbar gemacht wird.
- Die WebForms-Anwendung interagiert mit dem WCF-Dienst.

Contoso wird die App mit Visual Studio und den SDK Tools wie folgt in einen Container konvertieren:

1. Das Repository wird lokal auf einen Entwicklungscomputer geklont:

    **git clone https://github.com/Microsoft/SmartHotel360-internal-booking-apps.git**

    ![Container](./media/contoso-migration-rearchitect-container-sql/container1.png)

2. Mit Visual Studio öffnet Contoso die Projektmappendatei (SmartHotel.Registration.sln) im Verzeichnis **SmartHotel360-interne-Buchung-Apps\src\Registration** des lokalen Repositorys.  Es werden zwei Apps angezeigt. Die Web-Front-End-App SmartHotel.Registration.Web und die WCF-Dienst-App SmartHotel.Registration.WCF.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container2.png)


3. Contoso klickt mit der rechten Maustaste auf die Web-App > **Hinzufügen** > **Unterstützung für Containerorchestrator**.
4. In **Unterstützung für Containerorchestrator hinzufügen** wird **Service Fabric** ausgewählt.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container3.png)

5. Contoso wiederholt den Vorgang für die SmartHotel.Registration.WCF-App.
6. Jetzt überprüft Contoso, wie sich die Lösung geändert hat.

    - Die neue App heißt nun **SmartHotel.RegistrationApplication/**.
    - Sie umfasst zwei Dienste: **SmartHotel.Registration.WCF** und **SmartHotel.Registration.Web**.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container4.png)

7. Die Docker-Datei wurde von Visual Studio erstellt, und die erforderlichen Bilder wurden lokal auf den Entwicklercomputer gezogen.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container5.png)

8. Es wird eine Manifestdatei (**ServiceManifest.xml**) von Visual Studio erstellt und geöffnet. Diese Datei informiert Service Fabric darüber, wie der Container bei der Bereitstellung in Azure konfiguriert werden soll.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container6.png)

9. Eine weitere Manifestdatei (**ApplicationManifest.xml) enthält die Konfigurationsanwendungen für die Container.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container7.png)

## <a name="step-5-publish-the-app"></a>Schritt 5: Veröffentlichen der App


Schließlich veröffentlicht Contoso die App in der ACR und im Service Fabric-Cluster.

> [!NOTE]
> In der SmartHotel-App wurden einige mit dem Service Fabric-Cluster verbundene Änderungen vorgenommen. Sowohl der ursprüngliche als auch der modernisierte App-Code kann von [GitHub](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) heruntergeladen werden. Die geänderte Datei heißt **AppliationModern/ApplicationParameters/Cloud.xml**.


1. In Visual Studio wird die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank-Instanz aktualisiert. Die Verbindungszeichenfolge steht in der Datenbank im Azure-Portal zur Verfügung.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish1.png)

2. Contoso veröffentlicht die App mit Visual Studio in Service Fabric. Hierzu klickt es mit der rechten Maustaste auf die Service Fabric-Anwendung > **Veröffentlichen**.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish2.png)

3. Das Unternehmen wählt das Abonnement, den Verbindungsendpunkt und die ACR aus. Dann wird auf **Veröffentlichen** geklickt.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish3.png)

4. Nach Abschluss der Bereitstellung wird von SmartHotel jetzt Service Fabric ausgeführt.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish4.png)

5. Um eine Verbindung mit der App herzustellen, leitet Contoso den Datenverkehr an die öffentliche IP-Adresse des Azure-Lastenausgleichs in Form von deren Service Fabric-Knoten weiter.

    ![Veröffentlichen](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-6-extend-the-app-and-republish"></a>Schritt 6: Erweitern der App und erneutes Veröffentlichen

Nachdem nun die SmartHotel-App und die Datenbank in Azure laufen, möchte Contoso die App erweitern.

- Die Contoso-Entwickler erstellen gerade den Prototypen einer neuen .NET Core-Anwendung, die auf dem Service Fabric-Cluster ausgeführt werden soll.
- Die App wird dann zum Pullen von Standpuntkdaten aus Cosmos DB verwendet.
- Diese Daten werden in Form von Tweets vorliegen, die mit einer serverlosen Azure-Funktion und der Cognitive Services-Textanalyse-API verarbeitet werden.

### <a name="provision-azure-cosmos-db"></a>Bereitstellen von Azure Cosmos DB

Im ersten Schritt erstellt Contoso eine Azure Cosmos-Datenbank.

1. Dabei wird eine Azure Cosmos DB-Ressource aus dem Azure Marketplace erstellt.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Contoso gibt einen Datenbanknamen (**contosmarthotel**) an, wählt die SQL-API aus und platziert die Ressource in der Produktionsressourcengruppe in der primären Region „USA, Osten 2“.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. In **Erste Schritte** wählt das Unternehmen **Daten-Explorer** aus und fügt eine neue Sammlung hinzu.
4. In **Sammlung hinzufügen** gibt esIDs an und legt die Speicherkapazität und den Durchsatz fest.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. Im Portal öffnet Contoso die neue Datenbank > **Sammlung** > **Dokumente** und klickt auf **Neues Dokument**.
6. Der folgende JSON-Code wird in das Dokumentfenster eingefügt. Dies sind Beispieldaten in Form eines einzelnen Tweets.

    ```
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel",
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

Nach der Bereitstellung einer Cosmos DB-Instanz kann Contoso die App so konfigurieren, dass sie sich mit dieser verbindet.

1. In Visual Studio öffnet das Unternehmen im Projektmappen-Explorer die Datei ApplicationModern\ApplicationParameters\cloud.xml.

    ![Standpunkt-App](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Die folgenden beiden Parameter werden angegeben:

   ```
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Standpunkt-App](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Erneutes Veröffentlichen der App

Nach dem Erweitern der App veröffentlicht Contoso diese erneut in Azure.

1. Contoso klickt im Portal mit der rechten Maustaste auf die Service Fabric-App > **Veröffentlichen**.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish1.png)

2. Das Unternehmen wählt das Abonnement, den Verbindungsendpunkt und die ACR aus. Dann wird auf **Veröffentlichen** geklickt.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish2.png)

4. Nach Abschluss der Bereitstellung wird von SmartHotel jetzt Service Fabric ausgeführt. Die drei Dienste werden nun in der Service Fabric-Verwaltungskonsole angezeigt.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish3.png)

5. Contoso kann die Dienste durchsuchen, um festzustellen, ob die SentimentIntegration-App aktiv ist und ausgeführt wird.

    ![Erneut veröffentlichen](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach der Migration muss Contoso die folgenden Schritte zur Bereinigung ausführen:  

- Entfernen der lokalen VMs aus dem vCenter-Bestand.
- Entfernen der VMs aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation zur Anzeige der neuen Speicherorte für die SmartHotel-App. Anzeigen, dass die Datenbank in Azure SQL-Datenbank und das Front-End in Service Fabric ausgeführt wird.
- Überprüfen sämtlicher Ressourcen, die mit den außer Betrieb genommenen VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.


## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

- Contoso muss gewährleisten, dass seine neue Datenbank **SmartHotel-Registration** sicher ist. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)
- Insbesondere sollte es den Container aktualisieren, sodass dieser SSL mit Zertifikaten verwendet.
- Zudem sollte die Verwendung von Key Vault zum Schutz von Geheimnissen für die Service Fabric-Apps berücksichtigt werden. [Weitere Informationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management)

### <a name="backups"></a>Backups

- Contoso muss die Sicherungsanforderungen für Azure SQL-Datenbank überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)
- Das Unternehmen sollte die Implementierung von Failovergruppen berücksichtigen, um ein regionales Failover für die Datenbank bereitzustellen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)
- Contoso kann die geografische Replikation für die Premium-SKU von ACR nutzen. [Weitere Informationen](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)
- Contoso muss den Einsatz der Web-App in der Hauptregion „USA, Osten 2“ und „USA, Mitte“ in Erwägung ziehen, wenn die Web-App für Container verfügbar wird. Das Unternehmen könnte den Traffic Manager konfigurieren, um ein Failover bei regionalen Ausfällen sicherzustellen.

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nachdem alle Ressourcen bereitgestellt wurden, sollte Contoso Azure-Tags basierend auf seiner [Infrastrukturplanung](contoso-migration-infrastructure.md#set-up-tagging) zuweisen.
- Die gesamte Lizenzierung ist in die Kosten für die PaaS-Dienste integriert, die Contoso verwendet. Dies wird über EA verrechnet.
1. Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 

## <a name="conclusion"></a>Zusammenfassung

Im vorliegenden Artikel hat Contoso der SmartHotel-App in Azure einen neuen Host zugewiesen, indem das Unternehmen die Front-End-VM der App mithilfe des Site Recovery-Diensts zu Azure migriert hat. Das Unternehmen migriert die App-Datenbank zu einer Azure SQL-Datenbank-Instanz.






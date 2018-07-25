---
title: Umgestalten einer Service Desk-App für Contoso unter Linux zu Azure App Service und Azure MySQL | Microsoft-Dokumentation
description: Erfahren Sie, wie Contoso eine lokale Linux-App umgestaltet, indem sie zu Azure App Service mit GitHub für die Webebene und Azure SQL-Datenbank migriert wird.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 5981c708abdaa12a662075cc5bf5aae14ccc35c2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002159"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-the-azure-app-service-and-azure-mysql"></a>Contoso-Migration: Umgestalten einer Service Desk-App für Contoso unter Linux zu Azure App Service und Azure MySQL

In diesem Artikel wird gezeigt, wie Contoso die lokale Linux-Service Desk-App (osTicket) auf zwei Ebenen umgestaltet, indem sie zu Azure App Service mit GitHub-Integration und Azure MySQL migriert wird.

Dieses Dokument ist das zehnte in einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird, und wie verschiedene Migrationstypen ausgeführt werden. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Contoso-Migrationsszenarios wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen SmartHotel-App, die unter VMware ausgeführt wird, durchführt. Sie bewerten App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Datenbankmigrations-Assistent von Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso die App SmartHotel zu Azure migriert. Sie migrieren die App-Web-VM mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und verwenden die App-Datenbank und den Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview), um zu einer verwalteten SQL-Instanz zu migrieren. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die SmartHotel-App nur mit Site Recovery zu Azure IaaS-VMs migriert.
[Artikel 6: Zuweisen eines neuen Hosts zu Azure-VMs und SQL Server-Verfügbarkeitsgruppen](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Das Unternehmen verwendet Site Recovery zum Migrieren der App-VMs und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-Verfügbarkeitsgruppe. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Zeigt, wie Contoso die osTicket-Linux-App mithilfe von Azure Site Recovery zu Azure IaaS-VMs migriert.
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die osTicket-Linux-App migriert. Das Unternehmen verwendet Site Recovery für die VM-Migration und MySQL-Workbench, um zu einer Azure MySQL Server-Instanz zu migrieren. | Verfügbar
[Artikel 9: Umgestalten einer App in Azure-Web-Apps und Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer containerbasierten Azure-Web-App und die App-Datenbank zur Azure SQL Server-Instanz migriert. | Verfügbar
Artikel 10: Umgestalten einer Linux-App in Azure-Web-Apps und Azure MySQL Server | Zeigt, wie Contoso die Linux-App osTicket in Azure App Service mit einem PHP 7.0-Docker-Container migriert. Die Codebasis für die Bereitstellung wird zu GitHub migriert. Die App-Datenbank wird zu Azure MySQL migriert. | Dieser Artikel.
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer Contoso-App in einen Azure-Container und eine Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure überarbeitet. Es wandelt den Tarif für die Web-App in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Services, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar

In diesem Artikel migriert Contoso eine Service Desk-App (osTicket) auf zwei Ebenen von Linux Apache MySQL PHP (LAMP) zu Azure. Wenn Sie diese Open Source-App verwenden möchten, können Sie sie von [GitHub](https://github.com/osTicket/osTicket) herunterladen.



## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit seinen Geschäftspartnern zusammengearbeitet, um zu verstehen, was sie erreichen möchten:

- **Handhabung des Unternehmenswachstums**: Contoso wächst und erschließt neue Märkte. Das Unternehmen benötigt zusätzliche Kundendienstmitarbeiter. 
- **Skalierung**: Die Lösung erstellt werden soll, sodass Contoso beim Ausbau der Geschäftsbereiche weitere Kundendienstmitarbeiter einstellen kann.
- **Verbesserte Resilienz**: In der Vergangenheit waren nur interne Benutzer von Problemen mit dem System betroffen. Beim neuen Geschäftsmodell wirken sich diese auch auf externe Benutzer aus, aber die App muss jederzeit verfügbar sein.

## <a name="migration-goals"></a>Migrationsziele

Für die Bestimmung der besten Migrationsmethode hat das Contoso-Cloudteam Ziele für diese Migration festgelegt:

- Eine Skalierung der Anwendung sollte über die aktuelle lokale Kapazität und Leistung hinaus möglich sein.  Contoso verschiebt die Anwendung, um die Vorteile der bedarfsgesteuerten Skalierung von Azure zu nutzen.
- Contoso möchte die App-Codebasis in eine Continuous Delivery-Pipeline verschieben.  Da App-Änderungen auf GitHub übertragen werden, möchte das Unternehmen diese Änderungen bereitstellen, ohne das zusätzliche Aufgaben für das Betriebspersonal anfallen.
- Die Anwendung muss widerstandsfähig sein und über Wachstums- und Failovermöglichkeiten verfügen. Die App soll in zwei verschiedenen Azure-Regionen eingesetzt und automatisch skaliert werden.
- Nachdem die App in die Cloud verschoben wurde, soll der Aufwand für die Datenbankadministration auf ein Minimum reduziert werden.

## <a name="solution-design"></a>Lösungsentwurf
Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess, einschließlich der für die Migration genutzten Azure-Dienste.


## <a name="current-architecture"></a>Aktuelle Architektur

- Die App wird über zwei VM-Schichten (OSTICKETWEB und OSTICKETMYSQL) zur Verfügung gestellt.
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).

![Aktuelle Architektur](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Nachdem die aktuelle Architektur, Ziele und Migrationsanforderungen formuliert wurden, entwirft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess, einschließlich der für die Migration genutzten Azure-Dienste.

- Die App auf Webebene auf OSTICKETWEB wird durch die Einrichtung von Azure App Service in zwei Azure Regionen migriert. Azure App Service für Linux wird mit dem PHP 7.0-Docker-Container implementiert.
- Der App-Code wird nach GitHub verschoben, und es wird eine Azure-Web-App für Continuous Delivery mit GitHub konfiguriert.
- Azure-App-Server werden sowohl in der primären (USA, Osten 2) als auch in der sekundären (USA, Mitte) Region eingesetzt.
- Traffic Manager wird vor den beiden Azure-Web-Apps in beiden Regionen eingerichtet.
- Traffic Manager wird im Prioritätsmodus konfiguriert, um den Datenverkehr über „USA, Osten 2“ zu erzwingen.
- Wenn der Azure-App-Server in „USA, Osten 2“ offline geht, können Benutzer auf die App, für die ein Failover ausgeführt wurde, in „USA, Mitte“ zugreifen.
- Die App-Datenbank wird mit den MySQL Workbench-Tools zum Azure MySQL-PaaS-Dienst migriert. Die lokale Datenbank wird lokal gesichert und direkt in Azure MySQL wiederhergestellt.
- Die Datenbank befindet sich in der primären Region „USA, Osten 2“, im Datenbanksubnetz (PROD-DB-EUS2) im Produktionsnetzwerk (VNET-PROD-EUS2):
- Da es sich um die Migration einer Produktionsworkload handelt, befinden sich die Azure-Ressourcen für die App in der Ressourcengruppe für die Produktion **ContosoRG**.
- Die Traffic Manager-Ressource wird von Contoso in der Infrastrukturressourcengruppe **ContosoInfraRG** bereitgestellt.
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


![Szenarioarchitektur](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png) 


## <a name="migration-process"></a>Migrationsprozess

Contoso wird den Migrationsprozess wie folgt abschließen:

1. In einem ersten Schritt richtet Contoso die Azure-Infrastruktur ein, einschließlich der Bereitstellung von Azure App Service-Instanzen, der Einrichtung von Traffic Manager und der Bereitstellung einer Azure MySQL-Instanz.
2. Nach der Vorbereitung von Azure migriert das Unternehmen die Datenbank mithilfe von MySQL Workbench. 
3. Sobald die Datenbank in Azure ausgeführt wird, richtet Contoso ein privates GitHub-Repository für Azure App Service mit Continuous Delivery ein und lädt es mit der osTicket-App.
4. Im Azure-Portal wird die App von GitHub mit Azure App Service in den Docker-Container geladen. 
5. Contoso optimiert die DNS-Einstellungen und konfiguriert die automatische Skalierung für die App.

![Migrationsprozess](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Azure App Service](https://azure.microsoft.com/services/app-service/) | Der Dienst wird ausgeführt und skaliert Anwendungen mit dem Azure-PaaS-Dienst für Websites.  | Der Preis richtet sich nach der Größe der Instanzen und den benötigten Features. [Weitere Informationen](https://azure.microsoft.com/pricing/details/app-service/windows/)
[Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | Ein Lastenausgleichsmodul, das DNS verwendet, um Benutzer zu Azure oder externen Websites und Diensten zu leiten. | Der Preis richtet sich nach der Anzahl der empfangenen DNS-Abfragen und der Anzahl der überwachten Endpunkte. | [Weitere Informationen](https://azure.microsoft.com/pricing/details/traffic-manager/)
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | Die Datenbank basiert auf der Open Source-MySQL-Server-Engine. Sie stellt eine vollständig verwaltete, unternehmensgerechte MySQL Community-Edition als Database-as-a-Service für die Entwicklung und Bereitstellung von Apps bereit. | Der Preis richtet sich nach den Compute-, Speicher- und Sicherungsanforderungen. [Weitere Informationen](https://azure.microsoft.com/pricing/details/mysql/)

 
## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie (und Contoso) dieses Szenario ausführen möchten, sind hier die entsprechenden Anforderungen aufgeführt.

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie sollten bereits im Zuge früherer Artikel dieser Reihe ein Abonnement erstellt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-Infrastruktur** | Contoso richtet die Azure-Infrastruktur ein, wie in [Azure infrastructure for migration (Azure-Infrastruktur für die Migration)](contoso-migration-infrastructure.md) beschrieben.



## <a name="scenario-steps"></a>Szenarioschritte

Azure geht bei der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Bereitstellen von Azure App Service**: Contoso wird Web-Apps in den primären und sekundären Regionen bereitstellen.
> * **Schritt 2: Einrichten von Traffic Manager**: Das Unternehmen richtet Traffic Manager vorgelagert vor den Web-Apps ein, um den Datenverkehr zu routen und auszugleichen.
> * **Schritt 3: Bereitstellen von MySQL**: Contoso stellt in Azure eine Azure MySQL-Datenbank-Instanz bereit.
> * **Schritt 4: Migrieren der Datenbank**: Die Datenbank wird mit MySQL Workbench migriert. 
> * **Schritt 5: Einrichten von GitHub**: Contoso richtet ein lokales GitHub-Repository für die App-Websites bzw. den App-Code ein.
> * **Schritt 6: Bereitstellen der Web-Apps**: Contoso setzt die Web-Apps von GitHub ein.




## <a name="step-1-provision-azure-app-services"></a>Schritt 1: Bereitstellen von Azure App Service

Contoso stellt zwei Web-Apps (eine in jeder Region) mithilfe von Azure App Service zur Verfügung.

1. Das Unternehmen erstellt eine Web-App-Ressource in der primären Region „USA, Osten 2“ (**osticket-eus2**) aus dem Azure Marketplace.
2. Es platziert die Ressource in die Produktionsressourcengruppe **ContosoRG**.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. In der primären Region (**APP-SVP-EUS2**) erstellt es einen neuen App Service-Plan unter Verwendung der Standardgröße.

     ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Contoso wählt ein Linux-Betriebssystem mit PHP 7.0-Runtimestapel, bei dem es sich um einen Docker-Container handelt.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Es erstellt eine zweite Web-App (**osticket-cus**) und einen App Service-Plan für die Region „USA, Mitte“.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Benötigen Sie weitere Hilfe?**

- Erfahren Sie mehr über [Azure App Service-Web-Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview).
- Erfahren Sie mehr über [Azure App Service unter Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>Schritt 2: Einrichten von Traffic Manager

Contoso richtet Traffic Manager ein, um eingehende Webanforderungen an Web-Apps zu leiten, die auf der osTicket-Webebene ausgeführt werden.

1. Das fiktive Unternehmen Contoso erstellt eine Traffic Manager-Ressource (**osticket.trafficmanager.net**) aus dem Azure Marketplace. Es nutzt das prioritätsbasierte Routing, sodass „USA, Osten 2“ der primäre Standort ist. Es platziert die Ressource in seiner Infrastrukturressourcengruppe (**ContosoInfraRG**). Beachten Sie, dass Traffic Manager global und nicht an einen bestimmten Standort gebunden ist.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Jetzt konfiguriert Contoso Traffic Manager mit Endpunkten. Die Web-App „USA, Osten 2“ wird als primärer Standort (**osticket-eus2**) und die App „USA, Mitte“ als sekundärer Standort (**osticket-cus**) hinzugefügt.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. Nach dem Hinzufügen der Endpunkte können diese überwacht werden.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Benötigen Sie weitere Hilfe?**

- Erfahren Sie mehr über [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Erfahren Sie mehr über das [Routing von Datenverkehr zu einem prioritätsbasierten Endpunkt](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>Schritt 3: Bereitstellen von Azure Database for MySQL

Contoso stellt eine MySQL-Datenbankinstanz in der primären Region für die USA, Osten 2, zur Verfügung.

1. Im Azure-Portal erstellen die Verantwortlichen von Contoso eine Azure Database for MySQL-Ressource. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Sie fügen den Namen **contosoosticket** für die Azure-Datenbank hinzu. Sie fügen die Datenbank zur Ressourcengruppe für die Produktion **ContosoRG** hinzu, und geben Anmeldeinformationen für sie an.
3. Die lokale MySQL-Datenbank ist in Version 5.7 vorhanden. Deshalb wählen sie diese Version für Kompatibilitätszwecke. Sie verwenden die Standardgrößen, die ihren Datenbankanforderungen entsprechen.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. Für **Optionen für Sicherungsredundanz** wählt Contoso die Verwendung von **Georedundanz** aus. Mit dieser Option können sie ihre Datenbank in der sekundären Region „USA Mitte“ wiederherstellen, wenn ein Ausfall auftritt. Sie können diese Option nur konfigurieren, wenn sie die Datenbank zur Verfügung stellen.

    ![Redundanz](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Contoso richtet die Verbindungssicherheit ein. In der Datenbank > **Verbindungssicherheit** richtet das Unternehmen Firewallregeln ein, um der Datenbank den Zugriff auf Azure-Dienste zu ermöglichen.
5. Sie fügen die Client-IP-Adresse der lokalen Arbeitsstation zu den Start- und End-IP-Adressen hinzu. Dadurch können die Web-Apps zusammen mit dem Datenbankclient, der die Migration durchführt, auf die MySQL-Datenbank zugreifen.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>Schritt 4: Migrieren der Datenbank

Contoso migriert die Datenbank durch Sicherung und Wiederherstellung mit MySQL-Tools. Das Unternehmen installiert MySQL Workbench, speichert die Datenbank aus OSTICKETMYSQL und stellt sie dann wieder in der Azure Database for MySQL Server her.

### <a name="install-mysql-workbench"></a>Installieren von MySQL Workbench

1. Contoso überprüft die [Voraussetzungen und lädt MySQL Workbench herunter](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Die Verantwortlichen bei Contoso installieren MySQL Workbench for Windows entsprechend den [Installationsanweisungen](https://dev.mysql.com/doc/workbench/en/wb-installing.html). Der für die Installation verwendete Computer muss für die OSTICKETMYSQL-VM und Azure über das Internet zugänglich sein.
3. In MySQL Workbench erstellen sie eine MySQL-Verbindung mit OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Sie exportieren die Datenbank als **Osticket** in eine lokale, eigenständige Datei.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. Nachdem die Datenbank lokal gesichert wurde, erstellen sie eine Verbindung mit der Azure Database for MySQL-Instanz.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Jetzt kann Contoso die Datenbank aus der eigenständige Datei in die Azure MySQL-Instanz importieren (wiederherstellen). Ein neues Schema (Osticket) wird für die Instanz erstellt.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. Nach dem Wiederherstellen der Daten können diese mit Workbench abgefragt und im Azure-Portal angezeigt werden.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Abschließend muss Contoso die Datenbankinformationen in den Web-Apps aktualisieren. In der MySQL-Instanz wird **Verbindungszeichenfolge** geöffnet. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. In der Liste der Zeichenfolgen sucht Contoso die Web-App-Einstellungen aus und klickt darauf, um diese zu kopieren.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Contoso öffnet ein Editor-Fenster und fügt die Zeichenfolge in eine neue Datei ein. Dann aktualisiert es diese, damit sie mit der osticket-Datenbank, MySQL-Instanz und Anmeldeinformationseinstellungen übereinstimmt.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Im Azure-Portal kann Contoso in der **Übersicht** der MySQL-Instanz den Servernamen und die zugehörigen Anmeldeinformationen überprüfen.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>Schritt 5: Einrichten von GitHub

Contoso erstellt ein neues privates GitHub-Repository und stellt eine Verbindung zur osTicket-Datenbank in Azure MySQL her. Dann lädt es die Azure-Web-App mit der App.  

1.  Das Unternehmen durchsucht das öffentliche GitHub-Repository der osTicket-Software und forkt es an das GitHub-Konto von Contoso.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. Nach dem Forken navigiert es zum **include**-Ordner und findet die Datei **ost-config.php**.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. Die Datei wird im Browser geöffnet und bearbeitet.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. Im Editor aktualisiert Contoso die Datenbankdetails, insbesondere **DBHOST** und **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Dann führt Contoso für die Änderungen einen Commit aus.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Für jede Web-App (**osticket-eus2** und **osticket-cus**) ändert Contoso die **Anwendungseinstellungen** im Azure-Portal.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Contoso gibt die Verbindungszeichenfolge mit dem Namen **osticket** ein und kopiert die Zeichenfolge vom Editor in den **Wertebereich**. Contoso wählt in der Dropdownliste neben der Zeichenfolge **MySQL** aus und speichert die Einstellungen.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>Schritt 6: Konfigurieren der Web-Apps

Als letzten Schritt im Migrationsprozess konfiguriert Contoso die Web-Apps mit den osTicket-Websites.



1. In der primären Web-App (**osticket-eus2**) wird **Bereitstellungsoption** geöffnet und die Quelle auf **GitHub** festgelegt.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Contoso wählt die Bereitstellungsoptionen aus.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. Nach dem Festlegen der Optionen wird die Konfiguration im Azure-Portal als „Ausstehend“ angezeigt.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. Nachdem die Konfiguration aktualisiert wurde und die osTicket-Web-App von GitHub in den Docker-Container mit Azure App Service geladen wurde, erscheint die Website als „Aktiv“.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Contoso wiederholt dann die obigen Schritte für die sekundäre Web-App (**osticket-cus**).
6. Nachdem die Site konfiguriert wurde, ist sie über das Traffic Manager-Profil erreichbar. Der DNS-Name ist der neue Speicherort der osTicket-App. [Weitere Informationen](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record)

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Contoso wünscht einen DNS-Namen, der leicht zu merken ist. Das Unternehmen erstellt einen Aliasdatensatz (CNAME) **osticket.contoso.com**, der auf den Traffic Manager-Namen im DNS seiner Domänencontroller zeigt.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Es konfiguriert sowohl die Web-App **osticket-eus2** als auch **osticket-cus**, um die benutzerdefinierten Hostnamen zuzulassen.

    ![Konfigurieren der App](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Einrichten der automatischen Skalierung

Schließlich richtet Contoso eine automatische Skalierung für die App ein. Dies stellt sicher, dass bei der Verwendung der Apps durch Agents die App-Instanzen entsprechend den Geschäftsanforderungen erweitert oder verkleinert werden. 

1. In App Service **APP-SRV-EUS2** öffnet Contoso **Skalierungseinheit**.
2. Das Unternehmen konfiguriert eine neue Einstellung für die automatische Skalierung mit einer einzigen Regel, die die Anzahl der Instanzen um eins erhöht, wenn der CPU-Prozentsatz für die aktuelle Instanz zehn Minuten lang über 70% liegt.

    ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Contoso konfiguriert die gleiche Einstellung für **APP-SRV-CUS**, um sicherzustellen, dass das gleiche Verhalten gilt, wenn ein Failover der App auf die sekundäre Region ausgeführt wird. Der einzige Unterschied ist, dass der Grenzwert für die Instanz auf 1 gesetzt wird, da dies nur für Failover gilt.

   ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nachdem die Migration abgeschlossen ist, wird die osTicket-App in eine Azure-Web-App mit Continuous Delivery mit einem privaten GitHub-Repository umgestaltet. Zur Verbesserung der Resilienz wird die App in zwei Regionen ausgeführt. Die osTicket-Datenbank wird nach der Migration zur PaaS-Plattform in der Azure-Datenbank für MySQL ausgeführt.

Contoso muss nun wie folgt vorgehen: 
- Das Unternehmen entfernt die VMware-VMs aus dem vCenter-Bestand.
- Es entfernt die lokalen VMs aus lokalen Sicherungsaufträgen.
- Es aktualisiert die interne Dokumentation und zeigt neue Speicherorte und IP-Adressen an. 
- Das Unternehmen überprüft sämtliche Ressourcen, die mit den lokalen VMs interagieren, und aktualisiert sämtliche relevanten Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.
- Es konfiguriert die Überwachung so, dass sie auf die URL von osticket-trafficmanager.net zeigt, um nachzuverfolgen, ob die App ausgeführt wird.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die App jetzt ausgeführt wird, muss Contoso seine neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüfte die App auf eventuell vorhandene Sicherheitsprobleme. Es wurde festgestellt, dass die Kommunikation zwischen der osTicket-App und der MySQL-Datenbank-Instanz nicht für SSL konfiguriert ist. Das Team muss dies tun, um sicherzustellen, dass der Datenbankverkehr nicht gehackt werden kann. [Weitere Informationen](https://docs.microsoft.com/azure/mysql/howto-configure-ssl)

### <a name="backups"></a>Backups

- Die osTicket-App enthält keine Zustandsdaten und muss daher auch nicht gesichert werden.
- Das Unternehmen muss die Sicherung für die Datenbank nicht konfigurieren. Azure Database for MySQL erstellt automatisch Serversicherungen und -speicher. Es hat sich für die Georedundanz für die Datenbank entschieden, damit sie robust und einsatzbereit ist. Sicherungen können verwendet werden, um für Ihren Server den Stand zu einem bestimmten Zeitpunkt wiederherzustellen. [Weitere Informationen](https://docs.microsoft.com/azure/mysql/concepts-backup)


### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Es gibt keine Lizenzierungsprobleme für die PaaS-Bereitstellung.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management.




---
title: Umgestalten einer Contoso-App durch Migration zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen darüber, wie Contoso einer lokalen App einen neuen Host zuweist, indem diese zu einer Azure Container-Web-App und einer Azure SQL Server-Datenbank migriert wird.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: f4a348815ef058cb795ed12e8f118b494650a555
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005189"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Contoso-Migration: Umgestalten einer lokalen App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz

In diesem Artikel wird erläutert, wie Contoso die App SmartHotel in Azure umgestaltet. Das Unternehmen migriert die App-Front-End-VM zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL-Datenbank-Instanz.

Dieses Dokument stammt aus einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und Szenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten und verschiedene Migrationstypen durchführen. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und die App-Datenbank mithilfe des [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App zu Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Contoso verwendet Site Recovery, um die App-VMs zu migrieren, und den Database Migration Service, um die App-Datenbank zu einem SQL Servercluster zu migrieren, das durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen von Azure-VMs und Azure MySQL Server als neue Hosts für eine Linux-App](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
Artikel 9: Umgestalten einer App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz migriert. | Dieser Artikel
[Artikel 10: Umgestalten einer Linux-App zu einer Azure-Web-App und einer Azure Database for MySQL-Instanz](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App zu Azure-Web-Apps auf mehreren Websites migriert, die in GitHub für Continuous Delivery integriert sind. Es migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Service, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar


In diesem Artikel migriert Contoso die zweischichtige Windows-App. Die NET-App SmartHotel wird auf VMware-VMs in Azure ausgeführt. Falls Sie diese App verwenden möchten: Sie wird als Open Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was die Unternehmen mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst, und das verlangt den lokalen Systeme und Infrastrukturen vieles ab.
- **Effizienzsteigerung**: Contoso muss unnötige Verfahren entfernen und Prozesse für Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schnell sein, weder Geld noch Zeit verschwenden, und Kundenanforderungen schneller bearbeiten.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Die IT-Experten müssen schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren.  Es darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT-Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.
- **Kosten**: Contoso möchte die Lizenzierungskosten minimieren.

## <a name="migration-goals"></a>Migrationsziele

Das Cloudteam von Contoso hat sich folgende Ziele für die Migration gesetzt. Anhand dieser Ziele wird die beste Migrationsmethode bestimmt.

**Anforderungen** | **Details**
--- | --- 
**App** | Die App wird auch in Zukunft eine wichtige Rolle in Azure spielen, wie es heute schon der Fall ist.<br/><br/> Sie sollte die gleichen Leistungsmerkmale wie aktuell in VMware aufweisen.<br/><br/> Das Unternehmen möchte nicht in die App investieren. Vorerst möchte Contoso lediglich einfach eine sichere Migration in die Cloud durchführen.<br/><br/> Die Unterstützung für Windows Server 2008 R2, unter dem die App derzeit läuft, soll eingestellt werden.<br/><br/> Das Unternehmen möchte weg von SQL Server 2008 R2 und hin zu einer modernen PaaS-Datenbankplattform, die den Verwaltungsaufwand minimiert.<br/><br/> Seine Investitionen in die SQL Server-Lizenzierung und in die Software Assurance möchte Contoso nutzen, wo immer dies möglich ist.<br/><br/> Darüber hinaus möchte Contoso die Auswirkungen des Single Point of Failure in der Webebene abschwächen.
**Einschränkungen** | Die App besteht aus einer ASP.NET-App und einem WCF-Dienst, die auf derselben VM ausgeführt werden. Contoso möchte dies über Azure App Service auf zwei Web-Apps aufteilen. 
**Azure** | Das Unternehmen möchte die App zu Azure migrieren, jedoch nicht auf VMs ausführen. Sowohl in der Web- als auch in der Datenschicht soll auf Azure-PaaS-Dienste zurückgegriffen werden. 

## <a name="solution-design"></a>Lösungsentwurf

Nachdem die Ziele und Anforderungen formuliert wurden, entwirft und überprüft Contoso eine Bereitstellungslösung und identifiziert den Migrationsprozess, einschließlich der für die Migration genutzten Azure-Dienste.

### <a name="current-app"></a>Aktuelle App

- Die lokale SmartHotel-App ist auf zwei VMs aufgeteilt (WEBVM und SQLVM).
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5)
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.


### <a name="proposed-solution"></a>Vorgeschlagene Lösung

- Für die Datenbankebene der App verglich Contoso mithilfe [dieses Artikels](https://docs.microsoft.com/azure/sql-database/sql-database-features) Azure SQL-Datenbank mit SQL Server. Aus verschiedenen Gründen hat sich das Unternehmen für Azure SQL-Datenbank entschieden:
    - Azure SQL-Datenbank ist ein verwalteter Dienst für relationale Datenbanken. Er bietet eine vorhersagbare Leistung auf mehreren Serviceleveln bei nahezu keinem Administrationsaufwand. Zu den Vorteilen gehören dynamische Skalierbarkeit ohne Ausfallzeiten, integrierte intelligente Optimierung sowie globale Skalierbarkeit und Verfügbarkeit.
    - Contoso kann den einfachen Datenmigrations-Assistenten (DMA) nutzen, um die lokale Datenbank zu bewerten und zu Azure SQL-Datenbank zu migrieren.
    - Mit der Software Assurance kann das Unternehmen seine vorhandenen Lizenzen mit dem Azure-Hybridvorteil für SQL Server zu ermäßigten Preisen für eine SQL-Datenbank-Instanz austauschen. So sind Einsparungen von bis zu 30 % möglich.
    - SQL-Datenbank bietet eine Reihe von Sicherheitsfeatures, darunter Always Encrypted, dynamische Datenmaskierung und Sicherheit bzw. Bedrohungserkennung auf Zeilenebene.
- Für die Webebene der App hat sich Contoso dazu entschieden, Azure App Service zu verwenden. Über diesen PaaS-Dienst kann die App mit nur wenigen Konfigurationsänderungen bereitgestellt werden. Das Unternehmen führt die Änderung mit Visual Studio durch und stellt zwei Web-Apps bereit. Eine für die Website, und eine für den WCF-Dienst.
  
### <a name="solution-review"></a>Bewertung der Lösung
Contoso wertet den vorgeschlagen Entwurf durch Erstellen einer Liste mit Vor- und Nachteilen aus.

**Aspekt** | **Details**
--- | ---
**Vorteile** | Der SmartHotel-App-Code muss für die Migration zu Azure nicht geändert werden.<br/><br/> Das Unternehmen kann seine Investition in die Software Assurance mit dem Azure-Hybridvorteil für SQL Server und Windows Server nutzen.<br/><br/> Nach der Migration ist die Unterstützung für Windows Server 2008 R2 nicht mehr erforderlich. [Weitere Informationen](https://support.microsoft.com/lifecycle)<br/><br/> Contoso kann die Webebene der App mit mehreren Instanzen konfigurieren, sodass diese kein Single Point of Failure mehr ist.<br/><br/> Das Unternehmen ist nicht mehr auf das veraltete SQL Server 2008 R2-System angewiesen.<br/><br/> SQL-Datenbank unterstützt die technischen Anforderungen von Contoso. Das Unternehmen bewertete die lokale Datenbank mithilfe des Datenbankmigrations-Assistenten und stellte fest, dass sie kompatibel ist.<br/><br/> SQL-Datenbank verfügt über eine integrierte Fehlertoleranz, die Contoso nicht einrichten muss. Dadurch wird sichergestellt, dass die Datenschicht kein Single Point of Failover mehr ist.
**Nachteile** | Azure App Service unterstützt nur eine App-Bereitstellung für jede Web-App. Dies bedeutet, dass zwei Web-Apps bereitgestellt werden müssen (eine für die Website und eine für den WCF-Dienst).<br/><br/> Wenn Contoso den Datenmigrations-Assistenten anstelle des Datenmigrationsdiensts zur Migration seiner Datenbank verwendet, verfügt das Unternehmen nicht über die entsprechende Infrastruktur für die Migration von Datenbanken in größerem Umfang. Das Unternehmen muss eine andere Region erstellen, um ein Failover zu gewährleisten, wenn die primäre Region nicht verfügbar ist.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

![Szenarioarchitektur](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Migrationsprozess

1. Contoso stellt eine Azure SQL-Datenbank-Instanz bereit, zu der es die SmartHotel-Datenbank migriert.
2. Das Unternehmen stellt Web-Apps bereit und konfiguriert diese und stellt außerdem die SmartHotel-App für diese bereit.

    ![Migrationsprozess](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenmigrations-Assistent (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Sie verwenden den DMA zum Bewerten und Erkennen von Kompatibilitätsproblemen, die sich auf die Funktionalität ihrer Datenbank in Azure auswirken könnten. Der DMA bewertet die Featureparität zwischen SQL-Quellen und -Zielen, und empfiehlt Verbesserungen der Leistung und Zuverlässigkeit. | Sie können das Tool kostenlos herunterladen.
[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) | Ein intelligenter, vollständig verwalteter Dienst für relationale Clouddatenbanken. | Die Kosten ergeben sich durch Features, Durchsatz und Größe. [Weitere Informationen](https://azure.microsoft.com/pricing/details/sql-database/managed/)
[Azure App Service-Web-Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) | Es wird gezeigt, wie leistungsstarke Cloud-Apps auf einer vollständig verwalteten Plattform erstellt werden können. | Die Kosten basieren auf Größe, Standort und Nutzungsdauer. [Weitere Informationen](https://azure.microsoft.com/pricing/details/app-service/windows/)

## <a name="prerequisites"></a>Voraussetzungen

Für die Ausführung dieses Szenarios benötigen Sie (und Contoso) Folgendes:

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie müssten bereits ein Abonnement erstellt haben, als Sie die Beurteilung im ersten Artikel dieser Reihe durchgeführt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.
**Azure-Infrastruktur** | [Weitere Informationen](contoso-migration-infrastructure.md) zur Vorgehensweise von Contoso beim Einrichten einer Azure-Infrastruktur.


## <a name="scenario-steps"></a>Szenarioschritte

Contoso geht bei der Ausführung der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Bereitstellen einer SQL-Datenbank-Instanz in Azure**: Contoso stellt eine SQL-Instanz in Azure bereit. Nachdem die App-Website zu Azure migriert wurde, verweist die Web-App für den WCF-Dienst auf diese Instanz.
> * **Schritt 2: Migrieren der Datenbank mit dem DMA**: Das Unternehmen migriert die App-Datenbank mit dem Datenmigrations-Assistenten.
> * **Schritt 3: Bereitstellen von Web-Apps**: Contoso stellt die beiden Web-Apps bereit.
> * **Schritt 4: Konfigurieren von Verbindungszeichenfolgen**: Das Unternehmen konfiguriert Verbindungszeichenfolgen, damit die Web-App in der Webebene, die Web-App für den WCF-Dienst und die SQL-Instanz kommunizieren können.
> * **Schritt 5: Veröffentlichen von Web-Apps**: Im letzten Schritt veröffentlicht Contoso die Apps in Azure.


## <a name="step-1-provision-an-azure-sql-database"></a>Schritt 1: Bereitstellen einer Azure SQL-Datenbank-Instanz

1. Contoso entscheidet sich, eine SQL-Datenbank-Instanz in Azure zu erstellen. 

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Das Unternehmen gibt einen Datenbanknamen an, der mit der Datenbank übereinstimmt, die auf der lokalen VM (**SmartHotel.Registration**) ausgeführt wird. Die Datenbank wird dann in der ContosoRG-Ressourcengruppe platziert. Dies ist die Ressourcengruppe, die das Unternehmen für die Produktionsressourcen in Azure verwendet.

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Das fiktive Unternehmen richtet eine neue SQL Server-Instanz (**sql-smarthotel-eus2**) in der primären Region ein.

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Den Tarif legt das Unternehmen entsprechend seiner Server- und Datenbankanforderungen fest. Zudem entscheidet es sich, Geld mit dem Azure-Hybridvorteil zu sparen, da es bereits über eine SQL Server-Lizenz verfügt.
5. Zur Größenanpassung nutzt es das auf virtuellen Kernen basierende Kaufmodell und legt die Grenzwerte für seine erwarteten Anforderungen fest.

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Dann erstellt Contoso die Datenbankinstanz.

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Nach dem Erstellen der Instanz öffnet das Unternehmen die Datenbank und notiert sich Details, die es für die Migration mit dem Datenbankmigrations-Assistenten benötigt.

    ![Bereitstellen von SQL](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Benötigen Sie weitere Hilfe?**

- [Erhalten Sie Hilfe](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bei der Bereitstellung einer SQL-Datenbank-Instanz.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) zu Ressourcengrenzwerten des auf virtuellen Kernen basierenden Kaufmodells.


## <a name="step-2-migrate-the-database-with-dma"></a>Schritt 2: Migrieren der Datenbank mit dem DMA

Contoso wird die SmartHotel-Datenbank mit dem DMA migrieren.

### <a name="install-dma"></a>Installieren des DMA

1. Es lädt das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) auf die lokale SQL Server-VM (**SQLVM**) herunter.
2. Es führt das Setup (DownloadMigrationAssistant.msi) auf der VM aus.
3. Auf der Seite **Fertigstellen** wählen sie **Launch Microsoft Data Migration Assistant** (Microsoft-Datenmigrations-Assistenten starten) aus, bevor sie den Assistenten beenden.

### <a name="migrate-the-database-with-dma"></a>Migrieren der Datenbank mit dem DMA

1. Im DMA erstellt das Unternehmen ein neues Projekt (**SmartHotelDB**) und wählt **Migration** aus. 
2. Als Quellservertyp wird **SQL Server** und als Ziel **Azure SQL-Datenbank** ausgewählt. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. In den Migrationsdetails fügt das Unternehmen **SQLVM** als Quellserver und die **SmartHotel.Registration**-Datenbank als Ziel hinzu. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Ein Fehler wird angezeigt, der anscheinend mit der Authentifizierung zusammenhängt. Beim Ermitteln der Ursache wird jedoch festgestellt, dass das Problem durch den Punkt (.) im Datenbanknamen verursacht wird. Daher wurde beschlossen, eine neue SQL-Datenbank-Instanz mit dem Namen **SmartHotel-Registration** bereitzustellen, um das Problem zu beheben. Beim erneuten Ausführen des DMA lässt sich **SmartHotel-Registration** jetzt auswählen, und der Assistent kann fortgesetzt werden.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. Unter **Objekte auswählen** wählt das Unternehmen die Datenbanktabellen aus und generiert ein SQL-Skript.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. Nachdem das Skript durch den DMS erstellt wurde, klickt Contoso auf **Schema bereitstellen**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. Der DMA bestätigt, dass die Bereitstellung erfolgreich war.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. Jetzt kann mit der Migration begonnen werden.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Nach Abschluss der Migration kann Contoso überprüfen, ob die Datenbank auf der Azure SQL-Datenbank-Instanz ausgeführt wird.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. Die zusätzliche SQL-Datenbank-Instanz **SmartHotel.Registration** wird von Contoso im Azure-Portal gelöscht.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>Schritt 3: Bereitstellen von Web-Apps

Nachdem die Datenbank migriert wurde, kann Contoso die beiden Web-Apps bereitstellen.

1. Das Unternehmen wählt **Web-App** im Portal aus.

    ![Web-App](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Es gibt einen App-Namen an (**SHWEB-EUS2**), führt die App unter Windows aus und platziert sie in der Produktionsressourcengruppe **ContosoRG**. Anschließend erstellt es eine neue App Service-Instanz und einen neuen App Service-Plan.

    ![Web-App](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Nachdem die Web-App bereitgestellt wurde, wird der Prozess zum Erstellen einer Web-App für den WCF-Dienst (**SHWCF-EUS2**) wiederholt.

    ![Web-App](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. Anschließend navigiert Contoso zu der Adresse der Apps, um zu überprüfen, ob diese erfolgreich erstellt wurden.

## <a name="step-4-configure-connection-strings"></a>Schritt 4: Konfigurieren von Verbindungszeichenfolgen

Contoso muss sicherstellen, dass die Web-Apps und die Datenbank allesamt kommunizieren können. Zu diesem Zweck konfiguriert das Unternehmen Verbindungszeichenfolgen im Code und in den Web-Apps.

1. In der Web-App für den WCF-Dienst (**SHWCF-EUS2**) fügt Contoso unter **Einstellungen** > **Anwendungseinstellungen** eine neue Verbindungszeichenfolge mit dem Namen  **DefaultConnection** hinzu.
2. Die Verbindungszeichenfolge wird von der Datenbank **SmartHotel Registrierung** abgerufen und sollte mit den entsprechenden Anmeldeinformationen aktualisiert werden.

    ![Verbindungszeichenfolge](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Mithilfe von Visual Studio öffnet Contoso die Projektmappendatei im Ordner **SmartHotel360-internal-booking-apps**. Der Abschnitt **connectionStrings** der Datei „web.config“ für den WCF-Dienst „SmartHotel.Registration.Wcf“ sollte mit der Verbindungszeichenfolge aktualisiert werden.

     ![Verbindungszeichenfolge](media/contoso-migration-refactor-web-app-sql/string2.png)

4. Der Abschnitt **client** der Datei „web.config“ für SmartHotel.Registration.Web sollte dahingehend geändert werden, dass es auf den neuen Speicherort des WCF-Diensts verweist. Dies ist die URL der WCF-Web-App, die den Dienstendpunkt hostet.

    ![Verbindungszeichenfolge](media/contoso-migration-refactor-web-app-sql/strings3.png)


## <a name="step-5-publish-web-apps"></a>Schritt 5: Veröffentlichen von Web-Apps

Im letzten Schritt veröffentlicht Contoso die Web-Apps in Azure.

1. In Visual Studio klickt Contoso mit der rechten Maustaste auf das Projekt „SmartHotel.REgistration.Wcf“ und dann auf **Veröffentlichen**.

    ![Veröffentlichen](media/contoso-migration-refactor-web-app-sql/publish-web1.png)

2. Nun wird mit der Veröffentlichung begonnen.

    ![Veröffentlichen](media/contoso-migration-refactor-web-app-sql/publish-web2.png)

3. Contoso wählt eine vorhandene App Service-Instanz aus, da das Unternehmen bereits die Web-App erstellt hat.

    ![Veröffentlichen](media/contoso-migration-refactor-web-app-sql/publish-web3.png)

4. Nachdem die WCF-App ausgewählt wurde, wird sie von Visual Studio bereitgestellt.

    ![Veröffentlichen](media/contoso-migration-refactor-web-app-sql/publish-web4.png)

5. Anschließend wiederholt das Unternehmen den Prozess zum Veröffentlichen der Web-App „SmartHotel.Registration.Web“.

    ![Veröffentlichen](media/contoso-migration-refactor-web-app-sql/publish-web5.png)


An dieser Stelle wurde die Anwendung erfolgreich zu Azure migriert.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach der Migration muss Contoso die folgenden Schritte zur Bereinigung ausführen:  

- Entfernen der lokalen VMs aus dem vCenter-Bestand.
- Entfernen der VMs aus lokalen Sicherungsaufträgen.
- Aktualisieren der internen Dokumentation zur Anzeige der neuen Speicherorte für die SmartHotel-App. Anzeigen, dass die Datenbank in Azure SQL-Datenbank und das Front-End in zwei Web-Apps ausgeführt wird.
- Überprüfen sämtlicher Ressourcen, die mit den außer Betrieb genommenen VMs interagieren, und Aktualisieren sämtlicher relevanter Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.


## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die migrierten Ressourcen in Azure enthalten sind, muss Contoso die neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

- Contoso muss gewährleisten, dass seine neue Datenbank **SmartHotel-Registration** sicher ist. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)
- Insbesondere sollte das Unternehmen die Web-Apps für die Verwendung von SSL mit Zertifikaten aktualisieren.

### <a name="backups"></a>Backups

- Contoso muss die Sicherungsanforderungen für Azure SQL-Datenbank überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)
- Das Unternehmen sollte die Implementierung von Failovergruppen berücksichtigen, um ein regionales Failover für die Datenbank bereitzustellen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)
- Contoso muss hinsichtlich der Resilienz die Bereitstellung der Web-App in der Hauptregion „USA, Osten 2“ und „USA, Mitte“ in Erwägung ziehen. Das Unternehmen könnte den Traffic Manager konfigurieren, um ein Failover bei regionalen Ausfällen sicherzustellen.

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nachdem alle Ressourcen bereitgestellt wurden, sollte Contoso Azure-Tags basierend auf seiner [Infrastrukturplanung](contoso-migration-infrastructure.md#set-up-tagging) zuweisen.
- Die gesamte Lizenzierung ist in die Kosten für die PaaS-Dienste integriert, die Contoso verwendet. Dies wird über EA verrechnet.
1. Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 

## <a name="conclusion"></a>Zusammenfassung

Im vorliegenden Artikel hat Contoso die SmartHotel-App in Azure umgestaltet, indem das Unternehmen die Front-End-VM der App mithilfe zweier Azure-Web-Apps migriert hat. Das Unternehmen hat die App-Datenbank zu einer Azure SQL-Datenbank-Instanz migriert.







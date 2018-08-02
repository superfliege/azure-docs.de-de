---
title: Übersicht zur Contoso-Migration in Azure | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Migrationsstrategie und Szenarien, die Contoso verwendet, um das lokale Rechenzentrum zu Azure zu migrieren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: be9302d47766ddc7d75496e28e9b18a8e870c878
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216239"
---
# <a name="contoso-migration-overview"></a>Contoso-Migration: Übersicht


Dieser Artikel veranschaulicht, wie die fiktive Organisation Contoso ihre lokale Infrastruktur in die [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/)-Cloud migriert. 

Dieses Dokument ist das erste in einer Reihe von Artikeln, die zeigen, wie das fiktive Unternehmen Contoso Ressourcen zu Azure migriert. Die Reihe enthält Informationen und Szenarien, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird und wie verschiedene Migrationstypen ausgeführt werden. Die Szenarios werden an Komplexität zunehmen, und wir werden mit der Zeit zusätzliche Artikel hinzufügen. Die Artikel zeigen, wie das Unternehmen Contoso seine Migration durchführt, verweisen jedoch durchgängig auf allgemeine Informationen und spezielle Anweisungen.

## <a name="introduction"></a>Einführung

Azure bietet Zugriff auf einen umfassenden Satz von Clouddiensten. Als Entwickler und IT-Spezialisten können Sie diese Dienste zum Erstellen, Bereitstellen und Verwalten von Anwendungen für verschiedene Tools und Frameworks über ein globales Netzwerk von Rechenzentren verwenden. Im Rahmen der Herausforderungen, die Ihr Unternehmen in Bezug auf die digitalen Veränderungen bewältigen muss, können Sie mithilfe der Azure-Cloud ermitteln, wie Sie Ressourcen und Vorgänge optimieren, sich mit Ihren Kunden und Mitarbeitern vernetzen und Ihre Produkte transformieren.

Es ist aber auch klar, dass viele Organisationen trotz aller Vorteile, die sich mit der Cloud in Bezug auf Geschwindigkeit und Flexibilität, Kostensenkung, Leistung und Zuverlässigkeit ergeben, noch längere Zeit lokale Rechenzentren betreiben müssen. Als Antwort auf die Hürden, die bei der Umstellung auf die Cloud überwunden werden müssen, verfügt Azure über eine Hybrid Cloud-Strategie, bei der Brücken zwischen Ihren lokalen Rechenzentren und der öffentlichen Azure-Cloud gebildet werden. Beispiele hierfür sind die Verwendung von Azure-Cloudressourcen, z.B. Azure Backup zum Schützen von lokalen Ressourcen, oder der Einsatz von Azure-Analysen, um Erkenntnisse zu lokalen Workloads zu erhalten. 

Im Rahmen der Hybrid Cloud-Strategie werden für Azure kontinuierlich erweiterte Lösungen zum Migrieren von lokalen Apps und Workloads in die Cloud bereitgestellt. Sie können Ihre lokalen Ressourcen mit einfachen Schritten umfassend bewerten, um zu ermitteln, wie sie in der Azure-Cloud ausgeführt werden. Nachdem Sie über eine ausführliche Bewertung verfügen, können Sie die Migration der Ressourcen zu Azure beruhigt angehen. Wenn die Ressourcen in Azure aktiv sind und ausgeführt werden, können Sie sie optimieren, um Zugriff, Flexibilität, Sicherheit und Zuverlässigkeit sicherzustellen und zu verbessern.

## <a name="migration-strategies"></a>Migrationsstrategien

Die Strategien für die Migration zur Cloud lassen sich grob in vier Kategorien unterteilen: Zuweisen eines neuen Hosts, Umgestalten, Überarbeiten oder Neuerstellen. Welche Strategie Sie übernehmen, richtet sich nach Ihrer Business-Treibern und Migrationszielen. Sie könnten mehrere Strategien übernehmen. Z.B. könnten Sie entscheiden, einfachen Apps bzw. Apps, die nicht unternehmenskritisch sind, einen neuen Host zuzuweisen (Lift & Shift), jedoch diejenigen zu überarbeiten, die komplexer und unternehmenskritisch sind. Betrachten wir nun die Strategien.


**Strategie** | **Definition** | **Einsatzgebiete** 
--- | --- | --- 
**Zuweisen eines neuen Hosts** | Häufig als „Lift & Shift“-Migration bezeichnet. Diese Option erfordert keine Änderungen des Codes und ermöglicht Ihnen die schnelle Migration Ihrer vorhandenen Apps zu Azure. Um die Vorteile der Cloud ohne die mit Änderungen des Codes verbundenen Risiken und Kosten zu nutzen, wird jede App im vorliegenden Zustand migriert. | Wenn Sie Apps schnell in die Cloud verschieben müssen.<br/><br/> Wenn Sie eine App verschieben möchten, ohne sie zu ändern.<br/><br/> Wenn Ihre Apps so überarbeitet sind, dass sie nach der Migration die [Azure-IaaS](https://azure.microsoft.com/overview/what-is-iaas/)-Skalierbarkeit nutzen können.<br/><br/> Wenn Apps für Ihr Unternehmen wichtig sind, aber keine unmittelbaren Änderungen der App-Funktionen erforderlich sind.
**Umgestalten** | Die häufig als „Umpacken“ bezeichnete Umgestaltung erfordert nur minimale Änderungen der Apps, sodass sie eine Verbindung mit [Azure-PaaS](https://azure.microsoft.com/overview/what-is-paas/) herstellen und Cloudangebote verwenden können.<br/><br/> Sie könnten Ihre vorhandenen Apps beispielsweise zu Azure App Service oder Azure Kubernetes Service (AKS) migrieren.<br/><br/> Alternativ könnten Sie relationale und nicht relationale Datenbanken für die verwaltete Azure SQL-Datenbank-Instanz, Azure Database for MySQL, Azure Database for PostgreSQL und Azure Cosmos DB umgestalten. | Wenn Ihre App einfach zum Arbeiten in Azure umgepackt werden kann.<br/><br/> Wenn Sie innovative, von Azure bereitgestellte DevOps-Methoden anwenden möchten oder erwägen, DevOps mit einer Containerstrategie für Workloads zu verwenden.<br/><br/> Für die Umgestaltung müssen Sie die Portabilität Ihrer vorhandenen Codebasis und verfügbare Entwicklungsfertigkeiten berücksichtigen.
**Überarbeiten** | Beim Überarbeiten für die Migration liegt der Fokus auf dem Ändern und Erweitern von App-Funktionalität und Codebasis, um die App-Architektur für die Cloudskalierbarkeit zu optimieren.<br/><br/> Sie könnten z.B. eine monolithische Anwendung in eine Gruppe von Microservices unterteilen, die zusammenarbeiten und einfach zu skalieren sind.<br/><br/> Alternativ könnten Sie auch Ihre relationalen und nicht relationalen Datenbanken zu vollständig verwalteten DBaaS-Lösungen überarbeiten, z.B. die verwaltete Azure SQL-Datenbank-Instanz, Azure Database for MySQL, Azure Database for PostgreSQL und Azure Cosmos DB. | Wenn Ihre Apps eine größere Überarbeitung benötigen, um neue Funktionen zu integrieren oder effektiv auf einer Cloudplattform zu funktionieren.<br/><br/> Wenn Sie vorhandene Anwendungsinvestitionen nutzen, Skalierbarkeitsanforderungen erfüllen, innovative Azure DevOps-Methoden anwenden und die Verwendung virtueller Computer minimieren möchten.
**Neuerstellen** | Das Neuerstellen geht einen Schritt weiter, da eine App in diesem Fall unter Verwendung von Azure-Cloudtechnologien von Grund auf neu erstellt wird.<br/><br/> Sie könnten beispielsweise Greenfield-Apps mit nativen Cloudtechnologien wie Azure Functions, Azure AI, verwalteten Azure SQL-Datenbank-Instanzen und Azure Cosmos DB erstellen. | Wenn Sie schnelle Entwicklung wünschen und Funktionalität sowie Lebensdauer vorhandener Apps eingeschränkt sind.<br/><br/> Wenn Sie bereit sind, Business-Innovation (einschließlich von Azure bereitgestellter DevOps-Methoden) voranzutreiben, neue Anwendungen mit nativen Cloudtechnologien zu erstellen und die Weiterentwicklungen in KI, Blockchain und IoT zu nutzen.

## <a name="migration-articles"></a>Artikel zur Migration

Die Artikel der Reihe werden in der folgenden Tabelle zusammengefasst.  

- Die einzelnen Migrationsszenarien basieren auf geringfügig variierten Geschäftszielen, die die Migrationsstrategie bestimmen.
- Für die einzelnen Bereitstellungsszenarien erhalten Sie Informationen zu Business-Treibern und Zielen, einer vorgeschlagenen Architektur, Schritten zum Ausführen der Migration sowie Empfehlungen zur Bereinigung und den nächsten Schritten nach Abschluss der Migration.

**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: Übersicht | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Dieser Artikel
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die SQL Server-Datenbank der App mit dem Datenbankmigrations-Assistenten. | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und einer verwalteten SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die lokale App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von Azure Site Recovery und die App-Datenbank mithilfe von Azure Database Migration Service zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Hier wird veranschaulicht, wie Contoso mit dem Azure Site Recovery-Dienst die VMs der SmartHotel-App zu Azure-VMs migriert. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Hier wird erläutert, wie Contoso mit Azure Site Recovery die SmartHotel-App migriert, um die App-VMs zu migrieren, und mit Azure Database Migration Service die App-Datenbank zu einem SQL Server-Cluster migriert, der durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Azure Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Azure Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 9: Umgestalten einer App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz migriert. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App zu einer Azure-Web-App und einer Azure Database for MySQL-Instanz](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App zu Azure-Web-Apps auf mehreren Websites migriert, die für Continuous Delivery in GitHub integriert sind. Die App-Datenbank wird zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Service, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar






### <a name="demo-apps"></a>Demo-Apps

In den Artikeln werden zwei Demo-Apps verwendet – SmartHotel und osTicket.

- **SmartHotel360**: Diese App wurde von Microsoft als Test-App entwickelt, die Sie beim Arbeiten mit Azure verwenden können. Sie wird als Open-Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden. Es ist eine ASP.NET-App, die mit einer SQL Server-Datenbank verbunden ist. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Windows Server 2008 R2 und SQL Server 2008 R2 ausgeführt werden. Die App-VMs werden lokal gehostet und von vCenter Server verwaltet.
- **osTicket**: Dies ist eine unter Linux ausgeführte Open Source-App für Service Desk-Tickets. Sie können es von [GitHub](https://github.com/osTicket/osTicket) herunterladen. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Ubuntu 16.04 LTS mithilfe von Apache 2, PHP 7.0 und MySQL 5.7 ausgeführt werden.
    

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [Contoso eine lokale Infrastruktur und eine Azure-Infrastruktur als Vorbereitung auf die Migration einrichtet](contoso-migration-infrastructure.md).




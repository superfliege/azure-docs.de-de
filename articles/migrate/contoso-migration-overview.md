---
title: Übersicht zur Contoso-Migration in Azure | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Migrationsstrategie und Szenarien, die Contoso verwendet, um das lokale Rechenzentrum zu Azure zu migrieren.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: f18af40c67195ee2e1e74c04c0834eea06fb14d1
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53191953"
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
Artikel 1: Übersicht | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. | Dieser Artikel
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Contoso führt eine Bewertung seiner lokalen App SmartHotel360 durch, die in VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten. | Verfügbar
[Artikel 4: Rehosten einer App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert den virtuellen Front-End-Computer der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank mit dem [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten Azure SQL-Datenbank-Instanz. | Verfügbar   
[Artikel 5: Rehosten einer App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der SmartHotel360-App mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar   [Artikel 5: Rehosten einer App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der App SmartHotel360 mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Rehosten einer App auf Azure-VMs und in einer SQL Server AlwaysOn-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die App SmartHotel360. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar [Artikel 7: Rehosten einer Linux-App auf Azure-VMs](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe des Site Recovery-Diensts per Lift & Shift-Vorgang eine Migration seiner Linux-App „osTicket“ zu virtuellen Azure-Computern durch. | Verfügbar
[Artikel 8: Rehosten einer Linux-App auf Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert seine Linux-App „osTicket“ mithilfe von Site Recovery zu Azure-VMs. Die App-Datenbank wird zu Azure Database for MySQL migriert, indem MySQL Workbench verwendet wird. | Verfügbar
[Artikel 9: Umgestalten einer App in eine Azure-Web-App und in Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert die SmartHotel360-App zu einer Azure-Web-App und die App-Datenbank mithilfe des Datenbankmigrations-Assistenten zu einer Azure SQL Server-Instanz. | Verfügbar    
[Artikel 10: Umgestalten einer Linux-App in eine Azure-Web-App und in Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert die Linux-App „osTicket“ mithilfe von Azure Traffic Manager zu einer Azure-Web-App in mehreren Azure-Regionen. Zur Sicherstellung der Continuous Delivery erfolgt eine Integration in GitHub. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von Team Foundation Server in Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer App in einen Azure-Container und Azure SQL-Datenbank](contoso-migration-rearchitect-container-sql.md) | Contoso migriert die SmartHotel-App zu Azure. Anschließend wird die App-Webschicht zu einem Windows-Container umstrukturiert, der in Azure Service Fabric ausgeführt wird, und die Datenbank wird zu einer Azure SQL-Datenbank umstrukturiert. | Verfügbar    
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z.B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar 
[Artikel 14: Skalieren einer Migration zu Azure](contoso-migration-scale.md) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor. | Verfügbar

In diesem Artikel richtet Contoso sämtliche Infrastrukturelemente ein, die für die Durchführung aller Migrationsszenarien erforderlich sind. 







### <a name="demo-apps"></a>Demo-Apps

In den Artikeln werden zwei Demo-Apps verwendet – SmartHotel360 und osTicket.

- **SmartHotel360**: Diese App wurde von Microsoft als Test-App entwickelt, die Sie beim Arbeiten mit Azure verwenden können. Sie wird als Open-Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden. Es ist eine ASP.NET-App, die mit einer SQL Server-Datenbank verbunden ist. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Windows Server 2008 R2 und SQL Server 2008 R2 ausgeführt werden. Die App-VMs werden lokal gehostet und von vCenter Server verwaltet.
- **osTicket**:  Dies ist eine unter Linux ausgeführte Open Source-App für Service Desk-Tickets. Sie können es von [GitHub](https://github.com/osTicket/osTicket) herunterladen. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Ubuntu 16.04 LTS mithilfe von Apache 2, PHP 7.0 und MySQL 5.7 ausgeführt werden.
    

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [Contoso eine lokale Infrastruktur und eine Azure-Infrastruktur als Vorbereitung auf die Migration einrichtet](contoso-migration-infrastructure.md).




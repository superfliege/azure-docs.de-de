---
title: Übersicht zur Contoso-Migration in Azure | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Migrationsstrategie und Szenarien, die Contoso verwendet, um das lokale Rechenzentrum zu Azure zu migrieren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: ec0308bb2e39c3801305748f19783e70d58b0b7e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232035"
---
# <a name="contoso-migration-overview"></a>Contoso-Migration: Übersicht


Dies ist der erste Artikel in einer Reihe, der veranschaulicht, wie das fiktive Unternehmen Contoso seine lokale Infrastruktur zur [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/)-Cloud migriert. 

Die Artikelreihe stellt Beispielbereitstellungen vor, die Ihnen helfen, verschiedene Optionen für die Migration zu Azure zu verstehen und auszuprobieren, und Beispielbereitstellungen zum Arbeiten in einer Hybridumgebung. Die Artikel zeigen, wie das Unternehmen Contoso seine Migration durchführt, verweisen jedoch durchgängig auf allgemeine Informationen und spezielle Anweisungen.

## <a name="introduction"></a>Einführung

Azure bietet Zugriff auf einen umfassenden Satz von Clouddiensten. Als Entwickler und IT-Spezialisten können Sie diese Dienste zum Erstellen, Bereitstellen und Verwalten von Anwendungen für verschiedene Tools und Frameworks über ein globales Netzwerk von Rechenzentren verwenden. Im Rahmen der Herausforderungen, die Ihr Unternehmen in Bezug auf die digitalen Veränderungen bewältigen muss, können Sie mithilfe der Azure-Cloud ermitteln, wie Sie Ressourcen und Vorgänge optimieren, sich mit Ihren Kunden und Mitarbeitern vernetzen und Ihre Produkte transformieren.

Es ist aber auch klar, dass viele Organisationen trotz aller Vorteile, die sich mit der Cloud in Bezug auf Geschwindigkeit und Flexibilität, Kostensenkung, Leistung und Zuverlässigkeit ergeben, noch längere Zeit lokale Rechenzentren betreiben müssen. Als Antwort auf die Hürden, die bei der Umstellung auf die Cloud überwunden werden müssen, verfügt Azure über eine Hybrid Cloud-Strategie, bei der Brücken zwischen Ihren lokalen Rechenzentren und der öffentlichen Azure-Cloud gebildet werden. Beispiele hierfür sind die Verwendung von Azure-Cloudressourcen, z.B. Sicherungen zum Schützen von lokalen Ressourcen, oder der Einsatz von Azure-Analysen, um Erkenntnisse zu lokalen Workloads zu erhalten. 

Im Rahmen der Hybrid Cloud-Strategie werden für Azure kontinuierlich erweiterte Lösungen zum Migrieren von lokalen Apps und Workloads in die Cloud bereitgestellt. Sie können Ihre lokalen Ressourcen mit einfachen Schritten umfassend bewerten, um zu ermitteln, wie sie in der Azure-Cloud ausgeführt werden. Nachdem Sie über eine ausführliche Bewertung verfügen, können Sie die Migration der Ressourcen zu Azure beruhigt angehen. Wenn die Ressourcen in Azure aktiv sind und ausgeführt werden, können Sie sie optimieren, um Zugriff, Flexibilität, Sicherheit und Zuverlässigkeit sicherzustellen und zu verbessern.

## <a name="migration-strategies"></a>Migrationsstrategien

Die Strategien für die Migration zur Cloud lassen sich grob in vier Kategorien unterteilen: Zuweisen eines neuen Hosts, Umgestalten, Überarbeiten oder Neuerstellen. Welche Strategie Sie übernehmen, richtet sich nach Ihrer Business-Treibern und Migrationszielen. Sie könnten mehrere Strategien übernehmen. Z.B. könnten Sie entscheiden, einfachen Apps bzw. Apps, die nicht unternehmenskritisch sind, einen neuen Host zuzuweisen (Lift & Shift), jedoch diejenigen zu überarbeiten, die komplexer und unternehmenskritisch sind. Betrachten wir nun die Strategien.


**Strategie** | **Definition** | **Einsatzgebiete** 
--- | --- | --- 
**Zuweisen eines neuen Hosts** | Häufig als „Lift & Shift“-Migration bezeichnet. Diese Option erfordert keine Änderungen des Codes und ermöglicht Ihnen die schnelle Migration Ihrer vorhandenen Apps zu Azure. Um die Vorteile der Cloud ohne die mit Änderungen des Codes verbundenen Risiken und Kosten zu nutzen, wird jede App im vorliegenden Zustand migriert. | Wenn Sie Apps schnell in die Cloud verschieben müssen.<br/><br/> Wenn Sie eine App verschieben möchten, ohne sie zu ändern.<br/><br/> Wenn Ihre Apps so überarbeitet sind, dass sie nach der Migration die [Azure-IaaS](https://azure.microsoft.com/overview/what-is-iaas/)-Skalierbarkeit nutzen können.<br/><br/> Wenn Apps für Ihr Unternehmen wichtig sind, aber keine unmittelbaren Änderungen der App-Funktionen erforderlich sind.
**Umgestalten** | Die häufig als „Umpacken“ bezeichnete Umgestaltung erfordert nur minimale Änderungen der Apps, sodass sie eine Verbindung mit [Azure-PaaS](https://azure.microsoft.com/overview/what-is-paas/) herstellen und Cloudangebote verwenden können.<br/><br/> Sie könnten Ihre vorhandenen Apps beispielsweise zu Azure App Service oder Azure Kubernetes Service (AKS) migrieren.<br/><br/> Alternativ könnten Sie relationale und nicht relationale Datenbanken für die verwaltete Azure SQL-Datenbank-Instanz, Azure Database for MySQL, Azure Database for PostgreSQL und Azure Cosmos DB umgestalten. | Wenn Ihre App einfach zum Arbeiten in Azure umgepackt werden kann.<br/><br/> Wenn Sie innovative, von Azure bereitgestellte DevOps-Methoden anwenden möchten oder erwägen, DevOps mit einer Containerstrategie für Workloads zu verwenden.<br/><br/> Für die Umgestaltung müssen Sie die Portabilität Ihrer vorhandenen Codebasis und verfügbare Entwicklungsfertigkeiten berücksichtigen.
**Überarbeiten** | Beim Überarbeiten für die Migration liegt der Fokus auf dem Ändern und Erweitern von App-Funktionalität und Codebasis, um die App-Architektur für die Cloudskalierbarkeit zu optimieren.<br/><br/> Sie könnten z.B. eine monolithische Anwendung in eine Gruppe von Microservices unterteilen, die zusammenarbeiten und einfach zu skalieren sind.<br/><br/> Alternativ könnten Sie auch Ihre relationalen und nicht relationalen Datenbanken zu vollständig verwalteten DBaaS-Lösungen überarbeiten, z.B. die verwaltete Azure SQL-Datenbank-Instanz, Azure Database for MySQL, Azure Database for PostgreSQL und Azure Cosmos DB. | Wenn Ihre Apps eine größere Überarbeitung benötigen, um neue Funktionen zu integrieren oder effektiv auf einer Cloudplattform zu funktionieren.<br/><br/> Wenn Sie vorhandene Anwendungsinvestitionen nutzen, Skalierbarkeitsanforderungen erfüllen, innovative Azure DevOps-Methoden anwenden und die Verwendung virtueller Computer minimieren möchten.
**Neuerstellen** | Das Neuerstellen geht einen Schritt weiter, da eine App in diesem Fall unter Verwendung von Azure-Cloudtechnologien von Grund auf neu erstellt wird.<br/><br/> Sie könnten z.B. Greenfield-Apps mit nativen Cloudtechnologien wie serverloser Technologie, Azure AI, verwalteten Azure SQL-Datenbank-Instanzen und Azure Cosmos DB erstellen. | Wenn Sie schnelle Entwicklung wünschen und Funktionalität sowie Lebensdauer vorhandener Apps eingeschränkt sind.<br/><br/> Wenn Sie bereit sind, Business-Innovation (einschließlich von Azure bereitgestellter DevOps-Methoden) voran zu treiben, neue Anwendungen mit nativen Cloudtechnologien zu erstellen und die Weiterentwicklungen in KI, Blockchain und IoT zu nutzen.

## <a name="migration-articles"></a>Artikel zur Migration

Die Artikel der Reihe zur Contoso-Migration werden in der folgenden Tabelle zusammengefasst.  

- Die Komplexität der Szenarien steigt, und wir ergänzen sie von Zeit zu Zeit.
- Die einzelnen Migrationsszenarien basieren auf geringfügig variierten Geschäftszielen, die die Migrationsstrategie bestimmen.
- Für die einzelnen Bereitstellungsszenarien erhalten Sie Informationen zu Business-Treibern und Zielen, einer vorgeschlagenen Architektur, Schritten zum Ausführen der Migration sowie Empfehlungen zur Bereinigung und den nächsten Schritten nach Abschluss der Migration.

**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: Übersicht (dieser Artikel) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-overview.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für sämtliche Contoso-Migrationsszenarien wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md) | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen, unter VMware ausgeführten App SmartHotel durchführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die SQL Server-Datenbank-App mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso die App SmartHotel zu Azure migriert. Die App-Web-VM wird mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) migriert und die App-Datenbank mit dem Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz migriert. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel mit dem SiteRecovery-Dienst migriert.
[Artikel 6: Zuweisen eines neuen Hosts zu Azure-VMs und SQL Server-Verfügbarkeitsgruppen](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Das Unternehmen verwendet Site Recovery zum Migrieren der App-VM und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-AlwaysOn-Verfügbarkeitsgruppe. | Verfügbar
[Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso die App-VMs mithilfe von Site Recovery zu Azure migriert. | Geplant
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs und Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die App-VM mithilfe von Site Recovery migriert und MySQL-Workbench zum Migrieren zu einer Azure MySQL Server-Instanz verwendet. | Geplant



### <a name="demo-apps"></a>Demo-Apps

In den Artikeln werden zwei Demo-Apps verwendet – SmartHotel und osTicket.

- **SmartHotel360**: Diese App wurde von Microsoft als Test-App entwickelt, die Sie beim Arbeiten mit Azure verwenden können. Sie wird als Open-Source-App bereitgestellt und kann von [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden. Es ist eine ASP.NET-App, die mit einer SQL Server-Datenbank verbunden ist. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Windows Server 2008 R2 und SQL Server 2008 R2 ausgeführt werden. Die App-VMs werden lokal gehostet und von vCenter Server verwaltet.
- **osTicket**: Dies ist eine unter Linux ausgeführte Open Source-App für Service Desk-Tickets. Sie können es von [GitHub](https://github.com/osTicket/osTicket) herunterladen. Derzeit befindet sich die App auf zwei VMware-VMs, die unter Ubuntu 16.04LTS mithilfe von Apache-2, PHP 7.0 und MySQL 5.7 ausgeführt werden.
    

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie](contoso-migration-infrastructure.md) Contoso die lokale und Azure-Infrastruktur zur Vorbereitung auf die Migration einrichtet.




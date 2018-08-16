---
title: Bewerten von lokalen Workloads für die Contoso-Migration zu Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Contoso seine lokalen Computer in Bezug auf die Migration zu Azure bewertet, indem Azure Migrate und der Datenmigrations-Assistent verwendet werden.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: raynew
ms.openlocfilehash: 50d1b8fca8e5377c35810e08258a0ecc3770ae75
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422323"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Contoso-Migration: Bewerten von lokalen Workloads für die Migration zu Azure

In diesem Artikel bewertet Contoso die lokale SmartHotel-App als Vorbereitung auf die Migration der App zu Azure.

Dieser Artikel ist der dritte einer Artikelreihe, mit der dokumentiert wird, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und eine Reihe von Bereitstellungsszenarien, in denen die Einrichtung einer Migrationsinfrastruktur veranschaulicht wird, die Eignung der lokalen Ressourcen für die Migration bewertet wird und verschiedene Arten von Migrationen durchgeführt werden. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Der Reihe werden im Laufe der Zeit weitere Artikel hinzugefügt.

Artikel | Details | Status
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Enthält eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die darin verwendeten Beispiel-Apps. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Verfügbar
Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure | Contoso führt eine Bewertung seiner lokalen zweischichtigen SmartHotel-App durch, die per VMware ausgeführt wird. Contoso bewertet die App-VMs mithilfe des Diensts [Azure Migrate](migrate-overview.md). Contoso bewertet die SQL Server-Datenbank der App mit dem [Datenmigrations-Assistenten](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Dieser Artikel
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale SmartHotel-App per Lift & Shift-Vorgang eine Migration zu Azure durch. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank zu einer verwalteten Azure SQL-Datenbank-Instanz, indem der [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) verwendet wird. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der SmartHotel-App mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Contoso migriert die SmartHotel-App. Contoso verwendet Site Recovery, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe von Site Recovery per Lift & Shift-Vorgang eine Migration seiner Linux-App „osTicket“ zu Azure-VMs durch. | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App auf Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert seine Linux-App „osTicket“ mithilfe von Site Recovery zu Azure-VMs. Die App-Datenbank wird zu Azure Database for MySQL migriert, indem MySQL Workbench verwendet wird. | Verfügbar
[Artikel 9: Umgestalten einer App in eine Azure-Web-App und in Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert seine SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App in eine Azure-Web-App und in Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert seine Linux-App „osTicket“ an mehreren Standorten zu einer Azure-Web-App. Die Web-App wird in GitHub integriert, um die kontinuierliche Bereitstellung (Continuous Delivery) zu ermöglichen. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von Team Foundation Server in Visual Studio Team Services](contoso-migration-tfs-vsts.md) | Contoso migriert seine lokale Team Foundation Server-Bereitstellung zu Visual Studio Team Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer App in einen Azure-Container und Azure SQL-Datenbank](contoso-migration-rearchitect-container-sql.md) | Contoso migriert seine SmartHotel-App zu Azure und strukturiert die App anschließend um. Contoso strukturiert die App-Webschicht in einen Windows-Container und die App-Datenbank mithilfe von Azure SQL-Datenbank um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt seine SmartHotel-App neu, indem verschiedene Azure-Funktionen und -Dienste verwendet werden, z.B. Azure App Service, Azure Kubernetes Service, Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar


## <a name="overview"></a>Übersicht

Contoso erwägt die Migration zu Azure und möchte anhand einer technischen und finanziellen Bewertung ermitteln, ob seine lokalen Workloads für die Migration zur Cloud geeignet sind. Das Contoso-Team möchte vor allem die Computer- und Datenbankkompatibilität für die Migration bewerten. Die Kapazität und die Kosten für die Ausführung der Ressourcen von Contoso in Azure sollen geschätzt werden.

Als Einstieg und zum besseren Verständnis der beteiligten Technologie bewertet Contoso zwei seiner lokalen Apps. Dies ist in der folgenden Tabelle zusammengefasst. Das Unternehmen führt eine Bewertung der Migrationsszenarien durch, in denen ein neuer Host zugewiesen wird und die Apps für die Migration umgestaltet werden. Weitere Informationen zum erneuten Hosten und Umgestalten finden Sie in der [Contoso-Migrationsübersicht](contoso-migration-overview.md).

App-Name | Plattform | App-Ebenen | Details
--- | --- | --- | ---
SmartHotel<br/><br/> (Verwaltung der Reiseanforderungen von Contoso) | Wird mit einer SQL Server-Datenbank unter Windows ausgeführt | App mit zwei Schichten. Die Front-End-ASP.NET-Website wird auf einer VM (**WEBVM**) und die SQL Server-Instanz auf einer anderen VM (**SQLVM**) ausgeführt. | VMs sind VMware, die auf einem mit vCenter Server verwalteten ESXi-Host ausgeführt werden.<br/><br/> Sie können die Beispiel-App von [GitHub](https://github.com/Microsoft/SmartHotel360) herunterladen.
osTicket<br/><br/> (Service Desk-App von Contoso) | Wird auf Linux/Apache mit MySQL PHP (LAMP) ausgeführt | App mit zwei Schichten. Eine Front-End-PHP-Website wird auf einer VM (**OSTICKETWEB**) und die MySQL-Datenbank auf einer anderen VM (**OSTICKETMYSQL**) ausgeführt. | Die App wird von Kundendienst-Apps verwendet, um Probleme für die internen Mitarbeitern und externen Kunden zu verfolgen.<br/><br/> Sie können das Beispiel von [GitHub](https://github.com/osTicket/osTicket) herunterladen.

## <a name="current-architecture"></a>Aktuelle Architektur

In diesem Diagramm ist die aktuelle lokale Infrastruktur von Contoso dargestellt:

![Aktuelle Contoso-Architektur](./media/contoso-migration-assessment/contoso-architecture.png)  

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
- **Effizienzsteigerung**: Contoso muss unnötige Verfahren entfernen und Prozesse für seine Entwickler und Benutzer optimieren. Die IT-Abteilung muss schnell sein und darf weder Geld noch Zeit verschwenden, damit die Kundenanforderungen in kürzerer Zeit erfüllt werden können.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Sie muss schneller reagieren können, als sich die Gegebenheiten des Markts ändern, damit das Unternehmen auf dem globalen Markt Erfolg hat. Die IT-Abteilung von Contoso darf keine Hindernisse in den Weg stellen und keine Geschäftsabläufe blockieren.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die IT-Abteilung von Contoso Systeme bereitstellen, die mit der gleichen Geschwindigkeit mitwachsen können.

## <a name="assessment-goals"></a>Bewertungsziele

Das Cloudteam von Contoso hat sich Ziele für die Migrationsbewertungen gesetzt:

- Nach der Migration sollten die Apps in Azure die gleichen Leistungsmerkmale aufweisen, über die sie derzeit in der lokalen VMware-Umgebung von Contoso verfügen. Die Umstellung auf die Cloud bedeutet nicht, dass die Leistung der App weniger wichtig ist.
- Contoso muss die Kompatibilität seiner Anwendungen und Datenbanken in Bezug auf die Azure-Anforderungen verstehen. Außerdem muss Contoso mit den Hostingoptionen in Azure vertraut sein.
- Die Datenbankverwaltung von Contoso sollte nach dem Verlagern von Apps in die Datenbank reduziert werden.  
- Contoso möchte nicht nur verstehen, welche Migrationsoptionen verfügbar sind, sondern auch über die Infrastrukturkosten nach der Verlagerung in die Cloud Bescheid wissen.

## <a name="assessment-tools"></a>Bewertungstools

Contoso nutzt Microsoft-Tools für seine Migrationsbewertung. Die Tools sind an den Zielen des Unternehmens ausgerichtet und sollten Contoso alle benötigten Informationen liefern.

Technologie | BESCHREIBUNG | Kosten
--- | --- | ---
[Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso nutzt den Datenmigrations-Assistenten, um Kompatibilitätsprobleme zu bewerten und zu erkennen, die ggf. die Datenbankfunktionalität des Unternehmens in Azure beeinträchtigen können. Mit dem Datenmigrations-Assistenten wird die Featureparität zwischen SQL-Quellen und -Zielen bewertet. Er stellt Empfehlungen zu Verbesserungen der Leistung und Zuverlässigkeit bereit. | Der Datenmigrations-Assistent ist ein kostenloses Tool, das heruntergeladen werden kann.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso nutzt den Azure Migrate-Dienst, um seine VMware-VMs zu bewerten. Azure Migrate bewertet die Eignung der Computer für die Migration. Der Dienst stellt Schätzungen zur Größe und zu den Kosten für die Ausführung in Azure bereit.  | Ab Mai 2018 ist Azure Migrate ein kostenloser Dienst.
[Dienstzuordnung](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Für Azure Migrate wird eine Dienstzuordnung verwendet, um Abhängigkeiten zwischen Computern anzuzeigen, die das Unternehmen migrieren möchte. | Die Dienstzuordnung ist Teil von Azure Log Analytics. Derzeit kann Contoso die Dienstzuordnung 180 Tage lang nutzen, ohne dass Gebühren anfallen.

In diesem Szenario lädt Contoso den Datenmigrations-Assistenten herunter und führt ihn aus, um die lokale SQL Server-Datenbank für seine Reise-App zu bewerten. Contoso verwendet Azure Migrate mit Zuordnung von Abhängigkeiten, um die App-VMs zu bewerten, bevor die Migration zu Azure migriert wird.

## <a name="assessment-architecture"></a>Architektur für die Bewertung

![Architektur für Migrationsbewertung](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso ist ein fiktiver Name für ein typisches Unternehmen.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) und lokale Domänencontroller (**CONTOSODC1**, **CONTOSODC2**).
- VMware-VMs befinden sich auf VMware ESXi-Hosts mit Version 6.5 (**contosohost1**, **contosohost2**).
- Die VMware-Umgebung wird per vCenter Server 6.5 verwaltet (**vcenter.contoso.com** auf einer VM).
- Die Reise-App SmartHotel weist die folgenden Merkmale auf:
    - Die App ist auf zwei VMware-VMs angeordnet (**WEBVM** und **SQLVM**).
    - Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com**.
    - Die VMs werden mit SP1 auf Windows Server 2008 R2 Datacenter ausgeführt.
- Die VMware-Umgebung wird mit einem vCenter Server (**vcenter.contoso.com**) verwaltet, der auf einer VM ausgeführt wird.
- Service Desk-App „osTicket“:
    - Die App ist auf zwei VMs angeordnet (**OSTICKETWEB** und **OSTICKETMYSQL**).
    - Auf den VMs wird Ubuntu Linux Server 16.04-LTS ausgeführt.
    - Auf **OSTICKETWEB** werden Apache 2 und PHP 7.0 ausgeführt.
    - Auf **OSTICKETMYSQL** wird MySQL 5.7.22 ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Contoso und andere Benutzer müssen für diese Bewertung die folgenden Voraussetzungen erfüllen:

- Berechtigungen „Besitzer“ oder „Mitwirkender“ für das Azure-Abonnement oder für eine Ressourcengruppe im Azure-Abonnement.
- Eine lokale vCenter Server-Instanz mit Version 6.5, 6.0 oder 5.5.
- Ein Konto mit Lesezugriff für vCenter Server bzw. Berechtigungen für die Kontoerstellung.
- Berechtigungen zum Erstellen einer VM auf der vCenter Server-Instanz mit einer OVA-Vorlage.
- Mindestens einen ESXi-Host mit Version 5.0 oder höher.
- Mindestens zwei lokale VMware-VMs (mit Ausführung einer SQL Server-Datenbank auf einer VM).
- Berechtigungen zum Installieren von Azure Migrate-Agents auf jeder VM.
- Die VMs sollten über eine direkte Internetverbindung verfügen.  
        - Sie können den Internetzugriff auf die [erforderlichen URLs](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) beschränken.  
        - Wenn Ihre VMs nicht über Internetkonnektivität verfügen, muss das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) von Azure Log Analytics darauf installiert sein, und Agent-Datenverkehr muss damit weitergeleitet werden.
- FQDN der VM, auf der die SQL Server-Instanz ausgeführt wird, für die Datenbankbewertung.
- Für die Windows Firewall auf der SQL Server-VM sollten externe Verbindungen über TCP-Port 1433 (Standard) zugelassen sein. Dieses Setup ermöglicht die Verbindungsherstellung für den Datenmigrations-Assistenten.

## <a name="assessment-overview"></a>Bewertungsübersicht

Hier ist angegeben, wie Contoso die Bewertung durchführt:

> [!div class="checklist"]
> * **Schritt 1: Herunterladen und Installieren des Datenmigrations-Assistenten**: Contoso bereitet den Datenmigrations-Assistenten auf die Bewertung der lokalen SQL Server-Datenbank vor.
> * **Schritt 2: Bewerten der Datenbank mit dem Datenmigrations-Assistenten**: Contoso führt die Datenbankbewertung durch und analysiert sie.
> * **Schritt 3: Vorbereiten der VM-Bewertung mit Azure Migrate**: Contoso richtet die lokalen Konten ein und passt die VMware-Einstellungen an.
> * **Schritt 4: Ermitteln der lokalen VMs mit Azure Migrate**: Contoso erstellt eine Azure Migrate-Collector-VM. Anschließend führt Contoso den Collector aus, um die VMs für die Bewertung zu ermitteln.
> * **Schritt 5: Vorbereiten der Abhängigkeitsanalyse mit Azure Migrate**: Contoso installiert Azure Migrate-Agents auf den VMs, damit das Unternehmen die Zuordnung der Abhängigkeiten zwischen VMs anzeigen kann.
> * **Schritt 6: Bewerten der VMs mit Azure Migrate**: Contoso überprüft die Abhängigkeiten, gruppiert die VMs und führt die Bewertung durch. Nach Abschluss der Bewertung wird sie von Contoso als Vorbereitung auf die Migration analysiert.

## <a name="step-1-download-and-install-data-migration-assistant"></a>Schritt 1: Herunterladen und Installieren des Datenmigrations-Assistenten

1. Contoso lädt den Datenmigrations-Assistenten über das [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) herunter.
    - Der Datenmigrations-Assistent kann auf jedem Computer installiert werden, mit dem eine Verbindung mit der SQL Server-Instanz hergestellt werden kann. Es ist nicht erforderlich, dass Contoso ihn auf dem SQL Server-Computer ausführt.
    - Der Datenmigrations-Assistent sollte nicht auf dem SQL Server-Hostcomputer ausgeführt werden.
2. Contoso führt die heruntergeladene Setupdatei (DownloadMigrationAssistant.msi) aus, um die Installation zu starten.
3. Auf der Seite **Fertigstellen** wählt Contoso die Option **Launch Microsoft Data Migration Assistant** (Microsoft-Datenmigrations-Assistenten starten) aus, bevor der Assistent beendet wird.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Schritt 2: Ausführen und Analysieren der Datenbankbewertung für SmartHotel

Contoso kann jetzt eine Bewertung durchführen, um seine lokale SQL Server-Datenbank für die SmartHotel-App zu analysieren.

1. Im Datenmigrations-Assistenten wählt Contoso **Neu** > **Bewertung** und vergibt für die Bewertung dann einen Projektnamen (**SmartHotel**).
2. Unter **Quellservertyp** wählt Contoso die Option **SQL Server auf Azure-VMs**.

    ![Datenmigrations-Assistent – Auswählen der Quelle](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Derzeit unterstützt der Datenmigrations-Assistent die Bewertung für die Migration einer verwalteten Azure SQL-Datenbank-Instanz nicht. Als Problemumgehung verwendet Contoso SQL Server auf einer Azure-VM als angenommenes Ziel für die Bewertung.

3. Unter **Zielversion auswählen** wählt Contoso „SQL Server 2017“ als Zielversion aus. Contoso muss diese Version auswählen, weil sie von der verwalteten SQL-Datenbank-Instanz verwendet wird.
4. Contoso wählt Berichte aus, um Informationen zur Kompatibilität und zu neuen Features zu erhalten:
    - Unter **Kompatibilitätsprobleme** werden Informationen zu Änderungen angezeigt, die die Migration ggf. verhindern oder für die vor der Migration eine kleinere Anpassung erforderlich ist. Mit diesem Bericht erhält Contoso Informationen zu allen derzeit verwendeten Features, die als veraltet eingestuft wurden. Die Probleme sind nach Kompatibilitätsgrad angegeben.
    - Unter **Empfehlung zu neuen Features** werden Informationen zu neuen Features auf der SQL Server-Zielplattform angezeigt, die nach der Migration für die Datenbank verwendet werden können. Empfehlungen zu neuen Features befinden sich unter den Überschriften **Leistung**, **Sicherheit** und **Speicher**.

    ![Datenmigrations-Assistent – Kompatibilitätsprobleme und neue Features](./media/contoso-migration-assessment/dma-assessment-2.png)

2. Unter **Mit Server verbinden** gibt Contoso den Namen der VM, auf der die Datenbank ausgeführt wird, und die Anmeldeinformationen für den Zugriff auf diese VM an. Contoso wählt die Option **Serverzertifikat vertrauen** aus, um sicherzustellen, dass die VM auf SQL Server zugreifen kann. Anschließend wählt Contoso die Option **Verbinden**.

    ![Datenmigrations-Assistent – Mit Server verbinden](./media/contoso-migration-assessment/dma-assessment-3.png)

3. Unter **Quelle hinzufügen** fügt Contoso die Datenbank hinzu, die bewertet werden soll, und wählt dann **Weiter**, um die Bewertung zu starten.
4. Die Bewertung wird erstellt.

    ![Datenmigrations-Assistent – Erstellen der Bewertung](./media/contoso-migration-assessment/dma-assessment-4.png)

5. Unter **Ergebnisse überprüfen** zeigt Contoso die Ergebnisse der Bewertung an.

### <a name="analyze-the-database-assessment"></a>Analysieren der Datenbankbewertung

Die Ergebnisse werden angezeigt, sobald sie verfügbar sind. Wenn Contoso Probleme behebt, muss die Option **Bewertung neu starten** gewählt werden, um die Bewertung erneut durchzuführen.

1. Im Bericht **Kompatibilitätsprobleme** überprüft Contoso mögliche Probleme für jeden Kompatibilitätsgrad. Kompatibilitätsgrade sind den SQL Server-Versionen wie folgt zugeordnet:

    - 100: SQL Server 2008/Azure SQL-Datenbank
    - 110: SQL Server 2012/Azure SQL-Datenbank
    - 120: SQL Server 2014/Azure SQL-Datenbank
    - 130: SQL Server 2016/Azure SQL-Datenbank
    - 140: SQL Server 2017/Azure SQL-Datenbank

    ![Datenmigrations-Assistent – Bericht zu Kompatibilitätsproblemen](./media/contoso-migration-assessment/dma-assessment-5.png)

2. Im Bericht **Featureempfehlungen** zeigt Contoso die Leistungs-, Sicherheits- und Speicherfeatures an, die von der Bewertung nach der Migration empfohlen werden. Es werden verschiedene Features empfohlen, z.B. In-Memory-OLTP, Columnstore-Indizes, Stretch Database, Always Encrypted, dynamische Datenmaskierung und Transparent Data Encryption.

    ![Datenmigrations-Assistent – Bericht mit Featureempfehlungen](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Contoso sollte [Transparent Data Encryption für alle SQL Server-Datenbanken aktivieren](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017). Dies ist noch wichtiger, wenn sich eine Datenbank in der Cloud befindet, als wenn sie lokal gehostet wird. Transparent Data Encryption sollte erst nach der Migration aktiviert werden. Wenn Transparent Data Encryption bereits aktiviert ist, muss Contoso das Zertifikat bzw. den asymmetrischen Schlüssel in die Masterdatenbank des Zielservers verschieben. Erfahren Sie, wie Sie [eine per Transparent Data Encryption geschützte Datenbank auf eine andere SQL Server-Instanz verschieben](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Contoso kann die Bewertung im JSON- oder CSV-Format exportieren.

> [!NOTE]
> Gehen Sie für umfangreichere Bewertungen wie folgt vor:
> - Führen Sie mehrere Bewertungen gleichzeitig durch, und zeigen Sie den Status der Bewertungen auf der Seite **Alle Bewertungen** an.
> - Fassen Sie die Bewertungen in einer [SQL Server-Datenbank](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database) zusammen.
> - Fassen Sie die Bewertungen in einem [Power BI-Bericht](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017) zusammen.

## <a name="step-3-prepare-for-vm-assessment-by-using-azure-migrate"></a>Schritt 3: Vorbereiten der VM-Bewertung mit Azure Migrate

Contoso muss ein VMware-Konto erstellen, das von Azure Migrate zum automatischen Ermitteln von VMs für die Bewertung verwendet werden kann. Außerdem muss das Unternehmen überprüfen, ob es über Rechte zum Erstellen einer VM verfügt, die Ports notieren, die geöffnet sein müssen, und die Einstellungsebene für Statistiken festlegen.

### <a name="set-up-a-vmware-account"></a>Einrichten eines VMware-Kontos

Für die VM-Ermittlung ist ein Konto mit Lesezugriff in vCenter Server mit den folgenden Eigenschaften erforderlich:

- **Benutzertyp**: Mindestens ein Benutzer mit Lesezugriff.
- **Berechtigungen**: Aktivieren Sie für das Rechenzentrumobjekt das Kontrollkästchen **Propagate to Child Objects** (An untergeordnete Objekte weitergeben). Wählen Sie für **Rolle** die Option **Schreibgeschützt**.
- **Details**: Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat Zugriff auf alle Objekte im Rechenzentrum.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (An untergeordnetes Objekt weitergeben) zu.

### <a name="verify-permissions-to-create-a-vm"></a>Überprüfen der Berechtigungen für die Erstellung einer VM

Contoso vergewissert sich, dass es über Berechtigungen zum Erstellen einer VM per Import einer Datei im OVA-Format verfügt. Informieren Sie sich, wie Sie [eine Rolle mit Berechtigungen erstellen und zuweisen](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Überprüfen der Ports

Die Contoso-Bewertung verwendet Abhängigkeitszuordnung. Für die Abhängigkeitszuordnung muss ein Agent auf VMs installiert werden, auf die zugegriffen wird. Der Agent muss über den TCP-Port 443 jeder VM jeweils eine Verbindung mit Azure herstellen können. Informieren Sie sich über die [Verbindungsanforderungen](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).

### <a name="set-statistics-settings"></a>Festlegen der Statistikeinstellungen

Bevor Contoso mit der Bereitstellung beginnt, muss das Unternehmen die Statistikeinstellungen für vCenter Server auf Ebene 3 festlegen. 

> [!NOTE]
> - Nach dem Festlegen der Ebene muss Contoso mindestens einen Tag warten, bevor die Bewertung durchgeführt wird. Andernfalls funktioniert die Bewertung ggf. nicht wie erwartet.
> - Bei einer höheren Ebene als 3 funktioniert die Bewertung zwar, aber es sollte Folgendes beachtet werden:
>    - Die Leistungsdaten für Datenträger und das Netzwerk werden nicht erfasst.
>    - Für den Speicher wird für Azure Migrate ein Standarddatenträger in Azure empfohlen, der die gleiche Größe wie der lokale Datenträger hat.
>    - In Bezug auf das Netzwerk wird für jeden lokalen Netzwerkadapter die Verwendung eines entsprechenden Netzwerkadapters in Azure empfohlen.
>    - Für den Computebereich untersucht Azure Migrate die VM-Kerne und die Arbeitsspeichergröße und empfiehlt eine Azure-VM mit der gleichen Konfiguration. Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.
> - Weitere Informationen zum Festlegen der Größe über Ebene 3 finden Sie unter [Festlegen der Größe](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Legen Sie die Ebene wie folgt fest:

1. Auf dem vSphere-Webclient öffnet Contoso die vCenter Server-Instanz.
2. Contoso wählt **Verwalten** > **Einstellungen** > **Allgemein** > **Bearbeiten**.
3. Unter **Statistiken** legt Contoso die Einstellungen für die Statistikebene auf **Ebene 3** fest.

    ![vCenter Server-Statistikebene](./media/contoso-migration-assessment/vcenter-statistics-level.png)

## <a name="step-4-discover-vms"></a>Schritt 4: Ermitteln von VMs

Zum Ermitteln von VMs erstellt Contoso ein Azure Migrate-Projekt. Contoso lädt die Collector-VM herunter und richtet sie ein. Anschließend führt Contoso den Collector aus, um die zugehörigen lokalen VMs zu ermitteln.

### <a name="create-a-project"></a>Erstellen eines Projekts

1. Im [Azure-Portal](https://portal.azure.com) sucht Contoso nach **Azure Migrate**. Anschließend erstellt Contoso ein Projekt.
2. Contoso gibt einen Projektnamen (**ContosoMigration**) und das Azure-Abonnement an. Das Unternehmen erstellt eine neue Azure-Ressourcengruppe (**ContosoFailoverRG**). 
    > [!NOTE]
    > - Sie können ein Azure Migrate-Projekt nur in der Region „USA, Westen-Mitte“ oder „USA, Osten“ erstellen.
    > - Sie können eine Migration für einen beliebigen Zielstandort planen.
    > - Der Projektstandort wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

    ![Azure Migrate – Erstellen eines Migrationsprojekts](./media/contoso-migration-assessment/project-1.png)

### <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als *Collectorappliance* bezeichnet wird. Der virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen VMs an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance lädt Contoso eine OVA-Vorlage herunter und importiert sie dann auf der lokalen vCenter Server-Instanz, um die VM zu erstellen.

1. Im Azure Migrate-Projekt wählt Contoso **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**. Contoso lädt die OVA-Vorlagendatei herunter.
2. Contoso kopiert die Projekt-ID und den Schlüssel. Das Projekt und die ID werden zum Konfigurieren des Collectors benötigt.

    ![Azure Migrate – Herunterladen der OVA-Datei](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Vor der Bereitstellung der VM überprüft Contoso, ob die OVA-Datei sicher ist:

1. Auf dem Computer, auf dem die Datei heruntergeladen wurde, öffnet Contoso ein Eingabeaufforderungsfenster mit Administratorrechten.
2. Contoso führt den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    
    **Beispiel** 
    
    ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss den folgenden Einstellungen entsprechen (Version 1.0.9.12):

    Algorithmus | Hashwert
    --- | ---
    MD5 | d0363e5d1b377a8eb08843cf034ac28a
    SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
    SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Erstellen der Collectorappliance

Jetzt kann Contoso die heruntergeladene Datei auf die vCenter Server-Instanz importieren und die Collectorappliance-VM bereitstellen:

1. In der Konsole des vSphere-Clients wählt Contoso **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![vSphere-Webclient – Bereitstellen der OVF-Vorlage](./media/contoso-migration-assessment/vcenter-wizard.png)

2. Im Assistenten für die Bereitstellung der OVF-Vorlage wählt Contoso die Option **Quelle** und gibt dann den Speicherort der OVA-Datei an.
3. Unter **Name and Location** (Name und Speicherort) gibt Contoso einen Anzeigenamen für die Collector-VM an. Anschließend wählt das Unternehmen den Bestandsspeicherort aus, unter dem die VM gehostet werden soll. Außerdem gibt Contoso den Host bzw. den Cluster an, auf bzw. in dem die Collectorappliance ausgeführt werden soll.
4. Unter **Speicher** gibt Contoso den Speicherort an. Unter **Datenträgerformat** wählt Contoso aus, wie der Speicher bereitgestellt werden soll.
5. Unter **Netzwerkzuordnung** gibt Contoso das Netzwerk an, über das die Collector-VM verbunden werden soll. Das Netzwerk benötigt Internetkonnektivität, um Metadaten an Azure zu senden.
6. Contoso überprüft die Einstellungen und wählt dann **Power on after deployment** > **Finish** (Nach Bereitstellung einschalten > Fertig stellen). Wenn die Appliance erstellt wird, wird eine Meldung mit dem Hinweis angezeigt, dass der Vorgang erfolgreich abgeschlossen wurde.

### <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

Contoso führt jetzt den Collector aus, um VMs zu ermitteln. Derzeit unterstützt der Collector nur **Englisch (USA)** als Sprache des Betriebssystems und der Collectoroberfläche.

1. In der Konsole des vSphere-Clients wählt Contoso die Option **Konsole öffnen**. Contoso gibt für die Collector-VM die Einstellungen für Sprache, Zeitzone und Kennwort an.
2. Auf dem Desktop wählt Contoso die Verknüpfung **Run collector** (Collector ausführen).

    ![Konsole des vSphere-Clients – Collectorverknüpfung](./media/contoso-migration-assessment/collector-shortcut.png)

3. Im Azure Migrate-Collector wählt Contoso die Option **Erforderliche Komponenten einrichten**. Contoso akzeptiert die Lizenzbedingungen und liest die Drittanbieterinformationen.
4. Der Collector überprüft, ob die VM über Internetzugriff verfügt, die Uhrzeit synchronisiert ist und der Collectordienst ausgeführt wird. (Der Collectordienst wird standardmäßig auf der VM installiert.) Außerdem installiert Contoso die VMware PowerCLI.

    > [!NOTE]
    > Es wird vorausgesetzt, dass die VM über direkten Internetzugriff ohne Proxy verfügt.

    ![Azure Migrate-Collector – Überprüfen der erforderlichen Komponenten](./media/contoso-migration-assessment/collector-verify-prereqs.png)

5. Unter **vCenter Server-Details angeben** gibt Contoso den Namen (FQDN) oder die IP-Adresse der vCenter Server-Instanz und die schreibgeschützten Anmeldeinformationen für die Ermittlung ein.
6. Contoso wählt einen Bereich für die VM-Ermittlung aus. Der Collector kann nur virtuelle Computer ermitteln, die sich innerhalb des angegebenen Bereichs befinden. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Der Bereich sollte nicht mehr als 1.500 VMs enthalten.

    ![vCenter Server-Details angeben](./media/contoso-migration-assessment/collector-connect-vcenter.png)

7. Unter **Migrationsprojekt angeben** gibt Contoso die Angaben für die ID und den Schlüssel für das Azure Migrate-Projekt ein, die im Portal kopiert wurden. Um die ID und den Schlüssel für das Projekt zu erhalten, kann Contoso für das Projekt zur Seite **Übersicht** und dann zu **Computer ermitteln** navigieren.  

    ![Festlegen des Migrationsprojekts](./media/contoso-migration-assessment/collector-connect-azure.png)

8. Contoso kann unter **Sammlungsfortschritt anzeigen** die Ermittlung überwachen und sich vergewissern, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.

    ![Sammlungsfortschritt anzeigen](./media/contoso-migration-assessment/collector-collection-process.png)

### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nachdem der Collectorvorgang abgeschlossen wurde, überprüft Contoso, ob die VMs im Portal angezeigt werden:

1. Im Azure Migrate-Projekt wählt Contoso **Verwalten** > **Computer**. Contoso überprüft, ob die zu ermittelnden VMs angezeigt werden.

    ![Azure Migrate – Ermittelte Computer](./media/contoso-migration-assessment/discovery-complete.png)

2. Derzeit sind auf den Computern keine Azure Migrate-Agents installiert. Contoso muss die Agents installieren, um die Abhängigkeiten anzuzeigen.

    ![Azure Migrate – Agent-Installation erforderlich](./media/contoso-migration-assessment/machines-no-agent.png)

## <a name="step-5-prepare-for-dependency-analysis"></a>Schritt 5: Vorbereiten der Abhängigkeitsanalyse

Zum Anzeigen von Abhängigkeiten zwischen VMs, die bewertet werden sollen, lädt Contoso die Agents herunter und installiert sie auf den App-VMs. Contoso installiert für seine Apps Agents auf allen VMs – sowohl für Windows als auch für Linux.

### <a name="take-a-snapshot"></a>Erstellen einer Momentaufnahme

Contoso erstellt vor der Installation der Agents eine Momentaufnahme, damit eine Kopie der VMs vorhanden ist, bevor sie geändert werden.

![Momentaufnahme eines Computers](./media/contoso-migration-assessment/snapshot-vm.png)

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents

1. Unter **Computer** wählt Contoso den Computer aus. In der Spalte **Abhängigkeiten** wählt Contoso die Option **Installation erforderlich**.
2. Im Bereich **Computer ermitteln**:
    - Der Microsoft Monitoring Agent (MMA) und der Abhängigkeits-Agent werden für jede Windows-VM heruntergeladen.
    - Der MMA und der Abhängigkeits-Agent werden für jede Linux-VM heruntergeladen.
3. Contoso kopiert die ID und den Schlüssel des Arbeitsbereichs. Contoso benötigt für die MMA-Installation die Arbeitsbereichs-ID und den zugehörigen Schlüssel.

    ![Agent-Download](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Installieren der Agents auf den Windows-VMs

Contoso führt die Installation auf jeder VM durch.

#### <a name="install-the-mma-on-windows-vms"></a>Installieren des MMA auf den Windows-VMs

1. Contoso doppelklickt auf den heruntergeladenen Agent.
2. Unter **Zielordner** behält Contoso den Standardordner für die Installation bei und wählt dann **Weiter**.
3. Unter **Agent-Setupoptionen** wählt Contoso **Connect the agent to Azure Log Analytics** (Agent mit Azure Log Analytics verbinden) > **Weiter**.

    ![Microsoft Monitoring Agent-Setup – Agent-Setupoptionen](./media/contoso-migration-assessment/mma-install.png)

4. Unter **Azure Log Analytics** fügt Contoso die Angaben zur Arbeitsbereichs-ID und zum zugehörigen Schlüssel ein, die im Portal kopiert wurden.

    ![Microsoft Monitoring Agent-Setup – Azure Log Analytics](./media/contoso-migration-assessment/mma-install2.png)

5. Unter **Bereit zur Installation** installiert Contoso den MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Installieren des Abhängigkeits-Agents unter Windows-VMs

1. Contoso doppelklickt auf den heruntergeladenen Abhängigkeits-Agent.
2. Contoso akzeptiert die Lizenzbedingungen und wartet, bis die Installation abgeschlossen wurde.

    ![Setup des Abhängigkeits-Agents – Installation](./media/contoso-migration-assessment/dependency-agent.png)

### <a name="install-the-agents-on-linux-vms"></a>Installieren des Agents auf Linux-VMs

Contoso führt die Installation auf jeder VM durch.

#### <a name="install-the-mma-on-linux-vms"></a>Installieren des MMA auf Linux-VMs

1. Contoso installiert die Python-Bibliothek „ctypes“ auf jeder VM mit dem folgenden Befehl:

    `sudo apt-get install python-ctypeslib`
2. Contoso muss den Befehl zur Installation des MMA-Agents als Root-Benutzer ausführen. Um zum Root-Benutzer zu werden, führt Contoso den folgenden Befehl aus und gibt dann das Kennwort für den Root-Benutzer ein:

    `sudo -i`
3. Contoso installiert den MMA:
    - Contoso gibt die Arbeitsbereichs-ID und den Schlüssel für den Befehl ein.
    - Die Befehle sind für 64-Bit.
    - Die Arbeitsbereichs-ID und der Primärschlüssel sind im Portal der Microsoft Operations Management Suite (OMS) angegeben. Wählen Sie **Einstellungen** und dann die Registerkarte **Verbundene Quellen**.
    - Führen Sie die folgenden Befehle aus, um den OMS-Agent herunterzuladen, die Prüfsumme zu überprüfen und den Agent anschließend zu installieren und das Onboarding dafür durchzuführen:

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```

#### <a name="install-the-dependency-agent-on-linux-vms"></a>Installieren des Abhängigkeits-Agents auf Linux-VMs

Nach der MMA-Installation führt Contoso die Installation des Abhängigkeits-Agents auf den Linux-VMs durch:

1. Der Abhängigkeits-Agent wird mit „InstallDependencyAgent-Linux64.bin“ auf den Linux-Computern installiert. Hierbei handelt es sich um ein Shellskript mit einer selbstextrahierenden Binärdatei. Contoso führt die Datei mit „sh“ aus oder fügt der Datei selbst Ausführungsberechtigungen hinzu.

2. Contoso installiert den Abhängigkeits-Agent für Linux als Root-Benutzer:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```

## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Schritt 6: Durchführen und Analysieren der VM-Bewertung

Contoso kann jetzt die Abhängigkeiten der Computer überprüfen, und eine Gruppe erstellen. Anschließend wird die Bewertung für die Gruppe durchgeführt.

### <a name="verify-dependencies-and-create-a-group"></a>Überprüfen von Abhängigkeiten und Erstellen einer Gruppe

1. Contoso wählt die Option **Abhängigkeiten anzeigen**, um zu ermitteln, welche Computer analysiert werden sollen.

    ![Azure Migrate – Anzeigen von Computerabhängigkeiten](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Für SQLVM werden in der Abhängigkeitsübersicht die folgenden Informationen angezeigt:

    - Prozessgruppen bzw. Prozesse mit aktiven Netzwerkverbindungen auf der SQLVM für den angegebenen Zeitraum (standardmäßig eine Stunde)
    - Eingehende (Client) und ausgehende TCP-Verbindungen (Server) zu bzw. von allen abhängigen Computern.
    - Abhängige Computer mit installierten Azure Migrate-Agents werden als separate Felder angezeigt
    - Für Computer ohne installierte Agents werden Informationen zum Port und zur IP-Adresse angezeigt

3. Für Computer, auf denen der Agent installiert ist (WEBVM), wählt Contoso das Feld für den entsprechenden Computer aus, um weitere Informationen anzuzeigen. Diese Informationen umfassen den FQDN, das Betriebssystem und die MAC-Adresse.

    ![Azure Migrate – Anzeigen der Gruppenabhängigkeiten](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Contoso wählt die VMs aus, die der Gruppe hinzugefügt werden sollen (SQLVM und WEBVM). Contoso hält beim Klicken die STRG-TASTE gedrückt, um mehrere VMs auszuwählen.
5. Contoso wählt **Gruppe erstellen** und gibt dann einen Namen ein (**smarthotelapp**).

    > [!NOTE]
    > Sie können den Zeitbereich erweitern, um detailliertere Abhängigkeiten anzuzeigen. Sie können eine bestimmte Dauer oder das Start- und Enddatum auswählen.

### <a name="run-an-assessment"></a>Durchführen einer Bewertung

1. Unter **Gruppen** öffnet Contoso die Gruppe (**smarthotelapp**) und wählt dann die Option **Bewertung erstellen**.

    ![Azure Migrate – Erstellen einer Bewertung](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Um die Bewertung anzuzeigen, wählt Contoso **Verwalten** > **Bewertungen**.

Contoso verwendet die Standardeinstellungen für Bewertungen, aber Sie können die [Einstellungen auch anpassen](how-to-modify-assessment.md).

### <a name="analyze-the-vm-assessment"></a>Analysieren der VM-Bewertung

Eine Azure Migrate-Bewertung enthält Informationen dazu, ob die lokalen VMs mit Azure kompatibel sind, welche Größenanpassung für die Azure-VM richtig ist und welche geschätzten monatlichen Azure-Kosten anfallen.

![Azure Migrate – Bewertungsbericht](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

![Azure Migrate – Bewertungsanzeige](./media/contoso-migration-assessment/assessment-display.png)

Eine Bewertung verfügt über eine Zuverlässigkeitsstufe, die zwischen 1 und 5 Sternen liegt (1 Stern ist die niedrigste und 5 Sterne die höchste Stufe).
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand dieses Werts können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Zuverlässigkeitsstufen sind nützlich, wenn Sie die *leistungsbasierte Größenanpassung* verwenden. Unter Umständen verfügt Azure Migrate nicht über genügend Datenpunkte für die nutzungsbasierte Größenanpassung. Bei der Größenanpassung vom Typ *Wie lokal* wird die Zuverlässigkeit immer mit 5 Sternen bewertet, da Azure Migrate über alle Datenpunkte verfügt, die zum Bestimmen der VM-Größe erforderlich sind.
- Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent):

   Verfügbarkeit von Datenpunkten | Zuverlässigkeitsstufe
   --- | ---
   0 % bis 20 % | 1 Stern
   21 % bis 40 % | 2 Sterne
   41 % bis 60 % | 3 Sterne
   61 % bis 80 % | 4 Sterne
   81 % bis 100 % | 5 Sterne

#### <a name="verify-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

![Azure Migrate – Bewertungsbereitschaft](./media/contoso-migration-assessment/azure-readiness.png)  

Im Bewertungsbericht sind die in der Tabelle zusammengefassten Informationen enthalten. Azure Migrate benötigt die folgenden Informationen, um die leistungsbasierte Größenanpassung anzeigen zu können. Falls die Informationen nicht erfasst werden können, ist die Bewertung der Größenanpassung ggf. nicht genau.

- Nutzungsdaten für CPU und Arbeitsspeicher.
- Lese/Schreib-IOPS-Wert und Durchsatz für jeden Datenträger, der an die VM angefügt ist.
- Netzwerk-E/A-Informationen für jeden Netzwerkadapter, der an die VM angefügt ist.

Einstellung | Anzeige | Details
--- | --- | ---
**Azure-VM-Bereitschaft** | Gibt an, ob die VM für die Migration bereit ist. | Mögliche Status:</br><br/>- Bereit für Azure<br/><br/>- Bereit mit Bedingungen <br/><br/>- Nicht bereit für Azure<br/><br/>- Bereitschaft unbekannt<br/><br/> Wenn eine VM nicht bereit ist, werden in Azure Migrate einige Lösungsschritte angezeigt.
**Azure-VM-Größe** | Für VMs, die bereit sind, stellt Azure Migrate eine Empfehlung zur Größe der Azure-VM bereit. | Die Empfehlung für die Größenanpassung richtet sich nach den Bewertungseigenschaften:<br/><br/>- Wenn Sie die leistungsbasierte Größenanpassung verwendet haben, wird bei der Größenanpassung der Leistungsverlauf der VMs berücksichtigt.<br/><br/>- Wenn Sie die Größenanpassung vom Typ *Wie lokal* verwendet haben, basiert die Größenanpassung auf der Größe der lokalen VM, und es werden keine Nutzungsdaten verwendet.
**Vorgeschlagenes Tool** | Da auf den Azure-Computern die Agents ausgeführt werden, prüft Azure Migrate die Prozesse, die im Computer ausgeführt werden. Es wird ermittelt, ob der Computer ein Datenbankcomputer ist.
**VM-Informationen** | Im Bericht werden die Einstellungen für die lokale VM angezeigt, z.B. Betriebssystem, Starttyp und Informationen zu Datenträger und Speicher.

#### <a name="review-monthly-cost-estimates"></a>Überprüfen der geschätzten monatlichen Kosten

In dieser Ansicht werden die Compute- und Speichergesamtkosten für die Ausführung der VMs in Azure angezeigt. Außerdem werden für jeden Computer die Details angezeigt.

![Azure Migrate – Azure-Kosten](./media/contoso-migration-assessment/azure-costs.png)

- Kostenschätzungen werden anhand der Größenempfehlungen für einen Computer berechnet.
- Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert.

## <a name="clean-up-after-assessment"></a>Bereinigen nach der Bewertung

- Nachdem die Bewertung abgeschlossen ist, behält Contoso die Azure Migrate-Appliance für zukünftige Auswertungen bei.
- Contoso deaktiviert die VMware-VM. Contoso nutzt sie dann erneut, wenn weitere VMs ausgewertet werden.
- Contoso behält das Projekt **Contoso-Migration** in Azure bei. Das Projekt wird derzeit in der Ressourcengruppe **ContosoFailoverRG** in der Azure-Region „USA Osten“ bereitgestellt.
-  Für die Collector-VM gilt eine 180-Tage-Evaluierungslizenz. Wenn dieser Zeitraum abgelaufen ist, muss Contoso den Collector herunterladen und wieder einrichten.

## <a name="conclusion"></a>Zusammenfassung

In diesem Szenario bewertet Contoso seine SmartHotel-App-Datenbank mit dem Tool für die Bewertung der Datenmigration. Das Unternehmen bewertet die lokalen VMs, indem es den Azure Migrate-Dienst verwendet. Contoso überprüft die Bewertungen, um sicherzustellen, dass die lokalen Ressourcen für die Migration zu Azure bereit sind.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel dieser Serie weist Contoso für die SmartHotel-App in Azure einen neuen Host zu, indem die Lift & Shift-Migration verwendet wird. Contoso migriert die Front-End-WEBVM für die App per Azure Site Recovery. Das Unternehmen migriert die App-Datenbank zu einer verwalteten Azure SQL-Datenbank-Instanz, indem der Database Migration Service verwendet wird. [Erste Schritte](contoso-migration-rehost-vm-sql-managed-instance.md) mit dieser Bereitstellung.

---
title: Bewerten von lokalen Workloads für die Contoso-Migration zu Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Contoso die lokalen Computer für die Migration zu Azure mit Azure Migration und Database Migration bewertet
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: fa6fb4ffe1eea98392b2199f379431b0dffc6774
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006565"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Contoso-Migration: Bewerten von lokalen Workloads für die Migration zu Azure

In diesem Artikel erfahren Sie, wie Contoso die lokale SmartHotel-App als Vorbereitung für die Migration zu Azure bewertet.

Dieses Dokument ist das dritte in einer Reihe von Artikeln, die dokumentieren, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und eine Reihe von Bereitstellungsszenarios, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten, und verschiedene Migrationstypen durchführen. Die Szenarios werden an Komplexität zunehmen, und mit der Zeit werden zusätzliche Artikel hinzugefügt.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Migrationsartikel wird die gleiche Infrastruktur verwendet. | Verfügbar
Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure  | Zeigt, wie Contoso eine Bewertung einer lokalen, zweischichtigen App SmartHotel auf einer VMware ausführt. Contoso bewertet App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Migrations-Assistenten von Azure Database](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Dieser Artikel
[Artikel 4: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und einer verwalteten SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso eine Migration zu Azure für die lokale App SmartHotel per Lift & Shift ausführt. Contoso migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und die App-Datenbank mithilfe des [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten SQL-Instanz. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel zu Azure-VMs mit dem Site Recovery-Dienst migriert. | Verfügbar
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und zur SQL Server Always On-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Contoso verwendet Site Recovery, um die App-VMs zu migrieren, und den Database Migration Service, um die App-Datenbank zu einem SQL Servercluster zu migrieren, das durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso mithilfe von Site Recovery eine Migration der Linux-App osTicket per Lift & Shift zu Azure-VMs durchführt | Verfügbar
[Artikel 8: Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osTicket mithilfe von Site Recovery zu Azure-VMs und die App-Datenbank mithilfe von MySQL Workbench zu einer Azure MySQL Server-Instanz migriert. | Verfügbar
[Artikel 9: Umgestalten einer App zu einer Azure-Web-App und einer Azure SQL-Datenbank-Instanz](contoso-migration-refactor-web-app-sql.md) | Zeigt, wie Contoso die SmartHotel-App zu einer Azure-Web-App und die App-Datenbank zu einer Azure SQL Server-Instanz migriert. | Verfügbar
[Artikel 10: Umgestalten einer Linux-App zu einer Azure-Web-App und einer Azure Database for MySQL-Instanz](contoso-migration-refactor-linux-app-service-mysql.md) | Zeigt, wie Contoso die Linux-osTicket-App zu Azure-Web-Apps auf mehreren Websites migriert, die in GitHub für Continuous Delivery integriert sind. Es migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von TFS in VSTS](contoso-migration-tfs-vsts.md) | Zeigt, wie Contoso seine lokale Team Foundation Server-Bereitstellung (TFS) durch Migration zu Visual Studio Team Services (VSTS) in Azure migriert. | Verfügbar
[Artikel 12: Umstrukturieren einer App zu einem Azure-Container und einer Azure SQL-Datenbank-Instanz](contoso-migration-rearchitect-container-sql.md) | Zeigt, wie Contoso seine SmartHotel-App zu Azure migriert und in Azure umstrukturiert. Es strukturiert die App-Webebene in einen Windows-Container und die App-Datenbank in eine Azure SQL-Datenbank-Instanz um. | Verfügbar
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Zeigt, wie Contoso seine SmartHotel-App mit einer Reihe von Azure-Funktionen und -Diensten wie App Service, Azure Kubernetes, Azure Functions, Cognitive Services und Cosmos DB neu erstellt. | Verfügbar


## <a name="overview"></a>Übersicht

Das Unternehmen Contoso erwägt die Migration zu Azure und möchte anhand einer technischen und finanziellen Bewertung ermitteln, ob seine lokalen Workloads für die Migration zur Cloud geeignet sind. Insbesondere will das Contoso-Team die Kompatibilität von Computern und Datenbanken für die Migration bewerten, und es sollen die Kapazität und die Kosten zum Ausführen der Ressourcen in Azure geschätzt werden.

Um zu Anfang die verwendeten Technologien besser kennenzulernen werden sie zwei der lokalen Apps bewerten, was in der folgenden Tabelle zusammengefasst wird. Beachten Sie, dass sie Migrationsszenarios bewerten, die Migrations-Apps erneut hosten und umgestalten. Weitere Informationen zum erneuten Hosten und Umgestalten finden Sie in der [Contoso-Migrationsübersicht](contoso-migration-overview.md).

**App-Name** | **Plattform** | **Anwendungsebenen** | **Details**
--- | --- | --- | ---
SmartHotel<br/><br/> Verwaltet Reiseanforderungen von Contoso | Wird mit einer SQL Server-Datenbank unter Windows ausgeführt | App auf zwei Ebenen, bei der die Front-End-ASP.NET-Website auf einer VM (WEBVM) und der SQL Server auf einer anderen VM (SQLVM) ausgeführt werden | VMs sind VMware, die auf einem von vCenter Server verwalteten ESXi-Host ausgeführt werden.<br/><br/> Die Beispiel-App kann aus [GitHub](https://github.com/Microsoft/SmartHotel360) heruntergeladen werden.
OSTicket<br/><br/> Contoso Service Desk-App | Wird auf Linux/Apache mit einer MySQL PHP (LAMP) ausgeführt. | Die App auf zwei Ebenen mit einer Front-End-PHP-Website wird auf einer VM (OSTICKETWEB) und die MySQL-Datenbank auf einer anderen VM (OSTICKETMYSQL) ausgeführt | Die App wird von Kundendienst-Apps verwendet, um Probleme für die internen Mitarbeitern und externen Kunden zu verfolgen.<br/><br/> Die Beispiel-App kann aus [GitHub](https://github.com/osTicket/osTicket) heruntergeladen werden.

## <a name="current-architecture"></a>Aktuelle Architektur


Hier ist eine Darstellung der aktuellen lokalen Infrastruktur von Contoso.

![Contoso-Architektur](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso verfügt über ein Hauptrechenzentrum in New York City, im Osten der USA.
- Das Unternehmen hat drei zusätzliche Zweigstellen in Nordamerika.
- Das wichtigste Rechenzentrum ist über eine auf Glasfaser basierende Metro-Ethernet-Verbindung (500 Mbit/s) mit dem Internet verbunden.
- Jede Verzweigung ist lokal über Business-Class-Verbindungen mit dem Internet verbunden und IPSec-VPN-Tunnel führen zurück zum Hauptrechenzentrum. Dadurch ist das gesamtes Netzwerk des Unternehmens dauerhaft verbunden und die Internetverbindung optimiert.
- Das Hauptrechenzentrum ist vollständig mit VMware virtualisiert. Sie verfügen über zwei ESXi 6.5-Virtualisierungshosts, die von vCenter Server 6.5 verwaltet werden.
- Für die Identitätsverwaltung verwendet Contoso Active Directory sowie DNS-Server im internen Netzwerk.
- Die Domänencontroller im Rechenzentrum werden auf VMware-VMs ausgeführt. Die Domänencontroller an lokalen Standorten werden auf physischen Servern ausgeführt.





## <a name="business-drivers"></a>Geschäftsfördernde Faktoren

Das IT Leadership-Team hat eng mit den Geschäftspartnern zusammengearbeitet, um zu verstehen, was das Unternehmen mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und daher geraten die lokalen Systeme und Infrastrukturen unter Druck.
- **Effizienzsteigerung**: Contoso muss unnötige Prozeduren minimieren und Prozesse für seine Entwickler und Benutzer optimieren.  Die IT-Abteilung muss schnell sein, weder Geld noch Zeit verschwenden, und Kundenanforderungen schneller bearbeiten.
- **Steigerung der Flexibilität**: Die Contoso-IT-Abteilung muss schneller auf die Unternehmensanforderungen reagieren. Die IT-Experten müssen schneller reagieren als die Änderungen im Marketplace geschehen, um den Erfolg in einer globalen Wirtschaft zu garantieren.  Es darf nicht im Weg stehen oder zum Geschäftshindernis werden.
- **Skalierung**: Da das Unternehmen erfolgreich wächst, muss die Contoso-IT-Systeme bereitstellen, die mit der gleichen Geschwindigkeit wachsen können.

## <a name="assessment-goals"></a>Bewertungsziele

Das Cloud-Team von Contoso hat sich Ziele für seine Migrationsbewertungen gesetzt:

- Nach der Migration sollten die Apps in Azure über dieselben Leistungsaspekte verfügen, wie sie sie heute in ihrer lokalen VMWare-Umgebung haben.  Die Umstellung auf die Cloud bedeutet nicht, dass die Leistung der App weniger wichtig ist.
- Contoso muss die Kompatibilität ihrer Anwendungen und Datenbanken mit Azure-Anforderungen garantieren, und ihre Hostingoptionen in Azure verstehen.
- Die Datenbankverwaltung von Contoso sollte nach dem Verschieben von Apps in die Datenbank reduziert werden.  
- Contoso möchte nicht nur die Migrationsoptionen verstehen, sondern auch die Infrastrukturkosten nach dem Umzug in die Cloud.

## <a name="assessment-tools"></a>Bewertungstools
Contoso verwendet Microsoft-Tools für die Bewertung. Diese Tools harmonieren mit den Zielen und sollten alle benötigten Informationen bereitstellen.

**Technologie** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenmigrations-Assistent (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Sie verwenden den DMA zum Bewerten und Erkennen von Kompatibilitätsproblemen, die sich auf die Funktionalität ihrer Datenbank in Azure auswirken könnten. Der DMA bewertet die Featureparität zwischen SQL-Quellen und -Zielen, und empfiehlt Verbesserungen der Leistung und Zuverlässigkeit. | Sie können das Tool kostenlos herunterladen.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso wird diesen Dienst verwenden, um ihre VMware-VMs zu bewerten. Die Migrationseignung der Computer wird bewertet, und es werden Größenanpassungs- und Kostenschätzungen für die Ausführung in Azure bereitgestellt.  | Für die Nutzung dieses Diensts werden derzeit (Mai 2018) keine Gebühren berechnet.
[Dienstzuordnung](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Für Azure Migrate wird eine Dienstzuordnung verwendet, um Abhängigkeiten zwischen Computern anzuzeigen, die Sie migrieren möchten. |  Die Dienstzuordnung ist Teil von Azure Log Analytics. Sie kann derzeit 180 Tage lang genutzt werden, ohne dass Kosten berechnet werden.

Für dieses Szenario lädt Contoso den DMA herunter und führt ihn aus, um die lokale SQL Server-Datenbank für die Reise-App zu bewerten. Sie verwenden Azure Migrate mit Zuordnung von Abhängigkeiten, um die App-VMs zu bewerten, bevor sie sie zu Azure migrieren.



## <a name="assessment-architecture"></a>Architektur für die Bewertung


![Architektur für Migrationsbewertung](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso ist ein fiktiver Name für ein typisches Unternehmen.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) mit einem lokalen Domänencontroller (CONTOSODC1, CONTOSODC2).
- VMware-VMs befinden sich auf einem VMware ESXI-Host unter Version 6.5. Hosts: **contosohost1**, **contosohost2**
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter**) auf einer VM verwaltet.
- Die SmartHotel-Reise-App:
    - Die App befindet sich auf zwei VMware-VMs: **WEBVM** und **SQLVM**.
    - Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com**.
    - Die VMs werden mit SP1 auf Windows Server 2008 R2 Datacenter ausgeführt.
- Die VMware-Umgebung wird mit einem vCenter Server (**vcenter.contoso.com**) verwaltet, der auf einer VM ausgeführt wird.
- Die OSTicket Service Desk-App:
    - Die App wird über zwei VM-Ebenen **OSTICKETWEB** und **OSTICKETMYSQL** zur Verfügung gestellt.
    - Die VMs werden auf Ubuntu Linux Server 16.04-LTS ausgeführt.
    - Die OSTICKETWEB-VM führt Apache 2 und PHP 7.0 aus.
    - Die OSTICKETMYSQL-VM führt MySQL 5.7.22 aus.

![Architecture](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Voraussetzungen

Contoso (und Sie) benötigen Folgendes für die Bewertung:

- Zugriff als Besitzer oder Mitwirkender für das Azure-Abonnement oder für eine Ressourcengruppe im Azure-Abonnement.
- Einen lokalen vCenter Server mit Version 5.5, 6.0 oder 6.5.
- Ein Konto mit Lesezugriff für den vCenter Server bzw. Berechtigungen für die Kontoerstellung.
- Berechtigungen zum Erstellen einer VM auf dem vCenter Server mit einer .OVA-Vorlage.
- Mindestens einen ESXi-Host mit Version 5.0 oder höher.
- Mindestens zwei lokale VMware-VMs (mit Ausführung einer SQL Server-Datenbank auf einer VM).
- Berechtigungen zum Installieren von Azure Migrate-Agents auf jeder VM.
- Die VMs sollten über eine direkte Internetverbindung verfügen.
        - Sie können den Internetzugriff auf die [erforderlichen URLs](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) beschränken.
        – Bei Computern ohne Internetverbindung muss das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) auf ihnen installiert werden.
- FQDN der VM, auf der die SQL Server-Instanz ausgeführt wird, für die Datenbankbewertung.
- Für die Windows Firewall auf der SQL Server-VM sollten externe Verbindungen über TCP-Port 1433 (Standard) zugelassen sein, damit der DMA eine Verbindung herstellen kann.


## <a name="assessment-overview"></a>Bewertungsübersicht

Im Folgenden wird beschrieben, wie Contoso die Bewertung ausführt:


> [!div class="checklist"]
> * **Schritt 1: Herunterladen und Installieren des DMA**: Bereiten Sie den DMA für die Bewertung der lokalen SQL Server-Datenbank vor.
> * **Schritt 2: Bewerten der Datenbank mit dem DMA**: Führen Sie die Datenbankbewertung durch, und analysieren Sie sie.
> * **Schritt 3: Vorbereiten der VM-Bewertung mit Azure Migrate**: Richten Sie lokale Konten ein, und optimieren Sie die VMware-Einstellungen.
> * **Schritt 4: Ermitteln von lokalen VMs mit Azure Migrate**: Erstellen Sie eine Azure Migrate-Collector-VM. Anschließend führen sie den Collector aus, um die VMs für die Bewertung zu ermitteln.
> * **Schritt 5: Vorbereiten der Abhängigkeitsanalyse mit Azure Migrate**: Installieren Sie Azure Migrate-Agents auf den VMs, damit die Zuordnung der Abhängigkeiten zwischen VMs angezeigt wird.
> * **Schritt 6: Bewerten der VMs mit Azure Migrate**: Überprüfen Sie die Abhängigkeiten, gruppieren Sie die VMs, und führen Sie die Bewertung durch. Nachdem die Bewertung abgeschlossen wurde, analysieren sie sie als Vorbereitung für die Migration.


## <a name="step-1-download-and-install-the-dma"></a>Schritt 1: Herunterladen und Installieren des DMA

1. Contoso lädt den DMA aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) herunter.
    - Der Assistent kann auf jedem Computer installiert werden, auf dem eine Verbindung mit der SQL-Instanz hergestellt werden kann. Es ist nicht erforderlich, dass Sie ihn auf dem SQL Server-Computer ausführen.
    - Sie sollten ihn aber nicht auf dem SQL Server-Hostcomputer ausführen.
2. Sie führen die heruntergeladene Setupdatei (DownloadMigrationAssistant.msi) aus, um die Installation zu starten.
3. Auf der Seite **Fertigstellen** wählen sie **Launch Microsoft Data Migration Assistant** (Microsoft-Datenmigrations-Assistenten starten) aus, bevor sie den Assistenten beenden.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Schritt 2: Ausführen und Analysieren der Datenbankbewertung für SmartHotel

Contoso kann jetzt eine Bewertung durchführen, um ihren lokalen SQL Server für die SmartHotel-App zu analysieren.

1. Sie klicken im DMA auf **Neu**, wählen die Option **Bewertung** aus, und geben der Bewertung den Projektnamen **SmartHotel**.
2. Sie wählen den **Typ des Zielservers** als **SQL Server on Azure Virtual Machines** (SQL Server auf virtuellen Azure-Computern) aus.

    ![Quelle auswählen](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Derzeit wird die Bewertung für die Migration zu einer verwalteten SQL-Instanz vom DMA nicht unterstützt. Als Problemumgehung verwendet Contoso SQL Server auf Azure VM als angenommenes Ziel für die Bewertung.

3. In **Zielversion auswählen** wählen sie SQL Server 2017 als Zielversion aus. Die Verantwortlichen müssen diese Option aktivieren, da es die Version ist, die von der verwalteten SQL-Instanz verwendet wird.
4. Sie wählen sie zum Ermitteln von Informationen zu Kompatibilität und neuen Features aus:
    - Unter **Compatibility Issues** (Kompatibilitätsprobleme) werden Informationen zu Änderungen angezeigt, die die Migration ggf. verhindern oder für die vor der Migration eine kleinere Anpassung erforderlich ist. Dadurch erhalten Sie Informationen zu allen derzeit verwendeten Features, die als veraltet markiert wurden. Die Probleme sind nach Kompatibilitätsgrad angegeben.
    - Unter **Empfehlung zu neuen Features** werden Informationen zu neuen Features auf der SQL Server-Zielplattform angezeigt, die nach der Migration für die Datenbank verwendet werden können. Diese sind nach Leistung, Sicherheit und Speicher angegeben.

    ![Ziel auswählen](./media/contoso-migration-assessment/dma-assessment-2.png)

2. In **Verbinden mit einem Server** geben sie den Namen der VM an, auf der die Datenbank ausgeführt wird, und die Anmeldeinformationen für den Zugriff auf diese VM. Sie müssen die Option **Serverzertifikat vertrauen** aktivieren, um sicherzustellen, dass sie Zugriff auf den SQL Server erhalten. Dann klicken sie auf **Verbinden**.

    ![Ziel auswählen](./media/contoso-migration-assessment/dma-assessment-3.png)

3. In **Quelle hinzufügen** fügen sie die zu bewertende Datenbank hinzu, und klicken auf **Weiter**, um die Bewertung zu starten.
4. Die Bewertung wird erstellt.

    ![Bewertung erstellen](./media/contoso-migration-assessment/dma-assessment-4.png)

5. In **Überprüfen der Ergebnisse** können sie die Bewertungsergebnisse sehen.


### <a name="analyze-the-database-assessment"></a>Analysieren der Datenbankbewertung

Die Ergebnisse werden angezeigt, sobald sie verfügbar sind. Beim Beheben von Problemen müssen sie auf **Bewertung neu starten** klicken, um die Bewertung erneut auszuführen.

1. Im Bericht **Kompatibilitätsprobleme** überprüfen sie mögliche Probleme auf jedem Kompatibilitätsgrad. Kompatibilitätsgrade sind den SQL Server-Versionen wie folgt zugeordnet:

    - 100: SQL Server 2008/Azure SQL-Datenbank
    - 110: SQL Server 2012/Azure SQL-Datenbank
    - 120: SQL Server 2014/Azure SQL-Datenbank
    - 130: SQL Server 2016/Azure SQL-Datenbank
    - 140: SQL Server 2017/Azure SQL-Datenbank

    ![Kompatibilitätsprobleme](./media/contoso-migration-assessment/dma-assessment-5.png)

2. Im Bericht **Featureempfehlungen** kann Contoso die Leistungs-, Sicherheits- und Speicherfeatures anzeigen, die die Bewertung nach der Migration empfiehlt. Es werden verschiedene Features empfohlen, z.B. In-Memory-OLTP und Columnstore, Stretch Database, Always Encrypted, Dynamic Data Masking und Transparent Data Encryption (TDE).

    ![Featureempfehlungen](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Es wird empfohlen, dass Contoso für alle SQL Server-Datenbanken [TDE aktiviert](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017). Dies ist noch wichtiger, wenn sich die Datenbanken in der Cloud befinden. TDE sollte nur nach der Migration aktiviert werden. Wenn TDE bereits aktiviert ist, müssen Sie das Zertifikat oder den asymmetrischen Schlüssel in die Masterdatenbank des Zielservers verschieben. [Weitere Informationen](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017)

2. Sie können die Bewertung im JSON- oder CSV-Format exportieren.

Bei Durchführung einer umfangreicheren Bewertung können Sie Folgendes tun:

- Sie können mehrere Bewertungen gleichzeitig durchführen und den Status der Bewertungen anzeigen, indem Sie die Seite **Alle Bewertungen** öffnen.
- [Bewertungen in einer SQL Server-Datenbank zusammenfassen](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Bewertungen in einem Power BI-Bericht zusammenfassen](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Schritt 3: Vorbereiten der VM-Bewertung mit Azure Migrate

Contoso muss ein VMware-Konto erstellen, das von Azure Migrate zum automatischen Ermitteln von VMs für die Bewertung verwendet wird. Außerdem muss das Unternehmen überprüfen, ob es über Berechtigungen zum Erstellen einer VM verfügt, die Ports notieren, die geöffnet sein müssen, und die Einstellungsebene für Statistiken festlegen.

### <a name="set-up-a-vmware-account"></a>Einrichten eines VMware-Kontos

 Die VM-Ermittlung erfordert ein Konto mit Lesezugriff in vCenter mit den folgenden Eigenschaften:

- Benutzertyp: Mindestens ein Benutzer mit Lesezugriff.
- Berechtigungen: Data Center object (Rechenzentrumsobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle: schreibgeschützt).
- Details: Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.

### <a name="verify-permissions-to-create-a-vm"></a>Überprüfen der Berechtigungen für die Erstellung einer VM

Contoso vergewissert sich, dass es über Berechtigungen zum Erstellen einer VM per Import einer Datei im OVA-Format verfügt. [Weitere Informationen](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1)

### <a name="verify-ports"></a>Überprüfen der Ports

Die Contoso-Bewertung verwendet Abhängigkeitszuordnung. Für dieses Feature muss auf den VMs, die Sie bewerten, ein Agent installiert sein. Der Agent muss über den TCP-Port 443 jeder VM jeweils eine Verbindung mit Azure herstellen können. [Erfahren Sie mehr](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) zu den Verbindungsanforderungen.


### <a name="set-statistics-settings"></a>Festlegen der Statistikeinstellungen

Bevor sie die Bereitstellung starten, sollte Contoso die Statistikeinstellungen für vCenter Server auf Ebene 3 festlegen. Beachten Sie Folgendes:

- Nachdem sie die Ebene festgelegt haben, müssen sie mindestens einen Tag warten, bis sie die Bewertung durchführen können. Andernfalls verläuft der Vorgang unter Umständen nicht erwartungsgemäß.
- Bei einer höheren Ebene als 3 funktioniert die Bewertung zwar, aber es sollte Folgendes beachtet werden:
    - Die Leistungsdaten für Datenträger und das Netzwerk werden nicht erfasst.
    - Für den Speicher wird für Azure Migrate ein Standarddatenträger in Azure empfohlen, der die gleiche Größe wie der lokale Datenträger hat.
    - In Bezug auf das Netzwerk wird für jeden lokalen Netzwerkadapter die Verwendung eines entsprechenden Netzwerkadapters in Azure empfohlen.
    - Für den Computebereich untersucht Azure Migrate die VM-Kerne und die Arbeitsspeichergröße und empfiehlt eine Azure-VM mit der gleichen Konfiguration. Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) zur Größenanpassung für Ebene 3.

Sie legen die Ebene wie folgt fest:

1. Auf dem vSphere-Web-Client öffnen sie die vCenter Server-Instanz.
2. In **Verwalten** > **Einstellungen** > **Allgemein** klicken sie auf **Bearbeiten**.
3. In **Statistiken** legen sie die Einstellungen für die Statistikebene auf **Ebene 3** fest.

    ![vCenter-Statistikebene](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Schritt 4: Ermitteln von VMs

Zum Ermitteln von VMs erstellt Contoso ein Azure Migrate-Projekt. Sie laden die Collector-VM herunter und richten sie ein, und führen den Collector aus, um ihre lokalen VMs zu ermitteln.

### <a name="create-a-project"></a>Erstellen eines Projekts

1. Im [Azure-Portal](https://portal.azure.com) suchen sie nach **Azure Migrate**, und erstellen ein Projekt (ContosoMigration).
2. Sie geben einen Projektnamen und das Azure-Abonnement ein, und erstellen die neue Azure-Ressourcengruppe **ContosoFailoverRG**. Beachten Sie Folgendes:

    - Azure Migrate-Projekte können nur in der Region „USA, Westen-Mitte“ oder „USA, Osten“ erstellt werden.
    - Sie können eine Migration für einen beliebigen Zielstandort planen.
    - Der Projektstandort wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collector-Appliance lädt Contoso eine .OVA-Vorlage herunter und importiert sie auf dem Server von vCenter Server (lokal), um die VM zu erstellen.

1. Im Azure Migrate-Projekt klicken sie auf > **Erste Schritte** > **Ermitteln und Bewerten** > **Computer ermitteln** und laden die .OVA-Vorlagendatei herunter.
2. Sie kopieren Projekt-ID und -Schlüssel. Diese sind für die Konfiguration des Collectors notwendig.

    ![Herunterladen der OVA-Datei](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Vor der Bereitstellung der VM überprüft Contoso, ob die .OVA-Datei sicher ist.

1. Die Verantwortlichen öffnen ein Administratorbefehlsfenster auf dem Computer, auf den sie die Datei heruntergeladen haben.
2. Sie führen den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss den folgenden Einstellungen entsprechen (Version 1.0.9.12):

**Algorithmus** | **Hashwert**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Erstellen der Collectorappliance

Jetzt kann Contoso die heruntergeladene Datei auf den Server von vCenter Server importieren, und die Konfigurationsserver-VM bereitstellen.

1. Die Verantwortlichen im Unternehmen klicken in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/contoso-migration-assessment/vcenter-wizard.png)

2. Sie geben im Assistenten zum Bereitstellen der OVF-Vorlage unter > **Quelle** den Speicherort der OVA-Datei an.
3. Sie geben unter **Name und Speicherort** einen Anzeigenamen für die Collector-VM und den Inventarspeicherort an, unter dem die VM gehostet wird. Sie geben auch den Host oder Cluster an, auf bzw. in dem die Collector-Appliance ausgeführt werden soll.
5. In **Speicher** geben sie den Speicherort an, und in **Datenträgerformat**, wie sie den Speicher bereitstellen möchten.
7. Unter **Netzwerkzuordnung** geben sie das Netzwerk an, mit dem die Collector-VM eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können.
8. Sie überprüfen die Einstellungen, und wählen **Power on after deployment** (Nach der Bereitstellung einschalten)> **Fertigstellen** aus. Nachdem die Appliance erstellt wurde, wird eine Meldung mit dem Hinweis angezeigt, dass der Vorgang erfolgreich abgeschlossen wurde.

### <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

Jetzt führen sie den Collector zum Ermitteln von VMs aus. Beachten Sie: Der Collector unterstützt derzeit nur „Englisch (USA)“ als Sprache des Betriebssystems und der Collectoroberfläche.

1. Die Verantwortlichen wechseln in der vSphere-Clientkonsole zu **Konsole öffnen** und geben die Sprache, Zeitzone und Kennworteinstellungen für die Collector-VM an.
2. Auf dem Desktop klicken sie auf die Verknüpfung **Collector ausführen**.

    ![Collectorverknüpfung](./media/contoso-migration-assessment/collector-shortcut.png)

4. Sie wechseln im Azure Migrate-Collector zu **Set up prerequisites** (Erforderliche Komponenten einrichten), akzeptieren die Lizenzbedingungen und lesen die Informationen der Drittanbieter.
5. Der Collector überprüft, ob die VM über Internetzugriff verfügt, die Uhrzeit synchronisiert ist und der Collectordienst ausgeführt wird (standardmäßig auf der VM installiert). Außerdem wird die VMWare PowerCLI installiert.

    > [!NOTE]
    > Wir setzen voraus, dass die VM über direkten Internetzugriff ohne Proxy verfügt.

    ![Überprüfen der Voraussetzungen](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. In **Angeben von vCenter Server-Details** geben sie den Namen (FQDN) oder die IP-Adresse desServers von vCenter Server und die schreibgeschützten Anmeldeinformationen für die Ermittlung an.
7. Sie wählen einen Bereich für die Ermittlung von VMs aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1500 VMs umfassen.

    ![Herstellen einer Verbindung mit vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. Unter **Migrationsprojekt angeben** geben sie die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Sie erhalten sie erneut im Projekt **Übersicht** Seite > **Computer ermitteln**.  

    ![Herstellen einer Verbindung mit Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. Contoso kann in **Sammlungsfortschritt anzeigen** die Ermittlung überwachen, und sich vergewissern, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.

    ![Collector-Vorgang wird durchgeführt](./media/contoso-migration-assessment/collector-collection-process.png)



### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nachdem der Collector-Vorgang abgeschlossen ist, überprüft Contoso, ob die VMs im Portal angezeigt werden.

1. Im Azure Migrate-Projekt > **Verwalten** > **Computer** überprüfen die Mitarbeiter, dass die VMs angezeigt werden, die Sie ermitteln möchten.

    ![Ermittelte Computer](./media/contoso-migration-assessment/discovery-complete.png)

3. Beachten Sie, dass auf den Computern derzeit keine Azure Migrate-Agents installiert sind. Contoso muss diese installieren, um Abhängigkeiten anzuzeigen.

    ![Ermittelte Computer](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Schritt 5: Vorbereiten der Abhängigkeitsanalyse

Contoso lädt die Agents herunter und installiert sie auf den App-VMs, um die Abhängigkeiten zwischen den VMs anzuzeigen, bewerten werden sollen. Contoso führt dies auf allen VMs für ihre Apps aus, sowohl für Windows als auch für Linux.

### <a name="take-a-snapshot"></a>Erstellen einer Momentaufnahme

Sie behalten eine Kopie der VM vor der Änderung, indem sie eine Momentaufnahme erstellen, bevor die Agents installiert werden.

![Momentaufnahme eines Computers](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents

1. Auf der Seite **Computer** wählen sie den Computer aus und klicken dann in der Spalte **Abhängigkeiten** auf **Installation erforderlich**.
2. Auf der Seite **Computer ermitteln** führen Sie Folgendes aus:
    - Herunterladen des MMA und Abhängigkeit-Agents für jede Windows-VM
    - Herunterladen des MMA und Abhängigkeits-Agents für jede Linux-VM
3. Jetzt kopieren sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel. Sie benötigen diese für die MMA-Installation.

    ![Agent-Download](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Installieren der Agents auf den Windows-VMs

Sie führen die Installation auf jeder VM aus.

#### <a name="install-the-mma-on-windows-vms"></a>Installieren des MMA auf den Windows-VMs

1. Sie doppelklicken auf den heruntergeladenen Agent.
2. Unter **Zielordner** behalten sie den Standardinstallationsordner bei und klicken auf **Weiter**.
2. Unter **Agent-Setupoptionen** wählen sie die Option **Connect the agent to Azure Log Analytics** (Agent mit Azure Log Analytics verbinden) > **Weiter** aus.

    ![MMA-Installation](./media/contoso-migration-assessment/mma-install.png)

5. In **Azure Log Analytics** fügen sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den sie im Portal kopiert haben.

    ![MMA-Installation](./media/contoso-migration-assessment/mma-install2.png)

6. Unter **Bereit zur Installation** installieren sie den MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Installieren des Abhängigkeits-Agents unter Windows-VMs

1. Sie doppelklicken auf den heruntergeladenen Abhängigkeits-Agent.
2. Sie akzeptieren die Lizenzbedingungen und warten, bis die Installation abgeschlossen wurde.

    ![Abhängigkeits-Agent](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Installieren des Agents auf Linux-VMs

Sie führen die Installation auf jeder VM aus.

#### <a name="install-the-mma-on-linux-vms"></a>Installieren des MMA auf Linux-VMs

1. Sie installieren die Python Ctypes-Bibliothek auf jeder VM mithilfe von **sudo apt-get install python-ctypeslib**.
2. Sie sollten den Befehl zur Installation des MMA-Agents als Stamm ausführen.  Der folgende Befehl muss ausgeführt und das Stammkennwort **sudo -i** eingegeben werden, um einen Stamm zu erstellen.
3. Jetzt installieren sie den MMA-Agent.
    - Geben Sie die richtige Arbeitsbereichs-ID und den entsprechenden Schlüssel in das Befehlsfenster ein.
    - Die Befehle sind für 64-Bit.
    - Die **Arbeitsbereichs-ID** und der **Primärschlüssel** befinden sich im OMS-Portal unter > **Einstellungen** auf der Registerkarte **Verbundene Datenquellen**.
    - Führen Sie die folgenden Befehle aus, um den OMS-Agent herunterzuladen, die Prüfsumme zu überprüfen und den Agent anschließend zu installieren und zu integrieren.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```



#### <a name="install-the-dependency-agent-on-linux-vms"></a>Installieren des Abhängigkeits-Agents auf Linux-VMs

Nach der MMA-Installation kann Contoso den Abhängigkeits-Agent auf der Linux-VM installieren.

1. Der Abhängigkeits-Agent wird mit „InstallDependencyAgent-Linux64.bin“ auf den Linux-Computern installiert. Dabei handelt es sich um ein Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit „sh“ ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

2. Sie installieren den Abhängigkeits-Agent für Linux als Stamm:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Schritt 6: Durchführen und Analysieren der VM-Bewertung

Contoso kann jetzt die Abhängigkeiten der Computer überprüfen, und eine Gruppe erstellen. Dann führen sie die Bewertung für die Gruppe durch.

### <a name="verify-dependencies-and-create-a-group"></a>Überprüfen von Abhängigkeiten und Erstellen einer Gruppe


1. Sie klicken zur Computerbewertung auf **Abhängigkeiten anzeigen**.

    ![Anzeigen von Computerabhängigkeiten](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Für die SQLVM werden in der Abhängigkeitsübersicht die folgenden Informationen angezeigt:

    - Prozessgruppen bzw. Prozesse mit aktiven Netzwerkverbindungen auf der SQLVM für den angegebenen Zeitraum (standardmäßig eine Stunde)
    - Eingehende (Client) und ausgehende TCP-Verbindungen (Server) zu bzw. von allen abhängigen Computern.
    - Abhängige Computer mit installierten Azure Migrate-Agents werden als separate Felder angezeigt
    - Für Computer ohne installierte Agents werden Informationen zum Port und zur IP-Adresse angezeigt.

3. Sie klicken für Computer mit installiertem Agent (WEBVM) auf das Computerfeld, um weitere Informationen anzuzeigen, z.B. FQDN, Betriebssystem und MAC-Adresse.

    ![Anzeigen der Gruppenabhängigkeiten](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Sie wählen nun die VMs aus, die sie der Gruppe hinzufügen möchten (SQLVM und WEBVM).  Wenn sie beim Klicken die STRG-TASTE gedrückt halten, können sie mehrere VMs auswählen.
5. Sie klicken auf **Gruppe erstellen**, und geben einen Namen an (smarthotelapp).

> [!NOTE]
    > Sie können den Zeitbereich erweitern, um detailliertere Abhängigkeiten anzuzeigen. Sie können eine bestimmte Dauer oder das Start- und Enddatum auswählen.


### <a name="run-an-assessment"></a>Durchführen einer Bewertung


1. Auf der Seite **Gruppen**  öffnen sie die Gruppe (smarthotelapp) und klicken auf **Bewertung erstellen**.

    ![Erstellen einer Bewertung](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Die Bewertung wird auf der Seite **Verwalten** > **Bewertungen** angezeigt.

Contoso verwendete die Standardeinstellungen für Bewertungen, aber Sie können die Einstellungen auch anpassen. [Weitere Informationen](how-to-modify-assessment.md)



### <a name="analyze-the-vm-assessment"></a>Analysieren der VM-Bewertung

Eine Azure Migrate-Bewertung enthält Informationen dazu, ob die lokalen VMs mit Azure kompatibel sind, welche Größenanpassung für die Azure-VM richtig ist und welche geschätzten monatlichen Azure-Kosten anfallen.

![Bewertungsbericht](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

![Bewertungsanzeige](./media/contoso-migration-assessment/assessment-display.png)

Für eine Bewertung wird eine Zuverlässigkeitsstufe vergeben, die zwischen 1 Stern und 5 Sternen liegt (1 Stern = niedrigste Stufe, 5 Sterne = höchste Stufe).
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand dieses Werts können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe ist hilfreich, wenn Sie eine *leistungsbasierte Größenanpassung* durchführen, da Azure Migrate möglicherweise nicht über genügend Datenpunkte für eine nutzungsbasierte Größenanpassung verfügt. Bei der Größenanpassung vom Typ *Wie lokal* wird die Zuverlässigkeit immer mit fünf Sternen bewertet, da Azure Migrate über alle Datenpunkte verfügt, die zum Bestimmen der VM-Größe erforderlich sind.
- Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent):

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

#### <a name="verify-readiness"></a>Überprüfen der Bereitschaft

![Bewertungsbereitschaft](./media/contoso-migration-assessment/azure-readiness.png)  

Im Bewertungsbericht sind die Informationen in einer Tabelle zusammengefasst. Beachten Sie, dass Azure Migrate die folgenden Informationen benötigt, um die leistungsbasierte Größenanpassung anzeigen zu können. Falls diese Informationen nicht erfasst werden können, ist die Bewertung der Größenanpassung ggf. nicht genau.

- Nutzungsdaten für CPU und Arbeitsspeicher.
- Lese/Schreib-IOPS-Wert und Durchsatz für jeden Datenträger, der an die VM angefügt ist.
- Netzwerk-E/A-Informationen für jeden Netzwerkadapter, der an die VM angefügt ist.


**Einstellung** | **Anzeige** | **Details**
--- | --- | ---
**Azure-VM-Bereitschaft** | Gibt an, ob die VM für die Migration bereit ist. | Mögliche Status:</br><br/>- Bereit für Azure<br/><br/>- Bereit mit Bedingungen <br/><br/>- Nicht bereit für Azure<br/><br/>- Bereitschaft unbekannt<br/><br/> Wenn eine VM nicht bereit ist, werden Schritte zur Behebung des Problems angegeben.
**Azure-VM-Größe** | Für VMs, die bereit sind, wird eine Azure-VM-Größe empfohlen. | Die Empfehlung für die Größenanpassung richtet sich nach den Bewertungseigenschaften:<br/><br/>- Wenn Sie die leistungsbasierte Größenanpassung verwendet haben, wird bei der Größenanpassung der Leistungsverlauf der VMs berücksichtigt.<br/><br/>- Wenn Sie die Größenanpassung vom Typ „Wie lokal“ verwendet haben, basiert die Größenanpassung auf der Größe der lokalen VM, und es werden keine Nutzungsdaten verwendet.
**Vorgeschlagenes Tool** | Da die Agents auf unseren Computern ausgeführt werden, untersucht Azure Migrate die auf dem Computer ausgeführten Prozesse und ermittelt, ob es sich um einen Datenbankcomputer handelt.
**VM-Informationen** | Im Bericht werden die Einstellungen für die lokale VM angezeigt, z.B. Informationen zu Betriebssystem, Starttyp, Datenträger und Speicher.


#### <a name="review-monthly-cost-estimates"></a>Überprüfen der geschätzten monatlichen Kosten

In dieser Ansicht sind die gesamten Compute- und Speicherkosten für den Betrieb der virtuellen Computer in Azure zusammen mit den Details für die einzelnen Computer angegeben.

![Bewertungsbereitschaft](./media/contoso-migration-assessment/azure-costs.png)

- Kostenschätzungen werden anhand der Größenempfehlungen für einen Computer berechnet.
- Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert.


## <a name="clean-up-after-assessment"></a>Bereinigen nach der Bewertung

- Nachdem die Bewertung abgeschlossen ist, behält Contoso die Azure Migration-Appliance für zukünftige Auswertungen bei.
- Die Verantwortlichen schalten die VM-VMware aus. Sie starten sie erneut, wenn sie die zusätzlichen VMs bewerten.
- Sie behalten das Contoso Migration-Projekt in Azure bei.  Es ist derzeit in der Ressourcengruppe ContosoFailoverRG in der Azure-Region „USA Osten“ bereitgestellt.
-  Die Collector-VM besitzt eine 180-Tage-Evaluierungslizenz. Wenn dieses Limit abläuft, muss der Collector erneut heruntergeladen und eingerichtet werden.


## <a name="conclusion"></a>Zusammenfassung

In diesem Szenario bewertete Contoso die Datenbank der SmartHotel-App mithilfe des DMA-Tools sowie die lokalen VMs mithilfe des Azure Migrate-Diensts. Dann überprüfen die Verantwortlichen bei Contoso die Bewertungen, um sicherzustellen, dass die lokalen Ressourcen für die Migration zu Azure bereit sind.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel dieser Serie hostet Contoso mit einer Lift-Shift-Migration die SmartHotel-App erneut in Azure. Contoso migriert die App-Frontend-WEBVM mithilfe von Azure Site Recovery, und verwendet die App-Datenbank und den Database Migration Service, um zu einer verwalteten Azure SQL-Instanz zu migrieren. [Erste Schritte](contoso-migration-rehost-vm-sql-managed-instance.md) mit dieser Bereitstellung.

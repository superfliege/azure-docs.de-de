---
title: Contoso – Einrichten einer Infrastruktur für die Migration | Microsoft Docs
description: Erfahren Sie, wie Contoso eine Azure-Infrastruktur für die Migration zu Azure einrichtet.
services: azure-migrate
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/1/2018
ms.author: raynew
ms.openlocfilehash: 5dfe768ddb3509f896b90f913ffecdf33907357a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57876679"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso – Bereitstellen einer Migrationsinfrastruktur

In diesem Artikel bereitet Contoso seine lokale Infrastruktur für die Migration vor und richtet eine Azure-Infrastruktur ein, um die Migration vorzubereiten und sein Geschäft in einer Hybridumgebung zu betreiben.

- Es handelt sich um eine Beispielarchitektur, die für Contoso spezifisch ist.
- Ob Sie alle in diesem Artikel beschriebenen Elemente benötigen, hängt von Ihrer Migrationsstrategie ab. Wenn Sie beispielsweise nur native Cloud-Apps in Azure erstellen, benötigen Sie möglicherweise eine weniger komplexe Netzwerkstruktur.

Dieser Artikel ist Teil einer Artikelreihe, die dokumentiert, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und eine Reihe von Bereitstellungsszenarien, in denen die Einrichtung einer Migrationsinfrastruktur veranschaulicht wird, die Eignung der lokalen Ressourcen für die Migration bewertet wird und verschiedene Arten von Migrationen durchgeführt werden. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Der Reihe werden im Laufe der Zeit weitere Artikel hinzugefügt.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. | Verfügbar
Artikel 2: Bereitstellen einer Azure-Infrastruktur | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Migrationsartikel der Reihe wird dieselbe Infrastruktur verwendet. | Dieser Artikel
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](contoso-migration-assessment.md)  | Contoso führt eine Bewertung seiner lokalen SmartHotel360-App durch, die auf VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten. | Verfügbar
[Artikel 4: Rehosten einer App auf einer Azure-VM und einer verwalteten Azure SQL-Datenbank-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert den virtuellen Front-End-Computer der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migriert die App-Datenbank mit dem [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu einer verwalteten Azure SQL-Datenbank-Instanz. | Verfügbar   
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](contoso-migration-rehost-vm.md) | Contoso migriert die VMs der App SmartHotel360 mithilfe des Site Recovery-Diensts zu Azure-VMs. | Verfügbar
[Artikel 6: Rehosten einer App auf Azure-VMs und in einer SQL Server AlwaysOn-Verfügbarkeitsgruppe](contoso-migration-rehost-vm-sql-ag.md) | Bei der Migration der App verwendet Contoso Site Recovery, um die virtuellen Computer der App zu migrieren, und Database Migration Service, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der durch eine Always On-Verfügbarkeitsgruppe geschützt wird. | Verfügbar
[Artikel 7: Rehosten einer Linux-App auf Azure-VMs](contoso-migration-rehost-linux-vm.md) | Contoso führt mithilfe des Site Recovery-Diensts per Lift & Shift-Vorgang eine Migration seiner Linux-App „osTicket“ zu virtuellen Azure-Computern durch. | Verfügbar
[Artikel 8: Rehosten einer Linux-App auf Azure-VMs und Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migriert seine Linux-App „osTicket“ mithilfe von Site Recovery zu Azure-VMs. Die App-Datenbank wird zu Azure Database for MySQL migriert, indem MySQL Workbench verwendet wird. | Verfügbar
[Artikel 9: Umgestalten einer App in eine Azure-Web-App und in Azure SQL-Datenbank](contoso-migration-refactor-web-app-sql.md) | Contoso migriert die SmartHotel360-App zu einer Azure-Web-App und die App-Datenbank mithilfe des Datenbankmigrations-Assistenten zu einer Azure SQL Server-Instanz. | Verfügbar    
[Artikel 10: Umgestalten einer Linux-App in eine Azure-Web-App und in Azure Database for MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migriert seine Linux-App „osTicket“ an mehreren Standorten zu einer Azure-Web-App. Die Web-App wird in GitHub integriert, um die kontinuierliche Bereitstellung (Continuous Delivery) zu ermöglichen. Das Unternehmen migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. | Verfügbar
[Artikel 11: Umgestalten von Team Foundation Server in Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure. | Verfügbar
[Artikel 12: Umstrukturieren einer App in einen Azure-Container und Azure SQL-Datenbank](contoso-migration-rearchitect-container-sql.md) | Contoso migriert seine SmartHotel-App zu Azure. Anschließend wird die Webebene der App in einen Windows-Container umstrukturiert, der in Azure Service Fabric ausgeführt wird, und die App-Datenbank wird in eine Azure SQL-Datenbank umstrukturiert. | Verfügbar    
[Artikel 13: Neuerstellen einer App in Azure](contoso-migration-rebuild.md) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z.B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB. | Verfügbar
[Artikel 14: Skalieren einer Migration zu Azure](contoso-migration-scale.md) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor. | Verfügbar 

In diesem Artikel richtet Contoso sämtliche Infrastrukturelemente ein, die für die Durchführung aller Migrationsszenarien erforderlich sind. 


## <a name="overview"></a>Übersicht

Bevor die Migration zu Azure erfolgen kann, muss Contoso zunächst eine Azure-Infrastruktur vorbereiten.  Im Allgemeinen müssen fünf Bereiche betrachtet werden:

**Schritt 1: Azure-Abonnements:** Wie soll Azure durch Contoso erworben werden, und wie soll die Interaktion mit der Azure-Plattform und den Diensten erfolgen?  
**Schritt 2: Hybrididentität:** Wie wird der Zugriff auf lokale und Azure-basierte Ressourcen nach der Migration verwaltet und gesteuert? Wie wird die Identitätsverwaltung von Contoso erweitert oder in die Cloud ausgelagert?  
**Schritt 3: Notfallwiederherstellung und Ausfallsicherheit:** Wie stellt Contoso die Resilienz von Apps und Infrastruktur bei Ausfällen und Notfällen sicher?  
**Schritt 4: Netzwerk:** Wie sollte Contoso die Netzwerkinfrastruktur gestalten und die Konnektivität zwischen seinem lokalen Rechenzentrum und Azure einrichten?  
**Schritt 5: Sicherheit:** Wie schützt Contoso die Hybrid-/Azure-Bereitstellung?  
**Schritt 6: Governance:** Wie sorgt Contoso dafür, dass die Bereitstellung die Sicherheits- und Governanceanforderungen erfüllt?

## <a name="before-you-start"></a>Vorbereitung

Bevor wir uns die Infrastruktur ansehen, kann es sinnvoll sein, einige Hintergrundinformationen über die Azure-Funktionen durchzulesen, die in diesem Artikel erörtert werden:

- Für den Erwerb des Zugriffs auf Azure steht eine Reihe von Optionen zur Verfügung, darunter nutzungsbasierte Zahlung, Enterprise Agreements (EA), Lizenzierung nach dem Open License-Verfahren durch Microsoft-Handelspartner oder durch Microsoft-Partner, die als Cloud Solution Provider (CSP) bezeichnet werden. Erfahren Sie mehr über [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/), und lesen Sie, wie [Azure-Abonnements organisiert sind](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Verschaffen Sie sich einen Überblick über die [Identitäts- und Zugriffsverwaltung](https://www.microsoft.com/trustcenter/security/identity) in Azure. Informieren Sie sich insbesondere über [Azure AD und das Erweitern eines lokalen ADs in die Cloud](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Es steht ein nützliches E-Book über [Identitäts- und Zugriffsverwaltung in einer Hybridumgebung](https://azure.microsoft.com/resources/hybrid-cloud-identity/) zum Download zur Verfügung.
- Azure bietet eine robuste Netzwerkinfrastruktur mit Optionen für Hybridverbindungen. Verschaffen Sie sich einen Überblick über [Netzwerke und Netzwerkzugriffssteuerung](https://docs.microsoft.com/azure/security/security-network-overview).
- Lesen Sie eine Einführung in [Azure-Sicherheit](https://docs.microsoft.com/azure/security/azure-security) und über das Erstellen eines Plans für [Governance](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>Lokale Architektur

Hier ist eine Darstellung der aktuellen lokalen Infrastruktur von Contoso.

 ![Contoso-Architektur](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso verfügt über ein Hauptrechenzentrum in New York City, im Osten der USA.
- Außerdem besitzt es drei weitere Zweigstellen in den USA.
- Das wichtigste Rechenzentrum ist über eine auf Glasfaser basierende Metro-Ethernet-Verbindung (500 Mbit/s) mit dem Internet verbunden.
- Jede Verzweigung ist lokal über Business-Class-Verbindungen mit dem Internet verbunden und IPSec-VPN-Tunnel führen zurück zum Hauptrechenzentrum. Dadurch ist das gesamtes Netzwerk dauerhaft verbunden und die Internetverbindung optimiert.
- Das Hauptrechenzentrum ist vollständig mit VMware virtualisiert. Contoso verfügt über zwei ESXi 6.5-Virtualisierungshosts, die von vCenter Server 6.5 verwaltet werden.
- Für die Identitätsverwaltung verwendet Contoso Active Directory sowie DNS-Server im internen Netzwerk.
- Die Domänencontroller im Rechenzentrum werden auf VMware-VMs ausgeführt. Die Domänencontroller in lokalen Niederlassungen werden auf physischen Servern ausgeführt.


## <a name="step-1-buy-and-subscribe-to-azure"></a>Schritt 1: Kaufen und Abonnieren von Azure

Contoso muss entscheiden, wie Azure erworben werden soll, wie die Architektur von Abonnements ausgelegt wird und wie Dienste und Ressourcen lizenziert werden.

### <a name="buy-azure"></a>Kauf von Azure

Contoso entscheidet sich für ein [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Dies bringt eine im Voraus zu leistende finanzielle Verpflichtung für Azure mit sich, berechtigt Contoso aber zu großen Vorteilen, einschließlich flexibler Abrechnungsoptionen und einer optimierten Preisgestaltung.

- Contoso hat eine Schätzung der jährlichen Ausgaben für Azure erstellt. Bei Vertragsabschluss hat Contoso das erste Jahr vollständig bezahlt.
- Contoso muss alle Übertragungen vor Ablauf des Jahres verwenden, um keine bereits bezahlten Leistungen verfallen zu lassen.
- Sollte Contoso die Übertragungen überschreiten, stellt Microsoft ihnen die Differenz in Rechnung.
- Alle jenseits der Übertragung anfallenden Kosten werden zu den vertraglich vereinbarten Preisen berechnet. Für Überschreitungen werden keine Strafen fällig.

### <a name="manage-subscriptions"></a>Verwalten von Abonnements

Nach dem Erwerb von Azure muss Contoso herausfinden, wie die Azure-Abonnements verwaltet werden sollen. Contoso verfügt über ein EA und ist kann somit beliebig viele Azure-Abonnements einrichten.

- Eine Azure Enterprise-Registrierung definiert, wie ein Unternehmen Azure-Dienste modelliert und verwendet und legt eine Kernstruktur für die Governance fest.
- Als ersten Schritt hat Contoso eine Struktur für die Enterprise-Registrierung bestimmt (ein so genanntes Unternehmensgerüst). Contoso hat [diesen Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance) verwendet, um das Gerüst zu verstehen und zu entwerfen.
- Fürs Erste hat sich Contoso für einen funktionalen Abonnementverwaltungsansatz entschieden.
  - Das Unternehmen verwendet eine zentrale IT-Abteilung, die auch die Kontrolle über das Azure-Budget hat. Diese Gruppe hält als einzige Abonnements.
  - Contoso plant, dieses Modell später zu erweitern, sodass andere Gruppen im Unternehmen als Abteilungen an der Enterprise-Registrierung teilnehmen können.
  - Innerhalb der IT-Abteilung hat Contoso zwei Abonnements strukturiert, Produktion und Entwicklung.
  - Wenn Contoso zukünftig zusätzliche Abonnements benötigt, muss das Unternehmen den Zugriff, die Richtlinien und die Compliance für diese Abonnements verwalten. Hierzu führt Contoso [Azure-Verwaltungsgruppen](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview) als zusätzliche Ebene oberhalb der Abonnements ein.

    ![Enterprise-Struktur](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Untersuchen der Lizenzierung

Nach der Konfiguration der Abonnements kann sich Contoso mit der Microsoft-Lizenzierung befassen. Die Lizenzierungsstrategie hängt von den Ressourcen ab, die Contoso nach Azure migrieren möchte, sowie davon, wie virtuelle Azure-Computer und Azure-Dienste ausgewählt und bereitgestellt werden. 

#### <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Bei der Bereitstellung von VMs in Azure beinhalten Standardimages eine Lizenz, die Contoso die verwendete Software pro Minute in Rechnung stellt. Contoso ist jedoch seit langem Microsoft-Kunde und hat EAs und Lizenzen im Open License-Programm mit Software Assurance (SA) unterhalten. 

Der Azure-Hybridvorteil stellt eine kostengünstige Methode für die Migration von Contoso zur Verfügung und ermöglicht Einsparungen bei Workloads von Azure-VMs und SQL Server durch Umwandlung oder Wiederverwendung von Lizenzen der Windows Server Datacenter- und Standard-Editionen, die durch Software Assurance abgedeckt sind. Dadurch kann Contoso einen niedriger angesetzten Computesatz für VMs und SQL Server bezahlen. [Weitere Informationen](https://azure.microsoft.com/pricing/hybrid-benefit/)


#### <a name="license-mobility"></a>Lizenzmobilität

Lizenzmobilität durch SA bietet Microsoft-Volumenlizenz-Kunden wie Contoso die Flexibilität, berechtigte Serveranwendungen mit aktiver SA in Azure bereitzustellen. Hierdurch entfällt die Notwendigkeit zum Kauf neuer Lizenzen. Ohne anfallende Mobilitätsgebühren können die vorhandenen Lizenzen auf einfache Weise in Azure bereitgestellt werden. [Weitere Informationen](https://azure.microsoft.com/pricing/license-mobility/)

#### <a name="reserve-instances-for-predictable-workloads"></a>Reservieren von Instanzen für vorhersehbare Workloads

Vorhersehbare Workloads sind solche, die bei laufenden VMs jederzeit verfügbar sein müssen. Beispielsweise Branchenanwendungen wie ein SAP-ERP-System.  Demgegenüber sind nicht vorhersehbare Workloads solche, die variabel sind. Etwa VMs, die bei hohem Bedarf aktiv und außerhalb von Spitzenzeiten inaktiv sind.

![Reservierte Instanz](./media/contoso-migration-infrastructure/reserved-instance.png) 

Für die Verwendung reservierter Instanzen für bestimmte VM-Instanzen, die über lange Zeiträume beibehalten werden müssen, kann Contoso im Gegenzug sowohl einen Rabatt als auch priorisierte Kapazität erhalten. Mit [reservierten Azure-Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Kombination mit dem Azure-Hybridvorteil kann Contoso gegenüber der regulären nutzungsbasierten Bezahlung bis zu 82 % sparen (Stand April 2018).


## <a name="step-2-manage-hybrid-identity"></a>Schritt 2: Verwalten von Hybrididentitäten

Das Erteilen und Steuern des Benutzerzugriffs auf Azure-Ressourcen mit Identity & Access Management (IAM) ist ein wichtiger Schritt bei der Zusammenstellung einer Azure-Infrastruktur.  

- Contoso hat sich entschieden, das vorhandene lokale Active Directory in die Cloud zu erweitern, statt ein neues, separates System in Azure aufzubauen.
- Zu diesem Zweck erstellt das Unternehmen ein Azure-basiertes Active Directory.
- Bei Contoso ist Office 365 nicht implementiert, daher muss ein neues Azure AD bereitgestellt werden.
- Office 365 verwendet Azure AD für die Benutzerverwaltung. Bei Verwendung von Office 365 wäre bereits ein Azure AD-Mandant vorhanden, der als primäres AD-Verzeichnis verwendet werden kann.
- [Weitere Informationen](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) zu Azure AD für Office 365 und zum [Hinzufügen eines Abonnements](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) zu einem vorhandenen Azure AD-Mandanten.

### <a name="create-an-azure-ad"></a>Erstellen eines Azure ADs

Contoso verwendet die Azure AD Free-Edition, die in Azure-Abonnements enthalten ist. Contoso-Administratoren richten wie folgt ein AD-Verzeichnis ein:

1. Sie navigieren im [Azure-Portal](https://portal.azure.com/) zu **Ressource erstellen** > **Identität** > **Azure Active Directory**.
2. Sie geben in **Verzeichnis erstellen** einen Namen für das Verzeichnis ein, einen Anfangsdomänennamen und eine Region, in der das Azure AD-Verzeichnis erstellt werden soll.

    ![Azure AD erstellen](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Das erstellte Verzeichnis hat zunächst einen Domänennamen im Format **<Domänenname>.onmicrosoft.com**. Der Name kann weder geändert noch gelöscht werden. Stattdessen muss der registrierte Domänenname zu Azure AD hinzugefügt werden.

### <a name="add-the-domain-name"></a>Hinzufügen des Domänennamens

Die Administratoren von Contoso müssen den Standarddomänennamen als benutzerdefinierten Namen zu Azure AD hinzufügen, um diesen verwenden zu können. Diese Option ermöglicht es ihnen, vertraute Benutzernamen zuzuweisen. Beispielsweise kann sich ein Benutzer mit der E-Mail-Adresse billg@contoso.com anmelden und ist nicht auf billg@contosomigration.microsoft.com angewiesen. 

Damit ein benutzerdefinierter Domänenname eingerichtet werden kann, fügen sie ihn dem Verzeichnis sowie einen DNS-Eintrag hinzu, und bestätigen den Namen dann in Azure AD.

1. Sie fügen die Domäne unter **Benutzerdefinierte Domänennamen** > **Benutzerdefinierte Domäne hinzufügen** hinzu.
2. Um einen DNS-Eintrag in Azure verwenden zu können, müssen sie ihn bei ihrer Domänenregistrierungsstelle registrieren. 

    - Sie notieren sich in der Liste **Benutzerdefinierte Domänennamen** die DNS-Informationen zu dem Namen. Das Unternehmen verwendet einen MX-Eintrag.
    - Zu diesem Zweck benötigen sie Zugriff auf den Namenserver. Sie melden sich bei der Domäne „Contoso.com“ an und erstellen mithilfe der notierten Detailinformationen einen neuen MX-Eintrag für den von Azure AD bereitgestellten DNS-Eintrag.  
1. Nachdem die DNS-Einträge verteilt sind, klicken sie im Detailnamen für die Domäne auf **Überprüfen**, um den benutzerdefinierten Domänennamen zu überprüfen.

     ![Azure AD-DNS](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Einrichten von Gruppen und Benutzern lokal und in Azure

Nachdem Azure AD nun betriebsbereit ist, müssen die Administratoren von Contoso den lokalen AD-Gruppen, die mit Azure AD synchronisiert werden sollen, Mitarbeiter hinzufügen. Dabei empfiehlt es sich, lokale Gruppennamen zu verwenden, die mit den Namen von Ressourcengruppen in Azure übereinstimmen. Dies erleichtert die Identifikation von Übereinstimmungen zu Synchronisierungszwecken.

#### <a name="create-resource-groups-in-azure"></a>Erstellen von Ressourcengruppen in Azure

Azure-Ressourcengruppen fassen Azure-Ressourcen zu Gruppen zusammen. Mithilfe einer Ressourcengruppen-ID kann Azure Vorgänge für Ressourcen in der betreffenden Gruppe ausführen.

- Ein Azure-Abonnement kann mehrere Ressourcengruppen aufweisen, aber eine Ressourcengruppe kann nur innerhalb eines einzelnen Abonnements vorkommen.
- Darüber hinaus kann eine einzelne Ressourcengruppe über mehrere Ressourcen verfügen, eine Ressource kann jedoch jeweils nur einer einzelnen Ressourcengruppe angehören.

Die Administratoren von Contoso richten Azure-Ressourcengruppen wie in der folgenden Tabelle zusammengefasst ein.

**Ressourcengruppe** | **Details**
--- | ---
**ContosoCobRG** | Diese Gruppe enthält alle Ressourcen, die mit der Geschäftskontinuität (Continuity of Business, COB) zusammenhängen.  Sie enthält Tresore, die Contoso für den Azure Site Recovery- und den Azure Backup-Dienst verwendet.<br/><br/> Sie enthält außerdem Ressourcen, die für die Migration verwendet werden, einschließlich des Azure Migrate-Diensts und der Database Migration Services.
**ContosoDevRG** | Diese Gruppe enthält Ressourcen für Entwicklung und Tests.
**ContosoFailoverRG** | Diese Gruppe dient als Landungszone für Ressourcen bei einem Failovervorgang.
**ContosoNetworkingRG** | Diese Gruppe enthält alle Netzwerkressourcen.
**ContosoRG** | Diese Gruppe enthält Ressourcen im Zusammenhang mit Produktionsanwendungen und -datenbanken.

Die Ressourcengruppen werden in folgender Weise erstellt:

1. Sie fügen im Azure-Portal > **Ressourcengruppen** eine Gruppe hinzu.
2. Sie geben für jede Gruppe einen Namen, das Abonnement, zu dem die Gruppe gehört und die Region an.
3. Ressourcengruppen werden in der Liste **Ressourcengruppen** angezeigt.

    ![Ressourcengruppen](./media/contoso-migration-infrastructure/resource-groups.png) 

##### <a name="scaling-resource-groups"></a>Skalieren von Ressourcengruppen

Contoso fügt später bei Bedarf weitere Ressourcengruppen hinzu. Das Unternehmen kann beispielsweise eine Ressourcengruppe für jede App oder jeden Dienst definieren, um diese jeweils unabhängig voneinander zu schützen und zu verwalten.

#### <a name="create-matching-security-groups-on-premises"></a>Erstellen von übereinstimmenden lokalen Sicherheitsgruppen

1. Im lokalen Active Directory richten die Administratoren von Contoso Sicherheitsgruppen ein, deren Namen mit den Namen der Azure-Ressourcengruppen übereinstimmen.
 
    ![Lokale AD-Sicherheitsgruppen](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Zu Verwaltungszwecken erstellen sie eine zusätzliche Gruppe, die allen anderen Gruppen hinzugefügt wird. Dieser Gruppe werden Rechte an allen Ressourcengruppen in Azure erteilt. Der Gruppe wird eine begrenzte Zahl globaler Administratoren hinzugefügt.

### <a name="synchronize-ad"></a>Synchronisieren von AD

Contoso möchte eine gemeinsame Identität für den Zugriff auf lokale Ressourcen und Ressourcen in der Cloud bereitstellen. Zu diesem Zweck integrieren sie das lokale Active Directory in Azure AD. Dieses Modell ermöglicht Folgendes:

- Benutzer und Organisationen können eine einzelne Identität für den Zugriff auf lokale Anwendungen und Clouddienste wie Office 365 oder Tausende weiterer Websites im Internet nutzen.
- Administratoren können die Gruppen in AD nutzen, um [Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure zu implementieren.

Um die Integration zu erleichtern, verwendet Contoso das [Azure AD Connect-Tool](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Nach der Installation und Konfiguration auf einem Domänencontroller synchronisiert das Tool die lokalen AD-Identitäten mit dem Azure AD. 

### <a name="download-the-tool"></a>Herunterladen des Tools


1. Im Azure-Portal navigieren die Administratoren von Contoso zu **Azure Active Directory** > **Azure AD Connect** und laden die neueste Version des Tools auf den Server herunter, der für die Synchronisierung verwendet werden soll.

    ![AD Connect herunterladen](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Sie starten die Installation von **AzureADConnect.msi** über die Option **Express-Einstellungen verwenden**. Dies ist die gebräuchlichste Installation, die für Topologien mit einer einzelnen Gesamtstruktur und Synchronisierung von Kennworthashes zur Authentifizierung verwendet werden kann.

    ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. In **Mit Azure AD verbinden** geben sie die Anmeldeinformationen für das Herstellen der Verbindung mit dem Azure AD an (im Format „CONTOSO\admin“ oder „contoso.com\admin“).

    ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. Unter **Mit AD DS verbinden** geben sie Anmeldeinformationen für das lokale AD an.

     ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. In **Bereit zur Konfiguration** klicken sie auf **Starten Sie den Synchronisierungsvorgang, nachdem die Konfiguration abgeschlossen wurde**, um die Synchronisierung sofort zu starten. Anschließend führen sie die Installation durch.

Beachten Sie Folgendes:
- Contoso hat eine direkte Verbindung mit Azure. Wenn sich das lokale AD hinter einem Proxy befindet, lesen Sie diesen [Artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- Nach der ersten Synchronisierung sind lokale AD-Objekte im Azure AD sichtbar.

    ![Lokales AD in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Das IT-Team von Contoso ist basierend auf der jeweiligen Rolle in jeder Gruppe vertreten.

    ![Lokale AD-Mitglieder in Azure](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>Einrichten von RBAC

Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Benutzern genau die Zugriffsrechte gewähren, die diese zum Ausführen von Aufgaben benötigen. Sie weisen Benutzern, Gruppen und Anwendungen auf Bereichsebene eine passende RBAC-Rolle zu. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. 

Als Nächstes weisen die Administratoren von Contoso den AD-Gruppen, die aus der lokalen Infrastruktur synchronisiert wurden, Rollen zu.

1. Klicken Sie in der Ressourcengruppe **ControlCobRG** auf **Zugriffssteuerung (IAM)** > **Rollenzuweisung hinzufügen**.
2. Wählen Sie unter **Rollenzuweisung hinzufügen** > **Rolle** > **Mitwirkender** in der Liste die AD-Gruppe **ContosoCobRG** aus. Die Gruppe wird dann in der Liste **Ausgewählte Mitglieder** angezeigt. 
3. Sie wiederholen das mit den gleichen Berechtigungen für die anderen Ressourcengruppen (mit Ausnahme von **ContosoAzureAdmins**), indem sie dem AD-Konto, das der Ressourcengruppe entspricht, die Mitwirkender-Berechtigungen hinzufügen.
4. Für die **ContosoAzureAdmins**-AD-Gruppe weisen sie die Rolle **Besitzer** zu.

    ![Lokale AD-Mitglieder in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>Schritt 3: Konzipieren für Ausfallsicherheit und Notfälle

### <a name="set-up-regions"></a>Einrichten von Regionen

Azure-Ressourcen werden in Regionen bereitgestellt.

- Regionen sind in Geografien organisiert, und Datenresidenz, Datenhoheit, Compliance und Ausfallsicherheit werden innerhalb von geografischen Grenzen eingelöst.
- Eine Region setzt sich aus einer Reihe von Rechenzentren zusammen. Diese Rechenzentren werden innerhalb eines durch Latenz definierten Umkreises bereitgestellt und sind über ein dediziertes regionales Netzwerk mit geringer Latenz verbunden.
- Jede Azure-Region ist zwecks Ausfallsicherheit mit einer anderen Region gepaart.
- Lesen Sie mehr über [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/), und verstehen Sie, [wie Regionen gepaart werden](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso hat sich für „USA, Osten 2“ (mit Standort in Virginia) als primäre Region und für „USA, Mitte“ (mit Standort in Iowa) als sekundäre Region entschieden. Für diese Entscheidung gibt es eine Reihe von Gründen:

- Das Contoso-Rechenzentrum befindet sich in New York, und die Latenz zum nächstgelegenen Rechenzentrum wurde berücksichtigt.
- Die Region „USA, Osten 2“ verfügt über alle Dienste und Produkte, die Contoso benötigt. In Bezug auf die verfügbaren Produkte und Dienste sind nicht alle Azure-Regionen identisch. Hier können Sie die [Azure-Produkte nach Regionen](https://azure.microsoft.com/global-infrastructure/services/) überprüfen.
- USA, Mitte ist die gepaarte Region für USA, Osten 2 in Azure.

Bei der Planung der Hybridumgebung muss sich Contoso überlegen, wie Ausfallsicherheit und eine Strategie zur Notfallwiederherstellung im Regionsentwurf verwirklicht werden können. Allgemein reichen die Strategien von einer Bereitstellung in einer einzelnen Region, die sich für die Ausfallsicherheit auf Features der Azure-Plattform wie Fehlerdomänen und Paarung von Regionen verlässt, bis hin zu einem vollständigen Aktiv/Aktiv-Modell, in dem Clouddienste und Datenbanken in zwei Regionen bereitgestellt sind und auch Benutzer aus beiden Regionen bedienen.

Contoso hat sich für einen Mittelweg entschieden. Das Unternehmen stellt Apps und Ressourcen in einer primären Region bereit und pflegt eine vollständige Kopie der Infrastruktur in der sekundären Region, sodass diese im Notfall oder bei einem Ausfall der App oder der Region als vollständige Sicherung fungieren kann.

### <a name="set-up-availability-zones"></a>Einrichten von Verfügbarkeitszonen

Verfügbarkeitszonen tragen dazu bei, Apps und Daten vor Datencenterausfällen zu schützen.

- Jede Verfügbarkeitszone ist ein individueller physischer Standort in einer Azure-Region.
- Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. 
- In allen aktivierten Regionen sind mindestens drei separate Zonen vorhanden.
- Die physische Trennung der Zonen innerhalb einer Region schützt Anwendungen und Daten vor Datencenterausfällen.

Contoso stellt Verfügbarkeitszonen bereit, da Apps nach Skalierbarkeit, hoher Verfügbarkeit und Resilienz verlangen. [Weitere Informationen](https://docs.microsoft.com/azure/availability-zones/az-overview) 


## <a name="step-4-design-a-network-infrastructure"></a>Schritt 4: Entwerfen einer Netzwerkinfrastruktur

Nach der Erstellung des Regionskonzepts kann sich Contoso der Netzwerkstrategie zuwenden. Das Unternehmen muss sich überlegen, wie das lokale Rechenzentrum und Azure miteinander verbunden werden und miteinander kommunizieren sollen und wie die Netzwerkinfrastruktur in Azure gestaltet werden soll. Dazu muss Contoso insbesondere folgende Schritte ausführen:

- **Planen der Hybridnetzwerk-Konnektivität:** Contoso muss sich überlegen, wie netzwerkübergreifende Verbindungen zwischen der lokalen Umgebung und Azure hergestellt werden sollen.
- **Entwerfen einer Azure-Netzwerkinfrastruktur:** Contoso muss entscheiden, wie Netzwerke über Regionen hinweg bereitgestellt werden sollen. Wie kommunizieren die Netzwerke innerhalb der gleichen Region und regionsübergreifend?
- **Entwerfen und Einrichten von Azure-Netzwerken:** Contoso muss Azure-Netzwerke und Subnetze einrichten und entscheiden, was darin gespeichert werden soll.

### <a name="plan-hybrid-network-connectivity"></a>Planen der Hybridnetzwerk-Konnektivität

Contoso hat eine [Reihe von Architekturen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) für das Hybridnetzwerk zwischen Azure und dem lokalen Datencenter in Erwägung gezogen. [Weitere Informationen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) über das Vergleichen von Optionen.

Zur Erinnerung: Die lokale Netzwerkinfrastruktur von Contoso besteht derzeit aus dem Datencenter in New York und lokalen Niederlassungen im östlichen Teil der USA.  Alle Standorte verfügen über eine Business-Class-Verbindung zum Internet.  Jede dieser Niederlassungen ist mit dem Rechenzentrum durch einen IPsec-VPN-Tunnel durch das Internet verbunden.

![Contoso-Netzwerk](./media/contoso-migration-infrastructure/contoso-networking.png) 

Hier sehen Sie, für welche Implementierung von Hybridkonnektivität sich Contoso entschieden hat:

1. Einrichten einer neuen VPN-Verbindung von Standort zu Standort zwischen dem Contoso-Rechenzentrum in New York und den zwei Azure-Regionen in USA, Osten 2 und USA, Mitte.
2. Datenverkehr aus den Niederlassungen, die die virtuellen Azure-Netzwerke als Ziel haben, wird durch das Contoso-Hauptrechenzentrum geleitet. 
3. Beim zentralen Hochskalieren der Azure-Bereitstellung richtet Contoso eine ExpressRoute-Verbindung zwischen dem Datencenter und den Azure-Regionen ein. Contoso behält die VPN-Site-to-Site-Verbindung dann nur noch für Failoverzwecke bei.
    - [Erfahren Sie mehr](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) über die Wahl zwischen einer VPN- und einer ExpressRoute-Hybridlösung.
    - Überprüfen Sie [ExpressRoute-Standorte und -Unterstützung](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Nur VPN**

![Contoso-VPN](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN und ExpressRoute**

![Contoso-VPN/ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>Entwerfen der Azure-Netzwerkinfrastruktur

Contoso muss bei der Netzwerkimplementierung unbedingt darauf achten, dass die Hybridbereitstellung sicher und skalierbar ist. Zu diesem Zweck wählt Contoso einen langfristigen Ansatz und entwirft virtuelle Netzwerke (VNets) mit dem Augenmerk auf Ausfallsicherheit und Unternehmenstauglichkeit. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) zum Planen von VNets.

Bei der Verbindung der beiden Regionen hat sich Contoso für ein Hub-zu-Hub-Netzwerkmodell entschieden:

- Innerhalb der einzelnen Regionen verwendet Contoso ein Hub-and-Spoke-Modell (Nabe und Speiche).
- Zum Verbinden von Netzwerken und Hubs verwendet Contoso Azure-Netzwerkpeering.

#### <a name="network-peering"></a>Netzwerkpeering

Azure stellt Netzwerkpeering zum Verbinden von VNets und Hubs zur Verfügung. Globales Peering ermöglicht Verbindungen zwischen VNets und Hubs in verschiedenen Regionen. Lokales Peering verbindet VNets innerhalb der gleichen Region. Das Peering von VNets bietet eine Reihe von Vorteilen:

- Der Netzwerkdatenverkehr zwischen VNets, die mittels Peering verknüpft sind, ist privat.
- Datenverkehr zwischen den VNets bleibt innerhalb des Microsoft-Backbone-Netzwerks. Die Kommunikation zwischen den VNets kommt ganz ohne öffentliches Internet, Gateways oder Verschlüsselung aus.
- Peering stellt standardmäßig eine Verbindung mit hoher Bandbreite und geringer Latenz zwischen Ressourcen in unterschiedlichen VNets bereit.

[Weitere Informationen](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zu Netzwerkpeering.

#### <a name="hub-to-hub-across-regions"></a>Hub-zu-Hub über Regionen hinweg

Contoso wird einen Hub in jeder Region bereitstellen. Ein Hub ist ein virtuelles Netzwerk (VNet) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert. Die Hub-VNets sind untereinander mithilfe von globalem VNet-Peering verbunden. Globales VNet-Peering verbindet VNets über die Grenzen von Azure-Regionen hinweg.

- Der Hub in jeder Region ist mittels Peering mit seinem Partnerhub in der anderen Region verbunden.
- Der Hub ist mittels Peering mit jedem Netzwerk in seiner Region verbunden und kann Verbindungen mit allen Netzwerkressourcen herstellen.

    ![Globales Peering](./media/contoso-migration-infrastructure/global-peering.png) 

#### <a name="hub-and-spoke-within-a-region"></a>Hub-and-Spoke innerhalb einer Region

Innerhalb der einzelnen Regionen wird Contoso VNets zu verschiedenen Zwecken in Form von Spokenetzwerken vom Regionalhub bereitstellen. VNets innerhalb einer Region verwenden Peering für die Verbindung mit ihrem Hub und untereinander.

#### <a name="design-the-hub-network"></a>Entwerfen des Hubnetzwerks

Innerhalb des von Contoso gewählten Hub-and-Spoke-Modells muss noch über das Routing von Datenverkehr aus dem lokalen Datencenter und aus dem Internet entschieden werden. Für diese Routinglösung hat sich Contoso für die Hubs in „USA, Osten 2“ und „USA, Mitte“ entschieden:

- Contoso entwirft ein Netzwerk, das als „reverse c“ (umgekehrtes C) bekannt ist, denn dies ist der Pfad, dem die Pakete aus dem eingehenden in das ausgehende Netzwerk folgen.
- Die Netzwerkarchitektur hat zwei Grenzen: eine nicht vertrauenswürdige Front-End-Umkreiszone und eine vertrauenswürdige Back-End-Zone.
- Eine Firewall weist einen Netzwerkadapter in jeder Zone auf und kontrolliert den Zugriff auf die vertrauenswürdigen Zonen.
- Aus dem Internet:
    - Datenverkehr aus dem Internet trifft auf eine öffentliche IP-Adresse mit Lastenausgleich im Umkreisnetzwerk.
    - Dieser Datenverkehr wird durch die Firewall geroutet und unterliegt den Firewallregeln.
    - Nachdem die Netzwerk-Zugangskontrollen implementiert sind, wird der Verkehr an den entsprechenden Ort in der vertrauenswürdigen Zone weitergeleitet.
    - Ausgehender Verkehr aus dem VNet wird mithilfe von benutzerdefinierten Routen (User-defined Routes, UDRs) in das Internet geroutet. Der Datenverkehr durchläuft zwangsläufig die Firewall und wird auf Einhaltung der Contoso-Richtlinien geprüft.
- Aus dem Contoso-Rechenzentrum:
    - Eingehender Datenverkehr über VPN-Standort-zu-Standort (oder ExpressRoute) trifft auf die öffentliche IP-Adresse des Azure-VPN-Gateways.
    - Der Datenverkehr wird durch die Firewall geroutet und unterliegt den Firewallregeln.
    - Nach dem Anwenden der Firewallregeln wird der Datenverkehr an ein internes Lastenausgleichsmodul (Standard-SKU) im vertrauenswürdigen Subnetz der internen Zone weitergeleitet.
    - Ausgehender Datenverkehr aus dem vertrauenswürdigen Subnetz an das lokale Rechenzentrum über VPN wird durch die Firewall geroutet, und die Regeln werden angewendet, bevor der Datenverkehr über die VPN-Standort-zu-Standort-Verbindung geleitet wird.



### <a name="design-and-set-up-azure-networks"></a>Entwerfen und Einrichten von Azure-Netzwerken

Nach der Einrichtung der Netzwerk- und Routingtopologie ist Contoso jetzt bereit für die Einrichtung von Azure-Netzwerken und -Subnetzen.

- Contoso implementiert ein privates Netzwerk der Klasse A in Azure (0.0.0.0 bis 127.255.255.255). Das ist möglich, da lokal aktuell ein privater Adressraum der Klasse B (172.160.0/16) implementiert ist, sodass Contoso sicher sein kann, dass sich die Adressbereiche nicht überschneiden.
- Das Unternehmen stellt VNets in der primären und sekundären Region bereit.
- Dabei verwendet Contoso eine Namenskonvention mit dem Präfix **VNET** und der Regionsabkürzung **EUS2** oder **CUS**. Bei diesem Standard erhalten die Hub-Netzwerke die Namen **VNET-HUB-EUS2** (USA, Osten 2) und **VNET-HUB-CUS** (USA, Mitte).
- Da Contoso über keine [IPAM-Lösung](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top) verfügt, muss das Netzwerkrouting ohne NAT geplant werden.


#### <a name="virtual-networks-in-east-us-2"></a>Virtuelle Netzwerke in USA, Osten 2

USA, Osten 2 ist die primäre Region, die Contoso zum Bereitstellen von Ressourcen und Diensten verwenden wird. Die Netzwerkarchitektur wird von Contoso innerhalb dieser Region wie folgt gestaltet:

- **Hub:** Das Hub-VNET in „USA, Osten 2“ ist der zentrale Punkt der primären Konnektivität mit dem lokalen Rechenzentrum.
- **VNETs:** Spoke-VNETs in „USA, Osten 2“ können ggf. zum Isolieren von Workloads verwendet werden. Über das Hub-VNet hinaus wird Contoso in USA, Osten 2 zwei Spoke-VNets betreiben:
    - **VNET-DEV-EUS2**. Dieses VNet steht dem Entwicklungs- und Testteam als voll funktionsfähiges Netzwerk für Entwicklungsprojekte zur Verfügung. Es soll als Pilotbereich für die Produktion fungieren und baut in seiner Funktion auf der Produktionsinfrastruktur auf.
    - **VNET-PROD-EUS2:** Azure-IaaS-Produktionskomponenten befinden sich in diesem Netzwerk. 
    -  Jedes VNet verfügt über einen eigenen, eindeutigen Adressraum ohne Überschneidungen. Contoso plant, das Routing ohne NAT zu konfigurieren.
- **Subnetze**:
    - In jedem Netzwerk ist ein Subnetz für jede Anwendungsschicht vorhanden
    - Jedem Subnetz im Produktionsnetzwerk entspricht ein Subnetz im Entwicklungs-VNet.
    - Darüber hinaus enthält das Produktionsnetzwerk ein Subnetz für Domänencontroller.

Die VNETs in USA, Osten 2 sind in der folgenden Tabelle zusammengefasst.

**VNET** | **Bereich** | **Peer**
--- | --- | ---
**VNET-HUB-EUS2** | 10.240.0.0/20 | VNET-HUB-CUS2, VNET-DEV-EUS2, VNET-PROD-EUS2
**VNET-DEV-EUS2** | 10.245.16.0/20 | VNET-HUB-EUS2
**VNET-PROD-EUS2** | 10.245.32.0/20 | VNET-HUB-EUS2, VNET-PROD-CUS

![Hub/Spoke in der primären Region](./media/contoso-migration-infrastructure/primary-hub-peer.png) 


#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>Subnetze im Hub-Netzwerk USA, Osten 2 (VNET-HUB-EUS2)

**Subnetz/Zone** | **CIDR** | **Verwendbare IP-Adressen
--- | --- | ---
**IB-UntrustZone** | 10.240.0.0/24 | 251
**IB-TrustZone** | 10.240.1.0/24 | 251
**OB-UntrustZone** | 10.240.2.0/24 | 251
**OB-TrustZone** | 10.240.3.0/24 | 251
**GatewaySubnets** | 10.240.10.0/24 | 251


#### <a name="subnets-in-the-east-us-2-dev-network-vnet-dev-eus2"></a>Subnetze im Entwicklungsnetzwerk USA, Osten 2 (VNET-DEV-EUS2)

Das Entwicklungs-VNet wird vom Entwicklungsteam als Pilotbereich für die Produktion verwendet. Es verfügt über drei Subnetze.

**Subnetz** | **CIDR** | **Adressen** | **In Subnetz**
--- | --- | --- | ---
**DEV-FE-EUS2** | 10.245.16.0/22 | 1019 | Front-Ends/Webschicht-VMs
**DEV-APP-EUS2** | 10.245.20.0/22 | 1019 | App-Schicht-VMs
**DEV-DB-EUS2** | 10.245.24.0/23 | 507 | Datenbank-VMs


#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>Subnetze im Produktionsnetzwerk USA, Osten 2 (VNET-PROD-EUS2)

Azure-IaaS-Komponenten befinden sich im Produktionsnetzwerk. Jede App-Schicht weist ihr eigenes Subnetz auf. Die Subnetze entsprechen denen im Entwicklungsnetzwerk, mit einem zusätzlichen Netzwerk für Domänencontroller.

**Subnetz** | **CIDR** | **Adressen** | **In Subnetz**
--- | --- | --- | ---
**PROD-FE-EUS2** | 10.245.32.0/22 | 1019 | Front-Ends/Webschicht-VMs
**PROD-APP-EUS2** | 10.245.36.0/22 | 1019 | App-Schicht-VMs
**PROD-DB-EUS2** | 10.245.40.0/23 | 507 | Datenbank-VMs
**PROD-DC-EUS2** | 10.245.42.0/24 | 251 | Domänencontroller-VMs


![Hub-Netzwerkarchitektur](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Virtuelle Netzwerke in USA, Mitte (sekundäre Region)

USA Mitte ist die sekundäre Region von Contoso. Die Netzwerkarchitektur wird von Contoso innerhalb dieser Region wie folgt gestaltet:

- **Hub:** Das Hub-VNET in der Region „USA, Osten 2“ ist der zentrale Punkt für die Konnektivität mit dem lokalen Rechenzentrum, und die Spoke-VNETs in der Region „USA, Osten 2“ werden getrennt von anderen Spokes verwaltet und können ggf. zur Isolierung von Workloads verwendet werden.
- **VNETs:** Contoso verwendet zwei VNETs in „USA, Mitte“:
    - VNET-PROD-CUS. Diese VNet ist ein Produktionsnetzwerk, ähnlich wie VNET-PROD_EUS2. 
    - VNET-ASR-CUS. Diese VNet fungiert als Ort, an dem nach einem Failover von der lokalen Infrastruktur VMs erstellt werden, oder als Ort für Azure-VMs, bei denen ein Failover von der primären zur sekundären Region eingetreten ist. Diese Netzwerk ist ähnlich wie die Produktionsnetzwerke, weist aber keine Domänencontroller auf.
    -  Jedes VNet in der Region verfügt über einen eigenen Adressraum ohne Überschneidungen. Contoso konfiguriert das Routing ohne NAT.
- **Subnetze**: Die Architektur der Subnetze ähnelt der in „USA, Osten 2“. Einzige Unterschied: Contoso benötigt kein Subnetz für Domänencontroller.

Die VNETs in USA, Mitte sind in der folgenden Tabelle zusammengefasst.

**VNET** | **Bereich** | **Peer**
--- | --- | ---
**VNET-HUB-CUS** | 10.250.0.0/20 | VNET-HUB-EUS2, VNET-ASR-CUS, VNET-PROD-CUS
**VNET-ASR-CUS** | 10.255.16.0/20 | VNET-HUB-CUS, VNET-PROD-CUS
**VNET-PROD-CUS** | 10.255.32.0/20 | VNET-HUB-CUS, VNET-ASR-CUS, VNET-PROD-EUS2  


![Hub/Spoke in der gepaarten Region](./media/contoso-migration-infrastructure/paired-hub-peer.png)


#### <a name="subnets-in-the--central-us-hub-network-vnet-hub-cus"></a>Subnetze im Hub-Netzwerk USA, Mitte (VNET-HUB-CUS)

**Subnetz** | **CIDR** | **Verwendbare IP-Adressen**
--- | --- | ---
**IB-UntrustZone** | 10.250.0.0/24 | 251
**IB-TrustZone** | 10.250.1.0/24 | 251
**OB-UntrustZone** | 10.250.2.0/24 | 251
**OB-TrustZone** | 10.250.3.0/24 | 251
**GatewaySubnet** | 10.250.2.0/24 | 251


#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>Subnetze im Produktionsnetzwerk USA, Mitte (VNET-PROD-CUS)

Parallel zum Produktionsnetzwerk in der primären Region USA, Osten 2 gibt es ein Produktionsnetzwerk in der sekundären Region USA, Mitte. 

**Subnetz** | **CIDR** | **Adressen** | **In Subnetz**
--- | --- | --- | ---
**PROD-FE-CUS** | 10.255.32.0/22 | 1019 | Front-Ends/Webschicht-VMs
**PROD-APP-CUS** | 10.255.36.0/22 | 1019 | App-Schicht-VMs
**PROD-DB-CUS** | 10.255.40.0/23 | 507 | Datenbank-VMs
**PROD-DC-CUS** | 10.255.42.0/24 | 251 | Domänencontroller-VMs

#### <a name="subnets-in-the-central-us-failoverrecovery-network-in-central-us-vnet-asr-cus"></a>Subnetze im Failover-/Wiederherstellungsnetzwerk in USA, Mitte (VNET-ASR-CUS)

Das VNET-ASR-CUS-Netzwerk wird für Failoverzwecke zwischen den Regionen verwendet. Für Replikation und Failover bei Azure-VMs zwischen den Regionen wird Site Recovery verwendet. Es fungiert darüber hinaus als Netzwerk für geschützte Workloads, die lokal bleiben, für die aber bei der Notfallwiederherstellung ein Failover ausgeführt werden muss, und verbindet das Contoso-Rechenzentrum mit dem Azure-Netzwerk.

VNET-ASR-CUS ist das gleiche einfache Subnetz wie das Produktions-VNET in der Region „USA, Osten 2“, kommt jedoch ohne ein Subnetz für den Domänencontroller aus.

**Subnetz** | **CIDR** | **Adressen** | **In Subnetz**
--- | --- | --- | ---
**ASR-FE-CUS** | 10.255.16.0/22 | 1019 | Front-Ends/Webschicht-VMs
**ASR-APP-CUS** | 10.255.20.0/22 | 1019 | App-Schicht-VMs
**ASR-DB-CUS** | 10.255.24.0/23 | 507 | Datenbank-VMs

![Hub-Netzwerkarchitektur](./media/contoso-migration-infrastructure/azure-networks-cus.png)

#### <a name="configure-peered-connections"></a>Konfigurieren von Verbindungen mit Peering

Der Hub in jeder Region ist mittels Peering mit dem Hub in der anderen Region und mit allen VNets innerhalb der Hubregion verbunden. Dadurch können Hubs kommunizieren und alle VNets innerhalb einer Region „sehen“. Beachten Sie Folgendes:

- Durch Peering wird eine Verbindung mit zwei Seiten erstellt. Eine vom einleitenden Peer auf dem ersten VNet, die andere auf dem zweiten VNet.
- In einer Hybridbereitstellung muss Datenverkehr, der zwischen Peers übertragen wird, von der VPN-Verbindung zwischen dem lokalen Rechenzentrum und Azure gesehen werden. Dies wird mithilfe einiger spezifischer Einstellungen ermöglicht, die für Verbindungen mit Peering festgelegt werden müssen.

Für alle Verbindungen von Spoke-VNets durch den Hub in das lokale Rechenzentrum muss Contoso die Weiterleitung von Datenverkehr und die Durchquerung der VPN-Gateways zulassen.

##### <a name="domain-controller"></a>Domänencontroller

Hinsichtlich der Domänencontroller im Netzwerk VNET-PROD-EUS2 wünscht Contoso den Fluss des Datenverkehrs zwischen dem EUS2-Hub-/Produktionsnetzwerk und über die VPN-Verbindung zum lokalen Rechenzentrum. Hierzu müssen die Administratoren von Contoso Folgendes zulassen:

1. **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen** für die Verbindung mit Peering. In unserem Beispiel wäre das die Verbindung von VNET-HUB-EUS2 mit VNET-PROD-EUS2.

    ![Peering](./media/contoso-migration-infrastructure/peering1.png)

2. **Weitergeleiteten Datenverkehr zulassen** und **Remotegateways verwenden** auf der anderen Seite der Peeringverbindung, für die Verbindung von VNET-PROD-EUS2 mit VNET-HUB-EUS2.

    ![Peering](./media/contoso-migration-infrastructure/peering2.png)

3. Lokal wird eine statische Route eingerichtet, die den zu routenden lokalen Datenverkehr über den VPN-Tunnel an das VNet leitet. Die Konfiguration würde auf dem Gateway abgeschlossen, das den VPN-Tunnel von Contoso zu Azure bereitstellt. Dafür verwenden sie RRAS.

    ![Peering](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Produktionsnetzwerke 

Ein Peernetzwerk mit Spokes kann ein Peernetzwerk mit Spokes in einer anderen Region über einen Hub nicht sehen.

Damit die Contoso-Produktionsnetzwerke in beiden Regionen füreinander sichtbar sind, müssen die Administratoren von Contoso zwischen VNET-PROD-EUS2 und VNET-PROD-CUS eine direkte Verbindung mit Peering erstellen. 

![Peering](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>Einrichten von DNS

Beim Bereitstellen von Ressourcen in virtuellen Netzwerken bietet sich für die Domänennamenauflösung eine Reihe von Optionen. Sie können die in Azure verfügbare Namensauflösung verwenden oder DNS-Server für die Auflösung bereitstellen. Welche Art der Namensauflösung Sie verwenden, hängt davon ab, wie die Ressourcen miteinander kommunizieren müssen. Hier finden Sie [weitere Informationen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) über den Azure DNS-Dienst.

Die Administratoren von Contoso haben entschieden, dass der Azure DNS-Dienst keine gute Wahl für die Hybridumgebung ist. Stattdessen sollen die lokalen DNS-Server genutzt werden.

- Da es sich um ein Hybridnetzwerk handelt, müssen alle VMs – lokal wie in Azure – in der Lage sein, Namen aufzulösen, um ordnungsgemäß zu funktionieren. Dies bedeutet, dass auf alle VNets benutzerdefinierte DNS-Einstellungen angewendet werden müssen.
- Contoso verfügt derzeit über bereitgestellte DCs im Contoso-Rechenzentrum und in den Niederlassungen. Die primären DNS-Server sind CONTOSODC1(172.16.0.10) und CONTOSODC2(172.16.0.1)
- Wenn die VNets bereitgestellt werden, werden die lokalen Domänencontroller für die Verwendung als DNS-Server in den Netzwerken festgelegt. 
- Um dies zu konfigurieren, muss bei Verwendung eines benutzerdefinierten DNS im VNet der DNS-Liste die IP-Adresse des rekursiven Azure-Resolvers (z. B. 168.63.129.16) hinzugefügt werden.  Dazu konfiguriert Contoso die DNS-Servereinstellungen auf jedem VNet. Beispielsweise wären dies die benutzerdefinierten DNS-Einstellungen für das VNET-HUB-EUS2-Netzwerk:
    
    ![Benutzerdefinierter DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Neben den lokalen Domänencontrollern möchte Contoso zur Unterstützung der Azure-Netzwerke noch vier weitere hinzufügen (jeweils zwei pro Region). Contoso stellt also Folgendes in Azure bereit:

**Region** | **DC** | **VNET** | **Subnetz** | **IP-Adresse**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.5
CUS | CONTOSODC5 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4
CUS | CONTOSODC6 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4

Nach der Bereitstellung der lokalen Domänencontroller muss Contoso die DNS-Einstellungen in den Netzwerken beider Regionen aktualisieren, damit die neuen Domänencontroller in den Listen der DNS-Server vorhanden sind.



#### <a name="set-up-domain-controllers-in-azure"></a>Einrichten von Domänencontrollern in Azure

Nach der Aktualisierung der Netzwerkeinstellungen sind die Administratoren von Contoso bereit, die Domänencontroller in Azure zu implementieren.

1. Im Azure-Portal stellen sie eine neue Windows Server-VM im passenden VNet bereit.
2. Sie erstellen an jedem Standort Verfügbarkeitsgruppen für die VM. Verfügbarkeitsgruppen haben diese Aufgaben:
   - Sie stellen sicher, dass die Azure-Gesamtstruktur die VMs in verschiedene Infrastrukturen in der Azure-Region aufteilt. 
   - Sie ermöglichen Contoso die Qualifikation für die 99,95 %-SLA für VMs in Azure.  [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)

     ![Verfügbarkeitsgruppe](./media/contoso-migration-infrastructure/availability-group.png) 
3. Nach der Bereitstellung des virtuellen Computers öffnen sie die Netzwerkschnittstelle für den virtuellen Computer. Sie legen die private IP-Adresse als statisch fest und geben eine gültige Adresse an.

    ![VM-NIC](./media/contoso-migration-infrastructure/vm-nic.png)

4. Jetzt wird ein neuer Datenträger an die VM angefügt. Dieser Datenträger enthält die Active Directory-Datenbank und die SYSVOL-Freigabe. 
   - Die Größe des Datenträgers bestimmt die unterstützte Anzahl IOPS.
   - Im Lauf der Zeit muss die Datenträgergröße möglicherweise heraufgesetzt werden, wenn die Umgebung wächst.
   - Das Laufwerk darf nicht auf Lesen/Schreiben für die Hostzwischenspeicherung festgelegt sein. Active Directory-Datenbanken unterstützen dies nicht.

     ![Active Directory-Datenträger](./media/contoso-migration-infrastructure/ad-disk.png)

5. Nach dem Hinzufügen des Datenträgers wird über Remotedesktop eine Verbindung mit der VM hergestellt und der Server-Manager geöffnet.
6. Anschließend wird unter **Datei- und Speicherdienste** der Assistent für neue Volumes ausgeführt und sichergestellt, dass das Laufwerk auf der lokalen VM den Laufwerksbuchstaben F: oder höher erhält.

     ![Assistent für neue Volumes](./media/contoso-migration-infrastructure/volume-wizard.png)

7. Im Server-Manager wird die Rolle **Active Directory Domain Services** hinzugefügt. Anschließend wird der virtuelle Computer als Domänencontroller konfiguriert.

      ![Serverrolle](./media/contoso-migration-infrastructure/server-role.png)  

9. Nachdem die VM als DC konfiguriert und neu gestartet wurde, wird der DNS-Manager geöffnet und der Azure DNS-Resolver für Weiterleitung konfiguriert.  Dies erlaubt dem DC die Weiterleitung von DNS-Abfragen, die er nicht auflösen kann, an das Azure-DNS.

    ![DNS-Weiterleitung](./media/contoso-migration-infrastructure/dns-forwarder.png)

10. Jetzt werden die benutzerdefinierten DNS-Einstellungen für jedes VNet mit dem passenden Domänencontroller für die VNet-Region aktualisiert. Sie beinhalten die lokalen DCs in der Liste.

### <a name="set-up-active-directory"></a>Einrichten von Active Directory

AD ist ein kritischer Dienst im Netzwerk und muss ordnungsgemäß konfiguriert werden. Die Administratoren von Contoso erstellen AD-Standorte für das Contoso-Datencenter und für die Regionen EUS2 und CUS.  

1. Zusammen mit dem Rechenzentrumsstandort (ContosoDatacenter) erstellen sie zwei neue Standorte (AZURE-EUS2 und AZURE-CUS).
2. Nach dem Erstellen der Standorte werden Subnetze an den Standorten erstellt, um VNets und Rechenzentrum einander zuzuordnen.

    ![DC-Subnetze](./media/contoso-migration-infrastructure/dc-subnets.png)

3. Anschließend werden zwei Standortverknüpfungen erstellt, um alles zu verbinden. Dann sollten die Domänencontroller an ihre Standorte gebracht werden.

    ![DC-Verknüpfungen](./media/contoso-migration-infrastructure/dc-links.png)

4. Nachdem alles konfiguriert wurde, ist die Active Directory-Replikationstopologie implementiert.
    
    ![DC-Replikation](./media/contoso-migration-infrastructure/ad-resolution.png)

5. Nachdem alles abgeschlossen wurde, wird eine Liste der Domänencontroller und Standorte im lokalen Active Directory-Verwaltungscenter angezeigt.

    ![AD-Verwaltungscenter](./media/contoso-migration-infrastructure/ad-center.png)

## <a name="step-5-plan-for-governance"></a>Schritt 5: Planen der Governance

Azure stellt übergreifend in den Diensten und auf der Azure-Plattform eine Reihe von Governance-Steuerelementen zur Verfügung. [Weitere Informationen](https://docs.microsoft.com/azure/security/governance-in-azure) zu den grundlegenden Optionen.

Bei der Konfiguration von Identität und Zugriffssteuerung hat Contoso bereits mit der Implementierung einiger Governance- und Sicherheitsaspekte begonnen. Allgemein gibt es drei Bereiche, die berücksichtigt werden müssen:

- **Richtlinie:** Policy wendet in Azure Regeln und Wirkungen auf Ihre Ressourcen an und setzt sie durch, sodass die Ressourcen sich konform zu Unternehmensanforderungen und SLAs verhalten.
- **Sperren:** Azure erlaubt das Sperren von Abonnements, Ressourcengruppen und sonstigen Ressourcen, sodass diese nur von Personen geändert werden können, die dazu berechtigt sind.
- **Tags:** Ressourcen können mithilfe von Tags gesteuert, überwacht und verwaltet werden. Tags fügen Ressourcen Metadaten an, die Informationen über Ressourcen oder Besitzer bereitstellen.

### <a name="set-up-policies"></a>Einrichten von Richtlinien

Der Azure Policy-Dienst führt eine Bewertung von Ressourcen durch, um die zu ermitteln, die nicht mit den implementierten Richtliniendefinitionen kompatibel sind. Angenommen, es wäre eine Richtlinie implementiert, die nur bestimmte Typen von VMs zulässt oder für Ressourcen ein bestimmtes Tag vorschreibt. 

Azure-Richtlinien legen eine Richtliniendefinition fest, und die Richtlinienzuweisung gibt den Umfang an, in dem eine Richtlinie angewendet werden soll. Der Umfang kann von einer Verwaltungsgruppe bis zu einer Ressourcengruppe reichen. [Weitere Informationen](../governance/policy/tutorials/create-and-manage.md) zum Erstellen und Verwalten von Richtlinien.

Contoso möchte mit zwei Richtlinien beginnen:

- Eine Richtlinie soll sicherstellen, dass Ressourcen nur in den Regionen EUS2 und CUS bereitgestellt werden können.
- Eine Richtlinie soll die VM-SKUs auf genehmigte SKUs beschränken. Die Absicht dahinter ist, den Einsatz von teuren VM-SKUs zu verhindern.

#### <a name="limit-resources-to-regions"></a>Einschränken von Ressourcen auf Regionen

Contoso verwendet die integrierte Richtliniendefinition **Zulässige Standorte** zum Einschränken von Ressourcenregionen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und suchen Sie nach **Richtlinie**.
2. Wählen Sie **Zuweisungen** > **Richtlinie zuweisen** aus.
3. Wählen Sie in der Liste der Richtlinien **Zulässige Standorte** aus.
4. Legen Sie **Bereich** auf den Namen des Azure-Abonnements fest, und wählen Sie die zwei Regionen in der Zulassungsliste aus.

    ![Durch die Richtlinie zugelassene Regionen](./media/contoso-migration-infrastructure/policy-region.png)

5. Standardmäßig wird die Richtlinie mit **Verweigern** festgelegt, was bewirkt, dass eine Bereitstellung im Abonnement, die nicht in EUS2 oder CUS erfolgt, fehlschlägt. Hier sehen Sie, was geschieht, wenn jemand im Contoso-Abonnement versucht, eine Bereitstellung in USA, Westen einzurichten.

    ![Richtlinienfehler](./media/contoso-migration-infrastructure/policy-failed.png)

#### <a name="allow-specific-vm-skus"></a>Zulassen bestimmter VM-SKUs

Contoso verwendet die integrierte Richtliniendefinition **SKUs für virtuelle Computer zulassen**, um den Typ der VMs einzuschränken, die im Abonnement erstellt werden können. 

![SKU-Richtlinie](./media/contoso-migration-infrastructure/policy-sku.png)



#### <a name="check-policy-compliance"></a>Überprüfen der Richtlinienkonformität

Richtlinien treten sofort in Kraft, und Contoso kann Ressourcen auf Konformität überprüfen. 

1. Klicken Sie im Azure-Portal auf den Link **Compliance**.
2. Das Compliance-Dashboard wird angezeigt. Sie können einen Drilldown ausführen, um weitere Details anzuzeigen.

    ![Richtlinienkonformität](./media/contoso-migration-infrastructure/policy-compliance.png)


### <a name="set-up-locks"></a>Einrichten von Sperren

Contoso hat lange das ITIL-Framework für die Verwaltung seiner Systeme verwendet. Einer der wichtigsten Aspekte des Frameworks ist die Änderungssteuerung, und Contoso möchte sicherstellen, dass auch eine Änderungssteuerung in der Azure-Bereitstellung implementiert wird.

Contoso wird Sperren in folgender Weise implementieren:

- Alle Produktions- oder Failoverkomponenten müssen sich in einer Ressourcengruppe befinden, die eine Schreibschutzsperre aufweist.  Das bedeutet, dass die Sperre zum Ändern oder Löschen von Produktionselementen entfernt werden muss. 
- Nicht zur Produktion gehörende Ressourcengruppen erhalten CanNotDelete-Sperren. Damit können berechtigte Benutzer eine Ressource lesen oder ändern, sie aber nicht löschen.

[Weitere Informationen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) über Sperren.

### <a name="set-up-tagging"></a>Einrichten des Tagging

Um Ressourcen beim Hinzufügen nachzuverfolgen, wird es für Contoso zunehmend wichtig, sie einer entsprechenden Abteilung, einem Kunden und einer Umgebung zuzuordnen. 

Über das Bereitstellen von Informationen über Ressourcen und Besitzer hinaus geben Tags Contoso die Möglichkeit, Ressourcen zusammenzufassen und zu gruppieren und diese Daten zur verbrauchsbasierten Kostenzuteilung zu verwenden.

Contoso muss seine Azure-Ressourcen auf eine Weise visualisieren, die für das Unternehmen sinnvoll ist. Beispielsweise nach Rolle oder Abteilung. Beachten Sie, dass Ressourcen gleiche Tags aufweisen können, obwohl sie sich nicht in der gleichen Ressourcengruppe befinden. Contoso erstellt eine einfache Taxonomie für Tags, damit alle die gleichen Tags verwenden.

**Tagname** | **Wert**
--- | ---
CostCenter | 12345: Es muss sich um eine in SAP gültige Kostenstelle handeln.
BusinessUnit | Name der Geschäftseinheit (von SAP). Entspricht CostCenter.
ApplicationTeam | E-Mail-Alias des Teams, das den Support für die App besitzt.
CatalogName | Name der App oder des freigegebenen Diensts nach dem von der Ressource unterstützten Dienstkatalog.
ServiceManager | E-Mail-Alias des ITIL-Dienst-Managers für die Ressource.
COBPriority | Vom Unternehmen für BCDR festgelegte Priorität. Werte von 1–5.
ENV | Die möglichen Werte sind DEV, STG und PROD. Sie stehen für Entwicklung, Staging und Produktion.

Beispiel:  

 ![Azure-Tag](./media/contoso-migration-infrastructure/azure-tag.png)

Nach dem Erstellen des Tags kehrt Contoso zum Erstellen neuer Azure-Richtliniendefinitionen und -zuweisungen zurück, um die Verwendung der erforderlichen Tags in der gesamten Organisation durchzusetzen.


## <a name="step-6-consider-security"></a>Schritt 6: Planen der Sicherheit

Sicherheit ist in der Cloud entscheidend, und Azure bietet eine große Bandbreite an Sicherheitstools und -funktionen. Diese unterstützen Sie beim Aufbau von sicheren Lösungen auf der sicheren Azure-Plattform. Lesen Sie [Sicher in der vertrauenswürdigen Cloud](https://azure.microsoft.com/overview/trusted-cloud/), um mehr über Azure-Sicherheit zu erfahren.

Für Contoso ist eine Reihe von Hauptaspekten zu berücksichtigen:

- **Azure Security Center:** Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Mit Security Center können Sie Sicherheitsrichtlinien für Ihre Workloads anwenden, die Angriffsfläche für Bedrohungen verringern sowie Angriffe erkennen und darauf reagieren.  [Weitere Informationen](https://docs.microsoft.com/azure/security-center/security-center-intro)
- **Netzwerksicherheitsgruppen (NSGs):** Eine NSG ist ein Filter (Firewall) mit einer Liste von Sicherheitsregeln, deren Anwendung zur Zulassung oder Ablehnung von Netzwerkverkehr an mit Azure-VNETs verbundene Ressourcen führt. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-network/security-overview)
- **Datenverschlüsselung:** Azure Disk Encryption ist eine Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. [Weitere Informationen](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

### <a name="work-with-the-azure-security-center"></a>Arbeiten mit dem Azure Security Center

Contoso möchte sich schnell einen Überblick über die Sicherheitslage der neuen Hybrid Cloud (und insbesondere der Azure-Workloads) verschaffen.  Daher hat sich Contoso für die Implementierung von Azure Security Center entschieden, zunächst mit diesen Features: 

- Zentrale Richtlinienverwaltung
- Kontinuierliche Bewertung
- Umsetzbare Empfehlungen 

#### <a name="centralize-policy-management"></a>Zentrale Richtlinienverwaltung

Mit der zentralen Richtlinienverwaltung stellt Contoso dank zentraler Verwaltung von Sicherheitsrichtlinien in der gesamten Umgebung die Erfüllung der Sicherheitsanforderungen sicher. Das Unternehmen kann schnell und einfach eine Richtlinie implementieren, die für alle ihre Azure-Ressourcen gilt.

![Sicherheitsrichtlinie](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Bewertung und Aktion

Contoso nutzt die kontinuierliche Sicherheitsbewertung, die die Sicherheit von Computern, Netzwerken, Massenspeichern, Daten und Anwendungen überwacht, um mögliche Sicherheitsprobleme zu entdecken. 

- Security Center analysiert den Sicherheitszustand der Compute-, Infrastruktur- und Datenressourcen von Contoso sowie der Azure-Apps und -Dienste.
- Die kontinuierliche Bewertung hilft dem Contoso-Betriebsteam dabei, potenzielle Sicherheitsprobleme zu erkennen, z.B. Systeme mit fehlenden Sicherheitsupdates oder ungeschützten Netzwerkports. 
- Insbesondere möchte Contoso sicherstellen, dass sämtliche virtuellen Computer geschützt sind. Security Center ist hier eine Hilfe, indem es die Integrität von VMs überprüft und nach Priorität geordnete, umsetzbare Empfehlungen zum Korrigieren von Sicherheitsrisiken macht, bevor diese ausgenutzt werden.

![Überwachung](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Arbeiten mit NSGs

Contoso kann durch Verwendung einer Netzwerksicherheitsgruppe den Netzwerkdatenverkehr auf Ressourcen in einem virtuellen Netzwerk beschränken.

- Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen.
- Bei Anwendung auf ein Subnetz gelten Regeln für alle Ressourcen des Subnetzes. Über die Netzwerkschnittstellen hinaus schließt dies auch Instanzen von Azure-Diensten ein, die im Subnetz bereitgestellt sind.
- Mit Anwendungssicherheitsgruppen (ASGs) können Sie die Netzwerksicherheit als natürliche Erweiterung einer App-Struktur konfigurieren und VMs gruppieren sowie auf der Grundlage dieser Gruppen Netzwerksicherheitsrichtlinien definieren.
    - Anwendungssicherheitsgruppen ermöglichen Contoso die bedarfsabhängige Wiederverwendung der Sicherheitsrichtlinie des Unternehmens, ohne dass explizite IP-Adressen manuell gewartet werden müssen. Die Plattform übernimmt die komplexe Verarbeitung von expliziten IP-Adressen und mehreren Regelsätzen, damit Sie sich auf Ihre Geschäftslogik konzentrieren können.
    - Contoso kann eine Anwendungssicherheitsgruppe in einer Sicherheitsregel als Quelle und Ziel angeben. Sobald die Sicherheitsrichtlinie definiert ist, kann Contoso VMs erstellen und die VM-NICs einer Gruppe zuweisen. 


Contoso implementiert eine Mischung aus NSGs und ASGs. Contoso hat Bedenken hinsichtlich der NSG-Verwaltung. Darüber hinaus hat das Unternehmen Bedenken hinsichtlich der übermäßigen Verwendung von NSGs und der zusätzlichen Komplexität für das Betriebspersonal. Contoso geht wie folgt vor:

- Der gesamte ein- und ausgehende Datenverkehr aller Subnetze (Nord-Süd) unterliegt einer NSG-Regel, mit Ausnahme der Gateway-Subnetze in den Hub-Netzwerken.
- Alle Firewalls und Domänencontroller sind sowohl mit Subnetz-NSGs als auch mit NIC-NSGs geschützt.
- Auf alle Produktionsanwendungen werden ASGs angewendet.


Zur Veranschaulichung hat Contoso ein Modell für seine Anwendungen erstellt.

![Sicherheit](./media/contoso-migration-infrastructure/asg.png)


Die den ASGs zugeordneten NSGs werden mit geringsten Rechten konfiguriert, um sicherzustellen, dass nur zulässige Pakete von einem Teil des Netzwerks an ihr Ziel fließen können.

**Aktion** | **Name** | **Quelle** | **Ziel** | **Port**
--- | --- | --- | --- | --- 
ZULASSEN | AllowiInternetToFE | VNET-HUB-EUS1/IB-TrustZone | APP1-FE 80, 443
ZULASSEN | AllowWebToApp | APP1-FE | APP1-DB | 1433
ZULASSEN | AllowAppToDB | APP1-APP | Beliebig | Beliebig
Verweigern | DenyAllInbound | Beliebig | Beliebig | Beliebig

### <a name="encrypt-data"></a>Verschlüsseln von Daten

Azure Disk Encryption ist in Azure Key Vault integriert, damit die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in einem Key Vault-Abonnement gesteuert und verwaltet werden können. Dies stellt sicher, dass alle ruhenden Daten auf VM-Datenträgern in Azure Storage verschlüsselt sind.  

- Contoso hat festgelegt, dass für bestimmte VMs Verschlüsselung erforderlich ist.
- Die Verschlüsselung soll auf virtuelle Computer angewendet werden, die Kundendaten, vertrauliche Daten oder EPI-Daten enthalten.


## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel hat Contoso eine Azure-Infrastruktur spwie eine Richtlinie für Azure-Abonnements, Hybrididentität, Notfallwiederherstellung, Netzwerk, Governance und Sicherheit eingerichtet. 

Nicht alle Schritte, die Contoso hier zurückgelegt hat, sind für eine Migration in die Cloud erforderlich. In diesem Fall wollte das Unternehmen eine Netzwerkinfrastruktur planen, die für alle Migrationstypen verwendet werden kann und sicher, robust und skalierbar ist. 

Mit dieser Infrastruktur ist Contoso nun bereit, den nächsten Schritt zu tun und die Migration auszuprobieren.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Migrationsszenario wird Contoso die [lokale zweischichtige SmartHotel360-App für die Migration zu Azure bewerten](contoso-migration-assessment.md). 

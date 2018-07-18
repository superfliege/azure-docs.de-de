---
title: Contoso – Einrichten einer Infrastruktur für die Migration | Microsoft Docs
description: Erfahren Sie, wie Contoso eine Azure-Infrastruktur für die Migration zu Azure einrichtet.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: bf1406c8e361e0a1433b0e26c477c3c34e987fcf
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38562757"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso – Bereitstellen einer Migrationsinfrastruktur

In diesem Artikel wird dargelegt, wie Contoso eine lokale und Azure-Infrastruktur in Vorbereitung der Migration zu Azure und zum Betreiben des Geschäfts in einer Hybridumgebung einrichtet.

- Es handelt sich um eine Beispielarchitektur, die für Contoso spezifisch ist.
- Ob Sie alle im Artikel beschriebenen Elemente benötigen, hängt von Ihrer Migrationsstrategie ab. Wenn Sie beispielsweise nur native Cloud-Apps in Azure erstellen, benötigen Sie möglicherweise eine weniger komplexe Netzwerkstruktur.

Dieses Dokument ist das zweite in einer Reihe von Artikeln, die dokumentieren, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen in die Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und eine Zusammenstellung von Bereitstellungsszenarien, die die Einrichtung einer Migrationsinfrastruktur veranschaulichen, die Eignung der lokalen Ressourcen für die Migration bewerten, und verschiedene Typen von Migrationen durchführen. Die Szenarios werden an Komplexität zunehmen, und mit der Zeit werden zusätzliche Artikel hinzugefügt.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
Artikel 2: Bereitstellen einer Azure-Infrastruktur (dieser Artikel) | Beschreibt, wie Contoso die lokale und die Azure-Infrastruktur für die Migration vorbereitet. Für alle Contoso-Migrationsszenarios wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md) | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen SmartHotel-App, die unter VMware ausgeführt wird, durchführt. Sie bewerten App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Datenbankmigrations-Assistent von Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso die App SmartHotel zu Azure migriert. Das Unternehmen migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), und verwendet die App-Datenbank und den Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview), um zu einer verwalteten SQL-Instanz zu migrieren. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert.
[Artikel 6: Zuweisen von Azure-VMs und SQL Server-Verfügbarkeitsgruppen als neue Hosts](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Das Unternehmen verwendet Site Recovery zum Migrieren der App-VMs und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-Verfügbarkeitsgruppe. | Verfügbar
[Artikel 7: Zuweisen von Azure-VMs als neue Hosts zu einer Linux-App](contoso-migration-rehost-linux-vm.md) | Veranschaulicht, wie Contoso die Linux-App osTicket zu Azure VMs migriert. | Verfügbar
[Artikel 8: Zuweisen von Azure-VMs und Azure MySQL Server als neue Hosts für eine Linux-App](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux osTicket-App mithilfe von Site Recovery und MySQL-Workbench (Sicherung und Wiederherstellung) zu einer Azure MySQL Server-Instanz migriert. | Verfügbar

In diesem Artikel richtet Contoso sämtliche Infrastrukturelemente ein, die für die Durchführung der Migrationsszenarien erforderlich sind. 


## <a name="overview"></a>Übersicht

Bevor die Migration zu Azure erfolgen kann, ist es von entscheidender Wichtigkeit, dass Contoso die vorhandene Infrastruktur vorbereitet.  Im Allgemeinen müssen fünf große Gebiete betrachtet werden:

**Schritt 1: Azure-Abonnements**: Wie soll Azure erworben werden, und wie soll die Interaktion mit der Azure-Plattform und den Diensten erfolgen?  
**Schritt 2: Hybrididentität**: Wie sollen Steuerung und Verwaltung des Zugriffs auf lokale und Azure-Ressourcen nach der Migration erfolgen? Wie wird die Identitätsverwaltung in die Cloud erweitert oder ausgelagert?  
**Schritt 3: Notfallwiederherstellung und Ausfallsicherheit**: Wie kann sichergestellt werden, dass Apps und Infrastruktur bei Ausfällen und Notfällen resilient sind?  
**Schritt 4: Netzwerk**: Wie soll die Netzwerkinfrastruktur entworfen und Konnektivität zwischen dem lokalen Rechenzentrum und Azure hergestellt werden?  
**Schritt 5: Sicherheit**: Wie wird die Hybrid- bzw. die Azure-Bereitstellung abgesichert?  
**Schritt 6: Governance**: Wie wird die Bereitstellung auf die Sicherheits- und Governanceanforderungen ausgerichtet?

## <a name="before-you-start"></a>Vorbereitung

Bevor wir uns die Infrastruktur ansehen, kann es sinnvoll sein, einige Hintergrundinformationen über die Azure-Funktionen durchzulesen, die in diesem Artikel erörtert werden:

- Für den Erwerb des Zugriffs auf Azure steht eine Reihe von Optionen zur Verfügung, darunter nutzungsbasierte Zahlung, Enterprise Agreements (EA), Lizenzierung nach dem Open License-Verfahren durch Microsoft-Handelspartner oder durch Microsoft-Partner, die als Cloud Solution Provider (CSP) bezeichnet werden. Erfahren Sie mehr über [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/), und lesen Sie, wie [Azure-Abonnements organisiert sind](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Verschaffen Sie sich einen Überblick über die [Identitäts- und Zugriffsverwaltung](https://www.microsoft.com/en-us/trustcenter/security/identity) in Azure. Informieren Sie sich insbesondere über [Azure AD und das Erweitern eines lokalen ADs in die Cloud](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Es steht ein nützliches E-Book über [Identitäts- und Zugriffsverwaltung in einer Hybridumgebung](https://azure.microsoft.com/resources/hybrid-cloud-identity/) zum Download zur Verfügung.
- Azure bietet eine robuste Netzwerkinfrastruktur mit Optionen für Hybridverbindungen. Verschaffen Sie sich einen Überblick über [Netzwerke und Netzwerkzugriffssteuerung](https://docs.microsoft.com/azure/security/security-network-overview).
- Lesen Sie eine Einführung in [Azure-Sicherheit](https://docs.microsoft.com/azure/security/azure-security) und über das Erstellen eines Plans für [Governance](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>Lokale Architektur

Hier ist eine Darstellung der aktuellen lokalen Infrastruktur von Contoso.

 ![Contoso-Architektur](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso verfügt über ein Hauptrechenzentrum in New York City, im Osten der USA.
- Das Unternehmen hat drei zusätzliche Zweigstellen in Nordamerika.
- Das wichtigste Rechenzentrum ist über eine auf Glasfaser basierende Metro-Ethernet-Verbindung (500 Mbit/s) mit dem Internet verbunden.
- Jede Verzweigung ist lokal über Business-Class-Verbindungen mit dem Internet verbunden und IPSec-VPN-Tunnel führen zurück zum Hauptrechenzentrum. Dadurch ist das gesamtes Netzwerk des Unternehmens dauerhaft verbunden und die Internetverbindung optimiert.
- Das Hauptrechenzentrum ist vollständig mit VMware virtualisiert. Sie verfügen über zwei ESXi 6.5-Virtualisierungshosts, die von vCenter Server 6.5 verwaltet werden.
- Für die Identitätsverwaltung verwendet Contoso Active Directory sowie DNS-Server im internen Netzwerk.
- Die Domänencontroller im Rechenzentrum werden auf VMware-VMs ausgeführt. Die Domänencontroller in lokalen Niederlassungen werden auf physischen Servern ausgeführt.


## <a name="step-1-buy-and-subscribe-to-azure"></a>Schritt 1: Kauf und Abonnement von Azure

Contoso muss entscheiden, wie Azure erworben werden soll, wie die Architektur von Abonnements ausgelegt wird und wie Dienste und Ressourcen lizenziert werden.

### <a name="buy-azure"></a>Kauf von Azure

Contoso entscheidet sich für ein [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Dies bringt eine im Voraus zu leistende finanzielle Verpflichtung für Azure mit sich, berechtigt Contoso aber zu großen Vorteilen, einschließlich flexibler Abrechnungsoptionen und einer optimierten Preisgestaltung.

- Contoso hat eingeschätzt, in welcher Höhe sich ihre jährlichen Ausgaben für Azure bewegen werden. Bei Vertragsabschluss haben sie das erste Jahr ganz bezahlt.
- Contoso muss alle bestehenden Übertragungen vor Ablauf des Jahres verwenden, andernfalls verlieren sie den Gegenwert für den entrichteten Preis.
- Wenn die Übertragungen aus gleich welchen Gründen überschritten werden, stellt Microsoft ihnen die Differenz in Rechnung.
- Alle jenseits der Übertragung anfallenden Kosten werden zu den gleichen Preisen wie im Vertrag vereinbart berechnet. Für Überschreitungen werden keine Strafen fällig.

### <a name="manage-subscriptions"></a>Verwalten von Abonnements

Nach dem Erwerb von Azure muss Contoso herausfinden, wie die Abonnements verwaltet werden sollen. Sie verfügen über ein EA und sind daher in der Zahl der Azure-Abonnements, die sie einrichten können, nicht beschränkt.

- Eine Azure Enterprise-Registrierung definiert, wie ein Unternehmen Azure-Dienste modelliert und verwendet und legt eine Kernstruktur für Governance fest.
- Als ersten Schritt hat Contoso eine Struktur für ihre Enterprise-Registrierung bestimmt (als Unternehmensgerüst bezeichnet). [Dieser Artikel](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance) war ihnen beim Verständnis und dem Entwurf eines Gerüsts nützlich.
- Fürs Erste hat sich Contoso für einen funktionalen Ansatz beim Verwalten der Abonnements entschieden.
    - Im Unternehmen gibt es eine zentrale IT-Abteilung, die auch die Kontrolle über das Azure-Budget innehat. Diese Gruppe hält als einzige Abonnements.
    - Dieses Modell soll in Zukunft erweitert werden, sodass andere Gruppen im Unternehmen als Abteilungen an der Enterprise-Registrierung teilnehmen können.
    - Innerhalb der IT-Abteilung hat Contoso zwei Abonnements strukturiert, Produktion und Entwicklung.
    - Wenn Contoso zukünftig zusätzliche Abonnements benötigt, müssen sie den Zugriff, die Richtlinien und die Compliance für diese Abonnements verwalten. Dies ist durch die Einführung von [Azure-Verwaltungsgruppen](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview) als zusätzliche Ebene oberhalb der Abonnements möglich.

    ![Enterprise-Struktur](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Untersuchen der Lizenzierung

Nach der Konfiguration der Abonnements kann sich Contoso mit der Microsoft-Lizenzierung befassen. Ihre Lizenzierungsstrategie hängt von den Ressourcen ab, die sie nach Azure migrieren möchten, und von der Auswahl und der Art der Bereitstellung der Azure-VMs und -Dienste. 

#### <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Bei der Bereitstellung von VMs in Azure beinhalten Standardimages eine Lizenz, die Contoso die verwendete Software pro Minute in Rechnung stellt. Contoso ist jedoch seit langem Microsoft-Kunde und hat EAs und Lizenzen im Open License-Programm mit Software Assurance (SA) unterhalten. 

Der Azure-Hybridvorteil stellt eine kostengünstige Methode für die Migration von Contoso zur Verfügung und ermöglicht Einsparungen bei Workloads von Azure-VMs und SQL Server durch Umwandlung oder Wiederverwendung von Lizenzen der Windows Server Datacenter- und Standard-Editionen, die durch Software Assurance abgedeckt sind. Dadurch kann Contoso einen niedriger angesetzten Computesatz für VMs und SQL Server bezahlen. [Weitere Informationen](https://azure.microsoft.com/pricing/hybrid-benefit/)


#### <a name="license-mobility"></a>Lizenzmobilität

Lizenzmobilität durch SA bietet Microsoft-Volumenlizenz-Kunden wie Contoso die Flexibilität, berechtigte Serveranwendungen mit aktiver SA in Azure bereitzustellen. Hierdurch entfällt die Notwendigkeit zum Kauf neuer Lizenzen. Ohne anfallende Mobilitätsgebühren können die vorhandenen Lizenzen auf einfache Weise in Azure bereitgestellt werden. [Weitere Informationen](https://azure.microsoft.com/pricing/license-mobility/)

#### <a name="reserve-instances-for-predictable-workloads"></a>Reservieren von Instanzen für vorhersehbare Workloads

Vorhersehbare Workloads sind solche, die bei laufenden VMs jederzeit verfügbar sein müssen. Beispielsweise Branchenanwendungen wie ein SAP-ERP-System.  Demgegenüber sind nicht vorhersehbare Workloads solche, die variabel sind. Etwa VMs, die bei hohem Bedarf aktiv und außerhalb von Spitzenzeiten inaktiv sind.

![Reservierte Instanz](./media/contoso-migration-infrastructure/reserved-instance.png) 

Im Tausch für die Verwendung von reservierten Instanzen für bestimmte VM-Instanzen, von denen sie wissen, dass sie über lange Zeitabschnitte hinweg aufrecht erhalten werden müssen, kann Contoso sowohl einen Rabatt als auch priorisierte Kapazität erhalten. Mit [reservierten Azure-Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Kombination mit dem Azure-Hybridvorteil kann Contoso gegenüber der regulären nutzungsbasierten Bezahlung bis zu 82 % sparen (Stand April 2018).


## <a name="step-2-manage-hybrid-identity"></a>Schritt 2: Verwalten von Hybrididentität

Das Erteilen und Steuern des Benutzerzugriffs auf Azure-Ressourcen mit Identity & Access Management (IAM) ist ein wichtiger Schritt bei der Zusammenstellung Ihrer Azure-Infrastruktur.  

- Contoso hat sich entschieden, das vorhandene lokale Active Directory in die Cloud zu erweitern, statt ein neues, separates System in Azure aufzubauen.
- Zu diesem Zweck erstellen sie ein Azure-basiertes Active Directory.
- Bei Contoso ist Office 365 nicht implementiert, daher muss ein neues Azure AD bereitgestellt werden.
- Office 365 verwendet Azure AD für die Benutzerverwaltung. Falls Contoso Office 365 einsetzte, wäre bereits ein Azure AD-Mandant vorhanden, der als primäres AD verwendet werden könnte.
- [Erfahren Sie mehr](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) über Azure AD für Office 365 und über das [Hinzufügen eines Abonnements](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) zu einem vorhandenen Azure AD.

### <a name="create-an-azure-ad"></a>Erstellen eines Azure ADs

Contoso verwendet die Azure AD Free Edition, die in Azure-Abonnements enthalten ist. Contoso fügt ein neues AD-Verzeichnis wie folgt hinzu:

1. Sie navigieren im [Azure-Portal](http://portal.azure.com/) zu **Ressource erstellen** > **Identität** > **Azure Active Directory**.
2. Sie geben in **Verzeichnis erstellen** einen Namen für das Verzeichnis ein, einen Anfangsdomänennamen und eine Region, in der das Azure AD-Verzeichnis erstellt werden soll.

    ![Azure AD erstellen](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Wenn das Verzeichnis erstellt wird, weist es den Namen einer Anfangsdomäne im Format „domaenenname.onmicrosoft.com“ auf. Der Name kann weder geändert noch gelöscht werden. Stattdessen muss Contoso Azure AD seinen registrierten Domänennamen hinzufügen.

### <a name="add-the-domain-name"></a>Hinzufügen des Domänennamens

Damit sie ihren Standarddomänennamen verwenden können, muss Contoso ihn als benutzerdefinierten Namen zu Azure AD hinzufügen. Diese Option ermöglicht es Administratoren, vertraute Benutzernamen zuzuweisen. Beispielsweise kann sich ein Benutzer mit der E-Mail-Adresse billg@contoso.com anmelden, statt auf billg@contosomigration.onmicrosoft.com angewiesen zu sein. 

Um den benutzerdefinierten Namen einzurichten, fügen sie ihn dem Verzeichnis hinzu, fügen einen DNS-Eintrag hinzu und bestätigen den Namen dann in Azure AD.

1. Sie fügen die Domäne unter **Benutzerdefinierte Domänennamen** > **Benutzerdefinierte Domäne hinzufügen** hinzu.
2. Um einen DNS-Eintrag in Azure verwenden zu können, müssen sie ihn bei ihrer Domänenregistrierungsstelle registrieren. 

    - Sie notieren sich in der Liste **Benutzerdefinierte Domänennamen** die DNS-Informationen zu dem Namen. Contoso verwendet einen MX-Eintrag.
    - Zu diesem Zweck benötigen sie Zugriff auf den Namenserver. Im Fall Contoso haben sie sich bei der Domäne „Contoso.com“ angemeldet und mithilfe der notierten Detailinformationen einen neuen MX-Eintrag für den von Azure AD bereitgestellten DNS-Eintrag erstellt.  
1. Nachdem die DNS-Einträge verteilt wurden, klicken sie im Detailnamen für die Domäne auf **Überprüfen**, um den benutzerdefinierten Namen zu überprüfen.

     ![Azure AD-DNS](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Einrichten von Gruppen und Benutzern lokal und in Azure

Jetzt, da ihr Azure AD betriebsbereit ist, muss Contoso den lokalen AD-Gruppen, die mit Azure AD synchronisiert werden sollen, Mitarbeiter hinzufügen. Wir empfehlen, lokale Gruppennamen zu verwenden, die mit den Namen von Ressourcengruppen in Azure übereinstimmen. Dies erleichtert die Identifikation von Übereinstimmungen zu Synchronisierungszwecken.

#### <a name="create-resource-groups-in-azure"></a>Erstellen von Ressourcengruppen in Azure

Azure-Ressourcengruppen fassen Azure-Ressourcen zu Gruppen zusammen. Mithilfe einer Ressourcengruppen-ID kann Azure Vorgänge für Ressourcen in der betreffenden Gruppe ausführen.

- Ein Azure-Abonnement kann mehrere Ressourcengruppen aufweisen, aber eine Ressourcengruppe kann nur innerhalb eines einzelnen Abonnements vorkommen.
- Darüber hinaus kann eine einzelne Ressourcengruppe über mehrere Ressourcen verfügen, eine Ressource kann jedoch jeweils nur einer einzelnen Gruppe angehören.

Contoso richtet Azure-Ressourcengruppen wie in der folgenden Tabelle zusammengefasst ein.

**Ressourcengruppe** | **Details**
--- | ---
**ContosoCobRG** | Diese Gruppe enthält alle Ressourcen, die mit der Geschäftskontinuität (Continuity of Business, COB) zusammenhängen.  Sie enthält Tresore, die Contoso bei der Verwendung des Azure Site Recovery-Diensts und des Azure Backup-Diensts erstellt.<br/><br/> Sie enthält außerdem Ressourcen, die für die Migration verwendet werden, einschließlich des Azure Migrate-Diensts und der Database Migration Services.
**ContosoDevRG** | Diese Gruppe enthält Ressourcen für Entwicklung und Tests.
**ContosoFailoverRG** | Diese Gruppe dient als Landungszone für Ressourcen bei einem Failovervorgang.
**ContosoNetworkingRG** | Diese Gruppe enthält alle Netzwerkressourcen.
**ContosoRG** | Diese Gruppe enthält Ressourcen im Zusammenhang mit Produktionsanwendungen und -datenbanken.

Die Ressourcengruppen werden in folgender Weise erstellt:

1. Sie fügen im Azure-Portal > **Ressourcengruppen** eine Gruppe hinzu.
2. Sie geben für jede Gruppe einen Namen, das Abonnement, zu dem die Gruppe gehört und die Region an.
3. Ressourcengruppen werden in der Liste **Ressourcengruppen** angezeigt.

    ![Ressourcengruppen](./media/contoso-migration-infrastructure/resource-groups.png) 


#### <a name="create-matching-security-groups-on-premises"></a>Erstellen von übereinstimmenden lokalen Sicherheitsgruppen

1. In ihrem lokalen Active Directory richtet Contoso Sicherheitsgruppen ein, deren Namen mit den Namen der Azure-Ressourcengruppen übereinstimmen.
 
    ![Lokale AD-Sicherheitsgruppen](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Zu Verwaltungszwecken erstellen sie eine zusätzliche Gruppe, die allen anderen Gruppen hinzugefügt wird. Dieser Gruppe werden Rechte an allen Ressourcengruppen in Azure erteilt. Der Gruppe wird eine begrenzte Zahl globaler Administratoren hinzugefügt.

### <a name="synchronize-ad"></a>Synchronisieren von AD

Contoso möchte eine gemeinsame Identität für den Zugriff auf lokale Ressourcen und Ressourcen in der Cloud bereitstellen. Zu diesem Zweck wird das lokale Active Directory in Azure AD integriert. Dieses Modell ermöglicht Folgendes:

- Benutzer und Organisationen können eine einzelne Identität für den Zugriff auf lokale Anwendungen und Clouddienste wie Office 365 oder Tausende weiterer Websites im Internet nutzen.
- Administratoren können die Gruppen in AD nutzen, um [Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure zu implementieren.

Um die Integration zu erleichtern, verwendet Contoso das [Azure AD Connect-Tool](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Nach der Installation und Konfiguration auf einem Domänencontroller synchronisiert das Tool die lokalen AD-Identitäten mit dem Azure AD. 

### <a name="download-the-tool"></a>Herunterladen des Tools

1. Im Azure-Portal navigiert Contoso zu **Azure Active Directory** > **Azure AD Connect** und lädt die neueste Version des Tools auf den Server herunter, der für die Synchronisierung verwendet werden soll.

    ![AD Connect herunterladen](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Sie starten die Installation von **AzureADConnect.msi** mit der Option **Express-Einstellungen verwenden**. Dies ist die gebräuchlichste Installation, die für Topologien mit einer einzelnen Gesamtstruktur und Synchronisierung von Kennworthashes zur Authentifizierung verwendet werden kann.

    ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. In **Mit Azure AD verbinden** geben sie die Anmeldeinformationen für das Herstellen der Verbindung mit dem Azure AD an (im Format „CONTOSO\admin“ oder „contoso.com\admin“).

    ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. In **Herstellen einer Verbindung mit AD DS** geben sie Anmeldeinformationen für ihr lokales AD an.

     ![AD Connect-Assistent](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. In **Bereit zur Konfiguration** klicken sie auf **Starten Sie den Synchronisierungsvorgang, nachdem die Konfiguration abgeschlossen wurde**, um die Synchronisierung sofort zu starten. Anschließend führen sie die Installation durch.


- Contoso hat eine direkte Verbindung mit Azure. Wenn sich das lokale AD hinter einem Proxy befindet, lesen Sie diesen [Artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- Nach der ersten Synchronisierung sind lokale AD-Objekte im Azure AD sichtbar.

    ![Lokales AD in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Das IT-Team von Contoso ist in jeder Gruppe vertreten, nach Maßgabe seiner Rolle.

    ![Lokale AD-Mitglieder in Azure](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>Einrichten von RBAC

Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Benutzern nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen. Sie weisen Benutzern, Gruppen und Anwendungen auf Bereichsebene eine passende RBAC-Rolle zu. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. 

Contoso weist jetzt den AD-Gruppen, die aus der lokalen Infrastruktur synchronisiert wurden, Rollen zu.

1. In der **ControlCobRG**-Ressourcengruppe klicken sie auf **Zugriffssteuerung (IAM)** > **Hinzufügen**.
2. In **Berechtigungen hinzufügen** > **Rolle** wählen sie **Mitwirkender** und dann die AD-Gruppe **ContosoCobRG** in der Liste aus. Die Gruppe wird dann in der Liste **Ausgewählte Mitglieder** angezeigt. 
3. Sie wiederholen das mit den gleichen Berechtigungen für die anderen Ressourcengruppen (mit Ausnahme von **ContosoAzureAdmins**), indem sie dem AD-Konto, das der Ressourcengruppe entspricht, die Mitwirkender-Berechtigungen hinzufügen.
4. Für die **ContosoAzureAdmins**-AD-Gruppe weisen sie die Rolle **Besitzer** zu.

    ![Lokale AD-Mitglieder in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>Schritt 3: Auslegung für Ausfallsicherheit und Notfälle

Azure-Ressourcen werden in Regionen bereitgestellt.
- Regionen sind in Geografien organisiert, und Datenresidenz, Datenhoheit, Compliance und Ausfallsicherheit werden innerhalb von geografischen Grenzen eingelöst.
- Eine Region setzt sich aus einer Reihe von Rechenzentren zusammen. Diese Rechenzentren werden innerhalb eines durch Latenz definierten Umkreises bereitgestellt und sind über ein dediziertes regionales Netzwerk mit geringer Latenz verbunden.
- Jede Azure-Region ist zwecks Ausfallsicherheit mit einer anderen Region gepaart.
- Lesen Sie mehr über [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/), und verstehen Sie, [wie Regionen gepaart werden](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso haben sich für USA, Osten 2 (mit Standort in Virginia) als primäre Region und für USA, Mitte als sekundäre Region entschieden. Für diese Entscheidung gibt es eine Reihe von Gründen:

- Das Contoso-Rechenzentrum befindet sich in New York, und die Latenz zum nächstgelegenen Rechenzentrum wurde berücksichtigt.
- Die Region USA, Osten 2 verfügt über alle Dienste und Produkte, auf die sie angewiesen sind. In Bezug auf die verfügbaren Produkte und Dienste sind nicht alle Azure-Regionen identisch. Hier können Sie die [Azure-Produkte nach Regionen](https://azure.microsoft.com/global-infrastructure/services/) überprüfen.
- USA, Mitte ist die gepaarte Region für USA, Osten 2 in Azure.

Bei der Planung seiner Hybridumgebung muss Contoso erwägen, wie Ausfallsicherheit und eine Strategie zur Notfallwiederherstellung in seinem Regionsentwurf verwirklicht werden können. Allgemein reichen die Strategien von einer Bereitstellung in einer einzelnen Region, die sich für die Ausfallsicherheit auf Features der Azure-Plattform wie Fehlerdomänen und Paarung von Regionen verlässt, bis hin zu einem vollständigen Aktiv/Aktiv-Modell, in dem Clouddienste und Datenbanken in zwei Regionen bereitgestellt sind und auch Benutzer aus beiden Regionen bedienen.

Contoso hat sich für einen Mittelweg entschieden. Sie stellen ihre Anwendungen und Ressourcen in einer primären Region bereit und unterhalten eine vollständige Infrastruktur in der sekundären Region, sodass diese bei einem schweren Notfall bei den Anwendungen oder dem Ausfall einer Region als vollständige Sicherung dienen kann.


## <a name="step-4-design-a-network-infrastructure"></a>Schritt 4: Entwerfen einer Netzwerkinfrastruktur

Nachdem das Regionskonzept festgelegt ist, kann Contoso sich jetzt der Netzwerkstrategie zuwenden. Sie müssen sich überlegen, wie sich ihr lokales Rechenzentrum und Azure miteinander verbinden und kommunizieren und wie die Netzwerkinfrastruktur in Azure ausgelegt werden soll. Insbesondere ist Folgendes erforderlich:

**Planen der Hybridnetzwerk-Konnektivität**: Herausfinden, wie die Netzwerkverbindung zwischen der lokalen Infrastruktur und Azure funktionieren soll.
**Entwerfen einer Azure-Netzwerkinfrastruktur**: Entscheiden, wie sie Netzwerke in ihren Regionen bereitstellen. Wie die Netzwerkkommunikation innerhalb der gleichen Region und regionsübergreifend erfolgen soll.
**Entwerfen und Einrichten von Azure-Netzwerken**: Einrichten der Azure-Netzwerke und Subnetze und entscheiden, was darin gespeichert werden soll.

### <a name="plan-hybrid-network-connectivity"></a>Planen der Hybridnetzwerk-Konnektivität

Contoso hat eine [Reihe von Architekturen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) für das Hybridnetzwerk zwischen Azure und ihrem lokalen Rechenzentrum in Erwägung gezogen. [Weitere Informationen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) über das Vergleichen von Optionen.

Zur Erinnerung: Die lokale Netzwerkinfrastruktur von Contoso besteht derzeit aus ihrem Rechenzentrum in New York und lokalen Niederlassungen im östlichen Teil der USA.  Alle Standorte verfügen über eine Business-Class-Verbindung zum Internet.  Jede dieser Niederlassungen ist mit dem Rechenzentrum durch einen IPSec-VPN-Tunnel durch das Internet verbunden.

![Contoso-Netzwerk](./media/contoso-migration-infrastructure/contoso-networking.png) 

Hier sehen Sie, für welche Implementierung von Hybridkonnektivität sich Contoso entschieden hat:

1. Einrichten einer neuen VPN-Verbindung von Standort zu Standort zwischen dem Contoso-Rechenzentrum in New York und den zwei Azure-Regionen in USA, Osten 2 und USA, Mitte.
2. Datenverkehr aus den Niederlassungen, die die virtuellen Azure-Netzwerke als Ziel haben, wird durch das Contoso-Hauptrechenzentrum geleitet. 
3. Bei der Hochskalierung ihrer Azure-Bereitstellung richten sie eine ExpressRoute-Verbindung zwischen ihrem Rechenzentrum und den Azure-Regionen ein. In diesem Fall wird die VPN-Verbindung von Standort zu Standort nur für Failoverzwecke beibehalten.
    - [Erfahren Sie mehr](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) über die Wahl zwischen einer VPN- und einer ExpressRoute-Hybridlösung.
    - Überprüfen Sie [ExpressRoute-Standorte und -Unterstützung](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Nur VPN**

![Contoso-VPN](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN und ExpressRoute**

![Contoso-VPN/ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>Entwerfen der Azure-Netzwerkinfrastruktur

Es ist von entscheidender Bedeutung, dass Contoso seine Netzwerke in einer Weise implementiert, die seine Hybridbereitstellung sicher und skalierbar macht. Zu diesem Zweck wählt Contoso einen langfristigen Ansatz und entwirft virtuelle Netzwerke (VNets) mit dem Augenmerk auf Ausfallsicherheit und Unternehmenstauglichkeit. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) zum Planen von VNets.

Für die Verbindung seiner zwei Regionen hat sich Contoso für ein Hub-zu-Hub-Netzwerkmodell (Nabe zu Nabe) entschieden:

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

Innerhalb des Hub-and-Spoke-Modells, für das sich Contoso entschieden hat, muss noch über das Routing von Datenverkehr aus dem lokalen Rechenzentrum und aus dem Internet entschieden werden. Für diese Routinglösung hat sich Contoso für die Hubs in USA, Osten 2 und USA, Mitte entschieden:

- Sie entwerfen ein Netzwerk, das als „reverse c“ (umgekehrtes C) bekannt ist, denn dies ist der Pfad, dem die Pakete aus dem eingehenden in das ausgehende Netzwerk folgen.
- Ihre Netzwerkarchitektur weist zwei Grenzen auf, eine nicht vertrauenswürdige Front-End-Umkreiszone und eine vertrauenswürdige Back-End-Zone.
- Eine Firewall weist einen Netzwerkadapter in jeder Zone auf und kontrolliert den Zugriff auf die vertrauenswürdigen Zonen.
- Aus dem Internet:
    - Datenverkehr aus dem Internet trifft auf eine öffentliche IP-Adresse mit Lastenausgleich im Umkreisnetzwerk.
    - Dieser Datenverkehr wird durch die Firewall geroutet und unterliegt den Firewallregeln.
    - Nachdem die Netzwerk-Zugangskontrollen implementiert sind, wird der Verkehr an den entsprechenden Ort in der vertrauenswürdigen Zone weitergeleitet.
    - Ausgehender Verkehr aus dem VNet wird mithilfe von benutzerdefinierten Routen (User-defined Routes, UDRs) in das Internet geroutet. Der Datenverkehr durchläuft zwangsläufig die Firewall und wird auf Einhaltung der Contoso-Richtlinien geprüft.
- Aus dem Contoso-Rechenzentrum:
    - Eingehender Datenverkehr über VPN-Standort-zu-Standort (oder ExpressRoute) trifft auf die öffentliche IP-Adresse des Azure-VPN-Gateways.
    - Der Datenverkehr wird durch die Firewall geroutet und unterliegt den Firewallregeln.
    - Nach dem Anwenden der Regeln wird der Datenverkehr an ein internes Lastenausgleichsmodul (Standard-SKU) im vertrauenswürdigen Subnetz der internen Zone weitergeleitet.
    - Ausgehender Datenverkehr aus dem vertrauenswürdigen Subnetz an das lokale Rechenzentrum über VPN wird durch die Firewall geroutet, und die Regeln werden angewendet, bevor der Datenverkehr über die VPN-Standort-zu-Standort-Verbindung geleitet wird.



### <a name="design-and-set-up-azure-networks"></a>Entwerfen und Einrichten von Azure-Netzwerken

Mit festgelegter Netzwerk- und Routingtopologie ist Contoso jetzt bereit, seine Azure-Netzwerke und Subnetze einzurichten.

- Contoso implementiert ein privates Netzwerk der Klasse A in Azure (0.0.0.0 bis 127.255.255.255). Dies funktioniert, da lokal aktuell ein privater Adressraum der Klasse B implementiert ist (172.160.0/16), sodass sie sicher sein können, dass es keine Überschneidung der Adressbereiche gibt.
- Sie werden VNets in ihren primären und sekundären Regionen bereitstellen.
- Dazu kommt eine Namenskonvention zum Einsatz, die das Präfix **VNET** und die Regionsabkürzung **EUS2** oder **CUS** enthält. Bei diesem Standard erhalten die Hub-Netzwerke die Namen **VNET-HUB-EUS2** (USA, Osten 2) und **VNET-HUB-CUS** (USA, Mitte).
- Contoso verfügt nicht über eine [IPAM-Lösung](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top), daher muss das Netzwerkrouting ohne NAT geplant werden.


#### <a name="virtual-networks-in-east-us-2"></a>Virtuelle Netzwerke in USA, Osten 2

USA, Osten 2 ist die primäre Region, die Contoso zum Bereitstellen von Ressourcen und Diensten verwenden wird. So sieht die Architektur ihrer Netzwerke innerhalb dieser Region aus:

- **Hub**: Das Hub-VNet in USA, Osten 2 ist der zentrale Punkt der primären Konnektivität mit dem lokalen Rechenzentrum.
- **VNets**: Spoke-VNets in USA, Osten 2 können ggf. zum Isolieren von Workloads verwendet werden. Über das Hub-VNet hinaus wird Contoso in USA, Osten 2 zwei Spoke-VNets betreiben:
    - **VNET-DEV-EUS2**. Dieses VNet steht dem Entwicklungs- und Testteam als voll funktionsfähiges Netzwerk für Entwicklungsprojekte zur Verfügung. Es soll als Pilotbereich für die Produktion fungieren und baut in seiner Funktion auf der Produktionsinfrastruktur auf.
    - **VNET-PROD-EUS2**: Azure-IaaS-Produktionskomponenten befinden sich in diesem Netzwerk. 
    -  Jedes VNet verfügt über einen eigenen, eindeutigen Adressraum ohne Überschneidungen. Für die Konfiguration des Routings soll NAT nicht erforderlich sein.
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
**PROD-DC-EUS2** | 10.245.42.0/23 | 251 | Domänencontroller-VMs


![Hub-Netzwerkarchitektur](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Virtuelle Netzwerke in USA, Mitte (sekundäre Region)

USA Mitte ist die sekundäre Region von Contoso. So sieht die Architektur ihrer Netzwerke innerhalb dieser Region aus:

- **Hub**: Das Hub-Vnet in USA, Osten 2 ist der zentrale Konnektivitätspunkt mit dem lokalen Rechenzentrum, und die Spoke-VNets in USA, Osten 2 werden getrennt von anderen Spokes verwaltet und können ggf. zur Isolierung von Workloads verwendet werden.
- **VNets**: Sie werden zwei VNets in USA, Mitte betreiben:
    - VNET-PROD-CUS. Diese VNet ist ein Produktionsnetzwerk, ähnlich wie VNET-PROD_EUS2. 
    - VNET-ASR-CUS. Diese VNet fungiert als Ort, an dem nach einem Failover von der lokalen Infrastruktur VMs erstellt werden, oder als Ort für Azure-VMs, bei denen ein Failover von der primären zur sekundären Region eingetreten ist. Diese Netzwerk ist ähnlich wie die Produktionsnetzwerke, weist aber keine Domänencontroller auf.
    -  Jedes VNet in der Region verfügt über einen eigenen Adressraum ohne Überschneidungen. Für die Konfiguration des Routings soll NAT nicht erforderlich sein.
- **Subnetze**: Die Architektur der Subnetze ähnelt der in USA, Osten 2. Mit dem Unterschied, dass sie kein Subnetz für Domänencontroller benötigen.

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

VNET-ASR-CUS ist das gleiche einfache Subnetz wie das Produktions-VNET in USA, Osten 2, kommt jedoch ohne ein Domänencontroller-Subnetz aus.

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

Hinsichtlich der Domänencontroller im Netzwerk VNET-PROD-EUS2 wünscht Contoso den Fluss des Datenverkehrs zwischen dem EUS2-Hub-/Produktionsnetzwerk und über die VPN-Verbindung zum lokalen Rechenzentrum. Dazu müssen die folgenden Dinge erlaubt werden:

1. **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen** für die Verbindung mit Peering. In unserem Beispiel wäre das die Verbindung von VNET-HUB-EUS2 mit VNET-PROD-EUS2.

    ![Peering](./media/contoso-migration-infrastructure/peering1.png)

2. **Weitergeleiteten Datenverkehr zulassen** und **Remotegateways verwenden** auf der anderen Seite der Peeringverbindung, für die Verbindung von VNET-PROD-EUS2 mit VNET-HUB-EUS2.

    ![Peering](./media/contoso-migration-infrastructure/peering2.png)

3. Lokal wird eine statische Route eingerichtet, die den zu routenden lokalen Datenverkehr über den VPN-Tunnel in das VNet leitet. Die Konfiguration würde auf dem Gateway abgeschlossen, das den VPN-Tunnel von Contoso zu Azure bereitstellt. Contoso verwendet dazu den Windows Routing- und RAS-Dienst.

    ![Peering](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Produktionsnetzwerke 

Ein Peernetzwerk mit Spokes kann ein Peernetzwerk mit Spokes in einer anderen Region über einen Hub nicht sehen.

Damit die Contoso-Produktionsnetzwerke in beiden Regionen einander sehen können, muss eine direkte Verbindung mit Peering zwischen VNET-PROD-EUS2 und VNET-PROD-CUS erstellt werden. 

![Peering](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>Einrichten von DNS

Beim Bereitstellen von Ressourcen in virtuellen Netzwerken bietet sich für die Domänennamenauflösung eine Reihe von Optionen. Sie können die in Azure verfügbare Namensauflösung verwenden oder DNS-Server für die Auflösung bereitstellen. Welche Art der Namensauflösung Sie verwenden, hängt davon ab, wie die Ressourcen miteinander kommunizieren müssen. Hier finden Sie [weitere Informationen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) über den Azure DNS-Dienst.

Contoso hat entschieden, dass der Azure DNS-Dienst keine gute Wahl für seine Hybridumgebung darstellt. Stattdessen sollen die lokalen DNS-Server genutzt werden.

- Da es sich um ein Hybridnetzwerk handelt, müssen alle VMs – lokal wie in Azure – in der Lage sein, Namen aufzulösen, um ordnungsgemäß zu funktionieren. Dies bedeutet, dass auf alle VNets benutzerdefinierte DNS-Einstellungen angewendet werden müssen.
- Contoso verfügt derzeit über bereitgestellte DCs im Contoso-Rechenzentrum und in den Niederlassungen. Die primären DNS-Server sind CONTOSODC1(172.16.0.10) und CONTOSODC2(172.16.0.1)
- Wenn die VNets bereitgestellt werden, werden die lokalen Domänencontroller für die Verwendung als DNS-Server in den Netzwerken festgelegt. 
- Um dies zu konfigurieren, muss bei Verwendung eines benutzerdefinierten DNS im VNet der DNS-Liste die IP-Adresse des rekursiven Azure-Resolvers (z. B. 168.63.129.16) hinzugefügt werden.  Dazu konfiguriert Contoso die DNS-Servereinstellungen auf jedem VNet. Beispielsweise wären dies die benutzerdefinierten DNS-Einstellungen für das VNET-HUB-EUS2-Netzwerk:
    
    ![Benutzerdefinierter DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Über ihre lokalen Domänencontroller hinaus wird Contoso vier weitere zur Unterstützung seiner Azure-Netzwerke hinzufügen, zwei für jede Region. So sieht das aus, was in Azure bereitgestellt wird:

**Region** | **DC** | **VNET** | **Subnetz** | **IP-Adresse**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.5
CUS | CONTOSODC5 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4
CUS | CONTOSODC6 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4

Nach der Bereitstellung der lokalen Domänencontroller muss Contoso die DNS-Einstellungen in den Netzwerken beider Regionen aktualisieren, damit die neuen Domänencontroller in den Listen der DNS-Server vorhanden sind.



#### <a name="set-up-domain-controllers-in-azure"></a>Einrichten von Domänencontrollern in Azure

Nach der Aktualisierung der Netzwerkeinstellungen ist Contoso bereit, die Domänencontroller in Azure zu implementieren.

1. Im Azure-Portal stellen sie eine neue Windows Server-VM im passenden VNet bereit.
2. Sie erstellen an jedem Standort Verfügbarkeitsgruppen für die VM. Verfügbarkeitsgruppen haben diese Aufgaben:
    - Sie stellen sicher, dass die Azure-Gesamtstruktur die VMs in verschiedene Infrastrukturen in der Azure-Region aufteilt. 
    -  Sie ermöglichen Contoso die Qualifikation für die 99,95 %-SLA für VMs in Azure.  [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)

    ![Verfügbarkeitsgruppe](./media/contoso-migration-infrastructure/availability-group.png) 
3. Nach der Bereitstellung der VM legen sie die Netzwerkschnittstelle für die VM fest. In diesem Fall wird die private IP-Adresse als statisch festgelegt und eine gültige Adresse angegeben.

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

AD ist ein kritischer Dienst im Netzwerk und muss ordnungsgemäß konfiguriert werden. Contoso erstellt AD-Standorte für das Contoso-Rechenzentrum und für die Regionen EUS2 und CUS.  

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

- **Policy**: Policy wendet in Azure Regeln und Wirkungen auf Ihre Ressourcen an und setzt sie durch, sodass die Ressourcen sich konform zu Unternehmensanforderungen und SLAs verhalten.
- **Sperren**: Azure erlaubt das Sperren von Abonnements, Ressourcengruppen und sonstigen Ressourcen, sodass diese nur von Personen geändert werden können, die dazu berechtigt sind.
- **Tags**: Ressourcen können mithilfe von Tags gesteuert, überwacht und verwaltet werden. Tags fügen Ressourcen Metadaten an, die Informationen über Ressourcen oder Besitzer bereitstellen.

### <a name="set-up-policies"></a>Einrichten von Richtlinien

Der Azure Policy-Dienst führt eine Bewertung von Ressourcen durch, um die zu ermitteln, die nicht mit den implementierten Richtliniendefinitionen kompatibel sind. Nehmen Sie an, es wäre eine Richtlinie implementiert, die nur bestimmte Typen von VMs zulässt oder für Ressourcen ein bestimmtes Tag vorschreibt. 

Azure-Richtlinien legen eine Richtliniendefinition fest, und die Richtlinienzuweisung gibt den Umfang an, in dem eine Richtlinie angewendet werden soll. Der Umfang kann von einer Verwaltungsgruppe bis zu einer Ressourcengruppe reichen. [Weitere Informationen](https://docs.microsoft.com/azure/azure-policy/create-manage-policy) zum Erstellen und Verwalten von Richtlinien.

Contoso möchte mit zwei Richtlinien beginnen:

- Eine Richtlinie soll sicherstellen, dass Ressourcen nur in den Regionen EUS2 und CUS bereitgestellt werden können.
- Dadurch sollen die VM-SKUs auf genehmigte SKUs beschränkt werden. Die Absicht dahinter ist, den Einsatz von teuren VM-SKUs zu verhindern.

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

Contoso hat lange Zeit das ITIL-Framework für die Verwaltung seiner Systeme eingesetzt. Einer der wichtigsten Aspekte des Frameworks ist die Änderungssteuerung, und Contoso möchte sicherstellen, dass Änderungssteuerung in seiner Azure-Bereitstellung implementiert wird.

Contoso wird Sperren in folgender Weise implementieren:

- Alle Produktions- oder Failoverkomponenten müssen sich in einer Ressourcengruppe befinden, die eine Schreibschutzsperre aufweist.  Das bedeutet, dass die Sperre zum Ändern oder Löschen von Produktionselementen entfernt werden muss. 
- Nicht zur Produktion gehörende Ressourcengruppen erhalten CanNotDelete-Sperren. Damit können berechtigte Benutzer eine Ressource lesen oder ändern, sie aber nicht löschen.

[Weitere Informationen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) über Sperren.

### <a name="set-up-tagging"></a>Einrichten des Tagging

Um Ressourcen beim Hinzufügen nachzuverfolgen, wird es für Contoso zunehmend wichtig, sie einer entsprechenden Abteilung, einem Kunden und einer Umgebung zuzuordnen. 

Über das Bereitstellen von Informationen über Ressourcen und Besitzer hinaus geben Tags Contoso die Möglichkeit, Ressourcen zusammenzufassen und zu gruppieren und diese Daten zur verbrauchsbasierten Kostenzuteilung zu verwenden.

Contoso muss seine Azure-Ressourcen auf eine Weise visualisieren, die für das Unternehmen sinnvoll ist. Beispielsweise nach Rolle oder Abteilung. Beachten Sie, dass Ressourcen gleiche Tags aufweisen können, obwohl sie sich nicht in der gleichen Ressourcengruppe befinden. Zu diesem Zweck erstellt Contoso eine einfache Taxonomie für Tags, damit alle die gleichen Tags verwenden.

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


## <a name="step-6-consider-security"></a>Schritt 6: Sicherheitsaspekte

Sicherheit ist in der Cloud entscheidend, und Azure bietet eine große Bandbreite an Sicherheitstools und -funktionen. Diese unterstützen Sie beim Aufbau von sicheren Lösungen auf der sicheren Azure-Plattform. Lesen Sie [Sicher in der vertrauenswürdigen Cloud](https://azure.microsoft.com/overview/trusted-cloud/), um mehr über Azure-Sicherheit zu erfahren.

Für Contoso ist eine Reihe von Hauptaspekten zu berücksichtigen

- **Azure Security Center**Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Mit Security Center können Sie Sicherheitsrichtlinien für Ihre Workloads anwenden, die Angriffsfläche für Bedrohungen verringern sowie Angriffe erkennen und darauf reagieren.  [Weitere Informationen](https://docs.microsoft.com/azure/security-center/security-center-intro)
- **Netzwerksicherheitsgruppen (NSGs)**: Eine NSG ist ein Filter (eine Firewall), der eine Liste mit Sicherheitsregeln enthält, deren Anwendung zur Zulassung oder Ablehnung von Netzwerkverkehr an mit Azure-VNets verbundene Ressourcen führt. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-network/security-overview)
- **Datenverschlüsselung**: Azure Disk Encryption ist eine Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. [Weitere Informationen](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

### <a name="work-with-the-azure-security-center"></a>Arbeiten mit dem Azure Security Center

Contoso sucht nach einem schnellen Überblick über die Sicherheitslage seiner neuen Hybridcloud, insbesondere seiner Azure-Workloads.  Daher hat sich Contoso für die Implementierung von Azure Security Center entschieden, zunächst mit diesen Features: 

- Zentrale Richtlinienverwaltung
- Kontinuierliche Bewertung
- Umsetzbare Empfehlungen 

#### <a name="centralize-policy-management"></a>Zentrale Richtlinienverwaltung

Mit der zentralen Richtlinienverwaltung stellt Contoso Konformität mit Sicherheitsanforderungen durch die zentrale Verwaltung von Sicherheitsrichtlinien in der gesamten Umgebung sicher. Sie können schnell und einfach eine Richtlinie implementieren, die für alle ihre Azure-Ressourcen gilt.

![Sicherheitsrichtlinie](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Bewertung und Aktion

Contoso nutzt die kontinuierliche Sicherheitsbewertung, die die Sicherheit von Computern, Netzwerken, Massenspeichern, Daten und Anwendungen überwacht, um mögliche Sicherheitsprobleme zu entdecken. 

- Security Center analysiert den Sicherheitszustand der Compute-, Infrastruktur- und Datenressourcen von Contoso sowie der Azure-Apps und -Dienste.
- Die kontinuierliche Bewertung hilft dem Contoso-Betriebsteam dabei, potenzielle Sicherheitsprobleme zu erkennen, z.B. Systeme mit fehlenden Sicherheitsupdates oder ungeschützten Netzwerkports. 
- Insbesondere möchte Contoso sicherstellen, dass sämtliche VMs geschützt sind. Security Center ist hier eine Hilfe, indem es die Integrität von VMs überprüft und nach Priorität geordnete, umsetzbare Empfehlungen zum Korrigieren von Sicherheitsrisiken macht, bevor diese ausgenutzt werden.

![Überwachung](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Arbeiten mit NSGs

Contoso kann durch Verwendung einer Netzwerksicherheitsgruppe den Netzwerkdatenverkehr auf Ressourcen in einem virtuellen Netzwerk beschränken.

- Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen.
- Bei Anwendung auf ein Subnetz gelten Regeln für alle Ressourcen des Subnetzes. Über die Netzwerkschnittstellen hinaus schließt dies auch Instanzen von Azure-Diensten ein, die im Subnetz bereitgestellt sind.
- Mit Anwendungssicherheitsgruppen (ASGs) können Sie die Netzwerksicherheit als natürliche Erweiterung einer App-Struktur konfigurieren und VMs gruppieren sowie auf der Grundlage dieser Gruppen Netzwerksicherheitsrichtlinien definieren.
    - Anwendungssicherheitsgruppen ermöglichen Ihnen die bedarfsabhängige Wiederverwendung Ihrer Sicherheitsrichtlinie, ohne dass Sie explizite IP-Adressen manuell warten müssen. Die Plattform übernimmt die komplexe Verarbeitung von expliziten IP-Adressen und mehreren Regelsätzen, damit Sie sich auf Ihre Geschäftslogik konzentrieren können.
    - Sie können eine Anwendungssicherheitsgruppe in einer Sicherheitsregel als Quelle und Ziel angeben. Nachdem Ihre Sicherheitsrichtlinie definiert ist, können Sie VMs erstellen und die VM-NICs einer Gruppe zuweisen. 


Contoso implementiert eine Mischung aus NSGs und ASGs. Sie haben Bedenken hinsichtlich der NSG-Verwaltung. Sie befürchten einen übermäßigen Einsatz von NSGs mit entsprechender Komplexität, der sich dann das Betriebsteam ausgesetzt sieht.  Vor diesem Hintergrund haben sie zwei Schlüsselprinzipien etabliert, die als allgemeine Regeln zur Anwendung kommen:

- Der gesamte ein- und ausgehende Datenverkehr aller Subnetze (Nord-Süd) unterliegt einer NSG-Regel, mit Ausnahme der Gateway-Subnetze in den Hub-Netzwerken.
- Alle Firewalls und Domänencontroller sind sowohl mit Subnetz-NSGs als auch mit NIC-NSGs geschützt.
- Auf alle Produktionsanwendungen werden ASGs angewendet.


Contoso hat ein Modell zur Veranschaulichung der Auswirkungen für seine Anwendungen erstellt.

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
- Verschlüsselung soll auf VMs angewendet werden, die Kundendaten, vertrauliche Daten oder EPI-Daten enthalten.


## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel hat Contoso seine Azure-Infrastruktur eingerichtet und Infrastrukturrichtlinien für Azure-Abonnements, Hybridsicherheit, Notfallwiederherstellung, Netzwerk Governance und Sicherheit eingerichtet oder geplant. 

Nicht alle Schritte, die Contoso hier zurückgelegt hat, sind für eine Migration in die Cloud erforderlich. Für ihren besonderen Fall wollten sie eine Netzwerkinfrastruktur planen, die für alle Migrationstypen verwendet werden kann und sicher, robust und skalierbar ist. 

Mit dieser nun implementierten Infrastruktur sind sie bereit, den nächsten Schritt zu tun und Migration auszuprobieren.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Migrationsszenario wird Contoso die [lokale zweischichtige SmartHotel-App für die Migration zu Azure bewerten](contoso-migration-assessment.md). 

---
title: Azure Security and Compliance Blueprint – IaaS-Webanwendungen gemäß DSGVO
description: Azure Security and Compliance Blueprint – IaaS-Webanwendungen gemäß DSGVO
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 08e078841a6fb7f43db6d052ecefca3aabec8687
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263372"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Azure Security and Compliance Blueprint – IaaS-Webanwendungen gemäß DSGVO

## <a name="overview"></a>Übersicht
Die Datenschutz-Grundverordnung (DSGVO) enthält viele Anforderungen für das Sammeln, Speichern und Verwenden personenbezogener Daten. Diese umfassen auch das Identifizieren und Schützen dieser Daten durch Organisationen, die Einhaltung der Transparenzanforderungen, das Erkennen und Melden von Sicherheitsverletzungen im Zusammenhang mit personenbezogenen Daten sowie das Schulen von Datenschutzbeauftragten und anderen Mitarbeitern. Die DSGVO bietet Einzelpersonen mehr Kontrolle über ihre persönlichen Daten und ist mit vielen neuen Pflichten für Organisationen verbunden, die personenbezogene Daten sammeln, verarbeiten oder analysieren. Die DSGVO enthält neue Vorschriften für Organisationen, die Menschen in der Europäischen Union (EU) Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. Die DSGVO gilt unabhängig vom Standort der Organisation.

Microsoft hat Azure basierend auf branchenweit führenden Sicherheitsmaßnahmen und Datenschutzrichtlinien zum Schutz von Daten in der Cloud entwickelt, zu denen auch personenbezogene Daten gehören, die der DSGVO unterliegen. Gemäß den eigenen [Vertragsbedingungen](https://aka.ms/Online-Services-Terms) muss Microsoft die Anforderungen von datenverarbeitenden Organisationen einhalten.

Dieser Azure Security and Compliance Blueprint bietet Anleitungen für die Bereitstellung einer IaaS-Umgebung (Infrastructure-as-a-Service), die für eine einfache, internetfähige Webanwendung geeignet ist. Diese Lösung zeigt Möglichkeiten, mit denen Kunden bestimmte Sicherheits- und Complianceanforderungen der DSGVO erfüllen können, und sie dient Kunden als Grundlage für das Erstellen und Konfigurieren eigener IaaS-Webanwendungslösungen in Azure. Kunden können diese Referenzarchitektur nutzen und für die Einhaltung der DSGVO den aus [vier Schritten bestehenden Prozess](https://aka.ms/gdprebook) von Microsoft befolgen:
1. Ermitteln: Identifizieren Sie, welche personenbezogenen Daten vorhanden sind und wo sie sich befinden.
2. Verwalten: Legen Sie fest, wie personenbezogene Daten genutzt werden und wie darauf zugegriffen wird.
3. Schützen: Richten Sie Sicherheitsmaßnahmen ein, um Sicherheitsrisiken und Sicherheitsverletzungen bei Daten zu verhindern, zu erkennen und darauf zu reagieren.
4. Berichten: Bewahren Sie benötigte Unterlagen auf, und verwalten Sie Datenanfragen und Benachrichtigungen zu Datenschutzverletzungen.

Diese Referenzarchitektur, die zugehörige Implementierungsanleitung und das Bedrohungsmodell sollen Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollten nicht unverändert in einer Produktionsumgebung verwendet werden. Beachten Sie Folgendes:
- Diese Architektur dient als Basis, um Kunden bei der Bereitstellung von Workloads in Azure in DSGVO-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können.

## <a name="architecture-diagram-and-components"></a>Architekturdiagramm und Komponenten
Diese Lösung stellt eine Referenzarchitektur für eine IaaS-Webanwendung mit einem SQL Server-Back-End bereit. Die Architektur umfasst eine Webschicht, eine Datenschicht, eine Active Directory-Infrastruktur, ein Application Gateway und ein Lastenausgleichsmodul. Virtuelle Computer, die in den Web- und Datenschichten bereitgestellt werden, werden in einer Verfügbarkeitsgruppe konfiguriert, und SQL Server-Instanzen werden in einer AlwaysOn-Verfügbarkeitsgruppe für Hochverfügbarkeit konfiguriert. Virtuelle Computer sind in Domänen eingebunden, und Active Directory-Gruppenrichtlinien werden verwendet, um Sicherheits- und Konformitätskonfigurationen auf Betriebssystemebene durchzusetzen. Ein Bastionhost für die Verwaltung bietet Administratoren eine sichere Verbindung für den Zugriff auf bereitgestellte Ressourcen. **In Azure werden das Konfigurieren einer VPN- oder ExpressRoute-Verbindung für die Verwaltung und der Datenimport in das Subnetz der Referenzarchitektur empfohlen.**

![Abbildung der IaaS-Webanwendung für die DSGVO-Referenzarchitektur](images/gdpr-iaaswa-architecture.png?raw=true "Abbildung der IaaS-Webanwendung für die DSGVO-Referenzarchitektur")

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

- Azure Virtual Machines
    - (1) Management/Bastion (Windows Server 2016 Datacenter)
    - (2) Active Directory-Domänencontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-Clusterknoten (SQL Server 2017 unter Windows Server 2016)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Verfügbarkeitsgruppen
    - (1) Active Directory-Domänencontroller
    - (1) SQL-Clusterknoten
    - (1) Web/IIS
- Virtuelles Azure-Netzwerk
    - (1) /16 virtuelle Netzwerke
    - (5) /24 Subnetze
    - DNS-Einstellungen werden auf beiden Domänencontrollern festgelegt.
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF Application Gateway (aktiviert)
        - Firewallmodus: Prävention
        - Regelsatz: OWASP 3.0
        - Listener: Port 443
- Azure Storage
    - (7) Georedundante Speicherkonten
- Cloudzeuge
- Recovery Services-Tresor
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Log Analytics
- Azure Security Center

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur
Im folgenden Abschnitt werden die Elemente für Entwicklung und Implementierung beschrieben.

**Bastionhost**: Der Bastionhost ist der einzelne Zugangspunkt, über den Benutzer Zugriff auf die bereitgestellten Ressourcen in dieser Umgebung erhalten. Der Bastionhost bietet eine sichere Verbindung mit den bereitgestellten Ressourcen, indem nur Remotedatenverkehr von öffentlichen IP-Adressen auf einer Sicherheitsliste zugelassen wird. Damit RDP-Datenverkehr (Remotedesktopprotokoll) zugelassen wird, muss die Quelle des Datenverkehrs in der NSG (Netzwerksicherheitsgruppe) definiert sein.

Diese Lösung erstellt einen virtuellen Computer als Bastionhost in der Domäne mit den folgenden Konfigurationen:
-   [Antimalware-Erweiterung](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Log Analytics-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure-Diagnoseerweiterung](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) mit Azure Key Vault
-   Eine [Richtlinie zum automatischen Herunterfahren](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/), um die Nutzung der Ressourcen virtueller Computer zu verringern, wenn sie nicht benötigt werden
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ist aktiviert, damit Anmeldeinformationen und andere Geheimnisse in einer geschützten Umgebung, die vom ausgeführten Betriebssystem isoliert ist, ausgeführt werden.

### <a name="virtual-network"></a>Virtuelles Netzwerk
Die Architektur definiert ein privates virtuelles Netzwerk mit dem Adressraum 10.200.0.0/16.

**Netzwerksicherheitsgruppen**: Diese Lösung stellt Ressourcen in einer Architektur mit einem separaten Websubnetz, Datenbanksubnetz, Active Directory-Subnetz und Managementsubnetz in einem virtuellen Netzwerk bereit. Subnetze werden logisch durch Regeln für Netzwerksicherheitsgruppen getrennt, die auf die einzelnen Subnetze angewendet werden, um den Datenverkehr zwischen den Subnetzen auf das für die System- und Verwaltungsfunktionen erforderliche Maß einzuschränken.

Details finden Sie in der Konfiguration für [Netzwerksicherheitsgruppen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json), die mit dieser Lösung bereitgestellt wird. Organisationen können Netzwerksicherheitsgruppen konfigurieren, indem sie die Datei oben bearbeiten und dabei [diese Dokumentation](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als Leitfaden verwenden.

Jedes der Subnetze verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG):
- 1 NSG für Application Gateway (LBNSG)
- 1 NSG für Bastionhost (MGTNSG)
- 1 NSG für primären und Sicherungsdomänencontroller (ADNSG)
- 1 NSG für Computer mit SQL Server und Cloudzeuge (SQLNSG)
- 1 NSG für Webschicht (WEBNSG)

### <a name="data-in-transit"></a>Daten während der Übertragung
Azure verschlüsselt standardmäßig die gesamte Kommunikation zu und von Azure-Rechenzentren. Darüber hinaus werden alle Transaktionen mit Azure Storage über HTTPS durchgeführt.

### <a name="data-at-rest"></a>Ruhende Daten
Die Architektur schützt ruhende Daten durch verschiedene Maßnahmen wie Verschlüsselung und Datenbanküberwachung.

**Azure Storage**: Um die Anforderungen für verschlüsselte ruhende Daten zu erfüllen, wird überall in [Azure Storage](https://azure.microsoft.com/services/storage/) die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/storage-service-encryption) verwendet. Dadurch werden personenbezogene Daten geschützt, um die Zusagen und Anforderungen der Organisation im Hinblick auf Sicherheit und Konformität gemäß der DSGVO einzuhalten.

**Azure Disk Encryption**: Azure Disk Encryption wird zur Verschlüsselung der Datenträger von virtuellen Windows-IaaS-Computern verwendet. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) nutzt die BitLocker-Funktion von Windows, um Volumeverschlüsselung für Betriebssysteme und Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern.

**SQL Server:** In der SQL Server-Instanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:
-   Die [AD-Authentifizierung und -Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten.
-   Die [SQL-Datenbank-Überprüfung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in einem Azure Storage-Konto.
-   Die SQL-Datenbank ist so konfiguriert, dass sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) für die Echtzeitverschlüsselung und -entschlüsselung der Datenbank, der zugehörigen Sicherungen und der Transaktionsprotokolldateien verwendet, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte personenbezogenen Daten keinen unbefugten Zugriffen ausgesetzt sind.
-   [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) verhindern sämtlichen Zugriff auf Datenbankserver, bis die richtigen Berechtigungen erteilt werden. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
-   Die [SQL-Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) ermöglicht die Erkennung und Reaktion auf potenzielle Bedrohungen, sobald sie auftreten. Dazu werden Sicherheitswarnungen bei verdächtigen Datenbankaktivitäten, potenziellen Sicherheitslücken, Angriffen durch Einschleusung von SQL-Befehlen und anormalen Mustern beim Datenbankzugriff ausgegeben.
-   [Always Encrypted-Spalten](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) stellen sicher, dass vertrauliche personenbezogene Daten im Datenbanksystem niemals im Klartextformat angezeigt werden. Nach dem Aktivieren der Datenverschlüsselung können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel haben, auf Klartextdaten zugreifen.
- Das Feature [Erweiterte Eigenschaften](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kann zum Beenden der Verarbeitung von Datenbesitzern verwendet werden, da es Benutzern ermöglicht, Datenbankobjekten benutzerdefinierte Eigenschaften hinzufügen und Daten als „Eingestellt“ zu markieren und damit Anwendungslogik zu unterstützen, die das Verarbeiten der zugehörigen personenbezogenen Daten verhindert.
- Die [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ermöglicht Benutzern das Festlegen von Richtlinien zum Einschränken des Zugriffs auf Daten, um die Verarbeitung zu beenden.
- Die [dynamische Datenmaskierung in SQL-Datenbank (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung vertraulicher personenbezogener Daten ein, indem diese Daten für nicht berechtigte Benutzer maskiert werden. DDM kann automatisch potenziell vertrauliche Daten ermitteln und eine geeignete Maskierung vorschlagen. Dies hilft bei der Identifikation der personenbezogenen Daten, für die der Schutz durch die DSGVO gilt, und bei der Reduzierung des Zugriffs, damit sie die Datenbank nicht per nicht autorisiertem Zugriff verlassen. **Hinweis: Kunden müssen die DDM-Einstellungen an ihr Datenbankschema anpassen.**

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf personenbezogene Daten in der Azure-Umgebung:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in AAD erstellt. Dies gilt auch für Benutzer, die auf SQL Server zugreifen.
-   Die Authentifizierung für die Anwendung erfolgt mithilfe von AAD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Darüber hinaus verwendet die Verschlüsselung der Datenbankspalten AAD auch, um die Anwendung bei SQL Server zu authentifizieren. Weitere Informationen finden Sie unter [Schützen von vertraulichen Daten in SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Die [rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer zum Erledigen ihrer Arbeit benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu gewähren, können Administratoren auch nur bestimmte Aktionen für den Zugriff auf personenbezogene Daten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) können Kunden die Anzahl der Benutzer mit Zugriff auf bestimmte Ressourcen einschränken.  Administratoren können mit AAD Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.
- Eine bereitgestellte IaaS-Active-Directory-Instanz bietet Identitätsverwaltung auf Betriebssystemebene für bereitgestellte virtuelle IaaS-Computer.

### <a name="security"></a>Sicherheit
**Geheimnisverwaltung**: Die Lösung verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen personenbezogener Daten und des Zugriffs auf diese Daten:
- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule (HSMs) geschützt. Der Schlüsseltyp ist ein HSM-geschützter 2.048-Bit-RSA-Schlüssel.
- Allen Benutzern und Identitäten werden mit RBAC die mindestens erforderlichen Berechtigungen erteilt.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.
- Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern.

**Patchverwaltung**: Virtuelle Windows-Computer, die mit dieser Referenzarchitektur bereitgestellt werden, sind standardmäßig so konfiguriert, dass sie automatische Updates vom Windows Update-Dienst erhalten. Diese Lösung umfasst auch den Dienst [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), über den aktualisierte Bereitstellungen erstellt werden können, um virtuelle Computer bei Bedarf zu patchen.

**Schutz vor Schadsoftware**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) für Virtual Machines bietet Echtzeitschutzfunktionen, mit denen die Identifizierung und Entferung von Viren, Spyware und anderer bösartiger Software ermöglicht wird, mit konfigurierbaren Warnmeldungen, wenn bekannte bösartige oder unerwünschte Software versucht, sich auf geschützten virtuellen Computern zu installieren oder die Ausführung zu starten.

**Sicherheitswarnungen**: [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) ermöglicht Kunden die Überwachung von Datenverkehr, das Erfassen von Protokollen und das Analysieren von Datenquellen auf Bedrohungen. Darüber hinaus greift Azure Security Center auf die vorhandene Konfiguration der Azure-Dienste zu, um Empfehlungen für die Konfiguration und Verwaltung zu geben und zur Verbesserung der Sicherheit und zum Schutz personenbezogener Daten beizutragen. Azure Security Center umfasst einen [Bericht über Bedrohungen](https://docs.microsoft.com/azure/security-center/security-center-threat-report) zu jeder erkannten Bedrohung, der den Teams, die für die Bearbeitung von Incidents zuständig sind, beim Untersuchen und Beheben von Bedrohungen als Hilfe dient.

**Application Gateway**: Die Architektur verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Funktionen:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Aktivierte [SSL-Auslagerung](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Deaktivieren von [TLS v1.0 und v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-Modus)
- [Präventionsmodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) mit OWASP 3.0-Regelsatz
- Aktivieren der [Diagnoseprotokollierung](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Benutzerdefinierte Integritätstests](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) und [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieten zusätzlichen Schutz und zusätzliche Benachrichtigungen. Azure Security Center stellt außerdem ein Reputationssystem bereit.

### <a name="business-continuity"></a>Geschäftskontinuität

**Hochverfügbarkeit**: Die Lösung stellt alle virtuellen Computer in einer [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bereit. Verfügbarkeitsgruppen stellen sicher, dass die virtuellen Computer über mehrere isolierte Hardwarecluster verteilt sind, um die Verfügbarkeit zu verbessern. Während einer geplanten oder ungeplanten Wartung ist mindestens ein virtueller Computer verfügbar, um die Azure-SLA von 99,95 % zu erfüllen.

**Recovery Services-Tresor**: Der [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) speichert Sicherungsdaten und schützt alle Konfigurationen von Azure Virtual Machines in dieser Architektur. Durch einen Recovery Services-Tresor können Kunden Dateien und Ordner von einer IaaS-VM wiederherstellen, ohne die gesamte VM wiederherstellen zu müssen. So werden die Wiederherstellungszeiten verkürzt.

**Cloudzeuge**: Ein [Cloudzeuge](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) ist ein Failovercluster-Quorumzeuge in Windows Server 2016, der Azure als Vermittlungspunkt nutzt. Der Cloudzeuge erhält wie jeder andere Quorumzeuge eine Stimme und kann damit an Quorumberechnungen teilnehmen. Er verwendet aber den öffentlich verfügbaren Azure Blob Storage-Standard. Damit entfällt der zusätzliche Wartungsaufwand für virtuelle Computer, die in einer öffentlichen Cloud gehostet werden.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung

Log Analytics bietet eine umfassende Protokollierung von System- und Benutzeraktivitäten sowie der Systemintegrität. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist eine Lösung für die Erfassung und Analyse von Daten, die von Ressourcen in Azure-basierten und lokalen Umgebungen generiert werden.
- **Aktivitätsprotokolle**: [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in einem Abonnement ausgeführt wurden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
- **Diagnoseprotokolle**: [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) umfassen sämtliche Protokolle, die von jeder Ressource ausgegeben werden. Zu diesen Protokollen gehören Windows-Ereignissystemprotokolle, Azure Storage-Protokolle, Key Vault-Überwachungsprotokolle sowie Application Gateway-Zugriffs- und -Firewallprotokolle.
- **Protokollarchivierung**: Alle Diagnoseprotokolle werden für die Archivierung in ein zentrales und verschlüsseltes Azure Storage-Konto geschrieben. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden. Diese Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige mit Azure Log Analytics verbunden.

Darüber hinaus sind in dieser Architektur die folgenden Überwachungslösungen enthalten:
-   [Active Directory-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Die Lösung „Active Directory-Integritätsüberprüfung“ bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Antischadsoftwarebewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Die Antischadsoftwarelösung meldet Schadsoftware, Bedrohungen und den Schutzstatus.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Die Azure Automation-Lösung dient der Speicherung, Ausführung und Verwaltung von Runbooks.
-   [Sicherheit und Überwachung](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Das Sicherheits- und Überwachungsdashboard ermöglicht einen allgemeinen Einblick in den Sicherheitszustand von Ressourcen. Hierzu werden Metriken zu Sicherheitsdomänen, relevanten Problemen, Erkennungen, Bedrohungsdaten und allgemeinen Sicherheitsabfragen bereitgestellt.
-   [SQL-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): Die Lösung „SQL-Integritätsüberprüfung“ bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt für die Kunden eine priorisierte Liste mit speziellen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Updateverwaltung](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Die Updateverwaltungslösung ermöglicht die Verwaltung von Sicherheitsupdates des Betriebssystems durch die Kunden. Dies umfasst auch den Status der verfügbaren Updates und den Fortschritt beim Installieren der erforderlichen Updates.
-   [Agent-Integritätsdiagnose](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Die Lösung „Agent-Integritätsdiagnose“ meldet die Anzahl bereitgestellter Agents sowie deren geografische Verteilung. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
-   [Azure-Aktivitätsprotokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Die Lösung „Aktivitätsprotokollanalyse“ hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.
-   [Änderungsnachverfolgung](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Die Lösung „Änderungsnachverfolgung“ ermöglicht es Kunden, problemlos Änderungen in der Umgebung zu erkennen.

## <a name="threat-model"></a>Bedrohungsmodell

Das Datenflussdiagramm (DFD) für diese Referenzarchitektur steht zum [Download](https://aka.ms/gdprIaaSdfd) bereit und ist unten angegeben. Dieses Modell kann Kunden dabei helfen, bei Änderungen die potenziellen Risikopunkte in der Systeminfrastruktur zu verstehen.

![IaaS-Webanwendung für das DSGVO-Bedrohungsmodell](images/gdpr-iaaswa-threat-model.png?raw=true "IaaS-Webanwendung für das DSGVO-Bedrohungsmodell")

## <a name="compliance-documentation"></a>Konformitätsdokumentation

Im [Azure Security and Compliance Blueprint – DSGVO-Zuständigkeitsmatrix für Kunden](https://aka.ms/gdprCRM) werden die Aufgaben von Controllern und Verarbeitern für alle DSGVO-Artikel aufgeführt. Beachten Sie, dass der Kunde bei Azure-Diensten in der Regel der Controller und Microsoft der Verarbeiter ist.

Die [Azure Security and Compliance Blueprint – Implementierungsmatrix für DSGVO-konforme IaaS-Webanwendungen](https://aka.ms/gdprIaaSweb) enthält Informationen darüber, auf welche DSGVO-Artikel durch die Architektur der IaaS-Webanwendung verwiesen wird, sowie eine detaillierte Beschreibung, wie die Implementierung die Anforderungen der einzelnen Artikel erfüllt.

## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen
### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
Für sichere Verbindungen mit Ressourcen, die als Teil dieser Referenzarchitektur einer IaaS-Webanwendung bereitgestellt werden, muss ein sicherer VPN-Tunnel oder eine [ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) konfiguriert werden. Durch die ordnungsgemäße Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen in einem „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Der [IPsec-Tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wird bei dieser Option als Verschlüsselungsmechanismus verwendet.

Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Haftungsausschluss

- Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.  
- Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.  
- Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.  
- Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.  
- Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
- Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.

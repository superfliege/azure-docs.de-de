---
title: Azure Security and Compliance Blueprint – PaaS-Webanwendungen gemäß DSGVO
description: Azure Security and Compliance Blueprint – PaaS-Webanwendungen gemäß DSGVO
services: security
author: jomolesk
ms.assetid: 229759a1-f984-4985-a3fd-3719a7d1c8ff
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c338fed118e330280824754277a2fc31a1eaa7ba
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161873"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-gdpr"></a>Azure Security and Compliance Blueprint – PaaS-Webanwendungen gemäß DSGVO

## <a name="overview"></a>Übersicht
Die Datenschutz-Grundverordnung (DSGVO) enthält viele Anforderungen für das Sammeln, Speichern und Verwenden personenbezogener Daten. Diese umfassen auch das Identifizieren und Schützen dieser Daten durch Organisationen, die Einhaltung der Transparenzanforderungen, das Erkennen und Melden von Sicherheitsverletzungen im Zusammenhang mit personenbezogenen Daten sowie das Schulen von Datenschutzbeauftragten und anderen Mitarbeitern. Die DSGVO bietet Einzelpersonen mehr Kontrolle über ihre persönlichen Daten und ist mit vielen neuen Pflichten für Organisationen verbunden, die personenbezogene Daten sammeln, verarbeiten oder analysieren. Die DSGVO enthält neue Vorschriften für Organisationen, die Menschen in der Europäischen Union (EU) Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. Die DSGVO gilt unabhängig vom Standort der Organisation.

Microsoft hat Azure basierend auf branchenweit führenden Sicherheitsmaßnahmen und Datenschutzrichtlinien zum Schutz von Daten in der Cloud entwickelt, zu denen auch personenbezogene Daten gehören, die der DSGVO unterliegen. Gemäß den eigenen [Vertragsbedingungen](http://aka.ms/Online-Services-Terms) muss Microsoft die Anforderungen von datenverarbeitenden Organisationen einhalten.

Dieser Azure Security and Compliance Blueprint enthält Anleitungen für die Bereitstellung einer PaaS-Umgebung (Platform-as-a-Service), die für eine einfache, internetfähige Webanwendung geeignet ist. Diese Lösung zeigt Möglichkeiten auf, mit denen Kunden die spezifischen Sicherheits- und Complianceanforderungen der DSGVO erfüllen können. Und sie dient Kunden als Grundlage für das Erstellen und Konfigurieren eigener PaaS-Webanwendungslösungen in Azure. Kunden können diese Referenzarchitektur nutzen und für die Einhaltung der DSGVO den aus [vier Schritten bestehenden Prozess](https://aka.ms/gdprebook) von Microsoft befolgen:
1. Ermitteln: Identifizieren Sie, welche personenbezogenen Daten vorhanden sind und wo sie sich befinden.
2. Verwalten: Legen Sie fest, wie personenbezogene Daten verwendet werden und wie darauf zugegriffen wird.
3. Schützen: Richten Sie Sicherheitsmaßnahmen ein, um Sicherheitsrisiken und Sicherheitsverletzungen bei Daten zu verhindern, zu erkennen und darauf zu reagieren.
4. Melden: Führen Sie die erforderliche Dokumentation, und erfassen Sie Datenanforderungen und Benachrichtigungen über Sicherheitsverletzungen.

Diese Referenzarchitektur, die zugehörige Implementierungsanleitung und das Bedrohungsmodell sollen Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollten nicht unverändert in einer Produktionsumgebung verwendet werden. Beachten Sie Folgendes:
- Diese Architektur dient als Basis, um Kunden bei der Bereitstellung von Workloads in Azure in DSGVO-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können.

## <a name="architecture-diagram-and-components"></a>Architekturdiagramm und Komponenten
Diese Lösung stellt eine Referenzarchitektur für eine PaaS-Webanwendung mit einem Azure SQL-Datenbank-Back-End bereit. Die Webanwendung wird in einer isolierten Azure App Service-Umgebung gehostet. Hierbei handelt es sich um eine private, dedizierte Umgebung in einem Azure-Datencenter. In der Umgebung wird ein Lastenausgleich für den Datenverkehr der Webanwendung für die VMs durchgeführt, die von Azure verwaltet werden. Diese Architektur umfasst auch Netzwerksicherheitsgruppen, ein Application Gateway, Azure DNS und einen Load Balancer. Darüber hinaus ermöglicht Application Insights über die Operations Management Suite (OMS) die Echtzeit-Anwendungsleistungsverwaltung und entsprechende Analysen. **Azure empfiehlt das Konfigurieren einer VPN- oder ExpressRoute-Verbindung für die Verwaltung und den Datenimport in das Subnetz der Referenzarchitektur.**

![Abbildung der PaaS-Webanwendung für die DSGVO-Referenzarchitektur](images/gdpr-paaswa-architecture.png?raw=true "Abbildung der PaaS-Webanwendung für die DSGVO-Referenzarchitektur")

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

- Azure Active Directory (AAD)
- Azure Key Vault
- Azure SQL-Datenbank
- Application Gateway
    - (1) WAF Application Gateway (aktiviert)
        - Firewallmodus: Prävention
        - Regelsatz: OWASP 3.0
        - Listener: Port 443
- Azure Virtual Network
- Netzwerksicherheitsgruppen
- Azure DNS
- Azure Storage
- Operations Management Suite (OMS)
- Azure Monitor
- Application Insights
- Azure Security Center
- App Service-Umgebung v2
- Azure Load Balancer
- Azure-Web-App
- Azure Resource Manager

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur
Im folgenden Abschnitt werden die Elemente der Bereitstellung und Implementierung beschrieben.

**Azure Resource Manager**: Mit [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) können Kunden mit den Ressourcen der Lösung als Gruppe arbeiten. Kunden können alle Ressourcen für die Lösung in einem einzigen koordinierten Vorgang bereitstellen, aktualisieren oder löschen. Kunden verwenden eine Vorlage für die Bereitstellung, die für unterschiedliche Umgebungen geeignet sein kann, z.B. Testing, Staging und Produktion. Resource Manager umfasst Sicherheits-, Überwachungs- und Kennzeichnungsfunktionen, mit denen Kunden ihre Ressourcen nach der Bereitstellung verwalten können.

**App Service-Umgebung v2**: Die [Azure App Service-Umgebung](https://docs.microsoft.com/en-us/azure/app-service/environment/intro) ist ein App Service-Feature, das eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von App Service-Anwendungen in größerem Umfang bereitstellt.

Aufgrund der Isolierung werden in ASUs nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Netzwerk bereitgestellt. Kunden haben eine genauere Kontrolle über den eingehenden und ausgehenden Anwendungs-Netzwerkdatenverkehr, und Anwendungen können über virtuelle Netzwerke sichere Hochgeschwindigkeitsverbindungen mit lokalen Unternehmensressourcen herstellen.

Die Verwendung von ASUs für diese Architektur ist für die folgenden Steuerelemente/Konfigurationen zulässig:

- Hosten in einem geschützten virtuellen Azure-Netzwerk und in Netzwerksicherheitsregeln
- ASE, konfiguriert mit einem selbstsignierten ILB-Zertifikat für die HTTPS-Kommunikation
- [Interner Lastenausgleichsmodus](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-environment-with-internal-load-balancer) (Modus 3)
- Deaktivieren von [TLS 1.0](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Ändern von [TLS-Verschlüsselung](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Steuern der [N/W-Port für eingehenden Datenverkehr](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [WAF – Daten einschränken](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Zulassen von [Azure SQL-Datenbank-Datenverkehr](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure-Web-App**: [Azure-Web-Apps](https://docs.microsoft.com/en-us/azure/app-service/) ermöglichen Kunden das Erstellen und Hosten von Webanwendungen in der Programmiersprache ihrer Wahl, ohne dass eine Infrastruktur verwaltet werden muss. Der Dienst bietet automatische Skalierung und Hochverfügbarkeit, unterstützt Windows und Linux und ermöglicht automatisierte Bereitstellungen über GitHub, Visual Studio Team Services oder ein anderes beliebiges Git-Repository.

### <a name="virtual-network"></a>Virtual Network
Die Architektur definiert ein privates VNet mit dem Adressraum 10.200.0.0/16.

**Netzwerksicherheitsgruppen**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) enthalten Zugriffssteuerungslisten (ACLs), die den Datenverkehr in einem VNet zulassen oder verweigern. NSGs können dafür verwendet werden, den Datenverkehr auf der Ebene eines Subnetzes oder einzelner VMs zu schützen. Die folgenden NSGs sind vorhanden:
- 1 NSG für Application Gateway
- 1 NSG für die App Service-Umgebung
- 1 NSG für Azure SQL-Datenbank

Jede NSG verfügt über bestimmte offene Ports und Protokolle, damit die Lösung sicher und richtig ausgeführt werden kann. Darüber hinaus werden die folgenden Konfigurationen für jede NSG aktiviert:
  - [Diagnoseprotokolle und -ereignisse](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log) sind aktiviert und werden in einem Speicherkonto gespeichert.
  - OMS Log Analytics ist mit der [Diagnose der NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) verbunden.

**Subnetze**: Stellen Sie sicher, dass jedes Subnetz seiner entsprechenden NSG zugeordnet ist.

**Azure DNS**: Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. [Azure DNS](https://docs.microsoft.com/en-us/azure/dns/dns-overview) ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Azure-Infrastruktur durchführt. Durch das Hosten von Domänen in Azure können Benutzer DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsschritte wie für die anderen Azure-Dienste verwalten. Azure DNS unterstützt auch private DNS-Domänen.

**Azure Load Balancer**: Mit [Azure Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) können Kunden ihre Anwendungen skalieren und Hochverfügbarkeit für Dienste erzielen. Load Balancer unterstützt sowohl Szenarien mit eingehenden als auch ausgehenden Verbindungen, bietet niedrige Latenzen und einen hohen Durchsatz und unterstützt Millionen Datenflüsse für alle TCP- und UDP-Anwendungen.

### <a name="data-in-transit"></a>Daten während der Übertragung
Azure verschlüsselt standardmäßig die gesamte Kommunikation zu und von Azure-Rechenzentren. Alle Transaktionen für Azure Storage über das Azure-Portal werden per HTTPS durchgeführt.

### <a name="data-at-rest"></a>Ruhende Daten

Die Architektur schützt ruhende Daten durch die Verwendung von Verschlüsselung, Datenbanküberwachung und anderen Maßnahmen.

**Azure Storage**: Um die Anforderungen für verschlüsselte ruhende Daten zu erfüllen, wird für den gesamten [Azure Storage](https://azure.microsoft.com/services/storage/) die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/storage-service-encryption) verwendet. Dadurch werden personenbezogene Daten geschützt, um die Zusagen und Anforderungen der Organisation im Hinblick auf Sicherheit und Konformität gemäß der DSGVO einzuhalten.

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) nutzt die BitLocker-Funktion von Windows, um Volumeverschlüsselung für Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträgerverschlüsselungsschlüssel zu erleichtern.

**Azure SQL-Datenbank**: In der Azure SQL-Datenbank-Instanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:
-   Die [AD-Authentifizierung und -Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten.
-   Die [SQL-Datenbank-Überprüfung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verfolgt Datenbankereignisse nach und schreibt diese in ein Überwachungsprotokoll in einem Azure Storage-Konto.
-   Die SQL-Datenbank ist so konfiguriert, dass sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) für die Echtzeitverschlüsselung und -entschlüsselung der Datenbank, der zugehörigen Sicherungen und der Transaktionsprotokolldateien verwendet, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte personenbezogene Daten keinen unbefugten Zugriffen ausgesetzt sind.
-   [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) verhindern sämtlichen Zugriff auf Datenbankserver, bis die richtigen Berechtigungen erteilt werden. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
-   Die [SQL-Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) ermöglicht die Erkennung und Reaktion auf potenzielle Bedrohungen, sobald sie auftreten. Dazu werden Sicherheitswarnungen bei verdächtigen Datenbankaktivitäten, potenziellen Sicherheitslücken, Angriffen durch Einschleusung von SQL-Befehlen und anormalen Mustern beim Datenbankzugriff ausgegeben.
-   [Always Encrypted-Spalten](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) stellen sicher, dass vertrauliche personenbezogene Daten im Datenbanksystem niemals im Klartextformat angezeigt werden. Nach dem Aktivieren der Datenverschlüsselung können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel haben, auf Klartextdaten zugreifen.
- [Erweiterte Eigenschaften](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) können zum Beenden der Verarbeitung von Datenbesitzern verwendet werden. Benutzern wird es ermöglicht, Datenbankobjekten benutzerdefinierte Eigenschaften hinzuzufügen und Daten als „Eingestellt“ zu markieren und damit Anwendungslogik zu unterstützen, die das Verarbeiten der zugehörigen personenbezogenen Daten verhindert.
- Die [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ermöglicht Benutzern das Festlegen von Richtlinien zum Einschränken des Zugriffs auf Daten, um die Verarbeitung zu beenden.
- Die [dynamische Datenmaskierung in SQL-Datenbank (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung vertraulicher personenbezogener Daten ein, indem diese Daten für nicht berechtigte Benutzer maskiert werden. DDM kann automatisch potenziell vertrauliche Daten ermitteln und eine geeignete Maskierung vorschlagen. Dies hilft bei der Identifikation der personenbezogenen Daten, für die der Schutz durch die DSGVO gilt, und bei der Reduzierung des Zugriffs, sodass sie die Datenbank nicht per nicht autorisiertem Zugriff verlassen. **Hinweis: Kunden müssen die DDM-Einstellungen an ihr Datenbankschema anpassen.**

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf personenbezogene Daten in der Azure-Umgebung:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in AAD erstellt. Dies gilt auch für Benutzer, die auf Azure SQL-Datenbank zugreifen.
-   Die Authentifizierung für die Anwendung erfolgt mithilfe von AAD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Darüber hinaus wird für Verschlüsselung der Datenbankspalten ebenfalls AAD verwendet, um die Anwendung in Azure SQL-Datenbank zu authentifizieren. Weitere Informationen finden Sie unter [Schützen von vertraulichen Daten in SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Die [rollenbasierte Zugriffssteuerung in Azure (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer zum Erledigen ihrer Arbeit benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu gewähren, können Administratoren auch nur bestimmte Aktionen für den Zugriff auf personenbezogene Daten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) können Kunden die Anzahl von Benutzern mit Zugriff auf bestimmte Informationen, z.B. personenbezogene Daten, einschränken.  Administratoren können mit AAD Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.

### <a name="security"></a>Sicherheit
**Geheimnisverwaltung**: Die Lösung verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen personenbezogener Daten und des Zugriffs auf diese Daten:
- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule (HSMs) geschützt. Der Schlüsseltyp ist ein HSM-geschützter 2.048-Bit-RSA-Schlüssel.
- Allen Benutzern und Identitäten werden mit RBAC die mindestens erforderlichen Berechtigungen erteilt.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

**Sicherheitswarnungen**: [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) ermöglicht Kunden die Überwachung von Datenverkehr, das Erfassen von Protokollen und das Analysieren von Datenquellen auf Bedrohungen. Darüber hinaus greift Azure Security Center auf die vorhandene Konfiguration der Azure-Dienste zu, um Empfehlungen für die Konfiguration und Verwaltung zu geben und zur Verbesserung der Sicherheit und zum Schutz personenbezogener Daten beizutragen. Azure Security Center umfasst einen [Bericht über Bedrohungen](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) zu jeder erkannten Bedrohung, der den Teams, die für die Bearbeitung von Incidents zuständig sind, beim Untersuchen und Beheben von Bedrohungen als Hilfe dient.

**Application Gateway**: Die Architektur verringert das Risiko von Sicherheitsrisiken durch ein Application Gateway mit Web Application Firewall (WAF) und aktiviertem OWASP-Regelsatz. Weitere Funktionen:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Aktivierte [SSL-Auslagerung](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Deaktivieren von [TLS v1.0 und v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-Modus)
- [Präventionsmodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) mit OWASP 3.0-Regelsatz
- Aktivieren der [Diagnoseprotokollierung](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)
- [Benutzerdefinierte Integritätstests](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) und [Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-security-recommendations) bieten zusätzlichen Schutz und zusätzliche Benachrichtigungen. Azure Security Center stellt außerdem ein Reputationssystem bereit.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung

OMS ermöglicht eine umfassende Protokollierung von System- und Benutzeraktivitäten sowie der Systemintegrität. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist eine OMS-Lösung für die Erfassung und Analyse von Daten, die von Ressourcen in Azure- und lokalen Umgebungen generiert werden.
- **Aktivitätsprotokolle**: [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) ermöglichen Einblicke in Vorgänge, die für Ressourcen in einem Abonnement durchgeführt werden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
- **Diagnoseprotokolle**: [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) sind alle Protokolle, die von sämtlichen Ressourcen ausgegeben werden. Zu diesen Protokollen gehören Windows-Ereignissystemprotokolle, Azure Storage-Protokolle, Key Vault-Überwachungsprotokolle sowie Application Gateway-Zugriffs- und -Firewallprotokolle.
- **Protokollarchivierung**: Alle Diagnoseprotokolle werden für die Archivierung in ein zentrales und verschlüsseltes Azure Storage-Konto geschrieben. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden. Für diese Protokolle wird dann eine Verbindung mit Azure Log Analytics hergestellt, um die Verarbeitung, Speicherung und Dashboardanzeige zu ermöglichen.

Darüber hinaus sind in dieser Architektur die folgenden OMS-Lösungen enthalten:
-   [Active Directory-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Die Active Directory-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Antischadsoftwarebewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Der Antischadsoftwarelösung meldet Schadsoftware, Bedrohungen und den Schutzstatus.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Die Azure Automation-Lösung dient der Speicherung, Ausführung und Verwaltung von Runbooks. Bei dieser Lösung werden Protokolle über Runbooks für Application Insights und Azure SQL-Datenbank erfasst.
-   [Sicherheit und Überwachung](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Das Sicherheits- und Überwachungsdashboard bietet einen allgemeinen Einblick in den Sicherheitszustand von Ressourcen. Hierzu werden Metriken zu Sicherheitsdomänen, relevanten Problemen, Erkennungen, Bedrohungsdaten und allgemeinen Sicherheitsabfragen bereitgestellt.
-   [SQL-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): Die SQL-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt für die Kunden eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Updateverwaltung](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Der Updateverwaltungslösung ermöglicht die Verwaltung von Betriebssystem-Sicherheitsupdates durch die Kunden. Dies umfasst auch den Status der verfügbaren Updates und den Fortschritt beim Installieren der erforderlichen Updates.
-   [Agent-Integritätsdiagnose](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Die Agent-Integritätsdiagnoselösung meldet, wie viele Agents bereitgestellt werden und wie ihre geografische Verteilung ist. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
-   [Azure-Aktivitätsprotokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Die Aktivitätsprotokollanalyse-Lösung hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.
-   [Änderungsnachverfolgung](https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking): Die Änderungsnachverfolgungslösung ermöglicht Kunden das Identifizieren von Änderungen in der Umgebung auf einfache Weise.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) unterstützt Benutzer beim Nachverfolgen der Leistung, Aufrechterhalten der Sicherheit und Identifizieren von Trends, indem Organisationen das Überwachen, Erstellen von Warnungen und Archivieren von Daten ermöglicht wird, z.B. die Nachverfolgung von API-Aufrufen in den Azure-Ressourcen von Kunden.

**Application Insights**
[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Mit Application Insights werden Leistungsanomalien erkannt, und Kunden können die Lösung für die Liveüberwachung der Webanwendung nutzen. Sie verfügt über leistungsstarke Analysetools, mit denen Kunden Probleme diagnostizieren und nachvollziehen können, wie ihre App von den Benutzern verwendet wird. Der Dienst unterstützt Kunden bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit.

## <a name="threat-model"></a>Bedrohungsmodell

Das Datenflussdiagramm für diese Referenzarchitektur steht zum [Download](https://aka.ms/gdprPaaSdfd) bereit und ist unten angegeben. Dieses Modell kann Kunden dabei helfen, bei Änderungen die potenziellen Risikopunkte in der Systeminfrastruktur zu verstehen.

![PaaS-Webanwendung für das DSGVO-Bedrohungsmodell](images/gdpr-paaswa-threat-model.png?raw=true "PaaS-Webanwendung für das DSGVO-Bedrohungsmodell")

## <a name="compliance-documentation"></a>Konformitätsdokumentation
Unter [Azure Security and Compliance Blueprint – DSGVO-Zuständigkeitsmatrix für Kunden](https://aka.ms/gdprCRM) sind die Aufgaben von Controllern und Verarbeitern für alle DSGVO-Artikel aufgeführt. Beachten Sie, dass der Kunde bei Azure-Diensten in der Regel der Controller und Microsoft der Verarbeiter ist.

[Azure Security and Compliance Blueprint – Implementierungsmatrix für DSGVO-konforme PaaS-Webanwendungen](https://aka.ms/gdprPaaSWeb) enthält Informationen darüber, auf welche DSGVO-Artikel durch die Architektur der PaaS-Webanwendung verwiesen wird, sowie eine detaillierte Beschreibung, wie die Implementierung die Anforderungen der einzelnen Artikel erfüllt.


## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen
### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
Zur Erzielung von sicheren Verbindungen mit Ressourcen, die als Teil dieser Referenzarchitektur einer PaaS-Webanwendung bereitgestellt werden, muss ein sicherer VPN-Tunnel oder eine [ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) konfiguriert werden. Durch die korrekte Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen in einem „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Der [IPsec-Tunnelmodus](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wird bei dieser Option als Verschlüsselungsmechanismus verwendet.

Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Haftungsausschluss

 - Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.
 - Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.
 - Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.
 - Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.
 - Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
 - Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.

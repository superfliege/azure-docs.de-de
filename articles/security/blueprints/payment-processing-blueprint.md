---
title: Azure Security and Compliance Blueprint – PCI-DSS-konforme Umgebungen für die Zahlungsverarbeitung
description: Azure Security and Compliance Blueprint – PCI-DSS-konforme Umgebungen für die Zahlungsverarbeitung
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 2f1e00a8-0dd6-477f-9453-75424d06a1df
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2018
ms.author: frasim
ms.openlocfilehash: 5851d5499c61cf99d7f85d07642a292f3b8c19d2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="azure-security-and-compliance-blueprint---pci-dss-compliant-payment-processing-environments"></a>Azure Security and Compliance Blueprint – PCI-DSS-konforme Umgebungen für die Zahlungsverarbeitung

## <a name="overview"></a>Übersicht

Die Zahlungsabwicklung für PCI-DSS-konforme Umgebungen bietet Anleitungen für die Bereitstellung einer PCI-DSS-konformen PaaS-Umgebung (Platform-as-a-Service), die zur Verarbeitung vertraulicher Kreditkartendaten geeignet ist. Der Plan präsentiert eine allgemeine Referenzarchitektur und soll dazu dienen, die Einführung von Microsoft Azure zu vereinfachen. Dieser Plan veranschaulicht eine End-to-End-Lösung, die die Anforderungen von Organisationen erfüllt, die nach einem cloudbasierten Ansatz suchen, um die Belastungen und Kosten einer Bereitstellung zu verringern.

Der Plan unterstützt Sie bei der Erfüllung der Anforderungen der strengen Payment Card Industry Data Security Standards (PCI-DSS 3.2) für das Erfassen, Speichern und Abrufen von Kartendaten für die Bezahlung. Er zeigt den ordnungsgemäßen Umgang mit Kreditkartendaten (einschließlich Kartennummer, Ablaufdatum und Überprüfungsdaten) in einer sicheren, konformen mehrstufigen Umgebung, die als End-to-End-PaaS-Lösung auf Basis von Azure bereitgestellt wird. Weitere Informationen zu den PCI DSS 3.2-Anforderungen und zu dieser Lösung finden Sie unter [Anforderungen des PCI-DSS: Allgemeine Übersicht](pci-dss-requirements-overview.md).

Dieser Plan soll Kunden ein besseres Verständnis für die spezifischen Anforderungen vermitteln und darf nicht unverändert in einer Produktionsumgebung verwendet werden. Sollen die Anforderungen einer PCI-DSS-konformen Lösung für eine benutzerdefinierte Lösung vollständig erfüllt werden, genügt es nicht, eine Anwendung ohne Änderungen in dieser Umgebung bereitzustellen. Beachten Sie Folgendes:
- Dieser Plan bietet eine Grundlage, um Kunden dabei zu unterstützen, Microsoft Azure in einer PCI-DSS-konformen Weise zu verwenden.
- Erreichen von PCI-DSS-Konformität erfordert, dass ein zugelassener Qualified Security Assessor (QSA) die Produktionslösung eines Kunden zertifiziert.
- Der Kunde ist für die Durchführung angemessener Sicherheits- und Konformitätsprüfungen für jede Lösung verantwortlich, die mit dieser grundlegenden Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden und der jeweiligen Region unterschiedlich sein können.  

Eine schnelle Übersicht über die Funktionsweise dieser Lösung finden Sie in diesem [Video](https://aka.ms/pciblueprintvideo), in dem die Bereitstellung erklärt und gezeigt wird.

## <a name="solution-components"></a>Lösungskomponenten

Die grundlegende Architektur besteht aus den folgenden Komponenten:

- **Architekturdiagramm**. Das Diagramm zeigt die Referenzarchitektur, die für die Contoso Webstore-Lösung verwendet wird.
- **Bereitstellungsvorlagen**. In dieser Bereitstellung werden [Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) verwendet, um die Komponenten der Architektur automatisch in Microsoft Azure bereitzustellen, indem während des Setups Konfigurationsparameter angegeben werden.
- **Automatisierte Bereitstellungsskripts**. Diese Skripts unterstützen beim Bereitstellen der End-to-End-Lösung. Zu den Skripts gehören:
    - Ein Skript zur Modulinstallation und zur Einrichtung für [globale Administratoren](/azure/active-directory/active-directory-assign-admin-roles-azure-portal). Mit diesem Skript werden die erforderlichen PowerShell-Module und globalen Administratorrollen installiert und wird geprüft, ob diese ordnungsgemäß konfiguriert sind.
    - Ein PowerShell-Installationsskript wird verwendet, um die End-to-End-Lösung bereitzustellen. Diese wird als ZIP-Datei und BACPAC-Datei bereitgestellt, die eine vorgefertigte Demowebanwendung mit [Beispielinhalten für eine SQL-Datenbank](https://github.com/Microsoft/azure-sql-security-sample) enthalten. Den Quellcode für diese Lösung finden Sie im [Blueprint-Coderepository][code-repo]. 

## <a name="architectural-diagram"></a>Architekturdiagramm

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Benutzerszenario

Der Plan behandelt den weiter unten angegebenen Anwendungsfall.

> Dieses Szenario veranschaulicht die Umstellung der Kreditkartenverarbeitung eines fiktiven Webstores auf eine Azure-basierte PaaS-Lösung. Die Lösung sammelt grundlegende Benutzerinformationen einschließlich Zahlungsdaten. Die Lösung verarbeitet keine Zahlungen mit diesen Karteninhaberdaten. Nachdem die Daten gesammelt wurden, sind Kunden dafür verantwortlich, Transaktionen über einen Zahlungsabwickler auszulösen und auszuführen. Weitere Informationen finden Sie [hier](https://aka.ms/pciblueprintprocessingoverview).

### <a name="use-case"></a>Anwendungsfall
Ein kleiner Webstore namens *Contoso Webstore* möchte sein Zahlungssystem in die Cloud verschieben. Der Webstore hat Microsoft Azure als Plattform ausgewählt, um Kaufvorgänge zu hosten und einer Sachbearbeiterin zu gestatten, Kreditkartenzahlungen der Kunden zu sammeln.

Der Administrator sucht nach einer Lösung, die schnell bereitgestellt werden kann, um die Ziele in einer cloudgestützten Lösung zu erreichen. Er verwendet dieses Proof of Concept (POC), um mit den Projektbeteiligten zu besprechen, wie Azure dazu verwendet werden kann, Kreditkartendaten zu sammeln, zu speichern und abzurufen, während gleichzeitig die strengen Anforderungen von Payment Card Industry Data Security Standard (PCI-DSS) eingehalten werden.

> Sie sind für die Durchführung angemessener Sicherheits- und Konformitätsprüfungen für jede Lösung verantwortlich, die mit der von dieser POC verwendeten Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten Ihrer Implementierung und Region unterschiedlich sein können. PCI-DSS erfordert, dass Sie direkt mit einem zugelassenen Qualified Security Assessor arbeiten, damit dieser Ihre für den Einsatz vorgesehene Lösung zertifiziert.

### <a name="elements-of-the-foundational-architecture"></a>Elemente der grundlegenden Architektur

Die grundlegende Architektur ist mit den folgenden fiktiven Elementen konzipiert:

Domänenwebsite `contosowebstore.com`

Benutzerrollen, mit denen der Anwendungsfall veranschaulicht und ein Einblick in die Benutzeroberfläche bereitgestellt wird

#### <a name="role-site-and-subscription-admin"></a>Rolle: Website- und Abonnementadministrator

|Item      |Beispiel|
|----------|------|
|Benutzername: |`adminXX@contosowebstore.com`|
| Name: |`Global Admin Azure PCI Samples`|
|Benutzertyp:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- Über das Administratorkonto ist es nicht möglich, Kreditkarteninformationen ohne Maskierung zu lesen. Alle Aktionen werden protokolliert.
- Mit dem Administratorkonto ist es weder möglich, sich in SQL-Datenbank anzumelden, noch diese zu verwalten.
- Mit dem Administratorkonto können Active Directory und Abonnements verwaltet werden.

#### <a name="role-sql-administrator"></a>Rolle: SQL-Administrator

|Item      |Beispiel|
|----------|------|
|Benutzername: |`sqlAdmin@contosowebstore.com`|
| Name: |`SQLADAdministrator PCI Samples`|
| Vorname: |`SQL AD Administrator`|
|Nachname: |`PCI Samples`|
|Benutzertyp:| `Administrator`|

- Mit dem Konto „sqladmin“ ist es nicht möglich, Kreditkarteninformationen ungefiltert anzuzeigen. Alle Aktionen werden protokolliert.
- Mit dem Konto „sqladmin“ kann die SQL-Datenbank verwaltet werden.

#### <a name="role-clerk"></a>Rolle: Sachbearbeiterin

|Item      |Beispiel|
|----------|------|
|Benutzername:| `receptionist_EdnaB@contosowebstore.com`|
| Name: |`Edna Benson`|
| Vorname:| `Edna`|
|Nachname:| `Benson`|
| Benutzertyp: |`Member`|

Edna Benson ist Empfangsdame und Sachbearbeiterin. Sie ist dafür verantwortlich, dass Kundeninformationen fehlerfrei sind und die Abrechnung erfolgt. Edna ist die Benutzerin, die für alle Interaktionen mit der Demowebsite von Contoso Webstore angemeldet wird. Edna hat die folgenden Rechte: 

- Edna kann Kundeninformationen erstellen und lesen.
- Edna kann Kundeninformationen ändern.
- Edna kann Kreditkartennummer, Ablaufdatum und Kreditkartenprüfnummer überschreiben oder ersetzen.

### <a name="contoso-webstore---estimated-pricing"></a>Contoso Webstore – geschätzte Kosten

Diese grundlegende Architektur und diese Beispielwebanwendung haben eine monatliche Gebührenstruktur sowie Nutzungskosten pro Stunde, die beim Festlegen der Größe der Lösung berücksichtigt werden müssen. Diese Kosten können mit dem [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) geschätzt werden. Seit September 2017 betragen die voraussichtlichen monatlichen Kosten für diese Lösung rund 2.500 USD (einschließlich einer monatlichen Nutzungsgebühr von 1.000 USD für ASE v2). Diese Kosten variieren entsprechend dem Nutzungsumfang und können geändert werden. Es obliegt dem Kunden, seine geschätzten monatlichen Kosten zum Zeitpunkt der Bereitstellung zu berechnen, um eine genauere Schätzung zu erhalten. 

In dieser Lösung wurden die folgenden Azure-Dienste verwendet. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

>- Application Gateway
>- Azure Active Directory
>- App Service-Umgebung v2
>- Log Analytics
>- Azure Key Vault
>- Netzwerksicherheitsgruppen
>- Azure SQL-Datenbank
>- Azure Load Balancer
>- Application Insights
>- Azure Security Center
>- Azure-Web-App
>- Azure-Automatisierung
>- Azure Automation-Runbooks
>- Azure DNS
>- Virtuelles Azure-Netzwerk
>- Virtueller Azure-Computer
>- Azure-Ressourcengruppe und Richtlinien
>- Azure Blob Storage
>- Rollenbasierte Zugriffssteuerung (RBAC) in Azure Active Directory

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur

Im folgenden Abschnitt werden die Elemente für Entwicklung und Implementierung beschrieben.

### <a name="network-segmentation-and-security"></a>Netzwerksegmentierung und -sicherheit

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Application Gateway

Die grundlegende Architektur verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Funktionen:

- [End-to-End-SSL](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Aktivierte [SSL-Auslagerung](/azure/application-gateway/application-gateway-ssl-portal)
- Deaktivieren von [TLS v1.0 und v1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (WAF-Modus)
- [Präventionsmodus](/azure/application-gateway/application-gateway-web-application-firewall-portal) mit OWASP 3.0-Regelsatz
- Aktivieren der [Diagnoseprotokollierung](/azure/application-gateway/application-gateway-diagnostics)
- [Benutzerdefinierte Integritätstests](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) und [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieten zusätzlichen Schutz und zusätzliche Benachrichtigungen. Azure Security Center stellt außerdem ein Reputationssystem bereit.

#### <a name="virtual-network"></a>Virtuelles Netzwerk

Die grundlegende Architektur definiert ein privates virtuelles Netzwerk mit dem Adressraum 10.0.0.0/16.

#### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Jede der Netzwerkschichten hat eine dedizierte Netzwerksicherheitsgruppe (NSG):
- Eine DMZ-Netzwerksicherheitsgruppe für Firewall- und Application Gateway-WAF
- Eine NSG für die verwaltende Jumpbox (Bastionhost bzw. geschützter Host)
- Eine NSG für die App Service-Umgebung

Jede der Netzwerksicherheitsgruppen (NSG) hat bestimmte Ports und Protokolle, die für den sicheren und richtigen Betrieb der Lösung geöffnet sind. Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](#network-security-groups).

Jede der Netzwerksicherheitsgruppen (NSG) hat bestimmte Ports und Protokolle, die für die sichere und richtige Arbeitsweise der Lösung geöffnet sind. Darüber hinaus werden die folgenden Konfigurationen für jede NSG aktiviert:
- Aktivierte [Diagnoseprotokolle und -ereignisse](/azure/virtual-network/virtual-network-nsg-manage-log) werden im Speicherkonto gespeichert. 
- Log Analytics verbunden mit der [Diagnose der NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

#### <a name="subnets"></a>Subnetze
 Stellen Sie sicher, dass jedes Subnetz seiner entsprechenden NSG zugeordnet ist.

#### <a name="custom-domain-ssl-certificates"></a>Benutzerdefinierte SSL-Zertifikate für die Domäne
 HTTPS-Datenverkehr wird mit einem benutzerdefinierten SSL-Zertifikat für die Domäne aktiviert.

### <a name="data-at-rest"></a>Ruhende Daten

Die Architektur schützt ruhende Daten durch Verwenden von Verschlüsselung, Datenbanküberwachung und andere Maßnahmen.

#### <a name="azure-storage"></a>Azure Storage

Um die Anforderungen für verschlüsselte ruhende Daten zu erfüllen, wird für den gesamten [Azure Storage](https://azure.microsoft.com/services/storage/) die Verschlüsselung [Storage Service Encryption](/azure/storage/storage-service-encryption) verwendet.

#### <a name="azure-sql-database"></a>Azure SQL-Datenbank

In der SQL-Datenbankinstanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:

- [AD-Authentifizierung und -Autorisierung](/azure/sql-database/sql-database-aad-authentication)
- [SQL-Datenbanküberwachung](/azure/sql-database/sql-database-auditing-get-started)
- [Transparente Datenverschlüsselung (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [Firewallregeln](/azure/sql-database/sql-database-firewall-configure), die ASE-Workerpools und Client-IP-Verwaltung zulassen
- [SQL-Bedrohungserkennung](/azure/sql-database/sql-database-threat-detection-get-started)
- [Always Encrypted-Spalten](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)
- [Dynamische Datenmaskierung für SQL-Datenbank](/azure/sql-database/sql-database-dynamic-data-masking-get-started) mit dem PowerShell-Skript nach der Bereitstellung

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung

[Log Analytics](https://azure.microsoft.com/services/log-analytics) kann für den Contoso Webstore eine umfangreiche Protokollierung aller System- und Benutzeraktivitäten bereitstellen, einschließlich Protokollierung der Daten des Karteninhabers. Änderungen können auf Ihre Richtigkeit hin überprüft werden. 

- **Aktivitätsprotokolle:** [Aktivitätsprotokolle](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in Ihrem Abonnement ausgeführt wurden.
- **Diagnoseprotokolle:** [Diagnoseprotokolle](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) sind alle Protokolle, die von jeder Ressource ausgegeben werden. Diese Protokolle umfassen Windows-Ereignissystemprotokolle, Azure Blob Storage-, Tabellen- und Warteschlangenprotokolle.
- **Firewallprotokolle:** Das Application Gateway stellt umfassende Diagnose- und Zugriffsprotokolle bereit. Firewallprotokolle stehen für Application Gateway-Ressourcen zur Verfügung, für die WAF aktiviert ist.
- **Protokollarchivierung:** Alle Diagnoseprotokolle sind so konfiguriert, dass sie in ein zentrales und verschlüsseltes Azure-Speicherkonto für die Archivierung mit einer definierten Beibehaltungsdauer (2 Tage) geschrieben werden. Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige mit Azure Log Analytics verbunden. [Log Analytics](https://azure.microsoft.com/services/log-analytics) ist ein Dienst, mit dem Daten gesammelt und analysiert werden können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert wurden.

### <a name="encryption-and-secrets-management"></a>Verschlüsselung und Verwaltung geheimer Schlüssel

Im Contoso Webstore werden alle Kreditkartendaten verschlüsselt und wird Azure Key Vault verwendet, um Schlüssel zu verwalten, sodass ein Abrufen von Karteninhaberdaten verhindert wird.

- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) unterstützt Sie dabei, kryptografische Schlüssel und geheime Schlüssel zu schützen, die von Cloudanwendungen und -diensten verwendet werden. 
- [SQL TDE](/sql/relational-databases/security/encryption/transparent-data-encryption) wird verwendet, um für jeden Kunden die Karteninhaberdaten, das Ablaufdatum und die Kreditkartenprüfnummer zu verschlüsseln.
- Daten werden mit [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) und BitLocker auf dem Datenträger gespeichert.

### <a name="identity-management"></a>Identitätsverwaltung

Die folgenden Technologien bieten Identitätsverwaltungsfunktionen in der Azure-Umgebung.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für die Lösung wurden in Azure Active Directory erstellt, so auch die Benutzer, die auf die SQL-Datenbank zugreifen.
- Authentifizierung für die Anwendung erfolgt mithilfe von Azure AD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications). Darüber hinaus wird für Verschlüsselung der Datenbankspalten ebenfalls Azure AD verwendet, um die Anwendung in der Azure SQL-Datenbank zu authentifizieren. Weitere Informationen finden Sie unter [Always Encrypted: Schützen von vertraulichen Daten in SQL-Datenbank](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory Identity Protection](/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten Ihrer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten Ihrer Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.
- Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure](/azure/role-based-access-control/role-assignments-portal) ermöglicht eine präzise Zugriffsverwaltung für Azure. Abonnementzugriff hat ausschließlich der Abonnementadministrator, und Azure Key Vault-Zugriff haben alle Benutzer.

Weitere Informationen zum Verwenden der Sicherheitsfunktionen von Azure SQL-Datenbank finden Sie im Beispiel [Contoso Clinic Demo Application](https://github.com/Microsoft/azure-sql-security-sample).
   
### <a name="web-and-compute-resources"></a>Web- und Computeressourcen

#### <a name="app-service-environment"></a>App Service-Umgebung

[Azure App Service](/azure/app-service/) ist ein verwalteter Dienst für die Bereitstellung von Web-Apps. Die Contoso Webstore-Anwendung wird als eine [App Service-Web-App](/azure/app-service-web/app-service-web-overview) bereitgestellt.

[Azure App Service-Umgebung (ASE v2)](/azure/app-service/app-service-environment/intro) ist ein App Service-Feature, das eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von App Service-Apps mit umfangreicher Skalierung bereitstellt. Sie ist ein Premium-Serviceplan, der in dieser grundlegenden Architektur verwendet wird, um PCI-DSS-Konformität zu ermöglichen.

Aufgrund der Isolierung werden in ASUs nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Netzwerk bereitgestellt. Kunden haben eine genauere Kontrolle über den eingehenden und ausgehenden Anwendungs-Netzwerkdatenverkehr, und Anwendungen können über virtuelle Netzwerke sichere Hochgeschwindigkeitsverbindungen mit lokalen Unternehmensressourcen herstellen.

Verwendung von ASUs für diese Architektur ist für die folgenden Steuerelemente/Konfigurationen zulässig:

- Hosten in einem geschützten virtuellen Netzwerk und Netzwerksicherheitsregeln
- ASU, konfiguriert mit einem selbstsignierten ILB-Zertifikat für HTTPS-Kommunikation
- [Interner Lastenausgleichsmodus](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (Modus 3)
- Deaktivieren von [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings): ein TLS-Protokoll, das aus einer PCI-DSS-Sicht veraltet ist
- Ändern von [TLS-Verschlüsselung](/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Steuern der [N/W-Port für eingehenden Datenverkehr](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [WAF – Daten einschränken](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Zulassen von [SQL-Datenbank-Datenverkehr](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)


#### <a name="jumpbox-bastion-host"></a>Jumpbox (Bastionhost)

Weil die App Service-Umgebung geschützt und gesperrt ist, muss es einen Mechanismus geben, der es erlaubt, alle DevOps-Releases oder -Änderungen zu berücksichtigen, die notwendig sein könnten, so z.B. die Möglichkeit, die Web-App mit Kudu zu überwachen. Der virtuelle Computer ist hinter NAT-Lastenausgleich geschützt, wodurch Sie die Möglichkeit erhalten, die virtuelle Maschine über einen anderen Port als TCP-Port 3389 zu verbinden. 

Ein virtueller Computer wurde als ein Jumpbox (Bastionshost) mit den folgenden Konfigurationen erstellt:

-   [Antimalware-Erweiterung](/azure/security/azure-security-antimalware)
-   [OMS-Erweiterung](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure-Diagnoseerweiterung](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) mit Azure Key Vault (berücksichtigt Azure Government, PCI-DSS, HIPAA und weitere Anforderungen)
-   Eine [Richtlinie zum automatischen Herunterfahren](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/), um die Nutzung der Ressourcen virtueller Computer zu verringern, wenn sie nicht benötigt werden

### <a name="security-and-malware-protection"></a>Sicherheit und Schutz vor Schadsoftware

[Azure Security Center](https://azure.microsoft.com/services/security-center/) bietet eine zentrale Ansicht der Sicherheitsstatus sämtlicher Azure-Ressourcen. Sie können auf einen Blick überprüfen, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und schnell Ressourcen ermitteln, die Ihre Aufmerksamkeit erfordern.  

Bei [Azure-Ratgeber](/azure/advisor/advisor-overview) handelt es sich um einen personalisierten Cloudberater, der Sie mit bewährten Methoden zum Optimieren von Azure-Bereitstellungen unterstützt. Das Tool analysiert die Konfiguration Ihrer Ressourcen und Telemetriedaten zur Nutzung und macht anschließend Vorschläge, wie Sie die Wirtschaftlichkeit, Leistung, Hochverfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) für Azure Cloud Services und virtuelle Computer ist eine Echtzeitschutz-Lösung, mit der Viren, Spyware und andere Schadsoftware identifiziert und entfernt werden können. Sie können Warnungen konfigurieren, die ausgelöst werden, wenn bekannte Schadsoftware oder unerwünschte Software versucht, sich selbst zu installieren oder auf Ihren Azure-Systemen ausgeführt zu werden.

### <a name="operations-management"></a>Operations Management

#### <a name="application-insights"></a>Application Insights

Verwenden Sie [Application Insights](https://azure.microsoft.com/services/application-insights/), um nützliche Erkenntnisse mit der Leistungsverwaltung für Anwendungen und Sofortanalysen zu sammeln.

#### <a name="log-analytics"></a>Log Analytics

[Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist ein Dienst in Azure, mit dem Sie Daten sammeln und analysieren können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert werden.

#### <a name="management-solutions"></a>Verwaltungslösungen

Diese zusätzlichen Verwaltungslösungen sollten in Betracht gezogen und konfiguriert werden:
- [Aktivitätsprotokollanalyse](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Azure-Netzwerkanalysen](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Azure SQL-Analysen](/azure/log-analytics/log-analytics-azure-sql)
- [Änderungsnachverfolgung](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Key Vault-Analysen](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Dienstzuordnung](/azure/operations-management-suite/operations-management-suite-service-map)
- [Sicherheit und Überwachung](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Antischadsoftware](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Updateverwaltung](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Security Center-Integration

Die standardmäßige Bereitstellung dient dazu, ein Mindestmaß an Security Center-Empfehlungen bereitzustellen, wodurch ein fehlerfreier und sicherer Konfigurationsstatus angegeben ist. Sie können die Datensammlung aus Azure Security Center aktivieren. Weitere Informationen finden Sie unter [Schnellstarthandbuch zu Azure Security Center](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Die Komponenten für die Bereitstellung dieser Lösung sind im [Repository für PCI Blueprint-Code][code-repo] verfügbar. Die Bereitstellung der grundlegenden Architektur erfordert mehrere Schritte, die über Microsoft PowerShell 5.x ausgeführt werden. Um eine Verbindung mit der Website herzustellen, müssen Sie einen benutzerdefinierten Domänennamen (z. B. „contoso.com“) bereitstellen. Dies wird mit dem Parameter `-customHostName` in Schritt 2 angegeben. Weitere Informationen finden Sie unter [Kaufen eines benutzerdefinierten Domänennamens für Azure-Web-Apps](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Ein benutzerdefinierter Domänenname ist nicht erforderlich, um die Lösung erfolgreich bereitzustellen und auszuführen, aber es wird Ihnen nicht möglich sein, zu Demonstrationszwecken eine Verbindung mit der Website herzustellen.

In den Skripts werden Domänenbenutzer zu dem Azure AD-Mandanten hinzugefügt, den Sie angeben. Es wird empfohlen, einen neuen Azure AD-Mandanten zu erstellen, der zum Testen verwendet wird.

Wenn Sie bei der Bereitstellung auf Probleme stoßen, lesen Sie [FAQ and troubleshooting](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

Es wird dringend empfohlen, eine Neuinstallation von PowerShell zu verwenden, um die Lösung bereitzustellen. Alternativ können Sie sich vergewissern, dass Sie die neuesten Module verwenden, die für die ordnungsgemäße Ausführung der Installationsskripts erforderlich sind. In diesem Beispiel erfolgt die Anmeldung bei der Jumpbox (Bastionhost) und werden die folgenden Befehle ausgeführt. Beachten Sie, dass dadurch der Befehl für die benutzerdefinierte Domäne aktiviert wird.


1. Installieren Sie die erforderlichen Module, und richten Sie die Administratorrollen ordnungsgemäß ein.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Ausführliche Nutzungsanweisungen finden Sie unter [Script Instructions – Setup Administrative Account and Permission](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Installieren Sie die Updateverwaltung der Lösung (solution-update-management). 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Ausführliche Nutzungsanweisungen finden Sie unter [Script Instructions – Deploy and Configure Azure Resources](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. Protokollierung und Überwachung. Wenn die Lösung bereitgestellt wurde, kann ein Log Analytics-Arbeitsbereich geöffnet werden, und die im Lösungsrepository bereitgestellten Beispielvorlagen veranschaulichen, wie ein Überwachungsdashboard konfiguriert wird. Die Beispielvorlagen finden Sie im Ordner [omsDashboards](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Beachten Sie, dass Daten in Log Analytics erfasst werden müssen, damit Vorlagen ordnungsgemäß bereitgestellt werden. Dies kann je nach Standortaktivität eine Stunde oder länger dauern.
 
    Beziehen Sie beim Einrichten Ihrer Log Analytics-Protokollierung unter Umständen Sie diese Ressourcen ein:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverFarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Bedrohungsmodell

Ein Datenflussdiagramm (DFD) und ein Beispielbedrohungsmodell für den Contoso-Webstore ([Azure Blueprint Program](https://aka.ms/pciblueprintthreatmodel))

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Kundenzuständigkeitsmatrix

Kunden sind dafür verantwortlich, eine Kopie der [Matrix der Verantwortungsübersicht](https://aka.ms/pciblueprintcrm32) zu erhalten. In dieser Matrix sind die PCI-DSS-Anforderungen, die in der Verantwortung des Kunden, und die Anforderungen skizziert, die in der Verantwortung von Microsoft Azure liegen.

## <a name="pci-compliance-review"></a>Überprüfung der PCI-Konformität 

Die Lösung wurde von Coalfire Systems Inc. (Qualified Security Assessor für PCI-DSS) geprüft. Die [PCI-Konformitätsüberprüfung und Anleitung zur Implementierung](https://aka.ms/pciblueprintprocessingoverview) enthält eine vom Prüfer verfasste Überprüfung der Lösung sowie Überlegungen für das Transformieren der Vorlage in eine produktionsreife Bereitstellung.

## <a name="disclaimer-and-acknowledgements"></a>Haftungsausschluss und Danksagung

*September 2017*

- Dieses Dokument dient nur zu Informationszwecken. MICROSOFT UND AVYAN ÜBERNEHMEN KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.  
- Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft- oder Avyan-Produkten oder -Lösungen.  
- Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.  

  > [!NOTE]
  > Bestimmte Empfehlungen in diesem Beitrag können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.  

- Die Lösung in diesem Dokument ist als eine grundlegende Architektur gedacht und darf nicht ungeändert für Produktionszwecke verwendet werden. Erzielen von PCI-Konformität erfordert, dass sich Kunden an ihren Qualified Security Assessor wenden.  
- Alle Kundennamen, Transaktionsdatensätze und jegliche verknüpften Daten auf dieser Seite sind frei erfunden, wurden für diese grundlegende Architektur erstellt und werden nur zur Veranschaulichung bereitgestellt. Tatsächliche Ähnlichkeiten oder Verbindungen sind rein zufällig, und keine darf als beabsichtigt angesehen werden.  
- Diese Lösung wurde gemeinsam von Microsoft und Avyan Consulting entwickelt und ist unter der [MIT-Lizenz](https://opensource.org/licenses/MIT) verfügbar.
- Diese Lösung wurde von Coalfire geprüft. Coalfire ist Microsofts PCI-DSS-Prüfer. Der [PCI Compliance Review](https://aka.ms/pciblueprintcrm32) stellt eine unabhängige Drittanbieterüberprüfung der Lösung und der Komponenten bereit, die zu beachten sind. 

### <a name="document-authors"></a>Autoren des Dokuments

- *Frank Simorjay (Microsoft)*  
- *Gururaj Pandurangi (Avyan Consulting)*


[code-repo]: https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms "Code-Repository"

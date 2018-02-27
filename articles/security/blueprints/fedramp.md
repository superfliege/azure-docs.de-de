---
title: "Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP"
description: "Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 9b605e500925e8435b15ec8055f8d8f376888aaf
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="azure-security-and-compliance-blueprint---fedramp-web-applications-automation"></a>Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP

## <a name="overview"></a>Übersicht

Das [Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov) ist ein Programm der US-Regierung, das einen standardisierten Ansatz zur Sicherheitsbewertung, Autorisierung und kontinuierlichen Überwachung von Cloudprodukten und -diensten bietet. Diese Azure Security and Compliance Blueprint-Automatisierung bietet Anleitungen für den Einsatz einer mit FedRAMP konformen IaaS-Umgebung (Infrastructure-as-a-Service), die für eine einfache, internetfähige Webanwendung geeignet ist. Diese Lösung automatisiert die Bereitstellung und Konfiguration von Azure-Ressourcen für eine allgemeine Referenzarchitektur. Sie zeigt Wege auf, wie Kunden spezifische Sicherheits- und Konformitätsanforderungen erfüllen können und dient als Grundlage für Kunden, um ihre eigenen Lösungen in Azure zu entwickeln und zu konfigurieren. Die Lösung implementiert eine Teilmenge der Regulierungen aus der FedRAMP High-Grundlage (basierend auf NIST SP 800-53). Weitere Informationen zu den FedRAMP High-Anforderungen und zu dieser Lösung finden Sie unter [FedRAMP High-Anforderungen: Übersicht](fedramp-controls-overview.md). ***Hinweis: Diese Lösung wird für Azure Government bereitgestellt.***

Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden. Es reicht nicht aus, eine Anwendung ohne Änderungen in dieser Umgebung bereitzustellen, um die Anforderungen der FedRAMP High-Grundlage vollständig zu erfüllen. Beachten Sie Folgendes:
- Diese Architektur bietet eine Grundlage, um Kunden bei der Verwendung von Azure in FedRAMP-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können. 

Eine schnelle Übersicht über die Funktionsweise dieser Lösung finden Sie in diesem [Video](https://aka.ms/fedrampblueprintvideo), in dem die Bereitstellung erklärt und gezeigt wird.

Klicken Sie [hier](https://aka.ms/fedrampblueprintrepo), um Anleitungen zur Bereitstellung zu erhalten.

## <a name="solution-components"></a>Lösungskomponenten

Diese Azure Security and Compliance Blueprint-Automatisierung stellt automatisch eine Referenzarchitektur für IaaS-Webanwendungen mit vorkonfigurierten Sicherheitsregulierungen bereit, um Kunden bei der Einhaltung der FedRAMP-Anforderungen zu unterstützen. Die Lösung besteht aus Azure Resource Manager-Vorlagen und PowerShell-Skripts, die die Ressourcenbereitstellung und -konfiguration steuern. Die begleitende [Konformitätsdokumentation](#compliance-documentation) verweist auf die Vererbung der Sicherheitsregulierungen aus Azure und beschreibt die bereitgestellten Ressourcen und Konfigurationen, die auf die Sicherheitsregulierungen aus NIST SP 800-53 abgestimmt sind, sodass Unternehmen ihren Konformitätsverpflichtungen schneller nachkommen können.

## <a name="architecture-diagram"></a>Architekturdiagramm

Diese Lösung stellt eine Referenzarchitektur für eine IaaS-Webanwendung mit einem Datenbank-Back-End bereit. Die Architektur umfasst eine Webschicht, eine Datenschicht, eine Active Directory-Infrastruktur, ein Application Gateway und ein Lastenausgleichsmodul. Virtuelle Computer, die in den Web- und Datenschichten bereitgestellt werden, werden in einer Verfügbarkeitsgruppe konfiguriert, und SQL Server-Instanzen werden in einer AlwaysOn-Verfügbarkeitsgruppe für Hochverfügbarkeit konfiguriert. Virtuelle Computer sind in Domänen eingebunden, und Active Directory-Gruppenrichtlinien werden verwendet, um Sicherheits- und Konformitätskonfigurationen auf Betriebssystemebene durchzusetzen. Eine Management Jumpbox (Bastionhost) bietet Administratoren eine sichere Verbindung für den Zugriff auf bereitgestellte Ressourcen.

![Alternativer Text](images/fedramp-architectural-diagram.png?raw=true "Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP")

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

* **Dokumentation zu virtuellen Computern**
    - (1) Management/Bastion (Windows Server 2016 Datacenter)
    - (2) Active Directory-Domänencontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-Clusterknoten (SQL Server 2016 unter Windows Server 2012 R2)
    - (1) SQL Server-Zeuge (Windows Server 2016 Datacenter)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
* **Verfügbarkeitsgruppen**
    - (1) Active Directory-Domänencontroller
    - (1) SQL-Clusterknoten und -Zeuge
    - (1) Web/IIS
* **Azure Virtual Network**
    - (1) /16 virtuelle Netzwerke
    - (5) /24 Subnetze
    - DNS-Einstellungen werden auf beiden Domänencontrollern festgelegt.
* **Azure-Lastenausgleich**
    - (1) SQL-Lastenausgleich
* **Azure Application Gateway**
    - (1) WAF Application Gateway (aktiviert)
      - Firewallmodus: Prävention
      - Regelsatz: OWASP 3.0
      - Listener: Port 443
* **Azure Storage**
    - (7) Georedundante Speicherkonten
* **Azure Backup**
    - (1) Recovery Services-Tresor
* **Azure-Schlüsseltresor**
    - (1) Schlüsseltresor
* **Azure Active Directory**
* **Azure Resource Manager**
* **Azure Log Analytics**
* **Azure Automation**
    - (1) Automation-Konto
* **Operations Management Suite**
    - (1) OMS-Arbeitsbereich

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur

Im folgenden Abschnitt werden die Elemente für Entwicklung und Implementierung beschrieben.

### <a name="network-segmentation-and-security"></a>Netzwerksegmentierung und -sicherheit

#### <a name="application-gateway"></a>Application Gateway

Die Architektur verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Funktionen:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Aktivierte [SSL-Auslagerung](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Deaktivieren von [TLS v1.0 und v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-Modus)
- [Präventionsmodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) mit OWASP 3.0-Regelsatz

#### <a name="virtual-network"></a>Virtuelles Netzwerk

Die Architektur definiert ein privates virtuelles Netzwerk mit dem Adressraum 10.200.0.0/16.

#### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Diese Lösung stellt Ressourcen in einer Architektur mit einem separaten Websubnetz, Datenbanksubnetz, Active Directory-Subnetz und Managementsubnetz in einem virtuellen Netzwerk bereit. Subnetze werden logisch durch Regeln für Netzwerksicherheitsgruppen getrennt, die auf die einzelnen Subnetze angewendet werden, um den Datenverkehr zwischen den Subnetzen auf das für die System- und Verwaltungsfunktionen erforderliche Maß einzuschränken.

Details finden Sie in der Konfiguration für [Netzwerksicherheitsgruppen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json), die mit dieser Lösung bereitgestellt wird. Organisationen können Netzwerksicherheitsgruppen konfigurieren, indem sie die Datei oben bearbeiten und dabei [diese Dokumentation](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als Leitfaden verwenden.

Jedes der Subnetze verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG):
- 1 NSG für Application Gateway (LBNSG)
- 1 NSG für Jumpbox (MGTNSG)
- 1 NSG für primären und Sicherungsdomänencontroller (ADNSG)
- 1 NSG für Computer mit SQL Server und Dateifreigabezeuge (SQLNSG)
- 1 NSG für Webschicht (WEBNSG)

#### <a name="subnets"></a>Subnetze

Jedem Subnetz ist die entsprechende NSG zugeordnet.

### <a name="data-at-rest"></a>Ruhende Daten

Die Architektur schützt ruhende Daten mithilfe mehrerer Verschlüsselungsmaßnahmen.

#### <a name="azure-storage"></a>Azure Storage

Um den Verschlüsselungsanforderungen von ruhenden Daten gerecht zu werden, verwenden alle Speicherkonten [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

#### <a name="sql-database"></a>SQL-Datenbank

Die SQL-Datenbank ist so konfiguriert, dass sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) verwendet, die Echtzeitverschlüsselung und -entschlüsselung von Daten und Protokolldateien ausführt, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte Daten keinen unbefugten Zugriffen ausgesetzt sind. 

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Azure Disk Encryption wird zur Verschlüsselung der Datenträger von virtuellen Windows-IaaS-Computern verwendet. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) nutzt die BitLocker-Funktion von Windows, um Volumeverschlüsselung für Betriebssysteme und Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) bietet umfassende Protokollierung von System- und Benutzeraktivitäten sowie der Dateisystemintegrität. 

- **Aktivitätsprotokolle:** [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in Ihrem Abonnement ausgeführt wurden.
- **Diagnoseprotokolle:** [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) sind alle Protokolle, die von jeder Ressource ausgegeben werden. Zu diesen Protokollen gehören Windows-Ereignissystemprotokolle, Azure-Speicherprotokolle, Key Vault-Überwachungsprotokolle sowie Application Gateway-Zugriffs- und -Firewallprotokolle.
- **Protokollarchivierung:** Azure-Aktivitätsprotokolle und -Diagnoseprotokolle können für die Verarbeitung, Speicherung und Dashboardverwendung mit Azure Log Analytics verbunden werden. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden.

### <a name="secrets-management"></a>Verwaltung geheimer Schlüssel

Die Lösung verwendet Azure Key Vault zum Verwalten von Schlüsseln und geheimen Schlüsseln.

- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) unterstützt Sie dabei, kryptografische Schlüssel und geheime Schlüssel zu schützen, die von Cloudanwendungen und -diensten verwendet werden. 
- Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern.

### <a name="identity-management"></a>Identitätsverwaltung

Die folgenden Technologien bieten Identitätsverwaltungsfunktionen in der Azure-Umgebung.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.
- Die Authentifizierung mit einer vom Kunden bereitgestellten Webanwendung kann mit Azure AD ausgeführt werden. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ermöglicht eine präzise Zugriffsverwaltung für Azure. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt, und der Zugriff auf Ressourcen kann je nach Benutzerrolle eingeschränkt werden.
- Eine bereitgestellte IaaS-Active-Directory-Instanz bietet Identitätsverwaltung auf Betriebssystemebene für bereitgestellte virtuelle IaaS-Computer.
   
### <a name="compute-resources"></a>Computeressourcen

#### <a name="web-tier"></a>Webschicht

Die Lösung stellt virtuelle Computer der Webschicht in einer [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bereit. Verfügbarkeitsgruppen stellen sicher, dass die virtuellen Computer über mehrere isolierte Hardwarecluster verteilt sind, um die Verfügbarkeit zu verbessern.

#### <a name="database-tier"></a>Datenbankschicht

Die Lösung stellt virtuelle Computer der Datenbankschicht in einer Verfügbarkeitsgruppe als [AlwaysOn-Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview) bereit. Das Feature „AlwaysOn-Verfügbarkeitsgruppe“ ermöglicht für Funktionen für hohe Verfügbarkeit und Notfallwiederherstellung. 

#### <a name="active-directory"></a>Active Directory

Alle von der Lösung bereitgestellten virtuellen Computer sind in Domänen eingebunden, und Active Directory-Gruppenrichtlinien werden verwendet, um Sicherheits- und Konformitätskonfigurationen auf Betriebssystemebene durchzusetzen. Virtuelle Active Directory-Computer werden in einer Verfügbarkeitsgruppe bereitgestellt.


#### <a name="jumpbox-bastion-host"></a>Jumpbox (Bastionhost)

Eine Management Jumpbox (Bastionhost) bietet Administratoren eine sichere Verbindung für den Zugriff auf bereitgestellte Ressourcen. Die NSG, die dem Verwaltungssubnetz zugeordnet ist, in dem sich die der virtuelle Jumpbox-Computer befindet, erlaubt Verbindungen nur an TCP-Port 3389 für RDP. 

### <a name="malware-protection"></a>Schutz vor Schadsoftware

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) für Virtual Machines bietet Echtzeitschutzfunktionen, mit denen die Identifizierung und Entferung von Viren, Spyware und anderer bösartiger Software ermöglicht wird, mit konfigurierbaren Warnmeldungen, wenn bekannte bösartige oder unerwünschte Software versucht, sich auf geschützten virtuellen Computern zu installieren oder die Ausführung zu starten.

### <a name="patch-management"></a>Patchverwaltung

Virtuelle Windows-Computer, die durch diese Azure Security and Compliance Blueprint-Automatisierung bereitgestellt werden, sind standardmäßig so konfiguriert, dass sie automatische Updates von Windows Update Service erhalten. Diese Lösung stellt auch die OMS Azure Automation-Lösung bereit, mit der Updatebereitstellungen erstellt werden können, um Patches auf Windows-Servern bei Bedarf bereitzustellen.

### <a name="operations-management"></a>Operations Management

#### <a name="log-analytics"></a>Log Analytics

Log Analytics ist ein Dienst in der [Operations Management Suite (OMS)](https://azure.microsoft.com/services/log-analytics/), der die Erfassung und Analyse von Daten ermöglicht, die von Ressourcen in Ihren Azure- und lokalen Umgebungen generiert werden.

#### <a name="oms-solutions"></a>OMS-Lösungen

Die folgenden OMS-Lösungen werden als Teil dieser Lösung vorinstalliert:
- [AD-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment)
- [Antischadsoftwarebewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware)
- [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)
- [Sicherheit und Überwachung](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started)
- [SQL-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment)
- [Updateverwaltung](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management)
- [Agent-Integrität](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth)
- [Azure-Aktivitätsprotokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)
- [Änderungsnachverfolgung](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)

## <a name="compliance-documentation"></a>Konformitätsdokumentation

### <a name="customer-responsibility-matrix"></a>Kundenzuständigkeitsmatrix

Die [Kundenzuständigkeitsmatrix](https://aka.ms/blueprinthighcrm) (Excel-Arbeitsmappe) listet alle Sicherheitsregulierungen auf, die für die FedRAMP High-Grundlage erforderlich sind. Die Matrix gibt für jede Regulierung (oder Unterregulierung) an, ob die Implementierungsverantwortung für die Regulierung in der Verantwortung von Microsoft oder des Kunden liegt oder von beiden geteilt wird. 

### <a name="control-implementation-matrix"></a>Regulierungsimplementierungsmatrix

Die [Regulierungsimplementierungsmatrix](https://aka.ms/blueprintwacim) (Excel-Arbeitsmappe) listet alle Sicherheitsregulierungen auf, die für die FedRAMP High-Grundlage erforderlich sind. Die Matrix gibt für jede Regulierung (oder Unterregulierung), die in der Kundenzuständigkeitsmatrix als Kundenzuständigkeit bezeichnet wird, an, ob die Blueprintautomatisierung die Regulierung implementiert, und beschreibt, inwiefern die Implementierung den Regulierungsanforderungen entspricht. Dieser Inhalt ist auch [hier](fedramp-controls-overview.md) verfügbar.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Diese Azure Security and Compliance Blueprint-Automatisierung besteht aus JSON-Konfigurationsdateien und PowerShell-Skripts, die vom API-Dienst von Azure Resource Manager verwaltet werden, um Ressourcen innerhalb von Azure bereitzustellen. Ausführlichere Bereitstellungsanweisungen finden Sie [hier](https://aka.ms/fedrampblueprintrepo). ***Hinweis: Diese Lösung wird für Azure Government bereitgestellt.***

#### <a name="quickstart"></a>Schnellstart
1. Klonen oder laden Sie [dieses](https://aka.ms/fedrampblueprintrepo) GitHub-Repository auf die lokale Arbeitsstation herunter.

2. Führen Sie das PowerShell-Skript vor der Bereitstellung aus: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Klicken Sie auf die Schaltfläche unten, melden Sie sich im Azure-Portal an, geben Sie die erforderlichen ARM-Vorlagenparameter ein, und klicken Sie dann auf **Kaufen**.

    [![Bereitstellen in Azure](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="disclaimer"></a>Haftungsausschluss

- Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.  
- Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.  
- Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.  
- Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.  
- Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
- Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.

---
title: Vorlage für Azure Security and Compliance – Analyse für die DSGVO
description: Vorlage für Azure Security and Compliance – Analyse für die DSGVO
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: b4f40dfced7060dd01df7410d07ac5b7cfdf3176
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580700"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Vorlage für Azure Security and Compliance: Analyse für die DSGVO

## <a name="overview"></a>Übersicht
Die Datenschutz-Grundverordnung (DSGVO) enthält viele Anforderungen für das Sammeln, Speichern und Verwenden personenbezogener Daten. Diese umfassen auch das Identifizieren und Schützen dieser Daten durch Organisationen, die Einhaltung der Transparenzanforderungen, das Erkennen und Melden von Sicherheitsverletzungen im Zusammenhang mit personenbezogenen Daten sowie das Schulen von Datenschutzbeauftragten und anderen Mitarbeitern. Die DSGVO bietet Einzelpersonen mehr Kontrolle über ihre persönlichen Daten und ist mit vielen neuen Pflichten für Organisationen verbunden, die personenbezogene Daten sammeln, verarbeiten oder analysieren. Die DSGVO enthält neue Vorschriften für Organisationen, die Menschen in der Europäischen Union (EU) Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. Die DSGVO gilt unabhängig vom Standort der Organisation.

Microsoft hat Azure basierend auf branchenweit führenden Sicherheitsmaßnahmen und Datenschutzrichtlinien zum Schutz von Daten in der Cloud entwickelt, zu denen auch personenbezogene Daten gehören, die der DSGVO unterliegen. Gemäß den eigenen [Vertragsbedingungen](http://aka.ms/Online-Services-Terms) muss Microsoft die Anforderungen von datenverarbeitenden Organisationen einhalten.

Diese Vorlage für Azure Security and Compliance enthält eine Anleitung für das Bereitstellen einer Datenanalysearchitektur in Azure, die hilft, die Anforderungen der DSGVO zu erfüllen. Diese Lösung zeigt Möglichkeiten, mit denen Kunden bestimmte Sicherheits- und Konformitätsanforderungen erfüllen können, und sie dient Kunden als Grundlage für das Erstellen und Konfigurieren eigener Datenanalyselösungen in Azure. Kunden können diese Referenzarchitektur nutzen und für die Einhaltung der DSGVO den aus [vier Schritten bestehenden Prozess](https://aka.ms/gdprebook) von Microsoft befolgen:
1. Ermitteln: Identifizieren Sie, welche personenbezogenen Daten vorhanden sind und wo sie sich befinden.
2. Verwalten: Legen Sie fest, wie personenbezogene Daten verwendet werden und wie darauf zugegriffen wird.
3. Schützen: Richten Sie Sicherheitsmaßnahmen ein, um Sicherheitsrisiken und Sicherheitsverletzungen bei Daten zu verhindern, zu erkennen und darauf zu reagieren.
4. Melden: Führen Sie die erforderliche Dokumentation, und erfassen Sie Datenanforderungen und Benachrichtigungen über Sicherheitsverletzungen.

Diese Referenzarchitektur, die zugehörige Implementierungsanleitung und das Bedrohungsmodell sollen Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollten nicht unverändert in einer Produktionsumgebung verwendet werden. Beachten Sie Folgendes:
- Diese Architektur dient als Basis, um Kunden bei der Bereitstellung von Workloads in Azure in DSGVO-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können.

## <a name="architecture-diagram-and-components"></a>Architekturdiagramm und Komponenten
Diese Lösung umfasst eine Analyseplattform, auf der Kunden ihre eigenen Analysetools aufbauen können. Die Referenzarchitektur stellt einen generischen Anwendungsfall dar. Dabei geben Kunden Daten entweder durch einen Massenimport durch den SQL- bzw. den Datenadministrator oder durch betriebliche Datenupdates über einen betrieblichen Benutzer ein. Bei beiden Möglichkeiten wird Azure Functions für den Import von Daten in Azure SQL-Datenbank integriert. Azure Functions muss vom Kunden über das Azure-Portal konfiguriert werden, damit die Importaufgaben entsprechend der jeweiligen Analyseanforderungen des Kunden verarbeitet werden können.

Azure bietet den Kunden verschiedene Dienste zur Berichterstellung und Analyse. Diese Lösung integriert Azure Machine Learning-Dienste in Azure SQL-Datenbank, damit Daten schnell durchsucht werden und durch eine bessere Modellierung von Kundendaten schneller Ergebnisse geliefert werden können. Azure Machine Learning umfasst maschinelles Lernen und ist darauf ausgerichtet, die Abfragegeschwindigkeit zu erhöhen, indem neue Beziehungen zwischen Datasets ermittelt werden. Wenn die Daten über verschiedene statistische Funktionen trainiert wurden, können bis zu sieben zusätzliche Abfragepools (mit dem Kundenserver acht) mit denselben tabellarischen Modellen synchronisiert werden, um die Abfrageworkload auszuweiten und die Antwortzeiten zu reduzieren.

Für erweiterte Analysen und Berichterstattungen können Azure SQL-Datenbanken mit Columnstore-Indizes konfiguriert werden. Azure Machine Learning und Azure SQL-Datenbanken können abhängig von der Nutzung durch den Kunden zentral hoch- bzw. herunterskaliert oder vollständig ausgeschaltet werden. Jeglicher SQL-Datenverkehr wird mit SSL über selbstsignierte Zertifikate verschlüsselt. Azure empfiehlt als bewährte Methode die Verwendung einer vertrauenswürdigen Zertifizierungsstelle für erweiterte Sicherheit.

Sobald Daten in Azure SQL-Datenbank hochgeladen und von Azure Machine Learning trainiert wurden, werden Sie sowohl von dem betrieblichen Benutzer als auch vom SQL- bzw. Datenadministrator über Power BI verarbeitet. Power BI zeigt Daten intuitiv an und erfasst Informationen für mehrere Datasets, um zusätzliche Erkenntnisse zu erhalten. Power BI weist ein hohes Maß an Flexibilität auf und kann in Azure SQL-Datenbank integriert werden. Dadurch wird sichergestellt, dass Kunden eine Konfiguration vornehmen können, damit dieser Dienst entsprechend der jeweiligen Geschäftsanforderungen die verschiedensten Szenarios verarbeiten kann.

Die gesamte Lösung baut auf dem Service Azure Storage auf, den Kunden mit einem Konto über das Azure-Portal konfigurieren. Azure Storage verschlüsselt alle Daten über die Speicherdienstverschlüsselung, um die Vertraulichkeit von ruhenden Daten zu bewahren. Über einen sogenannten georedundanten Speicher (GRS) wird sichergestellt, dass es bei einem Zwischenfall im primären Rechenzentrum des Kunden nicht zu einem Datenverlust kommt, da eine zweite Kopie an einem weiteren Standort mehrere Hundert Kilometer entfernt gespeichert wird.

Aus Sicherheitsgründen verwaltet diese Architektur Ressourcen über Azure Active Directory und Azure Key Vault. Die Systemintegrität wird von der Operations Management Suite (OMS) und Azure Monitor überwacht. Der Kunde konfiguriert das Überwachen von Diensten zum Speichern von Protokollen und Anzeigen der Systemintegration in nur einem Dashboard, durch das man leicht navigieren kann.

Azure SQL-Datenbank wird in der Regel über SQL Server Management Studio (SSMS) verwaltet. SSMS wird über einen lokalen Computer ausgeführt, der für den Zugriff auf Azure SQL-Datenbank über eine sichere VPN- oder ExpressRoute-Verbindung konfiguriert ist. **Azure empfiehlt das Konfigurieren einer VPN- oder ExpressRoute-Verbindung für die Verwaltung und den Datenimport in die Ressourcengruppe der Referenzarchitektur.**

![Abbildung der Referenzarchitektur für Analytics gemäß DSGVO](images/gdpr-analytics-architecture.png?raw=true "Analytics for GDPR reference architecture diagram")

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

- Azure-Funktionen
- Azure SQL-Datenbank
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure Monitor
- Azure Storage
- Power BI-Dashboard
- Azure Data Catalog
- Azure Security Center
- Application Insights
- Azure Event Grid
- Netzwerksicherheitsgruppen

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur
Im folgenden Abschnitt werden die Elemente für Entwicklung und Implementierung beschrieben.

Mit **Azure Event Grid**
[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) können Kunden mühelos Anwendungen mit ereignisbasierten Architekturen erstellen. Sie wählen die zu abonnierende Azure-Ressource aus und geben dem Ereignishandler oder Webhook einen Endpunkt an, an den das Ereignis gesendet werden soll. Kunden können ihren Webhook-Endpunkt sichern, indem sie der Webhook-URL beim Erstellen eines Ereignisabonnements Abfrageparameter hinzufügen. Azure Event Grid unterstützt nur HTTPS-Webhook-Endpunkte. Azure Event Grid bietet die Möglichkeit, den Umfang zu steuern, indem unterschiedliche Benutzer Zugriff auf verschiedene Verwaltungsvorgänge erhalten, z.B. Auflisten und Erstellen von Ereignisabonnements und Generieren von Schlüsseln. Event Grid nutzt die rollenbasierte Zugriffsüberprüfung (Role-Based Access Control, RBAC) von Azure.

**Azure Functions**
[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) ist ein serverloser Computedienst, mit dem Benutzer Code bedarfsgesteuert ausführen können, ohne eine explizite Infrastruktur bereitstellen oder verwalten zu müssen. Verwenden Sie Azure Functions, um ein Skript oder einen Codeabschnitt als Reaktion auf verschiedene Ereignisse auszuführen.

**Azure Machine Learning**
[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) ist ein Data Science-Verfahren, mit dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen.

**Azure Data Catalog:** [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) gestaltet Datenquellen einfach ermittelbar und verständlich für die Benutzer, die die verwalteten Daten benötigen. Häufig verwendete Datenquellen können registriert und markiert und nach personenbezogenen Daten durchsucht werden. Die Daten verbleiben an ihrem vorhandenen Speicherort, aber eine Kopie der Metadaten wird Azure Data Catalog hinzugefügt, zusammen mit einem Verweis auf den Speicherort der Datenquelle. Die Metadaten werden außerdem indiziert, damit jede Datenquelle per Suchfunktion einfach ermittelt werden kann und für die Benutzer, die sie ermitteln, verständlich ist.

### <a name="virtual-network"></a>Virtuelles Netzwerk
Die Referenzarchitektur definiert ein privates VNet mit dem Adressraum 10.0.0.0/16.

**Netzwerksicherheitsgruppen:** [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) enthalten Zugriffssteuerungslisten, die den Datenverkehr in einem VNet zulassen oder ablehnen. NSGs können dafür verwendet werden, den Datenverkehr auf der Ebene eines Subnetzes oder einzelner VMs zu schützen. Die folgenden NSGs sind vorhanden:
  - eine NSG für Active Directory
  - eine NSG für die Workload

Jede der NSGs verfügt über bestimmte offene Ports und Protokolle, damit die Lösung sicher und richtig ausgeführt werden kann. Darüber hinaus werden die folgenden Konfigurationen für jede NSG aktiviert:
  - [Diagnoseprotokolle und -ereignisse](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) werden aktiviert und in einem Speicherkonto gespeichert.
  - OMS Log Analytics ist mit der [Diagnose der NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) verbunden.

**Subnetze:** Jedes Subnetz ist seiner entsprechenden NSG zugeordnet.

### <a name="data-in-transit"></a>Daten während der Übertragung
Azure verschlüsselt standardmäßig die gesamte Kommunikation zu und von Azure-Rechenzentren. Alle Transaktionen für Azure Storage über das Azure-Portal werden per HTTPS durchgeführt.

### <a name="data-at-rest"></a>Ruhende Daten

Die Architektur schützt ruhende Daten durch die Verwendung von Verschlüsselung, Datenbanküberwachung und anderen Maßnahmen.

**Azure Storage:** Um die Anforderungen für verschlüsselte ruhende Daten zu erfüllen, wird für den gesamten [Azure Storage](https://azure.microsoft.com/services/storage/) die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/storage-service-encryption) verwendet. Dadurch werden personenbezogene Daten geschützt, um die Zusagen und Anforderungen der Organisation im Hinblick auf Sicherheit und Konformität gemäß der DSGVO einzuhalten.

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) nutzt das BitLocker-Features von Windows, um Volumeverschlüsselung für Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträgerverschlüsselungsschlüssel zu erleichtern.

**Azure SQL-Datenbank:** In der Azure SQL-Datenbankinstanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:
-   Die [AD-Authentifizierung und -Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten.
-   Die [SQL-Datenbank-Überprüfung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verfolgt Datenbankereignisse nach und schreibt diese in ein Überwachungsprotokoll in einem Azure Storage-Konto.
-   Azure SQL-Datenbank ist so konfiguriert, dass [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) für die Echtzeitverschlüsselung und -entschlüsselung der Datenbank, der zugehörigen Sicherungen und der Transaktionsprotokolldateien verwendet wird, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte personenbezogene Daten keinen unbefugten Zugriffen ausgesetzt sind.
-   [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) verhindern sämtlichen Zugriff auf Datenbankserver, bis die richtigen Berechtigungen erteilt werden. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
-   Die [SQL-Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) ermöglicht die Erkennung und Reaktion auf potenzielle Bedrohungen, sobald sie auftreten. Dazu werden Sicherheitswarnungen bei verdächtigen Datenbankaktivitäten, potenziellen Sicherheitslücken, Angriffen durch Einschleusung von SQL-Befehlen und anormalen Mustern beim Datenbankzugriff ausgegeben.
-   [Always Encrypted-Spalten](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) stellen sicher, dass vertrauliche personenbezogene Daten im Datenbanksystem niemals im Klartextformat angezeigt werden. Nach dem Aktivieren der Datenverschlüsselung können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel haben, auf Klartextdaten zugreifen.
- [Erweiterte Eigenschaften](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) können zum Beenden der Verarbeitung von Datenbesitzern verwendet werden. Benutzern wird es ermöglicht, Datenbankobjekten benutzerdefinierte Eigenschaften hinzuzufügen und Daten als „Eingestellt“ zu markieren und damit Anwendungslogik zu unterstützen, die das Verarbeiten der zugehörigen personenbezogenen Daten verhindert.
- Die [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ermöglicht Benutzern das Festlegen von Richtlinien zum Einschränken des Zugriffs auf Daten, um die Verarbeitung zu beenden.
- Die [dynamische Datenmaskierung in SQL-Datenbank (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung vertraulicher personenbezogener Daten ein, indem diese Daten für nicht berechtigte Benutzer maskiert werden. DDM kann automatisch potenziell vertrauliche Daten ermitteln und eine geeignete Maskierung vorschlagen. Dies hilft bei der Identifikation der personenbezogenen Daten, für die der Schutz durch die DSGVO gilt, und bei der Reduzierung des Zugriffs, damit sie die Datenbank nicht per nicht autorisiertem Zugriff verlassen. **Hinweis: Kunden müssen die DDM-Einstellungen an ihr Datenbankschema anpassen.**

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf personenbezogene Daten in der Azure-Umgebung:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in AAD erstellt. Dies gilt auch für Benutzer, die auf Azure SQL-Datenbank zugreifen.
-   Die Authentifizierung für die Anwendung erfolgt mithilfe von AAD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Darüber hinaus wird für Verschlüsselung der Datenbankspalten ebenfalls AAD verwendet, um die Anwendung in Azure SQL-Datenbank zu authentifizieren. Weitere Informationen finden Sie unter [Schützen von vertraulichen Daten in SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Die [rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer zum Erledigen ihrer Arbeit benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu gewähren, können Administratoren auch nur bestimmte Aktionen für den Zugriff auf personenbezogene Daten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) können Kunden die Anzahl von Benutzern mit Zugriff auf bestimmte Informationen, z.B. personenbezogene Daten, einschränken.  Administratoren können mit AAD Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
-   [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.

### <a name="security"></a>Sicherheit
**Geheimnisverwaltung:** Die Lösung verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen personenbezogener Daten und des Zugriffs auf diese Daten:
- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule (HSMs) geschützt. Der Schlüsseltyp ist ein HSM-geschützter 2.048-Bit-RSA-Schlüssel.
- Allen Benutzern und Identitäten werden mit RBAC die mindestens erforderlichen Berechtigungen erteilt.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

**Sicherheitswarnungen:** [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) ermöglicht Kunden die Überwachung von Datenverkehr, das Erfassen von Protokollen und das Analysieren von Datenquellen auf Bedrohungen. Darüber hinaus greift Azure Security Center auf die vorhandene Konfiguration der Azure-Dienste zu, um Empfehlungen für die Konfiguration und Verwaltung zu geben und zur Verbesserung der Sicherheit und zum Schutz personenbezogener Daten beizutragen. Azure Security Center umfasst einen [Bericht über Bedrohungen](https://docs.microsoft.com/azure/security-center/security-center-threat-report) zu jeder erkannten Bedrohung, der den Teams, die für die Bearbeitung von Incidents zuständig sind, beim Untersuchen und Beheben von Bedrohungen als Hilfe dient.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung

Die [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) bietet umfassende Protokollierung von System- und Benutzeraktivitäten sowie der Dateisystemintegrität. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist eine OMS-Lösung für die Erfassung und Analyse von Daten, die von Ressourcen in Azure- und lokalen Umgebungen generiert werden.
- **Aktivitätsprotokolle:** [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in einem Abonnement ausgeführt wurden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
- **Diagnoseprotokolle:** [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) sind alle Protokolle, die von den einzelnen Ressourcen ausgegeben werden. Diese Protokolle umfassen Windows-Ereignissystemprotokolle, Azure Blob Storage-, Tabellen- und Warteschlangenprotokolle.
- **Protokollarchivierung:** Alle Diagnoseprotokolle schreiben in ein zentrales und verschlüsseltes Azure Storage-Konto für die Archivierung mit einer definierten Beibehaltungsdauer von 2 Tagen. Diese Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige mit Azure Log Analytics verbunden.

Darüber hinaus sind in dieser Architektur die folgenden OMS-Lösungen enthalten:
-   [Active Directory-Bewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment) Die Active Directory-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Antischadsoftwarebewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware) Die Antischadsoftwarelösung meldet Schadsoftware, Bedrohungen und den Schutzstatus.
-   [Azure Automation:](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) Die Azure Automation-Lösung dient der Speicherung, Ausführung und Verwaltung von Runbooks.
-   [Sicherheit und Überwachung:](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) Das Sicherheits- und Überwachungsdashboard bietet einen allgemeinen Einblick in den Sicherheitszustand von Ressourcen. Hierzu werden Metriken zu Sicherheitsdomänen, relevanten Problemen, Erkennungen, Bedrohungsdaten und allgemeinen Sicherheitsabfragen bereitgestellt.
-   [SQL-Bewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment) Die SQL-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt für die Kunden eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Updateverwaltung:](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) Die Updateverwaltungslösung ermöglicht die Verwaltung von Betriebssystem-Sicherheitsupdates durch die Kunden. Dies umfasst auch den Status der verfügbaren Updates und den Fortschritt beim Installieren der erforderlichen Updates.
-   [Agent-Integritätsdiagnose:](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth) Die Agent-Integritätsdiagnoselösung meldet, wie viele Agents bereitgestellt werden und wie ihre geografische Verteilung ist. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
-   [Azure-Aktivitätsprotokolle:](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity) Die Aktivitätsprotokollanalyse-Lösung hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.
-   [Änderungsnachverfolgung:](https://docs.microsoft.com/azure/automation/automation-change-tracking) Die Änderungsnachverfolgungslösung ermöglicht Kunden das Identifizieren von Änderungen in der Umgebung auf einfache Weise.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) unterstützt Kunden beim Nachverfolgen der Leistung, Aufrechterhalten der Sicherheit und Identifizieren von Trends, indem Organisationen das Überwachen, Erstellen von Warnungen und Archivieren von Daten ermöglicht wird, z.B. die Nachverfolgung von API-Aufrufen in den kundeneigenen Azure-Ressourcen.

**Application Insights**
[Application Insights](https://docs.microsoft.com/azure/application-insights/) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um aktive Webanwendungen zu überwachen. Der Dienst erkennt Leistungsanomalien und verfügt über leistungsstarke Analysetools, mit denen Kunden Probleme diagnostizieren und nachvollziehen können, wie die App von den Benutzern verwendet wird. Er unterstützt Benutzer bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit der App.

## <a name="threat-model"></a>Bedrohungsmodell

Das Datenflussdiagramm für diese Referenzarchitektur steht zum [Download](https://aka.ms/gdprAnalyticsdfd) bereit und ist unten angegeben. Dieses Modell kann Kunden dabei helfen, bei Änderungen die potenziellen Risikopunkte in der Systeminfrastruktur zu verstehen.

![Analytics für das Bedrohungsmodell gemäß DSGVO](images/gdpr-analytics-threat-model.png?raw=true "Analytics for GDPR threat model")

## <a name="compliance-documentation"></a>Konformitätsdokumentation
In der Vorlage für die [Azure Security and Compliance – DSGVO-Zuständigkeitsmatrix für Kunden](https://aka.ms/gdprCRM) sind die Aufgaben von Controllern und Verarbeitern für alle DSGVO-Artikel aufgeführt. Beachten Sie, dass der Kunde bei Azure-Diensten in der Regel der Controller und Microsoft der Verarbeiter ist.

Die Vorlage für die [Azure Security and Compliance – Implementierungsmatrix für Datenanalyse gemäß DSGVO](https://aka.ms/gdprAnalytics) enthält Informationen darüber, auf welche DSGVO-Artikel durch die Datenanalysearchitektur verwiesen wird, sowie eine detaillierte Beschreibung, wie die Implementierung die Anforderungen der einzelnen Artikel erfüllt.


## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen
### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder ein sicherer VPN-Tunnel müssen für eine sichere Verbindung mit Ressourcen, die als Teil dieser Referenzarchitektur für die Datenanalyse bereitgestellt werden, konfiguriert werden. Durch die korrekte Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen in einem „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Der [IPsec-Tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wird bei dieser Option als Verschlüsselungsmechanismus verwendet.

Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>ETL-Prozess (Extrahieren, Transformieren, Laden)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kann Daten in Azure SQL-Datenbank laden, ohne ein separates ETL- oder Importtool zu verwenden. PolyBase ermöglicht den Zugriff auf Daten über T-SQL-Abfragen. Microsoft Business Intelligence und der Analysestapel sowie mit SQL Server kompatible Drittanbietertools können mit PolyBase verwendet werden.

### <a name="azure-active-directory-setup"></a>Azure Active Directory-Einrichtungen
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ist wichtig für die Verwaltung der Bereitstellung von Zugriff für Mitarbeiter, die mit der Umgebung interagieren. Ein bereits vorhandenes Windows Server Active Directory-Verzeichnis kann über [vier Klicks](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express) in Azure Active Directory integriert werden. Die Kunden können außerdem die bereitgestellte Active Directory-Infrastruktur (Domänencontroller) an ein bereits vorhandenes Azure Active Directory-Verzeichnis binden, indem sie die bereitgestellte Active Directory-Infrastruktur als untergeordnete Domäne für eine AAD-Struktur festlegen.

## <a name="disclaimer"></a>Haftungsausschluss

 - Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.
 - Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.
 - Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.
 - Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.
 - Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
 - Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.

---
title: Azure Security and Compliance Blueprint – Data Warehouse gemäß DSGVO
description: Azure Security and Compliance Blueprint – Data Warehouse gemäß DSGVO
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 6a777418c5381f1f52bae31ad4e697248587fc6d
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576543"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Azure Security and Compliance Blueprint: Data Warehouse gemäß DSGVO

## <a name="overview"></a>Übersicht
Die Datenschutz-Grundverordnung (DSGVO) enthält viele Anforderungen für das Sammeln, Speichern und Verwenden personenbezogener Daten. Diese umfassen auch das Identifizieren und Schützen dieser Daten durch Organisationen, die Einhaltung der Transparenzanforderungen, das Erkennen und Melden von Sicherheitsverletzungen im Zusammenhang mit personenbezogenen Daten sowie das Schulen von Datenschutzbeauftragten und anderen Mitarbeitern. Die DSGVO bietet Einzelpersonen mehr Kontrolle über ihre persönlichen Daten und ist mit vielen neuen Pflichten für Organisationen verbunden, die personenbezogene Daten sammeln, verarbeiten oder analysieren. Die DSGVO enthält neue Vorschriften für Organisationen, die Menschen in der Europäischen Union (EU) Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. Die DSGVO gilt unabhängig vom Standort der Organisation.

Microsoft hat Azure basierend auf branchenweit führenden Sicherheitsmaßnahmen und Datenschutzrichtlinien zum Schutz von Daten in der Cloud entwickelt, zu denen auch personenbezogene Daten gehören, die der DSGVO unterliegen. Gemäß den eigenen [Vertragsbedingungen](http://aka.ms/Online-Services-Terms) muss Microsoft die Anforderungen von datenverarbeitenden Organisationen einhalten.

Dieser Azure Security and Compliance Blueprint enthält eine Anleitung für das Bereitstellen einer Data Warehouse-Architektur in Azure, die hilft, die Anforderungen der DSGVO zu erfüllen. Diese Lösung zeigt Möglichkeiten, mit denen Kunden bestimmte Sicherheits- und Complianceanforderungen erfüllen können, und sie dient Kunden als Grundlage für das Erstellen und Konfigurieren eigener Data Warehouse-Lösungen in Azure. Kunden können diese Referenzarchitektur nutzen und für die Einhaltung der DSGVO den aus [vier Schritten bestehenden Prozess](https://aka.ms/gdprebook) von Microsoft befolgen:
1. Ermitteln: Identifizieren Sie, welche personenbezogenen Daten vorhanden sind und wo sie sich befinden.
2. Verwalten: Legen Sie fest, wie personenbezogene Daten verwendet werden und wie darauf zugegriffen wird.
3. Schützen: Richten Sie Sicherheitsmaßnahmen ein, um Sicherheitsrisiken und Sicherheitsverletzungen bei Daten zu verhindern, zu erkennen und darauf zu reagieren.
4. Melden: Führen Sie die erforderliche Dokumentation, und erfassen Sie Datenanforderungen und Benachrichtigungen über Sicherheitsverletzungen.

Diese Referenzarchitektur, die zugehörige Implementierungsanleitung und das Bedrohungsmodell sollen Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollten nicht unverändert in einer Produktionsumgebung verwendet werden. Beachten Sie Folgendes:
- Diese Architektur dient als Basis, um Kunden bei der Bereitstellung von Workloads in Azure in DSGVO-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können.

## <a name="architecture-diagram-and-components"></a>Architekturdiagramm und Komponenten
Diese Lösung bietet eine Referenzarchitektur, die ein leistungsstarkes und sicheres Cloud-Data Warehouse implementiert. Es gibt zwei separate Datenschichten in dieser Architektur: eine, in die die Daten importiert und gespeichert werden und die in einer SQL-Clusterumgebung gestaffelt ist, und eine Schicht für Azure SQL Data Warehouse, in der die Daten für die Verarbeitung mit einem ETL-Tool geladen werden (z.B. [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)-T-SQL-Abfragen). Für die in Azure SQL Data Warehouse gespeicherten Daten können umfangreiche Analysen durchgeführt werden.

Azure bietet eine Vielzahl von Berichts- und Analysediensten für Kunden. Diese Lösung umfasst SQL Server Reporting Services (SSRS) für die schnelle Erstellung von Berichten aus Azure SQL Data Warehouse. Jeglicher SQL-Datenverkehr wird mit SSL über selbstsignierte Zertifikate verschlüsselt. Azure empfiehlt als bewährte Methode die Verwendung einer vertrauenswürdigen Zertifizierungsstelle für erweiterte Sicherheit.

Daten in Azure SQL Data Warehouse werden in relationalen Tabellen spaltenweise gespeichert. Dieses Format reduziert die Kosten für die Datenspeicherung erheblich und verbessert die Abfrageleistung.  Je nach Nutzungsanforderungen kann Azure SQL Data Warehouse Ressourcen zentral hoch- oder herunterskalieren oder vollständig herunterfahren, wenn keine aktiven Prozesse Computeressourcen erfordern.

Ein SQL-Load Balancer verwaltet den SQL-Datenverkehr und stellt eine hohe Leistung sicher. Alle virtuellen Computer in dieser Referenzarchitektur werden in einer Verfügbarkeitsgruppe mit SQL Server-Instanzen bereitgestellt, die in einer AlwaysOn-Verfügbarkeitsgruppe für Hochverfügbarkeit und die Notfallwiederherstellung konfiguriert wurden.

Diese Data Warehouse-Referenzarchitektur enthält auch eine Active Directory-Ebene (AD) für die Verwaltung von Ressourcen innerhalb der Architektur. Das Active Directory-Subnetz ermöglicht die einfache Übernahme in einer größeren AD-Gesamtstruktur und erlaubt damit den fortlaufenden Betrieb der Umgebung, selbst wenn der Zugriff auf die größere Gesamtstruktur nicht möglich ist. Alle virtuellen Computer sind in Domänen der Active Directory-Ebene eingebunden und verwenden Active Directory-Gruppenrichtlinien, um Sicherheits- und Konformitätskonfigurationen auf Betriebssystemebene durchzusetzen.

Ein virtueller Computer fungiert als geschützter Host (Bastionhost) für die Verwaltung und bietet Administratoren eine sichere Verbindung für den Zugriff auf bereitgestellte Ressourcen. Die Daten werden über diesen verwaltenden Bastionhost in den Staging-Bereich geladen. **Azure empfiehlt das Konfigurieren einer VPN- oder Azure ExpressRoute-Verbindung für die Verwaltung und den Datenimport in das Subnetz der Referenzarchitektur.**

![Abbildung der Referenzarchitektur für Data Warehouse gemäß DSGVO](images/gdpr-datawarehouse-architecture.png?raw=true "Abbildung der Referenzarchitektur für Data Warehouse gemäß DSGVO")

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt [Bereitstellungsarchitektur](#deployment-architecture).

-   Azure Virtual Machines
    - (1) Bastionhost
    -   (2) Active Directory-Domänencontroller
    -   (2) SQL Server-Clusterknoten
    -   (1) SQL Server-Zeuge
-   Verfügbarkeitsgruppen
    - Active Directory-Domänencontroller
    - SQL-Clusterknoten und -Zeuge
-   Virtual Network
    - (4) Subnetze
    -   (4) Netzwerksicherheitsgruppen
- SQL Data Warehouse
- SQL Server Reporting Services
- Azure SQL-Lastenausgleich
- Azure Active Directory
- Recovery Services-Tresor
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure Data Catalog
- Azure Security Center

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur
Im folgenden Abschnitt werden die Elemente für Entwicklung und Implementierung beschrieben.

**SQL Data Warehouse:** [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) ist ein Enterprise Data Warehouse (EDW) mit Massively Parallel Processing (MPP) zur schnellen Ausführung komplexer Abfragen für Daten im Petabyte-Bereich, sodass Benutzer personenbezogene Daten sehr schnell ermitteln können. Benutzer können große Datenmengen mit einfachen PolyBase-T-SQL-Abfragen in SQL Data Warehouse importieren und dann die Vorteile von MPP für aufwendige Analysen nutzen.

**SQL Server Reporting Services (SSRS):** [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) erlaubt die schnelle Erstellung von Berichten mit Tabellen, Diagrammen, Karten, Messgeräten, Matrizen u.v.m. für Azure SQL Data Warehouse.

**Data Catalog:** [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) gestaltet Datenquellen einfach ermittelbar und verständlich für die Benutzer, die die Daten verwalten. Häufig verwendete Datenquellen können registriert und markiert und nach personenbezogenen Daten durchsucht werden. Die Daten verbleiben an ihrem vorhandenen Speicherort, aber eine Kopie der Metadaten wird Azure Data Catalog hinzugefügt, zusammen mit einem Verweis auf den Speicherort der Datenquelle. Die Metadaten werden außerdem indiziert, damit jede Datenquelle per Suchfunktion einfach ermittelt werden kann und für die Benutzer, die sie ermitteln, verständlich ist.

**Bastionhost:** Der Bastionhost ist der einzelne Zugangspunkt, über den Benutzer Zugriff auf die bereitgestellten Ressourcen in dieser Umgebung erhalten. Der Bastionhost bietet eine sichere Verbindung mit den bereitgestellten Ressourcen, indem nur Remotedatenverkehr von öffentlichen IP-Adressen auf einer Sicherheitsliste zugelassen wird. Damit RDP-Datenverkehr (Remotedesktopprotokoll) zugelassen wird, muss die Quelle des Datenverkehrs in der NSG (Netzwerksicherheitsgruppe) definiert sein.

Diese Lösung erstellt einen virtuellen Computer als Bastionhost in der Domäne mit den folgenden Konfigurationen:
-   [Antischadsoftware-Erweiterung](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure-Diagnoseerweiterung](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) mit Azure Key Vault
-   Eine [Richtlinie zum automatischen Herunterfahren](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/), um die Nutzung der Ressourcen virtueller Computer zu verringern, wenn sie nicht benötigt werden
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ist aktiviert, damit Anmeldeinformationen und andere Geheimnisse in einer geschützten Umgebung, die vom ausgeführten Betriebssystem isoliert ist, ausgeführt werden.

### <a name="virtual-network"></a>Virtuelles Netzwerk
Die Referenzarchitektur definiert ein privates VNet mit dem Adressraum 10.0.0.0/16.

**Netzwerksicherheitsgruppen:** [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) enthalten Zugriffssteuerungslisten, die den Datenverkehr in einem VNet zulassen oder ablehnen. NSGs können dafür verwendet werden, den Datenverkehr auf der Ebene eines Subnetzes oder einzelner VMs zu schützen. Die folgenden NSGs sind vorhanden:
  - eine NSG für die Datenschicht (SQL Server-Cluster, SQL Server-Zeuge und SQL-Lastenausgleich)
  - eine NSG für den verwaltenden Bastionhost
  - eine NSG für Active Directory
  - eine NSG für SQL Server Reporting Services

Jede der NSGs verfügt über bestimmte offene Ports und Protokolle, damit die Lösung sicher und richtig ausgeführt werden kann. Darüber hinaus werden die folgenden Konfigurationen für jede NSG aktiviert:
  - [Diagnoseprotokolle und -ereignisse](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) werden aktiviert und in einem Speicherkonto gespeichert.
  - OMS Log Analytics ist mit der [Diagnose der NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) verbunden.

**Subnetze:** Jedes Subnetz ist seiner entsprechenden NSG zugeordnet.

### <a name="data-at-rest"></a>Ruhende Daten
Die Architektur schützt ruhende Daten durch verschiedene Maßnahmen wie Verschlüsselung und Datenbanküberwachung.

**Azure Storage:** Um die Anforderungen an verschlüsselte ruhende Daten zu erfüllen, wird für den gesamten [Azure Storage](https://azure.microsoft.com/services/storage/) die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/storage-service-encryption) verwendet. Dadurch werden personenbezogene Daten geschützt, um die Zusagen und Anforderungen der Organisation im Hinblick auf Sicherheit und Compliance gemäß der DSGVO einzuhalten.

**Azure Disk Encryption:** [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) nutzt das BitLocker-Feature von Windows, um Volumeverschlüsselung für Betriebssystem- und normale Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträgerverschlüsselungsschlüssel zu erleichtern.

**Azure SQL-Datenbank:** In der Azure SQL-Datenbankinstanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:
-   Die [AD-Authentifizierung und -Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten.
-   Die [SQL-Datenbank-Überprüfung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in einem Azure Storage-Konto.
-   Die SQL-Datenbank ist so konfiguriert, dass sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) für die Echtzeitverschlüsselung und -entschlüsselung der Datenbank, der zugehörigen Sicherungen und der Transaktionsprotokolldateien verwendet, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte personenbezogenen Daten keinen unbefugten Zugriffen ausgesetzt sind.
-   [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) verhindern sämtlichen Zugriff auf Datenbankserver, bis die richtigen Berechtigungen erteilt werden. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
-   Die [SQL-Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) ermöglicht die Erkennung und Reaktion auf potenzielle Bedrohungen, sobald sie auftreten. Dazu werden Sicherheitswarnungen bei verdächtigen Datenbankaktivitäten, potenziellen Sicherheitslücken, Angriffen durch Einschleusung von SQL-Befehlen und anormalen Mustern beim Datenbankzugriff ausgegeben.
-   [Always Encrypted-Spalten](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) stellen sicher, dass vertrauliche personenbezogene Daten im Datenbanksystem niemals im Klartextformat angezeigt werden. Nach dem Aktivieren der Datenverschlüsselung können nur Clientanwendungen oder App-Server, die Zugriff auf die Schlüssel haben, auf Klartextdaten zugreifen.
- Das Feature [Erweiterte Eigenschaften](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kann zum Beenden der Verarbeitung von Datenbesitzern verwendet werden, da es Benutzern ermöglicht, Datenbankobjekten benutzerdefinierte Eigenschaften hinzufügen und Daten als „Eingestellt“ zu markieren und damit Anwendungslogik zu unterstützen, die das Verarbeiten der zugehörigen personenbezogenen Daten verhindert.
- Die [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ermöglicht Benutzern das Festlegen von Richtlinien zum Einschränken des Zugriffs auf Daten, um die Verarbeitung zu beenden.
- Die [dynamische Datenmaskierung in SQL-Datenbank (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung vertraulicher personenbezogener Daten ein, indem diese Daten für nicht berechtigte Benutzer maskiert werden. DDM kann automatisch potenziell vertrauliche Daten ermitteln und eine geeignete Maskierung vorschlagen. Dies hilft bei der Identifikation der personenbezogenen Daten, für die der Schutz durch die DSGVO gilt, und bei der Reduzierung des Zugriffs, damit sie die Datenbank nicht per nicht autorisiertem Zugriff verlassen. **Hinweis: Kunden müssen die DDM-Einstellungen an ihr Datenbankschema anpassen.**

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf personenbezogene Daten in der Azure-Umgebung:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in AAD erstellt. Dies gilt auch für Benutzer, die auf SQL-Datenbank zugreifen.
-   Die Authentifizierung für die Anwendung erfolgt mithilfe von AAD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Darüber hinaus wird für Verschlüsselung der Datenbankspalten ebenfalls AAD verwendet, um die Anwendung in Azure SQL-Datenbank zu authentifizieren. Weitere Informationen finden Sie unter [Schützen von vertraulichen Daten in SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Die [rollenbasierte Zugriffssteuerung in Azure (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer zum Erledigen ihrer Arbeit benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu geben, können Administratoren nur bestimmte Aktionen für den Zugriff auf personenbezogene Daten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) können Kunden die Anzahl von Benutzern mit Zugriff auf bestimmte Informationen, z.B. personenbezogene Daten, einschränken.  Administratoren können mit AAD Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.

### <a name="security"></a>Sicherheit
**Geheimnisverwaltung:** Die Lösung verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen personenbezogener Daten und des Zugriffs auf diese Daten:
- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule (HSMs) geschützt. Der Schlüsseltyp ist ein HSM-geschützter 2.048-Bit-RSA-Schlüssel.
- Allen Benutzern und Identitäten werden mit RBAC die mindestens erforderlichen Berechtigungen erteilt.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

**Patchverwaltung:** Virtuelle Windows-Computer, die mit dieser Referenzarchitektur bereitgestellt werden, sind standardmäßig so konfiguriert, dass sie automatische Updates vom Windows Update-Dienst erhalten. Diese Lösung umfasst auch den OMS-Dienst [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), über den aktualisierte Bereitstellungen erstellt werden können, um virtuelle Computer bei Bedarf zu patchen.

**Schutz vor Schadsoftware:** [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) für Virtual Machines bietet Echtzeitschutzfunktionen, mit denen die Identifizierung und Entfernung von Viren, Spyware und anderer bösartiger Software ermöglicht wird, mit konfigurierbaren Warnmeldungen, wenn bekannte bösartige oder unerwünschte Software versucht, sich auf geschützten virtuellen Computern zu installieren oder die Ausführung zu starten.

**Sicherheitswarnungen:** [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) ermöglicht Kunden die Überwachung von Datenverkehr, das Erfassen von Protokollen und das Analysieren von Datenquellen auf Bedrohungen. Darüber hinaus greift Azure Security Center auf die vorhandene Konfiguration der Azure-Dienste zu, um Empfehlungen für die Konfiguration und Verwaltung zu geben und zur Verbesserung der Sicherheit und zum Schutz personenbezogener Daten beizutragen. Azure Security Center umfasst einen [Bericht über Bedrohungen](https://docs.microsoft.com/azure/security-center/security-center-threat-report) zu jeder erkannten Bedrohung, der den Teams, die für die Bearbeitung von Incidents zuständig sind, beim Untersuchen und Beheben von Bedrohungen als Hilfe dient.

### <a name="business-continuity"></a>Geschäftskontinuität
**Hochverfügbarkeit:** Serverworkloads werden in einer [Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zusammengefasst, um Hochverfügbarkeit für virtuelle Computer in Azure sicherzustellen. Während einer geplanten oder ungeplanten Wartung ist mindestens ein virtueller Computer verfügbar, um die Azure-SLA von 99,95 % zu erfüllen.

**Recovery Services-Tresor:** Der [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) speichert Sicherungsdaten und schützt alle Konfigurationen von Azure Virtual Machines in dieser Architektur. Durch einen Recovery Services-Tresor können Kunden Dateien und Ordner von einer IaaS-VM wiederherstellen, ohne die gesamte VM wiederherstellen zu müssen. So werden die Wiederherstellungszeiten verkürzt.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung
Die [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) bietet umfassende Protokollierung von System- und Benutzeraktivitäten sowie der Dateisystemintegrität. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist eine OMS-Lösung für die Erfassung und Analyse von Daten, die von Ressourcen in Azure- und lokalen Umgebungen generiert werden.
- **Aktivitätsprotokolle:** [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in einem Abonnement ausgeführt wurden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
- **Diagnoseprotokolle:** [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) sind alle Protokolle, die von den einzelnen Ressourcen ausgegeben werden. Diese Protokolle umfassen Windows-Ereignissystemprotokolle, Azure Blob Storage-, Tabellen- und Warteschlangenprotokolle.
- **Protokollarchivierung:** Alle Diagnoseprotokolle werden für die Archivierung in ein zentrales und verschlüsseltes Azure Storage-Konto geschrieben. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden. Diese Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige mit Azure Log Analytics verbunden.

Darüber hinaus sind in dieser Architektur die folgenden OMS-Lösungen enthalten:
-   [Active Directory-Bewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment) Die Active Directory-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Antischadsoftwarebewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware) Die Antischadsoftwarelösung meldet Schadsoftware, Bedrohungen und den Schutzstatus.
-   [Azure Automation:](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) Die Azure Automation-Lösung dient der Speicherung, Ausführung und Verwaltung von Runbooks.
-   [Sicherheit und Überwachung:](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) Das Sicherheits- und Überwachungsdashboard bietet einen allgemeinen Einblick in den Sicherheitszustand von Ressourcen. Hierzu werden Metriken zu Sicherheitsdomänen, relevanten Problemen, Erkennungen, Bedrohungsdaten und allgemeinen Sicherheitsabfragen bereitgestellt.
-   [SQL-Bewertung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment) Die SQL-Lösung zur Integritätsüberprüfung bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt für die Kunden eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
-   [Updateverwaltung:](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) Die Updateverwaltungslösung ermöglicht die Verwaltung von Betriebssystem-Sicherheitsupdates durch die Kunden. Dies umfasst auch den Status der verfügbaren Updates und den Fortschritt beim Installieren der erforderlichen Updates.
-   [Agent-Integritätsdiagnose:](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth) Die Agent-Integritätsdiagnoselösung meldet, wie viele Agents bereitgestellt werden und wie ihre geografische Verteilung ist. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
-   [Azure-Aktivitätsprotokolle:](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity) Die Aktivitätsprotokollanalyse-Lösung hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.
-   [Änderungsnachverfolgung:](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity) Die Änderungsnachverfolgungslösung ermöglicht Kunden das Identifizieren von Änderungen in der Umgebung auf einfache Weise.

## <a name="threat-model"></a>Bedrohungsmodell

Das Datenflussdiagramm für diese Referenzarchitektur steht zum [Download](https://aka.ms/gdprDWdfd) bereit und ist unten angegeben. Dieses Modell kann Kunden dabei helfen, bei Änderungen die potenziellen Risikopunkte in der Systeminfrastruktur zu verstehen.

![Bedrohungsmodell für Data Warehouse gemäß DSGVO](images/gdpr-datawarehouse-threat-model.png?raw=true "Bedrohungsmodell für Data Warehouse gemäß DSGVO")

## <a name="compliance-documentation"></a>Konformitätsdokumentation
In der Vorlage für die [Azure Security and Compliance – DSGVO-Zuständigkeitsmatrix für Kunden](https://aka.ms/gdprCRM) sind die Aufgaben von Controllern und Verarbeitern für alle DSGVO-Artikel aufgeführt. Beachten Sie, dass der Kunde bei Azure-Diensten in der Regel der Controller und Microsoft der Verarbeiter ist.

Die [Azure Security and Compliance Blueprint – Implementierungsmatrix für Data Warehouse gemäß DSGVO](https://aka.ms/gdprDW) enthält Informationen darüber, auf welche DSGVO-Artikel durch die Data Warehouse-Architektur verwiesen wird, sowie eine detaillierte Beschreibung, wie die Implementierung die Anforderungen der einzelnen Artikel erfüllt.

## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen
### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
Für sichere Verbindungen mit Ressourcen, die als Teil dieser Data Warehouse-Referenzarchitektur bereitgestellt werden, muss ein sicherer VPN-Tunnel oder [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) konfiguriert werden. Durch die ordnungsgemäße Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen in einem „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Der [IPsec-Tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wird bei dieser Option als Verschlüsselungsmechanismus verwendet.

Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>ETL-Prozess (Extrahieren, Transformieren, Laden)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kann Daten in Azure SQL Data Warehouse laden, ohne ein separates ETL- oder Importtool zu verwenden. PolyBase ermöglicht den Zugriff auf Daten über T-SQL-Abfragen. Microsoft Business Intelligence und der Analysestapel sowie mit SQL Server kompatible Drittanbietertools können mit PolyBase verwendet werden.

### <a name="azure-active-directory-setup"></a>Azure Active Directory-Einrichtungen
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ist wichtig für die Verwaltung der Bereitstellung von Zugriff für Mitarbeiter, die mit der Umgebung interagieren. Ein bereits vorhandenes Windows Server Active Directory-Verzeichnis kann über [vier Klicks](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express) in Azure Active Directory integriert werden. Die Kunden können außerdem die bereitgestellte Active Directory-Infrastruktur (Domänencontroller) an ein bereits vorhandenes Azure Active Directory binden, indem sie die bereitgestellte Active Directory-Infrastruktur als untergeordnete Domäne für eine AAD-Struktur festlegen.

### <a name="optional-services"></a>Optionale Dienste
Azure bietet eine Vielzahl von Diensten zur Unterstützung bei Speicherung und Staging formatierter und unformatierter Daten. Die folgenden Dienste können dieser Referenzarchitektur je nach den Kundenanforderungen hinzugefügt werden:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) ist ein spezieller verwalteter Clouddienst für komplexe Hybridprojekte mit ETL (Extrahieren, Transformieren und Laden), ELT (Extrahieren, Laden und Transformieren) und Datenintegration. Azure Data Factory bietet Funktionen für die Verfolgung und Suche personenbezogener Daten, einschließlich Visualisierungs- und Überwachungstools, mit denen ermittelt werden kann, wann Daten empfangen wurden und woher diese stammen. Mit Azure Data Factory können Kunden datengesteuerte Workflows (sogenannte Pipelines) erstellen und planen, die Daten aus unterschiedlichen Datenspeichern erfassen. Diese Pipelines ermöglichen Kunden Daten das Erfassen von Daten aus internen und externen Quellen. Kunden können die Daten dann verarbeiten und für die Ausgabe in Datenspeicher wie z.B. Azure SQL Data Warehouse transformieren.
- Kunden können ein Staging für unstrukturierte Daten in [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) durchführen. Dies bietet ihnen die Möglichkeit, Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort zu erfassen.  Azure Data Lake verfügt über Funktionen für die Extraktion und Konvertierung von Daten. Azure Data Lake Store ist mit den meisten Open-Source-Komponenten des Hadoop-Ökosystems kompatibel und kann sehr einfach in andere Azure-Dienste wie Azure SQL Data Warehouse integriert werden.

## <a name="disclaimer"></a>Haftungsausschluss

 - Dieses Dokument dient nur zu Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHEN, STILLSCHWEIGENDEN ODER GESETZLICH VORGEGEBENEN GARANTIEN HINSICHTLICH DER INFORMATIONEN IN DIESEM DOKUMENT. Dieses Dokument wird so bereitgestellt, wie es ist. Informationen und Stellungnahmen in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Kunden, die dieses Dokument lesen, tragen das Risiko, es zu verwenden.
 - Dieses Dokument gibt Kunden keine Rechte an geistigem Eigentum an Microsoft-Produkten oder -Lösungen.
 - Kunden dürfen dieses Dokument zu internen Referenzzwecken kopieren und verwenden.
 - Bestimmte Empfehlungen in diesem Dokument können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen in Azure sowie zu höheren Azure-Lizenz- oder -Abonnementkosten für den Kunden führen.
 - Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.
 - Dieses Dokument wurde als Referenz entwickelt und sollte nicht dazu verwendet werden, alle Mittel zu definieren, mit denen ein Kunde bestimmte Konformitätsanforderungen und -vorschriften erfüllen kann. Kunden sollten bei genehmigten Kundenimplementierungen rechtliche Unterstützung von ihrer Organisation einholen.

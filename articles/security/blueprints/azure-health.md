---
title: 'Azure Health Analytics: Entwurf'
description: Leitfaden zum Bereitstellen eines HIPAA/HITRUST Health Analytics-Entwurfs
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 26566e0a-0a54-49f4-a91d-48e20b7cef71
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2018
ms.author: simorjay
ms.openlocfilehash: bdd7dbf4f39529ac76fb496f0d459577e6f929dc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="azure-security-and-compliance-blueprint---hipaahitrust-health-data-and-ai"></a>Entwurf zu Azure-Sicherheit und -Kompatibilität: HIPAA/HITRUST-Gesundheitsdaten und -KI

## <a name="overview"></a>Übersicht

**Der Entwurf zu Azure-Sicherheit und -Kompatibilität: HIPAA/HITRUST-Gesundheitsdaten und -KI bietet eine schlüsselfertige Bereitstellung einer Azure PaaS-Lösung, um zu zeigen, wie Gesundheitsdaten sicher erfasst, gespeichert und analysiert werden können und wie mit ihnen interagiert werden kann, während es gleichzeitig möglich ist, die Konformitätsanforderungen der Branche zu erfüllen. Der Entwurf hilft, den Umstieg auf die Cloud und ihre Nutzung für Kunden mit Daten zu beschleunigen, die Regulierungen unterliegen.**

Der Entwurf zu Azure-Sicherheit und -Kompatibilität: HIPAA/HITRUST-Gesundheitsdaten und -KI stellt Tools und Anleitungen zur Verfügung, die Sie beim Bereitstellen einer sicheren, für Health Insurance Portability and Accountability Act (HIPAA) und Health Information Trust Alliance (HITRUST) bereiten PaaS-Umgebung (Platform-as-a-Service) für die Erfassung, Speicherung, Analyse von und Interaktion mit persönlichen und nicht-persönlichen medizinischen Daten in einer sicheren, mehrschichtigen Cloudumgebung unterstützen, die als End-to-End-Lösung bereitgestellt wird. Der Plan präsentiert eine allgemeine Referenzarchitektur und soll dazu dienen, die Einführung von Microsoft Azure zu vereinfachen. Diese bereitgestellte Architektur veranschaulicht eine Lösung, die die Anforderungen von Organisationen erfüllt, die nach einem cloudbasierten Ansatz suchen, um die Belastungen und Kosten einer Bereitstellung zu verringern.

![](images/components.png)

Die Lösung wurde dafür konzipiert, einen Beispieldatensatz, der mit Fast Healthcare Interoperability Resources (FHIR) – einem weltweiten Standard für den elektronischen Austausch von Gesundheitsinformationen – formatiert wurde, zu nutzen und sicher zu speichern. Kunden können dann mit Azure Machine Learning die Vorteile von leistungsstarken Business Intelligence-Tools und Analysen nutzen, um Vorhersagen zu überprüfen, die für die Beispieldaten getroffen wurden. Als Beispiel für die Art von Experiment, das Azure Machine Learning ermöglichen kann, enthält der Entwurf einen Beispieldatensatz, Skripts und Tools zur Vorhersage der Verweildauer eines Patienten in einer Krankenhauseinrichtung. 

Mit diesem Entwurf soll eine modulare Grundlage geschaffen werden, auf der sich die Kunden auf ihre spezifischen Anforderungen einstellen können, indem sie neue Azure Machine Learning-Experimente entwickeln, um sowohl klinische als auch betriebliche Anwendungsfallszenarien zu lösen. Er ist so konzipiert, dass er bei der Bereitstellung sicher und konform ist. Der Kunde ist jedoch für die richtige Konfiguration der Rollen und die Implementierung von Änderungen verantwortlich. Beachten Sie Folgendes:

-   Dieser Entwurf liefert eine Ausgangsbasis, um Kunden bei der Verwendung von Microsoft Azure in einer HITRUST- und HIPAA-Umgebung zu unterstützen.

-   Obwohl der Entwurf so konzipiert wurde, dass er mit HIPAA und HITRUST (durch das Common Security Framework, CSF) in Einklang gebracht werden kann, sollte er erst dann als konform angesehen werden, wenn er von einem externen Prüfer gemäß den HIPAA- und HITRUST-Zertifizierungsanforderungen zertifiziert wurde.

-   Kunden sind dafür verantwortlich, angemessene Sicherheits- und Konformitätsprüfungen für jede Lösung durchzuführen, die mit dieser grundlegenden Architektur erstellt wurde.

## <a name="deploying-the-automation"></a>Bereitstellen der Automatisierung

- Befolgen Sie die Anweisungen im Installationsleitfaden, um die Lösung bereitzustellen. 

[![](./images/deploy.png)](https://aka.ms/healthblueprintdeploy)

Eine schnelle Übersicht über die Funktionsweise dieser Lösung finden Sie in diesem [Video](https://aka.ms/healthblueprintvideo), in dem die Bereitstellung erklärt und gezeigt wird.

- Häufig gestellte Fragen (FAQ) finden Sie in der Anleitung [FAQ](https://aka.ms/healthblueprintfaq).

-   **Architekturdiagramm.** Das Diagramm zeigt die für den Entwurf verwendete Referenzarchitektur und das Anwendungsfall-Beispielszenario.

-   **Bereitstellungsvorlagen**. In dieser Bereitstellung werden [Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) verwendet, um die Komponenten der Architektur automatisch in Microsoft Azure bereitzustellen, indem während des Setups Konfigurationsparameter angegeben werden.

-   **[Automatisierte Bereitstellungsskripts](https://aka.ms/healthblueprintdeploy)**. Diese Skripts unterstützen die Bereitstellung der Lösung. Zu den Skripts gehören:


-   Ein Skript zur Modulinstallation und zur Einrichtung für [globale Administratoren](/azure/active-directory/active-directory-assign-admin-roles-azure-portal). Mit diesem Skript werden die erforderlichen PowerShell-Module und globalen Administratorrollen installiert und wird geprüft, ob diese ordnungsgemäß konfiguriert sind. 
-   Für die Bereitstellung der Lösung wird ein PowerShell-Installationsskript verwendet, das über eine ZIP-Datei zur Verfügung gestellt wird, die vordefinierte Demofunktionen enthält.

## <a name="solution-components"></a>Lösungskomponenten


Die grundlegende Architektur besteht aus den folgenden Komponenten:

-   **[Bedrohungsmodell](https://aka.ms/healththreatmodel)** Ein umfassendes Bedrohungsmodell wird im tm7-Format für die Verwendung mit dem [Microsoft Threat Modeling Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49168) bereitgestellt, das die Komponenten der Lösung, die Datenflüsse zwischen ihnen und die Vertrauensgrenzen zeigt. Das Modell kann Kunden dabei helfen, die potenziellen Risikopunkte in der Systeminfrastruktur bei der Entwicklung von Machine Learning-Komponenten oder anderen Änderungen zu verstehen.

-   **[Kundenimplementierungsmatrix](https://aka.ms/healthcrmblueprint)** Eine Microsoft Excel-Arbeitsmappe listet die relevanten HITRUST-Anforderungen auf und erläutert, wie Microsoft und der Kunde für die Erfüllung der jeweiligen Anforderungen verantwortlich sind.

-   **[Überprüfung der Integrität.](https://aka.ms/healthreviewpaper)** Die Lösung wurde von Coalfire Systems, Inc. geprüft. Die Konformitätsüberprüfung für das Gesundheitswesen (HIPAA und HITRUST) und Anleitung\' zur Implementierung enthält eine vom Prüfer verfasste Überprüfung der Lösung sowie Überlegungen für das Transformieren des Entwurfs in eine produktionsreife Bereitstellung.

# <a name="architectural-diagram"></a>Architekturdiagramm


![](images/refarch.png)

## <a name="roles"></a>Rollen


Der Entwurf definiert zwei Rollen für administrative Benutzer (Operatoren) und drei Rollen für Benutzer im Krankenhausmanagement und in der Patientenversorgung. Eine sechste Rolle wird für einen Prüfer definiert, um die Einhaltung von HIPAA und anderen Vorschriften zu bewerten. Rollenbasierte Zugriffssteuerung in Azure (Role-based Access Control, RBAC) ermöglicht ein zielgerichtetes Zugriffsmanagement für jeden Benutzer der Lösung durch integrierte und benutzerdefinierte Rollen. Ausführliche Informationen zu RBAC, Rollen und Berechtigungen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/overview) und [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

### <a name="site-administrator"></a>Siteadministrator


Der Siteadministrator ist für das Azure-Abonnement des Kunden verantwortlich. Er steuert die gesamte Bereitstellung, besitzt aber keinen Zugriff auf die Patientenakten.

-   Standardrollenzuweisungen: [Besitzer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)

-   Benutzerdefinierte Rollenzuweisungen: k.A.

-   Bereich: Abonnement

### <a name="database-analyst"></a>Datenbankanalyst

Der Datenbankanalyst verwaltet die SQL Server-Instanz und die Datenbank.
Er besitzt keinen Zugriff auf die Patientenakten.

-   Integrierten Rollenzuweisungen: [SQL-DB-Mitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-db-contributor), [SQL Server-Mitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-server-contributor)

-   Benutzerdefinierte Rollenzuweisungen: k.A.

-   Bereich: ResourceGroup

 ### <a name="data-scientist"></a>Data Scientist


Der Data Scientist betreibt den Azure Machine Learning-Dienst. Er kann Daten importieren, exportieren, verwalten und Berichte ausführen. Der Data Scientist besitzt Zugriff auf Patientendaten, verfügt aber nicht über Administratorrechte.

-   Integrierten Rollenzuweisungen: [Speicherkontomitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor)

-   Benutzerdefinierte Rollenzuweisungen: k.A.

-   Bereich: ResourceGroup

### <a name="chief-medical-information-officer-cmio"></a>Chief Medical Information Officer (CMIO)


Der CMIO ist das Bindeglied zwischen den Informatikern und Technikern einerseits und dem Gesundheitspersonal in einer Gesundheitseinrichtung andererseits. Zu seinen Pflichten gehört in der Regel die Verwendung von Analysesoftware, um festzustellen, ob die Ressourcen in der Organisation angemessen zugeordnet wurden.

-   Integrierten Rollenzuweisungen: Keine

### <a name="care-line-manager"></a>Pflegedienstleitung


Die Pflegedienstleitung ist direkt mit der Betreuung der Patienten befasst.
In dieser Rolle ist sie für die Überwachung des Zustands einzelner Patienten zuständig und trägt außerdem dafür Sorge, dass genügend Mitarbeiter verfügbar sind, um die speziellen Pflegebedürfnisse der Patienten zu erfüllen. Die Pflegedienstleitung ist für das Ergänzen und Aktualisieren der Patientenakten verantwortlich.

-   Integrierten Rollenzuweisungen: Keine

-   Benutzerdefinierte Rollenzuweisungen: Ist berechtigt, HealthcareDemo.ps1 auszuführen, um sowohl die Patientenaufnahme als auch die Entlassung durchzuführen.

-   Bereich: ResourceGroup

### <a name="auditor"></a>Prüfer


Der Prüfer wertet die Lösung bezüglich der Konformität aus. Er besitzt keinen direkten Zugriff auf das Netzwerk.

-   Integrierten Rollenzuweisungen: [Leser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader)

-   Benutzerdefinierte Rollenzuweisungen: k.A.

-   Bereich: Abonnement

## <a name="example-use-case"></a>Beispiel eines Anwendungsfalls


Das in diesem Entwurf enthaltene Anwendungsfallbeispiel veranschaulicht, wie der Entwurf verwendet werden kann, um Machine Learning und Analysen von Gesundheitsdaten in der Cloud zu ermöglichen. Contosoclinic ist ein kleines Krankenhaus in den USA. Die Administratoren des Krankenhausnetzwerks möchten mit Azure Machine Learning die Verweildauer eines Patienten zum Zeitpunkt der Aufnahme besser vorhersagen, um die Effizienz der betrieblichen Arbeitsauslastung zu erhöhen und die Qualität der Versorgung zu verbessern.

### <a name="predicting-length-of-stay"></a>Prognostizieren der Aufenthaltsdauer


Das Anwendungsfall-Beispielszenario verwendet Azure Machine Learning, um die Verweildauer eines neu aufgenommenen Patienten vorherzusagen, indem die medizinischen Details bei der Aufnahme mit den aggregierten historischen Daten früherer Patienten verglichen werden.
Der Entwurf enthält eine große Anzahl anonymisierter medizinischer Datensätze, um die Trainings- und Prognosefähigkeiten der Lösung zu zeigen. In einer Produktionsbereitstellung würden Kunden ihre eigenen Datensätze verwenden, um die Lösung für genauere Vorhersagen zu trainieren, die die einzigartigen Details ihrer Umgebung, Einrichtungen und Patienten widerspiegeln.

### <a name="users-and-roles"></a>Benutzer und Rollen


**Siteadministrator: Alex**

*E-Mail-Adresse: Alex\_SiteAdmin*

Alex Aufgabe besteht darin, Technologien auszuwerten, die den Aufwand für die Verwaltung eines lokalen Netzwerks verringern und die Kosten für das Management senken können. Alex evaluiert Azure schon seit einiger Zeit, hat sich aber schwer getan, die Dienste zu konfigurieren, die er benötigt, um die HiTrust-Konformitätsanforderungen zum Speichern von Patientendaten in der Cloud zu erfüllen. Alex hat sich für die Azure Health-KI entschieden, um eine konformitätsbereite Gesundheitslösung bereitzustellen, die die Anforderungen berücksichtigt, um die Kundenanforderungen für HiTrust zu erfüllen.

**Data Scientist: Debra**

*E-Mail-Adresse: Debra\_DataScientist*

Debra ist verantwortlich für die Verwendung und Erstellung von Modellen, die medizinische Daten analysieren, um Einblicke in die Patientenversorgung zu gewähren. Debra benutzt SQL und die statistische Programmiersprache R, um ihre Modelle zu erstellen.

**Datenbankanalyst: Danny**

*E-Mail-Adresse: Danny\_DBAnalyst*

Danny ist der Hauptansprechpartner für alles rund um den Microsoft SQL Server-Computer, der alle Patientendaten für Contosoclinic speichert. Danny ist ein erfahrener SQL Server-Administrator, der sich erst kürzlich mit der Azure SQL-Datenbank vertraut gemacht hat.

**Chief Medical Information Officer: Caroline**

Caroline arbeitet mit Chris (Pflegedienstleitung) und Debra (Data Scientist) zusammen, um herauszufinden, welche Faktoren die Verweildauer der Patienten beeinflussen.
Caroline verwendet die Vorhersagen der LOS-Lösung (Lenght-of-Stay, Verweildauer), um festzustellen, ob die Ressourcen im Krankenhausnetzwerk angemessen verteilt werden. Beispielsweise mithilfe des Dashboards, das in dieser Lösung bereitgestellt wird.

**Pflegedienstleitung: Chris**

*E-Mail-Adresse: Chris\_CareLineManager*

Als die Person, die direkt für die Patientenaufnahme und die Entlassungen in der Contosoclinic verantwortlich ist, nutzt Chris die Vorhersagen der LOS-Lösung, um sicherzustellen, dass ausreichend Personal zur Verfügung steht, um die Patienten während ihres Aufenthalts in der Einrichtung zu betreuen.

**Prüfer: Han**

*E-Mail-Adresse: Han\_Auditor*

Han ist ein zertifizierte Prüfer, der über Erfahrung mit ISO-, SOC- und HiTrust-Zertifizierungen verfügt. Han wurde eingestellt, um das Netzwerk der Contosoclinic zu überprüfen. Han kann die mit der Lösung bereitgestellte Kundenzuständigkeitsmatrix überprüfen, um sicherzustellen, dass der Entwurf und die LOS-Lösung zur Speicherung, Verarbeitung und Anzeige sensibler persönlicher Daten verwendet werden können.


# <a name="design-configuration"></a>Entwurfskonfiguration


In diesem Abschnitt werden die Standardkonfigurationen und Sicherheitsmaßnahmen beschrieben, die in dem skizzierten Entwurf enthalten sind:

- **ERFASSEN** von Rohdatenquellen einschließlich der FHIR-Datenquelle
- **SPEICHERN** vertraulicher Informationen
- **ANALYSIEREN** und Vorhersagen von Ergebnissen
- **INTERAGIEREN** mit den Ergebnissen und Vorhersagen
- **IDENTITÄTSVERWALTUNG** der Lösung
- **SICHERHEITSAKTIVIERTE** Features


## <a name="identity"></a>IDENTITÄT 

### <a name="azure-active-directory-and-role-based-access-control-rbac"></a>Azure Active Directory und rollenbasierte Zugriffssteuerung (RBAC)


**Authentifizierung:**

-   [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft\'. Alle Benutzer für die Lösung wurden in Azure Active Directory erstellt, so auch die Benutzer, die auf die SQL-Datenbank zugreifen.



-   Authentifizierung für die Anwendung erfolgt mithilfe von Azure AD. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

-   [Azure Active Directory Identity Protection](/azure/active-directory/active-directory-identityprotection) erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten Ihrer Organisation auswirken, konfiguriert automatisierte Antworten auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten Ihrer Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.

-   Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure](/azure/role-based-access-control/role-assignments-portal) ermöglicht eine präzise Zugriffsverwaltung für Azure. Abonnementzugriff besitzt ausschließlich der Abonnementadministrator, und Azure Key Vault-Zugriff ist auf den Siteadministrator beschränkt. Sichere Kennwörter (mindestens 12 Zeichen mit mindestens einem Groß-/Kleinbuchstaben, einer Zahl und einem Sonderzeichen) sind erforderlich.

-   Die mehrstufige Authentifizierung wird unterstützt, wenn der Schalter -enableMFA während der Bereitstellung aktiviert ist.

-   Kennwörter laufen nach 60 Tagen ab, wenn der Schalter -enableADDomainPasswordPasswordPolicy während der Bereitstellung aktiviert ist.

**Rollen:**

-   Die Lösung verwendet [integrierte Rollen](/azure/role-based-access-control/built-in-roles), um den Zugriff auf Ressourcen zu verwalten.

-   Allen Benutzern werden standardmäßig bestimmte integrierte Rollen zugewiesen.

### <a name="azure-key-vault"></a>Azure Key Vault

-   Folgende Daten werden in Key Vault gespeichert:

    -   Application Insights-Schlüssel
    -   Patientendaten-Speicherzugriffsschlüssel
    -   Patienten-Verbindungszeichenfolge
    -   Patientendaten-Tabellenname
    -   Azure ML-Webdienstendpunkt
    -   Azure ML-API-Dienstschlüssel

-   Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
-   Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
-   Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
-   Alle Schlüssel in Key Vault sind durch HSM \[Key Type = HSM Protected 2048-bit RSA Key\] geschützt.
-   Allen Benutzern/Identitäten werden mithilfe von rollenbasierter Zugriffssteuerung (Role Based Access Control, RBAC) die erforderlichen mindestens erforderlichen Berechtigungen erteilt.
-   Anwendungen teilen sich nur dann einen Key Vault, wenn sie sich gegenseitig vertrauen und zur Laufzeit Zugriff auf dieselben Geheimnisse benötigen.
-   Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
-   Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

## <a name="ingest"></a>ERFASSEN 

### <a name="azure-functions"></a>Azure-Funktionen
Die Lösung wurde so konzipiert, dass sie [Azure Functions](/azure/azure-functions/) verwendet, um die in der Analysedemo verwendeten Verweildauerdaten zu verarbeiten. Es wurden drei Funktionalitäten in den Funktionen erstellt.

**1. Massenimport von PHI-Patientendaten**

Beim Ausführen des Demoskripts: . \\HealthcareDemo.ps1 mit dem Schalter **BulkPatientAdmission** (wie unter **Bereitstellen und Ausführen der Demo** beschrieben) führt die folgende Verarbeitungspipeline aus:
1. **Azure Blob Storage**: CSV-Beispieldatei mit Patientendaten wird in den Speicher hochgeladen.
2. **Event Grid**: Ereignis veröffentlicht Daten für Azure-Funktion (Massenimport, Blobereignis).
3. **Azure Functions**: Führt die Verarbeitung aus und speichert die Daten im SQL-Speicher mit der sicheren Funktion event(type; blob url).
4. **SQL-Datenbank**: Der Datenbankspeicher für Patientendaten mit Tags für die Klassifizierung. Der ML-Prozess wird für das Trainingsexperiment ausgelöst.

![](images/dataflow.png)

Außerdem wurde die Azure-Funktion so konzipiert, dass sie mithilfe der folgenden Tags bestimmte sensible Daten im Beispieldatensatz lesen und schützen kann:
- dataProfile => „ePHI“
- owner => \<Siteadmin-UPN\>
- environment => „Pilot“
- department => „Global Ecosystem“. Das Tagging wurde auf den Beispieldatensatz angewendet, wobei Patientennamen als Klartext identifiziert wurden.

**2. Aufnehmen neuer Patienten**

Beim Ausführen des Demoskripts: . \\HealthcareDemo.ps1 mit dem Schalter **BulkPatientadmission** (wie unter **Bereitstellen und Ausführen der Demo** beschrieben) führt die folgende Verarbeitungspipeline aus: ![](images/securetransact.png)
**1. Azure-Funktion** wird ausgelöst, und die Funktion fordert ein [Bearertoken](/rest/api/) aus Azure Active Directory an.

**2. Key Vault** wird für ein Geheimnis angefordert, das dem angeforderten Token zugeordnet ist.

\*\*3. Azure-Rollen überprüfen die Anforderung und autorisieren die Zugriffsanforderung auf den Key Vault.

**4. Key Vault** gibt das Geheimnis zurück, in diesem Fall die SQL-DB-Verbindungszeichenfolge.

**5. Azure Functions** verwendet die Verbindungszeichenfolge, um eine sichere Verbindungen mit der SQL-Datenbank herzustellen, und setzt die weitere Verarbeitung zum Speichern der ePHI-Daten fort.

Um die Speicherung der Daten zu erreichen, wurde ein allgemeines API-Schema nach Fast Healthcare Interoperability Resources (FHIR, ausgesprochen wie das englische Wort „Fire“) implementiert. Die Funktion wurde mit den folgenden FHIR-Austauschelementen bereitgestellt:

-   [Patientenschema](https://www.hl7.org/fhir/patient.html) umfasst die Information dazu, „wer“ ein Patient ist.

-   [Beobachtungsschema](https://www.hl7.org/fhir/observation.html) umfasst das zentrale Element im Gesundheitswesen, das zur Unterstützung der Diagnose, zur Überwachung des Fortschritts, zur Bestimmung von Basislinien und Mustern und sogar zur Erfassung demografischer Merkmale verwendet wird. 

-   [Begegnungsschema](https://www.hl7.org/fhir/encounter.html) umfasst die Arten von Begegnungen, z.B. ambulante Aufnahme, Notfall, häusliche Gesundheit, stationäre Aufnahme und virtuelle Begegnungen.

-   [Zustandsschema](https://www.hl7.org/fhir/condition.html) umfasst detaillierte Informationen zu einem Zustand, einem Problem, einer Diagnose oder einem anderen Ereignis, einer Situation, einem Problem oder einem klinischen Konzept mit Anlass zu Bedenken.  



### <a name="event-grid"></a>Event Grid


Die Lösung unterstützt Azure Event Grid, einen einzelnen Dienst für die Verwaltung des Routings aller Ereignisse aus jeder Quelle an jedes Ziel, der Folgendes bietet:

-   [Sicherheit und Authentifizierung](/azure/event-grid/security-authentication)

-   [Rollenbasierte Zugriffssteuerung](/azure/event-grid/security-authentication#management-access-control) für verschiedene Verwaltungsvorgänge, z.B. das Auflisten von Ereignisabonnements, das Erstellen neuer Abonnements und das Generieren von Schlüsseln.

-   Überwachung

## <a name="store"></a>SPEICHERN 

### <a name="sql-database-and-server"></a>SQL-Datenbank und Server 


-   [Transparent Data Encryption (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) bietet Echtzeitverschlüsselung und -entschlüsselung von Daten, die in der Azure SQL-Datenbank gespeichert sind, unter Verwendung eines Schlüssels, der in Azure Key Vault gespeichert ist.

-   [SQL-Sicherheitsrisikobewertung](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) ist ein einfach zu konfigurierendes Tool, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können.

-   [Bedrohungserkennung der SQL-Datenbank](/azure/sql-database/sql-database-threat-detection) aktiviert.

-   [SQL-Datenbanküberwachung](/azure/sql-database/sql-database-auditing) aktiviert.

-   [Protokollierung von Metriken der SQL-Datenbank und Diagnoseprotokollierung](/azure/sql-database/sql-database-metrics-diag-logging) aktiviert.

-   [Firewallregeln auf Server- und Datenbankebene](/azure/sql-database/sql-database-firewall-configure) wurden verschärft.

-   [Always Encrypted-Spalten](/azure/sql-database/sql-database-always-encrypted-azure-key-vault) werden verwendet, um den Vornamen, den zweiten Namen und den Nachnamen des Patienten zu schützen.
    Darüber hinaus wird für Verschlüsselung der Datenbankspalten ebenfalls Azure Active Directory (AD) verwendet, um die Anwendung in der Azure SQL-Datenbank zu authentifizieren.

-   Der Zugriff auf die SQL-Datenbank und den SQL Server-Computer wird nach dem Prinzip der geringsten Berechtigung konfiguriert.

-   Nur erforderlichen IP-Adressen ist der Zugriff durch die SQL-Firewall erlaubt.

### <a name="storage-accounts"></a>Speicherkonten


-   [Daten in Bewegung werden nur mit TLS/SSL übertragen](/azure/storage/common/storage-require-secure-transfer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json).

-   Anonymer Zugriff ist für Container unzulässig.

-   Warnungsregeln werden zum Nachverfolgen anonymer Aktivität konfiguriert.

-   HTTPS ist für den Zugriff auf Speicherkontoressourcen erforderlich.

-   Anforderungsdaten für die Authentifizierung werden protokolliert und überwacht.

-   Daten im Blobspeicher sind im Ruhezustand verschlüsselt.

## <a name="analyze"></a>ANALYSIEREN

### <a name="machine-learning"></a>Machine Learning


-   [Protokollierung ist aktiviert](/azure/machine-learning/studio/web-services-logging) für Machine Learning-Webdienste.
- Die Verwendung von [Machine Learning](/azure/machine-learning/desktop-workbench/experimentation-service-configuration) Workbench erfordert die Entwicklung von Experimenten, die die Fähigkeit zur Vorhersage eines Lösungssatzes bereitstellen. [Integration der Workbench](/azure/machine-learning/desktop-workbench/using-git-ml-project) kann dabei helfen, die Verwaltung von Experimenten zu optimieren.

## <a name="security"></a>SICHERHEIT

### <a name="azure-security-center"></a>Azure Security Center
- [Azure Security Center](https://azure.microsoft.com/services/security-center/) bietet eine zentrale Ansicht der Sicherheitsstatus sämtlicher Azure-Ressourcen. Sie können auf einen Blick überprüfen, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und schnell Ressourcen ermitteln, die Ihre Aufmerksamkeit erfordern. 

- Bei [Azure-Ratgeber](/azure/advisor/advisor-overview) handelt es sich um einen personalisierten Cloudberater, der Sie mit bewährten Methoden zum Optimieren von Azure-Bereitstellungen unterstützt. Das Tool analysiert die Konfiguration Ihrer Ressourcen und Telemetriedaten zur Nutzung und macht anschließend Vorschläge, wie Sie die Wirtschaftlichkeit, Leistung, Hochverfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

### <a name="application-insights"></a>Application Insights
- [Application Insights](/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um Ihre aktiven Webanwendung zu überwachen. Die Anwendung erkennt Leistungsanomalien. Er verfügt über leistungsstarke Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von den Benutzern verwendet wird. Der Dienst unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer App.

### <a name="azure-alerts"></a>Azure-Warnungen
- Warnungen bieten eine Methode zur Überwachung von Azure-Diensten und ermöglichen es Ihnen, Bedingungen für Daten zu konfigurieren. Warnungen bieten auch Benachrichtigungen, wenn eine Warnungsbedingung mit den Überwachungsdaten übereinstimmt.

### <a name="operations-management-suite-oms"></a>Operations Management Suite (OMS)
[Operations Management Suite (auch als OMS bezeichnet)](/azure/operations-management-suite/operations-management-suite-overview) ist eine Sammlung von Verwaltungsdiensten.

-   Arbeitsbereich ist für Security Center aktiviert

-   Arbeitsbereich ist für Arbeitsauslastungsüberwachung aktiviert

-   Arbeitsauslastungsüberwachung ist aktiviert für:

    -   Identität und Zugriff

    -   Security and Audit

    -   Azure SQL-Datenbankanalysen

    -   [Azure WebApp Analytics](/azure/log-analytics/log-analytics-azure-web-apps-analytics)-Lösung

    -   Key Vault-Analysen

    -   Change Tracking

-   [Application Insights-Connector (Vorschau)](/azure/log-analytics/log-analytics-app-insights-connector) ist aktiviert

-   [Aktivitätsprotokollanalyse](/azure/log-analytics/log-analytics-activity) ist aktiviert

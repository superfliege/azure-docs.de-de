---
title: Blueprint zur Azure-Zahlungsverarbeitung – Kennwortanforderungen
description: PCI-DSS-Anforderung 2
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 0df24870-6156-4415-a608-dd385b6ae807
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 8fafc277d7410ab3f8f54b0defee127fd6ec2f4d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="password-requirements-for-pci-dss-compliant-environments"></a>Kennwortanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-2"></a>PCI-DSS-Anforderung 2

**Vom Hersteller bereitgestellte Standardwerte nicht als Systemkennwörter und andere Sicherheitsparameter nutzen**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Böswillige Benutzer (außerhalb und innerhalb einer Entität) verwenden häufig die Standardkennwörter und andere Standardeinstellungen von Herstellern, um Systeme anzugreifen. Diese Kennwörter und Einstellungen sind in Hacker-Communitys gut bekannt und lassen sich problemlos über öffentliche Informationen ermitteln.

## <a name="pci-dss-requirement-21"></a>PCI-DSS-Anforderung 2.1
 
**2.1** Ändern Sie vom Hersteller bereitgestellte Standardwerte immer, und entfernen oder deaktivieren Sie unnötige Standardkonten **vor** dem Installieren eines Systems im Netzwerk.
Dies gilt für ALLE Standardkennwörter, einschließlich, aber nicht beschränkt auf jene, die von Betriebssystemen, Software zur Bereitstellung von Sicherheitsdiensten, Anwendungs- und Systemkonten, Point-of-Sale (POS)-Terminals, SNMP (Simple Network Management Protocol)-Communityzeichenfolgen usw. verwendet werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Die Anforderungen der Kennwortrichtlinie von Microsoft Azure Active Directory gelten für die neuen Kennwörter, die von Kunden innerhalb des AADUX-Portals bereitgestellt werden. Von Kunden initiierte Self-Service-Kennwortänderungen erfordern eine Bestätigung des bisherigen Kennworts. Von Administrator zurückgesetzte Kennwörter müssen bei der nächsten Anmeldung geändert werden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore erfordert, dass Benutzer sichere Kennwörter für alle Benutzer verwenden. In der Demo werden keine Beispiel- oder Gastkonten aktiviert.<br /><br />WLAN und SNMP sind nicht in der Lösung implementiert.|



### <a name="pci-dss-requirement-211"></a>PCI-DSS-Anforderung 2.1.1

**2.1.1** Für Drahtlosumgebungen, die mit der Umgebung der Karteninhaberdaten verbunden sind oder Karteninhaberdaten übertragen, ändern Sie bei der Installation ALLE Standardwerte des Herstellers für Drahtlosumgebungen, einschließlich, aber nicht beschränkt auf standardmäßige Drahtlosverschlüsselungsschlüssel, Kennwörter und SNMP-Communityzeichenfolgen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | WLAN und SNMP sind nicht in der Lösung implementiert.|



## <a name="pci-dss-requirement-22"></a>PCI-DSS-Anforderung 2.2

**2.2** Entwickeln Sie Konfigurationsstandards für alle Systemkomponenten. Stellen Sie sicher, dass diese Standards alle bekannten Sicherheitsrisiken berücksichtigen und den branchenweit anerkannten Standards zur Verstärkung der Systemsicherheit entsprechen.
Quellen branchenweit anerkannten Standards zur Verstärkung der Systemsicherheit sind u. a.:
- Center for Internet Security (CIS)
- Internationale Organisation für Normung (ISO)
- SysAdmin Audit Network Security (SANS)-Institut
- National Institute of Standards Technology (NIST)

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Für Microsoft Azure entwickelt das OSSC Technical Security Services-Team Sicherheitskonfigurationsstandards für Systeme in der Microsoft Azure-Umgebung, die branchenweit anerkannten Standards zur Verstärkung der Sicherheit entsprechen. Diese Konfigurationen sind in Systemgrundwerten dokumentiert, und relevante Änderungen an der Konfiguration werden betroffenen Teams (z. B. IPAK-Team) mitgeteilt. Es werden Prozeduren zur Überwachung der Konformität mit Sicherheitskonfigurationsstandards implementiert. Die Sicherheitskonfigurationsstandards für Systeme in der Microsoft Azure-Umgebung entsprechen branchenweit anerkannten Standards zur Verstärkung der Sicherheit und werden mindestens einmal pro Jahr überprüft. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet die Verstärkung der Sicherheit aller Dienste im Bereich der Karteninhaberdaten-Umgebung (Cardholder Data Environment, CDE). <br /><br />Der Contoso Webstore stellt auch das [Azure Security Center](https://azure.microsoft.com/services/security-center/) bereit, in dem Sie eine zentralisierte Ansicht des Sicherheitszustands aller Ihrer Azure-Ressourcen finden. Sie können auf einen Blick überprüfen, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und schnell Ressourcen ermitteln, die Ihre Aufmerksamkeit erfordern.<br /><br />Der Contoso Webstore nutzt die Operations Management Suite zur Protokollierung aller Systemänderungen. Log Analytics ermöglicht die umfassende Protokollierung von Änderungen. Änderungen können auf Ihre Richtigkeit hin überprüft werden. Genauere Anweisungen finden Sie in den [PCI-Anleitungen für die Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-221"></a>PCI-DSS-Anforderung 2.2.1

**2.2.1** Implementieren Sie nur eine primäre Funktion pro Server, um zu verhindern, dass sich Funktionen, die unterschiedliche Sicherheitsstufen erfordern, auf demselben Server befinden. (Beispielsweise müssen Webserver, Datenbankserver und DNS auf separaten Servern implementiert werden.) 

> [!NOTE]
> Implementieren Sie bei Verwendung von Virtualisierungstechnologien nur eine primäre Funktion pro virtuelle Systemkomponente.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Contoso Webstore-Dienste werden als PaaS-Dienste bereitgestellt. Alle Dienste sind isoliert und mithilfe von Netzwerksegmentierung segmentiert.<br /><br />Der Contoso Webstore verwendet auch eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro), um wichtige Methoden durchzusetzen erzwingen. Weitere Informationen finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-222"></a>PCI-DSS-Anforderung 2.2.2

**2.2.2** Aktivieren Sie nur Dienste, Protokolle, Daemons usw., die für die Funktion des Systems erforderlich sind.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-Software- und Hardwarekonfigurationen werden mindestens vierteljährlich überprüft, um unnötige Funktionen, Ports, Protokolle und Dienste zu identifizieren und zu entfernen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Contoso Webstore-Dienste werden als PaaS-Dienste bereitgestellt. Alle Dienste sind isoliert und mithilfe von Netzwerksegmentierung segmentiert.<br /><br />Der Contoso Webstore verwendet auch eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro), um wichtige Methoden durchzusetzen erzwingen. Weitere Informationen finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-223"></a>PCI-DSS-Anforderung 2.2.3

**2.2.3** Implementieren Sie zusätzliche Sicherheitsfeatures für alle erforderlichen Dienste, Protokolle oder Daemons, die als unsicher gelten. 

> [!NOTE]
> Bei Verwendung von SSL/frühem TLS müssen die Anforderungen in Anhang A2 erfüllt werden.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Contoso Webstore-Dienste werden als PaaS-Dienste bereitgestellt. Alle Dienste sind isoliert und mithilfe von Netzwerksegmentierung segmentiert. Die Bereitstellung bietet auch eine Verstärkung der Sicherheit aller Dienste im Bereich der CDE. <br /><br />Der Contoso Webstore stellt auch das [Azure Security Center](https://azure.microsoft.com/services/security-center/) bereit, in dem Sie eine zentralisierte Ansicht des Sicherheitszustands aller Ihrer Azure-Ressourcen finden. Sie können auf einen Blick überprüfen, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und schnell Ressourcen ermitteln, die Ihre Aufmerksamkeit erfordern.<br /><br />Der Contoso Webstore verwendet auch eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro), um wichtige Methoden durchzusetzen erzwingen. Weitere Informationen finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-224"></a>PCI-DSS-Anforderung 2.2.4

**2.2.4** Konfigurieren Sie Systemsicherheitsparameter, um Missbrauch zu verhindern.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure stellt sicher, dass nur entsprechend berechtigte Mitarbeiter die Sicherheitskontrollen der Azure-Plattform konfigurieren können. Dazu sind mehrstufige Zugriffssteuerungen und eine dokumentierte Geschäftsanforderung erforderlich. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore nutzt AAD und AD RBAC zum Verwalten der ordnungsgemäßen Bereitstellung von Sicherheitsparametern. Weitere Informationen finden Sie in den [PCI-Anleitungen für Identitätsverwaltung](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-225"></a>PCI-DSS-Anforderung 2.2.5

**2.2.5** Entfernen Sie alle unnötigen Funktionen wie Skripts, Treiber, Features, Subsysteme, Dateisysteme und unnötige Webserver.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt Dokumentation zum Einrichten von Grenzen bereit. Das Bedrohungsmodell und das Datenflussdiagramm von Contoso veranschaulichen alle verwendeten Dienste und aktivierten Steuerelemente.|



## <a name="pci-dss-requirement-23"></a>PCI-DSS-Anforderung 2.3

**2.3** Verschlüsseln Sie Administratorzugriff, der nicht über die Konsole erfolgt, mithilfe von starker Kryptografie. 

> [!NOTE]
> Bei Verwendung von SSL/frühem TLS müssen die Anforderungen in Anhang A2 erfüllt werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure stellt sicher, dass beim Zugriff auf die Hypervisor-Infrastruktur die Nutzung von starker Kryptografie durchgesetzt wird. Microsoft Azure stellt auch sicher, dass Kunden über das Microsoft Azure-Verwaltungsportal mit starker Kryptografie auf ihre Dienst-/IaaS-Konsolen zugreifen können. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore zeigt, wie sichere Kennwörter in eine Lösung implementiert werden können. Zudem können alle Tests ausgeführt werden, um sicherzustellen, dass die Verschlüsselung in der gesamten Lösung implementiert ist.<br /><br />Der Contoso Webstore verwendet auch eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro), um wichtige Methoden durchzusetzen erzwingen. Weitere Informationen finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



## <a name="pci-dss-requirement-24"></a>PCI-DSS-Anforderung 2.4

**2.4** Verwalten Sie einen Bestand an Systemkomponenten, die sich im Bereich für PCI-DSS befinden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Bestand der PaaS-Lösung in der Contoso Webstore-Demo kann in der bereitgestellten Dokumentation überprüft werden. Weitere Informationen finden Sie in den [PCI-Anleitungen für vorinstallierte Verwaltungslösungen](payment-processing-blueprint.md#management-solutions).|



## <a name="pci-dss-requirement-25"></a>PCI-DSS-Anforderung 2.5

**2.5** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für das Verwalten von Standardwerten von Hersteller und andere Sicherheitsparameter dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt Dokumentation bereit, die eine Übersicht über Sicherheitsparameter bereitstellt und Dienstelemente dokumentiert. |



## <a name="pci-dss-requirement-26"></a>PCI-DSS-Anforderung 2.6

**2.6** Gemeinsam genutzte Hostinganbieter müssen die gehostete Umgebung und Karteninhaberdaten der einzelnen Entitäten schützen. Diese Anbieter müssen bestimmte Anforderungen erfüllen, die in *Appendix A: Additional PCI DSS Requirements for Shared Hosting Providers* (Anhang A: Zusätzliche PCI-DSS-Anforderungen für gemeinsam genutzte Hostinganbieter) erläutert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend Microsoft Azure ist kein freigegebener Hostinganbieter. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend Microsoft Azure ist kein freigegebener Hostinganbieter.|





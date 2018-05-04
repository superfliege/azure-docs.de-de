---
title: Blueprint zur Azure-Zahlungsverarbeitung – Anforderungen für ein sicheres System
description: PCI-DSS-Anforderung 6
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 79889fdb-52d2-42db-9117-6e2f33d501e0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: cd9054afc359d7568fcd08f983f374e0b2bc3792
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="secure-system-requirements-for-pci-dss-compliant-environments"></a>Anforderungen für ein sicheres System für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-6"></a>PCI-DSS-Anforderung 6

**Entwickeln und Warten sicherer Systeme und Anwendungen**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Skrupellose Personen nutzen Sicherheitslücken, um privilegierten Zugriff auf Systeme zu erhalten. Viele dieser Sicherheitslücken werden durch vom Hersteller bereitgestellte Sicherheitspatches behoben, die von den Entitäten, die die Systeme verwalten, installiert werden müssen. Alle Systeme müssen über alle geeigneten Softwarepatches verfügen, um vor der Ausnutzung und Gefährdung von Karteninhaberdaten durch böswillige Personen und schädliche Software zu schützen.

> [!NOTE]
> Geeignete Softwarepatches sind Patches, die umfassend ausgewertet und getestet wurden, um festzustellen, dass die Patches nicht mit bestehenden Sicherheitskonfigurationen in Konflikt stehen. Für intern entwickelte Anwendungen können durch die Verwendung von Standard-Systementwicklungsprozessen und sicheren Programmiertechniken zahlreiche Sicherheitsrisiken vermieden werden.

## <a name="pci-dss-requirement-61"></a>PCI-DSS-Anforderung 6.1

**6.1** Richten Sie einen Prozess zur Identifizierung von Sicherheitsrisiken ein, indem Sie Informationen zu Sicherheitsrisiken von namhaften externen Quellen einholen, und weisen Sie neu entdeckten Sicherheitsrisiken eine Risikoeinstufung zu (z. B. „hoch“, „mittel“ oder „niedrig“).

> [!NOTE]
> Die Risikoeinstufung sollte sich auf die bewährten Methoden der Branche sowie auf die Berücksichtigung potenzieller Auswirkungen stützen. Kriterien für die Einstufung von Sicherheitsrisiken können z. B. die Berücksichtigung der CVSS-Gesamtbewertung und/oder die Klassifizierung durch den Anbieter und/oder die Art der betroffenen Systeme sein. 
> 
> Die Methoden zur Bewertung von Sicherheitsrisiken und zur Zuweisung von Risikoeinstufungen variieren je nach Umgebung und Strategie der Organisation. In der Risikoeinstufung sollten mindestens alle Sicherheitsrisiken identifiziert werden, die als „hohes Risiko“ für die Umgebung eingestuft werden. Zusätzlich zur Risikoeinstufung können Sicherheitsrisiken als „kritisch“ eingestuft werden, wenn sie eine unmittelbare Bedrohung für die Umgebung darstellen, entscheidende Systeme beeinträchtigen und/oder zu einem potenziellen Schaden führen, wenn sie nicht behoben werden. Beispiele für geschäftskritische Systeme können Sicherheitssysteme, öffentlich zugängliche Geräte und Systeme, Datenbanken und andere Systeme sein, die Karteninhaberdaten speichern, verarbeiten oder übertragen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Es wurden Verfahren entwickelt und implementiert, um auf Sicherheitsrisiken auf Hypervisorhosts in der Bereichsgrenze zu prüfen. Die Sicherheitsrisikoüberprüfung wird für Serverbetriebssysteme, Datenbanken und Netzwerkgeräte mit den entsprechenden Tools für die Sicherheitsrisikoüberprüfung durchgeführt. Die Sicherheitsrisikoüberprüfungen werden mindestens vierteljährlich ausgeführt. Microsoft Azure beauftragt unabhängige Prüfer mit der Durchführung von Penetrationstests für die Bereichsgrenzen von Microsoft Azure. Außerdem werden routinemäßig Red-Team-Übungen durchgeführt und die Ergebnisse zur Verbesserung der Sicherheit genutzt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Informationen finden Sie in den [PCI-Anleitungen für die Minderung des Risikos von Sicherheitsschwachstellen](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-62"></a>PCI-DSS-Anforderung 6.2

**6.2** Stellen Sie sicher, dass alle Systemkomponenten und sämtliche Software vor bekannten Sicherheitsrisiken geschützt sind, indem Sie die vom Hersteller bereitgestellten Sicherheitspatches installieren. Installieren Sie kritische Sicherheitspatches innerhalb eines Monats nach der Veröffentlichung.

> [!NOTE]
> Wichtige Sicherheitspatches sollten gemäß dem in Anforderung 6.1 definierten Prozess der Risikoeinstufung identifiziert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ist dafür verantwortlich, dass alle Netzwerkgeräte und sämtliche Hypervisor-Betriebssystemsoftware vor bekannten Sicherheitsrisiken geschützt sind, indem es entsprechende, vom Hersteller bereitgestellte Sicherheitspatches installiert. Sofern ein Kunde nicht fordert, den Dienst nicht zu nutzen, ist ein Patchverwaltungsprozess vorhanden, der sicherstellt, dass Sicherheitsrisiken auf Betriebssystemebene rechtzeitig verhindert und behoben werden. Die Produktionsserver werden überprüft, um die Patchkonformität monatlich zu validieren. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore ist eine PaaS-Dienstlösung. Azure sorgt für die Verwaltung aller Dienstpatches.|



## <a name="pci-dss-requirement-63"></a>PCI-DSS-Anforderung 6.3

**6.3** Entwickeln Sie wie folgt sichere interne und externe Softwareanwendungen (einschließlich des webbasierten administrativen Zugriffs auf Anwendungen):
- Gemäß PCI-DSS (z. B. sichere Authentifizierung und Protokollierung)
- Auf Grundlage von Branchenstandards und/oder bewährten Methoden
- Integration von Informationssicherheit während des gesamten Lebenszyklus der Softwareentwicklung 

> [!NOTE]
> Dies gilt für sämtliche intern entwickelte Software sowie für Individual- und kundenspezifische Software, die von Drittanbietern entwickelt wurde.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-Anwendungen und -Endpunkte werden in Übereinstimmung mit der Microsoft Security Development Lifecycle (SDL)-Methodik entwickelt, die den DSS-Anforderungen entspricht. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde so konzipiert, dass er die bewährten Methoden der Branche zum Schutz von CHD befolgt. Die Bereitstellungsanleitung enthält die Details der Sicherheitsmaßnahmen, und die Protokollierung ist aktiviert.|



### <a name="pci-dss-requirement-631"></a>PCI-DSS-Anforderung 6.3.1

**6.3.1** Entfernen Sie Entwicklungs-, Test- und/oder benutzerdefinierte Anwendungskonten, Benutzer-IDs und Kennwörter, bevor Anwendungen aktiviert oder für Kunden freigegeben werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Für Hauptversionen vor der Produktionsbereitstellung wird von einem bestimmten Sicherheitsberater außerhalb des Azure-Entwicklungsteams eine abschließende Sicherheitsüberprüfung (Final Security Review, FSR) durchgeführt, um sicherzustellen, dass nur produktionsreife Anwendungen freigegeben werden. Im Rahmen dieser abschließenden Überprüfung wird sichergestellt, dass alle Testkonten und Testdaten entfernt wurden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore bietet einen Stagingdienst, der protokolliert wird und isoliert ist. Jede der Netzwerkschichten verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG): Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-632"></a>PCI-DSS-Anforderung 6.3.2

**6.3.2** Überprüfen Sie den benutzerdefinierten Code vor der Freigabe für die Produktion oder für Kunden, um potenzielle Sicherheitsrisiken bei der Programmierung zu identifizieren (entweder durch manuelle oder automatisierte Prozesse), um mindestens Folgendes einzubeziehen:
- Codeänderungen werden von anderen Personen als dem ursprünglichen Programmierer und von Personen überprüft, die sich mit Code Review-Verfahren und sicheren Programmierpraktiken auskennen.
- Code Reviews stellen sicher, dass der Code nach sicheren Programmierrichtlinien entwickelt wird.
- Entsprechende Korrekturen werden vor der Veröffentlichung durchgeführt.
- Die Code Review-Ergebnisse werden vor der Veröffentlichung vom Management überprüft und genehmigt. 

> [!NOTE]
> Diese Anforderung für Code Reviews gilt im Rahmen des Lebenszyklus der Systementwicklung für sämtlichen benutzerdefinierten Code (sowohl für internen als auch für öffentlichen Code). 
>
> Code Reviews können durch sachkundiges internes Personal oder durch Drittanbieter durchgeführt werden. Öffentlich zugängliche Webanwendungen unterliegen darüber hinaus zusätzlichen Kontrollen, um nach der Implementierung laufende Bedrohungen und Sicherheitsrisiken zu beseitigen, wie in der PCI-DSS-Anforderung 6.6 definiert.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-Anwendungen und -Endpunkte werden in Übereinstimmung mit der Microsoft Security Development Lifecycle (SDL)-Methodik entwickelt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore bietet einen Stagingdienst, der protokolliert wird und isoliert ist. Jede der Netzwerkschichten verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG): Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](payment-processing-blueprint.md#network-security-groups).|



## <a name="pci-dss-requirement-64"></a>PCI-DSS-Anforderung 6.4

**6.4** Befolgen Sie die Änderungssteuerungsprozesse und -verfahren für alle Änderungen an Systemkomponenten. Die Prozesse müssen Folgendes umfassen (siehe Anforderungen 6.4.1 bis 6.4.6).

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft befolgt den NIST-Leitfaden in Bezug auf Sicherheitsaspekte bei der Softwareentwicklung, indem die Informationssicherheit von Anfang an in den SDLC integriert werden muss. Die kontinuierliche Integration der Sicherheitsvorkehrungen in den Microsoft Security Development Lifecycle (SDL) ermöglicht Folgendes:<ul><li>Früherkennung und Verringerung von Sicherheitsrisiken und fehlerhaften Konfigurationen</li><li>Bewusstsein für mögliche Herausforderungen bei der Programmierung von Software, die durch erforderliche Sicherheitskontrollen verursacht werden</li><li>Identifizierung von gemeinsam genutzten Sicherheitsdiensten und Wiederverwendung von Tools für bewährte Methoden für die Sicherheit, die den Sicherheitsstatus durch bewährte Methoden und Verfahren verbessern</li><li>Durchsetzung des bereits umfassenden Risikomanagementprogramms von Microsoft</li></ul>Microsoft Azure hat Prozesse für Change Management und Release Management etabliert, um die Implementierung wichtiger Änderungen zu steuern. Dazu zählen:<ul><li>Identifikation und Dokumentation der geplanten Änderung</li><li>Identifikation von Geschäftszielen, Prioritäten und Szenarien bei der Produktplanung</li><li>Spezifikation des Feature-/Komponentendesigns</li><li>Überprüfung der Einsatzbereitschaft auf der Grundlage vordefinierter Kriterien/Prüflisten zur Beurteilung des Gesamtrisikos bzw. der Gesamtauswirkungen</li><li>Tests, Autorisierung und Change Management auf Grundlage von Einstiegs-/Beendigungskriterien für DEV- (Entwicklung), INT- (Integrationstest), STAGE- (Präproduktion) und PROD-Umgebungen (Produktion). Kunden sind für ihre eigenen Anwendungen verantwortlich, die in Microsoft Azure gehostet werden.</li></ul> |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Demo bietet einen Stagingdienst, der protokolliert wird und isoliert ist. <br /><br />Jede der Netzwerkschichten verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG): Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](payment-processing-blueprint.md#network-security-groups).<br /><br />Änderungen werden mit der Operations Management Suite protokolliert, und Runbooks werden zum Erfassen von Protokollen verwendet. Log Analytics ermöglicht die umfassende Protokollierung von Änderungen. Änderungen können auf Ihre Richtigkeit hin überprüft werden. Genauere Anweisungen finden Sie in den [PCI-Anleitungen für die Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-641"></a>PCI-DSS-Anforderung 6.4.1

**6.4.1** Trennen Sie Entwicklungs- und Testumgebungen von Produktionsumgebungen, und setzen Sie die Trennung mithilfe von Zugriffssteuerungen durch.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore bietet einen Stagingdienst, der protokolliert wird und isoliert ist. Jede der Netzwerkschichten verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG): Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-642"></a>PCI-DSS-Anforderung 6.4.2

**Abschnitt 6.4.2** Aufgabentrennung zwischen Entwicklungs-/Test- und Produktionsumgebung

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore bietet einen Stagingdienst, der protokolliert wird und isoliert ist. Jede der Netzwerkschichten verfügt über eine dedizierte Netzwerksicherheitsgruppe (NSG): Weitere Informationen finden Sie unter [PCI-Anleitungen für Netzwerksicherheitsgruppen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-643"></a>PCI-DSS-Anforderung 6.4.3

**6.4.3** Produktionsdaten (Live-PANs) werden nicht zum Testen oder Entwickeln verwendet.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore weist keine Daten für eine Live-Hauptkontonummer (Primary Account Number, PAN) auf.|



### <a name="pci-dss-requirement-644"></a>PCI-DSS-Anforderung 6.4.4

**6.4.4** Entfernen von Testdaten und -konten aus Systemkomponenten, bevor das System aktiviert wird oder in die Produktionsumgebung wechselt.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore besitzt keine Testkonten.|



### <a name="pci-dss-requirement-645"></a>PCI-DSS-Anforderung 6.4.5

**6.4.5** Änderungssteuerungsverfahren für die Implementierung von Sicherheitspatches und Softwareänderungen müssen Folgendes umfassen:
- **6.5.4.1** Dokumentation zu den Auswirkungen
- **6.5.4.2** Dokumentierte Genehmigung der Änderungen durch autorisierte Parteien
- **6.5.4.3** Funktionstests, um sicherzustellen, dass die Änderung keine negativen Auswirkungen auf die Sicherheit des Systems hat
- **6.5.4.4** Rückzugsverfahren

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore ist eine PaaS-Dienstlösung. Und Azure sorgt für die Verwaltung aller Dienstpatches.|



### <a name="pci-dss-requirement-646"></a>PCI-DSS-Anforderung 6.4.6

**6.4.6** Nach Abschluss einer wichtigen Änderung müssen alle relevanten PCI-DSS-Anforderungen für alle neuen oder geänderten Systeme und Netzwerke umgesetzt und die Dokumentation gegebenenfalls aktualisiert werden.

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore ist eine PaaS-Dienstlösung. Und Azure sorgt für die Verwaltung aller Dienstpatches.|



## <a name="pci-dss-requirement-65"></a>PCI-DSS-Anforderung 6.5

**6.5** Gehen Sie wie folgt auf häufige Sicherheitsrisiken bei der Programmierung in Softwareentwicklungsprozessen ein:
- Schulen Sie Entwickler mindestens einmal jährlich hinsichtlich aktueller, sicherer Programmierverfahren, einschließlich der Vermeidung häufiger Sicherheitsrisiken bei der Programmierung.
- Entwickeln Sie Anwendungen auf der Grundlage sicherer Programmierrichtlinien.

> [!NOTE]
> Die in den Abschnitten 6.5.1 bis 6.5.10 aufgeführten Sicherheitsrisiken entsprachen den aktuellen bewährten Methoden der Branche, als diese Version von PCI-DSS veröffentlicht wurde. Da jedoch die bewährten Methoden der Branche für die Verwaltung von Sicherheitsrisiken aktualisiert werden (z. B. OWASP Guide, SANS CWE Top 25, CERT Secure Coding usw.), müssen für diese Anforderungen die aktuellen bewährten Methoden verwendet werden. 
> 
> [!NOTE]
> Die nachfolgenden Anforderungen 6.5.1 bis 6.5.6 gelten für alle Anwendungen (intern oder extern). Die nachfolgenden Anforderungen 6.5.7 bis 6.5.10 gelten für Webanwendungen und Anwendungsschnittstellen (intern oder extern). 

- **6.5.1** Einschleusungsfehler, insbesondere Einschleusung von SQL-Befehlen. Berücksichtigen Sie auch die Einschleusung von Betriebssystembefehlen, LDAP- und XPath-Einschleusungsfehler sowie andere Einschleusungsfehler.
- **6.5.2** Pufferüberläufe
- **6.5.3** Unsicherer kryptographischer Speicher
- **6.5.4** Unsichere Kommunikation
- **6.5.5** Unsachgemäße Fehlerbehandlung
- **6.5.6** Alle Sicherheitsrisiken mit hohem Risiko, die bei der Identifizierung der Sicherheitsrisiken ermittelt wurden (wie in PCI-DSS-Anforderung 6.1 definiert)
- **6.5.7** Cross-Site Scripting (XSS)
- **6.5.8** Unzulässige Zugriffssteuerung (z. B. unsichere direkte Objektverweise, fehlende Beschränkung des URL-Zugriffs, Verzeichnisausnahmen und fehlende Beschränkung des Benutzerzugriffs auf Funktionen)
- **6.5.9** Websiteübergreifende Anforderungsfälschung (Cross-Site Request Forgery, CSRF)
- **6.5.10** Fehlerhafte Authentifizierung und Sitzungsverwaltung

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 6.4](#pci-dss-requirement-6-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Demo bietet einen Leitfaden zur sicheren Entwicklung, ein DFD- und Bedrohungsmodell zur Veranschaulichung sicherer Entwicklungsmethoden.|



## <a name="pci-dss-requirement-66"></a>PCI-DSS-Anforderung 6.6

**6.6** Bei öffentlich zugänglichen Webanwendungen sollten Sie ständig neue Bedrohungen und Sicherheitsrisiken behandeln und sicherstellen, dass diese Anwendungen durch eine der folgenden Methoden vor bekannten Angriffen geschützt sind:

- Überprüfung von öffentlich zugänglichen Webanwendungen mittels manueller oder automatisierter Tools oder Methoden zur Bewertung der Sicherheitsanfälligkeit von Anwendungen, mindestens einmal jährlich und nach jeder Änderung. 

   > [!NOTE]
   > Diese Bewertung ist nicht identisch mit der Sicherheitsrisikoüberprüfung, die für Anforderung 11.2 durchgeführt wurde. 

- Installation einer automatisierten technischen Lösung, die webbasierte Angriffe (z. B. eine Web Application Firewall) vor öffentlich zugänglichen Webanwendungen erkennt und verhindert, um den gesamten Datenverkehr kontinuierlich zu überprüfen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure testet öffentlich zugängliche Webanwendungen im Rahmen des SDL-Prozesses, bevor Anwendungen in der Produktionsumgebung bereitgestellt werden. Darüber hinaus überprüft Microsoft regelmäßig alle öffentlich zugänglichen Webanwendungen in der Produktionsumgebung, um mögliche Sicherheitsrisiken zu erkennen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Referenzlösung verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Informationen finden Sie in den [PCI-Anleitungen für die Minderung des Risikos von Sicherheitsschwachstellen](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-67"></a>PCI-DSS-Anforderung 6.7

**6.7** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für die Entwicklung und Pflege sicherer Systeme und Anwendungen dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Referenzlösung verringert das Risiko von Sicherheitsschwachstellen durch ein Application Gateway mit Web Application Firewall (WAF) und dem aktivierten OWASP-Regelsatz. Weitere Informationen finden Sie in den [PCI-Anleitungen für die Minderung des Risikos von Sicherheitsschwachstellen](payment-processing-blueprint.md#application-gateway).|





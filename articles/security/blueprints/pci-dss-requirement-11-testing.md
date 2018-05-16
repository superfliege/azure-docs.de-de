---
title: Blueprint zur Azure-Zahlungsverarbeitung – Testanforderungen
description: PCI-DSS-Anforderung 11
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 9753853b-ad80-4be4-9416-2583b8fd2029
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 21cb854ba35adcf7576874e3c6c7149fbd33d998
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="testing-requirements-for-pci-dss-compliant-environments"></a>Testanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-11"></a>PCI-DSS-Anforderung 11

**Regelmäßiges Testen der Sicherheitssysteme und -prozesse**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Ständig werden Sicherheitsrisiken von Personen mit schlechten Absichten wie auch von Forschern entdeckt oder entstehen bei der Entwicklung neuer Software. Systemkomponenten, Prozesse und angepasste Software sollten regelmäßig getestet werden, um sicherzustellen, dass die Sicherheitseinrichtungen fortlaufend an eine sich ändernde Umgebung angepasst werden.

## <a name="pci-dss-requirement-111"></a>PCI-DSS-Anforderung 11.1

**11.1** Implementieren Sie Prozesse, um vierteljährlich auf Vorhandensein drahtloser Zugriffspunkte (802.11) zu testen und alle autorisierten und nicht autorisierten drahtlosen Zugriffspunkte zu ermitteln.

> [!NOTE]
> Zu den Methoden, die bei diesem Prozess verwendet werden können, gehören beispielsweise Prüfungen auf drahtlose Netzwerke, physische und logische Kontrollen von Systemkomponenten und Infrastrukturen, eine Netzwerkzugriffssteuerung (NAC) oder drahtlose Systeme zur Angriffserkennung und -vermeidung.
Unabhängig davon, welche Methoden verwendet werden, müssen diese geeignet sein, autorisierte wie auch nicht autorisierte Geräte zu erkennen und zu identifizieren.


**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure gestattet keine drahtlosen Verbindungen in die Azure-Netzwerkumgebung. Interne Sicherheitsteams führen vierteljährlich eine Regelprüfung auf nicht autorisierte drahtlose Signale durch. Nicht autorisierte Signale werden untersucht und entfernt. Kunden ist es nicht gestattet, drahtlose Technologie in der Azure-Umgebung bereitzustellen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | WLAN und SNMP sind nicht in der Lösung implementiert.|



### <a name="pci-dss-requirement-1111"></a>PCI-DSS-Anforderung 11.1.1

**11.1.1** Betreiben Sie einen Bestand an autorisierten drahtlosen Zugriffspunkten einschließlich einer dokumentierten geschäftlichen Rechtfertigung.

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 11.1](#pci-dss-requirement-11-1). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | WLAN und SNMP sind nicht in der Lösung implementiert.|



### <a name="pci-dss-requirement-1112"></a>PCI-DSS-Anforderung 11.1.2

**11.1.2** Implementieren Sie Verfahren für eine Reaktion auf Incidents im Falle der Entdeckung nicht autorisierter drahtloser Zugriffspunkte.


**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 11.1](#pci-dss-requirement-11-1). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | WLAN und SNMP sind nicht in der Lösung implementiert.|



## <a name="pci-dss-requirement-112"></a>PCI-DSS-Anforderung 11.2

**11.2** Führen Sie mindestens vierteljährlich sowie nach allen größeren Änderungen im Netzwerk (z.B. nach Installation neuer Komponenten, Änderungen in der Netzwerktopologie, Modifikationen an den Firewallregeln oder Produktupgrades) interne und externe Überprüfungen zu Sicherheitsrisiken im Netzwerk durch. 

> [!NOTE]
> Für den vierteljährlichen Überprüfungsvorgang können mehrere Überprüfungsberichte zusammengefasst werden, um nachzuweisen, dass alle Systeme überprüft und alle maßgeblichen Sicherheitsrisiken bearbeitet wurden. Die Bereitstellung zusätzlicher Dokumentation kann erforderlich sein, um nachzuweisen, dass nicht behobene Sicherheitsrisiken gegenwärtig bearbeitet werden.
> Für die anfängliche PCI-DSS-Konformität ist die erfolgreiche Durchführung von vier Quartalsüberprüfungen nicht erforderlich, wenn der Prüfer bestätigt, dass 1) das aktuellste Ergebnis eine bestandene Überprüfung war, 2) die Entität Richtlinien und Verfahren dokumentiert hat, die eine vierteljährliche Überprüfung erfordern, und 3) in den Prüfergebnissen vermerkte Sicherheitsrisiken beseitigt wurden und diese Beseitigung durch eine Wiederholung der Überprüfungen nachgewiesen wurde. Für Folgejahre nach der PCI-DSS-Erstüberprüfung müssen vier Quartalsüberprüfungen bestanden worden sein.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure führt vierteljährlich interne und externe Überprüfungen auf Sicherheitsrisiken durch. Überprüfungen werden von qualifiziertem Fachpersonal durchgeführt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde im Istzustand Penetrationstests und Sicherheitsrisikoüberprüfungen unterzogen. Die Penetrationstestergebnisse können mithilfe gängiger Tools wie Nmap oder pentest tools.com reproduziert werden. Die Ergebnisse des Penetrationstests dienen der Feststellung einer unklaren Angriffsfläche ohne verwertbare Elemente. Ferner bieten [Azure Security Center](https://azure.microsoft.com/services/security-center/) und der [Azure Advisor](/azure/advisor/advisor-security-recommendations) Informationen zu Sicherheitsrisiken und deren Behebung.|



### <a name="pci-dss-requirement-1121"></a>PCI-DSS-Anforderung 11.2.1

**11.2.1** Führen Sie vierteljährlich interne Sicherheitsrisikoüberprüfungen durch. Bearbeiten Sie Sicherheitsrisiken und wiederholen Sie Überprüfungen, um sicherzustellen, dass alle hohen Sicherheitsrisiken entsprechend der Sicherheitsrisikoeinstufung der Entität (nach Anforderung 6.1) beseitigt wurden. Überprüfungen sind von qualifiziertem Fachpersonal durchzuführen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure führt Sicherheitsrisikoüberprüfungen für die zugrunde liegende Infrastruktur im Gültigkeitsbereich durch. Microsoft Azure implementiert Sicherheitsrisikoüberprüfungen für Serverbetriebssysteme, Datenbanken und Netzwerkgeräte mit den entsprechenden Tools für die Sicherheitsrisikoüberprüfung. Azure-Webanwendungen werden mit geeigneten branchenüblichen Lösungen überprüft. Die Sicherheitsrisikoüberprüfungen werden vierteljährlich ausgeführt.<br /><br />Überprüfungen werden ggf. für alle Systeme wiederholt, bis alle hohen Sicherheitsrisiken (gemäß Anforderung 6.1) beseitigt wurden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde im Istzustand Penetrationstests und Sicherheitsrisikoüberprüfungen unterzogen. Die Penetrationstestergebnisse können mithilfe gängiger Tools wie Nmap oder pentest tools.com reproduziert werden. Die Ergebnisse des Penetrationstests dienen der Feststellung einer unklaren Angriffsfläche ohne verwertbare Elemente. Ferner bieten [Azure Security Center](https://azure.microsoft.com/services/security-center/) und der [Azure Advisor](/azure/advisor/advisor-security-recommendations) Informationen zu Sicherheitsrisiken und deren Behebung.|



### <a name="pci-dss-requirement-1122"></a>PCI-DSS-Anforderung 11.2.2

**11.2.2** Lassen Sie vierteljährlich externe Sicherheitsrisikoüberprüfungen durch einen externen zertifizierten Prüfanbieter (Approved Scanning Vendor, ASV) durchführen, der vom Payment Card Industry Security Standards Council (PCI-SSC) autorisiert wurde. Wiederholen Sie Überprüfungen nach Bedarf so oft, bis sie bestanden wurden. 

> [!NOTE]
> Externe Sicherheitsrisikoüberprüfungen sind vierteljährlich durch einen externen zertifizierten Prüfanbieter (Approved Scanning Vendor, ASV) durchzuführen, der vom Payment Card Industry Security Standards Council (PCI-SSC) autorisiert wurde.
> Informationen zu den kundenseitigen Überprüfungsaufgaben, zur Vorbereitung einer Überprüfung usw. entnehmen Sie dem auf der PCI-SSC-Website veröffentlichten ASV Program Guide.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure führt externe Sicherheitsrisikoüberprüfungen für die zugrunde liegende Infrastruktur im Gültigkeitsbereich durch, soweit sie von außen zugänglich ist. Überprüfungen werden von einem autorisierten Überprüfungsanbieter (Approved Scan Vendor, ASV) ausgeführt.<br /><br />Microsoft Azure bezieht Benachrichtigungen zu monatlichen MSRC/OSSC-Patches und prüft mindestens vierteljährlich auf Sicherheitsrisiken. Erkannte Sicherheitsrisiken werden entsprechend dem vorgesehenen Zeitplan auf Basis der Risikostufe bewertet und beseitigt.<br /><br />In jedem Vierteljahr werden zielgerichtete und umfassende Überprüfungen auf Sicherheitsrisiken an priorisierten Komponenten der Microsoft Azure-Umgebung durchgeführt, um Sicherheitsrisiken zu erkennen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Beim Bereitstellen des Contoso Webstores sind Kunden der Demoversion für die Durchführung der vierteljährlichen externen Sicherheitsrisikoüberprüfungen und Wiederholungsüberprüfungen aller PaaS-Instanzen in ihrer CDE zuständig. Hierzu ist ein zertifizierter Überprüfungsanbieter (Approved Scan Vendor, ASV) hinzuzuziehen, der vom Payment Card Industry Security Standards Council autorisiert wurde.<br /><br />|



### <a name="pci-dss-requirement-1123"></a>PCI-DSS-Anforderung 11.2.3

**11.2.3** Führen Sie nach allen größeren Änderungen interne und externe Überprüfungen durch, und wiederholen Sie diese nach Bedarf. Überprüfungen sind von qualifiziertem Fachpersonal durchzuführen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Ergebnisse werden an Projektbeteiligte gemeldet, und die Wiederherstellung wird durch das Azure-Sicherheitsteam bis zum Abschluss nachverfolgt. Azure-Testergebnisse werden im Rahmen der Geheimhaltungsvereinbarung möglicherweise für Kunden freigegeben. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Kunden sind für die vierteljährliche Durchführung interner und externer Überprüfungen auf Sicherheitslücken sowie für ggf. erforderliche Überprüfungswiederholungen auf allen PaaS-Instanzen innerhalb ihrer CDE verantwortlich. Überprüfungen sind nach größeren Änderungen in den relevanten Teilen der Umgebung durchzuführen.<br /><br />Überprüfungen sind von einem ASV oder durch von der Organisation unabhängigen Mitarbeitern durchzuführen.|



## <a name="pci-dss-requirement-113"></a>PCI-DSS-Anforderung 11.3

**11.3** Implementieren Sie eine Methodik für Penetrationstests mit den folgenden Eigenschaften:
- Basiert auf branchenweit bewährten Penetrationstestmethoden (z.B. NIST SP800-115).
- Umfasst Themen für den gesamten CDE-Umkreis und kritische Systeme.
- Umfasst Tests, die sowohl innerhalb des Netzwerks als auch außerhalb durchgeführt werden.
- Umfasst Tests zur Bewertung von Steuereinrichtungen zur Segmentierung und Verkleinerung des Gültigkeitsbereichs.
- Definiert Penetrationstests in der Anwendungsschicht, die sich mindestens auf die in Anforderung 6.5 aufgelisteten Sicherheitsrisiken erstrecken.
- Definiert Penetrationstests in der Vermittlungsschicht, die sich auf Netzwerkfunktionen unterstützende Komponenten sowie Betriebssysteme erstrecken.
- Umfasst Prüfung und Einschätzungen von Bedrohungen und Sicherheitsrisiken, die in den letzten 12 Monaten aufgetreten sind.
- Spezifiziert die Aufbewahrung der Penetrationstestergebnisse und der Auswirkungen der Wiederherstellungsmaßnahmen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure bewertet Dienste durch CREST-zertifizierte Prüfer mithilfe von Penetrationstests von Drittanbietern, die auf den OWASP (Open Web Application Security Project) Top Ten basieren. Die Testergebnisse werden über ein Risikoregister nachverfolgt, welches regelmäßig kontrolliert und überprüft wird, um die Einhaltung von Sicherheitspraktiken sicherzustellen. <br /><br />Microsoft setzt Red Teaming für von Microsoft verwaltete Infrastrukturen, Dienste und Anwendungen ein. Beim Red Teaming und bei Penetrationstests für Live-Websites werden Endkundendaten nicht vorsätzlich als Ziel verwendet. Die Tests werden für Microsoft Azure-Infrastruktur und an Microsoft Azure-Plattformen sowie für die eigenen Anwendungen und Daten von Microsoft durchgeführt. Mandanten, Anwendungen und Daten von Kunden, die unter Azure gehostet werden, dienen niemals als Ziel.<br /><br />Microsoft Azure hat einen unabhängigen Prüfer mit der Entwicklung eines Systembewertungsplans und der Durchführung von Kontrolleinrichtungsbewertungen beauftragt. Bewertungen von Kontrolleinrichtungen werden jährlich ausgeführt, und die Ergebnisse werden den relevanten Akteuren mitgeteilt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde im Istzustand Penetrationstests und Sicherheitsrisikoüberprüfungen unterzogen. Die Penetrationstestergebnisse können mithilfe gängiger Tools wie Nmap oder pentest tools.com reproduziert werden. Die Ergebnisse des Penetrationstests dienen der Feststellung einer unklaren Angriffsfläche ohne verwertbare Elemente. Ferner bieten [Azure Security Center](https://azure.microsoft.com/services/security-center/) und der [Azure Advisor](/azure/advisor/advisor-security-recommendations) Informationen zu Sicherheitsrisiken und deren Behebung.|



### <a name="pci-dss-requirement-1131"></a>PCI-DSS-Anforderung 11.3.1

**11.3.1** Führen Sie mindestens einmal im Jahr sowie nach allen größeren Upgrades oder Änderungen an Infrastruktur oder Anwendungen (z.B. einem Betriebssystemupgrade, der Ergänzung eines Subnetzes in der Umgebung oder dem Hinzufügen eines Webservers zur Umgebung) *externe* Penetrationstests durch.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 11.3](#pci-dss-requirement-11-3). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde im Istzustand Penetrationstests und Sicherheitsrisikoüberprüfungen unterzogen. Die Penetrationstestergebnisse können mithilfe gängiger Tools wie Nmap oder pentest tools.com reproduziert werden. Die Ergebnisse des Penetrationstests dienen der Feststellung einer unklaren Angriffsfläche ohne verwertbare Elemente. Ferner bieten [Azure Security Center](https://azure.microsoft.com/services/security-center/) und der [Azure Advisor](/azure/advisor/advisor-security-recommendations) Informationen zu Sicherheitsrisiken und deren Behebung.|



### <a name="pci-dss-requirement-1132"></a>PCI-DSS-Anforderung 11.3.2

**11.3.2** Führen Sie mindestens einmal im Jahr sowie nach allen größeren Upgrades oder Änderungen an Infrastruktur oder Anwendungen (z.B. einem Betriebssystemupgrade, der Ergänzung eines Subnetzes in der Umgebung oder dem Hinzufügen eines Webservers zur Umgebung) *interne* Penetrationstests durch.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure beauftragt unabhängige Prüfer mit der Durchführung von Penetrationstests für die Bereichsgrenzen von Microsoft Azure. Außerdem werden routinemäßig Red-Team-Übungen durchgeführt und die Ergebnisse zur Verbesserung der Sicherheit genutzt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore wurde im Istzustand Penetrationstests und Sicherheitsrisikoüberprüfungen unterzogen. Die Penetrationstestergebnisse können mithilfe gängiger Tools wie Nmap oder pentest tools.com reproduziert werden. Die Ergebnisse des Penetrationstests dienen der Feststellung einer unklaren Angriffsfläche ohne verwertbare Elemente. Ferner bieten [Azure Security Center](https://azure.microsoft.com/services/security-center/) und der [Azure Advisor](/azure/advisor/advisor-security-recommendations) Informationen zu Sicherheitsrisiken und deren Behebung.|



### <a name="pci-dss-requirement-1133"></a>PCI-DSS-Anforderung 11.3.3

**11.3.3** Verwertbare Sicherheitsrisiken, die beim Penetrationstest erkannt wurden, werden beseitigt, und der Test wird wiederholt, um die Korrekturen zu überprüfen.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Für die Überwachung von Komponenten der Microsoft Azure-Plattform auf bekannte Sicherheitsrisiken wurden Verfahren implementiert. <br /><br /><br /><br />In jedem Vierteljahr werden zielgerichtete und umfassende Überprüfungen auf Sicherheitsrisiken an priorisierten Komponenten der Azure-Produktionsumgebung durchgeführt, um Sicherheitsrisiken zu erkennen. Die Ergebnisse sind den Projektbeteiligten zu melden, und die Beseitigung wird vom Team bis zum Abschluss nachverfolgt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) und [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieten Informationen zu Sicherheitsrisiken und deren Beseitigung und sollen sicherstellen, dass alle offenen Probleme in der CDE des Contoso Webstore-Demos behoben wurden.|



### <a name="pci-dss-requirement-1134"></a>PCI-DSS-Anforderung 11.3.4

**11.3.4** Wird die Segmentierung zur Isolierung der CDE von anderen Netzwerken verwendet, dann führen Sie mindestens einmal im Jahr sowie nach der Vornahme von Änderungen an Segmentierungskontrollen oder -methoden Penetrationstests durch, um nachzuweisen, dass die Segmentierungsmethoden funktionsfähig und wirksam sind. Alle nicht zum Gültigkeitsbereich gehörenden Systeme sind von Systemen in der CDE zu isolieren.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Für die Überwachung von Komponenten der Microsoft Azure-Plattform auf bekannte Sicherheitsrisiken wurden Verfahren implementiert. <br /><br /><br /><br />In jedem Vierteljahr werden zielgerichtete und umfassende Überprüfungen auf Sicherheitsrisiken an priorisierten Komponenten der Azure-Produktionsumgebung durchgeführt, um Sicherheitsrisiken zu erkennen. Die Ergebnisse sind den Projektbeteiligten zu melden, und die Beseitigung wird vom Team bis zum Abschluss nachverfolgt. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) und [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieten Informationen zu Sicherheitsrisiken und deren Beseitigung und sollen sicherstellen, dass alle offenen Probleme in der CDE des Contoso Webstore-Demos behoben wurden.|



### <a name="pci-dss-requirement-11341"></a>PCI-DSS-Anforderung 11.3.4.1

**11.3.4.1** *Zusätzliche Anforderung nur an Dienstanbieter:* Wird eine Segmentierung verwendet, dann überprüfen Sie den PCI-DSS-Geltungsbereich durch Ausführen von Penetrationstests für Segmentierungskontrollen mindestens alles sechs Monate sowie nach der Vornahme von Änderungen an Segmentierungskontrollen oder -methoden.

> [!NOTE]
> Diese Anforderung ist bis zum 31. Januar 2018 eine bewährte Methode, danach stellt sie eine Anforderung dar.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Siehe Abschnitt „Microsoft Azure“ für [Anforderung 11.3.4](#pci-dss-requirement-11-3-4). |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) und [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieten Informationen zu Sicherheitsrisiken und deren Beseitigung und sollen sicherstellen, dass alle offenen Probleme in der CDE des Contoso Webstore-Demos behoben wurden.|



## <a name="pci-dss-requirement-114"></a>PCI-DSS-Anforderung 11.4

**11.4** Verwenden Sie Verfahren zum Erkennen und/oder Vermeiden von Einbruchversuchen, um ein Eindringen ins Netzwerk zu erkennen bzw. zu vermeiden. Überwachen Sie den gesamten Datenverkehr am CDE-Umkreis sowie an wichtigen Stellen in der CDE selbst, und warnen Sie Mitarbeiter vor mutmaßlichen Gefährdungen.
Halten Sie alle IDS- und IPS-Engines, -Baselines und -Signaturen auf dem neuesten Stand.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure führt Echtzeitanalysen von Ereignissen innerhalb seiner Betriebsumgebung durch, und IDS-Systeme erzeugen in Quasi-Echtzeit Warnungen zu Ereignissen, die das System möglicherweise gefährden. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore ist ein PaaS-Dienst. Die Erkennung und Vermeidung von Einbruchsversuchen ins Netzwerk liegt in der Zuständigkeit von Azure. [Azure Security Center](https://azure.microsoft.com/services/security-center/) und [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieten für Einbruchsversuche Warnungen und Korrekturen.|



## <a name="pci-dss-requirement-115"></a>PCI-DSS-Anforderung 11.5

**11.5** Implementieren Sie einen Mechanismus zur Änderungserkennung (z.B. ein Tool zur Überwachung der Dateiintegrität), um Mitarbeiter im Fall nicht autorisierter Modifikationen an kritischen Systemdateien, Konfigurationsdateien oder Inhaltsdateien (einschließlich Änderungen, Ergänzungen oder Löschungen) zu warnen, und konfigurieren Sie die Software so, dass Vergleiche kritischer Dateien mindestens wöchentlich durchgeführt werden. 

> [!NOTE]
> Im Bereich der Änderungserkennung sind kritische Dateien normalerweise solche, die sich nicht regelmäßig ändern, deren Änderung aber auf ein System- oder Gefährdungsrisiko hinweisen könnte. Mechanismen zur Änderungserkennung wie z.B. Produkte zur Überwachung der Dateiintegrität sind bei Auslieferung normalerweise für kritische Dateien auf dem jeweiligen Betriebssystem vorkonfiguriert. Weitere wichtige Dateien, etwa solche für benutzerdefinierte Anwendungen, müssen bewertet und von der Entität (d.h. vom Händler oder Dienstanbieter) definiert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure verwaltet potenzielle Änderungen und Ereignisse, die die Sicherheit oder Verfügbarkeit der Dienste beeinträchtigen können, in einem online verfügbaren Dienstdashboard und benachrichtigt auch seine Kunden darüber. Änderungen an den Sicherheitsverpflichtungen von Microsoft Azure-Kunden werden zeitnah auf der Microsoft Azure-Website veröffentlicht.<br /><br />Die Installation von oder Änderungen an Software in der Microsoft Azure-Produktionsumgebung erfolgen ausschließlich durch autorisierte Administrationsmitarbeiter gemäß den Change Management-Verfahren. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Demo ist ein PaaS-Dienst, und die Änderungserkennung wurde mithilfe von Log Analytics implementiert. Weitere Informationen finden Sie in den [PCI-Anleitungen für vorinstallierte Verwaltungslösungen](payment-processing-blueprint.md#management-solutions).<br /><br />|



### <a name="pci-dss-requirement-1151"></a>PCI-DSS-Anforderung 11.5.1

**11.5.1** Implementieren Sie einen Prozess zur Reaktion auf Warnungen, die von der für die Änderungserkennung verwendeten Lösung generiert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Azure-Regeln zur Überwachung von Ereignissen bieten eine zusätzliche Überwachungsebene für Hochrisikovorgänge und stark gefährdete Ressourcen. Von Azure verwaltete Geräte werden auf Einhaltung bewährter Sicherheitsstandards überwacht. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Warnungen bei Änderungen werden von der Log Analytics-Implementierung bereitgestellt. Weitere Informationen finden Sie in den [PCI-Anleitungen für vorinstallierte Verwaltungslösungen](payment-processing-blueprint.md#management-solutions).<br /><br /><br /><br />|



## <a name="pci-dss-requirement-116"></a>PCI-DSS-Anforderung 11.6

**11.6** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für Sicherheitsüberwachung und -tests dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Warnungen bei Änderungen werden von der Log Analytics-Implementierung bereitgestellt. Weitere Informationen finden Sie in den [PCI-Anleitungen für vorinstallierte Verwaltungslösungen](payment-processing-blueprint.md#management-solutions).<br /><br /><br /><br />|





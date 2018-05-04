---
title: Azure Security and Compliance Blueprint – Automatisierung von Webanwendungen für FedRAMP – System- und Kommunikationsschutz
description: Automatisierung von Webanwendungen für FedRAMP – System- und Kommunikationsschutz
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: b96cdef1-ce3a-4f73-9a9e-f2cbd056d485
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 6749ad50cd1ea1cd4ec6ca2f86fef43a9f1515d9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="system-and-communications-protection-sc"></a>System- und Kommunikationsschutz (System and Communications, SC)

> [!NOTE]
> Diese Regelungen werden von NIST und dem U.S. Department of Commerce als Teil der NIST-Sonderveröffentlichung 800-53 Revision 4 definiert. Informationen zu Testverfahren und Anleitungen für jede Regelung finden Sie unter NIST 800-53 Rev. 4.

## <a name="nist-800-53-control-sc-1"></a>NIST 800-53, Regelung SC-1

#### <a name="system-and-communications-protection-policy-and-procedures"></a>System- und Kommunikationsschutz – Richtlinien und Verfahren

**SC-1** Die Organisation entwickelt, dokumentiert und verbreitet an [Festsetzung: von der Organisation definierte Personen oder Rollen] eine Richtlinie für den System- und Kommunikationsschutz, die sich mit Zweck, Umfang, Rollen, Zuständigkeiten, Managementverpflichtung, Koordination zwischen Organisationsentitäten und Compliance befasst, stellt Verfahren zum Ermöglichen der Implementierung der Richtlinie für den System- und Kommunikationsschutz und der zugehörigen Regelungen für den System- und Kommunikationsschutz bereit und überprüft und aktualisiert die aktuelle Richtlinie für den System- und Kommunikationsschutz [Festsetzung: von der Organisation definierte Häufigkeit] sowie die Verfahren für den System- und Kommunikationsschutz [Festsetzung: von der Organisation definierte Häufigkeit].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Richtlinien und Verfahren des Kunden für den System- und Kommunikationsschutz auf Unternehmensebene können ausreichen, um dieser Regelung zu genügen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-2"></a>NIST 800-53, Regelung SC-2

#### <a name="application-partitioning"></a>Anwendungspartitionierung

**SC-2** Das Informationssystem trennt die Benutzerfunktionalität (einschließlich der Benutzeroberflächendienste) von den Funktionen zur Verwaltung des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint erzwingt logische Zugriffssteuerungen und die Systemarchitektur, um Benutzerfunktionen von Systemverwaltungsfunktionen zu trennen. Die Benutzerfunktionalität ist auf kundenseitig bereitgestellte Webanwendungsoberflächen beschränkt. Oberflächen für Funktionen der Systemverwaltung sind von den Benutzeroberflächen getrennt. Die gesamte Verwaltungskonnektivität erfolgt über einen sicheren Bastion Host (Jumpbox), der sich in einem verwalteten Subnetz befindet und dessen Regeln für Netzwerksicherheitsgruppen so gestaltet sind, dass der Zugriff auf Produktionsressourcen in geeigneter Weise eingeschränkt wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-3"></a>NIST 800-53, Regelung SC-3

#### <a name="security-function-isolation"></a>Isolation von Sicherheitsfunktionen

**SC-3** Das Informationssystem isoliert Sicherheitsfunktionen von nicht sicherheitsrelevanten Funktionen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows verwaltet für jeden ausgeführten Prozess eine eigene Ausführungsdomäne, indem jedem Prozess ein privater virtueller Adressraum zugewiesen wird. Darüber hinaus implementiert die Lösung eine Architektur sowie Zugriffskontrollen, die darauf ausgelegt sind, Sicherheitsfunktionen bei Bedarf zu isolieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-4"></a>NIST 800-53, Regelung SC-4

#### <a name="information-in-shared-resources"></a>Informationen in gemeinsam genutzten Ressourcen

**SC-4** Das Informationssystem verhindert eine nicht autorisierte und unbeabsichtigte Übertragung von Informationen über gemeinsam genutzte Systemressourcen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Das Betriebssystem verwaltet Ressourcen (z.B. Speicher), sodass Informationen nur für Benutzer und Rollen mit entsprechenden Berechtigungen zugänglich sind. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-5"></a>NIST 800-53, Regelung SC-5

#### <a name="denial-of-service-protection"></a>Schutz vor Denial-of-Service-Angriffen

**SC-5** Das Informationssystem schützt durch [Festsetzung: von der Organisation definierte Sicherheitsmaßnahmen] vor folgenden Arten von Denial-of-Service-Angriffen bzw. begrenzt deren Auswirkungen: [Festsetzung: von der Organisation definierte Arten von Denial-of-Service-Angriffen oder Verweise auf Quellen für derartige Informationen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt eine Application Gateway-Instanz bereit, die Web Application Firewall- und Lastenausgleichsfunktionen umfasst. Bereitgestellte virtuelle Computer unterstützen die Web- und Datenbankebene. Zudem wird Active Directory in einer skalierbaren Verfügbarkeitsgruppe bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-6"></a>NIST 800-53, Regelung SC-6

#### <a name="resource-availability"></a>Ressourcenverfügbarkeit

**SC-6** Das Informationssystem schützt die Verfügbarkeit von Ressourcen durch die Zuweisung von [Festsetzung: von der Organisation definierte Ressourcen] nach [Auswahl (mindestens ein Element); Priorität; Kontingent; [Festsetzung: von der Organisation definierte Sicherheitsmaßnahmen]].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Jeder Windows-Prozess stellt die Ressourcen bereit, die zum Ausführen eines Programms erforderlich sind. Die Ressourcenpriorität wird vom Betriebssystem verwaltet. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-7a"></a>NIST 800-53, Regelung SC-7.a

#### <a name="boundary-protection"></a>Schutz von Grenzen

**SC-7.a** Das Informationssystem überwacht und steuert die Kommunikation an der Außengrenze des Systems und an wichtigen internen Grenzen innerhalb des Systems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt eine Application Gateway-Instanz und einen Lastenausgleich bereit und konfiguriert Regeln für Netzwerksicherheitsgruppen, um das Pendeln an Außengrenzen und zwischen den internen Subnetzen zu steuern. Ereignis- und Diagnoseprotokolle zu Application Gateway, Lastenausgleich und Netzwerksicherheitsgruppen werden von Log Analytics gesammelt, um Kunden die Überwachung zu ermöglichen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-7b"></a>NIST 800-53, Regelung SC-7.b

#### <a name="boundary-protection"></a>Schutz von Grenzen

**SC-7.b** Das Informationssystem implementiert Subnetze für öffentlich zugängliche Systemkomponenten, die [Auswahl: physisch; logisch] von internen Organisationsnetzwerken getrennt sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Ressourcen in einer Architektur mit einem separaten Websubnetz, Datenbanksubnetz, Active Directory-Subnetz und Verwaltungssubnetz bereit. Subnetze werden logisch durch Regeln für Netzwerksicherheitsgruppen getrennt, die auf die einzelnen Subnetze angewendet werden, um den Datenverkehr zwischen den Subnetzen auf das für die System- und Verwaltungsfunktionen erforderliche Maß einzuschränken (sodass beispielsweise externer Datenverkehr keinen Zugriff auf die Datenbank-, Verwaltungs- oder Active Directory-Subnetze hat). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-7c"></a>NIST 800-53, Regelung SC-7.c

#### <a name="boundary-protection"></a>Schutz von Grenzen

**SC-7.c** Das Informationssystem stellt eine Verbindung mit externen Netzwerken oder Informationssystemen nur über verwaltete Schnittstellen her, die aus entsprechend einer organisatorischen Sicherheitsarchitektur angeordneten Grenzschutzeinrichtungen bestehen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt eine Application Gateway-Instanz bereit, um externe Verbindungen mit einer vom Kunden bereitgestellten Webanwendung zu verwalten. Externe Verbindungen für den Verwaltungszugriff sind auf einen Bastion Host bzw. eine Jumpbox beschränkt, die in einem Verwaltungssubnetz bereitgestellt wird, dessen Netzwerksicherheitsregeln externe Verbindungen auf autorisierte IP-Adressen beschränken. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-3"></a>NIST 800-53, Regelung SC-7 (3)

#### <a name="boundary-protection--access-points"></a>Schutz von Grenzen | Zugriffspunkte

**SC-7 (3)** Die Organisation begrenzt die Anzahl der externen Netzwerkverbindungen mit dem Informationssystem.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt zwei öffentliche IP-Adressen bereit: eine mit Application Gateway verknüpfte IP-Adresse und eine mit dem Bastion Host bzw. der Jumpbox für die Verwaltung verknüpften IP-Adresse. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-4a"></a>NIST 800-53, Regelung SC-7 (4).a

#### <a name="boundary-protection--external-telecommunications-services"></a>Schutz von Grenzen | Externe Telekommunikationsdienste

**SC-7 (4).a** Die Organisation implementiert eine verwaltete Schnittstelle für jeden externen Telekommunikationsdienst.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt zwei öffentliche IP-Adressen bereit: eine mit Application Gateway verknüpfte IP-Adresse und eine mit dem Bastion Host bzw. der Jumpbox für die Verwaltung verknüpften IP-Adresse. Die Verwaltung dieser Schnittstellen erfolgt über Software-Defined Networking. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-4b"></a>NIST 800-53, Regelung SC-7 (4).b

#### <a name="boundary-protection--external-telecommunications-services"></a>Schutz von Grenzen | Externe Telekommunikationsdienste

**SC-7 (4).b** Die Organisation legt für jede verwaltete Schnittstelle eine Datenverkehrsflussrichtlinie fest.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt zwei öffentliche IP-Adressen bereit: eine mit Application Gateway verknüpfte IP-Adresse und eine mit dem Bastion Host bzw. der Jumpbox für die Verwaltung verknüpften IP-Adresse. Die Verwaltung dieser Schnittstellen erfolgt über Software-Defined Networking. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-4c"></a>NIST 800-53, Regelung SC-7 (4).c

#### <a name="boundary-protection--external-telecommunications-services"></a>Schutz von Grenzen | Externe Telekommunikationsdienste

**SC-7 (4).c** Die Organisation schützt die Vertraulichkeit und Integrität der Informationen, die über die einzelnen Schnittstellen übertragen werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Das durch diesen Blueprint bereitgestellte Webanwendungsgateway wird mit einem HTTPS-Listener konfiguriert, wodurch die Vertraulichkeit und Integrität der Kommunikationssitzungen gewährleistet werden. Vertraulichkeit und Integrität werden auch durch verschlüsselte Remotedesktopverbindungen mit der Jumpbox bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-4d"></a>NIST 800-53, Regelung SC-7 (4).d

#### <a name="boundary-protection--external-telecommunications-services"></a>Schutz von Grenzen | Externe Telekommunikationsdienste

**SC-7 (4).d** Die Organisation dokumentiert jede Ausnahme für die Datenverkehrsflussrichtlinie mit einer unterstützenden unternehmenskritischen Anforderung bzw. Geschäftsanforderung sowie Dauer dieser Anforderung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Kunden sind nicht für Rechenzentrumsvorgänge (bestehend aus Telekommunikationsdiensten) verantwortlich. Alle Telekommunikationsdienste werden von Microsoft Azure bereitgestellt und verwaltet. Diese Regelung wird von Azure geerbt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-4e"></a>NIST 800-53, Regelung SC-7 (4).e

#### <a name="boundary-protection--external-telecommunications-services"></a>Schutz von Grenzen | Externe Telekommunikationsdienste

**SC-7 (4).e** Die Organisation überprüft Ausnahmen für die Datenverkehrsflussrichtlinie [Festsetzung: von der Organisation definierte Häufigkeit] und entfernt Ausnahmen, die nicht mehr durch eine explizite unternehmenskritische Anforderung bzw. Geschäftsanforderung unterstützt werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Kunden sind nicht für Rechenzentrumsvorgänge (bestehend aus Telekommunikationsdiensten) verantwortlich. Alle Telekommunikationsdienste werden von Microsoft Azure bereitgestellt und verwaltet. Diese Regelung wird von Azure geerbt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-5"></a>NIST 800-53, Regelung SC-7 (5)

#### <a name="boundary-protection--deny-by-default--allow-by-exception"></a>Schutz von Grenzen | Richtlinie zur standardmäßigen Verweigerung von Programmen und ausnahmebasierten Zulassung

**SC-7 (5)** Das Informationssystem verweigert über verwaltete Schnittstellen standardmäßig Netzwerkkommunikationsdatenverkehr und erlaubt diesen nur basierend auf Ausnahmen (d.h. einer Richtlinie zur Verweigerung aller Programme und ausnahmebasierten Zulassung).

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Regelsätze, die auf durch diesen Blueprint bereitgestellte Netzwerksicherheitsgruppen angewendet werden, werden mit einem Schema konfiguriert, das den Zugriff standardmäßig verweigert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-7"></a>NIST 800-53, Regelung SC-7 (7)

#### <a name="boundary-protection--prevent-split-tunneling-for-remote-devices"></a>Schutz von Grenzen | Unterbinden von getrenntem Tunneln für Remotegeräte

**SC-7 (7)** Das Informationssystem verhindert in Verbindung mit einem Remotegerät, dass das Gerät gleichzeitig andere Verbindungen als Remoteverbindungen mit dem System herstellt und über andere Verbindungsarten mit Ressourcen in externen Netzwerken kommuniziert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Richtlinie für die Konfiguration von Remotegeräten des Kunden auf Unternehmensebene kann getrenntes Tunneln umfassen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-8"></a>NIST 800-53, Regelung SC-7 (8)

#### <a name="boundary-protection--route-traffic-to-authenticated-proxy-servers"></a>Schutz von Grenzen | Weiterleiten von Datenverkehr an authentifizierte Proxyserver

**SC-7 (8)** Das Informationssystem leitet über authentifizierte Proxyserver mit verwalteten Schnittstellen [Festsetzung: von der Organisation definierten internen Kommunikationsdatenverkehr] an [Festsetzung: von der Organisation definierte externe Netzwerke] weiter.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Weiterleitung von kundenspezifischen Informationen über einen authentifiziertes Proxy an ein externes Netzwerk verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-10"></a>NIST 800-53, Regelung SC-7 (10)

#### <a name="boundary-protection--prevent-unauthorized-exfiltration"></a>Schutz von Grenzen | Vermeidung von nicht autorisierter Exfiltration

**SC-7 (10)** Die Organisation verhindert die nicht autorisierte Exfiltration von Informationen über verwaltete Schnittstellen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Organisation ist dafür verantwortlich, eine nicht autorisierte Exfiltration von Informationen über verwaltete Schnittstellen zu verhindern. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-12"></a>NIST 800-53, Regelung SC-7 (12)

#### <a name="boundary-protection--host-based-protection"></a>Schutz von Grenzen | Hostbasierter Schutz

**SC-7 (12)** Die Organisation implementiert [Festsetzung: von der Organisation definierte hostbasierte Schutzmechanismen für Grenzen] auf [Festsetzung: von der Organisation definierten Informationssystemkomponenten].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer werden mit einer aktivierten hostbasierten Firewall konfiguriert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-13"></a>NIST 800-53, Regelung SC-7 (13)

#### <a name="boundary-protection--isolation-of-security-tools--mechanisms--support-components"></a>Schutz von Grenzen | Isolation von Sicherheitstools, -mechanismen und unterstützenden Komponenten

**SC-7 (13)** Die Organisation isoliert [Festsetzung: von der Organisation definierte Informationssicherheitstools, -mechanismen und unterstützende Komponenten] von anderen internen Komponenten des Informationssystems, indem sie physisch getrennte Subnetze mit verwalteten Schnittstellen in anderen Systemkomponenten implementiert.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Ressourcen in einer Architektur mit einem separaten Verwaltungssubnetz für die kundenseitige Bereitstellung von Informationssicherheitstools und unterstützenden Komponenten bereit. Subnetze werden anhand von Regeln für Netzwerksicherheitsgruppen logisch getrennt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-18"></a>NIST 800-53, Regelung SC-7 (18)

#### <a name="boundary-protection--fail-secure"></a>Schutz von Grenzen | Sicherer Ausfall

**SC-7 (18)** Bei einem Betriebsausfall einer Grenzschutzeinrichtung fällt das Informationssystem auf sichere Weise aus.

**Zuständigkeiten:** `Azure Only`

|||
|---|---|
| **Kunde** | Zum Geltungsbereich der in Azure bereitgestellten Systeme gehören keine kundenseitig gesteuerten physischen Grenzschutzeinrichtungen. |
| **Anbieter (Microsoft Azure)** | Microsoft Azure stellt geographisch getrennte und redundante Gatewayserver und SSL-VPNs bereit. Wenn ein Gatewaysystem ausfällt, fällt es sicher aus, und der Zugriff ist auf die Umgebung beschränkt. Um eine Verbindung mit der Microsoft Azure-Umgebung herzustellen, muss ein Benutzer eine separate Verbindung mit einem aktiven von Microsoft Azure verwalteten Gatewayserver herstellen. <br /> Wenn Microsoft Azure-Netzwerkgeräte (einschließlich Edgerouter, als Zugriffspunkte fungierende Router, Lastenausgleich, Aggregationsswitches und TORS) ausfallen, wird der betroffene Schaltkreis getrennt und fällt so auf sichere Weise aus. So wird verhindert, dass bei einem Ausfall eines Microsoft Azure-Netzwerkgeräts externe Informationen in das Gerät gelangen oder Informationen unbefugt freigegeben werden. Dank der integrierten Redundanz können Microsoft Azure-Ressourcen ohne Beeinträchtigung der Verfügbarkeit ausfallen. |


 ### <a name="nist-800-53-control-sc-7-20"></a>NIST 800-53, Regelung SC-7 (20)

#### <a name="boundary-protection--dynamic-isolation--segregation"></a>Schutz von Grenzen | Dynamische Isolation und Trennung

**SC-7 (20)** Das Informationssystem bietet die Möglichkeit, [Festsetzung: von der Organisation definierte Komponenten des Informationssystems] von anderen Komponenten des Systems dynamisch zu isolieren bzw. zu trennen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, sicherzustellen, dass das System die kundenseitig bereitgestellten Ressourcen dynamisch isoliert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-7-21"></a>NIST 800-53, Regelung SC-7 (21)

#### <a name="boundary-protection--isolation-of-information-system-components"></a>Schutz von Grenzen | Isolation von Komponenten des Informationssystems

**SC-7 (21)** Die Organisation setzt Grenzschutzmechanismen ein, um [Festsetzung: von der Organisation definierte Informationssystemkomponenten], die [Festsetzung: von der Organisation definierte unternehmenskritische und/oder Geschäftsfunktionen] unterstützen, zu trennen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt Ressourcen in einer Architektur mit einem separaten Websubnetz, Datenbanksubnetz, Active Directory-Subnetz und Verwaltungssubnetz bereit. Subnetze werden logisch durch Regeln für Netzwerksicherheitsgruppen getrennt, die auf die einzelnen Subnetze angewendet werden, um den Datenverkehr zwischen den Subnetzen auf das für die System- und Verwaltungsfunktionen erforderliche Maß einzuschränken. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-8"></a>NIST 800-53, Regelung SC-8

#### <a name="transmission-confidentiality-and-integrity"></a>Vertraulichkeit und Integrität von übertragenen Informationen

**SC-8** Das Informationssystem schützt die [Auswahl (mindestens ein Element): Vertraulichkeit; Integrität] der übertragenen Informationen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | SI-8 (1) Die Implementierung erfüllt die Anforderung dieser Regelung. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-8-1"></a>NIST 800-53, Regelung SC-8 (1)

#### <a name="transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>Vertraulichkeit und Integrität von übertragenen Informationen | Kryptografischer oder alternativer physischer Schutz

**SC-8 (1)** Das Informationssystem implementiert kryptografische Mechanismen, um bei der Übertragung von Informationen [Auswahl (mindestens ein Element): eine unbefugte Offenlegung von Informationen zu verhindern; Änderungen an Informationen zu erkennen], sofern keine anderweitigen Sicherheitsvorkehrungen durch [Festsetzung: von der Organisation definierte alternative physische Schutzmaßnahmen] getroffen wurden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint konfiguriert Ressourcen so, dass nur sichere Protokolle für die Kommunikation verwendet werden. Die WAF-Komponente von Application Gateway ist dafür konfiguriert, Kommunikation von externen Benutzern über HTTPS/TLS anzunehmen und mit dem Back-End-Pool ausschließlich über HTTPS/TLS zu kommunizieren. SQL Server ist für die Kommunikation ausschließlich über HTTPS/TLS konfiguriert. Remotedesktopdienste werden für die Verwendung von sicheren Verbindungen konfiguriert. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-10"></a>NIST 800-53, Regelung SC-10

#### <a name="network-disconnect"></a>Netzwerktrennung

**SC-10** Das Informationssystem beendet am Ende der Sitzung oder nach [Festsetzung: von der Organisation definiertem Zeitraum] Inaktivität die Netzwerkverbindung einer Kommunikationssitzung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die Authentifizierung für Remotedesktopsitzungen wird von Active Directory verwaltet. Sobald der Zugriff für einen Benutzer in Active Directory deaktiviert ist, werden umgehend Remotesitzungen beendet. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-12"></a>NIST 800-53, Regelung SC-12

#### <a name="cryptographic-key-establishment-and-management"></a>Einrichtung und Verwaltung von Kryptografieschlüsseln

**SC-12** Die Organisation erstellt und verwaltet die im Informationssystem verwendeten Kryptografieschlüssel, die gemäß [Festsetzung: von der Organisation definierten Anforderungen in Bezug auf Schlüsselgenerierung, -verteilung, -speicherung, -zugriff und -zerstörung] für die Kryptografie erforderlich sind.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Dieser Blueprint stellt eine Azure Key Vault-Instanz bereit. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Azure Key Vault kann durch die Schlüsselgenerierungsfunktion eines mit FIPS 140-2 Level 2 konformen Hardwaresicherheitsmoduls (HSM) Schlüssel generieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-12-1"></a>NIST 800-53, Regelung SC-12 (1)

#### <a name="cryptographic-key-establishment-and-management--availability"></a>Einrichtung und Verwaltung von Kryptografieschlüsseln | Verfügbarkeit

**SC-12 (1)** Bei einem etwaigen Verlust von Kryptografieschlüsseln durch Benutzer stellt die Organisation die Verfügbarkeit der Informationen sicher.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Key Vault wird verwendet, um die in diesem Blueprint verwendeten Kryptografieschlüssel und Geheimnisse zu speichern. Key Vault optimiert den Schlüsselverwaltungsprozess für Schlüssel, die auf Daten zugreifen und diese verschlüsseln. In Key Vault werden folgende Authentifikatoren gespeichert: das Azure-Kennwort für das bereitgestellte Konto, das Administratorkennwort für den virtuellen Computer und das Kennwort für das SQL Server-Dienstkonto. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-12-2"></a>NIST 800-53, Regelung SC-12 (2)

#### <a name="cryptographic-key-establishment-and-management--symmetric-keys"></a>Einrichtung und Verwaltung von Kryptografieschlüsseln | Symmetrische Schlüssel

**SC-12 (2)** Die Organisation generiert, steuert und verteilt symmetrische Kryptografieschlüssel mithilfe von [Auswahl: NIST FIPS-konformen; NSA-zertifizierten] Schlüsselverwaltungstechnologien und -prozessen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Azure Key Vault wird zum Generieren, Steuern und Verteilen von Kryptografieschlüsseln verwendet. Azure Key Vault kann durch die Schlüsselgenerierungsfunktion eines mit FIPS 140-2 Level 2 konformen Hardwaresicherheitsmoduls (HSM) Schlüssel generieren. Schlüssel werden in sicher verschlüsselten Containern in Azure Key Vault gespeichert und verwaltet. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-12-3"></a>NIST 800-53, Regelung SC-12 (3)

#### <a name="cryptographic-key-establishment-and-management--asymmetric-keys"></a>Einrichtung und Verwaltung von Kryptografieschlüsseln | Asymmetrische Schlüssel

**SC-12 (3)** Die Organisation generiert, steuert und verteilt asymmetrische Kryptografieschlüssel unter Verwendung von [Auswahl: NSA-zertifizierten Schlüsselverwaltungstechnologien und -prozessen; genehmigten PKI-Zertifikaten der Klasse 3 oder vorgefertigtem Schlüsselerstellungsmaterial; genehmigten PKI-Zertifikaten der Klassen 3 oder 4 und Hardwaresicherheitstokens, die den privaten Schlüssel des Benutzers schützen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Generierung, Steuerung und Verteilung von asymmetrischen Kryptografieschlüsseln verantwortlich (sofern sie innerhalb von kundenseitig bereitgestellten Ressourcen verwendet werden). |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-13"></a>NIST 800-53, Regelung SC-13

#### <a name="cryptographic-protection"></a>Kryptografischer Schutz

**SC-13** Das Informationssystem implementiert [Festsetzung: von der Organisation definierte kryptografische Verwendungen und die Art der für die Verwendung erforderlichen Kryptografie] in Übereinstimmung mit den geltenden Bundesgesetzen, Executive Orders, Anordnungen, Richtlinien, Verordnungen und Normen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Von diesem Blueprint werden Windows-Authentifizierung, Remotedesktopdienste und BitLocker verwendet. Diese Komponenten können für die Nutzung von gemäß FIPS 140 überprüften Kryptografiemodulen konfiguriert werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-15a"></a>NIST 800-53, Regelung SC-15.a

#### <a name="collaborative-computing-devices"></a>Gemeinsame Computergeräte

**SC-15.a** Das Informationssystem verbietet die Remoteaktivierung von gemeinsamen Computergeräten mit folgenden Ausnahmen: [Festsetzung: von der Organisation definierte Ausnahmen, bei denen die Remoteaktivierung erlaubt sein soll].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine gemeinsamen Computergeräte bereitgestellt. Hinweis: Zum Geltungsbereich der in Azure bereitgestellten Systeme gehören keine physischen gemeinsamen Computergeräte. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-15b"></a>NIST 800-53, Regelung SC-15.b

#### <a name="collaborative-computing-devices"></a>Gemeinsame Computergeräte

**SC-15.b** Das Informationssystem zeigt Benutzern, die an den Geräten arbeiten, einen expliziten Hinweis zur Verwendung an.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine gemeinsamen Computergeräte bereitgestellt. Hinweis: Zum Geltungsbereich der in Azure bereitgestellten Systeme gehören keine physischen gemeinsamen Computergeräte. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-17"></a>NIST 800-53, Regelung SC-17

#### <a name="public-key-infrastructure-certificates"></a>Public Key-Infrastrukturzertifikate

**SC-17** Die Organisation stellt Public Key-Zertifikate im Rahmen einer [Festsetzung: von der Organisation definierte Zertifikatsrichtlinie] aus oder bezieht Public Key-Zertifikate von einem zugelassenen Dienstanbieter.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde muss für die Ausstellung von Zertifikaten eventuell eine Public Key-Infrastruktur auf Unternehmensebene einsetzen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-18a"></a>NIST 800-53, Regelung SC-18.a

#### <a name="mobile-code"></a>Mobiler Code

**SC-18.a** Die Organisation definiert akzeptable und inakzeptable mobile Codes und Technologien für mobile Codes.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die System- und Kommunikationsschutzverfahren des Kunden auf Unternehmensebene können akzeptable und inakzeptable mobile Codes definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-18b"></a>NIST 800-53, Regelung SC-18.b

#### <a name="mobile-code"></a>Mobiler Code

**SC-18.b** Die Organisation definiert Nutzungsbeschränkungen und Implementierungsrichtlinien für akzeptable mobile Codes und Technologien für mobile Codes.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die System- und Kommunikationsschutzverfahren des Kunden auf Unternehmensebene können Einschränkungen für die Verwendung von mobilen Codes definieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-18c"></a>NIST 800-53, Regelung SC-18.c

#### <a name="mobile-code"></a>Mobiler Code

**SC-18.c** Die Organisation autorisiert, überwacht und steuert die Verwendung von mobilen Codes innerhalb des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde muss möglicherweise einen Prozess zur Autorisierung, Überwachung und Steuerung der Nutzung mobiler Codes auf Unternehmensebene implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-19a"></a>NIST 800-53, Regelung SC-19.a

#### <a name="voice-over-internet-protocol"></a>Voice over IP

**SC-19.a** Die Organisation definiert Nutzungsbeschränkungen und Implementierungsrichtlinien für VoIP-Technologien (Voice over IP), die davon abhängig sind, welche Schäden diese am Informationssystem bei einer Verwendung mit böswilliger Absicht verursachen können.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine VoIP-Technologien (Voice over IP) bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-19b"></a>NIST 800-53, Regelung SC-19.b

#### <a name="voice-over-internet-protocol"></a>Voice over IP

**SC-19.b** Die Organisation autorisiert, überwacht und steuert die Verwendung von VoIP innerhalb des Informationssystems.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Im Rahmen dieses Blueprints werden keine VoIP-Technologien (Voice over IP) bereitgestellt. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-20a"></a>NIST 800-53, Regelung SC-20.a

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Sicherer Namens- und Adressauflösungsdienst (autoritative Quelle)

**SC-20.a** Das Informationssystem stellt zusätzliche Artefakte zur Datenursprungsauthentifizierung und -integritätsüberprüfung zusammen mit den autoritativen Namensauflösungsdaten zur Verfügung, die das System als Reaktion auf externe Anfragen zur Namens- und Adressauflösung zurückgibt.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz des Namens- und Adressauflösungsdienst verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-20b"></a>NIST 800-53, Regelung SC-20.b

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Sicherer Namens- und Adressauflösungsdienst (autoritative Quelle)

**SC-20.b** Das Informationssystem bietet die Möglichkeit, den Sicherheitsstatus von untergeordneten Zonen anzuzeigen und (wenn die untergeordnete Zone sichere Auflösungsdienste unterstützt) die Vertrauenskette zwischen über- und untergeordneten Domänen zu überprüfen, sofern diese als Teil eines verteilten, hierarchischen Namespaces betrieben werden.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für den Schutz des Namens- und Adressauflösungsdienst verantwortlich. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-21"></a>NIST 800-53, Regelung SC-21

#### <a name="secure-name--address-resolution-service-recursive-or-caching-resolver"></a>Sicherer Namens- und Adressauflösungsdienst (rekursiver oder Cachekonfliktlöser)

**SC-21** Das Informationssystem fordert die Datenursprungsauthentifizierung und -integritätsüberprüfung für die Antworten der Namens- und Adressauflösung, die das System von autoritativen Quellen erhält, an bzw. führt diese durch.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist für die Konfiguration von kundenseitig bereitgestellten Ressourcen verantwortlich, sodass die Datenursprungsauthentifizierung und -integritätsüberprüfung für Antworten von Namens- und Adressauflösungsdiensten, die von autoritativen Quellen empfangen werden, angefordert und durchgeführt werden. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-22"></a>NIST 800-53, Regelung SC-22

#### <a name="architecture-and-provisioning-for-name--address-resolution-service"></a>Architektur und Bereitstellung für den Namens- und Adressauflösungsdienst

**SC-22** Die Informationssysteme, die gemeinsam den Namens- und Adressauflösungsdienst für eine Organisation bereitstellen, sind fehlertolerant und implementieren eine interne bzw. externe Rollentrennung.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, sicherzustellen, dass die Systeme, die Adressauflösungsdienste für kundenseitig bereitgestellte Ressourcen zur Verfügung stellen, fehlertolerant sind und eine interne bzw. externe Rollentrennung implementieren. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-23"></a>NIST 800-53, Regelung SC-23

#### <a name="session-authenticity"></a>Echtheit von Sitzungen

**SC-23** Das Informationssystem schützt die Echtheit von Kommunikationssitzungen.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Remotezugriff auf Ressourcen, die durch diesen Blueprint bereitgestellt werden (einschließlich Azure-Portal, Remotedesktopverbindung und Webanwendungsgateway), ist durch TLS geschützt. TLS stellt die Echtheit der Kommunikation auf Sitzungsebene sicher. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-23-1"></a>NIST 800-53, Regelung SC-23 (1)

#### <a name="session-authenticity--invalidate-session-identifiers-at-logout"></a>Echtheit von Sitzungen | Invalidierung von Sitzungs-IDs bei der Abmeldung

**SC-23 (1)** Das Informationssystem macht Sitzungs-IDs ungültig, wenn sich der Benutzer abmeldet oder die Sitzung anderweitig beendet wird.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Remotezugriff auf Ressourcen, die durch diesen Blueprint bereitgestellt werden (einschließlich Azure-Portal, Remotedesktopverbindung und Webanwendungsgateway), ist durch TLS geschützt. Das Azure-Portal und Remotedesktopsitzungen machen Sitzungs-IDs bei der Abmeldung ungültig. Die Invalidierung von Websitzungen wird durch WAF-Regeln (Web Application Firewall) von Azure Application Gateway erzwungen. Die WAF wendet eine sitzungsbasierte Cookieaffinität an und führt nach 30 Minuten Inaktivität des Clients ein Sitzungstimeout durch. (Der Zeitraum kann nach der Bereitstellung entsprechend den organisationsspezifischen Regeln konfiguriert werden.) |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-24"></a>NIST 800-53, Regelung SC-24

#### <a name="fail-in-known-state"></a>Ausfall in bekanntem Zustand

**SC-24** Das Informationssystem fällt in einen [Festsetzung: von der Organisation definierten bekannten Zustand] bei [Festsetzung: von der Organisation definierten Arten von Ausfällen] aus, wobei [Festsetzung: von der Organisation definierte Systemstatusinformationen] bei einem Ausfall erhalten bleiben.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Der Kunde ist dafür verantwortlich, sicherzustellen, dass kundenseitig bereitgestellte Ressourcen in einem bekannten Zustand ausfallen. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-28"></a>NIST 800-53, Regelung SC-28

#### <a name="protection-of-information-at-rest"></a>Schutz von ruhenden Informationen

**SC-28** Das Informationssystem schützt die [Auswahl (mindestens ein Element): Vertraulichkeit; Integrität] von [Festsetzung: von der Organisation definierten ruhenden Informationen].

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | SI-28 (1) Die Implementierung erfüllt die Anforderung dieser Regelung. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ### <a name="nist-800-53-control-sc-28-1"></a>NIST 800-53, Regelung SC-28 (1)

#### <a name="protection-of-information-at-rest--cryptographic-protection"></a>Schutz von ruhenden Informationen | Kryptografischer Schutz

**SC-28 (1)** Das Informationssystem implementiert kryptografische Mechanismen, um eine unbefugte Offenlegung und Änderung von [Festsetzung: von der Organisation definierten Informationen] auf [Festsetzung: von der Organisation definierten Informationssystemkomponenten] zu verhindern.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer implementieren Datenträgerverschlüsselung, um die Vertraulichkeit und Integrität ruhender Informationen zu schützen. Die Azure-Datenträgerverschlüsselung für Windows wird mithilfe des BitLocker-Features von Windows implementiert. SQL Server ist für die Verwendung der TDE-Technologie (Transparent Data Encryption) konfiguriert, die Echtzeitverschlüsselung und -entschlüsselung von Daten und Protokolldateien ausführt, um ruhende Informationen zu schützen. TDE gewährleistet, dass gespeicherte Daten keinen unbefugten Zugriffen ausgesetzt sind. Kunden können bei Bedarf zusätzliche Kontrollmaßnahmen auf Anwendungsebene anwenden, um die Integrität der gespeicherten Informationen zu schützen. Die Vertraulichkeit und Integrität aller durch diesen Blueprint bereitgestellten Speicherblobs sind durch die Verwendung der Azure-Speicherdienstverschlüsselung (Storage Service Encryption, SSE) geschützt. Die SSE schützt ruhende Daten innerhalb von Azure-Speicherkonten mithilfe von 256-Bit-AES-Verschlüsselung. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |


 ## <a name="nist-800-53-control-sc-39"></a>NIST 800-53, Regelung SC-39

#### <a name="process-isolation"></a>Prozessisolation

**SC-39** Das Informationssystem verwaltet für jeden Ausführungsprozess eine eigene Ausführungsdomäne.

**Zuständigkeiten:** `Customer Only`

|||
|---|---|
| **Kunde** | Die durch diesen Blueprint bereitgestellten virtuellen Computer führen Windows-Betriebssysteme aus. Windows verwaltet für jeden ausgeführten Prozess eine eigene Ausführungsdomäne, indem jedem Prozess ein privater virtueller Adressraum zugewiesen wird. |
| **Anbieter (Microsoft Azure)** | Nicht zutreffend |

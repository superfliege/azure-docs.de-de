---
title: Blueprint zur Azure-Zahlungsverarbeitung – Firewallanforderungen
description: PCI-DSS-Anforderung 1
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: b1935d88-acae-42f9-bc25-bb0766f876ab
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 4e04d6417f1468c1bafada1a93ab63a73e39653d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="firewall-requirements-for-pci-dss-compliant-environments"></a>Firewallanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-1"></a>PCI-DSS-Anforderung 1

**Installieren und Pflegen einer Firewallkonfiguration zum Schutz der Karteninhaberdaten**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Firewalls sind Geräte, die den Computerdatenverkehr zwischen den Netzwerken einer Entität (intern) und nicht vertrauenswürdigen Netzwerken (extern) sowie den Datenverkehr in und aus sensibleren Bereichen innerhalb der internen vertrauenswürdigen Netzwerke einer Entität steuern. Die Datenumgebung des Karteninhabers ist ein Beispiel für einen sensibleren Bereich innerhalb des vertrauenswürdigen Netzwerks einer Entität.
Eine Firewall überprüft den gesamten Netzwerkdatenverkehr und blockiert alle Übertragungen, die nicht den festgelegten Sicherheitskriterien entsprechen.
Alle Systeme müssen vor unbefugten Zugriffen aus nicht vertrauenswürdigen Netzwerken geschützt werden, sei es beim Betreten des Systems über das Internet als E-Commerce, beim Internetzugang der Mitarbeiter über Desktopbrowser, beim E-Mail-Zugang der Mitarbeiter, bei dedizierten Verbindungen wie Business-to-Business-Verbindungen, über drahtlose Netzwerke oder über andere Quellen. Oftmals können scheinbar unbedeutende Pfade zu und von nicht vertrauenswürdigen Netzwerken ungeschützte Pfade zu Schlüsselsystemen bereitstellen. Firewalls sind ein wichtiger Schutzmechanismus für jedes Computernetzwerk.
Andere Systemkomponenten können Firewallfunktionen bereitstellen, sofern sie die Mindestanforderungen für Firewalls gemäß Anforderung 1 erfüllen. Werden innerhalb der Datenumgebung des Karteninhabers andere Systemkomponenten zur Bereitstellung der Firewallfunktionen verwendet, so müssen diese Geräte in den Geltungsbereich und die Bewertung der Anforderung 1 einbezogen werden.

## <a name="pci-dss-requirement-11"></a>PCI-DSS-Anforderung 1.1

**1.1** Erstellen und implementieren Sie Firewall- und Routerkonfigurationsstandards, die Folgendes umfassen (siehe 1.1.1 bis 1.1.7).


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet Firewalls für die CDE mittels PaaS-Isolation, und eine Implementierung der App Service-Umgebung stellt sicher, dass Datenein- und -ausgang der CDE geschützt sind.<br /><br />Eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro) ist ein Premium-Service-Plan, der aus Konformitätsgründen genutzt wird. Weitere Informationen zu Steuerelementen und Konfigurationen der App Service-Umgebung finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-111"></a>PCI-DSS-Anforderung 1.1.1

**1.1.1** Ein formaler Prozess zum Genehmigen und Testen aller Netzwerkverbindungen und Änderungen an der Firewall sowie den Routerkonfigurationen.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Eine Contoso Webstore-Instanz erstellt ein CI/CD DevOps-Modell, um sicherzustellen, dass alle Änderungen ordnungsgemäß verwaltet werden. Log Analytics ermöglicht die umfassende Protokollierung von Änderungen. Änderungen können auf Ihre Richtigkeit hin überprüft werden. Genauere Anweisungen finden Sie in den [PCI-Anleitungen für die Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).<br /><br />[Azure Security Center](https://azure.microsoft.com/services/security-center/) bietet eine zentrale Ansicht der Sicherheitsstatus sämtlicher Azure-Ressourcen. Sie können auf einen Blick überprüfen, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und schnell Ressourcen ermitteln, die Ihre Aufmerksamkeit erfordern.|



### <a name="pci-dss-requirement-112"></a>PCI-DSS-Anforderung 1.1.2

**1.1.2** Aktuelles Netzwerkdiagramm, das alle Verbindungen zwischen der Datenumgebung des Karteninhabers und anderen Netzwerken, einschließlich drahtloser Netzwerke, identifiziert

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Beachten Sie die Referenzarchitektur und die Designdokumentation für den Contoso Webstore, die als Teil des Installationsschemas der Lösung bereitgestellt werden.|



### <a name="pci-dss-requirement-113"></a>PCI-DSS-Anforderung 1.1.3

**1.1.3** Aktuelles Diagramm, das alle Datenflüsse der Karteninhaber system- und netzwerkübergreifend darstellt.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Weitere Informationen finden Sie im DFD- und Bedrohungsmodell für den Contoso Webstore.|



### <a name="pci-dss-requirement-114"></a>PCI-DSS-Anforderung 1.1.4

**1.1.4** Anforderungen an eine Firewall für jede Internetverbindung und zwischen jeder entmilitarisierten Zone (Demilitarized Zone, DMZ) und der internen Netzwerkzone

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure setzt Geräte für den Schutz von Bereichsgrenzen wie Gateways, Netzwerk-ACLs und Anwendungsfirewalls ein, um die Kommunikation an externen und internen Grenzen auf Plattformebene zu steuern. Der Kunde konfiguriert diese dann nach seinen Vorgaben und Anforderungen. Microsoft Azure filtert die Kommunikation beim Betreten der Plattform. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet eine DMZ mittels PaaS-Isolation, und eine Implementierung der App Service-Umgebung stellt sicher, dass Datenein- und -ausgang der CDE geschützt sind.<br /><br />Eine [App Service-Umgebung (ASE)](/azure/app-service-web/app-service-app-service-environment-intro) ist ein Premium-Service-Plan, der aus Konformitätsgründen genutzt wird. Weitere Informationen zu Steuerelementen und Konfigurationen der App Service-Umgebung finden Sie in den [PCI-Anleitungen für die App Service-Umgebung](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-115"></a>PCI-DSS-Anforderung 1.1.5

**1.1.5** Beschreibung von Gruppen, Rollen und Verantwortlichkeiten für die Verwaltung von Netzwerkkomponenten

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verwendet die [rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](/azure/role-based-access-control/role-assignments-portal), um Benutzerrollen zu isolieren. Die rollenbasierte Zugriffssteuerung in Azure ermöglicht eine präzise Zugriffsverwaltung für Azure. Es gibt spezielle Konfigurationen für den Abonnementzugriff und den Zugriff auf Azure Key Vault.|



### <a name="pci-dss-requirement-116"></a>PCI-DSS-Anforderung 1.1.6

**1.1.6** Dokumentation der betrieblichen Rechtfertigung und Genehmigung der Nutzung aller zulässigen Dienste, Protokolle und Ports, einschließlich der Dokumentation der Sicherheitsmaßnahmen, die für die als unsicher geltenden Protokolle implementiert wurden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore öffnet im gesamten RA-Design nur die benötigten Ports und Protokolle. Details zum Datenfluss sind im DFD- und Bedrohungsmodell ersichtlich.|



### <a name="pci-dss-requirement-117"></a>PCI-DSS-Anforderung 1.1.7

**1.1.7** Anforderung zur Überprüfung von Firewall- und Routerregelsätzen mindestens alle sechs Monate

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Im Contoso Webstore werden die Firewallregelsätze überprüft, um sicherzustellen, dass keine unnötigen oder unbenutzten Regeln enthalten sind. Die Demoversion wird mit dem Ansatz der geringsten Rechte und minimalen Speicherbedarf bereitgestellt.|



## <a name="pci-dss-requirement-12"></a>PCI-DSS-Anforderung 1.2

**1.2** Erstellen Sie Firewall- und Routerkonfigurationen, die Verbindungen zwischen nicht vertrauenswürdigen Netzwerken und allen Systemkomponenten in der Datenumgebung des Karteninhabers einschränken. 

> [!NOTE]
> Ein „nicht vertrauenswürdiges Netzwerk“ ist jedes Netzwerk, das sich außerhalb der Netzwerke befindet, die zu der untersuchten Entität gehören, und/oder das außerhalb der Kontroll- oder Verwaltungsfähigkeit der Entität liegt.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die CDE des Contoso Webstores ist in der RA- und Bereitstellungsdokumentation definiert. Nicht vertrauenswürdige Netzwerke werden von vornherein ausgeschlossen.|



### <a name="pci-dss-requirement-121"></a>PCI-DSS-Anforderung 1.2.1

**1.2.1** Beschränken Sie den ein- und ausgehenden Datenverkehr auf das für die Datenumgebung des Karteninhabers erforderliche Maß, und verweigern Sie insbesondere jeglichen anderen Datenverkehr.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die CDE des Contoso Webstores ist in der RA- und Bereitstellungsdokumentation definiert. Nicht vertrauenswürdige Netzwerke werden von vornherein ausgeschlossen. Die Contoso Webstore-Demo konfiguriert die Microsoft Azure-Anwendungsfirewall so, dass nur bestimmte Bereiche von IP-Adressen auf Microsoft Azure-Dienste zugreifen können. Der Contoso Webstore bietet an allen CDE-Bereichsgrenzen eine Firewall, die alle Berechtigungen verweigert. Alle Konfigurationen werden bei der Erstinstallation der Bereitstellung durchgeführt.

> [!NOTE]
> Die App Service-Umgebung (ASE) wird in dieser Lösung verwendet, um die CDE zu isolieren. Es ist jedoch wichtig, dass Ihr qualifizierter Sicherheitsprüfer (Qualified Security Assessor, QSA) diese Lösung auswertet, da die ASE eine DMZ-Isolation implementiert, die es ermöglicht, ausgehende Verbindungen durch die ASE herzustellen. PCI-DSS erfordert, dass alle nicht benötigten ein- und ausgehenden Verbindungen blockiert werden müssen. Damit die ASE ordnungsgemäß funktioniert, stellt die ASE bei Bedarf ausgehende Verbindungen her, wie unter [„Überlegungen zum Netzwerkbetrieb in einer App Service-Umgebung“](/azure/app-service/app-service-environment/network-info) definiert. Kunden sollten die ausgehenden Verbindungen mit Ihrem qualifizierten Sicherheitsprüfer auswerten, bevor sie die Lösung in einer Produktionsumgebung einsetzen, um sicherzustellen, dass sie den Anforderungen entspricht. |



### <a name="pci-dss-requirement-122"></a>PCI-DSS-Anforderung 1.2.2

**1.2.2** Sichern und synchronisieren Sie Routerkonfigurationsdateien.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet Konfigurationen, die für native Microsoft Azure-Netzwerksteuerelemente synchronisiert sind.|



### <a name="pci-dss-requirement-123"></a>PCI-DSS-Anforderung 1.2.3

**1.2.3** Installieren Sie Umkreisfirewalls zwischen allen drahtlosen Netzwerken und der Datenumgebung des Karteninhabers, und konfigurieren Sie diese Firewalls so, dass sie nur autorisierten Datenverkehr zwischen der drahtlosen Umgebung und der Datenumgebung des Karteninhabers zulassen.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore verfügt über keine aktivierten drahtlosen Lösungen oder Funktionen.|



## <a name="pci-dss-requirement-13"></a>PCI-DSS-Anforderung 1.3

**1.3** Verbieten Sie den direkten öffentlichen Zugriff zwischen dem Internet und allen Systemkomponenten in der Datenumgebung des Karteninhabers.

**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure setzt netzwerk- und hostbasierte Geräte für den Schutz von Bereichsgrenzen wie Firewalls, Load Balancer und Zugriffssteuerungslisten ein. Diese Geräte verwenden Mechanismen wie VLAN-Isolation, NAT und Paketfilterung, um den Kundendatenverkehr vom Internet- und Verwaltungsdatenverkehr zu trennen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt zum Zeitpunkt der Bereitstellung die Konfigurationen der Azure-Anwendungsfirewall zur Verfügung, um nur bestimmte Bereiche von IP-Adressen für den Zugriff auf die Website zuzulassen, einschließlich der geschützten Azure VMs in ihrer CDE.|



### <a name="pci-dss-requirement-131"></a>PCI-DSS-Anforderung 1.3.1

**1.3.1** Implementieren Sie eine DMZ, um den eingehenden Datenverkehr auf Systemkomponenten zu beschränken, die autorisierte öffentlich zugängliche Dienste, Protokolle und Ports bereitstellen.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Implementierung der DMZ stellt sicher, dass nur autorisierte Dienste eine Verbindung mit der CDE herstellen können.|



### <a name="pci-dss-requirement-132"></a>PCI-DSS-Anforderung 1.3.2

**1.3.2** Beschränken Sie den eingehenden Internetdatenverkehr auf IP-Adressen innerhalb der DMZ.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Implementierung der DMZ stellt sicher, dass nur autorisierte Dienste eine Verbindung mit der CDE herstellen können.|



### <a name="pci-dss-requirement-133"></a>PCI-DSS-Anforderung 1.3.3

**1.3.3** Implementieren Sie Antispoofingmaßnahmen, um gefälschte Quell-IP-Adressen zu erkennen und zu blockieren. (Blockieren Sie z. B. den Datenverkehr aus dem Internet mit einer internen Quelladresse.)

**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Von Microsoft Azure wird eine Netzwerkfilterung implementiert, um gefälschten Datenverkehr zu verhindern und ein- wie ausgehenden Datenverkehr auf vertrauenswürdige Plattformkomponenten zu beschränken. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-134"></a>PCI-DSS-Anforderung 1.3.4

**1.3.4** Erlauben Sie keinen unbefugten ausgehenden Datenverkehr von der Datenumgebung des Karteninhabers ins Internet.


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Architektur verhindert unbefugten ausgehenden Datenverkehr von der im Bereich liegenden Umgebung zum Internet. Dies wird durch die Konfiguration von Zugriffssteuerungslisten für ausgehenden Datenverkehr für genehmigte Ports und Protokolle in Microsoft Azure erreicht. Diese Steuerelemente umfassen den Zugriff auf die CDE in der SQL Server-Datenbank. <br /><br />Eine PaaS-SQL-Datenbankinstanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-135"></a>PCI-DSS-Anforderung 1.3.5

**1.3.5** Erlauben Sie nur „eingerichtete“ Verbindungen ins Netzwerk.


**Zuständigkeiten:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Von Microsoft Azure wird eine Netzwerkfilterung implementiert, um gefälschten Datenverkehr zu verhindern und ein- wie ausgehenden Datenverkehr auf vertrauenswürdige Plattformkomponenten zu beschränken. Das Microsoft Azure-Netzwerk ist getrennt, um den kundenseitigen Datenverkehr vom Verwaltungsdatenverkehr zu trennen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Nicht zutreffend|



### <a name="pci-dss-requirement-136"></a>PCI-DSS-Anforderung 1.3.6

**1.3.6** Platzieren Sie Systemkomponenten, die Karteninhaberdaten (z. B. eine Datenbank) in einer internen Netzwerkzone, die von der DMZ und anderen nicht vertrauenswürdigen Netzwerken getrennt ist.


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure verwendet die Netzwerkisolierung und NAT, um den kundenseitigen Datenverkehr vom Verwaltungsdatenverkehr zu trennen. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Die Contoso Webstore-Architektur verhindert unbefugten ausgehenden Datenverkehr von der im Bereich liegenden Umgebung zum Internet. Dies wird durch die Konfiguration von Zugriffssteuerungslisten für ausgehenden Datenverkehr für genehmigte Ports und Protokolle in Microsoft Azure erreicht. Diese Steuerelemente umfassen den Zugriff auf die CDE in der SQL Server-Datenbank. <br /><br />Eine PaaS-SQL-Datenbankinstanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-137"></a>PCI-DSS-Anforderung 1.3.7

**1.3.7** Geben Sie keine privaten IP-Adressen und Routinginformationen an Unbefugte weiter. 

> [!NOTE]
> Folgende Methoden zur Verschleierung der IP-Adressierung können verwendet werden, sind aber nicht darauf beschränkt:
> - Netzwerkadressübersetzung (NAT)
> - Platzierung von Servern, die Karteninhaberdaten enthalten, hinter Proxyservern/Firewalls
> - Entfernung oder Filterung von Routenankündigungen für private Netzwerke mit registrierter Adressierung
> - Interne Verwendung des RFC1918-Adressraums anstelle von registrierten Adressen


**Zuständigkeiten:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Microsoft Azure verwendet die Netzwerkadressübersetzung (Network Address Translation, NAT) und Netzwerkisolierung, um den kundenseitigen Datenverkehr vom Verwaltungsdatenverkehr zu trennen. Azur-Geräte werden durch ihre UUID eindeutig identifiziert und mit Kerberos authentifiziert. Von Azure verwaltete Netzwerkgeräte werden durch RFC1918-IP-Adressen identifiziert. |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore platziert alle Karteninhaberdaten hinter Proxyservern bzw. Firewalls und verwendet intern den RFC1918-Adressraum.|



## <a name="pci-dss-requirement-14"></a>PCI-DSS-Anforderung 1.4

**1.4** Installieren Sie persönliche Firewallsoftware oder gleichwertige Funktionen auf allen tragbaren Computergeräten (einschließlich unternehmenseigener und/oder mitarbeitereigener Geräte), die sich außerhalb des Netzwerks mit dem Internet verbinden (z. B. Laptops, die von Mitarbeitern verwendet werden) und die auch für den Zugriff auf die CDE verwendet werden. Firewallkonfigurationen (oder gleichwertige Konfigurationen) enthalten: Definition bestimmter Konfigurationseinstellungen.
- Die persönliche Firewall (oder eine gleichwertige Funktionalität) wird aktiv ausgeführt.
- Die persönliche Firewall (oder eine gleichwertige Funktionalität) kann von den Benutzern der tragbaren Computergeräte nicht verändert werden.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore bietet keinen Schutz für Endbenutzergeräte. [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) kann verwendet werden, um die mobilen Geräte zu verwalten, mit denen Ihre Mitarbeiter auf Unternehmensdaten zugreifen.|



## <a name="pci-dss-requirement-15"></a>PCI-DSS-Anforderung 1.5

**1.5** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für die Verwaltung von Firewalls dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;– Plan)** | Der Contoso Webstore stellt zum Zeitpunkt der Bereitstellung die Konfigurationen der Azure-Anwendungsfirewall zur Verfügung, um nur bestimmte Bereiche von IP-Adressen für den Zugriff auf die Website zuzulassen, einschließlich der geschützten Azure VMs in ihrer CDE.|





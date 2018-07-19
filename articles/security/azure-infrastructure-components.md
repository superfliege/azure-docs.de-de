---
title: Komponenten und Grenzen des Azure-Informationssystems
description: Dieser Artikel enthält eine allgemeine Beschreibung der Microsoft Azure-Architektur und -verwaltung.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 8db1dce5fcc56c229d1fdd746bafbd2fae2c9bad
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101605"
---
# <a name="azure-information-system-components-and-boundaries"></a>Komponenten und Grenzen des Azure-Informationssystems
Dieser Artikel enthält eine allgemeine Beschreibung der Microsoft Azure-Architektur und -verwaltung. Die Azure-Systemumgebung besteht aus den folgenden Netzwerken:

- Microsoft Azure-Produktionsnetzwerk (Azure-Netzwerk)
- Microsoft-Unternehmensnetzwerk (Unternehmensnetzwerk)

Separate IT-Teams sind für Betrieb und Wartung des Azure-Netzwerks und der Unternehmensnetzwerke verantwortlich.

## <a name="azure-architecture"></a>Azure-Architektur
Microsoft Azure ist eine Cloud Computing-Plattform und -Infrastruktur für die Erstellung, Bereitstellung und Verwaltung von Anwendungen und Diensten über ein Netzwerk von Rechenzentren, die von Microsoft verwaltetet werden. Basierend auf der Anzahl der von Kunden angegebenen Ressourcen erstellt Azure je nach Ressourcenbedarf virtuelle Computer. Diese virtuellen Computer werden auf einem Microsoft Azure-Hypervisor ausgeführt, der für die Verwendung in der Cloud ausgelegt und nicht öffentlich zugänglich ist.

Auf jedem physischen Serverknoten von Azure befindet sich ein Hypervisor, der direkt über die Hardware ausgeführt wird. Hypervisor teilt einen Knoten in eine variable Anzahl von virtuellen Computern (VM). Darüber hinaus weist jeder Knoten eine spezielle Stamm-VM auf, auf der das Hostbetriebssystem ausgeführt wird. Die Windows-Firewall ist auf jedem virtuellen Computer aktiviert. Die einzigen offenen und intern oder extern adressierbaren Ports sind solche, die explizit in der vom Kunden konfigurierten Dienstdefinitionsdatei definiert wurden. Der gesamte Datenverkehr und der Zugriff auf Datenträger und Netzwerk werden durch den Hypervisor und das Stammbetriebssystem vermittelt.

Auf der Hostschicht führen virtuelle Azure-Computer eine angepasste Windows Server-Version mit verstärkter Sicherheit aus. Microsoft Azure verwendet eine abgespeckte Version von Windows Server, die nur die Komponenten einschließt, die zum Hosten von virtuellen Computern erforderlich sind. Dies wird sowohl zur Verbesserung der Leistung als auch zur Verringerung der Angriffsfläche verwendet. Computergrenzen werden vom Hypervisor erzwungen, was nicht von der Sicherheit des Betriebssystems abhängig ist.

**Azure-Verwaltung von Fabric-Controller (FCs)**: In Azure werden virtuelle Computer, die auf physischen Servern (Blades/Knoten) ausgeführt werden, in „Clustern“ von ca. 1000 gruppiert. Die virtuellen Computer werden unabhängig von einer horizontal hochskalierten und redundanten Plattformsoftwarekomponente namens FC verwaltete.

Jeder FC verwaltet den Lebenszyklus von Anwendungen, die in seinem Cluster ausgeführt werden, und führt die Bereitstellung und Überwachung der Integrität der Hardware unter seiner Kontrolle durch. Er führt autonome Vorgänge aus, z.B. Reaktivierung der Instanzen von virtuellen Computern auf fehlerfreien Servern, wenn festgestellt wird, dass ein Server fehlgeschlagen ist. Der FC führt auch Anwendungsverwaltungsvorgänge durch, z.B. Bereitstellung, Aktualisierung und horizontale Hochskalierung von Anwendungen.

Das Rechenzentrum ist in Cluster unterteilt. Cluster isolieren Fehler auf FC-Ebene und verhindern, dass bestimmte Fehlerklassen Auswirkungen auf Servern über den Server Cluster hinaus haben, in dem sie auftreten. FCs, die einen bestimmten Azure-Cluster bedienen, werden in einem FC-Cluster gruppiert.

**Hardwarebestand**: Eine Bestandsaufnahme von Azure-Hardware und -Netzwerkgeräten wird während des Bootstrapkonfigurationsprozesses erstellt und in der Konfigurationsdatei datacenter.xml dokumentiert. Neue Hardware und Netzwerkkomponenten, die in die Azure-Produktionsumgebung aufgenommen werden, müssen dem Bootstrapkonfigurationsprozess folgen. Der FC ist verantwortlich für die Verwaltung des gesamten Bestands in der Konfigurationsdatei datacenter.xml.

**FC-verwaltetes Betriebssystem**: Das Betriebssystemteam stellt Betriebssystemimages in Form von virtuellen Datenträgern zur Verfügung, die auf allen virtuellen Host- und Gast-VMs in der Azure-Produktionsumgebung bereitgestellt werden. Das Betriebssystemteam erstellt diese „Basisimages“ über einen automatisierten Offlinebuildprozess. Das Basisimage ist eine Version des Betriebssystems, in dem der Kernel und andere Kernkomponenten geändert und optimiert wurden, um die Azure-Umgebung zu unterstützen.

Es gibt drei Arten von durch Fabric verwaltete Betriebssystemimages:

- Hostbetriebssystem: Ein angepasstes Betriebssystem, das auf Host-VMs ausgeführt wird
- Natives Betriebssystem: Wird auf Mandanten ausgeführt (z.B. Azure Storage), die keinen Hypervisor besitzen
- Gastbetriebssystem: Wird auf Gast-VMs ausgeführt

FC-verwaltete Host- und native Betriebssysteme sind für die Verwendung in der Cloud ausgelegt und nicht öffentlich zugänglich.

**Host- und natives Betriebssystem**:: Hostbetriebssystem und natives Betriebssystem sind Betriebssystemimages mit verstärkter Sicherheit, die die Fabric-Agents (FA) hosten und Computeknoten (wird als erster virtueller Computer auf dem Knoten ausgeführt) und Speicherknoten ausführen. Die Vorteile der Verwendung optimierter Basisimages des Host- und des nativen Betriebssystems sind die Verringerung der Oberfläche, die durch APIs verfügbar gemacht wird, oder nicht verwendeter Komponenten, die hohe Sicherheitsrisiken darstellen und den Speicherbedarf des Betriebssystems erhöhen. Die Betriebssysteme mit niedrigerem Speicherbedarf enthalten nur die Komponenten, die in Azure erforderlich sind. Dies verbessert die Leistung und verringert die Angriffsfläche.

**Gastbetriebssystem**: Interne Azure-Komponenten, die auf virtuellen Computern mit Gastbetriebssystem ausgeführt werden, haben anders als externe Kunden nicht die Möglichkeit, das Remote Desktop Protocol (RDP) auszuführen. Alle Änderungen an den Konfigurationseinstellungen müssen die Prozesse des Change Managements und des Release Managements durchlaufen.

## <a name="azure-datacenters"></a>Azure-Rechenzentren
Das Microsoft Cloud Infrastructure and Operations- (MCIO-)Team verwaltet die physische Infrastruktur und die Rechenzentrumseinrichtungen für alle Onlinedienste von Microsoft. MCIO ist primär für die Verwaltung der physischen und Umweltsteuerungssysteme in Rechenzentren zuständig sowie für die Verwaltung und Unterstützung der Geräte des äußeren Umkreisnetzwerks (Edge-Router und Rechenzentrumsrouter). MCIO ist auch für die Einrichtung der erforderlichen Mindestserverhardware auf Racks im Rechenzentrum zuständig. Kunden interagieren nicht direkt mit Azure.

## <a name="service-management--service-teams"></a>Servicemanagement und Serviceteams
Die Unterstützung des Azure-Diensts wird durch eine Reihe von Engineeringgruppen, den „Serviceteams“, verwaltet. Jedes Serviceteam ist für einen Bereich von Support für einen Supportbereich für Azure zuständig. Jedes Serviceteam muss rund im die Uhr einen Techniker zur Verfügung stellen, der Fehler im Dienst untersucht und behebt. Serviceteams haben nicht standardmäßig Zugang zu der Hardware, die in Azure ausgeführt wird.

Serviceteams sind:

- Anwendungsplattform
- Azure Active Directory
- Azure Compute
- Azure Net
- Cloud-Engineeringdienste
- ISSD: Sicherheit
- Mehrstufige Authentifizierung
- SQL-Datenbank
- Speicher

## <a name="types-of-users"></a>Benutzertypen
Alle internen Azure-Benutzer müssen ihren Mitarbeiterstatus mit einer Vertraulichkeitsstufe kategorisieren lassen, die ihren Zugriff auf Kundendaten (Zugriff oder kein Zugriff) definiert. Mitarbeiter (oder Auftragnehmer) von Microsoft gelten als interne Benutzer. Alle anderen Benutzer werden als externe Benutzer betrachtet. Benutzerberechtigungen in Azure (Autorisierungsberechtigung nach der Authentifizierung) werden in der folgenden Tabelle beschrieben:

| Rolle | Intern oder extern | Vertraulichkeitsstufe | Autorisierte Berechtigungen und ausgeführte Funktionen | Zugriffstyp
| --- | --- | --- | --- | --- |
| Azure-Rechenzentrumstechniker | Intern | Kein Zugriff auf Kundendaten | Verwalten der physischen Sicherheit des Standorts; Durchführen der Kontrollgänge innerhalb und außerhalb des Rechenzentrums und Überwachen aller Einstiegspunkte; Durchführen von Begleitdiensten innerhalb und außerhalb des Rechenzentrums für nicht sicherheitsüberprüftes Personal, das allgemeine Dienste erbringt (Bewirtung, Reinigung) oder IT-Arbeiten im Rechenzentrum durchführt; Durchführen von Routineüberwachung und -verwaltung der Netzwerkhardware; Durchführen von Incident Management und Problemlösungsarbeiten mithilfe einer Vielzahl von Tools; Durchführen von Routineüberwachung und -wartung der physischen Hardware in den Rechenzentren; Bedarfsgesteuerter Zugriff auf die Umgebung von Eigenschaftenbesitzern. Durchführen forensischer Untersuchungen, Protokollieren von Incidentberichten und Voraussetzen obligatorischer Sicherheitsschulungen und Richtlinienanforderungen; Operativer Besitz und Wartung kritischer Sicherheitstools, wie z.B. Scanner und Erfassung von Protokollen. | Beständiger Zugriff auf die Umgebung |
| Microsoft Azure-Incidentselektierung (Rapid Response-Engineers) | Intern | Zugriff auf Kundendaten | Verwalten der Kommunikation zwischen den Infrastrukturvorgängen, Support- und Azure Engineering-Teams; Selektierungsplattformincidents, Bereitstellungsprobleme und Dienstanforderungen. | Just-in-Time-Zugriff auf die Umgebung – mit eingeschränktem beständigem Zugriff auf Nicht-Kundensysteme |
| Microsoft Azure-Bereitstellungstechniker | Intern | Zugriff auf Kundendaten | Durchführen der Bereitstellung/des Upgrades von Plattformkomponenten, Software und geplanten Konfigurationsänderungen zur Unterstützung von Microsoft Azure. | Just-in-Time-Zugriff auf die Umgebung – mit eingeschränktem beständigem Zugriff auf Nicht-Kundensysteme |
| Microsoft Azure Customer Outage Support (Mandant) | Intern | Zugriff auf Kundendaten | Debuggen und Diagnostizieren von Plattformausfällen und Fehlern für die einzelnen Computemandanten und Microsoft Azure-Konten; Analysieren von Fehlern und Steuern kritischer Korrekturen von Plattform/Kunde, Steuern technischer Verbesserungen für die Unterstützung. | Just-in-Time-Zugriff auf die Umgebung – mit eingeschränktem beständigem Zugriff auf Nicht-Kundensysteme |
| Microsoft Azure Live Site Engineers (Überwachungsengineers) und Incident | Intern | Zugriff auf Kundendaten | Für die Diagnose und Behebung der Plattformintegrität mithilfe von Diagnosetools verantwortlich; Steuern von Korrekturen für Volumetreiber, Reparieren von Elementen als Ergebnis von Ausfällen und Unterstützung von Ausfallwiederherstellungsaktionen. | Just-in-Time-Zugriff auf die Umgebung – mit eingeschränktem beständigem Zugriff auf Nicht-Kundensysteme |
|Microsoft Azure-Kunden | Extern | N/V | N/V | N/V |

Azure verwendet eindeutige Bezeichner zur Authentifizierung von Organisationsbenutzern und Kunden (oder Prozesse, die im Namen der Organisationsbenutzer agieren) für alle Assets/Geräte, die Teil der Azure-Umgebung sind.

**Microsoft Azure-interne Authentifizierung**: Kommunikation zwischen internen Azure-Komponenten ist mit TLS-Verschlüsselung geschützt. In den meisten Fällen sind die X.509-Zertifikate selbstsigniert. Ausnahmen werden für Zertifikate mit Verbindungen, auf die von außerhalb der Azure-Netzwerks zugegriffen werden kann, und für die FCs vorgesehen. FCs haben Zertifikate, die von der Microsoft-Zertifizierungsstelle (CA) ausgestellt werden, hinter der eine vertrauenswürdige Stammzertifizierungsstelle steht. Dadurch kann problemlos ein Rollover für öffentliche FC-Schlüssel ausgeführt werden. Darüber hinaus werden öffentliche FC-Schlüssel von Microsoft-Entwicklungstools verwendet, sodass neue Anwendungsimages, die von Entwicklern gesendet werden, mit einem öffentlichen FC-Schlüssel verschlüsselt werden, um eingebettete vertrauliche Daten zu schützen.

**Microsoft Azure-Hardwaregeräteauthentifizierung**: Der FC verwaltet einen Satz von Anmeldeinformationen (Schlüssel und/oder Kennwörter), mit denen er sich bei verschiedenen Hardwaregeräten unter seiner Kontrolle authentifiziert. Das System, das zum Transportieren, Persistieren und Verwenden der neuen Anmeldeinformationen verwendet wird, soll verhindern, dass Azure-Entwickler, Administratoren und Sicherungsdienste/Mitarbeiter auf sensible, vertrauliche oder private Informationen zugreifen.

Auf dem öffentlichen Schlüssel der Masteridentität von FC basierende Verschlüsselung wird beim FC-Setup und bei FC-Neukonfigurationszeiten verwendet, um die für den Zugriff auf die Netzwerkhardwaregeräte verwendeten Anmeldeinformationen zu übertragen. Anmeldeinformationen werden vom FC bei Bedarf abgerufen und entschlüsselt.

**Netzwerkgeräte**: Netzwerkdienstkonten werden konfiguriert, indem das Azure-Netzwerkteam allen Microsoft Azure-Clients ermöglicht, für Netzwerkgeräte (Router, Switches und Lastenausgleich) zu authentifizieren.

## <a name="secure-service-administration"></a>Sichere Dienstverwaltung
Für Betriebspersonal von Microsoft Azure ist die Verwendung von sicheren Administratorarbeitsstationen erforderlich (Secure Admin Workstation, SAW); Kunden können ähnliche Steuerungen implementieren, indem Sie Arbeitsstationen mit privilegiertem Zugriff (Privileged Access Workstation oder PAW) verwenden. Der SAW-Ansatz ist eine Erweiterung der gut bewährten und empfohlenen Methode, bei den Administratoren das Admin- und das Benutzerkonto voneinander zu trennen. Bei dieser Methode wird ein persönlich zugewiesenes Administratorkonto verwendet, das von dem Standardbenutzerkonto des Benutzers getrennt ist. SAW baut auf dieser Kontotrennungsmethode auf, indem für solche sensiblen Konten eine vertrauenswürdige Arbeitsstation bereitgestellt wird.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie unter:

- [Azure-Einrichtungen, Gelände und physische Sicherheit](azure-physical-security.md)
- [Verfügbarkeit der Azure-Infrastruktur](azure-infrastructure-availability.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Produktionsvorgänge und Verwaltung von Azure](azure-infrastructure-operations.md)
- [Überwachen der Azure-Infrastruktur](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Schutz von Kundendaten in Azure](azure-protection-of-customer-data.md)

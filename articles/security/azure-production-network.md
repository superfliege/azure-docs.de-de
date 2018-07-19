---
title: Azure-Produktionsnetzwerk
description: Dieser Artikel enthält eine allgemeine Beschreibung des Microsoft Azure-Produktionsnetzwerks.
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
ms.openlocfilehash: 5c0bfae35464e73278a1efd9c04a03123bb9018a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101549"
---
# <a name="azure-production-network"></a>Azure-Produktionsnetzwerk
Zu den Benutzern des Azure-Produktionsnetzwerks gehören externe Kunden, die auf ihre eigenen Microsoft Azure-Anwendungen zugreifen, und interne Microsoft Azure-Supportmitarbeiter, die das Produktionsnetzwerk verwalten. In diesem Artikel werden die Sicherheitszugriffsmethoden und Schutzmechanismen besprochen, mit denen Verbindungen zum Azure-Produktionsnetzwerk hergestellt werden können.

## <a name="internet-routing-and-fault-tolerance"></a>Internetrouting und Fehlertoleranz
Eine global redundante interne und externe Domain Name Service-Infrastruktur von Microsoft Azure (WADNS), zusammen mit mehreren primären und sekundären Domain Name Service-Serverclustern, sorgt für die Fehlertoleranz. Gleichzeitig werden weitere Netzwerksicherheitssteuerungen von Microsoft Azure, wie NetScaler, verwendet, um Distributed Denial of Service-Angriffe (DDoS) zu verhindern und die Integrität der Microsoft Azure-DNS-Dienste zu schützen.

Die WADNS-Server befinden sich in mehreren Rechenzentren. Die WADNS-Implementierung enthält eine Hierarchie von sekundären/primären DNS-Servern für die öffentliche Auflösung der benutzerdefinierten Azure-Domänennamen. Die Domänennamen werden in der Regel in eine CloudApp.net-Adresse aufgelöst, die die virtuelle IP-Adresse (VIP) für den Dienst des Kunden umschließt. Die VIP-Adresse, die der internen DIP-Adresse (dedizierte IP) der Mandantenübersetzung entspricht, wird durch die Microsoft-Lastenausgleichsmodule geladen, die für diese VIP verantwortlich sind. Dieses Feature gibt es nur in Azure.

Azure wird in geografisch verteilten Azure-Rechenzentren in den USA gehostet und basiert auf hochmodernen Routingplattformen, die zuverlässige und skalierbare Architekturstandards nutzen. Einige wichtige Features sind:

- Multiprotokoll Label Switching-basierte (MLPS) Datenverkehrstechnik, die für eine effiziente Linkverwendung und eine sinnvolle Herabstufung des Diensts sorgen, sollte es zu einem Ausfall kommen.
- Netzwerke werden mit Redundanzarchitekturen bereitgestellt, die immer über den Anforderungen liegen.
- Extern werden Rechenzentren durch dedizierte Netzwerkverbindungen mit hoher Bandbreite unterstützt, die redundant Eigenschaften mit mehr als 1.200 Internetdienstanbietern global an mehreren Peeringpunkten verbinden und mehr als 2.000 Gbit/s an Edgekapazität zur Verfügung stellen.

Da Microsoft seine eigenen Netzwerkschaltungen zwischen Rechenzentren besitzt, kann Azure aufgrund dieser Features eine Netzwerkverfügbarkeit von mindestens 99,9% bieten, ohne auf die herkömmlichen Internetdienstanbieter zurückzugreifen.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Verbindung mit dem Produktionsnetzwerk und zugehörigen Firewalls
Entsprechend der Azure-Netzwerkrichtlinie für den Internetdatenverkehrsfluss wird Datenverkehr an das Azure-Produktionsnetzwerk geleitet, das sich im nächstgelegenen regionalen Rechenzentrum der USA befindet. Da die Produktionsrechenzentren von Azure über konsistente Netzwerkarchitektur und Hardware verfügen, gilt die folgende Beschreibung des Datenverkehrsflusses einheitlich für alle Rechenzentren.

Sobald der Internetdatenverkehr für Azure an das nächste Rechenzentrum weitergeleitet wurde, wird eine Verbindung mit den Accessroutern hergestellt. Diese Accessrouter dienen dazu, Datenverkehr zwischen Azure-Knoten und den von Kunden erzeugten VM-Instanzen zu isolieren. Netzwerkinfrastrukturgeräte an Zugriffs- und Edgestandorten fungieren als Grenzpunkte, auf die Eingangs- und Ausgangsfilter angewendet werden. Diese Router werden über eine mehrstufige ACL konfiguriert. Sie filtern unerwünschten Netzwerkdatenverkehr und begrenzen, falls erforderlich, die Datenübertragungsrate. Durch ACL berechtigter Datenverkehr wird an den Lastenausgleich weitergeleitet. Distributionrouter lassen nur durch Microsoft genehmigte IP-Adressen zu, bieten Spoofingschutz und anerkannte TCP-Verbindungen mithilfe von ACLs.

Geräte für den externen Lastenausgleich befinden sich hinter den Accessroutern und sind für die Netzwerkadressenübersetzung (NAT) von internet-routingfähigen IPs zu internen Azure-IPs verantwortlich. Sie übernehmen auch das Routing von Paketen an gültige produktionsinterne IP-Adressen und Ports und fungieren als ein Schutzmechanismus, um zu vermeiden, dass interner Adressraum des Produktionsnetzwerks verfügbar gemacht wird.

Standardmäßig erzwingt Microsoft Hypertext Transfer Protocol Secure (HTTPS) für die Übertragung des gesamten Datenverkehrs an die Webbrowser des Kunden, einschließlich aller Anmeldungen und des nachfolgenden Datenverkehrs. Die Verwendung von TLS v1.2 ermöglicht einen sicheren Tunnel für den Datenverkehr. ACLs für Access- und Corerouter stellen sicher, dass die Quelle des Datenverkehrs den Erwartungen entspricht.

Diese Architektur weist im Vergleich zu herkömmlicher Sicherheitsarchitektur einen entscheidenden Unterschied auf: Es gibt keine dedizierten Hardwarefirewalls, keine speziellen Geräte zur Angriffserkennung und Abwehr oder andere Sicherheitsanwendungen, wie man sie erwarten könnte, bevor eine Verbindung zur Azure-Produktionsumgebung hergestellt wird. Kunden erwarten diese Hardwarefirewallgeräte in der Regel im Azure-Netzwerk. Sie werden jedoch nicht innerhalb von Azure verwendet. Diese Sicherheitsfeatures sind stattdessen fast ausschließlich in der Software integriert, die die Azure-Umgebung ausführt. Sie bieten zuverlässige mehrschichtige Sicherheitsmechanismen, darunter auch Firewallfunktionen. Darüber hinaus lassen sich der Grenzbereich und die zugehörige, zunehmende Anzahl von sicherheitsrelevanten Geräten leichter verwalten und erfassen (siehe Abbildung oben), da sie von der Azure-Software verwaltet werden.

## <a name="core-security-and-firewall-features"></a>Wesentliche Sicherheits- und Firewallfeatures
Azure implementiert stabile Softwaresicherheits- und Firewallfeatures auf verschiedenen Ebenen, um Sicherheitsfunktionen zu erzwingen, die in einer herkömmlichen Umgebung den Schutz der grundlegenden Sicherheitsautorisierung gewährleisten.

### <a name="azure-security-features"></a>Sicherheitsfeatures von Azure
Azure verwendet hostbasierte Softwarefirewalls innerhalb des Produktionsnetzwerks. In der Azure-Hauptumgebung befinden sich mehrere grundlegende Sicherheits- und Firewallfeatures. Diese Sicherheitsfeatures spiegeln die umfassende Verteidigungsstrategie innerhalb der Azure-Umgebung wider. Die Kundendaten in Microsoft Azure werden durch die folgenden Firewalls geschützt:

**Hypervisorfirewall (Paketfilter):** Diese Firewall ist im Hypervisor implementiert und wird durch den FC-Agent konfiguriert. Die Firewall schützt den Mandanten, der auf dem virtuellen Computer ausgeführt wird, vor nicht autorisiertem Zugriff. Standardmäßig wird beim Erstellen eines virtuellen Computers sämtlicher Datenverkehr blockiert. Anschließend fügt der Fabric Controller-Agent dem Filter Regeln und Ausnahmen hinzu, um autorisierten Datenverkehr zuzulassen.

Es gibt zwei Kategorien von Regeln, die hier programmiert werden:

- Computerkonfigurations- oder Infrastrukturregeln: In der Standardeinstellung wird die gesamte Kommunikation blockiert. Es gibt Ausnahmen, damit ein VM Dynamic Host Configuration Protocol-Mitteilungen (DHCP) und DNS-Informationen erhalten und Datenverkehr an das „öffentliche“ Internet senden kann, und zwar an andere virtuelle Computer innerhalb des FC-Clusters und des Betriebssystem-Aktivierungsservers. Da die für den VM geltende Liste zulässiger ausgehender Ziele weder Microsoft Azure-Routersubnetze noch Microsoft-Eigenschaften enthält, stellen die Regeln eine Schutzebene für sie dar.
- Rollenkonfigurationsdatei: Diese definiert die eingehenden ACLs basierend auf dem Dienstmodell des Mandanten. Wenn ein Mandant beispielsweise auf einem bestimmten virtuellen Computer über ein Web-Front-End auf Port 80 verfügt, ist Port 80 für alle IP-Adressen geöffnet. Wenn die VM eine Workerrolle ausführt, steht diese Workerrolle nur für die VM zur Verfügung, die sich innerhalb desselben Mandanten befindet.

**Firewall des nativen Hosts:** Microsoft Azure-Fabric und Storage werden unter einem nativen Betriebssystem ohne Hypervisor ausgeführt. Daher wird die Windows-Firewall mit den beiden oben genannten Regelsätzen konfiguriert.

**Hostfirewall:** Die Hostfirewall dient zum Schutz des Hostbetriebssystems, auf dem der Hypervisor ausgeführt wird. Die Regeln werden so programmiert, dass nur dem FC und Jumpboxes die Kommunikation mit der Hostpartition über einen bestimmten Port gestattet wird. Die anderen Ausnahmen dienen zum Zulassen von DHCP-Antworten und DNS-Antworten. Azure verwendet eine Computerkonfigurationsdatei mit der Vorlage von Firewallregeln für die Hostpartition. Es gibt auch eine Firewallausnahme für Hosts, die VMs die Kommunikation mit Hostkomponenten und Wire- bzw. Metadatenservern über spezifische Protokolle/Ports ermöglicht.

**Gastfirewall:** Teil der Windows-Firewall des Gastbetriebssystems, die vom Kunden auf Kunden-VMs und -Speicher konfiguriert werden kann.

Zusätzliche integrierte Azure-Sicherheitsfeatures:

- Infrastrukturkomponenten sind zugewiesene IP-Adressen von dedizierten IP-Adressen (DIPs). Ein Angreifer im Internet kann keinen Datenverkehr an diese Adressen übertragen, da sie Microsoft nicht erreichen würden. Internet-Gatewayrouter filtern Pakete, die ausschließlich an interne Adressen adressiert sind, damit sie nicht in das Produktionsnetzwerk eindringen können. Die einzigen Komponenten, die Datenverkehr an virtuelle IP-Adressen zulassen, sind Lastenausgleichsmodule.
- Die Firewalls auf allen internen Knoten folgen drei wesentlichen Sicherheitserwägungen zur Architektur, die für jedes beliebige Szenario gelten:

   - Sie befinden sich hinter dem Load Balancer (Lastenausgleich) und akzeptieren Pakete von überall. Sie sind extern verfügbar und entsprechen den offenen Ports einer herkömmlichen Perimeterfirewall.
   - Sie akzeptieren Pakete nur von einer begrenzten Anzahl von Adressen. Dies ist Teil der umfassenden Verteidigungsstrategie gegen DoS-Angriffe. Solche Verbindungen werden kryptografisch authentifiziert.
   - Auf Firewalls kann nur von internen Knoten zugegriffen werden. In diesem Fall akzeptieren sie nur Pakete, die von einer Liste mit einzeln benannten Quell-IP-Adressen stammen. Dabei handelt es sich ausschließlich um DIPs im Azure-Netzwerk. Bei einem Angriff auf das Unternehmensnetzwerk könnten beispielsweise Anforderungen an diese Adressen weitergeleitet werden. Sie würden jedoch blockiert, wenn die Quelladresse des Pakets nicht auf dieser Liste im Azure-Netzwerk aufgeführt ist.
   - Der Accessrouter im Perimeter blockiert ausgehende Pakete mit einer Adresse im Azure-Netzwerk. Der Grund dafür sind die konfigurierten statischen Routen des Netzwerks.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure facilities, premises, and physical security (Azure-Einrichtungen, -Gelände und physische Sicherheit)](azure-physical-security.md)
- [Availability of Azure infrastructure (Verfügbarkeit der Azure-Infrastruktur)](azure-infrastructure-availability.md)
- [Azure information system components and boundaries (Komponenten und Grenzen des Azure-Informationssystems)](azure-infrastructure-components.md)
- [Azure-Netzwerkarchitektur](azure-infrastructure-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Azure production operations and management (Produktionsvorgänge und Verwaltung von Azure)](azure-infrastructure-operations.md)
- [Monitoring of Azure infrastructure (Überwachen der Azure-Infrastruktur)](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Protection of customer data in Azure (Schutz von Kundendaten in Azure)](azure-protection-of-customer-data.md)

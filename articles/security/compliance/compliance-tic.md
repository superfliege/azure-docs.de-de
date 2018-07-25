---
title: Leitfaden für eine vertrauenswürdige Internetverbindung mit Azure
description: Leitfaden für eine vertrauenswürdige Internetverbindung mit Azure und SaaS-Diensten
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: 9d71efa35713500911c67d1df15612b64c8e97da
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990828"
---
# <a name="trusted-internet-connection-guidance"></a>Leitfaden für eine vertrauenswürdige Internetverbindung (Trusted Internet Connection, TIC)

## <a name="background"></a>Hintergrund

Ziel der Initiative für vertrauenswürdige Internetverbindungen (Trusted Internet Connection, TIC) ist die Optimierung und Standardisierung der Sicherheit einzelner externer Netzwerkverbindungen, die derzeit von Bundesbehörden genutzt werden. Die Richtlinie wird im [Memorandum M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf) der US-Behörde OMB (Office of Management and Budget) beschrieben.

Das Programm für vertrauenswürdige Internetverbindungen (TIC-Programm) wurde im November 2007 vom OMB zur Verbesserung der Sicherheit in der Netzwerkumgebung von Behörden und der Funktionen zur Reaktion auf Incidents eingeführt. TIC wurde entwickelt, um Netzwerkanalysen des gesamten eingehenden und ausgehenden Datenverkehrs von „.gov“-Websites durchzuführen. So lassen sich spezifische Signaturen und musterbasierte Daten identifizieren und Verhaltensanomalien, wie z.B. Botnetaktivitäten, aufdecken. Die Behörden wurden beauftragt, ihre externen Netzwerkverbindungen zu konsolidieren und den gesamten Datenverkehr über die Angriffserkennungs- und Präventionsgeräte (bekannt als EINSTEIN) zu leiten, die an einer begrenzten Anzahl von Netzwerkendpunkten gehostet wurden (die als vertrauenswürdige Internetverbindungen bezeichnet werden).

Einfach ausgedrückt, die Behörden möchten mithilfe des TIC-Programms Folgendes erfahren:
- Wer ist in meinem Netzwerk (autorisiert oder nicht autorisiert)?
- Wann wird auf mein Netzwerk zugegriffen und warum?
- Auf welche Ressourcen wird zugegriffen?

Heute müssen alle externen Verbindungen von Behörden über eine vom OMB genehmigte vertrauenswürdige Internetverbindung geleitet werden. Bundesbehörden müssen am TIC-Programm teilnehmen, entweder als TIC Access Provider (TICAP) oder durch Beauftragung eines der größten Tier-1-Internetdienstanbieter, die als Managed Trusted Internet Protocol Service (MTIPS), d.h. als verwalteter vertrauenswürdiger Internetprotokolldienst, bezeichnet werden.  TIC umfasst obligatorische kritische Funktionen, die heute von der Behörde und dem MTIPS-Anbieter ausgeführt werden. In der aktuellen Version von TIC werden für jeden TICAP und MTIPS die EINSTEIN-Angriffserkennung der Version 2 und die EINSTEIN-Angriffspräventionsgeräte der Version 3 Accelerated (3A) eingesetzt. Zudem besteht eine Absichtserklärung zwischen der Behörde und dem Heimatschutzministerium (Department of Homeland Security, DHS), dass EINSTEIN-Funktionen in US-Bundessystemen eingesetzt werden.

Damit das Heimatschutzministerium das „.gov“-Netzwerk schützen kann, benötigt es Rohdatenfeeds des Netzwerkdatenflusses, um Incidents im gesamten Bundesunternehmen zu korrelieren und Analysen mit speziellen Tools durchzuführen. Router des Heimatschutzministeriums bieten die Möglichkeit, IP-Netzwerkdatenverkehr beim Passieren einer Schnittstelle in beide Richtungen zu sammeln. Durch die Analyse des Netzwerkdatenflusses kann ein Netzwerkadministrator z.B. die Quelle und das Ziel des Datenverkehrs oder die Klasse des Diensts bestimmen. Die Daten des Netzwerkverkehrs werden als „Daten ohne Inhalt“ (z.B. Header, Quell-IP-Adresse, Ziel-IP-Adresse, usw.) betrachtet und ermöglichen es dem Heimatschutzministerium, Informationen rund um den Inhalt zu erfahren, d.h. wer was und wie lange gemacht hat.

Die Initiative umfasst auch Sicherheitsrichtlinien, Leitfäden und Rahmenbedingungen, die eine lokale Infrastruktur voraussetzen. Da Regierungsbehörden in die Cloud wechseln, um Kosteneinsparungen, Betriebseffizienz und Innovationen zu erreichen, verlangsamen die Implementierungsanforderungen von TIC in einigen Fällen den Netzwerkdatenverkehr und begrenzen die Geschwindigkeit und Agilität, mit der Benutzer im Regierungsnetzwerk auf ihre cloudbasierten Daten zugreifen können.

Dieser Artikel beschreibt, wie Regierungsbehörden Microsoft Azure Government nutzen können, um die Konformität mit der TIC-Richtlinie für IaaS- und PaaS-Dienste zu erreichen.

## <a name="summary-of-azure-networking-options"></a>Zusammenfassung der Azure-Netzwerkoptionen

Es gibt drei Hauptoptionen für die Verbindung mit Azure-Diensten:

1. Direkte Internetverbindung: Verbinden Sie sich direkt über eine offene Internetverbindung mit den Azure-Diensten. Das Medium ist öffentlich, ebenso wie die Verbindung. Der Datenschutz erfolgt durch eine Verschlüsselung auf Anwendungs- und Transportebene. Die Bandbreite wird durch die Verbindung einer Website mit dem Internet begrenzt, und es können mehrere aktive Anbieter verwendet werden, um die Ausfallsicherheit zu gewährleisten.
1. Virtuelles privates Netzwerk: Verbinden Sie sich privat über ein VPN-Gateway mit Azure Virtual Network.
Das Medium ist öffentlich, da es die Standardinternetverbindung einer Website durchläuft, aber die Verbindung wird in einem Tunnel verschlüsselt, um den Datenschutz zu gewährleisten. Die Bandbreite ist abhängig von den VPN-Geräten und wird durch die gewählte Konfiguration begrenzt. Azure-Point-to-Site-Verbindungen sind in der Regel auf 100 MBit/s beschränkt, während Site-to-Site-Verbindungen auf 1,25 GBit/s beschränkt sind.
1. Microsoft ExpressRoute: ExpressRoute ist eine direkte Verbindung zu Microsoft-Diensten. Da die Verbindung über einen isolierten Fiber-Channel erfolgt, kann sie je nach Konfiguration öffentlich oder privat sein. Die Bandbreite ist typischerweise auf höchstens 10 GBit/s beschränkt.

Es gibt mehrere Möglichkeiten, die Anforderungen des Anhang H für vertrauenswürdige Internetverbindungen (Überlegungen zur Cloud) zu erfüllen, die im Dokument „Trusted Internet Connections (TIC) Reference Architecture Document, Version 2.0“ (Dokument zur TIC-Referenzarchitektur, Version 2.0) des US-Heimatschutzministeriums enthalten sind. Im gesamten Dokument wird der Leitfaden zum TIC-Programm des Heimatschutzministeriums als „TIC 2.0“ bezeichnet.

Um die Verbindung von einem Ministerium oder einer Behörde (Department/Agency, D/A) zu Azure oder Office 365 zu ermöglichen, ohne den Datenverkehr über deren vertrauenswürdige Internetverbindung zu leiten, muss der D/A-Benutzer einen verschlüsselten Tunnel und/oder eine dedizierte Verbindung zum Cloud-Dienstanbieter (Cloud Service Provider, CSP) verwenden. Die CSP-Dienste können sicherstellen, dass die Konnektivität zu den D/A-Cloudressourcen nicht dem öffentlichen Internet für den direkten Zugriff durch Regierungsangestellte zur Verfügung gestellt wird.

O365 ist mit TIC 2.0 Anhang H konform und verwendet entweder ExpressRoute mit aktiviertem [Microsoft-Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) oder eine Internetverbindung, die den gesamten Datenverkehr mit TLS 1.2 verschlüsselt.  D/A-Endbenutzer im D/A-Netzwerk können sich über ihr Behördennetzwerk und die TIC-Infrastruktur mit dem Internet verbinden. Der gesamte Remoteinternetzugriff auf O365 wird blockiert und über die Behörde geleitet. Der D/A-Benutzer kann sich auch über eine ExpressRoute-Verbindung mit Microsoft-Peering, einer Art öffentlichem Peering, mit O365 verbinden.  

Nur für Azure können die Optionen 2 (VPN) und 3 (ExpressRoute) diese Anforderungen erfüllen, wenn sie in Verbindung mit Diensten verwendet werden, die den Zugang zum Internet einschränken.

![Diagramm zu vertrauenswürdiger Internetverbindung und Microsoft](media/tic-diagram-a.png)

## <a name="how-azure-infrastructure-as-a-service-offerings-can-help-with-tic-compliance"></a>Unterstützung der TIC-Konformität durch Azure-Infrastructure-as-a-Service-Angebote

Die Einhaltung der TIC-Richtlinie mit IaaS ist relativ einfach, da Azure-Kunden das Routing in ihrem eigenen virtuellen Netzwerk verwalten.

Die wichtigste Voraussetzung für die Einhaltung der TIC-Referenzarchitektur ist, dass Ihr virtuelles Netzwerk (VNET) zu einer privaten Erweiterung des Netzwerks des Ministeriums bzw. der Behörde wird. Bei einer _privaten_ Erweiterung setzt die Richtlinie voraus, dass kein Datenverkehr das Netzwerk verlässt, außer über die lokale TIC-Netzwerkverbindung. Dieser Prozess wird als „Erzwingen von Tunneln“ bezeichnet, bei dem der gesamte Datenverkehr von jedem System in der CSP-Umgebung über ein lokales Gateway in einem Organisationsnetzwerk über die vertrauenswürdige Internetverbindung ins Internet geleitet wird.

Die TIC-Konformität von Azure-IaaS kann in zwei große Schritte unterteilt werden:

1. Konfiguration
1. Überwachung

### <a name="azure-iaas-tic-compliance-configuration"></a>Konfiguration der TIC-Konformität von Azure-IaaS

Um eine TIC-konforme Architektur mit Azure zu konfigurieren, müssen Sie zunächst den direkten Internetzugang zu Ihrem virtuellen Netzwerk verhindern und dann den Internetdatenverkehr über das lokale Netzwerk erzwingen.

#### <a name="prevent-direct-internet-access"></a>Verhindern eines direkten Zugriffs auf das Internet

Der Azure-IaaS-Netzwerkzugriff erfolgt über virtuelle Netzwerke, die aus Subnetzen bestehen, denen die Netzwerkschnittstellencontroller (Network Interface Controller, NIC) virtueller Computer zugeordnet sind.

Das einfachste Szenario zur Unterstützung der TIC-Konformität besteht darin, sicherzustellen, dass ein virtueller Computer oder eine Sammlung davon keine Verbindung zu externen Ressourcen herstellen kann. Die Trennung von externen Netzwerken kann mithilfe von Netzwerksicherheitsgruppen (NSGs) gewährleistet werden, mit denen der Datenverkehr zu einem oder mehreren NICs oder Subnetzen in Ihrem virtuellen Netzwerk gesteuert werden kann. Eine NSG enthält Regeln zur Zugriffssteuerung, die den Datenverkehr auf Grundlage der Richtung des Datenverkehrs, des Protokolls, der Quelladresse und des Quellports, und der Zieladresse und des Zielports zulässt oder verweigert. Die Regeln für eine NSG können jederzeit geändert werden, und die Änderungen werden auf alle zugeordneten Instanzen angewendet.  Weitere Informationen zum Erstellen einer NSG finden Sie in diesem Artikel unter [Erstellen einer Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-on-premises-network"></a>Erzwingen des Internetdatenverkehrs über das lokale Netzwerk

Azure erstellt automatisch Systemrouten und weist die Routen allen Subnetzen eines virtuellen Netzwerks zu. Sie können zwar keine Systemrouten erstellen oder entfernen, aber einige Systemrouten mit benutzerdefinierten Routen überschreiben. Azure erstellt Standardsystemrouten für jedes Subnetz und fügt zusätzliche optionale Standardrouten für spezifische Subnetze hinzu (bzw. jedes Subnetz, wenn Sie bestimmte Azure-Funktionen nutzen). Dieses Routing stellt sicher, dass der für das virtuelle Netzwerk bestimmt Datenverkehr auch innerhalb dieses virtuellen Netzwerks bleibt, die von der IANA festgelegten privaten Adressräume wie 10.0.0.0.0/8 entfallen (sofern nicht im Adressraum des virtuellen Netzwerks enthalten) und das „Notfall“-Routing von 0.0.0.0/0 zum Internetendpunkt des virtuellen Netzwerks erfolgt.

![Erzwingen von Tunneln für TIC](media/tic-diagram-c.png)

Damit der gesamte Datenverkehr tatsächlich die vertrauenswürdige D/A-Internetverbindung durchläuft, muss der gesamte Datenverkehr, der das virtuelle Netzwerk verlässt, über die lokale Verbindung geleitet werden.  Zum Anpassen von Routen können Sie entweder benutzerdefinierte Routen erstellen oder BGP-Routen (Border Gateway Protocol) zwischen Ihrem lokalen Netzwerkgateway und einem virtuellen Azure-Netzwerkgateway austauschen. Weitere Informationen zu benutzerdefinierten Routen finden Sie unter https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined. Weitere Informationen zum Border Gateway Protocol finden Sie ebenfalls unter https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol.

#### <a name="user-defined-routes"></a>Benutzerdefinierte Routen

Wenn Sie ein routenbasiertes virtuelles Netzwerkgateway verwenden, kann das Tunneln innerhalb von Azure durchgeführt werden, indem Sie eine benutzerdefinierte Route mit der Einstellung 0.0.0.0.0/0 für den Datenverkehr zum Weiterleiten an einen „nächsten Hop“ Ihres virtuellen Netzwerkgateways hinzufügen. Azure priorisiert benutzerdefinierte Routen gegenüber systemdefinierten Routen, was dazu führen würde, dass der gesamte Nicht-VNET-Datenverkehr an Ihr virtuelles Netzwerkgateway gesendet werden würde, das ihn dann an lokale Standorte weiterleiten kann. Einmal definiert, muss diese benutzerdefinierte Route mit allen vorhandenen oder neu erstellten Subnetzen in allen virtuellen Netzwerken in Ihrer Azure-Umgebung verknüpft werden.

![Benutzerdefinierte Routen und TIC](media/tic-diagram-d.png)

#### <a name="border-gateway-protocol"></a>Border Gateway Protocol

Wenn Sie ExpressRoute oder ein Border Gateway Protocol-fähiges (BGP) virtuelles Netzwerkgateway verwenden, ist BGP der bevorzugte Mechanismus zum Ankündigen von Routen. Mit einer von BGP angekündigten Route von 0.0.0.0.0/0 stellen ExpressRoute und BGP-fähige virtuelle Netzwerkgateways sicher, dass diese Standardroute auf alle Subnetze innerhalb Ihrer virtuellen Netzwerke angewendet wird.

### <a name="azure-iaas-tic-compliance-auditing"></a>TIC-Konformitätsprüfung mit Azure-IaaS

Azure bietet mehrere Möglichkeiten, die TIC-Konformität zu überprüfen.

#### <a name="effective-routes"></a>Effektive Routen

Zur Bestätigung, dass Ihre Standardroute weitergegeben wurde, können Sie die „effektiven Routen“ einer bestimmten VM, einem bestimmten NIC oder in einer Tabelle für benutzerdefinierte Routen überprüfen. Hierzu können Sie das Azure-Portal, wie unter https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal beschrieben, oder PowerShell, wie unter https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell beschrieben, verwenden. Das Blatt „Effektive Routen“ ermöglicht es Ihnen, die relevanten benutzerdefinierten Routen, die von BGP angekündigten Routen und die Systemrouten anzuzeigen, die für die jeweilige Entität gelten, wie unten dargestellt.

![Routen – Screenshot](media/tic-screen-1.png)

**Hinweis**: Sie können die effektiven Routen für einen NIC nur anzeigen, wenn diese mit einer laufenden VM verknüpft ist.

#### <a name="network-watcher"></a>Network Watcher

Azure Network Watcher bietet mehrere Tools, die zur Überprüfung der TIC-Konformität verwendet werden können.  Weitere Informationen zu Network Watcher finden Sie unter https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview.

##### <a name="network-security-groups-flow-logs"></a>Datenflussprotokolle für Netzwerksicherheitsgruppen 

Mit Azure Network Watcher können Sie Netzwerk-Datenflussprotokolle erfassen, die die Metadaten des umgebenden IP-Datenverkehrs anzeigen. Die Netzwerk-Datenflussprotokolle enthalten neben anderen Daten auch die Quell- und Zieladressen der Ziele. In Kombination mit Protokollen des virtuellen Netzwerkgateways, der lokalen Edgegeräte und/oder der vertrauenswürdigen Internetverbindung kann so überwacht werden, dass der gesamte Datenverkehr tatsächlich lokal weitergeleitet wird. 

## <a name="how-azure-platform-as-a-service-offerings-can-help-with-tic-compliance"></a>Unterstützung der TIC-Konformität durch Azure-Platform-as-a-Service-Angebote

Azure-PaaS-Dienste wie Azure Storage sind über eine im Internet erreichbare URL zugänglich. Jeder, der über genehmigte Anmeldeinformationen verfügt, kann von jedem Ort aus auf die Ressource zugreifen, ohne eine vertrauenswürdige Internetverbindung hierfür zu verwenden. Aus diesem Grund gehen viele Regierungskunden fälschlicherweise davon aus, dass Azure-PaaS-Dienste nicht der TIC-Richtlinie entsprechen. Tatsächlich ist die Einhaltung der TIC-Richtlinie bei vielen Azure-PaaS-Diensten durchaus möglich, indem die gleiche Architektur wie die der oben beschriebenen TIC-konformen IaaS-Umgebung verwendet wird, sofern sie mit einem virtuellen Netzwerk (VNET) verbunden werden können. Die Integration von Azure-PaaS-Diensten in ein Azure-VNET ermöglicht den privaten Zugriff auf den Dienst von diesem VNET. Zudem lässt sich ein benutzerdefiniertes Routing für 0.0.0.0/0 über benutzerdefinierte Routen oder BGP anwenden, wodurch sichergestellt wird, dass der gesamte internetgebundene Datenverkehr lokal über die TIC weitergeleitet wird.  Einige Azure-Dienste können mit den folgenden Mustern in VNETs integriert werden:

- **Bereitstellen einer dedizierten Instanz für den Dienst**: Immer mehr PaaS-Diensten können als dedizierte Instanzen bereitgestellt werden, deren Endpunkten mit VNETs verbunden sind. Beispielsweise kann eine App Service-Umgebung (ASE) im Modus „Isoliert“ bereitgestellt werden, sodass ihr Netzwerkendpunkt auf ein VNET beschränkt werden kann. Diese ASE kann dann viele Azure-PaaS-Dienste wie Web-Apps, APIs und Funktionen hosten.
- **VNET-Dienstendpunkte**: Immer mehr PaaS-Dienste bieten die Möglichkeit, ihren Endpunkt auf eine private VNET-IP-Adresse anstatt auf eine öffentliche Adresse zu verschieben.

Dienste, die die Bereitstellung von dedizierten Instanzen in einem VNET oder auf Dienstendpunkten ab Mai 2018 unterstützen, sind unten aufgeführt: *(Verfügbarkeit bezieht sich auf Azure Commercial; Verfügbarkeitsinformationen zu Azure Government stehen noch aus).

### <a name="service-endpoints"></a>Dienstendpunkte

|Dienst                   |Status            |
|--------------------------|------------------|
|Azure Key Vault            | Private Vorschau  |
|Cosmos DB                 | Private Vorschau  |
|Identity                  | Private Vorschau  |
|Azure Data Lake           | Private Vorschau  |
|SQL Postgres/MySQL       | Private Vorschau  |
|Azure SQL Data Warehouse  | Öffentliche Vorschau   |
|Azure SQL                 | Allgemein verfügbar               |
|Speicher                   | Allgemein verfügbar               |

### <a name="vnet-injection"></a>Einschleusung von VNET

|Dienst                            |Status            |
|-----------------------------------|------------------|
|Verwaltete SQL-Instanz               | Öffentliche Vorschau   |
|Azure Container Service (AKS)       | Öffentliche Vorschau   |
|Service Fabric                     | Allgemein verfügbar               |
|API Management                     | Allgemein verfügbar               |
|Azure Active Directory             | Allgemein verfügbar               |
|Azure Batch                        | Allgemein verfügbar               |
|ASE                                | Allgemein verfügbar               |
|Redis                              | Allgemein verfügbar               |
|HDI                                | Allgemein verfügbar               |
|Compute-VM-Skalierungsgruppe  | Allgemein verfügbar               |
|Compute-Clouddienst              | Allgemein verfügbar               |

### <a name="vnet-integration-details"></a>Details zur VNET-Integration

Das folgende Diagramm zeigt den allgemeinen Netzwerkdatenfluss für den Zugriff auf PaaS-Dienste unter Verwendung von VNET-Einschleusung und VNET-Diensttunneln.  Weitere Informationen zu Netzwerkdienstgateways, VNETs und Diensttags finden Sie unter https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags.

![PaaS-Konnektivitätsoptionen für TIC](media/tic-diagram-e.png)

1. Private Verbindung mit Azure über ExpressRoute. Das private Peering von ExpressRoute mit erzwungenen Tunneln wird verwendet, um ein Routing des gesamten VNET-Datenverkehrs des Kunden über ExpressRoute zurück an den lokalen Standort zu erzwingen. Microsoft-Peering ist nicht erforderlich.
2. Durch die Kombination aus Azure VPN Gateway und dem Microsoft-Peering von ExpressRoute kann die End-to-End-IPSec-Verschlüsselung zwischen Kunden-VNET und lokalem Edge überlagert werden. 
3. Die Netzwerkverbindung mit dem Kunden-VNET wird über Netzwerksicherheitsgruppen gesteuert, die es dem Kunden ermöglichen, den Zugriff basierend auf IP-Adressen, Ports und Protokollen zu gewähren bzw. zu verweigern.
4. Durch die Einrichtung eines Dienstendpunkts für den Dienst des Kunden wird das Kunden-VNET auf den PaaS-Dienst erweitert.
5. Der PaaS-Dienstendpunkt ist standardmäßig so gesichert, dass er den Zugriff innerhalb des Kunden-VNET verweigert und nur von bestimmten Subnetzen zulässt.  Die Standardeinstellung verweigert auch Verbindungen aus dem Internet.
6. Für alle anderen Azure-Dienste, die auf Ressourcen im Kunden-VNET zugreifen müssen, sollte einer der folgenden Punkte zutreffen:  
  a. Direkt im VNET bereitgestellt  
  b. Selektiv zugelassen, basierend auf der Richtlinie des jeweiligen Azure-Diensts

#### <a name="option-1-dedicated-instance-vnet-injection"></a>Option 1: Dedizierte Instanz – „VNET-Einschleusung“

Mithilfe der VNET-Einschleusung können Kunden dedizierte Instanzen eines bestimmten Azure-Diensts, wie z.B. HDInsight, selektiv in ihrem eigenen VNET bereitstellen. Dienstinstanzen werden in einem dedizierten Subnetz im VNet des Kunden bereitgestellt. Die VNET-Einschleusung ermöglicht den Zugriff auf die Dienstressourcen über Adressen, die nicht über das Internet geroutet werden.  Lokale Instanzen können über den VNET-Adressraum direkt über ExpressRoute oder Site-to-Site-VPN auf diese Dienstinstanzen zugreifen, anstatt Firewalls für den öffentlichen Internetadressraum zu öffnen. Mit einer dedizierten Instanz, die an einen Endpunkt angeschlossen ist, können die gleichen Strategien wie bei der IaaS-TIC-Konformität Anwendung finden, wobei das Standardrouting sicherstellt, dass der internetgebundene Datenverkehr auf ein lokales virtuelles Netzwerkgateway umgeleitet wird. Der eingehende und ausgehende Zugriff lässt sich zudem durch Netzwerksicherheitsgruppen für das jeweilige Subnetz weiter steuern.

![VNET-Einschleusung – Übersichtsdiagramm](media/tic-diagram-f.png)

#### <a name="option-2-vnet-service-endpoints"></a>Option 2: VNET-Dienstendpunkte 

Immer mehr mandantenfähige Azure-Dienste bieten die Möglichkeit zur Verwendung von Dienstendpunkten, eine alternative Methode zur Integration in Azure-VNETs. VNET-Dienstendpunkte erweitern Ihren VNET-IP-Adressraum und die Identität Ihres VNET auf den Dienst über eine direkte Verbindung.  Der Datenverkehr vom VNET zum Azure-Dienst bleibt dabei immer innerhalb des Azure-Backbonenetzwerks. Sobald ein Dienstendpunkt für einen Dienst aktiviert ist, können die Verbindungen zum Dienst über vom Dienst bereitgestellte Richtlinien auf dieses VNET beschränkt werden. Zugriffsüberprüfungen werden in der Plattform durch den Azure-Dienst erzwungen. Der Zugriff auf gesperrte Ressourcen wird nur gewährt, wenn die Anforderung aus dem zulässigen VNET bzw. Subnetz stammt und/oder die beiden IP-Adressen zulässig sind, die bei Nutzung von ExpressRoute zur Identifizierung Ihres lokalen Datenverkehrs verwendet werden. Auf diese Weise kann effektiv verhindert werden, dass eingehender bzw. ausgehender Datenverkehr den PaaS-Dienst direkt verlässt.

![Dienstendpunkte – Übersichtsdiagramm](media/tic-diagram-g.png)

## <a name="using-cloud-native-tools-for-network-situational-awareness"></a>Verwendung von nativen Cloudtools zur Erkennung von Netzwerksituationen

Azure bietet native Cloudtools, die Ihnen dabei helfen, Situationen bezüglich der Datenverkehrsflüsse Ihres Netzwerks richtig einzuschätzen. Sie sind nicht verpflichtet, die TIC-Richtlinie einzuhalten, aber sie können Ihre Sicherheitsfunktionen erheblich verbessern.

### <a name="azure-policy"></a>Azure Policy

Azure Policy (https://azure.microsoft.com/services/azure-policy/) ist ein Azure-Dienst, der Ihrer Organisation eine bessere Möglichkeit bietet, Konformitätsinitiativen zu überprüfen und durchzusetzen.  Derzeit ist der Dienst in der Public Preview in Azure Commercial Clouds verfügbar, aber noch nicht in Microsoft Azure Government. So können Kunden, die sich mit der vertrauenswürdigen Internetverbindung (TIC) auseinandersetzen, mit der Planung und Prüfung ihrer Policy-Regeln beginnen, um eine künftige Konformität sicherzustellen. Azure Policy ist auf die Abonnementebene ausgerichtet und bietet eine zentrale Schnittstelle für die Verwaltung von Initiativen, Richtliniendefinitionen, Überprüfungs- und Durchsetzungsergebnissen sowie Ausnahmen. Zusätzlich zu den vielen integrierten Azure Policy-Definitionen können Administratoren ihre eigenen benutzerdefinierten Definitionen über einfache JSON-Vorlagen definieren. Für viele Kunden empfiehlt Microsoft, Überprüfungen gegenüber Durchsetzungen möglichst vorzuziehen.

Die folgenden Beispielrichtlinien können für TIC-Konformitätsszenarien nützlich sein:

|Richtlinie  |Beispielszenario  |Startvorlage  |
|---------|---------|---------|
|Tabelle mit benutzerdefinierten Routen erzwingen |     Stellen Sie sicher, dass die Standardroute in allen virtuellen Netzwerken auf ein zulässiges virtuelles Netzwerkgateway für das Routing auf „Lokal“ zeigt. | https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table |
|Überprüfung, wenn Network Watcher nicht für Region aktiviert ist  | Stellen Sie sicher, dass Network Watcher für alle verwendeten Regionen aktiviert ist.  | https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled |
|NSG X in jedem Subnetz  | Stellen Sie sicher, dass eine NSG (oder ein Satz von zulässigen NSGs) mit blockiertem Internetdatenverkehr auf alle Subnetze in jedem VNET angewendet wird. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet |
|NSG X auf jedem Netzwerkadapter | Stellen Sie sicher, dass eine NSG mit blockiertem Internetdatenverkehr auf alle NICs auf allen VMs angewendet wird. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic |
|Zulässiges VNET für VM-Netzwerkschnittstellen verwenden  | Stellen Sie sicher, dass sich alle NICs in einem zulässigen VNET befinden. | https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics |
|Allowed locations (Zulässige Speicherorte) | Stellen Sie sicher, dass alle Ressourcen in Regionen mit konformen VNETs und Network Watcher-Konfiguration bereitgestellt werden.  | https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs |
|Nicht zulässige Ressourcentypen, z.B. öffentliche IP-Adressen  | Unterbinden Sie die Bereitstellung von Ressourcentypen, die über keinen Konformitätsplan verfügen. Beispielsweise könnte diese Richtlinie verwendet werden, um die Bereitstellung von Ressourcen für öffentliche IP-Adressen zu unterbinden. NSG-Regeln können zwar dazu verwendet werden, den eingehenden Internetdatenverkehr effektiv zu blockieren, aber indem Sie verhindern, dass öffentliche IP-Adressen verwendet werden, bieten Sie noch weniger Angriffsfläche.    | https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type  |

### <a name="azure-traffic-analyticshttpsazuremicrosoftcomen-inblogtraffic-analytics-in-preview"></a>Azure [Traffic Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/)

Traffic Analytics von Azure Network Watcher nutzt Datenflussprotokolle und andere Protokolle, um einen Überblick über den Netzwerkdatenverkehr zu erhalten. Diese Daten können nützlich sein, um die TIC-Konformität zu überprüfen und mögliche Problemstellen zu identifizieren. Mithilfe eines Übersichtsdashboards lässt sich schnell überprüfen, welche VMs mit dem Internet kommunizieren, sodass dann eine entsprechende Liste für das TIC-Routing bereitgestellt werden kann.

![Traffic Analytics – Screenshot](media/tic-traffic-analytics-1.png)

Sie können eine „Landkarte“ verwenden, um schnell die wahrscheinlichen physischen Ziele des Internetdatenverkehrs für Ihre VMs zu identifizieren und verdächtige Orte oder Musteränderungen zu ermitteln.

![Traffic Analytics – Screenshot](media/tic-traffic-analytics-2.png)

Über eine Netzwerktopologiekarte lassen sich bestehende virtuelle Netzwerke schnell erfassen:

![Traffic Analytics – Screenshot](media/tic-traffic-analytics-3.png)

### <a name="azure-network-watcher-next-hop"></a>Azure Network Watcher – nächster Hop

Netzwerke in Regionen, die von Network Watcher überwacht werden, können „Nächster Hop“-Tests durchführen. Diese ermöglichen einen einfachen portalbasierten Zugriff auf eine Quelle und ein Ziel für einen Beispiel-Netzwerkdatenfluss, für den Network Watcher das „Nächster Hop“-Ziel auflöst. Dies kann auf VMs und Beispiel-Internetadressen angewendet werden, um sicherzustellen, dass der „nächste Hop“ tatsächlich das virtuelle Netzwerkgateway ist.

![Network Watcher – nächster Hop](media/tic-network-watcher.png)

## <a name="conclusions"></a>Zusammenfassung

Der Zugriff auf Microsoft Azure, Office 365 und Dynamics 365 kann problemlos so konfiguriert werden, dass er die im Mai 2018 formulierten und definierten Richtlinien im TIC 2.0 Anhang H erfüllt.  Microsoft ist sich bewusst, dass sich diese Richtlinien ändern können, und wird Kunden bestmöglich dabei unterstützen, neue Richtlinien rechtzeitig einzuhalten, sobald diese veröffentlicht werden.

## <a name="appendix-tic-patterns-for-common-workloads"></a>Anhang: TIC-Muster für gängige Workloads

| Category (Kategorie) | Workload | IaaS | Dedizierter PaaS / VNET-Einschleusung  | Dienstendpunkte  |
|---------|---------|---------|---------|--------|
| Compute | Virtuelle Linux-Computer | JA | | |
| Compute | Virtuelle Windows-Computer | JA | | |
| Compute | Virtual Machine Scale Sets | JA | | |
| Compute | Azure-Funktionen | | über App Service-Umgebung (ASE) | |
| Web- und mobile Anwendungen | Interne Webanwendung | | über App Service-Umgebung (ASE) | |
| Web- und mobile Anwendungen | Interne mobile Anwendung | | über App Service-Umgebung (ASE) | |
| Web- und mobile Anwendungen | API-Apps | | über App Service-Umgebung (ASE) | |
| Container | Azure Container Service (ACS) | | | JA |
| Container | Azure Container Service (AKS)* | | | JA |
| Datenbank | SQL-Datenbank | | Verwaltete Azure SQL-Datenbank-Instanz* | Azure SQL |
| Datenbank | Azure Database for MySQL | | | JA |
| Datenbank | Azure Database for PostgreSQL | | | JA |
| Datenbank | SQL Data Warehouse | | | JA |
| Datenbank | Azure Cosmos DB | | | JA |
| Datenbank | Redis Cache | | JA | |
| Speicher | Blobs (in englischer Sprache) | JA | | |
| Speicher | Dateien | JA | | |
| Speicher | Warteschlangen | JA | | |
| Speicher | Tabellen | JA | | |
| Speicher | Datenträger | JA | | |

*: Public Preview in Azure Government, Stand: Mai 2018  
**: Private Preview in Azure Government, Stand: Mai 2018

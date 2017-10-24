---
title: "Überlegungen zur Planung für integrierte Azure Stack-Systeme | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie aktuell bei der Planung und Vorbereitung für Azure Stack mit mehreren Knoten vorgehen können."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 90f8fa1a-cace-4bfa-852b-5abe2b307615
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: twooley
ms.openlocfilehash: 790b65720a8457b780dc727b0f67fcd12aed31bc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Überlegungen zur Planung für integrierte Azure Stack-Systeme

*Gilt für: Integrierte Azure Stack-Systeme*

Wenn Sie an einem integrierten Azure Stack-System interessiert sind, sollten Sie einige der wichtigsten Überlegungen zur Planung hinsichtlich der Bereitstellung verstehen und wissen, wie das System in Ihr Rechenzentrum passt. Dieses Thema bietet eine allgemeine Übersicht über diese Überlegungen.

Wenn Sie ein integriertes System erwerben möchten, führt Sie Ihr OEM-Hardwareanbieter (Originalgerätehersteller) dabei ausführlich durch einen Großteil des Planungsprozesses. Er führt auch die eigentliche Bereitstellung durch.

## <a name="management-considerations"></a>Überlegungen zur Verwaltung

Azure Stack ist ein geschlossenes System, bei dem die Infrastruktur aus Berechtigungs- und Netzwerksicht abgeriegelt ist. Dabei werden Netzwerk-Zugriffssteuerungslisten (ACLs) angewendet, um sämtlichen nicht autorisierten eingehenden Datenverkehr und sämtliche unnötige Kommunikation zwischen Infrastrukturkomponenten zu blockieren. Dadurch soll nicht autorisierten Benutzern der Zugriff auf das System verwehrt werden.

Für die tägliche Verwaltung und den Betrieb besteht kein uneingeschränkter Administratorzugriff auf die Infrastruktur. Azure Stack-Operatoren müssen das System über das Administratorportal oder mithilfe von Azure Resource Manager (über PowerShell oder die REST-API) verwalten. Es gibt keinen Zugriff auf das System durch andere Verwaltungstools wie Hyper-V-Manager oder Failovercluster-Manager. Es kann keine Software von Drittanbietern (z. B. Agenten) innerhalb der Komponenten der Azure Stack-Infrastruktur installiert werden, um den Schutz des Systems zu unterstützen. Die Interoperabilität mit externer Verwaltungs- und Sicherheitssoftware erfolgt über PowerShell oder die REST-API.

Wenn für die Behandlung von Problemen, die nicht durch Schritte zur Warnungsvermittlung behoben werden, eine höhere Zugriffsebene erforderlich ist, müssen Sie mit dem Support zusammenarbeiten. Über den Support gibt es eine Methode, um temporär den uneingeschränkten Administratorzugriff für das System bereitzustellen, um komplexere Operationen durchführen zu können. 

## <a name="deploy-connected-or-disconnected"></a>Online- oder Offline-Bereitstellung
 
Sie können Azure Stack entweder bei vorhandener Verbindung mit dem Internet (und mit Azure) oder offline bereitstellen. Wenn Sie Azure Stack optimal nutzen möchten, einschließlich der Hybridszenarien zwischen Azure Stack und Azure, sollten Sie die Bereitstellung mit Azure-Verbindung wählen. In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen den Bereitstellungsmodi zusammengefasst.

| Bereich | Modus „Verbunden“ | Modus „Getrennt“ |
| -------- | ------------- | ----------|
| Identitätsanbieter | Azure Active Directory (Azure AD) oder Active Directory-Verbunddienste (AD FS) | Nur AD FS |
| Marketplace-Syndikation | Elemente direkt aus dem Azure Marketplace in den Azure Stack-Marketplace herunterladen | Erfordert die manuelle Verwaltung des Marketplace in Azure Stack |
| Lizenzierungsmodell | Nutzungs- oder kapazitätsbasiert | Nur kapazitätsbasiert|
| Patch und Update  | Updatepakete direkt in Azure Stack herunterladen | Erfordert Wechselmedien und ein separat verbundenes Gerät |
| | | |

Der Bereitstellungsmodus kann später nicht ohne erneute Bereitstellung des gesamten Systems geändert werden.

## <a name="identity-considerations"></a>Überlegungen zur Identität

### <a name="choose-identity-provider"></a>Auswählen des Identitätsanbieters

Sie müssen bedenken, welchen Identitätsanbieter Sie für die Bereitstellung von Azure Stack nutzen möchten, Azure AD oder AD FS. Sie können die Identitätsanbieter nach der Bereitstellung nicht wechseln, ohne das System komplett neu bereitstellen zu müssen.

Ihre Wahl des Identitätsanbieters hat keinen Einfluss auf virtuelle Computer des Mandanten, auf das Identitätssystem sowie auf von ihnen verwendete Konten oder darauf, ob sie einer Active Directory-Domäne usw. beitreten können. Das ist was anderes.

**Gründe für den Einsatz von Azure AD**

- Sie verfügen bereits über Investitionen in Azure AD (z. B. Azure oder Office 365).
- Sie möchten dieselbe Identität für Azure- und Azure Stack-Clouds verwenden.
- Sie möchten Szenarien mit mehreren Mandanten unterstützen, in denen Sie verschiedene Organisationen in derselben Azure Stack-Instanz hosten können.
- Sie möchten eine REST-basierte Verzeichnisverwaltung über Azure AD Graph verwenden, um Benutzer, Gruppen usw. über APIs bereitzustellen.

> [!NOTE]
> Sie können eine Azure Stack-Bereitstellung nicht gleichzeitig mit einer Azure AD-Instanz und einer vorhandenen AD FS-Instanz verbinden. Wenn Sie mit Azure AD bereitstellen und eine vorhandene AD FS-Instanz verwenden möchten, können Sie Ihre lokale AD FS-Instanz mit Azure AD zu einem Verbund zusammenschließen.

**Gründe für den Einsatz von AD FS**

- Es besteht keine oder nur teilweise eine Internetverbindung.
- Es müssen gesetzliche/staatliche Anforderungen erfüllt werden.
- Sie möchten für Konten von Operatoren und Benutzern Ihr eigenes Identitätssystem (z. B. das Active Directory Ihres Unternehmens) verwenden. Zu diesem Zweck können Sie einen Verbund mit einer vorhandenen AD FS-Instanz (unter Windows Server 2012, 2012 R2 oder 2016) erstellen, die durch Active Directory gesichert wird.
- Sie verfügen über keinerlei Investitionen in Azure und möchten Azure AD nicht verwenden.

> [!NOTE]
> Sie können mit Azure Stack nur mit einer anderen AD FS-Instanz einen Verbund erstellen, die durch Active Directory gesichert wird. Andere Identitätsanbieter werden nicht unterstützt. Wenn Sie über einen anderen Identitätsanbieter verfügen, den Sie mit Azure Stack verwenden möchten, können Sie stattdessen eine Azure AD-basierte Bereitstellung verwenden.

### <a name="ad-fs-and-graph-integration"></a>Integration von AD FS und Graph

Wenn Sie die Bereitstellung von Azure Stack mithilfe von AD FS als Identitätsanbieter gewählt haben, müssen Sie die AD FS-Instanz für Azure Stack über eine Verbundvertrauensstellung mit einer vorhandenen AD FS-Instanz integrieren. Dies gestattet Identitäten in einer vorhandenen Active Directory-Gesamtstruktur die Authentifizierung mit Ressourcen in Azure Stack.

Sie können auch den Graph-Dienst in Azure Stack mit dem vorhandenen Active Directory integrieren. Dadurch können Sie die rollenbasierte Zugriffssteuerung (RBAC) in Azure Stack verwalten. Wenn der Zugriff auf eine Ressource delegiert wird, sucht die Graph-Komponente das Benutzerkonto mithilfe des LDAP-Protokolls in der vorhandenen Active Directory-Gesamtstruktur.

Das folgende Diagramm zeigt den integrierten Datenverkehrsfluss von AD FS und Graph.
![Diagramm mit Datenverkehrsfluss für AD FS und Graph](media/azure-stack-planning-considerations/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lizenzierungsmodell

Sie müssen entscheiden, welches Lizenzierungsmodell Sie verwenden möchten. Für eine verbundene Bereitstellung können Sie entweder die nutzungs- oder die kapazitätsbasierte Lizenzierung auswählen. Die nutzungsbasierte Lizenzierung erfordert eine Verbindung mit Azure, um die Nutzungsdaten zu melden, die dann über Azure Commerce abgerechnet werden. Bei einer Bereitstellung ohne Internetverbindung wird nur die kapazitätsbasierte Lizenzierung unterstützt. Weitere Informationen zu den Lizenzierungsmodellen finden Sie unter [Microsoft Azure Stack – Verpackung und Preise](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Entscheidungen zur Benennung

Sie müssen entsprechende Überlegungen zur Planung Ihres Azure Stack-Namespace anstellen, insbesondere zum Regionsnamen und zum externen Domänennamen. Der vollqualifizierte Domänennamen (FQDN) der Azure Stack-Bereitstellung für öffentliche Endpunkte ist eine Kombination aus den folgenden beiden Namen: &lt;*Region*&gt;&lt;*Exterer_FQDN*&gt;, z. B. *east.cloud.fabrikam.com*. In diesem Beispiel wären die Azure Stack-Portale unter den folgenden URLs verfügbar:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

In der folgenden Tabelle sind diese Entscheidungen zur Domänenbenennung zusammengefasst.

| Name | Beschreibung | 
| -------- | ------------- | 
|Regionsname | Der Name Ihres ersten Azure Stack-Region. Dieser Name wird als FQDN-Komponente für die öffentlichen virtuellen IP-Adressen (VIPs) verwendet, die von Azure Stack verwaltet werden. Der Regionsname gibt in der Regel eine physische Standort-ID an, z. B. den Standort des Rechenzentrums. | 
| Externer Domänenname | Der Name der DNS-Zone (Domain Name System) für Endpunkte mit externen VIPs. Wird im FQDN für diese öffentlichen VIPs verwendet. | 
| Privater (interner) Domänenname | Der Name der Domäne (und internen DNS-Zone), die in Azure Stack zur Infrastrukturverwaltung erstellt wurde. 
| | |

## <a name="certificate-requirements"></a>Zertifikatanforderungen

Für die Bereitstellung müssen Sie SSL-Zertifikate (Secure Sockets Layer) für öffentliche Endpunkte anbieten. Auf allgemeiner Ebene haben Zertifikate die folgenden Anforderungen:

> [!IMPORTANT]
> Die Zertifikatinformationen in diesem Artikel werden nur als Hilfestellungen bereitgestellt. Bevor Sie Zertifikate für Azure Stack erwerben, wenden Sie sich an Ihren OEM-Hardwarepartner. Er stellt Ihnen ausführliche Anleitungen und Anforderungen für Zertifikate bereit.

- Sie können ein einzelnes Platzhalterzertifikat oder eine Reihe von dedizierten Zertifikaten verwenden und dann Platzhalter nur für Endpunkte wie Speicher und Key Vault einsetzen.
- Zertifikate müssen von einer öffentlichen vertrauenswürdigen Zertifizierungsstelle oder Unternehmenszertifizierungsstelle ausgestellt werden.
- Das Zertifikat kann nicht von einer Zwischenzertifizierungsstelle ausgestellt werden.
 
In der folgenden Tabelle sind Dienste und Anzahl von öffentlichen Endpunkten aufgeführt, die für die anfängliche Azure Stack-Bereitstellung entsprechende Zertifikate erfordern. 

| Verwendung | Endpunkt 
| -------- | ------------- | 
| Azure Resource Manager (Administrator) | adminmanagment.[Region].[Externe_Domäne] |
| Azure Resource Manager (Benutzer) | managment.[Region].[Externe_Domäne] |
| Portal (Administrator) | adminportal.[Region].[Externe_Domäne] |
| Portal (Benutzer) | portal. [Region].[Externe_Domäne] |
| Key Vault (Benutzer) | &#42;.vault.[Region].[Externe_Domäne] |
| Key Vault (Administrator) | &#42;.adminvault.[Region].[Externe_Domäne] |
| Speicher | &#42;.blob.[Region].[Externe_Domäne]<br>&#42;.table.[Region].[Externe_Domäne]<br>&#42;.queue.[Region].[Externe_Domäne]  |
| Graph** | graph.[Region].[Externe_Domäne] |
| AD FS** | adfs.[Region].[Externe_Domäne] |
| | |

**Zertifikate für Graph- und AD FS-Endpunkte sind nur für AD FS-Bereitstellungen erforderlich.

Wenn Sie ein einzelnes Platzhalterzertifikat verwenden möchten, benötigen Sie insgesamt sechs alternative Antragstellernamen (SANs) für die anfängliche Azure Stack-Bereitstellung. Diese SANs sind: 

- &#42;.[Region].[Externe_Domäne]
- &#42;.vault.[Region].[Externe_Domäne]
- &#42;.adminvault.[Region].[Externe_Domäne]
- &#42;.blob.[Region].[Externe_Domäne]
- &#42;.table.[Region].[Externe_Domäne] 
- &#42;.queue.[Region].[Externe_Domäne]

## <a name="time-synchronization"></a>Zeitsynchronisierung

Sie müssen den Azure Stack-Zeitserver mit einem externen Zeitserver synchronisieren, der über die IP-Adresse aufgelöst werden kann. Für eine Bereitstellung ohne Verbindung zum Internet ist ein Zeitserver im Unternehmensnetzwerk erforderlich.

## <a name="network-connectivity"></a>Netzwerkverbindung

### <a name="dns-integration"></a>DNS-Integration

Zum Auflösen von externen DNS-Namen von Azure Stack (z. B. www.bing.com) müssen Sie DNS-Server bereitstellen, die Azure Stack für die Weiterleitung von DNS-Anforderungen verwenden kann, für die Azure Stack nicht autoritativ ist. Als Bereitstellungseingaben müssen Sie mindestens zwei Server zu Fehlertoleranzzwecken als DNS-Weiterleitungen bereitstellen.

Zum Auflösen von DNS-Namen von Azure Stack-Endpunkten und -Instanzen von außerhalb von Azure Stack (beispielsweise aus der Unternehmensgesamtstruktur) müssen Sie die DNS-Server, die die externe DNS-Zone für Azure Stack hosten, mit den DNS-Servern integrieren, die die übergeordnete Zone hosten, die Sie verwenden möchten. Dies erfordert die Auflösung von DNS-Namen zwischen Azure Stack und vorhandenen DNS-Zonen im Rechenzentrum. Dazu verwenden Sie Methoden wie die bedingte Weiterleitung oder die Zonendelegierung. Die bedingte Weiterleitung wird empfohlen, wenn Sie die direkte Kontrolle über die DNS-Server besitzen, die die übergeordnete Zone für Ihren externen DNS-Namespace von Azure Stack hosten. (Wenn die externe DNS-Zone von Azure Stack als untergeordnete Domäne Ihres Unternehmensdomänennamens (z. B. „azurestack.contoso.com“ und „contoso.com“) angezeigt wird, müssen Sie stattdessen die Zonendelegierung verwenden.)

### <a name="network-infrastructure"></a>Netzwerkinfrastruktur

Die Netzwerkinfrastruktur für Azure Stack besteht aus mehreren logischen Netzwerken, die auf den Switches konfiguriert sind. Das folgende Diagramm zeigt diese logischen Netzwerke und wie sie mit TOR- (Top-of-Rack), BMC- (Baseboard-Verwaltungscontroller) und Grenzswitches (Kundennetzwerk) integriert werden.

![Logisches Netzwerkdiagramm und Switchverbindungen](media/azure-stack-planning-considerations/NetworkDiagram.png)

Die folgende Tabelle zeigt die logischen Netzwerke und die zugehörigen IPv4-Subnetzbereiche, die Sie berücksichtigen müssen.

| Logisches Netzwerk | Beschreibung | Größe | 
| -------- | ------------- | ------------ | 
| Öffentliche VIP | Öffentliche IP-Adressen für einen kleinen Satz von Azure Stack-Diensten, wobei die restlichen Adressen von virtuellen Computern von Mandanten verwendet werden. Die Azure Stack-Infrastruktur verwendet 32 Adressen aus diesem Netzwerk. Wenn Sie App Service und SQL-Ressourcenanbieter verwenden möchten, werden weitere sieben Adressen verwendet. | /26 (62 Hosts) - /22 (1022 Hosts)<br><br>Empfohlen = /24 (254 Hosts) | 
| Switchinfrastruktur | Point-to-Point-IP-Adressen für Routingzwecke, dedizierte Switchverwaltungsschnittstellen und Loopbackadressen, die dem Switch zugewiesen sind. | /26 | 
| Infrastruktur | Für die Kommunikation mit internen Azure Stack-Komponenten. | /24 |
| Private | Für das Speichernetzwerk und die privaten virtuellen IP-Adressen. | /24 | 
| BMC | Für die Kommunikation mit BMCs auf den physischen Hosts. | /27 | 
| | | |

### <a name="uplink-to-border-switches"></a>Uplink zu Grenzswitches

Sie benötigen einen Uplink, der für die Grenzswitches im Rechenzentrum konfiguriert ist. Sie können diesen Datenverkehr der Ebene 3 von den TOR-Switches (Top-of-Rack), die Teil eines Systems mit integriertem Azure Stack sind, mithilfe einer der folgenden Methoden weiterleiten:

- Border Gateway Protocol (BGP) 
- Statisches Routing

Hierbei wird BGP empfohlen, da es die automatische Aktualisierung von Routen ermöglicht, die vom SLB MUX (Software Load Balancing Multiplexer) für externe Netzwerke veröffentlicht werden. Dies ist wichtig, wenn Sie nach der Bereitstellung öffentliche IP-Adressbereiche hinzufügen. Wenn Sie BGP-Peering von den TOR-Switches zum Aggregatswitch verwenden, mit dem die TOR-Switches verbunden sind, werden die öffentlichen IP-Adressbereiche, die Sie nach der Bereitstellung hinzufügen, dem Aggregatswitch automatisch ohne manuelle Eingriffe angekündigt. Wenn Sie das statische Routing verwenden, müssen Sie die Routen für die neue Bereiche jedes Mal manuell aktualisieren, wenn Sie einen öffentlichen IP-Adressblock hinzufügen.

### <a name="proxy-server"></a>Proxyserver

Azure Stack unterstützt nur transparente Proxyserver. Ein transparenter Proxy fängt Anforderungen auf der Vermittlungsschicht ab, ohne eine spezielle Clientkonfiguration zu erfordern.

### <a name="publish-azure-stack-services"></a>Veröffentlichen von Azure Stack-Diensten

Sie müssen Azure Stack-Dienste für Benutzer zur Verfügung stellen, die sich außerhalb von Azure Stack befinden. Azure Stack richtet für die eigenen Infrastrukturrollen verschiedene Endpunkte ein. Diesen Endpunkte werden VIPs aus dem öffentlichen IP-Adressenpool zugewiesen. Für jeden Endpunkt in der externen DNS-Zone, die zum Zeitpunkt der Bereitstellung angegeben wurde, wird ein DNS-Eintrag erstellt. Dem Benutzerportal wird z. B. der DNS-Hosteintrag von „portal.<*Region*>.<*Externer_FQDN*>“ zugewiesen. 

#### <a name="ports-and-urls"></a>Ports und URLs

Damit Sie Azure Stack-Dienste (wie Portale, Azure Resource Manager, DNS, usw.) für externe Netzwerke zur Verfügung stellen können, müssen Sie für diese Endpunkte den eingehenden Datenverkehr für bestimmte URLs, Ports und Protokolle zulassen.
 
In einer Bereitstellung mit einem Uplink zwischen einem transparenten Proxy und einem herkömmlichen Proxyserver müssen für die ausgehende Kommunikation bestimmte Ports und URLs zugelassen werden. Dazu gehören Ports und URLs für Identität, Marketplace-Syndikation, Patch und Update, Registrierung und Nutzungsdaten.

Weitere Informationen finden Sie unter:
- [Eingehende Ports und Protokolle](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Ausgehende Ports und URLs](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Herstellen einer Verbindung von Azure Stack zu Azure

Für Hybrid Cloud-Szenarien müssen Sie planen, wie Sie Azure Stack mit Azure verbinden möchten. Es gibt zwei unterstützte Methoden, um virtuelle Netzwerke in Azure Stack mit virtuellen Netzwerken in Azure zu verbinden: 
- **Standort zu Standort**: Eine VPN-Verbindung (virtuelles privates Netzwerk) über IPsec (IKE v1 und IKE v2). Diese Art von Verbindung erfordert ein VPN-Gerät oder einen Routing- und RAS-Dienst (RRAS). Weitere Informationen zu VPN-Gateways in Azure finden Sie unter [Informationen zu VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Die Kommunikation über diesen Tunnel ist verschlüsselt und sicher. Die Bandbreite ist jedoch durch den maximalen Durchsatz des Tunnels (100-200 MBit/s) beschränkt.
- **Ausgehende NAT**: Standardmäßig verfügen alle virtuellen Computer in Azure Stack mittels ausgehender NAT über Verbindungen mit externen Netzwerken. Jedem in Azure Stack erstellten virtuellen Netzwerk wird eine öffentliche IP-Adresse zugewiesen. Ganz gleich, ob dem virtuellen Computer eine öffentliche IP-Adresse direkt zugewiesen wird oder er sich hinter einem Load Balancer mit einer öffentlichen IP-Adresse befindet, verfügt er mittels der ausgehenden NAT über den ausgehenden Zugriff, indem die virtuelle IP-Adresse des virtuellen Netzwerks verwendet wird. Dies funktioniert nur für die Kommunikation, die vom virtuellen Computer eingeleitet wird und die für externe Netzwerke (Internet oder Intranet) bestimmt ist. Dies kann nicht für die Kommunikation mit dem virtuellen Computer von außerhalb verwendet werden.

#### <a name="hybrid-connectivity-options"></a>Optionen der Hybridkonnektivität

Für die Hybridkonnektivität ist es wichtig, die Art und den Ort der anzubietenden Bereitstellung zu berücksichtigen. Sie müssen prüfen, ob Sie den Netzwerkdatenverkehr pro Mandant isolieren müssen. Zudem müssen Sie überlegen, ob Sie eine Bereitstellung über Intranet oder Internet verwenden.

- **Azure Stack für einzelnen Mandanten**: Eine Azure Stack-Bereitstellung, die zumindest aus Netzwerksicht so aussieht, als ob es sich um einen Mandanten handelt. Es können viele Mandantenabonnements vorliegen, aber wie bei jedem Intranetdienst verläuft sämtlicher Datenverkehr über dieselben Netzwerke. Der Netzwerkdatenverkehr für ein Abonnement wird über dieselbe Netzwerkverbindung wie für ein anderes Abonnement übertragen und muss nicht über einen verschlüsselten Tunnel isoliert werden.

- **Azure Stack für mehrere Mandanten**: Eine Azure Stack-Bereitstellung, bei der der an Netzwerke, die außerhalb von Azure Stack verlaufen, gebundene Datenverkehr der einzelnen Mandantenabonnements vom Netzwerkdatenverkehr anderer Mandanten isoliert werden muss.
 
- **Intranetbereitstellung**: Eine Azure Stack-Bereitstellung, die auf einem Unternehmensintranet basiert und sich in der Regel in einem privaten IP-Adressraum und hinter einer oder mehreren Firewalls befindet. Die öffentlichen IP-Adressen sind nicht wirklich öffentlich, da sie nicht direkt über das öffentliche Internet weitergeleitet werden können.

- **Internetbereitstellung**: Eine Azure Stack-Bereitstellung, die mit dem öffentlichen Internet verbunden ist und über das Internet zugängliche öffentliche IP-Adressen für den öffentlichen VIP-Bereich verwendet. Die Bereitstellung kann sich weiterhin hinter einer Firewall befinden, aber der öffentliche VIP-Adressbereich ist direkt über das öffentliche Internet und Azure erreichbar.
 
In der folgenden Tabelle sind die Szenarien zur Hybridkonnektivität mit den jeweiligen Vor- und Nachteilen sowie mit Anwendungsfällen zusammengefasst.

| Szenario | Konnektivitätsmethode | Vorteile | Nachteile | Geeignet für |
| -- | -- | --| -- | --|
| Azure Stack für einzelnen Mandanten, Intranetbereitstellung | Ausgehende NAT | Höhere Bandbreite für schnellere Übertragungen Einfach zu implementieren, keine Gateways erforderlich | Datenverkehr nicht verschlüsselt, keine Isolierung oder Verschlüsselung außerhalb des TOR-Switches | Unternehmensbereitstellungen, bei denen alle Mandanten gleichermaßen vertrauenswürdig sind<br><br>Unternehmen, die über eine Azure ExpressRoute-Verbindung zu Azure verfügen |
| Azure Stack für mehrere Mandanten, Intranetbereitstellung | Standort-zu-Standort-VPN-Verbindung | Datenverkehr aus dem VNet des Mandanten zum Ziel ist sicher | Bandbreite durch Standort-zu-Standort-VPN-Tunnel begrenzt<br><br>Gateway im virtuellen Netzwerk und ein VPN-Gerät im Zielnetzwerk erforderlich | Unternehmensbereitstellungen, in denen gewisser Mandantendatenverkehr vor anderen Mandanten geschützt werden muss |
| Azure Stack für einzelnen Mandanten, Internetbereitstellung | Ausgehende NAT | Höhere Bandbreite für schnellere Übertragungen | Datenverkehr nicht verschlüsselt, keine Isolierung oder Verschlüsselung außerhalb des TOR-Switches | Hostingszenarien, in denen Mandanten ihre eigene Azure Stack-Bereitstellung und eine dedizierte Verbindung mit der Azure Stack-Umgebung erhalten Beispiel: ExpressRoute und MPLS (Multiprotocol Label Switching)
| Azure Stack für mehrere Mandanten, Internetbereitstellung | Standort-zu-Standort-VPN-Verbindung | Datenverkehr aus dem VNet des Mandanten zum Ziel ist sicher | Bandbreite durch Standort-zu-Standort-VPN-Tunnel begrenzt<br><br>Gateway im virtuellen Netzwerk und ein VPN-Gerät im Zielnetzwerk erforderlich | Hostingszenarien, in denen der Anbieter eine Cloud für mehrere Mandanten ohne gegenseitiges Vertrauen bereitstellen möchte und der Datenverkehr verschlüsselt werden muss.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Verwenden von ExpressRoute

Sie können Azure Stack über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) für Szenarien mit Intranetbereitstellungen für einzelne und mehrere Mandanten mit Azure verbinden. Sie benötigen eine bereitgestellte ExpressRoute-Verbindung über einen [Konnektivitätsanbieter](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Das folgende Diagramm zeigt ExpressRoute für ein Szenario mit einzelnem Mandanten (dabei stellt die Verbindung des Kunden die ExpressRoute-Verbindung dar).

![Diagramm mit ExpressRoute-Szenario mit einzelnem Mandanten](media/azure-stack-planning-considerations/ExpressRouteSingleTenant.PNG)

Das folgende Diagramm zeigt ExpressRoute für ein Szenario mit mehreren Mandanten.

![Diagramm mit ExpressRoute-Szenario mit mehreren Mandanten](media/azure-stack-planning-considerations/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externe Überwachung
Um eine Gesamtübersicht über alle Warnungen Ihrer Azure Stack-Bereitstellung sowie den zugehörigen Geräten zu erhalten und Warnungen zur Ticketerstellung in vorhandene Workflows zur IT-Dienstverwaltung zu integrieren, können Sie Azure Stack mit Überwachungslösungen externer Rechenzentren integrieren.

Der in der Azure Stack-Lösung enthaltene Hardwarelebenszyklushost ist ein Computer, der sich außerhalb von Azure Stack befindet, und vom OEM-Anbieter bereitgestellte Verwaltungstools für Hardware ausführt. Sie können diese Tools oder andere Lösungen verwenden, die direkt mit vorhandenen Überwachungslösungen in Ihr Rechenzentrum integriert werden.

In der folgenden Tabelle ist die Liste der derzeit verfügbaren Optionen zusammengefasst.

| Bereich | Externe Überwachungslösung |
| -- | -- |
| Azure Stack-Software | - [Azure Stack Management Pack für Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Nagios-Plug-In](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>- REST-basierte API-Aufrufe | 
| Physische Server (BMCs über IPMI) | - Operations Manager-Lieferantenverwaltungspaket<br>- Vom OEM-Hardwareanbieter bereitgestellte Lösung<br>- Plug-Ins des Hardwareanbieters Nagios | Vom OEM-Partner unterstützte Überwachungslösung (enthalten) | 
| Netzwerkgeräte (SNMP) | - Operations Manager-Netzwerkgeräteermittlung<br>- Vom OEM-Hardwareanbieter bereitgestellte Lösung<br>- Nagios Switch-Plug-In |
| Systemüberwachung für Mandantenabonnements | - [System Center Management Pack für Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Beachten Sie folgende Punkte:
- Die von Ihnen verwendete Lösung darf keine Agenten verwenden. Sie können keine Agenten von Drittanbietern in Azure Stack-Komponenten installieren. 
- Wenn Sie System Center Operations Manager verwenden möchten, erfordert dies Operations Manager 2012 R2 oder Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Backup und Notfallwiederherstellung

Die Planung der Sicherung und Notfallwiederherstellung umfasst das Planen für die zugrunde liegende Azure Stack-Infrastruktur, die virtuelle IaaS-Computer und PaaS-Dienste hostet, sowie für Mandantenanwendungen und -daten. Sie müssen diese separat planen.

### <a name="protect-infrastructure-components"></a>Schützen von Infrastrukturkomponenten

Azure Stack sichert Infrastrukturkomponenten in einer von Ihnen angegebenen Freigabe.

- Sie benötigen eine externe SMB-Dateifreigabe auf einem vorhandenen Windows-basierten Dateiserver oder auf einem Gerät eines Drittanbieters.
- Sie sollten dieselbe Freigabe für die Sicherung von Netzwerkswitches und des Hardwarelebenszyklushosts verwenden. Ihr OEM-Hardwareanbieter unterstützt Sie dabei, eine Anleitung zum Sichern und Wiederherstellen dieser Komponenten bereitzustellen, da sich diese außerhalb von Azure Stack befinden. Sie sind dafür zuständig, die Sicherungsworkflows auf Basis der Empfehlungen des OEM-Anbieters auszuführen.

Wenn ein schwerwiegender Datenverlust auftritt, können Sie die Sicherung der Infrastruktur dazu verwenden, um für die Bereitstellungsdaten (z. B. Bereitstellungseingaben und -IDs, Dienstkonten, Stammzertifikat der Zertifizierungsstelle, Verbundressourcen (in Bereitstellungen ohne Verbindung zum Internet), Pläne, Angebote, Abonnements, Kontingente, RBAC-Richtlinien- und -Rollenzuweisungen sowie geheime Key Vault-Schlüssel) ein erneutes Seeding durchzuführen.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Schützen von Mandantenanwendungen auf virtuellen IaaS-Computern

Azure Stack sichert keine Mandantenanwendungen und -daten. Sie müssen die Sicherung und Notfallwiederherstellung auf einem Ziel außerhalb von Azure Stack planen. Der Mandantenschutz ist eine vom Mandanten gesteuerte Aktivität. Für virtuelle IaaS-Computer können Mandanten In-Guest-Technologien verwenden, um Dateiordner, Anwendungsdaten und den Systemstatus zu schützen. Als Unternehmen oder Dienstanbieter möchten Sie jedoch möglicherweise eine Sicherungs- und Wiederherstellungslösung anbieten, die sich in demselben Rechenzentrum oder extern in einer Cloud befindet.

Zur Sicherung von virtuellen IaaS-Computern mit Linux oder Windows müssen Sie Sicherungsprodukte mit Zugriff auf das Gastbetriebssystem verwenden, um den Datei-, Ordner- und Systemstatus sowie Anwendungsdaten zu schützen. Sie können Azure Backup, System Center Data Center Protection Manager oder unterstützte Produkte von Drittanbietern verwenden.

Sie können Azure Site Recovery oder unterstützte Produkte von Drittanbietern verwenden, um Daten für den Notfall an einen sekundären Standort zu replizieren und Anwendungsfailover zu orchestrieren. (Bei der ersten veröffentlichten Version von integrierten Systemen werden Failbacks von Azure Site Recovery nicht unterstützt. Allerdings können Sie Failbacks über einen manuellen Prozess erreichen.) Darüber hinaus können Anwendungen, die die native Replikation unterstützen (z. B. Microsoft SQL Server), Daten an einen anderen Speicherort replizieren, an dem die Anwendung ausgeführt wird.

> [!IMPORTANT]
> Bei der ersten veröffentlichten Version von integrierten Systemen werden Schutztechnologien unterstützt, die auf der Gastebene eines virtuellen IaaS-Computers arbeiten. Sie können keine Agenten auf zugrunde liegenden Infrastrukturservern installieren.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Anwendungsfällen, zum Erwerb, zu Partnern und zu OEM-Hardwareanbietern finden Sie auf der Produktseite von [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).
- Informationen zur Roadmap und geografischen Verfügbarkeit für integrierte Azure Stack-Systeme finden Sie im Whitepaper: [Azure Stack: Eine Erweiterung von Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

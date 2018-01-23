---
title: Aspekte bei der Planung mit Azure Stack integrierter Systeme | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie aktuell bei der Planung und Vorbereitung für Azure Stack mit mehreren Knoten vorgehen können."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 6ba6bed8321e1ffde8bc8959443682725da36827
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Aspekte bei der Planung mit Azure Stack integrierter Systeme
Wenn Sie an einem mit Azure Stack integrierten System interessiert sind, müsse Sie einige der wichtigsten Aspekte bei der Planung der Bereitstellung verstehen und wissen, wie das System in Ihr Rechenzentrum passt. Dieser Artikel bietet einen allgemeinen Überblick über diese Aspekte, damit Sie wichtige Infrastrukturentscheidungen für Ihr Azure Stack-System mit mehreren Knoten treffen können. Ein Verständnis dieser Aspekte hilft Ihnen bei der Zusammenarbeit mit Ihrem OEM-Hardwareanbieter, sobald dieser Azure Stack in Ihrem Rechenzentrum bereitstellt.  

> [!NOTE]
> Azure Stack-Systeme mit mehreren Knoten können nur bei autorisierten Hardwareherstellern bezogen werden. 

Für die Bereitstellung von Azure Stack müssen Sie eine Reihe von Entscheidungen treffen, damit Azure Stack ordnungsgemäß in Ihre Umgebung integriert wird. Sie müssen diese Informationen Ihrem Lösungsanbieter und dem Hardwarehersteller während des Planungsprozesses zur Verfügung stellen, bevor die Bereitstellung beginnt, damit der Prozess schnell und reibungslos abläuft.

Die benötigten Informationen reichen von Netzwerk-, Sicherheits- und Identitätsinformationen bis hin zu vielen wichtigen Entscheidungen, die möglicherweise Wissen aus vielen verschiedenen Bereichen und von verschiedenen Entscheidungsträgern erfordern. Daher müssen Sie möglicherweise Mitarbeiter aus mehreren Teams in Ihrem Unternehmen hinzuziehen, um sicherzustellen, dass Sie über alle erforderlichen Informationen verfügen, bevor die Bereitstellung beginnt. Es ist möglicherweise hilfreich, sich mit Ihrem Hardwarehersteller zu besprechen, während Sie diese Informationen sammeln, da er Ratschläge geben kann, die Ihnen bei Ihren Entscheidungen helfen.

Während Sie die erforderlichen Informationen recherchieren und sammeln, müssen Sie möglicherweise vor der Bereitstellung einige Konfigurationsänderungen an Ihrer Netzwerkumgebung vornehmen. Dazu gehören die Reservierung von IP-Adressräumen für die Azure Stack-Lösung sowie die Konfiguration Ihrer Router, Switches und Firewalls zur Vorbereitung der Konnektivität mit den Switches der neuen Azure Stack-Lösung. Sichern Sie sich bei Ihrer Planung die Unterstützung der Fachbereichsexperten.

## <a name="management-considerations"></a>Überlegungen zur Verwaltung
Azure Stack ist ein geschlossenes System, bei dem die Infrastruktur aus Berechtigungs- und Netzwerksicht abgeriegelt ist. Dabei werden Netzwerk-Zugriffssteuerungslisten (ACLs) angewendet, um sämtlichen nicht autorisierten eingehenden Datenverkehr und sämtliche unnötige Kommunikation zwischen Infrastrukturkomponenten zu blockieren. Dadurch soll nicht autorisierten Benutzern der Zugriff auf das System verwehrt werden.

Für die tägliche Verwaltung und den Betrieb besteht kein uneingeschränkter Administratorzugriff auf die Infrastruktur. Azure Stack-Operatoren müssen das System über das Administratorportal oder mithilfe von Azure Resource Manager (über PowerShell oder die REST-API) verwalten. Es gibt keinen Zugriff auf das System durch andere Verwaltungstools wie Hyper-V-Manager oder Failovercluster-Manager. Es kann keine Software von Drittanbietern (z. B. Agenten) innerhalb der Komponenten der Azure Stack-Infrastruktur installiert werden, um den Schutz des Systems zu unterstützen. Die Interoperabilität mit externer Verwaltungs- und Sicherheitssoftware erfolgt über PowerShell oder die REST-API.

Wenn für die Behandlung von Problemen, die nicht durch Schritte zur Warnungsvermittlung behoben werden, eine höhere Zugriffsebene erforderlich ist, müssen Sie mit dem Support zusammenarbeiten. Über den Support gibt es eine Methode, um temporär den uneingeschränkten Administratorzugriff für das System bereitzustellen, um komplexere Operationen durchführen zu können. 

## <a name="identity-considerations"></a>Überlegungen zur Identität

### <a name="choose-identity-provider"></a>Auswählen des Identitätsanbieters
Sie müssen bedenken, welchen Identitätsanbieter Sie für die Bereitstellung von Azure Stack nutzen möchten, Azure AD oder AD FS. Sie können die Identitätsanbieter nach der Bereitstellung nicht wechseln, ohne das System komplett neu bereitstellen zu müssen.

Ihre Wahl des Identitätsanbieters hat keinen Einfluss auf virtuelle Computer des Mandanten, auf das Identitätssystem sowie auf von ihnen verwendete Konten oder darauf, ob sie einer Active Directory-Domäne usw. beitreten können. Das ist was anderes.

Im Artikel [Bereitstellungsentscheidungen für in Azure Stack integrierte Systeme](.\azure-stack-deployment-decisions.md) erfahren Sie mehr über die Wahl eines Identitätsanbieters.

### <a name="ad-fs-and-graph-integration"></a>Integration von AD FS und Graph
Wenn Sie die Bereitstellung von Azure Stack mithilfe von AD FS als Identitätsanbieter gewählt haben, müssen Sie die AD FS-Instanz für Azure Stack über eine Verbundvertrauensstellung mit einer vorhandenen AD FS-Instanz integrieren. Dies gestattet Identitäten in einer vorhandenen Active Directory-Gesamtstruktur die Authentifizierung mit Ressourcen in Azure Stack.

Sie können auch den Graph-Dienst in Azure Stack mit dem vorhandenen Active Directory integrieren. Dadurch können Sie die rollenbasierte Zugriffssteuerung (RBAC) in Azure Stack verwalten. Wenn der Zugriff auf eine Ressource delegiert wird, sucht die Graph-Komponente das Benutzerkonto mithilfe des LDAP-Protokolls in der vorhandenen Active Directory-Gesamtstruktur.

Das folgende Diagramm zeigt den integrierten Datenverkehrsfluss von AD FS und Graph.
![Diagramm mit Datenverkehrsfluss für AD FS und Graph](media/azure-stack-deployment-planning/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lizenzierungsmodell

Sie müssen entscheiden, welches Lizenzierungsmodell Sie verwenden möchten. Für eine verbundene Bereitstellung können Sie entweder die nutzungs- oder die kapazitätsbasierte Lizenzierung auswählen. Die nutzungsbasierte Lizenzierung erfordert eine Verbindung mit Azure, um die Nutzungsdaten zu melden, die dann über Azure Commerce abgerechnet werden. Bei einer Bereitstellung ohne Internetverbindung wird nur die kapazitätsbasierte Lizenzierung unterstützt. Weitere Informationen zu den Lizenzierungsmodellen finden Sie unter [Microsoft Azure Stack – Verpackung und Preise](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Entscheidungen zur Benennung

Sie müssen entsprechende Überlegungen zur Planung Ihres Azure Stack-Namespace anstellen, insbesondere zum Regionsnamen und zum externen Domänennamen. Der vollqualifizierte Domänennamen (FQDN) der Azure Stack-Bereitstellung für öffentliche Endpunkte ist eine Kombination aus den folgenden beiden Namen: &lt;*Region*&gt;&lt;*Exterer_FQDN*&gt;, z. B. *east.cloud.fabrikam.com*. In diesem Beispiel wären die Azure Stack-Portale unter den folgenden URLs verfügbar:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

In der folgenden Tabelle sind diese Entscheidungen zur Domänenbenennung zusammengefasst.

| NAME | BESCHREIBUNG | 
| -------- | ------------- | 
|Regionsname | Der Name Ihres ersten Azure Stack-Region. Dieser Name wird als FQDN-Komponente für die öffentlichen virtuellen IP-Adressen (VIPs) verwendet, die von Azure Stack verwaltet werden. Der Regionsname gibt in der Regel eine physische Standort-ID an, z. B. den Standort des Rechenzentrums. | 
| Externer Domänenname | Der Name der DNS-Zone (Domain Name System) für Endpunkte mit externen VIPs. Wird im FQDN für diese öffentlichen VIPs verwendet. | 
| Privater (interner) Domänenname | Der Name der Domäne (und internen DNS-Zone), die in Azure Stack zur Infrastrukturverwaltung erstellt wurde. 
| | |

## <a name="certificate-requirements"></a>Zertifikatanforderungen

Für die Bereitstellung müssen Sie SSL-Zertifikate (Secure Sockets Layer) für öffentliche Endpunkte anbieten. Auf allgemeiner Ebene haben Zertifikate die folgenden Anforderungen:

- Sie können ein einzelnes Platzhalterzertifikat oder eine Reihe von dedizierten Zertifikaten verwenden und dann Platzhalter nur für Endpunkte wie Speicher und Key Vault einsetzen.
- Zertifikate können von einer öffentlichen vertrauenswürdigen Zertifizierungsstelle oder einer vom Kunden verwalteten Zertifizierungsstelle ausgestellt werden.

Weitere Informationen darüber, welche PKI-Zertifikate (Public Key-Infrastruktur) für die Bereitstellung von Azure Stack erforderlich sind und wie Sie diese erhalten, finden Sie unter [Anforderungen für PKI-Zertifikate für Azure Stack](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Die Informationen zu PKI-Zertifikaten in diesem Artikel dienen nur als allgemeine Leitlinien. Bevor Sie PKI-Zertifikate für Azure Stack erwerben, wenden Sie sich an Ihren OEM-Hardwarepartner. Er stellt Ihnen ausführliche Anleitungen und Anforderungen für Zertifikate bereit.



## <a name="time-synchronization"></a>Zeitsynchronisierung
Sie müssen einen bestimmten Zeitserver auswählen, mit dessen Hilfe Azure Stack synchronisiert wird.  Die Uhrzeitsynchronisierung ist für Azure Stack und seine Infrastrukturrollen von entscheidender Bedeutung, da sie zur Generierung von Kerberos-Tickets dient, mit denen interne Dienste sich gegenseitig authentifizieren.

Sie müssen eine IP-Adresse für den Zeitsynchronisierungsserver angeben. Wenngleich die meisten Komponenten in der Infrastruktur eine URL auflösen können, können einige nur IP-Adressen unterstützen. Wenn Sie die Bereitstellungsoption „Getrennt“ verwenden, müssen Sie einen Zeitserver in Ihrem Unternehmensnetzwerk angeben, von dem Sie sicher sind, dass er über das Infrastrukturnetzwerk in Azure Stack erreichbar ist.


## <a name="network-connectivity"></a>Netzwerkverbindung
Dieser Abschnitt enthält Informationen zur Netzwerkinfrastruktur von Azure Stack, die Ihnen dabei helfen, wichtige Entscheidungen zu treffen, wie Sie Azure Stack am besten in Ihre bestehende Netzwerkumgebung integrieren können. 

> [!NOTE]
> Zum Auflösen von externen DNS-Namen von Azure Stack (z. B. www.bing.com) müssen Sie DNS-Server bereitstellen, die Azure Stack für die Weiterleitung von DNS-Anforderungen verwenden kann, für die Azure Stack nicht autoritativ ist. Weitere Informationen zu DNS-Anforderungen für Azure Stack finden Sie unter [Azure Stack-Rechenzentrumsintegration: DNS](azure-stack-integrate-dns.md).

### <a name="physical-network-design"></a>Entwurf des physischen Netzwerks
Die Azure Stack-Lösung benötigt eine zuverlässige und hoch verfügbare physische Infrastruktur, um ihren Betrieb und ihre Dienste zu unterstützen. Nachstehend sehen Sie ein Diagramm unseres empfohlenen Entwurfs:

![Empfohlener Entwurf des Azure Stack-Netzwerks](media/azure-stack-deployment-planning/recommended-design.png)


### <a name="logical-networks"></a>Logische Netzwerke
Logische Netzwerke stellen eine Abstraktion der zugrunde liegenden physischen Netzwerkinfrastruktur dar. Sie dienen zum Organisieren und Vereinfachen von Netzwerkzuweisungen für Hosts, virtuelle Computer und Dienste. Im Rahmen der Erstellung eines logischen Netzwerks werden Netzwerkstandorte erstellt, um die virtuellen lokalen Netzwerke (VLANs), IP-Subnetze und IP-Subnetz-/VLAN-Paare zu definieren, die dem logischen Netzwerk an jedem physischen Standort zugeordnet sind.
Die Netzwerkinfrastruktur für Azure Stack besteht aus den folgenden logischen Netzwerken, die auf den Switches konfiguriert sind:

### <a name="network-infrastructure"></a>Netzwerkinfrastruktur
Die Netzwerkinfrastruktur für Azure Stack besteht aus mehreren logischen Netzwerken, die auf den Switches konfiguriert sind. Das folgende Diagramm zeigt diese logischen Netzwerke und wie sie mit TOR- (Top-of-Rack), BMC- (Baseboard-Verwaltungscontroller) und Grenzswitches (Kundennetzwerk) integriert werden.

![Logisches Netzwerkdiagramm und Switchverbindungen](media/azure-stack-deployment-planning/NetworkDiagram.png)

#### <a name="bmc-network"></a>BMC-Netzwerk
Dieses Netzwerk ist für die Verbindung aller Baseboard-Verwaltungscontroller (BMCs, auch Dienstprozessoren genannt, z.B. iDRAC, iLO, iBMC, iBMC usw.) mit dem Verwaltungsnetzwerk bestimmt. Falls vorhanden, befindet sich der Hardware Lifecycle Host (HLH) in diesem Netzwerk und kann OEM-spezifische Software für die Hardwarewartung und/oder -überwachung bereitstellen. 

#### <a name="private-network"></a>Privates Netzwerk
Dieses Netzwerk des Typs „/24“ (254 Host-IP-Adressen) ist für die Azure Stack-Region privat (d.h. es reicht nicht über die Grenzswitches der Azure Stack-Region hinaus) und ist in zwei Subnetze aufgeteilt:

- **Speichernetzwerk**. Ein Netzwerk des Typs „/25“ (126 Host-IP-Adressen), das zur Unterstützung der Verwendung von „Direkte Speicherplätze“ und Server Message Block-Speicherdatenverkehr (SMB) sowie der Livemigration virtueller Computer verwendet wird. 
- **Internes VIP-Netzwerk (Virtuelle IP-Adresse)**. Ein Netzwerk des Typs „/25“, das ausschließlich internen VIPs für den softwaregestützten Lastenausgleich vorbehalten ist.

#### <a name="azure-stack-infrastructure-network"></a>Azure Stack-Infrastrukturnetzwerk
Dieses Netzwerk des Typs „/24“ ist internen Azure Stack-Komponenten zugeordnet, damit diese untereinander kommunizieren und Daten austauschen können. Dieses Subnetz erfordert routingfähige IP-Adressen, bleibt aber durch Verwendung von Zugriffssteuerungslisten (ACLs) für die Lösung privat. Es wird nicht erwartet, dass ein Routing über die Grenzswitches erfolgt. Ein Ausnahme bildet ein sehr kleiner Bereich der Größe eines Netzwerks des Typs „/27“, der von einigen dieser Dienste genutzt wird, wenn sie Zugriff auf externe Ressourcen und/oder das Internet benötigen. 

#### <a name="public-infrastructure-network"></a>Öffentliches Infrastrukturnetzwerk
Dieses Netzwerk des Typs „/27“ ist der sehr kleine Bereich des bereits erwähnten Subnetzes der Azure Stack-Infrastruktur. Es benötigt keine öffentlichen IP-Adressen, jedoch Internetzugriff über einen NAT- oder transparenten Proxy. Dieses Netzwerk wird für das ERCS (Emergency Recovery Console System) bereitgestellt. Die ERCS-VM benötigt bei der Registrierung bei Azure Internetzugriff und muss für die Problembehandlung zu Ihrem Verwaltungsnetzwerk geroutet werden können.

#### <a name="public-vip-network"></a>Öffentliches VIP-Netzwerk
Das öffentliche VIP-Netzwerk wird dem Netzwerkcontroller in Azure Stack zugewiesen. Es ist kein logisches Netzwerk auf dem Switch. Die SLB nutzt den Adresspool und ordnet Netzwerke des Typs „/32“ für die Workloads von Mandanten zu. In der Routingtabelle auf dem Switch werden diese IP-Adressen des Typs „/32“ über BGP als verfügbare Route angekündigt. Dieses Netzwerk enthält die extern zugänglichen oder öffentlichen IP-Adressen. Die Azure Stack-Infrastruktur verwendet mindestens 8 Adressen aus diesem öffentlichen VIP-Netzwerk, während der Rest von den VMs der Mandanten genutzt wird. Die Netzwerkgröße in diesem Subnetz kann von einem Minimum von „/26“ (64 Hosts) bis zu einem Maximum von „/22“ (1022 Hosts) reichen. Wir empfehlen Ihnen, ein Netzwerk des Typs „/24“ zu planen.

#### <a name="switch-infrastructure-network"></a>Switchinfrastrukturnetzwerk
Dieses Netzwerk des Typs „/26“ ist das Subnetz, das die routingfähigen Punkt-zu-Punkt-IP-Subnetze des Typs „/30“ (2 Host-IP-Adressen) und die Loopbacks enthält, die dedizierte Subnetze des Typs „/32“ für die In-band-Switchverwaltung und BGP-Router-ID sind. Dieser IP-Adressbereich muss extern von der Azure Stack-Lösung zu Ihrem Rechenzentrum geroutet werden können. Es kann sich dabei um private oder öffentliche IP-Adressen handeln.

#### <a name="switch-management-network"></a>Switchverwaltungsnetzwerk
Dieses Netzwerk des Typs „/29“ (6 Host-IP-Adressen) dient zum Verbinden der Verwaltungsports der Switches. Sie erlaubt einen Out-of-band-Zugriff für Bereitstellung, Verwaltung und Problembehandlung. Sie wird anhand des oben erwähnten Switchinfrastrukturnetzwerks berechnet.

### <a name="transparent-proxy"></a>Transparenter Proxy
Die Azure Stack-Lösung unterstützt keine normalen Webproxys. Wenn das Rechenzentrum verlangt, dass für den gesamten Datenverkehr ein Proxy verwendet wird, müssen Sie einen transparenten Proxy konfigurieren. Dieser muss den gesamten Datenverkehr gemäß den Richtlinien verarbeiten und den Datenverkehr zwischen den Zonen in Ihrem Netzwerk trennen. Ein transparenter Proxy (auch abfangender, inline oder erzwungener Proxy genannt) fängt die normale Kommunikation auf der Netzwerkschicht ab, ohne dass eine spezielle Clientkonfiguration erforderlich ist. Clients müssen nicht wissen, dass der Proxy vorhanden ist.

### <a name="publish-azure-stack-services"></a>Veröffentlichen von Azure Stack-Diensten

Sie müssen Azure Stack-Dienste für Benutzer zur Verfügung stellen, die sich außerhalb von Azure Stack befinden. Azure Stack richtet für die eigenen Infrastrukturrollen verschiedene Endpunkte ein. Diesen Endpunkte werden VIPs aus dem öffentlichen IP-Adressenpool zugewiesen. Für jeden Endpunkt in der externen DNS-Zone, die zum Zeitpunkt der Bereitstellung angegeben wurde, wird ein DNS-Eintrag erstellt. Dem Benutzerportal wird z. B. der DNS-Hosteintrag „portal.*&lt;region>.&lt;fqdn>*“ zugewiesen.

#### <a name="ports-and-urls"></a>Ports und URLs

Damit Sie Azure Stack-Dienste (wie Portale, Azure Resource Manager, DNS, usw.) für externe Netzwerke zur Verfügung stellen können, müssen Sie für diese Endpunkte den eingehenden Datenverkehr für bestimmte URLs, Ports und Protokolle zulassen.
 
In einer Bereitstellung mit einem Uplink zwischen einem transparenten Proxy und einem herkömmlichen Proxyserver müssen für die ausgehende Kommunikation bestimmte Ports und URLs zugelassen werden. Dazu gehören Ports und URLs für Identität, Marketplace-Syndikation, Patch und Update, Registrierung und Nutzungsdaten.

Weitere Informationen finden Sie unter 
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

![Diagramm mit ExpressRoute-Szenario mit einzelnem Mandanten](media/azure-stack-deployment-planning/ExpressRouteSingleTenant.PNG)

Das folgende Diagramm zeigt ExpressRoute für ein Szenario mit mehreren Mandanten.

![Diagramm mit ExpressRoute-Szenario mit mehreren Mandanten](media/azure-stack-deployment-planning/ExpressRouteMultiTenant.PNG)

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

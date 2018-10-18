---
title: Bewährte Methoden für die Azure-Netzwerksicherheit | Microsoft Docs
description: Dieser Artikel enthält eine Reihe von bewährten Methoden für die Netzwerksicherheit unter Verwendung der integrierten Azure-Funktionen.
services: security
documentationcenter: na
author: TomShinder
manager: mbaldwin
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: TomSh
ms.openlocfilehash: d89972ff0f7e3035fa20f8d9ee2863b68fa52e9f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124064"
---
# <a name="azure-network-security-best-practices"></a>Bewährte Methoden für die Azure-Netzwerksicherheit
Sie können [virtuelle Azure-Computer (VMs)](https://azure.microsoft.com/services/virtual-machines/) und Appliances mit anderen Geräten im Netzwerk verbinden, indem Sie sie in [Azure Virtual Networks](https://azure.microsoft.com/documentation/services/virtual-network/) anordnen. Dies ist ein Konstrukt, mit dem Sie virtuelle Netzwerkschnittstellenkarten mit einem virtuellen Netzwerk verbinden können, um die TCP/IP-basierte Kommunikation zwischen netzwerkfähigen Geräten zu ermöglichen. Virtuelle Computer, die mit einem Azure Virtual Network verbunden sind, können eine Verbindung mit Geräten im selben virtuellen Netzwerk, anderen virtuellen Netzwerken, im Internet oder sogar in eigenen lokalen Netzwerken herstellen.

In diesem Artikel werden die bewährten Methoden für die Azure-Netzwerksicherheit beschrieben. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit dem Azure-Netzwerk und den Erfahrungen von Kunden wie Ihnen abgeleitet.

In diesem Artikel wird für jede bewährte Methode Folgendes erläutert:

* Wobei es bei der bewährten Methode geht
* Warum Sie die bewährte Methode nutzen sollten
* Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
* Mögliche Alternativen zur bewährten Methode
* Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für die Azure-Netzwerksicherheit basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Erstellungszeitpunkt dieses Artikels existierten. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Die folgenden Abschnitte beschreiben bewährte Methoden für Netzwerksicherheit.

## <a name="logically-segment-subnets"></a>Logische Segmentsubnetze
Azure Virtual Networks ähneln einem LAN in Ihrem lokalen Netzwerk. Die Idee hinter einem Azure Virtual Network ist, dass Sie ein Netzwerk mit nur einem privaten IP-Adressraum erstellen, in dem Sie alle virtuellen Azure-Computer anordnen können. Die verfügbaren privaten IP-Adressräume liegen in den Bereichen der Klasse A (10.0.0.0/8), Klasse B (172.16.0.0/12) und Klasse C (192.168.0.0/16).

Zu den bewährten Methoden für die logische Segmentierung von Subnetzen gehören:

**Bewährte Methode**: Segmentieren des größeren Adressraums in Subnetze.   
**Detail**: Sie können [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)-basierte Subnetzprinzipien verwenden, um die Subnetze zu erstellen.

**Bewährte Methode**: Erstellen von Netzwerkzugriffssteuerungen zwischen Subnetzen. Das Routing zwischen den Subnetzen wird automatisch durchgeführt, und es ist nicht erforderlich, Routingtabellen manuell zu konfigurieren. Standardmäßig gibt es keine Netzwerkzugriffssteuerung zwischen den Subnetzen, die Sie im Azure Virtual Network erstellen.   
**Details**: Verwenden Sie eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG). Bei NSGs handelt es sich um einfache zustandsbehaftete Paketuntersuchungsgeräte, für die der 5-Tupel-Ansatz (Quell-IP, Quellport, Ziel-IP, Zielport und Layer-4-Protokoll) verwendet wird, um Zulassen/Verweigern-Regeln für den Netzwerkdatenverkehr zu erstellen. Sie können Datenverkehr für eine einzelne IP-Adresse, mehrere IP-Adressen und gesamte Subnetze in beiden Richtungen zulassen oder verweigern.

Bei der Verwendung von NSGs für die Netzwerkzugriffssteuerung zwischen Subnetzen können Sie das Ressourcen, die zu derselben Sicherheitszone oder Rolle gehören, in eigenen Subnetzen anordnen.

## <a name="control-routing-behavior"></a>Steuern des Routingverhaltens
Wenn Sie einen virtuellen Computer in einem Azure Virtual Network anordnen, kann die VM eine Verbindung mit jeder anderen VM in demselben virtuellen Netzwerk verbinden, auch wenn sich die anderen VMs in unterschiedlichen Subnetzen befinden. Dies ist möglich, weil standardmäßig aktivierte Systemrouten diese Art der Kommunikation ermöglichen. Diese Standardrouten ermöglichen VMs in demselben virtuellen Netzwerk die Initiierung von Verbindungen untereinander und mit dem Internet (gilt nur für ausgehende Kommunikation mit dem Internet).

Die standardmäßigen Systemrouten sind zwar für viele Bereitstellungsszenarien nützlich, aber es kann auch vorkommen, dass Sie die Routingkonfiguration für Ihre Bereitstellungen anpassen möchten. Sie können die nächste Hopadresse konfigurieren, um bestimmte Ziele zu erreichen.

Wir empfehlen Ihnen, beim Bereitstellen einer Sicherheitsappliance für ein virtuelles Netzwerk [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md) zu konfigurieren. Wir erläutern dies in einem späteren Abschnitt mit dem Titel [Beschränken und Schützen Ihrer kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Benutzerdefinierte Routen sind nicht erforderlich, und die Standardsystemrouten funktionieren in der Regel.
>
>

## <a name="enable-forced-tunneling"></a>Aktivieren der Tunnelerzwingung
Zum besseren Verständnis der Tunnelerzwingung ist es hilfreich, wenn Sie wissen, um was es bei „getrennten Tunneln“ geht. Das häufigste Beispiel für getrennte Tunnels tritt bei VPN (Virtual Private Network)-Verbindungen auf. Stellen Sie sich vor, dass Sie von Ihrem Hotelzimmer aus eine VPN-Verbindung mit dem Netzwerk Ihres Unternehmens herstellen. Anhand dieser Verbindung können Sie auf Unternehmensressourcen zugreifen. Die gesamte Kommunikation mit Ihrem Unternehmensnetzwerk erfolgt über den VPN-Tunnel.

Was passiert, wenn Sie eine Verbindung mit Ressourcen im Internet herstellen möchten? Wenn getrennte Tunnel aktiviert sind, verlaufen diese Verbindungen direkt ins Internet und nicht über den VPN-Tunnel. Einige Sicherheitsexperten betrachten dies als potenzielles Risiko. Sie empfehlen, getrennte Tunnel zu deaktivieren und sicherzustellen, dass alle Verbindungen, die für das Internet und für Unternehmensressourcen bestimmt sind, durch den VPN-Tunnel laufen. Der Vorteil der Deaktivierung von geteilten Tunneln besteht darin, dass Verbindungen zum Internet über die Sicherheitsvorrichtungen des Unternehmensnetzwerks erzwungen werden. Das wäre nicht der Fall, wenn sich der VPN-Client außerhalb des VPN-Tunnels mit dem Internet verbunden hätte.

Wir wenden uns nun wieder VMs in einem Azure Virtual Network zu. Die Standardrouten für ein Azure Virtual Network ermöglichen VMs das Initiieren eines Datenverkehrs mit dem Internet. Auch dies kann ein Sicherheitsrisiko darstellen, da diese ausgehenden Verbindungen die Angriffsfläche einer VM vergrößern und von Angreifern ausgenutzt werden können. Aus diesem Grund wird empfohlen, die [Tunnelerzwingung](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) auf Ihrer VM zu aktivieren, wenn Sie eine standortübergreifende Konnektivität zwischen Ihrem Azure Virtual Network und dem lokalen Netzwerk verwenden. Die standortübergreifende Konnektivität wird später in diesem bewährten Methoden für Netzwerke beschrieben.

Falls Sie keine standortübergreifende Verbindung verwenden, sollten Sie sicherstellen, dass Sie NSGs (bereits beschrieben) oder Sicherheitsappliances für Azure Virtual Networks (werden als Nächstes beschrieben) nutzen. So können Sie ausgehende Verbindungen mit dem Internet für Ihre virtuellen Azure-Computer verhindern.

## <a name="use-virtual-network-appliances"></a>Verwenden virtueller Network Appliances
NSGs und benutzerdefiniertes Routing können ein gewisses Maß an Netzwerksicherheit auf den Netzwerk- und Transportebenen des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model) bieten. Aber in einigen Situationen wollen oder müssen Sie die Sicherheit auf hohen Ebenen des Stapels aktivieren. In diesen Situationen ist es ratsam, von Azure-Partnern angebotene Appliances für die Sicherheit virtueller Netzwerke bereitzustellen.

Sicherheitsappliances für Azure-Netzwerke können eine bessere Sicherheit bieten als die auf Netzwerkebene verfügbaren Steuerungen. Netzwerksicherheitsfunktionen, die von Sicherheitsappliances für virtuelle Netzwerke bereitgestellt werden, sind beispielsweise:


* Firewall
* Angriffserkennung/Eindringschutz
* Verwaltung von Sicherheitsrisiken
* Anwendungssteuerung
* Netzwerkbasierte Erkennung von Anomalien
* Webfilterung
* Virenschutz
* Botnet-Schutz

Die für ein Azure Virtual Network verfügbaren Sicherheitsappliances finden Sie im [Azure Marketplace](https://azure.microsoft.com/marketplace/). Suchen Sie nach den Begriffen „Sicherheit“ und „Netzwerksicherheit“.

## <a name="deploy-perimeter-networks-for-security-zones"></a>Bereitstellen von Umkreisnetzwerken für Sicherheitszonen
Ein [Umkreisnetzwerk](https://docs.microsoft.com/azure/best-practices-network-security) (auch als DMZ bezeichnet) ist ein physisches oder logisches Netzwerksegment, das als zusätzliche Sicherheitsebene zwischen Ihren Ressourcen und dem Internet zu dient. Spezielle Geräte für die Netzwerkzugriffssteuerung am Rande eines Umkreisnetzwerks erlauben nur den gewünschten Datenverkehr in Ihrem virtuellen Netzwerk.

Umkreisnetzwerke sind nützlich, da Sie sich bei der Verwaltung, Überwachung, Protokollierung und Berichterstellung für die Netzwerkzugriffssteuerung auf die Geräte am Rand des Azure Virtual Network konzentrieren können. Normalerweise aktivieren Sie DDoS (Distributed Denial of Service)-Verhinderung, Angriffserkennungs-/Eindringschutzsysteme (Intrusion Detection/Intrusion Prevention, IDS/IPS), Firewallregeln und -richtlinien, Webfilterung, Antischadsoftware für das Netzwerk usw. Die Geräte für die Netzwerksicherheit sind zwischen dem Internet und Ihrem Azure Virtual Network angeordnet und verfügen in beiden Netzwerken über eine Schnittstelle.

Dies ist das grundlegende Design einer Umkreisnetzwerks, aber es gibt noch viele andere Designs, z.B. Back-to-Back, Tri-Homed und Multi-Homed.

Wir empfehlen Ihnen für alle Bereitstellungen mit hohen Sicherheitsanforderungen die Nutzung eines Umkreisnetzwerks, um die Netzwerksicherheitsstufe für Ihre Azure-Ressourcen zu erhöhen.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermeiden der Offenlegung gegenüber dem Internet mit dedizierten WAN-Links
Viele Organisationen haben sich für die Hybrid-IT-Route entschieden. Bei Hybrid-IT befinden sich einige Datenressourcen des Unternehmens in Azure, während andere weiterhin lokal gespeichert sind. In vielen Fällen werden einige Komponenten eines Diensts in Azure ausgeführt, während andere Komponenten weiterhin lokal vorhanden sind.

Beim Hybrid-IT-Szenario wird normalerweise eine Art von standortübergreifender Konnektivität verwendet. Standortübergreifende Konnektivität ermöglicht es dem Unternehmen, seine lokalen Netzwerke mit Azure Virtual Networks zu verbinden. Es sind zwei Lösungen für standortübergreifende Konnektivität verfügbar:

* **Site-to-Site-VPN**: Es ist eine vertrauenswürdige, zuverlässige und bewährte Technologie, aber die Verbindung erfolgt über das Internet. Die Bandbreite ist mit einem Maximum von ca. 200 MBit/s relativ begrenzt. Site-to-Site-VPN ist für einige Szenarien eine geeignete Option und wird in Abschnitt [Deaktivieren des RDP/SSH-Zugriffs auf virtuelle Computer](#disable-rdpssh-access-to-virtual-machines) näher erläutert.
* **Azure ExpressRoute**: Wir empfehlen, dass Sie für Ihre standortübergreifende Konnektivität [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) verwenden. ExpressRoute ist ein dedizierter WAN-Link zwischen Ihrem lokalen Standort und einem Exchange-Hostinganbieter. Da dies eine Telekommunikationsverbindung ist, werden Ihre Daten nicht über das Internet übertragen und unterliegen daher auch nicht den potenziellen Risiken der Internetkommunikation.

## <a name="optimize-uptime-and-performance"></a>Optimieren der Betriebszeit und Leistung
Wenn ein Dienst ausgefallen ist, kann nicht auf Informationen zugegriffen werden. Wenn die Leistung so schlecht ist, dass die Daten nicht genutzt werden können, werden die Daten als nicht verfügbar angesehen. Aus Gründen der Sicherheit müssen Sie alles unternehmen, um sicherzustellen, dass für Ihre Dienste eine optimale Betriebszeit und Leistung gewährleistet ist.

Eine beliebte und effektive Methode zum Verbessern der Verfügbarkeit und Leistung ist der Lastenausgleich. Der Lastenausgleich ist ein Verfahren zum Verteilen von Netzwerkdatenverkehr auf Server, die Teil eines Diensts sind. Wenn Sie im Rahmen Ihres Diensts beispielsweise Front-End-Webserver verwenden, können Sie den Lastenausgleich nutzen, um Datenverkehr auf die verschiedenen Front-End-Webserver zu verteilen.

Die Verteilung von Datenverkehr erhöht die Verfügbarkeit, weil Folgendes passiert, wenn einer der Webserver nicht mehr verfügbar ist: Das Lastenausgleichsmodul beendet das Senden von Datenverkehr an den Server und leitet ihn an die Server um, die noch online sind. Außerdem trägt der Lastenausgleich zur Steigerung der Leistung bei, da der Mehraufwand für den Prozessor, das Netzwerk und den Arbeitsspeicher zum Verarbeiten von Anforderungen auf alle Server mit Lastenausgleich verteilt wird.

Wir empfehlen Ihnen, den Lastenausgleich nach Möglichkeit immer zu nutzen, wenn diese Vorgehensweise für Ihre Dienste geeignet ist. Es folgen Szenarien sowohl auf der Ebene des Azure Virtual Network als auch auf der globalen Ebene, zusammen mit den jeweiligen Lastausgleichsoptionen.

**Szenario**: Sie haben eine Anwendung:

- Bei denen Anforderungen einer Benutzer-/Clientsitzung den gleichen virtuellen Back-End-Computer erreichen müssen. Beispiele hierfür wären Einkaufswagen-Apps und Web-E-Mail-Server.
- Dabei ist nur eine sichere Verbindung zulässig. Unverschlüsselte Kommunikation mit den Servern sind keine akzeptable Option.
- Für die mehrere HTTP-Anforderungen über die gleiche lange bestehende TCP-Verbindung an verschiedene Back-End-Server weitergeleitet werden bzw. für die dort ein Lastenausgleich erfolgen muss.

**Option für den Lastenausgleich**: Verwenden Sie [Azure Application Gateway](../application-gateway/application-gateway-introduction.md), einen Lastenausgleich für HTTP-Web-Datenverkehr. Application Gateway unterstützt die End-to-End-SSL-Verschlüsselung und [SSL-Terminierung](../application-gateway/application-gateway-introduction.md) am Gateway. Webserver können dann von Ver- und Entschlüsselungs-Overheads und unverschlüsseltem Datenverkehr zu den Back-End-Servern entlastet werden.

**Szenario**: Sie benötigen für eingehende Verbindungen aus dem Internet einen Lastenausgleich zwischen Ihren Servern in einem Azure Virtual Network. Szenarien sind, wenn Sie:

- Zustandslose Anwendungen haben, die eingehende Anforderungen aus dem Internet akzeptieren.
- Keine persistenten Sitzungen oder SSL-Auslagerung benötigen. Persistente Sitzungen sind eine Methode, die für den Anwendungslastenausgleich verwendet wird, um die Serveraffinität zu erreichen.

**Option für den Lastenausgleich**: Verwenden Sie das Azure-Portal, um [einen externen Lastenausgleich](../load-balancer/quickstart-create-basic-load-balancer-portal.md) zu erstellen, der eingehende Anfragen auf mehrere VMs verteilt, um eine höhere Verfügbarkeit zu gewährleisten.

**Szenario**: Sie benötigen einen Lastenausgleich für Verbindungen von VMs, die sich nicht im Internet befinden. In den meisten Fällen werden die Verbindungen, die für den Lastenausgleich akzeptiert werden, von Geräten in einem Azure Virtual Network initiiert, wie z.B. SQL Server-Instanzen oder interne Webserver.   
**Option für den Lastenausgleich**: Verwenden Sie das Azure-Portal, um [einen internen Lastenausgleich](../load-balancer/quickstart-create-basic-load-balancer-powershell.md) zu erstellen, der eingehende Anfragen auf mehrere VMs verteilt, um eine höhere Verfügbarkeit zu gewährleisten.

**Szenario**: Sie benötigen globalen Lastenausgleich, da Sie:

- Eine Cloudlösung haben, die weit über mehrere Regionen verteilt ist und ein Höchstmaß an Betriebszeit (Verfügbarkeit) erfordert.
- Ein Höchstmaß an Betriebszeit benötigen, um sicherzustellen, dass Ihr Dienst verfügbar ist, auch wenn ein ganzes Rechenzentrum nicht verfügbar ist.

**Option für den Lastenausgleich**: Verwenden Sie den Azure Traffic Manager. Traffic Manager ermöglicht Ihnen den Lastenausgleich für Verbindungen mit Ihren Diensten basierend auf dem Standort des Benutzers.

Wenn ein Benutzer Ihren Dienst beispielsweise von einem EU-Standort aus anfordert, wird die Verbindung an Ihre Dienste umgeleitet, die in einem EU-Rechenzentrum angeordnet sind. Mit diesem Teil des globalen Lastenausgleichs von Traffic Manager können Sie die Leistung verbessern, da die Herstellung der Verbindung mit dem nächsten Rechenzentrum schneller als die Verbindung mit weit entfernten Rechenzentren möglich ist.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>Deaktivieren des RDP/SSH-Zugriffs auf virtuelle Computer
Es ist möglich, mit dem [Remotedesktopprotokoll](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) und dem [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell)-Protokoll (SSH) eine Verbindung mit virtuellen Azure-Computern herzustellen. Diese Protokolle ermöglichen die Remoteverwaltung von VMs und sind Standard im Rechenzentrum.

Das potenzielle Sicherheitsproblem bei der Verwendung dieser Protokolle über das Internet besteht darin, dass Angreifer [Brute-Force](https://en.wikipedia.org/wiki/Brute-force_attack)-Verfahren einsetzen können, um Zugriff auf virtuelle Azure-Computer zu erlangen. Nachdem sich die Angreifer Zugang verschafft haben, können sie Ihre VM als Ausgangspunkt für die Kompromittierung anderer Computer im virtuellen Netzwerk nutzen oder sogar Netzwerkgeräte außerhalb von Azure angreifen.

Wir empfehlen Ihnen, den direkten RDP- und SSH-Zugriff auf Ihre virtuellen Azure-Computer über das Internet zu deaktivieren. Nachdem der direkte RDP- und SSH-Zugriff über das Internet deaktiviert wurde, haben Sie andere Möglichkeiten, um für die Durchführung der Remoteverwaltung auf diese VMs zuzugreifen.

**Szenario**: Ein Benutzer kann eine Verbindung mit einem Azure Virtual Network über das Internet herstellen.   
**Option**: [Point-to-Site-VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) ist ein anderer Ausdruck für eine VPN-Client/Server-Verbindung mit Remotezugriff. Nachdem die Point-to-Site-Verbindung hergestellt wurde, kann der Benutzer per RDP oder SSH eine Verbindung mit allen VMs im Azure Virtual Network herstellen, mit denen der Benutzer per Point-to-Site-VPN-Verbindung verbunden ist. Hierbei wird davon ausgegangen, dass der Benutzer dazu berechtigt ist, auf diese VMs zuzugreifen.

Eine Point-to-Site-VPN-Verbindung ist sicherer als direkte RDP- oder SSH-Verbindungen, da sich der Benutzer zweimal authentifizieren muss, bevor er die Verbindung mit einer VM herstellen kann. Zunächst muss sich der Benutzer authentifizieren (und berechtigt sein), um die Point-to-Site-VPN-Verbindung herzustellen. Danach muss sich der Benutzer authentifizieren (und berechtigt sein), um die RDP- oder SSH-Sitzung zu erstellen.

**Szenario**: Ermöglichen Sie es Benutzern in Ihrem lokalen Netzwerk, sich mit VMs in Ihrem Azure Virtual Network zu verbinden.   
**Option**: Über eine [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)-Verbindung wird ein gesamtes Netzwerk über das Internet mit einem anderen Netzwerk verbunden. Sie können eine Site-to-Site-VPN-Verbindung verwenden, um Ihr lokales Netzwerk mit einem Azure Virtual Network zu verbinden. Benutzer in Ihrem lokalen Netzwerk verbinden sich über das RDP- oder SSH-Protokoll über die Site-to-Site-VPN-Verbindung. Sie müssen keinen direkten RDP- oder SSH-Zugriff über das Internet ermöglichen.

**Szenario**: Verwenden Sie eine dedizierte WAN-Verbindung, um Funktionalität bereitzustellen, die der Site-to-Site-VPN-Verbindung ähnelt.   
**Option**: Verwenden Sie [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). Es bietet ähnliche Funktionen wie eine Site-to-Site-VPN-Verbindung. Im Folgenden werden die Hauptunterschiede erläutert:

- Die dedizierte WAN-Verbindung verläuft nicht über das Internet.
- Dedizierte WAN-Verbindungen sind in der Regel stabiler und leistungsfähiger.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Beschränken und Schützen Ihrer kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke
Mit Dienstendpunkten von virtuellen Netzwerken werden der private Adressenbereich Ihres virtuellen Netzwerks und die Identität Ihres virtuellen Netzwerks über eine direkte Verbindung auf die Azure-Dienste erweitert. Endpunkte ermöglichen es Ihnen, Ihre kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. Der Datenverkehr aus Ihrem virtuellen Netzwerk an den Azure-Dienst verbleibt immer im Backbone-Netzwerk von Microsoft Azure.

Dienstendpunkte bieten folgende Vorteile:

- **Verbesserte Sicherheit für Ihre Azure-Dienstressourcen**: Mit Dienstendpunkten können Ressourcen von Azure-Diensten auf Ihr virtuelles Netzwerk beschränkt und so geschützt werden. Das Sichern von Dienstressourcen in einem virtuellen Netzwerk erhöht die Sicherheit, da der Zugriff über das öffentliche Internet auf Ressourcen vollständig verhindert und nur Datenverkehr aus Ihrem virtuellen Netzwerk zugelassen wird.
- **Optimale Weiterleitung des Datenverkehrs für Azure-Dienste aus Ihrem virtuellen Netzwerk**: Für alle Routen in Ihrem virtuellen Netzwerk, die für Internetdatenverkehr den Weg auf Ihre lokalen und/oder virtuellen Geräte erzwingen (als Tunnelerzwingung bezeichnet), wird auch für den Datenverkehr von Azure-Diensten die gleiche Route wie für den Internet-Datenverkehr erzwungen. Dienstendpunkte ermöglichen eine optimale Weiterleitung für Azure-Datenverkehr.

  Endpunkte leiten den Datenverkehr der Dienste direkt aus Ihrem virtuellen Netzwerk an den Dienst im Azure-Backbonenetzwerk. Die Verwaltung von Datenverkehr im Azure-Backbonenetzwerk ermöglicht Ihnen weiterhin die Überwachung und Überprüfung von ausgehendem Internet-Datenverkehr aus Ihren virtuellen Netzwerken durch die Tunnelerzwingung, ohne dass sich dies auf den Datenverkehr der Dienste auswirkt. Informieren Sie sich über [benutzerdefinierte Routen und die Tunnelerzwingung](../virtual-network/virtual-networks-udr-overview.md).

- **Einfache Einrichtung mit weniger Verwaltungsaufwand**: Sie benötigen in Ihren virtuellen Netzwerken keine reservierten öffentlichen IP-Adressen mehr, um Azure-Ressourcen über die IP-Firewall zu schützen. Es sind keine NAT- oder Gatewaygeräte erforderlich, um die Dienstendpunkte einzurichten. Dienstendpunkte werden einfach per Klick in einem Subnetz konfiguriert. Es entsteht kein zusätzlicher Aufwand für die Verwaltung der Endpunkte.

Weitere Informationen zu Dienstendpunkten und den Azure-Diensten und Regionen, für die Dienstendpunkte verfügbar sind, finden Sie in [Dienstendpunkte im virtuellen Netzwerk](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="next-step"></a>Nächster Schritt
Weitere bewährte Methoden für die Sicherheit, die Sie beim Entwerfen, Bereitstellen und Verwalten Ihrer Cloudlösungen mithilfe von Azure verwenden können, finden Sie unter [Sicherheit in Azure: bewährte Methoden und Muster](security-best-practices-and-patterns.md).

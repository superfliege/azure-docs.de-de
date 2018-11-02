---
title: 'Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Analyse der Datenebene: Interoperabilität der Azure-Back-End-Konnektivitätsfeatures | Microsoft-Dokumentation'
description: Auf dieser Seite wird die Datenebenenanalyse der Testeinrichtung beschrieben, die erstellt wird, um die Interoperabilität der Features ExpressRoute, Site-to-Site-VPN und VNET-Peering zu analysieren.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: c9f3824b1e0f44338696ba3c2e434d60eee3af8b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947000"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---data-plane-analysis"></a>Interoperabilität von ExpressRoute, Site-to-Site-VPN und VNET-Peering – Analyse der Datenebene

In diesem Artikel wird die Datenebenenanalyse der Testeinrichtung beschrieben. Informationen zur Testeinrichtung finden Sie unter [Testeinrichtung][Setup]. Die Details zur Testeinrichtungskonfiguration finden Sie unter [Testeinrichtungskonfiguration][Configuration]. Informationen zur Analyse der Steuerungsebene für die Testeinrichtung finden Sie unter [Analyse der Steuerungsebene][Control-Analysis].

Bei der Analyse der Datenebene wird der Pfad untersucht, den Pakete in einer Topologie von einem lokalen Netzwerk (LAN/VNET) zu einem anderen durchlaufen. Der Datenpfad zwischen zwei lokalen Netzwerken muss nicht unbedingt symmetrisch sein. Aus diesem Grund analysieren wir in diesem Artikel den Weiterleitungspfad von einem lokalen Netzwerk zum anderen separat vom Pfad in umgekehrter Richtung.

##<a name="data-path-from-hub-vnet"></a>Datenpfad vom Hub-VNET

###<a name="path-to-spoke-vnet"></a>Pfad zum Spoke-VNET

Beim VNET-Peering wird die Funktionalität der Netzwerkbrücke zwischen den beiden VNETs emuliert, die per Peering verbunden sind. Hier ist die Traceroute-Ausgabe eines Hub-VNET für eine VM im Spoke-VNET angegeben:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

Der folgende Bildschirmclip zeigt die grafische Verbindung des Hub VNET und des Spoke-VNET in Azure Network Watcher:


[![1]][1]

###<a name="path-to-branch-vnet"></a>Pfad zum Branch-VNET

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop das VPN-Gateway des Hub-VNET. Der zweite Hop ist das VPN-Gateway des Branch-VNET, dessen IP-Adresse nicht im Hub-VNET angekündigt wird. Der dritte Hop ist die VM im Branch-VNET.

Der folgende Bildschirmclip zeigt die grafische Verbindung des Hub VNET und des Branch-VNET in Azure Network Watcher:

[![2]][2]

Für dieselbe Verbindung zeigt der folgende Bildschirmclip die Rasteransicht in Azure Network Watcher:

[![3]][3]

###<a name="path-to-on-premises-location-1"></a>Pfad zum lokalen Standort 1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt für die MSEE-Instanz. Der zweite und dritte Hop sind LAN-IP-Adressen für einen CE-Router bzw. den lokalen Standort 1. Diese IP-Adressen werden im Hub-VNET nicht angekündigt. Der vierte Hop ist die VM am lokalen Standort 1.


###<a name="path-to-on-premises-location-2"></a>Pfad zum lokalen Standort 2

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt für die MSEE-Instanz. Der zweite und dritte Hop sind LAN-IP-Adressen für einen CE-Router bzw. den lokalen Standort 2. Diese IP-Adressen werden im Hub-VNET nicht angekündigt. Der vierte Hop ist die VM am lokalen Standort 2.

###<a name="path-to-remote-vnet"></a>Pfad zum Remote-VNET

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt für die MSEE-Instanz. Der zweite Hop ist die Gateway-IP-Adresse des Remote-VNET. Der IP-Adressbereich des zweiten Hops wird im Hub-VNET nicht angekündigt. Der dritte Hop ist die VM im Remote-VNET.

##<a name="data-path-from-spoke-vnet"></a>Datenpfad aus dem Spoke-VNET

Noch einmal zur Erinnerung: Für das Spoke-VNET wird die Netzwerkansicht des Hub-VNET genutzt. Per VNET-Peering nutzt das Spoke-VNET die Remotegatewayverbindung des Hub-VNET so, als ob eine direkte Verbindung mit dem Spoke-VNET vorhanden wäre.

###<a name="path-to-hub-vnet"></a>Pfad zum Hub-VNET

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

###<a name="path-to-branch-vnet"></a>Pfad zum Branch-VNET

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop das VPN-Gateway des Hub-VNET. Der zweite Hop ist das VPN-Gateway des Branch-VNET, dessen IP-Adresse nicht im Hub-/Spoke-VNET angekündigt wird. Der dritte Hop ist die VM im Branch-VNET.

###<a name="path-to-on-premises-location-1"></a>Pfad zum lokalen Standort 1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt des Hub-VNET für die MSEE-Instanz. Der zweite und dritte Hop sind LAN-IP-Adressen für einen CE-Router bzw. den lokalen Standort 1. Diese IP-Adressen werden im Hub-/Spoke-VNET nicht angekündigt. Der vierte Hop ist die VM am lokalen Standort 1.

###<a name="path-to-on-premises-location-2"></a>Pfad zum lokalen Standort 2

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt des Hub-VNET für die MSEE-Instanz. Der zweite und dritte Hop sind LAN-IP-Adressen für einen CE-Router bzw. den lokalen Standort 2. Diese IP-Adressen werden im Hub-/Spoke-VNET nicht angekündigt. Der vierte Hop ist die VM am lokalen Standort 2.

###<a name="path-to-remote-vnet"></a>Pfad zum Remote-VNET

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop der ExpressRoute-Gatewaytunnel-Endpunkt des Hub-VNET für die MSEE-Instanz. Der zweite Hop ist die Gateway-IP-Adresse des Remote-VNET. Der IP-Adressbereich des zweiten Hops wird im Hub-/Spoke-VNET nicht angekündigt. Der dritte Hop ist die VM im Remote-VNET.

##<a name="data-path-from-branch-vnet"></a>Datenpfad vom Branch-VNET

###<a name="path-to-hub-vnet"></a>Pfad zum Hub-VNET

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop das VPN-Gateway des Branch-VNET. Der zweite Hop ist das VPN-Gateway des Hub-VNET, dessen IP-Adresse nicht im Remote-VNET angekündigt wird. Der dritte Hop ist die VM im Hub-VNET.

###<a name="path-to-spoke-vnet"></a>Pfad zum Spoke-VNET

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop das VPN-Gateway des Branch-VNET. Der zweite Hop ist das VPN-Gateway des Hub-VNET, dessen IP-Adresse im Remote-VNET nicht angekündigt wird, und der dritte Hop ist die VM im Spoke-VNET.

###<a name="path-to-on-premises-location-1"></a>Pfad zum lokalen Standort 1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

Im obigen Traceroute-Fall ist der erste Hop das VPN-Gateway des Branch-VNET. Der zweite Hop ist das VPN-Gateway des Hub-VNET, dessen IP-Adresse nicht im Remote-VNET angekündigt wird. Der dritte Hop ist der Beendigungspunkt des VPN-Tunnels auf dem primären CE-Router. Der vierte Hop ist eine interne IP-Adresse des lokalen Standorts 1 im LAN, die außerhalb des CE-Routers nicht angekündigt wird. Der fünfte Hop ist die Ziel-VM am lokalen Standort 1.

###<a name="path-to-on-premises-location-2-and-remote-vnet"></a>Pfad zum lokalen Standort 2 und zum Remote-VNET

Wie bereits für die Analyse der Steuerungsebene beschrieben, besteht gemäß Netzwerkkonfiguration für das Branch-VNET keine Sichtbarkeit in Bezug auf den lokalen Standort 2 und das Remote-VNET. Dies wird durch die folgenden Ping-Ergebnisse bestätigt. 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

##<a name="data-path-from-on-premises-location-1"></a>Datenpfad vom lokalen Standort 1

###<a name="path-to-hub-vnet"></a>Pfad zum Hub-VNET

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

Im obigen Traceroute-Fall sind die ersten beiden Hops Teil des lokalen Netzwerks. Der dritte Hop ist die primäre MSEE-Schnittstelle für den CE-Router. Der vierte Hop ist das ExpressRoute-Gateway des Hub-VNET, dessen IP-Adressbereich im lokalen Netzwerk nicht angekündigt wird. Der fünfte Hop ist die Ziel-VM.

Azure Network Watcher stellt nur eine Azure-bezogene Ansicht bereit. Aus diesem Grund haben wir für die lokale Ansicht den Netzwerkleistungsmonitor (Network Performance Monitor, NPM) von Azure verwendet. Der Netzwerkleistungsmonitor stellt Agents bereit, die auf Servern im Netzwerk außerhalb von Azure installiert werden können und eine Analyse des Datenpfads ermöglichen.

Der folgende Bildschirmclip zeigt die Topologieansicht der Konnektivität für die VM am lokalen Standort 1 mit der VM im Hub-VNET per ExpressRoute.

[![4]][4]

Zur Erinnerung: Für die Testeinrichtung wird ein Site-to-Site-VPN als Ersatzverbindung für ExpressRoute zwischen dem lokalen Standort 1 und dem Hub-VNET genutzt. Zum Testen des Datenpfads in umgekehrter Richtung lösen wir einen ExpressRoute-Verbindungsfehler zwischen dem primären CE-Router am lokalen Standort 1 und der entsprechenden MSEE-Instanz aus, indem wir die für die MSEE-Instanz bestimmte CE-Schnittstelle herunterfahren.

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

Der folgende Bildschirmclip zeigt die Topologieansicht der Konnektivität für die VM am lokalen Standort 1 mit der VM im Hub-VNET per Site-to-Site-VPN-Verbindung, wenn die ExpressRoute-Konnektivität nicht vorhanden ist.

[![5]][5]

###<a name="path-to-spoke-vnet"></a>Pfad zum Spoke-VNET

Wir stellen jetzt die primäre ExpressRoute-Konnektivität wieder her, um die Datenpfadanalyse für das Spoke-VNET durchzuführen.

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Wir stellen die primäre ExpressRoute 1-Konnektivität für den Rest der Datenpfadanalyse her.

###<a name="path-to-branch-vnet"></a>Pfad zum Branch-VNET

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

###<a name="path-to-on-premises-location-2"></a>Pfad zum lokalen Standort 2

Wie bereits für die Analyse der Steuerungsebene beschrieben, besteht gemäß Netzwerkkonfiguration für den lokalen Standort 1 keine Sichtbarkeit in Bezug auf den lokalen Standort 2. Dies wird durch die folgenden Ping-Ergebnisse bestätigt. 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

###<a name="path-to-remote-vnet"></a>Pfad zum Remote-VNET

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

##<a name="data-path-from-on-premises-location-2"></a>Datenpfad vom lokalen Standort 2

###<a name="path-to-hub-vnet"></a>Pfad zum Hub-VNET

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Pfad zum Spoke-VNET

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

###<a name="path-to-branch-vnet-on-premises-location-1-and-remote-vnet"></a>Pfad zum Branch-VNET, lokalen Standort 1 und Remote-VNET

Wie bereits für die Analyse der Steuerungsebene beschrieben, besteht gemäß Netzwerkkonfiguration für den lokalen Standort 1 keine Sichtbarkeit in Bezug auf das Branch-VNET, den lokalen Standort 1 und das Remote-VNET. 

##<a name="data-path-from-remote-vnet"></a>Datenpfad vom Remote-VNET

###<a name="path-to-hub-vnet"></a>Pfad zum Hub-VNET

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Pfad zum Spoke-VNET

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-branch-vnet-and-on-premises-location-2"></a>Pfad zum Branch-VNET und lokalen Standort 2

Wie bereits für die Analyse der Steuerungsebene beschrieben, besteht gemäß Netzwerkkonfiguration für das Remote-VNET keine Sichtbarkeit in Bezug auf das Branch-VNET und den lokalen Standort 2. 


### <a name="path-to-on-premises-location-1"></a>Pfad zum lokalen Standort 1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="further-reading"></a>Weitere nützliche Informationen

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Gemeinsames Verwenden von ExpressRoute und Site-to-Site-VPN-Konnektivität

####<a name="site-to-site-vpn-over-expressroute"></a>Site-to-Site-VPN über ExpressRoute

Site-to-Site-VPN kann mithilfe von ExpressRoute-/Microsoft-Peering konfiguriert werden, um Daten auf privatem Wege zwischen Ihrem lokalen Netzwerk und Ihren Azure VNETs auszutauschen und dabei Vertraulichkeit, Anti-Replay, Authentizität und Integrität sicherzustellen. Weitere Informationen zur Konfiguration eines Site-to-Site-IPSec-VPN im Tunnelmodus per ExpressRoute-/Microsoft-Peering finden Sie unter [Konfigurieren eines Site-to-Site-VPN über ExpressRoute-/Microsoft-Peering][S2S-Over-ExR]. 

Die wichtigste Einschränkung bei der Konfiguration des S2S-VPN per Microsoft-Peering ist der Durchsatz. Der Durchsatz des IPSec-Tunnels wird durch die Kapazität des VPN-Gateways eingeschränkt. Der Durchsatz des VPN-Gateways ist im Vergleich zum ExpressRoute-Durchsatz niedriger. In Szenarien dieser Art stellt die Verwendung des IPSec-Tunnels für Datenverkehr mit hohem Schutzfaktor und des privaten Peerings für den gesamten anderen Datenverkehr einen Beitrag zur Optimierung der ExpressRoute-Bandbreitenauslastung dar.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-to-Site-VPN als sicherer Failoverpfad für ExpressRoute
Die ExpressRoute-Verbindung wird als redundantes Leitungspaar bereitgestellt, um die Hochverfügbarkeit sicherzustellen. Sie können die georedundante ExpressRoute-Konnektivität in unterschiedlichen Azure-Regionen konfigurieren. Wenn Sie wie in unserer Testeinrichtung in einer bestimmten Azure-Region einen Failoverpfad für Ihre ExpressRoute-Konnektivität einrichten möchten, können Sie hierfür ein Site-to-Site-VPN verwenden. Wenn über ExpressRoute und das Site-to-Site-VPN die gleichen Präfixe angekündigt werden, gibt Azure ExpressRoute den Vorzug vor dem Site-to-Site-VPN. Zur Vermeidung des asymmetrischen Routings zwischen ExpressRoute und dem Site-to-Site-VPN sollte in der lokalen Netzwerkkonfiguration ExpressRoute ebenfalls den Vorzug vor der Site-to-Site-VPN-Konnektivität erhalten.

Weitere Informationen zur Konfiguration von parallelen ExpressRoute- und Site-to-Site-VPN-Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Erweitern der Back-End-Konnektivität auf Spoke-VNETs und Branchstandorte

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Spoke-VNET-Konnektivität per VNET-Peering

Die Hub-and-Spoke-VNET-Architektur wird häufig genutzt. Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt zwischen Ihren Spoke-VNETs und für Ihr lokales Netzwerk fungiert. Die Spokes sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können. Der Datenverkehr wird über eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet. Weitere Informationen zur Architektur finden Sie unter [Implementieren einer Hub-Spoke-Netzwerktopologie in Azure][Hub-n-Spoke].

Beim VNET-Peering in einer Region können Spoke-VNETs Hub-VNET-Gateways verwenden (sowohl VPN- als auch ExpressRoute-Gateways), um mit Remotenetzwerken zu kommunizieren.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Branch-VNET-Konnektivität per Site-to-Site-VPN

Wenn Branch-VNETs (in unterschiedlichen Regionen) und lokale Netzwerke über ein Hub-VNET miteinander kommunizieren sollen, wird als native Azure-Lösung die Site-to-Site-VPN-Konnektivität per VPN verwendet. Eine andere Möglichkeit ist die Nutzung eines virtuellen Netzwerkgeräts für das Routing im Hub.

Informationen zur Konfiguration von VPN-Gateways finden Sie unter [Konfigurieren von VPN Gateway][VPN]. Informationen zur Bereitstellung von hoch verfügbaren virtuellen Netzwerkgeräten finden Sie unter [Bereitstellen hoch verfügbarer virtueller Netzwerkgeräte][Deploy-NVA].

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie viele ExpressRoute-Leitungen Sie mit einem ExpressRoute-Gateway (oder umgekehrt) verbinden können, oder zu anderen Skalierungslimits von ExpressRoute finden Sie unter [ExpressRoute – FAQ][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "Network Watcher-Ansicht zur Konnektivität vom Hub-VNET zum Spoke-VNET"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "Network Watcher-Ansicht zur Konnektivität vom Hub-VNET zum Branch-VNET"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "Network Watcher-Rasteransicht zur Konnektivität vom Hub-VNET zum Branch-VNET"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "Netzwerkleistungsmonitor-Ansicht zur Konnektivität von der VM am Standort 1 zum Hub-VNET per ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "Netzwerkleistungsmonitor-Ansicht zur Konnektivität von der VM am Standort 1 zum Hub-VNET per Site-to-Site-VPN"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering





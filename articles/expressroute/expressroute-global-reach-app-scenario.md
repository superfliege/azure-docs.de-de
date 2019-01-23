---
title: Azure ExpressRoute Global Reach-Anwendungsszenario | Microsoft-Dokumentation
description: Auf dieser Seite finden Sie ein Anwendungsszenario für Global Reach.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,global reach
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: rambala
ms.openlocfilehash: 5174baae40d7f14720affe90f702c024541a5745
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54333039"
---
# <a name="expressroute-global-reach-application-scenario"></a>ExpressRoute Global Reach-Anwendungsszenario

Weitere Informationen zu ExpressRoute Global Reach finden Sie unter [ExpressRoute Global Reach (Vorschau)][Global Reach]. In diesem Artikel werden die einzelnen Schritte eines Anwendungsszenarios erläutert. Darüber hinaus finden Sie hier einen Vergleich zwischen der ExpressRoute Global Reach-Lösung und einigen anderen Lösungen sowie Informationen zur Konfiguration von Global Reach für das Beispielszenario und zur Überprüfung der Verbindungen. 

##<a name="application-scenario"></a>Anwendungsszenario

Fabrikam Inc. hat eine große Niederlassung und eine umfangreiche Azure-Bereitstellung im Osten der USA. Die Back-End-Konnektivität zwischen den lokalen und Azure-basierten Bereitstellungen wird bei Fabrikam mit ExpressRoute realisiert. Contoso Ltd. hat eine Niederlassung und eine Azure-Bereitstellung im Westen der USA. Die Back-End-Konnektivität zwischen den lokalen und Azure-basierten Bereitstellungen wird bei Contoso mit ExpressRoute realisiert.  

Fabrikam Inc. übernimmt Contoso Ltd. Nach der Fusion möchte Fabrikam die beiden Netzwerke miteinander verbinden. Die folgende Abbildung veranschaulicht das Szenario:

 [![1]][1]

Die gestrichelten Pfeile in der Mitte der obigen Abbildung stellen die erforderlichen Netzwerkverbindungen dar. Die folgende Tabelle zeigt die Routentabelle des primären privaten Peerings der ExpressRoute-Lösung von Contoso Ltd. vor der Übernahme:

[![2]][2]

Die folgende Tabelle zeigt die Routentabelle des primären privaten Peerings der ExpressRoute-Lösung von Fabrikam Inc. vor der Übernahme:

[![3]][3]

## <a name="traditional-solutions"></a>Herkömmliche Lösungen

### <a name="option-1-interconnect-on-premises-networks"></a>Option 1: Herstellen von Verbindungen zwischen lokalen Netzwerken

Diese Option wird in der folgenden Abbildung veranschaulicht. Wie Sie sehen, können Sie die beiden lokalen Netzwerke per Site-to-Site-VPN oder über andere Breitbandverbindungsoptionen miteinander verbinden und das gesamte Routing zwischen den beiden Entitäten verwalten. 

[![4]][4]

Diese Option hat folgende Nachteile:

- Für die regionsübergreifende Azure-Kommunikation der Bereitstellung wird eine suboptimale Route erzwungen.
- Ihnen entgehen bei der regionsübergreifenden Kommunikation die Hochverfügbarkeitsvorteile des Backbonenetzwerks von Microsoft.
- Sie sind selbst für das gesamte Routing der regionsübergreifenden Kommunikation verantwortlich.

### <a name="option-2-expressroute-cross-connectivity-and-interconnect-on-premises-networks"></a>Option 2: ExpressRoute-Querverbindungen und Herstellen von Verbindungen zwischen lokalen Netzwerken

Diese Option wird in der folgenden Abbildung veranschaulicht:

[![5]][5]

Wie in der obigen Abbildung zu sehen, können Sie zusätzlich für Konnektivität zwischen den ExpressRoute-Leitungen und den regionsübergreifenden VNETs sorgen. Die regionsübergreifende Konnektivität zwischen dem VNET und den ExpressRoute-Leitungen bietet folgende Kommunikationsmöglichkeiten:

- Die VNETs im Westen/Osten der USA kommunizieren jeweils mit den lokalen Netzwerken von Fabrikam/Contoso.
- Das VNET im Westen der USA kommuniziert mit dem VNET im Osten der USA.

Die Verbindung zwischen den beiden lokalen Netzwerken ist weiterhin erforderlich, damit die lokalen Netzwerke miteinander kommunizieren können.

Bei dieser Option kann die regionsübergreifende Kommunikation für die private Bereitstellung über den Microsoft-Backbone und die Kommunikation zwischen den lokalen Netzwerken über das externe Netzwerk abgewickelt werden. Der größte Nachteil dieser Lösung besteht darin, dass Sie mehrere regionsübergreifende Verbindungen einrichten müssen, was die Wartung und die Problembehandlung erschwert. Darüber hinaus haben Sie bei dieser Option nicht die Möglichkeit, den hochverfügbaren globalen Backbone von Microsoft für die Kommunikation zwischen den beiden lokalen Netzwerken zu nutzen.

### <a name="option-3-vnet-peering-and-interconnect-on-premises-networks"></a>Option 3: VNET-Peering und Herstellen von Verbindungen zwischen lokalen Netzwerken

Diese Option wird in der folgenden Abbildung veranschaulicht. In diesem Fall wird VNET-Peering für die regionsübergreifende VNET-Kommunikation verwendet. Wie Sie sehen, ist die Option unvollständig, da sie keine regionsübergreifende Kommunikation zwischen VNET und lokaler Umgebung beinhaltet (dargestellt durch die beiden gepunkteten Pfeillinien in der Mitte). Verwenden Sie für die regionsübergreifende Kommunikation mit der lokalen Umgebung entweder eine Verbindung zwischen lokalen Umgebungen (wie in Option 1) oder ExpressRoute-Konnektivität (wie in Option 2).

[![6]][6]

Diese Option sorgt für optimales Routing bei der regionsübergreifenden VNET-Kommunikation. Sie ist jedoch nicht optimal, wenn Fabrikam oder Contoso über eine komplexere Azure- Bereitstellung verfügt (beispielsweise über ein Hub-Spoke-VNET-Modell). Außerdem haben Sie genau wie bei den beiden vorherigen Optionen nicht die Möglichkeit, den hochverfügbaren globalen Backbone von Microsoft für die Kommunikation zwischen den beiden lokalen Netzwerken zu nutzen.

## <a name="global-reach"></a>Global Reach

ExpressRoute Global Reach verknüpft das private Peering der ExpressRoute-Leitungen, wie in der folgenden Abbildung dargestellt:

[![7]][7]

Die Einrichtung von Global Reach zwischen den beiden ExpressRoute-Leitungen ermöglicht die private Kommunikation zwischen den beiden lokalen Netzwerken und der Azure-Bereitstellung in den beiden Regionen. Dadurch erfüllt Global Reach sämtliche Kommunikationsanforderungen (dargestellt durch die gestrichelte Linie in der ersten Abbildung dieses Artikels) über das Backbonenetzwerk von Microsoft.

### <a name="configure-global-reach"></a>Konfigurieren von Global Reach

Informationen zum Konfigurieren von ExpressRoute Global Reach finden Sie unter [Konfigurieren von ExpressRoute Global Reach (Vorschauversion)][Configure Global Reach]. 

Da Fabrikam Inc. und Contoso Ltd. Azure als separate Unternehmen implementiert haben, befinden sich die ExpressRoute-Leitungen der beiden Unternehmen in zwei unterschiedlichen Azure-Abonnements. Zum Erstellen der abonnementübergreifenden Global Reach-Verbindung müssen Sie in der ExpressRoute-Leitung von Contoso Ltd. eine Autorisierung erstellen und an die ExpressRoute-Leitung von Fabrikam Inc. übergeben.


Melden Sie sich zum Erstellen einer Autorisierung für die ExpressRoute-Leitung von Contoso zunächst bei dem Azure-Konto von Contoso an, und wählen Sie das entsprechende Abonnement aus (sofern mehrere Abonnements vorhanden sind). PowerShell-Befehle für diese Schritte:

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```
Im Anschluss finden Sie die Schritte zum Erstellen einer ExpressRoute-Leitungsautorisierung:

```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```

Die Ausgabe von „Set-AzureRmExpressRouteCircuit“ enthält die ExpressRoute-Leitung (ExpressRouteCircuit). Notieren Sie sich die ID für das private Peering und den Autorisierungsschlüssel, die gegen Ende der Liste angegeben sind. Hier sehen Sie einen Ausschnitt aus einer PowerShell-Beispielausgabe:

```powershell
ServiceKey                       : <serviceKey>
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 65020,
                                       "PrimaryPeerAddressPrefix": "192.168.11.16/30",
                                       "SecondaryPeerAddressPrefix": "192.168.11.20/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 110,
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "4",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : [
                                     {
                                       "Name": "Auth4-ASH-Cust11",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/authorizations/Auth4-ASH-Cust11",
                                       "AuthorizationKey": "<authorizationKey>",
                                       "AuthorizationUseStatus": "Used",
                                       "ProvisioningState": "Succeeded"
                                     }
                                   ]
AllowClassicOperations           : False
GatewayManagerEtag               : 4
```

Mit der Peering-ID und dem Autorisierungsschlüssel können Sie die Global Reach-Verbindung unter der ExpressRoute-Leitung von Fabrikam erstellen. Melden Sie sich beim Azure-Konto von Fabrikam an. Sollten mehrere Abonnements vorhanden sein, wählen Sie das passende Abonnement aus.

Global Reach erstellt redundante Point-to-Point-Verbindungen zwischen den beiden MSEE-Paaren. Für die beiden Point-to-Point-Verbindungen müssen Sie ein Adresspräfix vom Typ „/29“ angeben. (Im aktuellen Beispiel wird 192.168.11.64/29 verwendet.) Erstellen Sie die Global Reach-Verbindung mithilfe der folgenden Befehle:

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix "__.__.__.__/29" -AuthorizationKey "########-####-####-####-############"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Nach Ausführung der obigen Befehle dauert es einige Minuten, bis die redundanten Global Reach-Verbindungen erstellt wurden.

### <a name="verify-global-reach"></a>Überprüfen von Global Reach

Die folgende Tabelle zeigt die Routentabelle des primären privaten Peerings der ExpressRoute-Lösung von Contoso Ltd. nach der Konfiguration von Global Reach:

[![8]][8]

Die folgende Tabelle zeigt die Routentabelle des primären privaten Peerings der ExpressRoute-Lösung von Fabrikam Inc. nach der Konfiguration von Global Reach:

[![9]][9]

In den obigen Tabellen sind alle erwarteten Zielnetzwerkpräfixe (unter „NETWORK“) sowie die entsprechenden nächsten Hops (unter „NEXT HOP“) aufgeführt.

Oben sehen Sie das Blatt zum Abrufen der Routingtabelle, auf das Sie im Azure-Portal unter „Privates Peering“ für eine ExpressRoute-Leitung zugreifen können. Eine ExpressRoute-Routingtabelle kann auch mit dem folgenden PowerShell-Befehl angezeigt werden:

```powershell
Get-AzExpressRouteCircuitRouteTable -DevicePath 'primary' -ExpressRouteCircuitName "Your_circuit_name" -PeeringType AzurePrivatePeering -ResourceGroupName "Your_resource_group"
```

Wenn Sie die Routingtabelle der sekundären MSEE anzeigen möchten, ersetzen Sie „primary“ im obigen Befehl durch „secondary“.

Als Nächstes überprüfen wir durch Pingen verschiedener Ziele aus unterschiedlichen Netzwerken die Konnektivität auf der Datenebene. Mit den drei folgenden Pings wird die Konnektivität auf der Datenebene mit dem lokalen Netzwerk von Contoso, dem virtuellen Azure-Netzwerk von Contoso und dem virtuellen Azure-Netzwerk von Fabrikam über das lokale Netzwerk von Fabrikam überprüft:


    C:\Users\FabrikamUser>ping 10.1.11.10
    
    Pinging 10.1.11.10 with 32 bytes of data:
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    
    Ping statistics for 10.1.11.10:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 69ms, Maximum = 69ms, Average = 69ms
    
    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=82ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=81ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 82ms, Average = 78ms
    
    C:\Users\FabrikamUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=2ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 3ms, Average = 2ms


Mit den beiden folgenden Pings wird die Konnektivität auf der Datenebene mit dem virtuellen Azure-Netzwerk von Contoso und dem virtuellen Azure-Netzwerk von Fabrikam über das lokale Netzwerk von Contoso überprüft:

    C:\Users\ContosoUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=6ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 5ms, Maximum = 6ms, Average = 5ms
    
    C:\Users\ContosoUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=74ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 73ms, Maximum = 74ms, Average = 73ms

Mit dem folgenden Ping wird die Konnektivität auf der Datenebene mit dem virtuellen Azure-Netzwerk von Contoso über das virtuelle Azure-Netzwerk von Fabrikam überprüft:

    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=78ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 78ms, Average = 77ms

## <a name="optimization-for-latency-sensitive-traffic"></a>Optimierung für wartezeitempfindlichen Datenverkehr

Global Reach leitet Datenverkehr über Microsoft Edge-Geräte. Für das spezifische Szenario in diesem Artikel können Sie das Routing zwischen den beiden VNETs durch Aktivieren von VNET-Peering optimieren. Analog dazu können Sie das Routing zwischen den beiden lokalen Netzwerken mithilfe eines Dienstanbieters optimieren, der eine direktere WAN-Verbindung zwischen den Standorten bereitstellen kann. In solchen Szenarien können Sie Global Reach-Routing als Failbackoption für diese Verbindungen verwenden. 

## <a name="next-steps"></a>Nächste Schritte

Global Reach wurde noch nicht in allen Ländern eingeführt. Ob Global Reach in den gewünschten Ländern verfügbar ist, erfahren Sie unter [ExpressRoute Global Reach (Vorschau)][Global Reach].

<!--Image References-->
[1]: ./media/expressroute-globalreach-applicationscenario/PreMergerScenario.png "Das Anwendungsszenario"
[2]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Premerger.png "ExpressRoute-Routingtabelle vor der Fusion (Contoso)"
[3]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Premerger.png "ExpressRoute-Routingtabelle vor der Fusion (Fabrikam)"
[4]: ./media/expressroute-globalreach-applicationscenario/S2SVPN-Solution.png "Herstellen von Verbindungen zwischen lokalen Netzwerken"
[5]: ./media/expressroute-globalreach-applicationscenario/ExR-XConnect-Solution.png "ExpressRoute-Querverbindungen"
[6]: ./media/expressroute-globalreach-applicationscenario/VNet-peering-Solution.png "VNET-Peering"
[7]: ./media/expressroute-globalreach-applicationscenario/GlobalReach-Solution.png "Global Reach"
[8]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Postmerger.png "ExpressRoute-Routingtabelle mit Global Reach (Contoso)"
[9]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Postmerger.png "ExpressRoute-Routingtabelle mit Global Reach (Fabrikam)"

<!--Link References-->
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach






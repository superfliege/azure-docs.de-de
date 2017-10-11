---
title: 'Abrufen von ARP-Tabellen: Ressourcen-Manager: Azure ExpressRoute-Problembehandlung | Microsoft Docs'
description: "Diese Seite enthält Anweisungen zum Abrufen der ARP Tabellen für eine ExpressRoute-Verbindung"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Das Abrufen der ARP Tabellen im Ressourcen-Manager-Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [PowerShell - Ressourcen-Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Classic](expressroute-troubleshooting-arp-classic.md)
> 
> 

Dieser Artikel führt Sie durch die Schritte, um zu erfahren, die ARP-Tabellen für ExpressRoute-Verbindung. 

> [!IMPORTANT]
> Dieses Dokument richtet sich darin, Ihnen die einfache Probleme diagnostizieren und korrigieren. Sie dient kein Ersatz für Microsoft-Support. Müssen Sie ein Support-Ticket mit öffnen [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Wenn Sie nicht zur Behebung des Problems mithilfe der unten beschriebenen Anleitungen.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Behandeln von Resolution Protocol (ARP) und ARP-Tabelle
ARP (Address Resolution Protocol, versucht) ist eine Schicht-2-Protokoll definiert [RFC 826](https://tools.ietf.org/html/rfc826). ARP wird verwendet, um die Ethernet-Adresse (MAC-Adresse) mit einer IP-Adresse zugeordnet.

Die ARP-Tabelle enthält eine Zuordnung von der ipv4-Adresse und MAC-Adresse für einen bestimmten peering. Die ARP-Tabelle für eine ExpressRoute-Leitung peering enthält die folgenden Informationen für jede Schnittstelle (Primär oder sekundär)

1. Zuordnung des lokalen Routers Schnittstelle IP-Adresse in die MAC-Adresse
2. Zuordnung von ExpressRoute-Router-Schnittstelle IP-Adresse in die MAC-Adresse
3. Alter der Zuordnung

ARP-Tabellen können Schicht-2-Konfiguration zu überprüfen und grundlegende Problembehandlung layer-2 Konnektivitätsprobleme zu beheben. 

Beispiel-ARP-Tabelle: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


Der folgende Abschnitt enthält Informationen für die Anzeige der ARP-Tabellen, die edgerouter des ExpressRoute angezeigt wird. 

## <a name="prerequisites-for-learning-arp-tables"></a>Voraussetzungen für das Erlernen von ARP-Tabellen
Stellen Sie sicher, dass Sie die folgenden verfügen, bevor Sie weitere Status

* Eine gültige ExpressRoute-Verbindung, die mit mindestens ein peering konfiguriert. Die Verbindung muss vollständig von der Dienstanbieter konfiguriert werden. Sie (oder Ihrem Dienstanbieter) muss mindestens eines der kann (Azure Public für private, Azure und Microsoft) für diese Leitung konfiguriert haben.
* IP-Adressbereiche für die Konfiguration der kann (Azure Public für private, Azure und Microsoft) verwendet. Überprüfen Sie die IP-Adresse Zuweisung Beispiele in der [ExpressRoute routing Anforderungsseite](expressroute-routing.md) abzurufenden einen Überblick über die Schnittstellen, die auf der Seite und auf der Seite ExpressRoute wie IP-Adressen zugeordnet sind. Erhalten Sie anhand der Informationen in der Peer-Konfiguration der [Konfigurationsseite der ExpressRoute-peering](expressroute-howto-routing-arm.md).
* Informationen aus Ihrem Netzwerkteam / Verbindungsanbieter für die MAC-Adressen von Schnittstellen, die mit dieser IP-Adressen verwendet.
* Sie müssen das neueste PowerShell-Modul für Azure (Version 1,50 oder höher) verfügen.

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Beim Abrufen der ARP-Tabellen für ExpressRoute-Verbindung
Dieser Abschnitt enthält Anweisungen für die Anzeige der ARP-Tabellen pro peering mithilfe von PowerShell. Sie oder Ihre Verbindungsanbieter muss vor der weiteren Fortschreiten peering konfiguriert haben. Jede Verbindung verfügt über zwei Pfade (Primär oder sekundär). Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-Tabellen für Azure privates peering
Das folgende Cmdlet bietet ARP Tabellen für Azure privates peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Beispielausgabe wird unter einem der Pfade angezeigt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-Tabellen für die öffentliche Azure-peering
Das folgende Cmdlet bietet ARP Tabellen für die öffentliche Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Beispielausgabe wird unter einem der Pfade angezeigt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-Tabellen für Microsoft-peering
Das folgende Cmdlet enthält ARP Tabellen für Microsoft-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Beispielausgabe wird unter einem der Pfade angezeigt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Verwenden dieser Informationen
Die ARP-Tabelle ein Peering kann verwendet werden, um zu bestimmen, Schicht-2-Konfiguration und der Konnektivität zu überprüfen. Dieser Abschnitt enthält einen Überblick darüber, wie ARP-Tabellen in verschiedenen Szenarien aussieht.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Wenn eine Verbindung in einem funktionsfähigen Zustand (erwarteten Zustand) ist der ARP-Tabelle
* Die ARP-Tabelle wird ein Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse und einen ähnlichen Eintrag für die Microsoft-Seite haben. 
* Das letzte Oktett der lokalen IP-Adresse wird immer eine ungerade Anzahl ergeben.
* Das letzte Oktett der IP-Adresse von Microsoft wird immer eine gerade Zahl sein.
* Dieselbe MAC-Adresse wird auf der Microsoft-Seite für alle 3 kann (primäre / sekundäre) angezeigt. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP Tabelle, wenn lokale / Konnektivität Anbieterseite sind Probleme aufgetreten
Wenn Probleme mit der lokalen oder Verbindungsanbieter, die möglicherweise angezeigt werden, dass entweder nur ein Eintrag, in der ARP-Tabelle oder die lokale MAC-Adresse angezeigt werden wird unvollständig angezeigt. Dadurch wird die Zuordnung zwischen dem MAC-Adresse und die IP-Adresse verwendet, in der Microsoft-Seite angezeigt. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

auf der Registerkarte
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Öffnen Sie eine Supportanfrage mit Ihrem Dienstanbieter, um solche Probleme zu debuggen. Wenn die ARP-Tabelle keine IP-Adressen der Schnittstellen, MAC-Adressen zugeordnet ist, überprüfen Sie die folgende Informationen:
> 
> 1. Wenn die erste IP-Adresse der /30 Subnetz zugewiesen, für die Verknüpfung zwischen dem einen MSEE PR und einen MSEE verwendet wird, auf der Oberfläche einen MSEE-Pr. Azure verwendet immer die zweite IP-Adresse für MSEEs.
> 2. Überprüfen Sie, ob der Kunde (C#-Tag) und VLAN-Tags Service (S-Tag) für einen MSEE PR und einen MSEE Paar übereinstimmen.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-Tabelle, wenn Microsoft-Seite Probleme aufgetreten sind.
* Sie sehen keine ARP-Tabelle, die für ein peering, wenn Probleme, auf der Microsoft-Seite vorliegen angezeigt. 
* Öffnen Sie ein Support-Ticket mit [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie, dass Sie ein Problem mit der Schicht-2-Konnektivität verfügen. 

## <a name="next-steps"></a>Nächste Schritte
* Überprüfen Sie die Layer 3-Konfigurationen für Ihre ExpressRoute-Verbindung
  * Abrufen Sie Route zum Ermitteln des Status der BGP-Sitzungen Zusammenfassung 
  * Abrufen der Routentabelle, um zu bestimmen, welche Präfixe über ExpressRoute angekündigt werden
* Überprüfen Sie die Datenübertragung anhand Bytes ein- / Ausgabeparameter
* Öffnen Sie ein Support-Ticket mit [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Wenn weiterhin Probleme auftreten.


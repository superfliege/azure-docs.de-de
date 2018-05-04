---
title: Informationen zum VPN-Gateway für Azure Stack | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu VPN-Gateways, die Sie mit Azure Stack verwenden, sowie zu deren Konfiguration.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: 9c821f20ce5826666a05121e1a39882fae0930d3
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Informationen zum VPN-Gateway für Azure Stack
*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*


Um Netzwerkdatenverkehr zwischen Ihrem virtuellen Azure-Netzwerk und Ihrem lokalen Standort senden zu können, müssen Sie für Ihr virtuelles Netzwerk zunächst ein Gateway für virtuelle Netzwerke erstellen.

Ein VPN Gateway ist eine Art von Gateway für virtuelle Netzwerke, mit dem verschlüsselter Datenverkehr über eine öffentliche Verbindung gesendet wird. Mit VPN-Gateways können Sie Datenverkehr sicher zwischen einem virtuellen Netzwerk in Azure Stack und einem virtuellen Netzwerk in Azure oder zwischen einem virtuellen Netzwerk und einem anderen, mit einem VPN-Gerät verbundenen Netzwerk senden.

Der gewünschte Gatewaytyp wird beim Erstellen eines Gateways für virtuelle Netzwerke angegeben. Azure Stack unterstützt eine einzelne Art von Gateway für virtuelle Netzwerke: VPN.

Jedes virtuelle Netzwerk kann über zwei Gateways für virtuelle Netzwerke, aber nur über eins von jedem Typ verfügen. Abhängig von den gewählten Einstellungen können Sie ggf. mehrere Verbindungen mit einem einzelnen VPN-Gateway erstellen. Ein Beispiel hierfür ist eine Verbindungskonfiguration mit mehreren Standorten.

> [!NOTE]
> In Azure muss der Bandbreitendurchsatz für die gewählte VPN-Gateway-SKU auf alle Verbindungen aufgeteilt werden, die damit hergestellt wurden.  In Azure Stack wird der Bandbreitenwert für die VPN-Gateway-SKU auf jede Verbindungsressource angewendet, die damit verbunden ist.     

> Ein Beispiel: In Azure bietet die VPN-Gateway-SKU „Basic“ eine aggregierte Durchsatzkapazität von etwa 100 Mbit/s.  Wenn Sie zwei Verbindungen mit diesem VPN-Gateway erstellen und eine dieser Verbindungen eine Bandbreite von 50 Mbit/s beansprucht, stehen für die andere Verbindung noch 50 Mbit/s zur Verfügung.   

> In Azure Stack wird *jeder* Verbindung mit der VPN-Gateway-SKU „Basic“ ein Durchsatz von 100 Mbit/s zugeteilt.

## <a name="configuring-a-vpn-gateway"></a>Konfigurieren von VPN Gateway
Eine VPN Gateway-Verbindung basiert auf mehreren, mit spezifischen Einstellungen konfigurierten Ressourcen. Die meisten der Ressourcen können separat konfiguriert werden. In manchen Fällen ist allerdings eine bestimmte Reihenfolge einzuhalten.

### <a name="settings"></a>Einstellungen
Die Einstellungen, die Sie für die einzelnen Ressourcen auswählen, sind für eine erfolgreiche Verbindungserstellung entscheidend. Informationen zu einzelnen Ressourcen und Einstellungen für VPN Gateway finden Sie unter [VPN-Gatewaykonfigurationseinstellungen für Azure Stack](azure-stack-vpn-gateway-settings.md). Dort können Sie sich ausführlicher mit Gatewaytypen, VPN-Typen, Verbindungstypen, Gatewaysubnetzen, lokalen Netzwerkgateways und verschiedenen anderen Ressourceneinstellungen beschäftigen, die Sie ggf. berücksichtigen sollten.

### <a name="deployment-tools"></a>Bereitstellungstools
Sie können zunächst mit einem Konfigurationstool wie dem Azure-Portal Ressourcen erstellen und konfigurieren. Später können Sie dann mit einem anderen Tool (beispielsweise PowerShell) zusätzliche Ressourcen konfigurieren oder ggf. vorhandene Ressourcen ändern. Derzeit können nicht alle Ressourcen und Ressourceneinstellungen über das Azure-Portal konfiguriert werden. Sollte ein bestimmtes Konfigurationstool benötigt werden, ist dies in den Anleitungen der Artikel zu den einzelnen Verbindungstopologien angegeben.

## <a name="connection-topology-diagrams"></a>Verbindungstopologiediagramme
Wichtig: Für VPN-Gateway-Verbindungen sind verschiedene Konfigurationen verfügbar. Sie müssen ermitteln, welche Konfiguration am besten zu Ihren Anforderungen passt. Die folgenden Abschnitte enthalten Informationen und Topologiediagramme zu den folgenden VPN-Gatewayverbindungen: Die folgenden Abschnitte enthalten Tabellen mit Folgendem:

- Verfügbares Bereitstellungsmodell
- Verfügbare Konfigurationstools
- Direkte Links zu Artikeln, sofern verfügbar

Orientieren Sie sich bei der Wahl einer geeigneten Verbindungstopologie an den Diagrammen und Beschreibungen in den folgenden Abschnitten. Die Diagramme zeigen die grundlegenden Topologien, aber Sie können auch komplexere Topologien erstellen, indem Sie die Diagramme als Anhaltspunkte verwenden.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Site-to-Site und Multi-Site (IPsec-/IKE-VPN-Tunnel)
### <a name="site-to-site"></a>Site-to-Site
Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das nicht hinter einer NAT-Einheit angeordnet ist. S2S-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.    
![Konfigurationsbeispiel für eine Site-to-Site-VPN-Verbindung](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multi-Site
Bei dieser Art von Verbindung handelt es sich um eine Abwandlung der Site-to-Site-Verbindung. Sie erstellen mehrere VPN-Verbindung über Ihr Gateway für virtuelle Netzwerke, durch die in der Regel mehrere lokale Standorte verbunden werden. Bei Verwendung mehrerer Verbindungen müssen Sie den VPN-Typ „RouteBased“ verwenden (wird bei Verwendung klassischer VNets als dynamisches Gateway bezeichnet). Da jedes virtuelle Netzwerk nur über ein einzelnes VPN-Gateway verfügen kann, wird die verfügbare Bandbreite von allen über das Gateway laufenden Verbindungen gemeinsam genutzt. Dies wird häufig als Multi-Site-Verbindung bezeichnet.   
![Beispiel für Multi-Site-Verbindung per Azure VPN Gateway](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## <a name="gateway-skus"></a>Gateway-SKUs
Beim Erstellen eines Gateways für virtuelle Netzwerke für Azure Stack müssen Sie die gewünschte Gateway-SKU angeben. Folgende SKUs für VPN-Gateways werden unterstützt:
- Basic
- Standard
- HighPerformance

Wenn Sie eine höhere Gateway-SKU (beispielsweise Standard über Basic oder HighPerformance über Standard oder Basic) auswählen, werden dem Gateway mehr CPUs und mehr Netzwerkbandbreite zugeteilt. Dadurch kann das Gateway einen höheren Netzwerkdurchsatz an das virtuelle Netzwerk bewältigen.

Die Gateway-SKU „UltraPerformance“ wird ausschließlich mit ExpressRoute verwendet und von Azure Stack nicht unterstützt.

Berücksichtigen Sie bei der Auswahl einer SKU Folgendes:
- Richtlinienbasierte Gateways werden von Azure Stack nicht unterstützt.
- Das Border Gateway-Protokoll (BGP) wird in der Basic-SKU nicht unterstützt.
- Die gemeinsame Verwendung von VPN Gateway und ExpressRoute wird in Azure Stack nicht unterstützt.
- Aktiv-Aktiv-S2S-VPN-Gatewayverbindungen können nur in der HighPerformance-SKU konfiguriert werden.

## <a name="estimated-aggregate-throughput-by-sku"></a>Voraussichtlicher aggregierter Durchsatz nach SKU
In der folgenden Tabelle sind die Gatewaytypen und der geschätzte zusammengefasste Durchsatz nach Gateway-SKU angegeben.

|   | VPN-Gatewaydurchsatz *(1)* | Max. IPsec-Tunnel für das VPN-Gateway *(2)* |
|-------|-------|-------|
|**Basic-SKU** ***(3)***    | 100 MBit/s  | 10    |
|**Standard-SKU**       | 100 MBit/s  | 10    |
|**High-Performance-SKU** | 200 MBit/s    | 5 |
***(1)*** Der VPN-Durchsatz ist kein garantierter Durchsatz für standortübergreifende Verbindungen über das Internet. Hierbei wird der maximal mögliche Durchsatz gemessen.  
***(2)*** Bei der maximalen Tunnelanzahl handelt es sich um die Summe pro Azure Stack-Bereitstellung für ALLE Abonnements.
***(3)*** BGP wird für die Basic-SKU nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über [Einstellungen für VPN-Gateways](azure-stack-vpn-gateway-settings.md) für Azure Stack.

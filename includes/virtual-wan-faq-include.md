---
title: Includedatei
description: Includedatei
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/10/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: de744a4a23b246223ed0f42f3d079b1ac2e5521a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47008820"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Was ist der Unterschied zwischen einem Gateway für virtuelle Azure-Netzwerke (VPN Gateway) und einem Azure Virtual WAN-Gateway (vpngateway)?

Virtual WAN ermöglicht eine umfassende Site-to-Site-Konnektivität und ist auf Durchsatz, Skalierbarkeit und Benutzerfreundlichkeit ausgelegt. ExpressRoute und Point-to-Site-Konnektivität sind derzeit in der Vorschauphase. CPE-Zweigstellengeräte werden automatisch bereitgestellt und mit Azure Virtual WAN verbunden. Diese Geräte sind über ein ständig wachsendes Ökosystem von SD-WAN- und VPN-Partnern erhältlich. Siehe die [Liste der bevorzugten Partner](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Welche Geräteanbieter (Virtual WAN-Partner) werden beim Start unterstützt? 

Derzeit wird die vollständig automatisierte Virtual WAN-Umgebung von Citrix und Riverbed unterstützt. Weitere Informationen finden Sie auf der Seite mit den Informationen zu [Virtual WAN-Partnern](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Muss ich ein Gerät eines bevorzugten Partners nutzen?

Nein. Sie können ein beliebiges VPN-fähiges Gerät nutzen, das die Anforderungen von Azure für die IKEv2/IKEv1-IPsec-Unterstützung erfüllt.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Wie automatisieren Virtual WAN-Partner die Konnektivität mit Azure Virtual WAN?

Softwaredefinierte Konnektivitätslösungen verwalten ihre Zweigstellengeräte normalerweise mithilfe eines Controllers oder über ein Center für die Gerätebereitstellung. Für den Controller können Azure-APIs verwendet werden, um die Konnektivität mit Azure Virtual WAN zu automatisieren. Weitere Informationen finden Sie unter „Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner“.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Ändern sich durch Virtual WAN vorhandene Konnektivitätsfeatures?   

Es werden keine vorhandenen Azure-Konnektivitätsfeatures geändert.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sind neue Resource Manager-Ressourcen für Virtual WAN verfügbar?
  
Ja. Mit Virtual WAN werden neue Resource Manager-Ressourcen eingeführt. Weitere Informationen finden Sie in der entsprechenden [Übersicht](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Wie viele VPN-Geräte können sich mit einem einzelnen Hub verbinden?

Pro virtuellem Hub werden bis zu 100 Verbindungen unterstützt. Jede Verbindung umfasst zwei Tunnel mit einer Aktiv-Aktiv-Konfiguration. Die Tunnel enden in einem virtuellen Azure-Hub (vpngateway).

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Kann das lokale VPN-Gerät eine Verbindung mit mehreren Hubs herstellen?

Ja. Der Datenverkehr erfolgt zu Beginn vom lokalen Gerät zum nächsten Microsoft-Edge und dann zum Virtual Hub.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wird das globale VNET-Peering für Azure Virtual WAN unterstützt? 

 Nein.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Können Spoke-VNETs, die über einen virtuellen Hub verbunden sind, miteinander kommunizieren?

Ja. Sie können das VNET-Peering direkt zwischen Spokes durchführen, die mit einem virtuellen Hub verbunden sind. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kann ich mein bevorzugtes virtuelles Netzwerkgerät (in einem NVA-VNET) mit Azure Virtual WAN bereitstellen und verwenden?

Ja. Sie können Ihr bevorzugte VNET des virtuellen Netzwerkgeräts mit Azure Virtual WAN verbinden. Verbinden Sie zunächst das VNET des virtuellen Netzwerkgeräts über eine Hub-VNET-Verbindung mit dem Hub. Erstellen Sie dann eine Virtual Hub-Route mit einem nächsten Hop, der auf das virtuelle Gerät zeigt. Sie können der Routingtabelle des virtuellen Hubs mehrere Routen zuweisen. Alle Spokes, die mit dem VNET des virtuellen Netzwerkgeräts verbunden sind, müssen außerdem mit dem virtuellen Hub verbunden sein. Dies stellt sicher, dass die Spoke-VNET-Routen an lokale Systeme weitergegeben werden.

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Kann ein NVA-VNET über ein Gateway für virtuelle Netzwerke verfügen?

Nein. Das NVA-VNET kann nicht über ein Gateway für virtuelle Netzwerke verfügen, wenn es mit dem virtuellen Hub verbunden ist. 

### <a name="is-there-support-for-bgp"></a>Ist Unterstützung für BGP vorhanden?

Ja. BGP wird unterstützt. Um sicherzustellen, dass Routen für ein VNET eines virtuellen Netzwerkgeräts richtig angekündigt werden, muss BGP für die Spokes deaktiviert werden, wenn sie mit einem VNET eines virtuellen Netzwerkgeräts verbunden sind (das wiederum mit einem virtuellen Hub verbunden ist). Verbinden Sie zusätzlich die Spoke-VNETs mit dem virtuellen Hub, um sicherzustellen, dass die Spoke-VNET-Routen an lokale Systeme weitergegeben werden.
Kann ich Datenverkehr per UDR auf dem virtuellen Hub weiterleiten?

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kann ich Datenverkehr per UDR auf dem virtuellen Hub weiterleiten?

Ja, Sie können Datenverkehr mithilfe der Routingtabelle des virtuellen Hubs an ein VNET weiterleiten.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sind Informationen zu Lizenzen oder Preisen für Virtual WAN vorhanden?
 
Ja. Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Wie erfolgt das Onboarding für neue Partner, die nicht in der Liste mit den Einführungspartnern aufgeführt sind?

Senden Sie eine E-Mail an azurevirtualwan@microsoft.com. Ein idealer Partner verfügt über ein Gerät, das für IKEv1- oder IKEv2-IPSec-Konnektivität bereitgestellt werden kann.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Ist es möglich, Azure Virtual WAN mit einer Resource Manager-Vorlage zu erstellen?

Eine einfache Konfiguration eines Virtual WAN mit einem Hub und einem VPN-Standort kann mit einer [Azure-Schnellstartvorlage](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network) erstellt werden. Virtual WAN ist in erster Linie ein von REST oder vom Portal gesteuerter Dienst.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Ist die Konnektivität zwischen Zweigstellen für Virtual WAN zulässig?

Ja. Die Konnektivität zwischen Zweigstellen ist in Virtual WAN für VPN und VPN zu ExpressRoute zulässig. Site-to-Site-VPN ist allgemein verfügbar. ExpressRoute und Point-to-Site-Konnektivität sind derzeit in der Vorschauphase.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Läuft der Datenverkehr zwischen den Zweigstellen über Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Inwiefern unterscheidet sich Virtual WAN vom vorhandenen Azure-Gateway für virtuelle Netzwerke?

Das VPN des Gateways für virtuelle Netzwerke ist auf 30 Tunnel begrenzt. Für Verbindungen sollten Sie bei einem größeren VPN-Umfang Virtual WAN verwenden. Sie können im Hub bis zu 100 Zweigstellenverbindungen mit 2 Gbit/s miteinander verbinden. Eine Verbindung ist ein Aktiv-Aktiv-Tunnel vom lokalen VPN-Gerät zum virtuellen Hub. Sie können einen Hub pro Region haben, was bedeutet, dass Sie mehr als 100 Zweigstellen über Hubs verbinden können.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Ist für diese Virtual WAN-Instanz eine ExpressRoute-Verbindung mit jeder Site erforderlich?

Nein. Für diese Virtual WAN-Instanz ist keine ExpressRoute-Verbindung mit jeder Site erforderlich. Es wird IPsec-Site-to-Site-Standardkonnektivität über Internetlinks vom Gerät zu einem Azure Virtual WAN-Hub genutzt. Ihre Standorte können über eine ExpressRoute-Leitung mit einem Anbieternetzwerk verbunden sein. Für Standorte, die über ExpressRoute in Virtual Hub (in der Vorschauphase) verbunden sind, können Standorte Zweigstelle-zu-Zweigstelle-Datenverkehr zwischen VPN und ExpressRoute aufweisen. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Gibt es bei Verwendung von Azure Virtual WAN einen Grenzwert für den Netzwerkdurchsatz?

Die Anzahl von Zweigstellen ist auf 100 Verbindungen pro Hub/Region und auf maximal 2 G im Hub begrenzt.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Lässt Virtual WAN für das lokale Gerät die parallele Nutzung mehrerer ISPs zu, oder wird immer nur ein VPN-Tunnel verwendet?

Ja. Sie können je nach Zweigstellengerät Aktiv-Aktiv-Tunnel (2 Tunnel = 1 Azure Virtual WAN-Verbindung) für eine einzelne Zweigstelle verwenden.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Wie wird Datenverkehr über den Azure-Backbone geleitet?

Für den Datenverkehr wird das folgende Muster verwendet: Zweigstellengerät > ISP > Microsoft Edge > Microsoft-Domänencontroller > Microsoft Edge > ISP > Zweigstellengerät.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Was wird bei diesem Modell für jede Site benötigt? Nur eine Internetverbindung?

Ja. Eine Internetverbindung und ein physisches Gerät – vorzugsweise von einem unserer integrierten [Partner](https://go.microsoft.com/fwlink/p/?linkid=2019615). Sie können optional die Konfiguration und Konnektivität mit Azure auf Ihrem bevorzugten Gerät manuell verwalten.

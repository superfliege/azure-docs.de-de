---
title: Includedatei
description: Includedatei
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/18/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d059cab5668eef8d4dafc1442ca9749a7dcf8c9d
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39162513"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Was ist der Unterschied zwischen einem Gateway für virtuelle Azure-Netzwerke (VPN Gateway) und einem Azure Virtual WAN-Gateway (vpngateway)?

Virtual WAN ermöglicht eine umfassende Site-to-Site-Konnektivität und ist auf Durchsatz, Skalierbarkeit und Benutzerfreundlichkeit ausgelegt. CPE-Branchgeräte werden automatisch bereitgestellt und mit Azure Virtual WAN verbunden. Diese Geräte sind über ein ständig wachsendes Ökosystem von SD-WAN- und VPN-Partnern erhältlich.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Welche Geräteanbieter (Virtual WAN-Partner) werden beim Start unterstützt? 

Derzeit wird die vollständig automatisierte Virtual WAN-Umgebung von Citrix und Riverbed unterstützt. Weitere Informationen finden Sie auf der Seite mit den Informationen zu [Virtual WAN-Partnern](https://aka.ms/virtualwan).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Muss ich ein Gerät eines bevorzugten Partners nutzen?

Nein. Sie können ein beliebiges VPN-fähiges Gerät nutzen, das die Anforderungen der Vorschauversion für die IKEv2-IPsec-Unterstützung erfüllt.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Wie automatisieren Virtual WAN-Partner die Konnektivität mit Azure Virtual WAN?

Softwaredefinierte Konnektivitätslösungen verwalten ihre Branchgeräte normalerweise mithilfe eines Controllers oder über ein Center für die Gerätebereitstellung. Für den Controller können Azure-APIs verwendet werden, um die Konnektivität mit Azure Virtual WAN zu automatisieren. Weitere Informationen finden Sie unter [Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner (Vorschauversion)](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Ändern sich durch Virtual WAN vorhandene Konnektivitätsfeatures?   

Es werden keine vorhandenen Azure-Konnektivitätsfeatures geändert.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sind neue Resource Manager-Ressourcen für Virtual WAN verfügbar?
  
Ja. Mit Virtual WAN werden neue Resource Manager-Ressourcen eingeführt. Weitere Informationen finden Sie in der entsprechenden [Übersicht](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Gelten für die Teilnahme an der Vorschauversion besondere Anforderungen? 

Bevor Sie eine Instanz von Azure Virtual WAN konfigurieren können, müssen Sie Ihr Abonnement zunächst für die Vorschauversion registrieren. Senden Sie für die Registrierung eine E-Mail mit Ihrer Abonnement-ID an <azurevirtualwan@microsoft.com>. Sie erhalten eine E-Mail zurück, nachdem Ihr Abonnement registriert wurde. Vor dem Erhalt der Bestätigung, dass Ihr Abonnement registriert wurde, können Sie nicht mit Virtual WAN im Portal arbeiten.

Überlegungen:

* Die Vorschauversion ist nur auf öffentliche Azure-Regionen beschränkt.
* Pro virtuellem Hub werden bis zu 100 Verbindungen unterstützt. Jede Verbindung umfasst zwei Tunnel mit einer Aktiv-Aktiv-Konfiguration. Die Tunnel enden in einem virtuellen Azure-Hub (vpngateway).
* Erwägen Sie die Nutzung dieser Vorschauversion, wenn Folgendes gilt:
  * Sie möchten eine aggregierte Bandbreite von weniger als 1 GBit/s pro virtuellem Hub bereitstellen.
  * Sie verfügen über ein VPN-Gerät, das die routenbasierte Konfiguration und die IKEv2-IPsec-Konnektivität unterstützt.
  * Sie möchten die SD-WAN-Nutzung und den Betrieb von Branchgeräten über Partner testen, die Unterstützung beim Start leisten (Riverbed und Citrix).<br>oder<br>Sie möchten die Konnektivität zwischen Branches und eine Verzweigung für die Azure-Konnektivität einrichten, die die Konfigurationsverwaltung Ihres lokalen Geräts umfasst. (Selbstkonfiguration)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wird das globale VNET-Peering für Azure Virtual WAN unterstützt? 

 Nein.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Können Spoke-VNETs, die über einen virtuellen Hub verbunden sind, miteinander kommunizieren?

Ja. Sie können das VNET-Peering direkt zwischen Spokes durchführen, die mit einem virtuellen Hub verbunden sind. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kann ich mein bevorzugtes virtuelles Netzwerkgerät (in einem NVA-VNET) mit Azure Virtual WAN bereitstellen und verwenden?

Ja. Sie können Ihr bevorzugtes virtuelles Netzwerkgerät (NVA-VNET) mit Azure Virtual WAN verbinden, aber hierfür sind Routingfunktionen im Hub erforderlich. Diese Funktionen sind in Planung. Alle Spokes, die mit dem NVA-VNET verbunden sind, müssen außerdem mit dem virtuellen Hub verbunden sein. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Kann ein NVA-VNET über ein Gateway für virtuelle Netzwerke verfügen?

Nein. Das NVA-VNET kann nicht über ein Gateway für virtuelle Netzwerke verfügen, wenn es mit dem virtuellen Hub verbunden ist. 

### <a name="is-there-support-for-bgp"></a>Ist Unterstützung für BGP vorhanden?

Ja. BGP wird unterstützt. Um sicherzustellen, dass Routen für ein NVA-VNET richtig angekündigt werden, muss für die Spokes BGP deaktiviert werden, wenn sie mit einem NVA-VNET verbunden sind (das wiederum mit einem virtuellen Hub verbunden ist). Verbinden Sie die Spoke-VNETs mit dem virtuellen Hub.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kann ich Datenverkehr per UDR auf dem virtuellen Hub weiterleiten?

Dies ist in Planung. Halten Sie sich auf dem Laufenden.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sind Informationen zu Lizenzen oder Preisen für Virtual WAN vorhanden?
 
Während der Vorschauphase fallen keine zusätzlichen Gebühren an. Die aktuellen [Preise für Azure VPN und ausgehenden Datenverkehr](https://azure.microsoft.com/pricing/details/vpn-gateway/) bleiben während der Vorschauphase unverändert.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Wie erfolgt das Onboarding für neue Partner, die nicht in der Liste mit den Einführungspartnern aufgeführt sind?

Senden Sie eine E-Mail an azurevirtualwan@microsoft.com. Ein idealer Partner verfügt über ein Gerät, das für IKEv2-IPSec-Konnektivität bereitgestellt werden kann.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Ist es möglich, Azure Virtual WAN mit einer Resource Manager-Vorlage zu erstellen?

Wir arbeiten daran. Derzeit basiert der Dienst auf REST und auf dem Portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Ist die Konnektivität zwischen Branches für Virtual WAN zulässig?

Ja. Die Konnektivität zwischen Branches ist für Virtual WAN zulässig.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Läuft der Datenverkehr zwischen den Branches über Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Inwiefern unterscheidet sich Virtual WAN vom vorhandenen Azure-Gateway für virtuelle Netzwerke?

Das VPN des Gateways für virtuelle Netzwerke ist auf 30 Tunnel begrenzt. Für Verbindungen sollten Sie bei einem größeren VPN-Umfang Virtual WAN verwenden. In der öffentlichen Vorschauphase liegt der Grenzwert hierfür bei 100 Branchverbindungen mit 1 GBit/s im Hub.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Ist für diese Virtual WAN-Instanz eine ExpressRoute-Verbindung mit jeder Site erforderlich?

Nein. Für diese Virtual WAN-Instanz ist keine ExpressRoute-Verbindung mit jeder Site erforderlich. Es wird IPsec-Site-to-Site-Standardkonnektivität über Internetlinks vom Gerät zu einem Azure Virtual WAN-Hub genutzt.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Gibt es bei Verwendung von Azure Virtual WAN einen Grenzwert für den Netzwerkdurchsatz?

In der öffentlichen Vorschauphase ist die Anzahl von Branches auf 100 Verbindungen pro Hub/Region und auf maximal 1 G im Hub begrenzt.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Lässt Virtual WAN für das lokale Gerät die parallele Nutzung mehrerer ISPs zu, oder wird immer nur ein VPN-Tunnel verwendet?

Ja. Sie können je nach Branchgerät Aktiv-Aktiv-Tunnel (2 Tunnel = 1 Azure Virtual WAN-Verbindung) für eine einzelne Branch verwenden.

### <a name="how-is-traffic-is-routed-on-the-azure-backbone"></a>Wie wird Datenverkehr über den Azure-Backbone geleitet?

Für den Datenverkehr wird das folgende Muster verwendet: Branchgerät > ISP > Microsoft Edge > Microsoft-Domänencontroller > Microsoft Edge > ISP > Branchgerät.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Was wird bei diesem Modell für jede Site benötigt? Nur eine Internetverbindung?

Ja. Eine Internetverbindung und ein physisches Gerät – vorzugsweise von einem unserer integrierten Partner. Dies gilt nicht, wenn Sie die Konfiguration und Konnektivität mit Azure über Ihr bevorzugtes Gerät manuell verwalten möchten.
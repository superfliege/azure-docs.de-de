---
title: "Herstellen einer Verbindung für ein virtuelles Azure-Netzwerk mit einem anderen VNet: Portal | Microsoft-Dokumentation"
description: Erstellen Sie eine VPN Gateway-Verbindung zwischen VNets unter Verwendung von Resource Manager und Azure-Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: cherylmc
ms.openlocfilehash: b2da1c7148e27ca8dd8eb774d4201415a969fada
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung über das Azure-Portal

Dieser Artikel zeigt, wie Sie eine VPN-Gatewayverbindung zwischen virtuellen Netzwerken erstellen. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen. Beim Verbinden von VNets aus unterschiedlichen Abonnements müssen die Abonnements nicht demselben Active Directory-Mandanten zugeordnet sein. 

Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell und das Azure-Portal. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure-Portal (klassisch)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinden von verschiedenen Bereitstellungsmodellen – Azure-Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinden von virtuellen Netzwerken aus verschiedenen Bereitstellungsmodellen mit PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![v2v-Diagramm](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen. Es ist möglich, Site-to-Site-Verbindungen (S2S, IPsec) anstelle von VNET-to-VNET-Verbindungen zwischen VNETs zu erstellen. Beide Verbindungstypen funktionieren bei der Kommunikation auf die gleiche Weise. Wenn Sie eine VNET-to-VNET-Verbindung erstellen und den Adressraum für ein VNET aktualisieren, führt das andere VNET die Weiterleitung an den aktualisierten Adressraum jedoch automatisch aus. Wenn Sie S2S-Verbindungen (IPsec) erstellen, müssen Sie den Adressraum für das lokale Netzwerkgateway manuell konfigurieren. Bei komplexen Konfigurationen empfiehlt es sich, anstelle von VNET-to-VNET-Verbindungen IPsec-Verbindungen zu erstellen. Dies ermöglicht es Ihnen, manuell einen zusätzlichen Adressraum für das lokale Netzwerkgateway anzugeben.

Wenn sich Ihre VNETs in der gleichen Region befinden, kann es hilfreich sein, sie mittels VNET-Peering zu verbinden. Beim VNET-Peering wird kein VPN-Gateway verwendet. Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).

Die VNET-zu-VNET-Kommunikation kann mit Konfigurationen für mehrere Standorte kombiniert werden. Auf diese Weise können Sie Netzwerktopologien einrichten, die wie in der folgenden Abbildung dargestellt standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren:

![Informationen zu Verbindungen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informationen zu Verbindungen")

### <a name="why-connect-virtual-networks"></a>Gründe für Verbindungen zwischen virtuellen Netzwerken

Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

* **Regionsübergreifende Georedundanz und Geopräsenz**

  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit Azure Traffic Manager und Load Balancer können Sie eine hoch verfügbare Workload mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.
* **Regionale Anwendungen mit mehreren Ebenen und Isolation oder Verwaltungsgrenze**

  * In derselben Region können Sie Anwendungen mit mehreren Ebenen und mehreren virtuellen Netzwerken einrichten, die aufgrund von Isolations- oder Verwaltungsanforderungen miteinander verbunden sind.

Sie können die Beispieleinstellungswerte nutzen, wenn Sie diese Schritte als Übung verwenden. Im Beispiel befinden sich die virtuellen Netzwerke im selben Abonnement, aber in verschiedenen Ressourcengruppen. Wenn sich Ihre VNets in unterschiedlichen Abonnements befinden, können Sie die Verbindung nicht im Portal herstellen. Sie können [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) oder die [CLI](vpn-gateway-howto-vnet-vnet-cli.md) verwenden. Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen](#faq) .

### <a name="values"></a>Beispieleinstellungen

**Werte für TestVNet1:**

* VNet-Name: TestVNet1
* Adressraum: 10.11.0.0/16
* Abonnement: Wählen Sie das zu verwendende Abonnement aus.
* Ressourcengruppe: TestRG1
* Standort: USA, Osten
* Subnetzname: FrontEnd
* Subnetzadressbereich: 10.11.0.0/24
* Name des Gatewaysubnetzes: GatewaySubnet (wird im Portal automatisch ausgefüllt)
* Adressbereich des Gatewaysubnetzes: 10.11.255.0/27
* DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers.
* Name des virtuellen Netzwerkgateways: TestVNet1GW
* Gatewaytyp: VPN
* VPN-Typ: Routenbasiert
* SKU: Wählen Sie die gewünschte Gateway-SKU aus.
* Name der öffentlichen IP-Adresse: TestVNet1GWIP
* Verbindungsname: TestVNet1toTestVNet4
* Gemeinsam verwendeter Schlüssel: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. In diesem Beispiel verwenden wir „abc123“. Wichtig: Der Wert muss beim Erstellen der Verbindung zwischen den VNets übereinstimmen.

**Werte für TestVNet4:**

* VNet-Name: TestVNet4
* Adressraum: 10.41.0.0/16
* Abonnement: Wählen Sie das zu verwendende Abonnement aus.
* Ressourcengruppe: TestRG4
* Standort: USA, Westen
* Subnetzname: FrontEnd
* Subnetzadressbereich: 10.41.0.0/24
* Name des Gatewaysubnetzes: GatewaySubnet (wird im Portal automatisch ausgefüllt)
* Adressbereich des Gatewaysubnetzes: 10.41.255.0/27
* DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers.
* Name des virtuellen Netzwerkgateways: TestVNet4GW
* Gatewaytyp: VPN
* VPN-Typ: Routenbasiert
* SKU: Wählen Sie die gewünschte Gateway-SKU aus.
* Name der öffentlichen IP-Adresse: TestVNet4GWIP
* Verbindungsname: TestVNet4toTestVNet1
* Gemeinsam verwendeter Schlüssel: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. In diesem Beispiel verwenden wir „abc123“. Wichtig: Der Wert muss beim Erstellen der Verbindung zwischen den VNets übereinstimmen.

## <a name="CreatVNet"></a>1. Erstellen und Konfigurieren von „TestVNet1“
Wenn Sie bereits über ein VNet verfügen, sollten Sie überprüfen, ob die Einstellungen mit Ihrem Entwurf des VPN Gateways kompatibel sind. Achten Sie besonders auf Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei. Wenn Ihr VNet mit den richtigen Einstellungen konfiguriert wurde, können Sie mit den Schritten im Abschnitt [Angeben eines DNS-Servers](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>So erstellen Sie ein virtuelles Netzwerk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Hinzufügen weiterer Adressräume und Erstellen von Subnetzen
Sie können dem VNet nach dem Erstellen weitere Adressräume hinzufügen und Subnetze erstellen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Erstellen eines Gatewaysubnetzes
Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz mit einem CIDR-Block vom Typ „/28“ oder „/27“, damit genügend IP-Adressen für zukünftige zusätzliche Konfigurationsanforderungen zur Verfügung stehen.

Wenn Sie diese Konfiguration als Übung erstellen, können Sie beim Erstellen des Gatewaysubnetzes [diese Beispieleinstellungen](#values) verwenden.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gatewaysubnetz
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Angeben eines DNS-Servers (optional)
DNS ist für VNet-zu-VNet-Verbindungen nicht erforderlich. Wenn Sie für Ressourcen, die in Ihren virtuellen Netzwerken bereitgestellt werden, die Namensauflösung verwenden möchten, müssen Sie aber einen DNS-Server angeben. Diese Einstellung bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Mit dieser Einstellung wird kein DNS-Server erstellt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Erstellen eines Gateways für das virtuelle Netzwerk
In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern. Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispieleinstellungen](#values) verwenden.

### <a name="to-create-a-virtual-network-gateway"></a>So erstellen Sie ein Gateway für das virtuelle Netzwerk
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Erstellen und Konfigurieren von „TestVNet4“
Nachdem Sie „TestVNet1“ konfiguriert haben, erstellen Sie „TestVNet4“. Wiederholen Sie hierzu die vorherigen Schritte mit den Werten von „TestVNet4“. Sie können mit dem Konfigurieren von „TestVNet4“ beginnen, auch wenn die Erstellung des virtuellen Netzwerkgateways für „TestVNet1“ noch nicht abgeschlossen ist. Achten Sie bei Verwendung eigener Werte darauf, dass sich die Adressräume nicht mit anderen VNets überschneiden, mit denen Sie eine Verbindung herstellen möchten.

## <a name="TestVNet1Connection"></a>7. Konfigurieren der TestVNet1-Gatewayverbindung
Nach Abschluss der Vorgänge für die virtuellen Netzwerkgateways für „TestVNet1“ und „TestVNet4“ können Sie die Verbindungen für das virtuelle Netzwerkgateway erstellen. In diesem Abschnitt erstellen Sie eine Verbindung von „VNet1“ zu „VNet4“. Diese Schritte gelten nur für VNets in demselben Abonnement. Wenn sich Ihre VNets in unterschiedlichen Abonnements befinden, müssen Sie PowerShell zum Herstellen der Verbindung verwenden. Informationen hierzu finden Sie im Artikel über [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). Wenn Ihre VNETs in verschiedenen Ressourcengruppen in demselben Abonnement enthalten sind, können Sie sie jedoch mithilfe des Portals verbinden.

1. Navigieren Sie unter **Alle Ressourcen** zum virtuellen Netzwerkgateway für Ihr VNet. Beispiel: **TestVNet1GW**. Klicken Sie auf **TestVNet1GW**, um die Seite für das virtuelle Netzwerkgateway zu öffnen.

  ![Seite „Verbindungen“](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/1to4connect2.png "Seite „Verbindungen“")
2. Klicken Sie auf **+ Hinzufügen**, um die Seite **Verbindung hinzufügen** zu öffnen.

  ![Hinzufügen der Verbindung](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add.png "Hinzufügen der Verbindung")
3. Geben Sie auf der Seite **Verbindung hinzufügen** im Feld „Name“ einen Namen für Ihre Verbindung ein. Beispiel: **TestVNet1toTestVNet4**.
4. Wählen Sie in der Dropdownliste **Verbindungstyp** die Option **VNet-zu-VNet** aus.
5. Der Wert des Felds **Erstes virtuelles Netzwerkgateway** wird automatisch ausgefüllt, da Sie diese Verbindung über das angegebene virtuelle Netzwerkgateway erstellen.
6. Das Feld **Zweites virtuelles Netzwerkgateway** ist das virtuelle Netzwerkgateway des VNets, mit dem Sie eine Verbindung herstellen möchten. Klicken Sie auf **Ein weiteres virtuelles Netzwerkgateway auswählen**, um die Seite **Ein Gateway für virtuelle Netzwerke auswählen** zu öffnen.
7. Sehen Sie sich die auf der Seite aufgeführten virtuellen Netzwerkgateways an. Beachten Sie, dass nur virtuelle Netzwerkgateways aus Ihrem Abonnement aufgeführt werden. Falls Sie eine Verbindung mit einem virtuellen Netzwerkgateway herstellen möchten, das nicht in Ihrem Abonnement enthalten ist, lesen Sie den [PowerShell-Artikel](vpn-gateway-vnet-vnet-rm-ps.md).
8. Klicken Sie auf das virtuelle Netzwerkgateway, mit dem Sie eine Verbindung herstellen möchten.
9. Geben Sie im Feld **Gemeinsam verwendeter Schlüssel** einen gemeinsam verwendeten Schlüssel für Ihre Verbindung ein. Diesen Schlüssel können Sie selbst generieren oder erstellen. Bei einer Site-to-Site-Verbindung wird für Ihr lokales Gerät und für Ihre Verbindung mit dem virtuellen Netzwerkgateway exakt der gleiche Schlüssel verwendet. Das Konzept ist hier ähnlich, nur dass diesmal keine Verbindung mit einem VPN-Gerät hergestellt wird, sondern mit einem anderen virtuellen Netzwerkgateway.
10. Klicken Sie unten auf der Seite auf **OK**, um die Änderungen zu speichern.

## <a name="TestVNet4Connection"></a>8. Konfigurieren der TestVNet4-Gatewayverbindung
Als Nächstes erstellen Sie eine Verbindung von „TestVNet4“ zu „TestVNet1“. Suchen Sie im Portal nach dem virtuellen Netzwerkgateway, das TestVNet4 zugeordnet ist. Führen Sie die Schritte im vorherigen Abschnitt aus, und ersetzen Sie dabei die Werte, um eine Verbindung von „TestVNet4“ zu „TestVNet1“ zu erstellen. Verwenden Sie den gleichen gemeinsam verwendeten Schlüssel.

## <a name="VerifyConnection"></a>9. Überprüfen der Verbindungen

Suchen Sie im Portal nach dem virtuellen Netzwerkgateway. Klicken Sie auf der Seite des virtuellen Netzwerkgateways auf **Verbindungen**, um die Verbindungsseite für das virtuelle Netzwerkgateway anzuzeigen. Sobald die Verbindung hergestellt wurde, ändern sich die Statuswerte in **Erfolgreich** und **Verbunden**. Sie können auf eine Verbindung doppelklicken, um die Seite **Zusammenfassung** zu öffnen und weitere Informationen anzuzeigen.

![Erfolgreich](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Erfolgreich")

Sobald Daten übertragen werden, werden Werte für „Eingehende Daten“ und „Ausgehende Daten“ angezeigt.

![Zusammenfassung](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Zusammenfassung")

## <a name="to-add-additional-connections"></a>So fügen Sie zusätzliche Verbindungen hinzu

Wenn Sie zusätzliche Verbindungen hinzufügen möchten, navigieren Sie zum virtuellen Netzwerkgateway, über das Sie die Verbindung erstellen möchten, und klicken Sie auf **Verbindungen**. Sie können eine weitere VNET-to-VNET-Verbindung oder eine IPsec-S2S-Verbindung mit einem lokalen Standort erstellen. Denken Sie daran, den **Verbindungstyp** entsprechend dem gewünschten Verbindungstyp einzustellen. Stellen Sie vor dem Erstellen weiterer Verbindungen sicher, dass sich der Adressraum für Ihr virtuelles Netzwerk nicht mit den Adressräumen überlappt, mit denen Sie eine Verbindung herstellen möchten. Anweisungen zum Erstellen einer S2S-Verbindung finden Sie unter [Erstellen einer Site-to-Site-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="faq"></a>Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen
Zeigen Sie die Details zu den häufig gestellten Fragen an, um zusätzliche Informationen zu VNet-zu-VNet-Verbindungen zu erhalten.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Beschränken des Netzwerkdatenverkehrs auf Ressourcen in einem virtuellen Netzwerk finden Sie unter [Netzwerksicherheit](../virtual-network/security-overview.md).

Informationen dazu, wie Azure Datenverkehr zwischen Azure-Ressourcen, lokalen Ressourcen und Internetressourcen weiterleitet, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](../virtual-network/virtual-networks-udr-overview.md).

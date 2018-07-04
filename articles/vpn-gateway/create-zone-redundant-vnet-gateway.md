---
title: Erstellen eines zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen – Vorschau | Microsoft-Dokumentation
description: Bereitstellen von VPN Gateway-Instanzen und ExpressRoute-Gateways in Verfügbarkeitszonen – Vorschau
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/25/2018
ms.author: cherylmc
ms.openlocfilehash: a08c0f772965ddb2b40ac1ced1f26ade4cba3197
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37017031"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Erstellen eines zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen – Vorschau

Sie können VPN- und ExpressRoute-Gateways in [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) bereitstellen. So erzielen Sie Stabilität, Skalierbarkeit und eine höhere Verfügbarkeit für die Gateways des virtuellen Netzwerks. Durch die Bereitstellung von Gateways in Azure-Verfügbarkeitszonen werden die Gateways innerhalb einer Region physisch und logisch getrennt. Gleichzeitig wird die Konnektivität Ihres lokalen Netzwerks mit Azure vor Ausfällen auf Zonenebene geschützt.

Zonenbasierte und zonenredundante Gateways bieten eine wesentlich bessere Leistung als reguläre Gateways für virtuelle Netzwerke. Darüber hinaus lassen sich zonenredundante oder zonenbasierte Gateways schneller erstellen als andere Gateways. Die Erstellung dauert bei einem ExpressRoute-Gateway etwa 15 Minuten, bei einem VPN-Gateway etwa 19 Minuten. Für andere Gateways dagegen müssen Sie eine Erstellungsdauer von 45 Minuten veranschlagen.

### <a name="zrgw"></a>Zonenredundante Gateways

Um Ihre Gateways für virtuelle Netzwerke automatisch über Verfügbarkeitszonen hinweg bereitzustellen, können Sie zonenredundante Gateways für virtuelle Netzwerke verwenden. Mit zonenredundanten Gateways können Sie bei allgemeiner Verfügbarkeit von einer Betriebszeit-SLA von 99.99 % profitieren, um auf Ihre unternehmenskritischen, skalierbaren Dienste in Azure zuzugreifen.

<br>
<br>

![Grafik: zonenredundante Gateways](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonenbasierte Gateways

Um Gateways in einer bestimmten Zone bereitzustellen, verwenden Sie zonenbasierte Gateways. Wenn Sie ein zonenbasiertes Gateway bereitstellen, werden beide Instanzen des Gateways in derselben Verfügbarkeitszone bereitgestellt.

<br>
<br>

![Grafik: zonenbasierte Gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Gateway-SKUs

Zonenredundante und zonenbasierte Gateways müssen die neuen Gateway-SKUs verwenden. Sobald Sie sich [in der Vorschau selbst registriert](#enroll) haben, können Sie die neuen SKUs für Gateways für virtuelle Netzwerke in allen Azure AZ-Regionen sehen. Diese SKUs ähneln den entsprechenden SKUs für ExpressRoute und VPN Gateway, sind aber speziell auf zonenredundante und zonenbasierte Gateways ausgelegt.

Folgende neue Gateway-SKUs stehen zur Verfügung:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>SKUs für öffentliche IP-Ressourcen

Zonenredundante und zonenbasierte Gateways verwenden die Azure-SKU für öffentliche IP-Ressourcen vom Typ *Standard*. Die Konfiguration der öffentlichen Azure-IP-Ressource bestimmt, ob Sie ein zonenredundantes oder zonenbasiertes Gateway bereitstellen. Wenn Sie eine öffentliche IP-Ressource mit einer *Basic*-SKU erstellen, verfügt das Gateway nicht über Zonenredundanz, und die Gatewayressourcen sind auf eine Region begrenzt.

### <a name="pipzrg"></a>Zonenredundante Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Standard**-SKU für öffentliche IP-Adressen erstellen, ohne eine Zone anzugeben, ändert sich das Verhalten, je nachdem, ob es sich um ein VPN-Gateway oder ein ExpressRoute-Gateway handelt. 

* Im Fall eines VPN-Gateways werden die beiden Gatewayinstanzen in zwei beliebigen der drei Zonen bereitgestellt, um für Zonenredundanz zu sorgen. 
* Da es bei einem ExpressRoute-Gateway mehr als zwei Instanzen geben kann, kann sich das Gateway über alle drei Zonen erstrecken.

### <a name="pipzg"></a>Zonenbasierte Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Standard**-SKU für öffentliche IP-Adressen erstellen und die Zone (1, 2 oder 3) angeben, werden alle Gatewayinstanzen in derselben Zone bereitgestellt.

### <a name="piprg"></a>Regionsbezogene Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Basic**-SKU für öffentliche IP-Adressen erstellen, wird das Gateway als regionsbezogenes Gateway bereitgestellt und bietet keine in das Gateway integrierte Zonenredundanz.

## <a name="before-you-begin"></a>Voraussetzungen

Sie können entweder eine lokal auf Ihrem Computer installierte PowerShell-Instanz oder die Azure Cloud Shell verwenden. Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Feature die neueste Version des PowerShell-Moduls verwenden.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Lokale Verwendung von PowerShell

Wenn Sie nicht die Cloud Shell nutzen, sondern PowerShell lokal auf Ihrem Computer verwenden möchten, müssen Sie das PowerShell-Modul 6.1.1 oder höher installieren. Um die installierte PowerShell-Version zu überprüfen, verwenden Sie den folgenden Befehl:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Registrieren in der Vorschau

Bevor Sie ein zonenredundantes oder zonenbasiertes Gateway konfigurieren können, müssen Sie Ihr Abonnement selbst in der Vorschau registrieren. Sobald Ihr Abonnement registriert wurde, werden Ihnen die neuen Gateway-SKUs in allen Azure AZ-Regionen angezeigt. 

Stellen Sie sicher, dass Sie bei Ihrem Azure-Konto angemeldet sind und das Abonnement verwenden, das Sie für diese Vorschau in die Whitelist eintragen möchten. Verwenden Sie das folgende Beispiel für die Registrierung:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Verwenden Sie den folgenden Befehl, um sicherzustellen, dass das AllowVMSSVirtualNetworkGateway-Feature in Ihrem Abonnement registriert ist:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

Das Ergebnis sieht etwa wie in diesem Beispiel aus:

![Bereitgestellt](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Deklarieren von Variablen

Die in den Beispielschritten verwendeten Werte werden hier unten aufgeführt. Darüber hinaus verwenden einige Beispiele deklarierte Variablen in den Schritten. Wenn Sie diese Schritte in Ihrer eigenen Umgebung ausführen, ersetzen Sie diese Werte durch Ihre eigenen. Wenn Sie einen Standort angeben, stellen Sie sicher, dass die angegebene Region unterstützt wird. Weitere Informationen finden Sie in den [häufig gestellten Fragen](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>3. Erstellen des virtuellen Netzwerks

Erstellen Sie eine Ressourcengruppe.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Erstellen Sie ein virtuelles Netzwerk.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>4. Hinzufügen des Gatewaysubnetzes

Das Gatewaysubnetz enthält die reservierten IP-Adressen, die von den Diensten des virtuellen Netzwerkgateways verwendet werden. Verwenden Sie die folgenden Beispiele, um ein Gatewaysubnetz hinzufügen und festzulegen:

Fügen Sie das Gatewaysubnetz hinzu.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Legen Sie die Konfiguration des Gatewaysubnetzes für das virtuelle Netzwerk fest.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Anfordern einer öffentlichen IP-Adresse
 
In diesem Schritt folgen Sie den Anweisungen, die auf das von Ihnen erstellte Gateway zutreffen. Die Auswahl der Zonen, in denen die Gateways bereitgestellt werden, richtet sich nach den Zonen, die für die öffentliche IP-Adresse angegeben wurden.

### <a name="ipzoneredundant"></a>Für zonenredundante Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Standard** an, und geben Sie keine Zone an. In diesem Fall handelt es sich bei der erstellten öffentlichen IP-Adresse vom Typ „Standard“ um eine zonenredundante öffentliche IP-Adresse.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Standard
```

### <a name="ipzonalgw"></a>Für zonenbasierte Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Standard** an. Geben Sie die Zone an (1, 2 oder 3). Alle Gatewayinstanzen werden in dieser Zone bereitgestellt.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Für regionsbezogene Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Basic** an. In diesem Fall wird das Gateway als regionsbezogenes Gateway bereitgestellt und bietet keine in das Gateway integrierte Zonenredundanz. Die Gatewayinstanzen werden jeweils in beliebigen Zonen erstellt.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>6. Erstellen der IP-Konfiguration

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. Erstellen des Gateways

Erstellen Sie das virtuelle Netzwerkgateway.

>[!NOTE]
>Zu diesem Zeitpunkt können Sie die Gateway-SKU nicht angeben. Die SKU wird für ExpressRoute automatisch auf ErGw1AZ festgelegt, für VPN Gateway auf VpnGw1AZ.
>

### <a name="for-expressroute"></a>Für ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Für VPN Gateway

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Senden von Feedback

Wir schätzen Ihr Feedback. Senden Sie eine E-Mail an aznetworkgateways@microsoft.com, um Probleme zu melden oder (positives oder negatives) Feedback zu den zonenredundanten und zonenbasierten VPN- und ExpressRoute-Gateways zu geben. Fügen Sie den Namen Ihres Unternehmens in eckigen Klammern in die Betreffzeile ein. Wenn Sie ein Problem melden, geben Sie auch Ihre Abonnement-ID an.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-do-i-sign-up-for-the-preview"></a>Wie melde ich mich für die Vorschau an?

Sie können sich mithilfe der PowerShell-Befehle in diesem Artikel [selbst registrieren](#enroll).

### <a name="what-will-change-when-i-enroll"></a>Was ändert sich durch die Registrierung?

Aus Ihrer Sicht: Sie können während der Vorschau Ihre Gateways mit Zonenredundanz bereitstellen. Das bedeutet, dass alle Instanzen der Gateways in Azure-Verfügbarkeitszonen bereitgestellt werden und jede Verfügbarkeitszone eine eigene Fehler- und Updatedomäne darstellt. Dadurch werden Ihre Gateways zuverlässiger, verfügbarer und robuster bei Zonenausfällen.

### <a name="what-regions-are-available-for-the-preview"></a>Welche Regionen sind für die Vorschau verfügbar?

Zonenredundante und zonenbasierte Gateways stehen in Produktionsregionen und öffentlichen Azure-Regionen zur Verfügung.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Fallen für die Teilnahme an dieser Vorschau Gebühren an?

Ihre Gateways werden während der Vorschau nicht berechnet. Für die Bereitstellung gilt allerdings auch keine SLA. Wir sind sehr an Ihrem Feedback interessiert.

> [!NOTE]
> Bei ExpressRoute-Gateways wird das Gateway nicht berechnet. Allerdings fallen für die Leitung selbst (nicht das Gateway) Gebühren an.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>In welchen Regionen kann ich das ausprobieren?

Die öffentliche Vorschau ist in den Regionen „USA, Mitte“ und „Frankreich, Mitte“ verfügbar (in diesen Azure-Regionen sind Verfügbarkeitszonen allgemein verfügbar). In Zukunft werden die zonenredundanten Gateways auch in anderen öffentlichen Azure-Regionen verfügbar sein.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kann ich meine vorhandenen Gateways für virtuelle Netzwerke in zonenredundante oder zonenbasierte Gateways ändern?

Die Migration vorhandener Gateways für virtuelle Netzwerke zu zonenredundanten oder zonenbasierten Gateways wird derzeit nicht unterstützt. Sie können jedoch Ihr vorhandenes Gateway löschen und ein neues zonenredundantes oder zonenbasiertes Gateway erstellen.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kann ich VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk bereitstellen?

Die Bereitstellung von VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk wird während der öffentlichen Vorschau unterstützt. Beachten Sie jedoch folgende Anforderungen und Einschränkungen:

* Reservieren Sie einen /27-IP-Adressbereich für das Gatewaysubnetz.
* Zonenredundante/zonenbasierte ExpressRoute-Gateways können nur mit zonenredundanten/zonenbasierten VPN-Gateways nebeneinander existieren.
* Stellen Sie das zonenredundante/zonenbasierte ExpressRoute-Gateway vor dem zonenredundanten/zonenbasierten VPN-Gateway bereit.
* Ein zonenredundantes/zonenbasiertes ExpressRoute-Gateway kann mit maximal 4 Leitungen verbunden werden.

## <a name="next-steps"></a>Nächste Schritte

Wir schätzen Ihr Feedback. Senden Sie eine E-Mail an aznetworkgateways@microsoft.com, um Probleme zu melden oder (positives oder negatives) Feedback zu den zonenredundanten und zonenbasierten VPN- und ExpressRoute-Gateways zu geben. Fügen Sie den Namen Ihres Unternehmens in eckigen Klammern in die Betreffzeile ein. Wenn Sie ein Problem melden, geben Sie auch Ihre Abonnement-ID an.
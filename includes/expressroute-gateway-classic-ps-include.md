Sie müssen ein VNet und ein gatewaysubnetz zunächst erstellen, bevor Sie arbeiten, auf die folgenden Aufgaben. Finden Sie im Artikel [Konfigurieren eines virtuellen Netzwerks mit dem klassischen Portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) für Weitere Informationen.   

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways
Verwenden Sie den folgenden Befehl, um ein Gateway zu erstellen. Achten Sie darauf, dass alle Werte für Ihre eigene ersetzen.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Stellen Sie sicher, dass das Gateway erstellt wurde
Verwenden Sie den folgenden Befehl, um sicherzustellen, dass das Gateway erstellt wurde. Dieser Befehl ruft auch die Gateway-ID, Sie für andere Vorgänge müssen ab.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Ändern Sie die Größe eines Gateways
Es gibt eine Reihe von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Den folgenden Befehl können Sie die Gateway-SKU jederzeit ändern.

> [!IMPORTANT]
> Dieser Befehl funktioniert nicht für UltraPerformance Gateway. Um Ihr Gateway mit einem Gateway UltraPerformance zu ändern, entfernen Sie zuerst das vorhandene ExpressRoute-Gateway, und erstellen Sie ein neues UltraPerformance-Gateway. Um Ihr Gateway über ein Gateway UltraPerformance herabstufen möchten, entfernen Sie zuerst das Gateway UltraPerformance, und klicken Sie dann erstellen Sie ein neues Gateway. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways
Verwenden Sie den folgenden Befehl, um ein Gateway zu entfernen

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>
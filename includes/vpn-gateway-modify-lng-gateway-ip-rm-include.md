### <a name="gwipnoconnection"></a>So ändern Sie das lokale Netzwerkgateway GatewayIpAddress - keine gatewayverbindung

Wenn das VPN-Gerät, dem Sie herstellen möchten ihre öffentliche IP-Adresse geändert wurde, müssen Sie das lokale Netzwerkgateway entsprechend dieser Änderung zu ändern. Verwenden Sie das Beispiel, um ein lokales Netzwerkgateway ändern, das nicht über ein Gateway-Verbindungsinformationen verfügt.

Wenn Sie diesen Wert ändern, können Sie auch die Adresspräfixe gleichzeitig ändern. Achten Sie darauf, dass Sie den vorhandenen Namen des Gateway des lokalen Netzwerks zu verwenden, um die aktuellen Einstellungen überschrieben. Wenn Sie einen anderen Namen verwenden, erstellen Sie ein neues lokales Netzwerk-Gateway anstelle der vorhandenen Dateigruppe überschrieben.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>So ändern Sie das lokale Netzwerkgateway GatewayIpAddress - Gateway-Verbindungsbroker

Wenn das VPN-Gerät, dem Sie herstellen möchten ihre öffentliche IP-Adresse geändert wurde, müssen Sie das lokale Netzwerkgateway entsprechend dieser Änderung zu ändern. Wenn bereits eine gatewayverbindung vorhanden ist, müssen Sie zuerst die Verbindung zu entfernen. Nachdem die Verbindung getrennt wird, können Sie ändern die Gateway IP-Adresse und erstellen Sie eine neue Verbindung erneut. Sie können auch die Adresspräfixe gleichzeitig ändern. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Wenn Sie die IP-Adresse des Gateways zu ändern, müssen Sie das VPN-Gateway zu löschen. Sie müssen nur die Verbindung entfernen möchten.
 

1. Entfernen Sie die Verbindung an. Sie finden den Namen der Verbindung mit dem Cmdlet "Get-AzureRmVirtualNetworkGatewayConnection".

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Ändern Sie den Wert "GatewayIpAddress". Sie können auch die Adresspräfixe gleichzeitig ändern. Achten Sie darauf, dass Sie den vorhandenen Namen des Gateway des lokalen Netzwerks zu verwenden, um die aktuellen Einstellungen überschrieben. Wenn Sie dies nicht tun, erstellen Sie ein neues lokales Netzwerk-Gateway anstelle der vorhandenen Dateigruppe überschrieben.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Die Verbindung zu erstellen. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp aus. Wenn Sie die Verbindung neu erstellen, verwenden Sie für Ihre Konfiguration den Verbindungstyp, der angegeben wird. Zusätzliche Verbindungstypen finden Sie unter der [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) Seite.  Zum Abrufen der VirtualNetworkGateway-namens können Sie das Cmdlet "Get-AzureRmVirtualNetworkGateway" ausführen.
   
    Legen Sie die Variablen an.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Die Verbindung zu erstellen.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```
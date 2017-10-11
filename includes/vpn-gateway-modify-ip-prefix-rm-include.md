### <a name="noconnection"></a>So ändern Sie die lokalen Netzwerk-Gateway IP-Adresspräfixen - keine gatewayverbindung

So fügen Sie zusätzliche Adresspräfixe hinzu:

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

So entfernen Sie die Adresspräfixe:<br>
Lassen Sie die Präfixe, die Sie nicht mehr benötigen. In diesem Beispiel wird nicht mehr benötigen als Präfix 20.0.0.0/24 (aus dem vorherigen Beispiel), damit wir das lokale Netzwerkgateway aktualisieren, ohne dieses Präfix.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>So ändern Sie die lokalen Netzwerk-Gateway IP-Adresspräfixen - Gateway-Verbindungsbroker

Wenn Sie über eine gatewayverbindung verfügen und zum Hinzufügen oder entfernen die IP-Adresspräfixen, die in das lokale Netzwerkgateway enthalten soll, müssen Sie die folgenden Schritte aus, in der Reihenfolge ausführen. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Wenn Sie IP-Adresspräfixen zu ändern, müssen Sie das VPN-Gateway zu löschen. Sie müssen nur die Verbindung entfernen möchten.


1. Entfernen Sie die Verbindung an.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Ändern Sie die Adresspräfixe für Ihr lokales Netzwerk-Gateway.
   
  Legen Sie die Variable für die Gatewayverbindung.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Ändern Sie die Präfixe an.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Die Verbindung zu erstellen. In diesem Beispiel konfigurieren wir einen IPsec-Verbindungstyp aus. Wenn Sie die Verbindung neu erstellen, verwenden Sie für Ihre Konfiguration den Verbindungstyp, der angegeben wird. Zusätzliche Verbindungstypen finden Sie unter der [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) Seite.
   
  Legen Sie die Variable für die VirtualNetworkGateway.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Die Verbindung zu erstellen. Dieses Beispiel verwendet die Variable $local, die Sie in Schritt 2 festlegen.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```
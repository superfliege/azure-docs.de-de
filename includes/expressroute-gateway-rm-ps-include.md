Die Schritte für diese Aufgabe verwenden Sie eine VNet basierend auf den Werten in der folgenden Konfiguration Verweisliste. Namen und zusätzliche Einstellungen werden auch in dieser Liste beschrieben. Wir verwenden nicht direkt in den Schritten, diese Liste, obwohl wir Variablen, die anhand der Werte in dieser Liste hinzufügen. Sie können die Liste für die Verwendung als Referenz, kopieren, die Werte durch eigene ersetzen.

**Konfiguration Verweisliste**

* Name des virtuellen Netzwerks = "TestVNet"
* Virtuelle netzwerkadressräume 192.168.0.0/16 =
* Ressourcengruppe "TestRG" =
* Subnet1 Name = "Front-End" 
* Subnet1 Adressraum = "192.168.1.0/24"
* Gateway-Subnet-Name: "GatewaySubnet" Sie müssen immer den Namen ein gatewaysubnetz *GatewaySubnet*.
* Gateway-Subnetzadresse = "192.168.200.0/26"
* Region "USA, Osten" =
* Gatewayname = "GW"
* Gateway-IP-Name = "GWIP"
* Gateway-IP-Konfiguration, Name = "Gwipconf"
* Typ "ExpressRoute" = dieser Typ ist für eine ExpressRoute-Konfiguration erforderlich.
* Öffentliche IP-gatewayname = "Gwpip"

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways
1. Verbinden Sie mit Ihrem Azure-Abonnement.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Deklarieren Sie Variablen für diese Übung. Achten Sie darauf, dass so bearbeiten Sie das Beispiel entsprechend die Einstellungen, die Sie verwenden möchten.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Speichert das virtuelle Netzwerkobjekt als Variable.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Ein gatewaysubnetz für Ihr virtuelles Netzwerk hinzufügen. Das Gateway-Subnetz muss den Namen "GatewaySubnet". Erstellen Sie ein Gateway-Subnetz, das /27 ist groß (/ 26, / 25, usw..).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Legen Sie die Konfiguration.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Speichern Sie das Gateway-Subnetz, als Variable.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Fordern Sie eine öffentliche IP-Adresse ein. Die IP-Adresse wird angefordert, bevor Sie das Gateway zu erstellen. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Es wird dynamisch zugewiesen. Sie verwenden diese IP-Adresse im nächsten Abschnitt. Die AllocationMethod muss dynamisch sein.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Erstellen Sie die Konfiguration für Ihr Gateway. Die Gatewaykonfiguration definiert das Subnetz und die öffentliche IP-Adresse verwenden. In diesem Schritt geben Sie die Konfiguration an, die beim Erstellen des Gateways verwendet werden. Dieser Schritt ist nicht tatsächlich jedoch stattdessen das Gateway erstellt werden. Verwenden Sie im Beispiel unten, um Ihre Gateway-Konfiguration zu erstellen.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Das Gateway zu erstellen. In diesem Schritt der **- Gatewaytyp** ist besonders wichtig. Sie müssen den Wert verwenden **ExpressRoute**. Nach dem Ausführen dieser Cmdlets, dauert das Gateway 45 Minuten oder länger zu erstellen.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-the-gateway-was-created"></a>Stellen Sie sicher, dass das Gateway erstellt wurde
Verwenden Sie die folgenden Befehle aus, um sicherzustellen, dass das Gateway erstellt wurde:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Ändern Sie die Größe eines Gateways
Es gibt eine Reihe von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Den folgenden Befehl können Sie die Gateway-SKU jederzeit ändern.

> [!IMPORTANT]
> Dieser Befehl funktioniert nicht für UltraPerformance Gateway. Um Ihr Gateway mit einem Gateway UltraPerformance zu ändern, entfernen Sie zuerst das vorhandene ExpressRoute-Gateway, und erstellen Sie ein neues UltraPerformance-Gateway. Um Ihr Gateway über ein Gateway UltraPerformance herabstufen möchten, entfernen Sie zuerst das Gateway UltraPerformance, und klicken Sie dann erstellen Sie ein neues Gateway.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Entfernen eines Gateways
Verwenden Sie den folgenden Befehl aus, um ein Gateway zu entfernen:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
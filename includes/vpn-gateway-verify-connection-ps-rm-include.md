Sie können überprüfen, ob die Verbindung erfolgreich war, mit dem Cmdlet "Get-AzureRmVirtualNetworkGatewayConnection", mit oder ohne "– Debug". 

1. Verwenden Sie das folgende Cmdlet-Beispiel, konfigurieren die Werte, die Ihre eigenen übereinstimmen. Wenn Sie aufgefordert werden, wählen Sie "A", um "All" ausführen. Im Beispiel "-Name" bezieht sich auf den Namen der Verbindung, die Sie testen möchten.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. Nachdem das Cmdlet beendet wurde, zeigt die Werte auf. Im folgenden Beispiel zeigt den Status der Verbindung, als "Verbunden", und sehen Sie Ingress- und Egress-Bytes.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```
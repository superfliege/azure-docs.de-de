Sie können überprüfen, ob die Verbindung mit dem Cmdlet 'Get-AzureVNetConnection' war erfolgreich.

1. Verwenden Sie das folgende Cmdlet-Beispiel, konfigurieren die Werte, die Ihre eigenen übereinstimmen. Der Name des virtuellen Netzwerks muss in Anführungszeichen eingeschlossen werden, wenn dieser Leerzeichen enthält.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Nachdem das Cmdlet beendet wurde, zeigt die Werte auf. Im folgenden Beispiel wird der Status der Verbindung wird als "Verbunden", und sehen Sie Ingress- und Egress-Bytes.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal
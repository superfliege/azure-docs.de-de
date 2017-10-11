### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Schritt 1: Navigieren Sie zum Gateway virtuellen Netzwerks

1. In der [Azure-Portal](https://portal.azure.com), navigieren Sie zu **alle Ressourcen**. 
2. Um dem Blatt "virtuelles Netzwerk-Gateway" zu öffnen, navigieren Sie zu dem Gateway des virtuellen Netzwerks, das Sie löschen möchten und klicken darauf.

### <a name="step-2-delete-connections"></a>Schritt 2: Löschen von Verbindungen

1. Klicken Sie auf das Blatt für Gateway des virtuellen Netzwerks auf **Verbindungen** an alle Verbindungen mit dem Gateway.
2. Klicken Sie auf die **"..."** Wählen Sie dann auf die Zeile mit dem Namen der Verbindung, **löschen** aus der Dropdownliste aus.
3. Klicken Sie auf **Ja** zu bestätigen, dass Sie die Verbindung löschen möchten. Wenn Sie mehrere Verbindungen haben, löschen Sie jede Verbindung.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Schritt 3: Löschen Sie das Gateway des virtuellen Netzwerks

Denken Sie daran, dass wenn Sie eine P2S-Konfiguration mit dem VNet neben der S2S-Konfiguration verfügen, das Gateway des virtuellen Netzwerks löschen automatisch alle P2S-Clients ohne Warnung getrennt wird.

1. Klicken Sie auf dem virtuellen Netzwerk-Gateway-Blatt auf **Übersicht**.
2. Auf der **Übersicht** Blatt, klicken Sie auf **löschen** beim Löschen des Gateways.

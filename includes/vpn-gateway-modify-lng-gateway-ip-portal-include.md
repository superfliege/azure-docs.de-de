### <a name="gwipnoconnection"></a>So ändern Sie die lokalen Netzwerk-Gateway-IP-Adresse - keine gatewayverbindung

Verwenden Sie das Beispiel, um ein lokales Netzwerkgateway ändern, das nicht über ein Gateway-Verbindungsinformationen verfügt. Wenn Sie diesen Wert ändern, können Sie auch die Adresspräfixe gleichzeitig ändern.

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Konfiguration**.
2. In der **IP-Adresse** ändern Sie die IP-Adresse.
3. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

### <a name="gwipwithconnection"></a>So ändern Sie den Adressraum des lokalen Netzwerks Gateway Gateway-IP-- gatewayverbindung vorhandenen

Um ein lokales Netzwerkgateway geändert werden, das eine Verbindung aufweist, müssen Sie zuerst die Verbindung zu entfernen. Nachdem die Verbindung getrennt wird, können Sie ändern die Gateway IP-Adresse und erstellen Sie eine neue Verbindung erneut. Sie können auch die Adresspräfixe gleichzeitig ändern. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Wenn Sie die IP-Adresse des Gateways zu ändern, müssen Sie das VPN-Gateway zu löschen. Sie müssen nur die Verbindung entfernen möchten.
 
#### <a name="1-remove-the-connection"></a>1. Entfernen Sie die Verbindung an.

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Verbindungen**.
2. Klicken Sie auf der **...**  in der Zeile für die Verbindung, klicken Sie dann auf **löschen**.
3. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

#### <a name="2-modify-the-ip-address"></a>2. Ändern Sie die IP-Adresse ein.

Sie können auch die Adresspräfixe gleichzeitig ändern.

1. In der **IP-Adresse** ändern Sie die IP-Adresse.
2. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

#### <a name="3-recreate-the-connection"></a>3. Erstellen Sie die Verbindung erneut.

1. Navigieren Sie zum Gateway virtuellen Netzwerks für das VNet. (Nicht das lokale Netzwerkgateway.)
2. Auf dem virtuellen Netzwerk-Gateway in der **Einstellungen** auf **Verbindungen**.
3. Klicken Sie auf die **+ Add** So öffnen die **Verbindung hinzufügen** Blatt.
4. Erstellen Sie die Verbindung erneut.
5. Klicken Sie auf **OK** zum Erstellen der Verbindung.
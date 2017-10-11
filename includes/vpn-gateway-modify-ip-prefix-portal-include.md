### <a name="noconnection"></a>So ändern Sie die lokalen Netzwerk-Gateway IP-Adresspräfixen - keine gatewayverbindung

#### <a name="to-add-additional-address-prefixes"></a>So fügen Sie zusätzliche Adresspräfixe hinzu:

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Konfiguration**.
2. Hinzufügen der IP-Adressbereich in die *zusätzlichen Adressbereich hinzufügen* Feld.
3. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

#### <a name="to-remove-address-prefixes"></a>So entfernen Sie die Adresspräfixe:

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Konfiguration**.
2. Klicken Sie auf die **"..."** in der Zeile, die das Präfix enthält, den Sie entfernen möchten.
3. Klicken Sie auf **entfernen**.
4. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

### <a name="withconnection"></a>So ändern Sie die lokalen Netzwerk-Gateway IP-Adresspräfixen - Gateway-Verbindungsbroker

Wenn Sie über eine gatewayverbindung verfügen und zum Hinzufügen oder entfernen die IP-Adresspräfixen, die in das lokale Netzwerkgateway enthalten soll, müssen Sie die folgenden Schritte aus, in der Reihenfolge ausführen. Dies führt zu Ausfallzeiten für Ihre VPN-Verbindung. Wenn Sie IP-Adresspräfixen zu ändern, müssen Sie das VPN-Gateway zu löschen. Sie müssen nur die Verbindung entfernen möchten.

#### <a name="1-remove-the-connection"></a>1. Entfernen Sie die Verbindung an.

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Verbindungen**.
2. Klicken Sie auf der **...**  in der Zeile für jede Verbindung, klicken Sie dann auf **löschen**.
3. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

#### <a name="2-modify-the-address-prefixes"></a>2. Ändern Sie die Adresspräfixe.

So fügen Sie zusätzliche Adresspräfixe hinzu:

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Konfiguration**.
2. Fügen Sie der IP-Adressbereich hinzu.
3. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

So entfernen Sie die Adresspräfixe:

1. Für das lokale Netzwerkgateway-Ressource in der **Einstellungen** auf **Konfiguration**.
2. Klicken Sie auf der **...**  auf die Zeile mit dem Präfix, das Sie entfernen möchten.
3. Klicken Sie auf **entfernen**.
4. Klicken Sie auf **speichern** zum Speichern der Einstellungen.

#### <a name="3-recreate-the-connection"></a>3. Erstellen Sie die Verbindung erneut.

1. Navigieren Sie zum Gateway virtuellen Netzwerks für das VNet. (Nicht das lokale Netzwerkgateway.)
2. Auf dem virtuellen Netzwerk-Gateway in der **Einstellungen** auf **Verbindungen**.
3. Klicken Sie auf die **+ Add** So öffnen die **Verbindung hinzufügen** Blatt.
4. Erstellen Sie die Verbindung erneut.
5. Klicken Sie auf **OK** zum Erstellen der Verbindung.
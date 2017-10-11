#### <a name="to-create-public-endpoints-on-the-virtual-device"></a>So erstellen öffentliche Endpunkte auf dem virtuellen Gerät

1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie auf **VMs**, und wählen Sie dann den virtuellen Computer, die als Ihr virtuelles Gerät verwendet wird.
3. Klicken Sie auf **Endpunkte**. Die **Endpunkte** Seite listet alle Endpunkte für den virtuellen Computer.
4. Klicken Sie auf **Hinzufügen**. Die **Add Endpoint** Dialogfeld wird angezeigt. Klicken Sie auf den Pfeil, um den Vorgang fortzusetzen.
5. Für die **Namen**, geben Sie den folgenden Namen für den Endpunkt: **WinRMHttps**.
6. Für die **Protokoll**, geben Sie **TCP**.
7. Für die **öffentlicher Port**, geben Sie die Portnummern, die Sie für die Verbindung verwenden möchten.
8. Für die **privater Port**, Typ **5986**.
9. Klicken Sie auf das Häkchen, um den Endpunkt zu erstellen.

Nachdem der Endpunkt erstellt wird, können Sie die Details zum Ermitteln der Adresse öffentliche virtuelle IP-Adresse (VIP) anzeigen. Notieren Sie sich diese Adresse.


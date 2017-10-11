1. Klicken Sie auf der linken Seite der Portalseite auf  **+**  , und geben Sie "Gateway im virtuellen Netzwerk" in der Suche. In **Ergebnisse**, suchen, und klicken Sie auf **Gateway des virtuellen Netzwerks**.
2. Klicken Sie unten auf dem Blatt "Gateway im virtuellen Netzwerk" klicken Sie auf **erstellen**. Daraufhin wird die **Gateway des virtuellen Netzwerks erstellen** Blatt.

    ![Erstellen Sie virtuelle Netzwerkgateway Blatt Felder](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "neues Gateway")

3. Auf der **Gateway des virtuellen Netzwerks erstellen** Blatt, geben Sie die Werte für Ihr virtuelles Netzwerk-Gateway.

  - **Namen**: Benennen Sie Ihr Gateway. Dies ist nicht identisch mit der Benennung ein gatewaysubnetz. Es ist der Name des Gatewayobjekts, das Sie erstellen.
  - **Gatewaytyp**: Wählen Sie **VPN**. VPN-Gateways verwenden Sie den Typ der virtuellen Netzwerk-Gateway **VPN**. 
  - **Typ "VPN"**: Wählen Sie den VPN-Typ, der für Ihre Konfiguration angegeben ist. Die meisten Konfigurationen erfordern einen weiterleitungsbasierte VPN-Typ.
  - **SKU**: Wählen Sie aus der Dropdownliste das Gateway-SKU. SKUs aufgeführt, die in der Dropdownliste richten sich nach dem Typ "VPN", die Sie auswählen. Weitere Informationen zum Gateway-SKUs finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Speicherort**: Möglicherweise müssen Sie einen Bildlauf zum Speicherort finden Sie unter. Anpassen der **Speicherort** Feld, um auf den Speicherort verweist, in dem das virtuelle Netzwerk befindet. Wenn der Speicherort nicht für die Region verweist, in dem das virtuelle Netzwerk befindet, wird das virtuelle Netzwerk nicht in der Dropdownliste der nächste Schritt "Wählen Sie ein virtuelles Netzwerk" angezeigt.
  - **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk zu dem Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **virtuelles Netzwerk** zum Öffnen des Blatts "Wählen Sie ein virtuelles Netzwerk". Wählen Sie das VNet aus. Wenn Ihr VNet nicht angezeigt wird, stellen Sie sicher, dass das Feld "Speicherort" in der Region zeigt in dem sich das virtuelle Netzwerk befindet.
  - **Öffentliche IP-Adresse**: das Blatt "Öffentliche IP-Adresse erstellen" erstellt ein öffentliche IP-Adresse-Objekt. Die öffentliche IP-Adresse wird dynamisch zugewiesen, wenn das VPN-Gateway erstellt wird. VPN-Gateway unterstützt derzeit nur *dynamische* öffentliche IP-Adresszuordnung. Dies bedeutet jedoch nicht, dass die IP-Adresse ändert, nachdem sie Ihr VPN-Gateway zugewiesen wurde. Nur die öffentliche IP-Adressänderungen ist, wenn das Gateway gelöscht und neu erstellt wird. Es wird über das Ändern der Größe, zurücksetzen oder bei anderen internen Wartung/Upgrades Ihres VPN-Gateways nicht geändert.

    - Klicken Sie zunächst **öffentliche IP-Adresse** Blatts "Öffentliche IP-Adresse auswählen" zu öffnen, klicken Sie dann auf **+ erstellen Sie eine neue** zum Öffnen des Blatts "Öffentliche IP-Adresse erstellen".
    - Geben Sie als Nächstes eine **Namen** für Ihre öffentliche IP-Adresse, klicken Sie dann auf **OK** am unteren Rand dieses Blatt, um die Änderungen zu speichern.

      ![Erstellen Sie die öffentliche IP-Adresse](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "PIP erstellen")

4. Überprüfen Sie die Einstellungen aus. Sie können auswählen, **an Dashboard anheften** unten auf dem Blatt ", wenn Sie Ihr Gateway auf dem Dashboard angezeigt werden soll. 
5. Klicken Sie auf **erstellen** zu beginnen, erstellen das VPN-Gateway. Die Einstellungen werden überprüft, und sehen Sie das "Bereitstellen von virtuellen Netzwerkgateway" Kachel im Dashboard. Erstellen eines Gateways kann bis zu 45 Minuten dauern. Sie müssen möglicherweise Ihre Portalseite, um den Status "abgeschlossen" finden Sie unter aktualisieren.

Nachdem das Gateway erstellt wurde, zeigen Sie die IP-Adresse, die ihr zugeordnet wurde, durch einen Blick auf das virtuelle Netzwerk im Verwaltungsportal, an. Das Gateway wird als ein verbundenes Gerät angezeigt. Sie können das verbundene Gerät (Gateway des virtuellen Netzwerks) zum Anzeigen weiterer Informationen klicken.
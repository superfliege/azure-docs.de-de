1. Navigieren Sie zu, und öffnen Sie das Blatt für Ihr virtuelles Netzwerk-Gateway. Es gibt mehrere Möglichkeiten zum Navigieren. In unserem Beispiel wir navigiert an das Gateway "VNet1GW" Navigieren Sie zu **TestVNet1-Übersicht > -> verbunden VNet1GW-Geräte >**.
2. Klicken Sie auf das Blatt für VNet1GW auf **Verbindungen**. Klicken Sie oben auf dem Blatt "Verbindungen" auf **+ Add** So öffnen die **Verbindung hinzufügen** Blatt.

    ![Standort-zu-Standort-Verbindung erstellen](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. Auf der **Verbindung hinzufügen** Blatt, füllen Sie die Werte zum Herstellen die Verbindung.

  - **Name:** benennen Sie die Verbindung. Wir verwenden **VNet1toSite2** in unserem Beispiel.
  - **Verbindungstyp:** wählen **Site-to-site(IPSec)**.
  - **Gateway des virtuellen Netzwerks:** der Wert wird festgelegt, da Sie eine Verbindung zwischen diesem Gateway herstellen.
  - **Lokales Netzwerkgateway:** klicken Sie auf **wählen Sie ein lokales Netzwerkgateway** , und wählen Sie das Gateway des lokalen Netzwerks, die Sie verwenden möchten. In unserem Beispiel verwenden wir **Standort2**.
  - **Gemeinsam verwendeter Schlüssel:** hier der Wert entsprechen muss, die Sie für Ihre lokalen VPN-Gerät verwenden. Im Beispiel wird verwendet "abc123", aber Sie können (und sollten) verwenden etwas komplexer. Wichtig ist, dass der Wert, den Sie hier den gleichen Wert sein muss, den Sie beim Konfigurieren des VPN-Geräts angegeben werden.
  - Die verbleibenden Werte für **Abonnement**, **Ressourcengruppe**, und **Speicherort** behoben werden.

4. Klicken Sie auf **OK** zum Herstellen die Verbindung. Sehen Sie *Verbindung erstellen* flash auf dem Bildschirm.
5. Sehen Sie die Verbindung nicht in der **Verbindungen** Blatt des Gateways des virtuellen Netzwerks. Der Status geht aus *unbekannt* auf *Herstellen einer Verbindung*, und klicken Sie dann auf *erfolgreich*.
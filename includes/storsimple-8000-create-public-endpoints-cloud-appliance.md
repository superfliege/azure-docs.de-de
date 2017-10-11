#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a>Öffentliche Endpunkte auf den Cloud-Anwendung erstellen

1. Melden Sie sich im Azure-Portal an.
2. Wechseln Sie zu **VMs**, und klicken Sie dann auswählen, und klicken Sie auf den virtuellen Computer, die als Cloud-Anwendung verwendet wird.
    
3. Sie müssen eine netzwerksicherheitsregel Gruppe (NSG) zur Steuerung des Datenverkehrs in und aus dem virtuellen Computer zu erstellen. Führen Sie die folgenden Schritte aus, um eine NSG-Regel zu erstellen.
    1. Wählen Sie **Netzwerksicherheitsgruppe**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Klicken Sie auf die Standard-Netzwerksicherheitsgruppe, die angezeigt werden.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Wählen Sie **eingehende Sicherheitsregeln**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Klicken Sie auf **+ Add** zum Erstellen einer Regel für eingehende Sicherheit.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        In der eingehenden Sicherheit Regel Blatts hinzufügen:

        1. Für die **Namen**, geben Sie den folgenden Namen für den Endpunkt: WinRMHttps.
        
        2. Für die **Priorität**, wählen Sie eine Zahl kleiner als 1000 (Dies ist die Priorität für die Standardregel). Je höher der Wert niedriger die Priorität.

        3. Legen Sie die **Quelle** auf **alle**.

        4. Für die **Service**Option **WinRM**. Die **Protokoll** auf automatisch festgelegt ist **TCP** und **Portbereich** festgelegt ist, um **5986**.

        5. Klicken Sie auf **OK** zum Erstellen der Regel.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Im letzten Schritt wird ein Subnetz oder eine bestimmte Netzwerkschnittstelle Ihrer Netzwerksicherheitsgruppe zugeordnet werden soll. Führen Sie die folgenden Schritte aus, um ein Subnetz der Netzwerksicherheitsgruppe zuzuordnen.
    1. Wechseln Sie zu **Subnetze**.
    2. Klicken Sie auf **+ zuordnen**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Wählen Sie Ihr virtuelle Netzwerk, und wählen Sie dann das entsprechende Subnetz.
    4. Klicken Sie auf **OK** zum Erstellen der Regel.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Nachdem die Regel erstellt wurde, können Sie die Details zum Ermitteln der Adresse öffentliche virtuelle IP-Adresse (VIP) anzeigen. Notieren Sie sich diese Adresse.



<!--author=alkohli last changed: 9/17/15-->

#### <a name="to-add-a-storage-account-in-storsimple-8000-series-update-10"></a>So fügen Sie ein Speicherkonto in StorSimple 8000 Series Update 1.0 hinzu
1. Klicken Sie auf der Zielseite StorSimple Manager-Dienst wählen Sie den Dienst, und doppelklicken Sie darauf. Dadurch gelangen Sie zu der **Schnellstart** Seite. Wählen Sie die **konfigurieren** Seite.
2. Klicken Sie auf **Speicherkonto hinzufügen/bearbeiten**.
3. In der **Speicherkonto hinzufügen/bearbeiten** (Dialogfeld), klicken Sie auf **Add new**.
4. In der **Anbieter** Feld, wählen Sie den entsprechenden Cloud-Dienstanbieter. Die unterstützten Anbieter sind Azure, Amazon S3, Amazon S3 mit RRS, HP und openstack hinweg. Geben Sie die Anmeldeinformationen und den Speicherort der Speicherkonto Ihrer Cloud-Service-Anbieter zugeordnet. Die Felder zum Eingeben von Anmeldeinformationen vorgelegt unterscheiden abhängig von den Cloud-Dienstanbieter, den Sie angegeben haben. 
   
   * Wenn Sie Azure als Ihre Cloud-Dienstanbieter ausgewählt haben, geben Sie die **Namen** und dem primären **Zugriffsschlüssel** für Ihr Microsoft Azure-Speicherkonto. Der Speicherort wird für ein Azure-Konto automatisch aufgefüllt.
     
        ![Azure Storage-Konto hinzufügen](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)
   * Wenn Sie Amazon S3 oder Amazon S3 mit RRS ausgewählt haben, geben Sie einen aussagekräftigen **Storage Account Name**, **Zugriffsschlüssel**, und **Geheimschlüssel**. Amazon S3 und Amazon S3 mit RRS werden die folgenden Speicherorte unterstützt:
     
     * US Standard
     * US West (Frankreich)
     * US West (Nordkalifornien)
     * Europa (Irland)
     * Asien-Pazifik (Singapur)
     * Asien-Pazifik (Sydney)
     * Asien-Pazifik (Tokio)
     * Südamerika (São Paulo)
       
       ![Amazon-Storage-Konto hinzufügen](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
   * Wenn Sie eine HP als Dienstanbieter Ihres Cloud ausgewählt haben, geben Sie einen aussagekräftigen **Speicherkontoname**, **Mandanten-ID**, **Benutzername**, und **Kennwort**. Für HP werden die folgenden Speicherorte unterstützt:
     
     * USA, Osten
     * USA, Westen
       
       ![HP-Storage-Konto hinzufügen](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
   * Wenn Sie ausgewählt haben **openstack hinweg** als Ihrem Cloud-Service-Anbieter bieten eine **Hostname**, **Zugriffsschlüssel**, und **Geheimschlüssel**.
     
     > [!NOTE]
     > Für alle die Cloud-Dienstanbieter, Azure ausgenommen, ist ein Anzeigename zulässig. Sie können verschiedene Anzeigenamen verwenden und mehr als ein Speicherkonto mit den gleichen Satz von Anmeldeinformationen zu erstellen.
     > 
     > 
     
        ![Hinzufügen eines Speicherkontos openstack hinweg](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)
5. Wählen Sie **SSL-Modus aktivieren** um einen sicheren Kanal für die Netzwerkkommunikation zwischen Ihrem Gerät und der Cloud zu erstellen. Deaktivieren der **SSL-Modus aktivieren** Kontrollkästchen nur, wenn Sie in einer privaten Cloud ausgeführt werden.
   
   > [!NOTE]
   > Bei Verwendung von HP als Ihr Anbieter wird immer SSL aktiviert werden.
   > 
   > 
6. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Sie werden benachrichtigt, nachdem das Speicherkonto erfolgreich erstellt wurde.
7. Das neu erstellte Speicherkonto wird angezeigt, auf die **konfigurieren** Seite **Speicherkonten**. Klicken Sie auf **speichern** um das neue Speicherkonto zu speichern. Klicken Sie auf **OK** Wenn zur Bestätigung aufgefordert.


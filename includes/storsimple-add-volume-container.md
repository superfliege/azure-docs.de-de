<!--author=SharS last changed: 1/7/2016-->

#### <a name="to-add-a-volume-container"></a>So fügen Sie einen volumecontainer hinzu
1. Auf der **Geräte** Seite, wählen Sie das Gerät, doppelklicken Sie darauf, und klicken Sie dann auf die **volumecontainer** Registerkarte.
2. Klicken Sie auf **hinzufügen** am unteren Rand der Seite. In der **volumecontainer erstellen** Dialogfeld Feld, gehen Sie folgendermaßen vor:
   
   1. Geben Sie einen eindeutigen **Namen** für Ihren volumecontainer. Dieser Name darf maximal 32 Zeichen enthalten.
   2. Wählen Sie eine **Speicherkonto** diesem volumecontainer zugeordnet werden soll. Sie können auswählen, über ein vorhandenes Speicherkonto innerhalb desselben Abonnements oder wählen Sie **Weitere hinzufügen,** an ein Speicherkonto aus einem anderen Abonnement auswählen. Sie können auch das Speicherkonto auswählen, das zuerst generiert wurde, wenn der Dienst erstellt wurde.
   3. Geben Sie die Bandbreite als **unbegrenzt** Wenn Sie die gesamte verfügbare Bandbreite nutzen möchten oder **benutzerdefinierte** um Bandbreite zu steuern. Geben Sie einen Wert zwischen 1 und 1000 Mbit/s, eine benutzerdefinierte Bandbreite zur Verfügung steht. Um Bandbreite basierend auf einem Zeitplan zuzuordnen, können Sie **bandbreitenvorlage auswählen**.
   4. Es wird empfohlen, dass stets **Cloudspeicherverschlüsselung aktivieren** zum Verschlüsseln der Daten, die in der Cloud werden ausgewählt. Deaktivieren Sie die Verschlüsselung nur dann, wenn Sie andere Mittel zum Verschlüsseln der Daten bereitstellen. Sie können nicht geändert, die Verschlüsselung festlegen, nachdem die Volume-Container erstellt wurde.
   5. Geben Sie einen **Cloudspeicher-Verschlüsselungsschlüssel** , das zwischen 8 und 32 Zeichen enthält. Das Gerät verwendet diesen Schlüssel, um die verschlüsselten Daten zugreifen. In der **Schlüssel für Cloudspeicherverschlüsselung bestätigen** Feld, geben Sie den cloudspeicher-Verschlüsselungsschlüssel erneut aus, um es zu bestätigen. 
   6. Klicken Sie auf den Pfeil, um mit der nächsten Seite fortfahren.
      
      ![Volumecontainer mit, bandbreitenvorlage 1 erstellen](./media/storsimple-add-volume-container/HCS_CreateVCBT1-include.png) 
3. Wenn Sie angegeben haben **bandbreitenvorlage auswählen**, wählen Sie aus der Dropdownliste der vorhandenen bandbreitenvorlagen. Überprüfen Sie die zeitplaneinstellungen, und klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png).
   
    ![Erstellen eines volumecontainers mit bandbreitenvorlage 2](./media/storsimple-add-volume-container/HCS_CreateVCBT2-include.png) 

Der volumecontainer wird gespeichert, und der neu erstellte volumecontainer wird aufgeführt, auf die **volumecontainer** Seite.


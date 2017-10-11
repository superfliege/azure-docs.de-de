1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com) an.  
2. Klicken Sie auf der Befehlsleiste am unteren Rand des Fensters auf **neu**.
3. Klicken Sie unter **berechnen**, klicken Sie auf **VM**, und klicken Sie dann auf **aus Katalog**.
   
    ![Erstellen eines neuen virtuellen Computers][Image1]
4. Klicken Sie unter der **SUSE** gruppieren, wählen Sie ein Image eines virtuellen Computers OpenSUSE, und klicken Sie dann auf den Pfeil, um den Vorgang fortzusetzen.
5. Auf der ersten **Konfiguration des virtuellen Computers** Seite:
   
   * Geben Sie einen **Name des virtuellen Computers**, z. B. "Testlinuxvm". Der Name muss zwischen 3 und 15 Zeichen enthalten, können nur Buchstaben, Zahlen und Bindestriche enthalten und muss mit einem Buchstaben beginnen und mit einem Buchstaben oder einer Ziffer enden.
   * Überprüfen Sie die **Ebene** , und wählen Sie eine **Größe**. Die Ebene bestimmt die Größen, denen Sie auswählen können. Die Größe wirkt sich auf die Kosten für verwenden, sowie Konfigurationsoptionen, z. B. wie viele Datenträger Sie anfügen können. Weitere Informationen finden Sie unter [Größen für virtuelle Maschinen](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Geben Sie einen **neuer Benutzername**, oder übernehmen Sie den Standardnamen **Azureuser**. Dieser Name wird auf die Datei/etc/sudoers.d/omsagent hinzugefügt.
   * Entscheiden, welche **Authentifizierung** verwenden. Allgemeine Richtlinien zur finden Sie unter [sichere Kennwörter](http://msdn.microsoft.com/library/ms161962.aspx).
6. Auf der nächsten **Konfiguration des virtuellen Computers** Seite:
   
   * Verwenden Sie den Standardnamen **Erstellen eines neuen Cloud-Diensts**.
   * In der **DNS-Namen** Geben Sie einen eindeutigen DNS-Namen, die als Teil der Adresse, z. B. "Testlinuxvm" verwendet.
   * In der **Region/Affinitätsgruppe/virtuelles Netzwerk** Feld, wählen Sie eine Region, in dem das virtuelle Image gehostet wird.
   * Klicken Sie unter **Endpunkte**, behalten Sie den SSH-Endpunkt. Sie können andere Benutzer nun oder hinzufügen, ändern oder löschen, nachdem die virtuelle Maschine erstellt wurde.
     
     > [!NOTE]
     > Wenn Sie eine virtuelle Maschine auf ein virtuelles Netzwerk verwenden möchten Sie **müssen** Geben Sie das virtuelle Netzwerk beim Erstellen des virtuellen Computers. Sie können ein virtuelles Netzwerk nach der Erstellung der virtuellen Maschine einen virtuellen Computer hinzufügen. Weitere Informationen finden Sie unter [Überblick über virtuelle Netzwerke](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. Für den letzten **Konfiguration des virtuellen Computers** Seite, behalten Sie die Standardeinstellungen, und klicken Sie dann auf das Häkchen, um den Vorgang abzuschließen.

Im Portal aufgeführt, die neue virtuelle Maschine unter **VMs**. Während der Status, als gemeldet wird **(Bereitstellung)**, wird der virtuelle Computer eingerichtet wird. Wenn der Status gemeldet wird, als **ausführen**, können Sie mit dem nächsten Schritt wechseln.

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
Verwenden Sie SSH oder PuTTY zur Verbindung mit dem virtuellen Computer je nach Betriebssystem auf dem Computer, den Sie eine Verbindung herstellen müssen:

* Verwenden Sie SSH auf einem Computer, auf dem Linux ausgeführt wird. Geben Sie in die Eingabeaufforderung Folgendes ein:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Geben Sie das Kennwort des Benutzers.
* Von einem Computer unter Windows können verwenden Sie PuTTY. Wenn Sie nicht installiert ist, laden Sie es der [PuTTY-Downloadseite][PuTTYDownload].
  
    Speichern Sie **putty.exe** in ein Verzeichnis auf Ihrem Computer. Öffnen Sie eine Eingabeaufforderung, navigieren Sie zu diesem Ordner und führen Sie **putty.exe**.
  
    Geben Sie den Hostnamen, z. B. "testlinuxvm.cloudapp.net", und geben Sie "22" für die **Port**.
  
    ![PuTTY-Bildschirm][Image6]  

## <a name="update-the-virtual-machine-optional"></a>Update der virtuellen Maschine (optional)
1. Nachdem Sie mit dem virtuellen Computer verbunden sind, können Sie optional Systemupdates und -Patches installieren. Um das Update auszuführen, geben Sie Folgendes ein:
   
    `$ sudo zypper update`
2. Wählen Sie **Software**, klicken Sie dann **Onlineupdate** zum Auflisten der verfügbaren Updates. Wählen Sie **Accept** zum Starten der Installation und alle neuen verfügbaren Patches (außer den optional) anwenden.
3. Wählen Sie nach Abschluss der Installation **Fertig stellen**.  Das System ist jetzt auf dem neuesten Stand.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png

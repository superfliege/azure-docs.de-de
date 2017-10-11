1. Auf **verbinden** erstellt und lädt eine Datei des Remotedesktopprotokolls (RDP-Datei). Klicken Sie auf **öffnen** , diese Datei zu verwenden.
2. Sie erhalten eine Warnung, die das RDP von einem unbekannten Verleger stammt. Dies ist normal. Klicken Sie im Fenster des Remotedesktops auf **verbinden** um den Vorgang fortzusetzen.
   
    ![Screenshot des eine Warnung zu einem unbekannten Herausgeber.](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. In der **Windows-Sicherheit** Fenster, geben Sie die Anmeldeinformationen für ein Konto auf dem virtuellen Computer, und klicken Sie dann auf **OK**.
   
     **Lokales Konto** -Dies ist in der Regel den lokalen Benutzernamen und das Kennwort, die Sie beim Erstellen der virtuellen Maschine angegeben. In diesem Fall die Domäne ist der Name des virtuellen Computers und eingegeben wird, als *%vmname*&#92; *Benutzername*.  
   
    **Virtuellen Computer in der Domäne** – Wenn der virtuelle Computer einer Domäne angehört Geben Sie den Benutzernamen im Format *Domäne*&#92; *Benutzername*. Das Konto muss auch in der Gruppe "Administratoren" sein oder über die entsprechende RAS-Berechtigungen mit dem virtuellen Computer.
   
    **Domänencontroller** – Wenn der virtuelle Computer ein Domänencontroller ist. Geben Sie den Benutzernamen und das Kennwort eines Administratorkontos Domäne für diese Domäne.
4. Klicken Sie auf **Ja** zum Überprüfen der Identität des virtuellen Computers und auf Anmeldung fertigzustellen.
   
   ![Screenshot der eine Nachricht zum Überprüfen der Identität des virtuellen Computers.](./media/virtual-machines-log-on-win-server/cert-warning.png)


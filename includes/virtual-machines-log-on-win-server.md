1. Nach Klicken auf **Verbinden** wird eine Remotedesktopprotokolldatei (RDP-Datei) erstellt und heruntergeladen. Klicken Sie auf **Öffnen** , damit diese Datei verwendet wird.
2. Es wird eine Warnung angezeigt mit dem Hinweis, dass die `.rdp`-Datei von einem unbekannten Herausgeber stammt. Dies ist normal. Klicken Sie im Fenster "Remotedesktop" auf **Verbinden** , um den Vorgang fortzusetzen.
   
    ![Screenshot einer Warnung zu einem unbekannten Herausgeber](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie die Anmeldeinformationen eines Kontos auf dem virtuellen Computer ein, und klicken Sie anschließend auf **OK**.
   
     **Lokales Konto**: Hierbei handelt es sich in der Regel um den Benutzernamen und das Kennwort des lokalen Kontos, die Sie beim Erstellen des virtuellen Computers angegeben haben. In diesem Fall ist die Domäne der Name des virtuellen Computers. Das Eingabeformat lautet *VM-Name*&#92;*Benutzername*.  
   
    **In die Domäne eingebundener virtueller Computer**: Falls der virtuelle Computer in eine Domäne eingebunden ist, geben Sie den Benutzernamen im Format *Domäne*&amp;#92;*Benutzername* ein. Das Konto muss außerdem entweder zur Gruppe „Administratoren“ gehören oder über Remotezugriffsrechte für den virtuellen Computer verfügen.
   
    **Domänencontroller** – Wenn der virtuelle Computer ein Domänencontroller ist, geben Sie den Benutzernamen und das Kennwort eines Domänenadministratorkontos für diese Domäne an.
4. Klicken Sie auf **Ja** , um die Identität des virtuellen Computers zu bestätigen und das Anmelden zu beenden.
   
   ![Screenshot mit einer Meldung zur Überprüfung der Identität des virtuellen Computers](./media/virtual-machines-log-on-win-server/cert-warning.png)


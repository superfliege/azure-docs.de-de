### <a name="determine-the-dns-name-of-the-virtual-machine"></a>Bestimmen Sie den DNS-Namen des virtuellen Computers
Um mit dem SQL Server-Datenbankmodul von einem anderen Computer zu verbinden, müssen Sie den Domain Name System (DNS)-Namen des virtuellen Computers kennen. (Dies ist der Name, die im Internet verwendet, um den virtuellen Computer zu identifizieren. Sie können die IP-Adresse verwenden, aber möglicherweise die IP-Adresse ändern, wenn Azure Ressourcen Redundanz- oder Wartungsgründen verschoben. Der DNS-Name ist stabil, da er eine neue IP-Adresse umgeleitet werden kann.)  

1. Wählen Sie in der Azure-Verwaltungsportal (oder aus dem vorherigen Schritt) **virtuelle Computer (klassisch)**.
2. Wählen Sie die SQL-VM.
3. Auf der **VM** Blatt, kopieren die **DNS-Namen** für den virtuellen Computer.
   
    ![DNS-name](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-to-the-database-engine-from-another-computer"></a>Verbinden Sie mit dem Datenbankmodul von einem anderen computer
1. Öffnen Sie auf einem Computer mit dem Internet verbunden ist SQL Server Management Studio.
2. In der **Verbindung mit Server herstellen** oder **Verbindung mit Datenbankmodul herstellen** Dialogfeld die **Servernamen** Geben Sie den DNS-Namen der virtuellen Maschine (in der vorherigen Aufgabe bestimmt) und einen öffentlichen Endpunkt-Portnummer im Format *DNSName, Portnummer* wie z. B. **mysqlvm.cloudapp.net,57500**.
   
    ![Herstellen einer Verbindung mithilfe von SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Wenn Sie nicht mehr wissen, dass die öffentlichen Endpunkt Portnummer Sie zuvor erstellt haben, finden Sie in der **Endpunkte** Clientbereich der **virtuelle Maschine** Blatt.
   
    ![Öffentlicher Port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. In der **Authentifizierung** wählen Sie im **SQL Server-Authentifizierung**.
4. In der **Anmeldung** geben den Namen einer Anmeldung, die Sie in einer früheren Aufgabe erstellt.
5. In der **Kennwort** Feld, geben Sie das Kennwort der Anmeldung, die Sie in einer früheren Aufgabe zu erstellen.
6. Klicken Sie auf **Verbinden**.


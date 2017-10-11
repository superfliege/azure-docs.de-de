1. Erweitern Sie im Failovercluster-Manager, **Rollen**, und markieren Sie die verfügbarkeitsgruppe.  

2. Auf der **Ressourcen** Registerkarte rechten Maustaste auf den Listenernamen, und klicken Sie dann auf **Eigenschaften**.

3. Klicken Sie auf die **Abhängigkeiten** Registerkarte. Wenn mehrere Ressourcen aufgeführt sind, stellen Sie sicher, dass die IP-Adressen, OR, not verfügen AND-Abhängigkeiten.  

4. Klicken Sie auf **OK**.

5. Maustaste auf den Listenernamen, und klicken Sie dann auf **Online schalten**.

6. Nach Abschluss der Listener online ist, auf die **Ressourcen** Registerkarte rechten Maustaste auf die verfügbarkeitsgruppe, und klicken Sie dann auf **Eigenschaften**.
   
    ![Die verfügbarkeitsgruppenressource konfigurieren](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Erstellen eine Abhängigkeit von der listenernamensressource (nicht die IP-Adresse Ressourcen Namen), und klicken Sie dann auf **OK**.
   
    ![Abhängigkeit vom Listenernamen hinzufügen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Starten Sie SQL Server Management Studio, und dann eine Verbindung mit dem primären Replikat herstellen.

9. Wechseln Sie zu **hohe Verfügbarkeit mit AlwaysOn** > **Verfügbarkeitsgruppen** > **\<AvailabilityGroupName\>** > **Verfügbarkeitsgruppenlistener**.  
    Den Namen des Listeners, den Sie im Failovercluster-Manager erstellt haben, sollte angezeigt werden.

10. Maustaste auf den Listenernamen, und klicken Sie dann auf **Eigenschaften**.

11. In der **Port** Feld die Portnummer für den verfügbarkeitsgruppenlistener angeben, mit der $EndpointPort, die Sie zuvor verwendet (in diesem Lernprogramm wurde 1433 die Standardeinstellung), und klicken Sie dann auf **OK**.


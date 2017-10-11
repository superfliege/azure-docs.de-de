In diesem Schritt erstellen Sie manuell den verfügbarkeitsgruppenlistener im Failovercluster-Manager und SQL Server Management Studio.

1. Öffnen Sie Failovercluster-Manager über den Knoten, der das primäre Replikat hostet.

2. Wählen Sie die **Netzwerke** Knoten, und notieren Sie den Clusternetzwerknamen. Dieser Name wird in der Variablen $ClusterNetworkName im PowerShell-Skript verwendet.

3. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

4. In der **Rollen** Bereich mit der rechten Maustaste in des Namens der verfügbarkeitsgruppe, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.
   
    ![Clientzugriffspunkt für verfügbarkeitsgruppe hinzufügen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. In der **Namen** Feld, einen Namen für den neuen Listener zu erstellen, klicken Sie auf **Weiter** zweimal, und klicken Sie dann auf **Fertig stellen**.  
    Schalten Sie den Listener oder die Ressource online zu diesem Zeitpunkt.

6. Klicken Sie auf die **Ressourcen** Registerkarte, und erweitern Sie dann den Clientzugriffspunkt, die Sie gerade erstellt haben. 
    Die IP-Adressressource für jedes Clusternetzwerk in Ihrem Cluster angezeigt. Wenn dies eine reine Azure-Lösung ist, wird nur eine IP-Adressenressource angezeigt.

7. Führen Sie eine der folgenden:
   
   * So konfigurieren Sie Hybrid-Lösung:
     
        a. Mit der rechten Maustaste in der IP-Adressressource, die Ihrem lokalen Subnetz entspricht, und wählen Sie dann **Eigenschaften**. Beachten Sie die IP-Adressennamen und den Netzwerknamen.
   
        b. Wählen Sie **statische IP-Adresse**, eine nicht verwendete IP-Adresse zuweisen, und klicken Sie dann auf **OK**.
 
   * So konfigurieren Sie eine reine Azure-Lösung:

        a. Mit der rechten Maustaste in der IP-Adressressource, der Ihrem Azure-Subnetz entspricht, und wählen Sie dann **Eigenschaften**.
       
       > [!NOTE]
       > Fällt der Listener später aufgrund von einem in Konflikt stehende IP-Adresse von DHCP ausgewählten online ist, können Sie eine gültige statische IP-Adresse in diesem Eigenschaftenfenster konfigurieren.
       > 
       > 

       b. In der gleichen **IP-Adresse** im Eigenschaftenfenster ändern die **IP-Adressname**.  
        Dieser Name wird in der Variablen $IPResourceName des PowerShell-Skripts verwendet. Wenn Ihre Lösung virtueller Azure-Netzwerke umfasst, wiederholen Sie diesen Schritt für jede IP-Adressressource.


Der verfügbarkeitsgruppenlistener wird einen IP-Adresse und die Namen, dem Gruppe der SQL Server-Verfügbarkeit überwacht. Um die verfügbarkeitsgruppenlistener zu erstellen, führen Sie folgende Schritte aus:

1. <a name="getnet"></a>Ruft den Namen der Netzwerk-Clusterressource.

    a. Verwenden Sie RDP Verbindung mit virtuellen Azure-Computer, die das primäre Replikat hostet. 

    b. Failovercluster-Manager zu öffnen.

    c. Wählen Sie die **Netzwerke** Knoten, und notieren Sie den Clusternetzwerknamen. Verwenden Sie diesen Namen in der `$ClusterNetworkName` Variable im PowerShell-Skript. In der folgenden Abbildung ist der Netzwerkname des **Clusternetzwerk 1**:

   ![Netzwerkname](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Fügen Sie den Clientzugriffspunkt.  
    Der Client Access Point ist der Netzwerkname, der Anwendungen verwenden, um mit den Datenbanken in einer verfügbarkeitsgruppe herstellen. Der Client Access Point im Failovercluster-Manager zu erstellen.

    a. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

    b. In der **Rollen** Bereich mit der rechten Maustaste in des Namens der verfügbarkeitsgruppe, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. In der **Namen** Feld, erstellen Sie einen Namen für den neuen Listener. 
   Der Name für den neuen Listener ist der Netzwerkname, der Anwendungen verwenden, um die Verbindung mit Datenbanken in der SQL Server-verfügbarkeitsgruppe.
   
    d. Zum Erstellen des Listeners fertig sind, klicken Sie auf **Weiter** zweimal, und klicken Sie dann auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource online zu diesem Zeitpunkt.

3. <a name="congroup"></a>Konfigurieren Sie die IP-Adressressource für die verfügbarkeitsgruppe dienen.

    a. Klicken Sie auf die **Ressourcen** Registerkarte, und erweitern Sie dann den Clientzugriffspunkt, die Sie erstellt haben.  
    Der Clientzugriffspunkt ist offline.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Mit der rechten Maustaste in die IP-Adressressource, und klicken Sie dann auf Eigenschaften. Notieren Sie den Namen der IP-Adresse ein, und verwenden sie in der `$IPResourceName` Variable im PowerShell-Skript.

    c. Klicken Sie unter **IP-Adresse**, klicken Sie auf **statische IP-Adresse**. Legen Sie die IP-Adresse als die Adresse, die Sie verwendet werden, wenn Sie die Adresse des Lastenausgleichsmoduls auf dem Azure-Portal festgelegt.

   ![Die IP-Ressource](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Stellen Sie die SQL Server-verfügbarkeitsgruppenressource abhängig von den Clientzugriffspunkt.

    a. Klicken Sie im Failovercluster-Manager auf **Rollen**, und klicken Sie dann auf die verfügbarkeitsgruppe.

    b. Auf der **Ressourcen** Registerkarte **sonstige Ressourcen**mit der rechten Maustaste auf die verfügbarkeitsgruppe für die Ressource, und klicken Sie dann auf **Eigenschaften**. 

    c. Fügen Sie auf der Registerkarte "Abhängigkeiten" den Namen der Ressource Client Access Point (Listener).

   ![Die IP-Ressource](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klicken Sie auf **OK**.

5. <a name="listname"></a>Stellen Sie die Client Access Point Ressource abhängig von der IP-Adresse.

    a. Klicken Sie im Failovercluster-Manager auf **Rollen**, und klicken Sie dann auf die verfügbarkeitsgruppe. 

    b. Auf der **Ressourcen** Registerkarte der rechten Maustaste auf die Client Access Point-Ressource unter **Servernamen**, und klicken Sie dann auf **Eigenschaften**. 

   ![Die IP-Ressource](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klicken Sie auf die **Abhängigkeiten** Registerkarte. Stellen Sie sicher, dass die IP-Adresse abhängig ist. Wenn es nicht der Fall ist, legen Sie eine Abhängigkeit auf die IP-Adresse. Wenn es mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR, not AND-Abhängigkeiten. Klicken Sie auf **OK**. 

   ![Die IP-Ressource](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Maustaste auf den Listenernamen, und klicken Sie dann auf **Online schalten**. 

    >[!TIP]
    >Sie können überprüfen, dass die Abhängigkeiten ordnungsgemäß konfiguriert sind. Wechseln Sie im Failovercluster-Manager, Rollen, Maustaste auf die verfügbarkeitsgruppe, und klicken Sie auf **Weitere Aktionen**, und klicken Sie dann auf **Abhängigkeitsbericht**. Wenn die Abhängigkeiten ordnungsgemäß konfiguriert sind, die verfügbarkeitsgruppe ist abhängig von den Namen des Netzwerks und der Netzwerkname ist abhängig von der IP-Adresse. 


6. <a name="setparam"></a>Legen Sie die Clusterparameter in PowerShell aus.
    
    a. Kopieren Sie das folgende PowerShell-Skript in einen der SQL Server-Instanzen. Aktualisieren Sie die Variablen für Ihre Umgebung.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Legen Sie die Clusterparameter, durch das PowerShell-Skript auf einem Clusterknoten ausgeführt wird.  

    > [!NOTE]
    > Wenn die SQL Server-Instanzen in separate Bereiche befinden, müssen Sie das PowerShell-Skript zweimal ausführen. Das erste Mal verwenden die `$ILBIP` und `$ProbePort` aus dem ersten Bereich. Verwenden Sie das zweite Mal die `$ILBIP` und `$ProbePort` aus dem zweiten Bereich. Netzwerkname des Clusters und der Cluster-IP-Ressourcenname sind identisch. 

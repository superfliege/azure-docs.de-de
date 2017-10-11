* In **Hinzufügen von vCenter**, geben Sie einen Anzeigenamen für den vSphere-Host oder vCenter-Server, und geben Sie die IP-Adresse oder FQDN des Servers. Lassen Sie den Port als 443, es sei denn, Ihre VMware-Server zum Abhören von Anforderungen an einen anderen Port konfiguriert werden. Wählen Sie das Konto, das eine Verbindung mit dem VMware vCenter oder vSphere ESXi-Server ist. Klicken Sie auf **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Wenn Sie den VMware vCenter-Server oder VMware vSphere-Host mit einem Konto an, das Administratorrechte auf dem vCenter oder Host-Server nicht hinzufügen, stellen Sie sicher, dass das Konto dieser Berechtigungen aktiviert: Datacenter, Datenspeicher, Ordner, Hosts, Netzwerk, Ressource, virtuellen Computer und vSphere verteilte wechseln. Darüber hinaus benötigt der VMware vCenter-Server die Speicher Ansichten Berechtigung aktiviert.

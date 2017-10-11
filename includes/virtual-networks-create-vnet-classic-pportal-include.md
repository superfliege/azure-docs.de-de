## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Vorgehensweise: erstellen eine klassische VNet in Azure-portal
Führen Sie zum Erstellen einer klassischen Vnets basierend auf den oben genannten Szenario die folgenden Schritte aus.

1. Navigieren Sie in einem Browser zum http://portal.azure.com und ggf. zur Registrierung mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **Networking** > **virtuelles Netzwerk**, beachten Sie, dass die **Bereitstellungsmodell auswählen** Liste bereits zeigt **klassischen**, und klicken Sie dann auf **erstellen**, wie in der folgenden Abbildung hervorgeht.
   
    ![Erstellen von VNet in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Auf der **virtuelles Netzwerk** Blade, der Typ der **Namen** der VNet, und klicken Sie dann auf **Adressraum**. Konfigurieren Sie Ihre Adresse Speicherplatz für das VNet und seinem ersten Subnetz, und klicken Sie auf **OK**. Die folgende Abbildung zeigt die CIDR-Block-Einstellungen für unser Szenario.
   
    ![Adresse Blatt "Speicherplatz"](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Klicken Sie auf **Ressourcengruppe** , und wählen Sie eine Ressourcengruppe zum Hinzufügen des virtuellen Netzwerks, oder klicken Sie auf **neue Ressourcengruppe erstellen** Hinzufügen des virtuellen Netzwerks zu einer neuen Ressourcengruppe. Die folgende Abbildung zeigt die gruppeneinstellungen für eine neue Ressourcengruppe namens **TestRG**. Weitere Informationen zu Ressourcengruppen finden Sie auf [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
    ![Erstellen Sie Gruppe Ressourcenblatt](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. Ändern Sie bei Bedarf die **Abonnement** und **Speicherort** Einstellungen für das VNet. 
6. Wenn Sie nicht, finden in das VNet als Kachel in möchten der **Startmenü anzuheften**, deaktivieren Sie **an Startmenü anzuheften**. 
7. Klicken Sie auf **erstellen** , und beachten Sie die Kachel mit der Bezeichnung **erstellen virtuelles Netzwerk** wie in der folgenden Abbildung dargestellt.
   
    ![Erstellen Sie VNet im Verwaltungsportal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Warten Sie, bis das VNet erstellt werden, und wenn Sie die Kachel unten angezeigt wird, klicken Sie auf, um weitere Subnetze hinzufügen.
   
    ![Erstellen Sie VNet im Verwaltungsportal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Daraufhin sollte die **Konfiguration** für das VNet aus, wie unten dargestellt. 
   
    ![Erstellen Sie VNet im Verwaltungsportal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Klicken Sie auf **Subnetze** > **hinzufügen**, geben Sie dann eine **Namen** , und geben Sie eine **-Adressbereich (CIDR-Block)** für Ihr Subnetz, und klicken Sie dann auf **OK**. Die folgende Abbildung zeigt die Einstellungen für das aktuelle Szenario.
    
    ![Erstellen von VNet in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)


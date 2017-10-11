### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Erstellen Sie einen TCP-Endpunkt für den virtuellen Computer
Um SQL Server über das Internet zugreifen zu können, benötigen den virtuellen Computer einen Endpunkt zum Abhören von eingehenden TCP-Kommunikation. Diese Azure-Konfigurationsschritt leitet eingehenden TCP-Port-Datenverkehr an einen TCP-Port, der für die virtuelle Maschine zugänglich ist.

> [!NOTE]
> Wenn Sie innerhalb der gleichen Cloud-Dienst oder virtuellen Netzwerk verbinden, müssen Sie keinen öffentlich zugängliche Endpunkt zu erstellen. In diesem Fall können Sie mit dem nächsten Schritt fortfahren. Weitere Informationen finden Sie unter [Verbindungsszenarien](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. Wählen Sie im Azure-Verwaltungsportal **virtuelle Computer (klassisch)**.
2. Anschließend wählen Sie die SQL Server-Computer.
3. Wählen Sie **Endpunkte**, und klicken Sie dann auf die **hinzufügen** Schaltfläche oben auf dem Blatt "Endpunkte".
   
    ![Portal Schritte zum Erstellen des](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. Auf der **Add Endpoint** Blatt bieten eine **Namen** z. B. SQLEndpoint.
5. Wählen Sie **TCP** für die **Protokoll**.
6. Für **öffentlichen Port**, geben Sie z. B. eine Portnummer **57500**.
7. Für **privaten Port**, geben Sie die SQL Server Überwachungsport, dessen Standard **1433**.
8. Klicken Sie auf **Ok** einen Endpunkt erstellen.


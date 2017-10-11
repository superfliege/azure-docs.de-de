## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Vorgehensweise: erstellen ein virtuelles Netzwerks mithilfe einer Netzwerk-Config-Datei von PowerShell
Azure verwendet eine XML-Datei, um alle virtuellen Netzwerke zu einem Abonnement zu definieren. Sie können diese Datei herunterladen, bearbeiten, um ändern oder Löschen von vorhandenen virtuellen Netzwerken und neue virtuelle Netzwerke erstellen. In diesem Lernprogramm erfahren Sie, wie diese Dateidownload genannt Network Configuration (oder Netcfg-Datei), und bearbeiten, um ein neues virtuelles Netzwerk erstellen. Weitere Informationen zu der Netzwerkkonfigurationsdatei finden Sie unter der [Konfigurationsschema für virtuelle Azure-Netzwerk](https://msdn.microsoft.com/library/azure/jj157100.aspx).

Führen Sie zum Erstellen eines virtuellen Netzwerks mit einem "Netcfg"-Datei mithilfe von PowerShell die folgenden Schritte aus:

1. Wenn Sie Azure PowerShell nie verwendet haben, führen Sie die Schritte in der [installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) Artikel, und klicken Sie dann bei Azure anmelden, und wählen Sie Ihr Abonnement.
2. Verwenden von Azure PowerShell-Konsole die **Get AzureVnetConfig** Cmdlet, um die Netzwerkkonfigurationsdatei in ein Verzeichnis auf Ihrem Computer herunterladen, durch den folgenden Befehl ausführen: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Erwartete Ausgabe:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Öffnen Sie die Datei, die Sie in Schritt 2 mit jeder beliebigen XML- oder Text-Editor-Anwendung gespeichert, und suchen Sie nach der  **<VirtualNetworkSites>**  Element. Wenn Sie alle Netzwerke, die bereits erstellt haben, wird jedes Netzwerk angezeigt, als eigene  **<VirtualNetworkSite>**  Element.
4. Um das virtuelle Netzwerk in diesem Szenario beschriebenen zu erstellen, fügen Sie die folgenden XML-Code direkt unter der  **<VirtualNetworkSites>**  Element:

   ```xml
        <VirtualNetworkSite name="TestVNet" Location="East US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>
   ```
   
5. Speichern Sie die Netzwerkkonfigurationsdatei.
6. Verwenden von Azure PowerShell-Konsole die **Satz AzureVnetConfig** Cmdlet, um die Netzwerkkonfigurationsdatei hochladen, indem Sie den folgenden Befehl ausführen: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Zurückgegebenen Ausgabe:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Wenn **OperationStatus** nicht *erfolgreich* in die zurückgegebene Ausgabe, überprüfen Sie die XML-Datei für Fehler und Schritt 6 abgeschlossen.

7. Verwenden von Azure PowerShell-Konsole die **Get AzureVnetSite** Cmdlet, um sicherzustellen, dass das neue Netzwerk hinzugefügt wurde, indem Sie den folgenden Befehl ausführen: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   Die zurückgegebene Ausgabe (abgekürzte) enthält den folgenden Text:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```

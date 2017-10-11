## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Vorgehensweise: erstellen eine klassische VNet mithilfe von Azure-CLI
Sie können die Azure-CLI zum Verwalten von Azure-Ressourcen über die Eingabeaufforderung von jedem Computer unter Windows, Linux und OS x verwenden. Um ein VNet mit der Azure-CLI erstellen, führen Sie die folgenden Schritte aus.

1. Wenn Sie Azure-CLI nie verwendet haben, finden Sie unter [installieren und Konfigurieren der Azure-CLI](../articles/cli-install-nodejs.md) und befolgen Sie die Anweisungen bis zu dem Punkt, in dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die **Netzwerk in Azure-Vnet erstellen** Befehl aus, um ein VNet und ein Subnetz erstellen, wie unten gezeigt. Die Liste angezeigt, nachdem die Ausgabe wird, die verwendeten Parameter erläutert.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Erwartete Ausgabe:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Der Name des Vnets erstellt werden soll. Für unser Szenario *TestVNet*
   * **-e (oder---Adressbereich)**. VNet-Adressraum. Für unser Szenario *192.168.0.0*
   * **-i (oder - Cidr)**. Die Netzwerkmaske im CIDR-Format. Für unser Szenario *16*.
   * **-n: (oder--Subnet-Name**). Der Name der das erste Subnetz. Für unser Szenario *Front-End-*.
   * **-p (oder--Subnet-Start-Ip)**. IP-Startadresse für Subnetz oder eine Subnetzadresse. Für unser Szenario *192.168.1.0*.
   * **-R (oder--Subnet-Cidr)**. Die Netzwerkmaske im CIDR-Format für das Subnetz. Für unser Szenario *24*.
   * **-l (oder--Speicherort)**. Azure-Region, in dem das VNet erstellt wird. Für unser Szenario *USA Mitte*.
3. Führen Sie die **Netzwerk in Azure-Vnet-Subnetz erstellen** Befehl zum Erstellen eines Subnetzes, wie unten dargestellt. Die Liste angezeigt, nachdem die Ausgabe wird, die verwendeten Parameter erläutert.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Hier wird die erwartete Ausgabe für den oben aufgeführten Befehl ein:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (oder--Vnet-Name**. Der Name des Vnets, in dem das Subnetz erstellt wird. Für unser Szenario *TestVNet*.
   * **-n (oder – Name)**. Der Name des neuen Subnetzes. Für unser Szenario *Back-End-*.
   * **– ein (oder---Adresspräfix)**. Subnetz CIDR-Block. Vier unser Szenario *192.168.2.0/24*.
4. Führen Sie die **Netzwerk in Azure Vnet anzeigen** Befehl, um die Eigenschaften des im neuen Vnet anzuzeigen, wie unten dargestellt.
   
            azure network vnet show
   
    Hier wird die erwartete Ausgabe für den oben aufgeführten Befehl ein:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK


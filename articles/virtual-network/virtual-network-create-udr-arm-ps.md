---
title: "Erstellen einer benutzerdefinierten Route zum Weiterleiten des Netzwerkdatenverkehrs über ein virtuelles Netzwerkgerät – PowerShell | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine benutzerdefinierte Route erstellen, mit der Sie den Netzwerkdatenverkehr über ein virtuelles Netzwerkgerät weiterleiten und so das Azure-Standardrouting außer Kraft setzen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: 4ca7f791b4c5c8bb9020144785b1c1aeb20db195
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="create-a-user-defined-route---powershell"></a>Erstellen einer benutzerdefinierten Route – PowerShell

In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte Route erstellen, mit der Sie Netzwerkdatenverkehr über ein virtuelles Netzwerkgerät von einem Subnetz an ein anderes Subnetz weiterleiten, die beide Teil eines [virtuellen Netzwerks](virtual-networks-overview.md) sind. Ein virtuelles Netzwerkgerät ist eine VM, auf der eine Netzwerkanwendung wie z.B. eine Firewall ausgeführt wird. Weitere Informationen zu vorkonfigurierten virtuellen Netzwerkgeräten, die Sie in einem virtuellen Azure-Netzwerk bereitstellen können, finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Wenn Sie Subnetze in einem virtuellen Netzwerk erstellen, erstellt Azure [Standardsystemrouten](virtual-networks-udr-overview.md#system-routes), durch die Ressourcen in allen Subnetzen wie in der folgenden Abbildung gezeigt miteinander kommunizieren können:

![Standardrouten](./media/create-user-defined-route/default-routes.png)

In diesem Tutorial erstellen Sie wie in der folgenden Abbildung gezeigt ein virtuelles Netzwerk mit einem öffentlichen und privaten Subnetz sowie einem DMZ-Subnetz. In der Regel werden Webserver in einem öffentlichen Subnetz und Anwendungen oder Datenbankserver in einem privaten Subnetz bereitgestellt. Sie erstellen in diesem Tutorial eine VM, die als virtuelles Netzwerkgerät innerhalb des DMZ-Subnetzes verwendet wird. Optional können Sie außerdem in jedem Subnetz eine VM erstellen, die über das virtuelle Netzwerkgerät mit einer anderen VM kommuniziert. Der gesamte Netzwerkdatenverkehr wird wie in der folgenden Abbildung gezeigt über das virtuelle Netzwerkgerät vom öffentlichen an das private Subnetz weitergeleitet:

![Benutzerdefinierte Routen](./media/create-user-defined-route/user-defined-routes.png)

In diesem Artikel wird das Erstellen einer benutzerdefinierten Route über das Azure Resource Manager-Bereitstellungsmodell beschrieben. Dieses Bereitstellungsmodell wird für das Erstellen benutzerdefinierter Routen empfohlen. Wenn Sie stattdessen eine benutzerdefinierten Route mithilfe des klassischen Azure-Bereitstellungsmodells erstellen müssen, finden Sie unter [Erstellen einer benutzerdefinierten Route (klassisch)](virtual-network-create-udr-classic-ps.md) weitere Informationen. Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie unter [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nach. Für weitere Informationen zu benutzerdefinierten Routen können Sie sich die [Übersicht zu benutzerdefinierten Routen](virtual-networks-udr-overview.md#user-defined-routes) ansehen.

## <a name="create-routes-and-network-virtual-appliance"></a>Erstellen der Routen und des virtuellen Netzwerkgeräts

Zum Ausführen der Skripts in Azure Cloud Shell können Sie entweder die neueste Version des [AzureRM](https://www.powershellgallery.com/packages/AzureRM/)-Moduls für PowerShell auf Ihrem PC installieren und konfigurieren oder einfach auf die Schaltfläche **Ausprobieren** in einem Skript klicken. Für Cloud Shell ist das AzureRM-Modul für PowerShell bereits installiert.
 
1. **Erforderliche Vorbereitung**: Erstellen Sie ein virtuelles Netzwerk mit zwei Subnetzen, indem Sie die Schritte unter [Erstellen eines virtuellen Netzwerks](#create-a-virtual-network) ausführen.
2. Wenn Sie PowerShell auf Ihrem Computer ausführen, melden Sie sich bei Azure mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an, indem Sie den Befehl `login-azurermaccount` ausführen. Wenn Sie Cloud Shell verwenden, werden Sie automatisch angemeldet. Cloud Shell muss möglicherweise neu gestartet werden, wenn Sie einen Wechsel von der Bash-Shell durchführen, die bei der Erstellung des erforderlichen virtuellen Netzwerks verwendet wurde.
3. Legen Sie die folgenden Variablen für die nächsten Schritte fest:

    ```azurepowershell-interactive
    $rgName="myResourceGroup"
    $location="eastus"
    ```

4. Erstellen Sie im vorhandenen virtuellen Netzwerk ein DMZ-Subnetz:

    ```azurepowershell-interactive
    $virtualNetwork = Get-AzureRmVirtualNetwork `
      -Name myVnet `
      -ResourceGroupName $rgName 
    Add-AzureRmVirtualNetworkSubnetConfig `
      -Name 'DMZ' `
      -AddressPrefix "10.0.2.0/24" `
      -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzureRmVirtualNetwork
    ```

5. Erstellen Sie eine statische öffentliche IP-Adresse für die VM des virtuellen Netzwerkgeräts.

    ```azurepowershell-interactive
    $Pip = New-AzureRmPublicIpAddress `
      -AllocationMethod Static `
      -ResourceGroupName $rgName `
      -Location $location `
      -Name myPublicIp-myVm-Nva
    
6. Create a network interface in the *DMZ* subnet, assign a static private IP address to it, and enable IP forwarding for the network interface. By assigning a static IP address to the network interface, you ensure that it doesn't change for the life of the virtual machine the network interface is attached to. IP forwarding must be enabled for each network interface that receives traffic not addressed to an IP address assigned to it.

    ```azurepowershell-interactive
    $virtualNetwork = Get-AzureRmVirtualNetwork `
      -Name myVnet `
      -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig `
      -Name DMZ `
      -VirtualNetwork $virtualNetwork
    $nic = New-AzureRmNetworkInterface `
      -ResourceGroupName $rgName `
      -Location $location `
      -Name 'myNic-Nva' `
      -SubnetId $subnet.Id `
      -PrivateIpAddress 10.0.2.4 `
      -EnableIPForwarding `
      -PublicIpAddressId $Pip.Id 
    ```

7. Erstellen Sie die VM des virtuellen Netzwerkgeräts. Beim virtuellen Netzwerkgerät kann es sich um eine VM handeln, auf der das Betriebssystem Linux oder Windows ausgeführt wird. Kopieren Sie zur Erstellung der VM das Skript für eines der Betriebssysteme, und fügen Sie es in die PowerShell-Sitzung ein. Wenn Sie eine Windows-VM erstellen, fügen Sie das Skript in einen Text-Editor ein, ändern Sie den Wert der Variable „$cred“, und fügen Sie den geänderten Text in die PowerShell-Sitzung ein:

    **Linux**

    ```azurepowershell-interactive
    # Define the admin user name and a blank password.
    $securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

    # Define the virtual machine configuration.
    $vmConfig = New-AzureRmVMConfig `
      -VMName 'myVm-Nva' `
      -VMSize Standard_DS2 | `
      Set-AzureRmVMOperatingSystem -Linux `
      -ComputerName 'myVm-Nva' `
      -Credential $cred -DisablePasswordAuthentication| `
      Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 14.04.2-LTS `
      -Version latest | `
      Add-AzureRmVMNetworkInterface -Id $nic.Id

    # Configure SSH Keys
    $sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
    Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"    

    # Create the virtual machine
    $vm = New-AzureRmVM `
      -ResourceGroupName $rgName `
      -Location $location `
      -VM $vmConfig
    ```

    **Windows**

    ```azurepowershell-interactive
    # Create user object
    $cred = Get-Credential -Message "Enter a username and password for the virtual machine."

    $vmConfig = New-AzureRmVMConfig `
      -VMName 'myVm-Nva' `
      -VMSize Standard_DS2 | `
      Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName 'myVm-Nva' `
      -Credential $cred | `
      Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
      Add-AzureRmVMNetworkInterface -Id $nic.Id
    
    $vm = New-AzureRmVM `
      -ResourceGroupName $rgName `
      -Location $location `
      -VM $vmConfig
    ```

8. Standardmäßig leitet Azure den Datenverkehr durch alle Subnetze des virtuellen Netzwerks. Damit der Datenverkehr des *öffentlichen* Subnetzes nicht direkt an das *private* Subnetz, sondern an das virtuelle Netzwerkgerät weitergeleitet wird, müssen Sie eine Route erstellen, um das Azure-Standardrouting außer Kraft zu setzen.

    ```azurepowershell-interactive    
    $routePrivate = New-AzureRmRouteConfig `
      -Name 'ToPrivateSubnet' `
      -AddressPrefix 10.0.1.0/24 `
      -NextHopType VirtualAppliance `
      -NextHopIpAddress $nic.IpConfigurations[0].PrivateIpAddress
    ```

9. Erstellen Sie eine Routingtabelle für das *öffentliche* Subnetz.

    ```azurepowershell-interactive
    $routeTablePublic = New-AzureRmRouteTable `
      -Name 'myRouteTable-Public' `
      -ResourceGroupName $rgName `
      -location $location `
      -Route $routePrivate
    ```
    
10. Ordnen Sie die Routingtabelle dem öffentlichen Subnetz zu. Hierdurch leitet Azure den gesamten ausgehenden Datenverkehr aus dem Subnetz entsprechend der Routen in der Routingtabelle weiter. Eine Routingtabelle kann entweder keinem oder mehreren Subnetzen zugeordnet werden. Einem Subnetz kann hingegen entweder keine oder genau eine Routingtabelle zugeordnet werden.

    ```azurepowershell-interactive
    Set-AzureRmVirtualNetworkSubnetConfig `
      -VirtualNetwork $virtualNetwork `
      -Name 'Public' `
      -AddressPrefix 10.0.0.0/24 `
      -RouteTable $routeTablePublic | `
    Set-AzureRmVirtualNetwork
    ```

11. Erstellen Sie eine Route, die über die VM des virtuellen Netzwerkgeräts den Datenverkehr vom *privaten* an das *öffentliche* Subnetz weiterleitet.

    ```azurepowershell-interactive    
    $routePublic = New-AzureRmRouteConfig `
      -Name 'ToPublicSubnet' `
      -AddressPrefix 10.0.0.0/24 `
      -NextHopType VirtualAppliance `
      -NextHopIpAddress $nic.IpConfigurations[0].PrivateIpAddress
    ```

12. Erstellen Sie die Routingtabelle für das *private* Subnetz.

    ```azurepowershell-interactive
    $routeTablePrivate = New-AzureRmRouteTable `
      -Name 'myRouteTable-Private' `
      -ResourceGroupName $rgName `
      -location $location `
      -Route $routePublic
    ```
      
13. Ordnen Sie die Routingtabelle dem *privaten* Subnetz zu.

    ```azurepowershell-interactive
    Set-AzureRmVirtualNetworkSubnetConfig `
      -VirtualNetwork $virtualNetwork `
      -Name 'Private' `
      -AddressPrefix 10.0.1.0/24 `
      -RouteTable $routeTablePrivate | `
    Set-AzureRmVirtualNetwork
    ```
    
14. **Optional**: Erstellen Sie jeweils eine VM im öffentlichen und privaten Subnetz, und überprüfen Sie, ob der Datenverkehr zwischen den VMs über das virtuelle Netzwerkgerät weitergeleitet wird. Führen Sie hierzu die Schritte im Abschnitt [Überprüfen des Routings](#validate-routing) aus.
15. **Optional**: Führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-resources) aus, um die Ressourcen zu löschen, die Sie in diesem Tutorial erstellen.

## <a name="validate-routing"></a>Überprüfen des Routings

1. Führen Sie die Schritte unter [Erstellen von Routen und virtuellen Netzwerkgeräten](#create-routes-and-network-virtual-appliance) aus, wenn Sie dies noch nicht getan haben.
2. Klicken Sie im folgenden Feld auf die Schaltfläche **Ausprobieren**. Dadurch wird Azure Cloud Shell geöffnet. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) bei Azure an, wenn Sie dazu aufgefordert werden. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren. Azure Cloud Shell ist eine kostenlose Bash-Shell, für die Azure CLI vorinstalliert ist. 

    Mit den folgenden Skripts werden zwei VMs erstellt – eine im *öffentlichen* und eine im *privaten* Subnetz. Mit den Skripts wird auch die IP-Weiterleitung für die Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Netzwerkgeräts aktiviert. Dadurch kann das Betriebssystem den Netzwerkdatenverkehr über die Netzwerkschnittstelle weiterleiten. Ein virtuelles Netzwerkgerät in einer Produktionsumgebung überprüft den Netzwerkdatenverkehr in der Regel vor der Weiterleitung. In diesem Tutorial leitet das einfache virtuelle Netzwerkgerät den Datenverkehr allerdings ohne Überprüfung weiter. 

    Klicken Sie im folgenden **Linux**- oder **Windows**-Skript auf die Schaltfläche **Kopieren**, und fügen Sie den Inhalt des Skripts in einen Text-Editor ein. Ändern Sie das Kennwort für die Variable *adminPassword*, und fügen Sie das Skript in Azure Cloud Shell ein. Führen Sie das Skript für das Betriebssystem aus, das Sie in Schritt 7 im Abschnitt [Erstellen von Routen und virtuellen Netzwerkgeräten](#create-routes-and-network-virtual-appliance) bei der Erstellung des virtuellen Netzwerkgeräts ausgewählt haben. 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Überprüfen Sie, ob die VMs des öffentlichen und privaten Subnetzes miteinander kommunizieren. 

    - Stellen Sie unter Linux eine [SSH-Verbindung](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) oder unter Windows eine [Remotedesktop-Verbindung](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) mit der öffentlichen IP-Adresse der *myVm-Public-VM* her.
    - Geben Sie in der Eingabeaufforderung der *myVm-Public-VM* `ping myVm-Private` ein. Es werden Antworten empfangen, da das virtuelle Netzwerkgerät den Netzwerkdatenverkehr vom öffentlichen an das private Subnetz weiterleitet.
    - Führen Sie in der *myVm-Public-VM* einen Traceroute-Vorgang für die VMs im öffentlichen und privaten Subnetz aus. Geben Sie je nach installiertem Betriebssystem der VM im öffentlichen und privaten Subnetz einen der folgenden Befehle ein:
        - **Windows**: Führen Sie in der Eingabeaufforderung den Befehl `tracert myvm-private` aus.
        - **Ubuntu**: Führen Sie den Befehl `tracepath myvm-private` aus.
      Der Netzwerkdatenverkehr wird zuerst an die IP-Adresse 10.0.2.4 (das virtuelle Netzwerkgerät) und anschließend an die IP-Adresse 10.0.1.4 (die VM im privaten Subnetz) weitergeleitet. 
    - Führen Sie die vorherigen Schritte aus, indem Sie eine Verbindung mit der *myVm-Private-VM* herstellen und die *myVm-Public-VM* anpingen. Die Traceroute veranschaulicht den Netzwerkdatenverkehr, der über die IP-Adresse 10.0.2.4 an die IP-Adresse 10.0.0.4 (die VM im öffentlichen Subnetz) weitergeleitet wird.
    - **Optional**: Verwenden Sie die Funktion „Nächster Hop“ von Azure Network Watcher, um den nächsten Hop zwischen zwei VMs in Azure zu überprüfen. Vor der Verwendung von Network Watcher müssen Sie zuerst [eine Instanz von Azure Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für die Region erstellen, in der Sie diesen Dienst verwenden möchten. In diesem Tutorial wird die Region „USA, Osten“ verwendet. Nachdem Sie eine Network Watcher-Instanz für die Region erstellt haben, geben Sie den folgenden Befehl ein, um sich Informationen zum nächsten Hop zwischen den VMs im öffentlichen und privaten anzeigen Subnetz anzeigen zu lassen:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       In der Ausgabe wird *10.0.2.4* für **nextHopIpAddress** und *VirtualAppliance* für **nextHopType** angezeigt.

> [!NOTE]
> Um die Konzepte in diesem Tutorial zu veranschaulichen, werden den VMs im öffentlichen und privaten Subnetz öffentliche IP-Adressen zugewiesen. Außerdem wird der Zugriff auf Netzwerkports in Azure für beide VMs aktiviert. Wenn Sie VMs für die Produktion erstellen, dürfen Sie diesen keine öffentliche IP-Adressen zuweisen. Außerdem stehen Ihnen zwei Möglichkeiten zur Verfügung, den an das private Subnetz weitergeleitete Netzwerkdatenverkehr filtern: Sie können entweder ein virtuelles Netzwerkgerät vor dem Subnetz bereitstellen oder aber den Subnetzen, der Netzwerkschnittstelle oder beiden eine Netzwerksicherheitsgruppe zuweisen. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Für dieses Tutorial ist ein vorhandenes virtuelles Netzwerk mit zwei Subnetzen erforderlich. Klicken Sie im folgenden Feld auf **Ausprobieren**, um schnell ein virtuelles Netzwerk zu erstellen. Durch Klicken auf die Schaltfläche **Ausprobieren** wird [Azure Cloud Shell](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) geöffnet. Obwohl in Cloud Shell sowohl PowerShell als auch die Bash-Shell ausgeführt werden können, wird in diesem Abschnitt die Bash-Shell verwendet, um das virtuelle Netzwerk zu erstellen. Für die Bash-Shell ist die Azure CLI installiert. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) bei Azure an, wenn Sie von Cloud Shell dazu aufgefordert werden. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren. Klicken Sie zur Erstellung des in diesem Tutorial verwendeten virtuellen Netzwerks im folgenden Feld auf die Schaltfläche **Kopieren**, und fügen Sie das Skript in Azure Cloud Shell ein:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Weitere Informationen zur Verwendung des Portals, von PowerShell oder von Azure Resource Manager-Vorlagen zum Erstellen eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Löschen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht. Geben Sie in PowerShell den folgenden Befehl ein:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie ein [hoch verfügbares virtuelles Netzwerkgerät](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuellen Netzwerkgeräten sind häufig mehrere Netzwerkschnittstellen und IP-Adressen zugewiesen. Erfahren Sie, wie Sie [einer vorhandenen VM Netzwerkschnittstellen hinzufügen](virtual-network-network-interface-vm.md#vm-add-nic) und [einer vorhandenen Netzwerkschnittstelle IP-Adressen hinzufügen](virtual-network-network-interface-addresses.md#add-ip-addresses). Obwohl VMs aller Größenkategorien mindestens über zwei Netzwerkschnittstellen verfügen, ist für jede Größenkategorie eine maximale Anzahl von unterstützten Netzwerkschnittstellen vorgesehen. Informationen zur maximalen Anzahl von unterstützten Netzwerkschnittstellen für die jeweilige Größenkategorie der VM finden Sie in den Artikeln zu [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json)- und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VM-Größenkategorien. 
- Erstellen Sie eine benutzerdefinierte Route, um den Netzwerkdatenverkehr mittels Tunnelerzwingung über eine [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) an einen lokalen Standort zu übertragen.

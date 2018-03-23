---
title: Weiterleiten von Netzwerkdatenverkehr – PowerShell | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie mithilfe von PowerShell Netzwerkdatenverkehr mit einer Routingtabelle weitergeleitet wird.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f91b143c75a82aa6760796770b3ae4d0e4ec53dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Weiterleiten von Netzwerkdatenverkehr über eine Routingtabelle mithilfe von PowerShell

Standardmäßig leitet Azure den Datenverkehr automatisch durch alle Subnetze des virtuellen Netzwerks. Sie können Ihre eigenen Routen zum Überschreiben des Azure-Standardroutings erstellen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist hilfreich, wenn Sie z.B. über eine Firewall Datenverkehr durch Subnetze weiterleiten möchten. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Zuordnen einer Routingtabelle zu einem Subnetz des virtuellen Netzwerks
> * Testen des Routings
> * Problembehandlung für Routingfehler

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

Standardmäßig leitet Azure den Datenverkehr durch alle Subnetze des virtuellen Netzwerks. Weitere Informationen zu Azure-Standardrouten finden Sie unter [Systemrouten](virtual-networks-udr-overview.md). Um das Azure-Standardrouting zu überschreiben, erstellen Sie eine Routingtabelle mit Routen und ordnen die Routingtabelle zum Subnetz eines virtuellen Netzwerks zu.

Bevor Sie eine Routingtabelle erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* für alle in diesem Artikel erstellten Ressourcen erstellt: 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Erstellen Sie eine Routingtabelle mit [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable). Im folgenden Beispiel wird eine Routingtabelle mit dem Namen *myRouteTablePublic* erstellt.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Erstellen einer Route

Eine Routingtabelle enthält null oder mehr Routen. Erstellen Sie eine Route, indem Sie ein Routingtabellenobjekt mit [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable) abrufen, eine Route mit [Add-AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig) erstellen und dann die Routingkonfiguration mit [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable) in die Routingtabelle schreiben. 

```azurepowershell-interactive
Get-AzureRmRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzureRmRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzureRmRouteTable
```

Die Route leitet sämtlichen an das Adresspräfix 10.0.1.0/24 gerichteten Datenverkehr über ein virtuelles Netzwerkgerät mit der IP-Adresse 10.0.2.4 weiter. Das virtuelle Netzwerkgerät und das Subnetz mit dem angegebenen Adresspräfix werden in den späteren Schritten erstellt. Die Route überschreibt das Azure-Standardrouting, das direkt Datenverkehr durch Subnetze weiterleitet. Jede Route gibt einen Typ des nächsten Hops an. Der Typ des nächste Hops weist Azure an, wie der Datenverkehr weitergeleitet werden soll. In diesem Beispiel handelt es sich beim Typ des nächsten Hops um *VirtualAppliance*. Um mehr über alle in Azure verfügbaren Typen des nächsten Hops und deren Verwendung zu erfahren, lesen Sie den Artikel zu den [Typen des nächsten Hops](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle zu einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen. Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Erstellen Sie durch die Erstellung dreier Subnetzkonfigurationen drei Subnetze mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Im folgenden Beispiel werden drei Subnetzkonfigurationen für die Subnetze *Öffentlich*, *Privat* und *DMZ* erstellt:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Die Adresspräfixe müssen innerhalb des für das virtuelle Netzwerk angegebenen Adresspräfixes liegen. Die Subnetzadresspräfixe dürfen einander nicht überlappen.

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dabei werden die Subnetze im virtuellen Netzwerk erstellt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Sie können null oder mehr Subnetzen eine Routingtabelle zuordnen. Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden. Ausgehender Datenverkehr aus einem Subnetz wird auf Grundlage der Azure-Standardrouten weitergeleitet und alle benutzerdefinierten Routen, die Sie einer Routentabelle hinzugefügt haben, ordnen Sie einem Subnetz zu. Ordnen Sie die Routingtabelle *myRouteTablePublic* mithilfe von [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) dem Subnetz *Öffentlich* zu, und schreiben Sie die Subnetzkonfiguration dann mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in das virtuelle Netzwerk.

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

Vor der Bereitstellung von Routingtabellen für die Produktion wird empfohlen, sich gründlich mit dem [Routing in Azure](virtual-networks-udr-overview.md) und den [Azure-Grenzwerten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut zu machen.

## <a name="test-routing"></a>Testen des Routings

Erstellen Sie zum Testen des Routings einen virtuellen Computer, der als virtuelles Netzwerkgerät fungiert, über das die im vorherigen Schritt erstellte Route Datenverkehr weiterleitet. Nachdem Sie das virtuelle Netzwerkgerät erstellt haben, stellen Sie einen virtuellen Computer in den Subnetzen *Öffentlich* und *Privat* bereit. Dann leiten Sie über das virtuelle Netzwerkgerät Datenverkehr vom Subnetz *Privat* an das Subnetz *Öffentlich* weiter.

### <a name="create-a-network-virtual-appliance"></a>Erstellen eines virtuellen Netzwerkgeräts

Wenn ein virtueller Computer eine Netzwerkanwendung ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bezeichnet. Virtuelle Netzwerkgeräte empfangen in der Regel Netzwerkdatenverkehr, führen einige Aktionen aus, leiten dann basierend auf der in der Netzwerkanwendung konfigurierten Logik Netzwerkdatenverkehr weiter bzw. verwerfen diesen. 

#### <a name="create-a-network-interface"></a>Erstellen einer Netzwerkschnittstelle

In einem vorherigen Schritt haben Sie eine Route erstellt, die ein virtuelles Netzwerkgerät als Typ des nächsten Hops angibt. Wenn ein virtueller Computer eine Netzwerkanwendung ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bezeichnet. In Produktionsumgebungen handelt es sich bei dem virtuellen Netzwerkgerät, das Sie bereitstellen, häufig um einen vorkonfigurierten virtuellen Computer. Die verschiedenen virtuellen Netzwerkgeräte sind im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1) verfügbar. In diesem Artikel wird ein grundlegender virtueller Computer erstellt.

An einen virtuellen Computer sind eine oder mehrere Netzwerkschnittstellen angefügt, die dem virtuellen Computer die Kommunikation mit dem Netzwerk ermöglichen. Damit Netzwerkdatenverkehr, der nicht für seine eigene IP-Adresse bestimmt ist, an eine Netzwerkschnittstelle gesendet werden kann, muss die IP-Weiterleitung für die Netzwerkschnittstelle aktiviert sein. Vor dem Erstellen einer Netzwerkschnittstelle müssen Sie mit [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) die ID des virtuellen Netzwerks und dann mit [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) die ID des Subnetzes abrufen. Erstellen Sie mit aktivierter IP-Weiterleitung eine Netzwerkschnittstelle mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) im Subnetz *DMZ*:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzureRmVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

#### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Um einen virtuellen Computer zu erstellen und diesem eine vorhandene Netzwerkschnittstelle anzufügen, müssen Sie mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zuerst die Konfiguration eines virtuellen Computers erstellen. Die Konfiguration beinhaltet die im vorherigen Schritt erstellte Netzwerkschnittstelle. Wählen Sie bei entsprechender Aufforderung den Benutzernamen und das Kennwort aus, mit denen Sie sich beim virtuellen Computer anmelden möchten. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine configuration.
$vmConfig = New-AzureRmVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer über der Konfiguration des virtuellen Computers. Im folgenden Beispiel wird ein virtueller Computer namens *myVmNva* erstellt. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können. Geben Sie bei entsprechender Aufforderung den Benutzernamen und das Kennwort ein, mit denen Sie sich beim virtuellen Computer anmelden möchten. In Produktionsumgebungen handelt es sich bei dem virtuellen Netzwerkgerät, das Sie bereitstellen, häufig um einen vorkonfigurierten virtuellen Computer. Die verschiedenen virtuellen Netzwerkgeräte sind im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1) verfügbar.

Azure hat 10.0.2.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.2.4 die erste verfügbare IP-Adresse im Subnetz *DMZ* von *myVirtualNetwork* ist.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie in einem späteren Schritt diesen Datenverkehr aus dem Subnetz *Öffentlich* über das virtuelle Netzwerkgerät an das Subnetz *Privat* weiterleiten können. 

Erstellen Sie mit *New-AzureRmVM* einen virtuellen Computer im Subnetz [Öffentlich](/powershell/module/azurerm.compute/new-azurermvm). Im folgenden Beispiel wird ein virtueller Computer namens *myVmWeb* im Subnetz *Öffentlich* des virtuellen Netzwerks *myVirtualNetwork* erstellt. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmWeb" `
  -AsJob
```

Azure hat 10.0.0.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.1.4 die erste verfügbare IP-Adresse im Subnetz *Öffentlich* von *myVirtualNetwork* ist.

Erstellen Sie einen virtuellen Computer im Subnetz *Privat*.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmMgmt"
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Azure hat 10.0.1.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.1.4 die erste verfügbare IP-Adresse im Subnetz *Privat* von *myVirtualNetwork* ist. 

Fahren Sie erst mit dem nächsten Schritt fort, wenn der virtuelle Computer erstellt ist und Azure eine Ausgabe an PowerShell zurückgegeben hat.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

Aktivieren Sie mit [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) eingehenden Datenverkehr über ICMP für die virtuellen Computer *myVmWeb* und *myVmMgmt* über die Windows-Firewall, um in einem späteren Schritt die Kommunikation zwischen den virtuellen Computern mithilfe von Tracert zu testen:

```powershell-interactive
Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmWeb `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS

Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmMgmt `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS
```

Die Ausführung der zuvor genannten Befehle kann eine Weile dauern. Fahren Sie erst mit dem nächsten Schritt fort, wenn die Ausführung der Befehle abgeschlossen ist und die Ausgabe an PowerShell zurückgegeben wurde. In diesem Artikel wird zwar Tracert verwendet, das Zulassen des ICMP-Datenverkehrs durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.

Sie stellen über das Internet eine Verbindung mit der öffentlichen IP-Adresse eines virtuellen Computers her. Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVmMgmt* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung mit dem virtuellen Computer *myVmMgmt*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt, auf Ihren Computer heruntergeladen und geöffnet. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren. 

Sie haben die IP-Weiterleitung innerhalb von Azure für die Netzwerkschnittstelle des virtuellen Computers unter [IP-Weiterleitung aktivieren](#enable-ip-forwarding) aktiviert. Ein Betriebssystem oder eine Anwendung, das bzw. die auf dem virtuellen Computer ausgeführt wird, muss ebenfalls Netzwerkdatenverkehr weiterleiten können. Wenn Sie ein virtuelles Netzwerkgerät in einer Produktionsumgebung bereitstellen, führt die Appliance in der Regel vor der Weiterleitung von Datenverkehr eine Filterung oder Protokollierung durch oder führt eine andere Funktion aus. In diesem Artikel leitet das Betriebssystem jedoch einfach den gesamten Datenverkehr weiter, den es empfängt. Aktivieren Sie die IP-Weiterleitung innerhalb des Betriebssystems von *myVmNva*:

Stellen Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVmMgmt* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmNva* her:

``` 
mstsc /v:myVmNva
```
    
Um die IP-Weiterleitung innerhalb des Betriebssystems des virtuellen Computers *myVmNva* zu aktivieren, geben Sie auf dem virtuellen Computer *myVmNva* den folgenden Befehl in PowerShell ein:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Starten Sie den virtuellen Computer *myVmNva* neu. Hierdurch wird auch die zugehörige Remotedesktopsitzung getrennt, wobei die für den virtuellen Computer *myVmMgmt* geöffnete Remotedesktopsitzung bestehen bleibt.

Nachdem der virtuelle Computer *myVmNva* neu gestartet wurde, testen Sie mithilfe des folgenden Befehls das Routing für den Netzwerkdatenverkehr vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb*.

```
tracert myvmweb
```

Die Antwort ähnelt dem folgenden Beispiel:

```
Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
  
Trace complete.
```

Beachten Sie, dass Datenverkehr direkt vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb* weitergeleitet wird. Azure-Standardrouten leiten Datenverkehr direkt durch Subnetze weiter.

Verwenden Sie den folgenden Befehl, um vom virtuellen Computer *myVmMgmt* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmWeb* herzustellen:

``` 
mstsc /v:myVmWeb
```

Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um Netzwerkdatenverkehr vom virtuellen Computer *myVmWeb* an den virtuellen Computer *myVmMgmt* weiterzuleiten:

```
tracert myvmmgmt
```

Die Antwort ähnelt dem folgenden Beispiel:

```
Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```

Wie Sie sehen können, ist der erste Hop 10.0.2.4 – die private IP-Adresse des virtuellen Netzwerkgeräts. Der zweite Hop ist 10.0.1.4, die private IP-Adresse des virtuellen Computers *myVmMgmt*. Die Route, die zur Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Öffentlich* zugeordnet wurde, hat bewirkt, dass Azure Datenverkehr über die NVA weiterleitet statt direkt über das Subnetz *Privat*.

Schließen Sie die Remotedesktopsitzungen für die virtuellen Computer *myVmWeb* und *myVmMgmt*.

## <a name="troubleshoot-routing"></a>Problembehandlung für Routingfehler

Wie Sie in den vorherigen Schritten erfahren haben, wendet Azure Standardrouten an, die Sie wahlweise mit Ihren eigenen Routen überschreiben können. In manchen Fällen kann Datenverkehr nicht gemäß ihren Vorstellungen weitergeleitet werden. Verwenden Sie [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher), um Network Watcher in der Region „USA, Osten“ zu aktivieren, sofern noch keine Network Watcher-Instanz in dieser Region vorhanden ist:

```azurepowershell-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region:
$nw = New-AzureRmNetworkWatcher `
 -Location eastus `
 -Name myNetworkWatcher_eastus `
 -ResourceGroupName myResourceGroup
```

Verwenden Sie [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop), um zu bestimmen, wie Datenverkehr zwischen zwei virtuellen Computern weitergeleitet wird. Der folgende Befehl testet z.B. das Datenverkehrrouting vom virtuellen Computer *myVmWeb* (10.0.0.4) an den virtuellen Computer *myVmMgmt* (10.0.1.4):

```azurepowershell-interactive
$vmWeb = Get-AzureRmVM `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup

Get-AzureRmNetworkWatcherNextHop `
  -DestinationIPAddress 10.0.1.4 `
  -NetworkWatcherName myNetworkWatcher_eastus `
  -ResourceGroupName myResourceGroup `
  -SourceIPAddress 10.0.0.4 `
  -TargetVirtualMachineId $vmWeb.Id
```
Nach einem kurzen Moment wird die folgende Ausgabe zurückgegeben:

```azurepowershell
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Die Ausgabe gibt an, dass die IP-Adresse des nächsten Hops für den Datenverkehr von *myVmWeb* an *myVmMgmt* 10.0.2.4 lautet (der virtuelle Computer *myVmNva*), der nächste Hop vom Typ *VirtualAppliance* ist und die das Routing auslösende Routingtabelle *myRouteTablePublic* ist.

Die effektiven Routen für die einzelnen Netzwerkschnittstellen stellen eine Kombination aus Azure-Standardrouten und sämtlichen Routen dar, die Sie definieren. Zeigen Sie mit [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) alle effektiven Routen für eine Netzwerkschnittstelle auf einem virtuellen Computer an. Um beispielsweise die wirksamen Routen für die Netzwerkschnittstelle *myVmWeb* auf dem virtuellen Computer *myVmWeb* anzuzeigen, geben Sie den folgenden Befehl ein:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Alle Standardrouten und die Route, die Sie in einem früheren Schritt hinzugefügt haben, werden zurückgegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein virtuelles Netzwerkgerät erstellt, die Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Sie können zwar viele Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, für einige Azure-PaaS-Dienste können jedoch keine Ressourcen in einem virtuellen Netzwerk bereitgestellt werden. Allerdings können Sie den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste nach wie vor ausschließlich auf den Datenverkehr eines Subnetzes eines virtuellen Netzwerks beschränken. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen beschränken können.

> [!div class="nextstepaction"]
> [Netzwerkzugriff auf PaaS-Ressourcen beschränken](virtual-network-service-endpoints-configure.md#azure-powershell)
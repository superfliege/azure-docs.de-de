---
title: Weiterleiten von Netzwerkdatenverkehr mit Azure PowerShell | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Netzwerkdatenverkehr mithilfe von PowerShell und einer Routingtabelle weitergeleitet wird.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 2aca1de567dbd4d37daf7f9dd7c407b669396a47
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "31603707"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Weiterleiten von Netzwerkdatenverkehr über eine Routingtabelle mithilfe von PowerShell

Standardmäßig leitet Azure den Datenverkehr automatisch durch alle Subnetze des virtuellen Netzwerks. Sie können eigene Routen erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist beispielsweise hilfreich, wenn Sie über ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) Datenverkehr zwischen Subnetzen weiterleiten möchten. In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Erstellen einer Routingtabelle
* Erstellen einer Route
* Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen
* Zuordnen einer Routingtabelle zu einem Subnetz
* Erstellen eines virtuellen Netzwerkgeräts, das Datenverkehr weiterleitet
* Bereitstellen von VMs in unterschiedlichen Subnetzen
* Weiterleiten von Datenverkehr aus einem Subnetz zu einem anderen über ein virtuelles Netzwerkgerät

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

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

Erstellen Sie eine Route, indem Sie ein Routingtabellenobjekt mit [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable) abrufen, eine Route mit [Add-AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig) erstellen und dann die Routingkonfiguration mit [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable) in die Routingtabelle schreiben. 

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

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen. Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt.

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

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dabei werden die Subnetze im virtuellen Netzwerk erstellt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Ordnen Sie die Routingtabelle *myRouteTablePublic* mithilfe von [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) dem Subnetz *Öffentlich* zu, und schreiben Sie die Subnetzkonfiguration dann mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in das virtuelle Netzwerk.

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

## <a name="create-an-nva"></a>Erstellen eines virtuellen Netzwerkgeräts

Eine NVA ist eine VM, die eine Netzwerkfunktion wie Routing, Firewall oder WAN-Optimierung ausführt.

Vor dem Erstellen eines virtuellen Computers erstellen Sie eine Netzwerkschnittstelle.

### <a name="create-a-network-interface"></a>Erstellen einer Netzwerkschnittstelle

Vor dem Erstellen einer Netzwerkschnittstelle müssen Sie mit [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) die ID des virtuellen Netzwerks und dann mit [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) die ID des Subnetzes abrufen. Erstellen Sie mit aktivierter IP-Weiterleitung eine Netzwerkschnittstelle mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) im Subnetz *DMZ*:

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

### <a name="create-a-vm"></a>Erstellen einer VM

Um einen virtuellen Computer zu erstellen und diesem eine vorhandene Netzwerkschnittstelle anzufügen, müssen Sie mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zuerst die Konfiguration eines virtuellen Computers erstellen. Die Konfiguration beinhaltet die im vorherigen Schritt erstellte Netzwerkschnittstelle. Wählen Sie bei entsprechender Aufforderung den Benutzernamen und das Kennwort aus, mit denen Sie sich beim virtuellen Computer anmelden möchten. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
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

Erstellen Sie den virtuellen Computer mithilfe der VM-Konfiguration mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Im folgenden Beispiel wird ein virtueller Computer namens *myVmNva* erstellt. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie in einem späteren Schritt diesen Datenverkehr aus dem Subnetz *Public* über das virtuelle Netzwerkgerät an das Subnetz *Private* weiterleiten können. 

Erstellen Sie mit *New-AzureRmVM* eine VM im Subnetz [Public](/powershell/module/azurerm.compute/new-azurermvm). Im folgenden Beispiel wird ein virtueller Computer namens *myVmPublic* im Subnetz *Public* des virtuellen Netzwerks *myVirtualNetwork* erstellt. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Erstellen Sie eine VM im Subnetz *Private*.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, wenn der virtuelle Computer erstellt ist und Azure eine Ausgabe an PowerShell zurückgegeben hat.

## <a name="route-traffic-through-an-nva"></a>Weiterleiten von Datenverkehr über eine NVA

Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse der *myVmPrivate*-VM zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse der *myVmPrivate*-VM zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung mit dem virtuellen Computer *myVmPrivate*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

Geben Sie den Benutzernamen und das Kennwort ein, wie beim Erstellen des virtuellen Computers angegeben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie anschließend auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren. 

In einem späteren Schritt wird das Routing mithilfe des Befehls „tracert.exe“ getestet. Tracert verwendet das Internet Control Message-Protokoll (ICMP), das über die Windows-Firewall verweigert wird. Geben Sie auf der VM *myVmPrivate* in PowerShell den folgenden Befehl ein, um ICMP in der Windows-Firewall zuzulassen:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

In diesem Artikel wird zwar die Routenverfolgung zum Testen des Routings verwendet, das Zulassen des ICMP-Datenverkehrs durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.

Sie haben die IP-Weiterleitung innerhalb von Azure für die Netzwerkschnittstelle der VM unter [Aktivieren der IP-Weiterleitung](#enable-ip-forwarding) aktiviert. Das Betriebssystem der VM oder eine Anwendung, die auf der VM ausgeführt wird, muss ebenfalls Netzwerkdatenverkehr weiterleiten können. Aktivieren Sie die IP-Weiterleitung innerhalb des Betriebssystems von *myVmNva*.

Stellen Sie über eine Eingabeaufforderung auf der VM *myVmPrivate* eine Remotedesktopverbindung mit der VM *myVmNva* her:

``` 
mstsc /v:myvmnva
```
    
Geben Sie auf der VM *myVmNva* in PowerShell den folgenden Befehl ein, um im Betriebssystem die IP-Weiterleitung zu aktivieren:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Starten Sie die VM *myVmNva* neu. Dadurch wird auch die Remotedesktopsitzung getrennt.

Erstellen Sie nach dem Neustart der VM *myVmNva* bei bestehender Verbindung mit der VM *myVmPrivate* eine Remotedesktopsitzung mit der VM *myVmPublic*:

``` 
mstsc /v:myVmPublic
```
    
Geben Sie auf der VM *myVmPublic* in PowerShell den folgenden Befehl ein, um ICMP-Datenverkehr in der Windows-Firewall zuzulassen:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Geben Sie auf der VM *myVmPublic* in PowerShell den folgenden Befehl ein, um die Weiterleitung von Netzwerkdatenverkehr der VM *myVmPublic* an der VM *myVmPrivate* zu testen:

```
tracert myVmPrivate
```

Die Antwort ähnelt dem folgenden Beispiel:
    
```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```
      
Wie Sie sehen können, ist der erste Hop 10.0.2.4, die private IP-Adresse des virtuellen Netzwerksgeräts. Der zweite Hop ist 10.0.1.4, die private IP-Adresse der VM *myVmPrivate*. Die Route, die der Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Public* zugeordnet wurde, hat bewirkt, dass Azure Datenverkehr über das virtuelle Netzwerkgerät weiterleitet und nicht direkt an das Subnetz *Private*.

Schließen Sie die Remotedesktopsitzung mit der VM *myVmPublic*, sodass Sie nur noch mit der VM *myVmPrivate* verbunden sind.

Geben Sie auf der VM *myVmPrivate* an einer Eingabeaufforderung den folgenden Befehl ein, um die Weiterleitung von Netzwerkdatenverkehr der VM *myVmPrivate* an der VM *myVmPublic* zu testen:

```
tracert myVmPublic
```

Die Antwort ähnelt dem folgenden Beispiel:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
   
Trace complete.
```

Wie Sie sehen, wird Datenverkehr direkt von der VM *myVmPrivate* zur VM *myVmPublic* weitergeleitet. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.

Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein einfaches virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Stellen Sie eine Vielzahl von vorkonfigurierten virtuellen Netzwerkgeräten, die Netzwerkfunktionen wie z.B. Firewall- und WAN-Optimierung ausführen, vom [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) aus bereit. Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) und [Erstellen, Ändern oder Löschen einer Routingtabelle](manage-route-table.md).

Sie können zwar in einem virtuellen Netzwerk viele Azure-Ressourcen bereitstellen, für einige Azure-PaaS-Dienste können jedoch keine Ressourcen in einem virtuellen Netzwerk bereitgestellt werden. Allerdings können Sie den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste nach wie vor auf den Datenverkehr nur eines Subnetzes eines virtuellen Netzwerks beschränken. Weitere Informationen finden Sie unter [Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen mit virtuellen Netzwerkdienstendpunkten mithilfe der Azure CLI](tutorial-restrict-network-access-to-resources-powershell.md).

---
title: Erstellen eines virtuellen Azure-Netzwerks mit mehreren Subnetzen – PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell ein virtuelles Netzwerk mit mehreren Subnetzen erstellen.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: c90bdc9381bf98e5c1457e8e28f74105227d8f8d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-powershell"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen mithilfe von PowerShell

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Durch die Erstellung mehrerer Subnetze in einem virtuellen Netzwerk können Sie Ihr Netzwerk segmentieren, sodass Sie den Datenverkehr zwischen Subnetzen filtern oder steuern können. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Subnetzes
> * Testen der Netzwerkkommunikation zwischen virtuellen Computern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *EastUS* erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Der Wert für **AddressPrefix** wird in CIDR-Notation angegeben. Das angegebene Adresspräfix enthält die IP-Adressen 10.0.0.0 bis 10.0.255.254.

## <a name="create-a-subnet"></a>Erstellen eines Subnetzes

Ein Subnetz wird erstellt, indem zuerst eine Subnetzkonfiguration erstellt und dann das virtuelle Netzwerk mit der Subnetzkonfiguration aktualisiert wird. Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration. Das folgende Beispiel erstellt zwei Subnetzkonfigurationen für die Subnetze *Öffentlich* und *Privat*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork
```

Das für ein Subnetz angegebene **AddressPrefix**-Wert muss innerhalb des für das virtuelle Netzwerk angegebenen Adresspräfixes liegen. Azure DHCP weist in einem Subnetz bereitgestellten Ressourcen IP-Adressen aus einem Subnetzadresspräfix zu. Azure weist die Adressen 10.0.0.4 bis 10.0.0.254 nur Ressourcen zu, die im Subnetz **Öffentlich** bereitgestellt werden, da Azure die ersten vier Adressen (in diesem Beispiel 10.0.0.0 bis 10.0.0.3 für das Subnetz) und die letzte Adresse (in diesem Beispiel 10.0.0.255 für das Subnetz) in jedem Subnetz reserviert.

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dabei werden die Subnetze im virtuellen Netzwerk erstellt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Es wird empfohlen, dass Sie sich vor der Bereitstellung von virtuellen Azure-Netzwerken und Subnetzen für die Produktion eingehend mit den [Überlegungen](manage-virtual-network.md#create-a-virtual-network) zu Adressbereichen und den [Netzwerkgrenzwerten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut machen. Nach der Bereitstellung von Ressourcen in Subnetzen müssen bei bestimmten Änderungen an virtuellen Netzwerken und Subnetzen (etwa bei Änderung der Adressbereiche) unter Umständen in Subnetzen bereitgestellte vorhandene Azure-Ressourcen erneut bereitgestellt werden.

## <a name="test-network-communication"></a>Testen der Netzwerkkommunikation

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie die Netzwerkkommunikation zwischen ihnen und dem Internet in einem späteren Schritt überprüfen können. 

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens *myVmWeb* im Subnetz *Öffentlich* des virtuellen Netzwerks *myVirtualNetwork* erstellt. Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können. Geben Sie bei entsprechender Aufforderung den Benutzernamen und das Kennwort ein, mit denen Sie sich beim virtuellen Computer anmelden möchten.

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

Azure hat automatisch 10.0.0.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.0.4 die erste verfügbare IP-Adresse im Subnetz *Öffentlich* ist. 

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

Das Erstellen des virtuellen Computers dauert einige Minuten. Azure hat 10.0.1.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.1.4 die erste verfügbare IP-Adresse im Subnetz *Öffentlich* von *myVirtualNetwork* ist. Diese ist jedoch in der zurückgegebenen Ausgabe nicht enthalten. 

Fahren Sie nicht mit den übrigen Schritten fort, bis der virtuelle Computer erstellt und von PowerShell eine Ausgabe zurückgegeben wurde.

Die in diesem Artikel erstellten virtuellen Computer verfügen über eine [Netzwerkschnittstelle](virtual-network-network-interface.md) mit einer IP-Adresse, die der Netzwerkschnittstelle dynamisch zugewiesen wird. Nach dem Bereitstellen des virtuellen Computers können Sie [mehrere öffentliche und private IP-Adressen hinzufügen oder für die IP-Adresszuweisung die statische Methode festlegen](virtual-network-network-interface-addresses.md#add-ip-addresses). Sie können [Netzwerkschnittstellen hinzufügen](virtual-network-network-interface-vm.md#vm-add-nic). Dabei gilt in Bezug auf die Anzahl der Grenzwert, der von der [VM-Größe](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) unterstützt wird, die Sie beim Erstellen eines virtuellen Computers auswählen. Sie können für einen virtuellen Computer auch [E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) aktivieren](create-vm-accelerated-networking-powershell.md). Dies ist jedoch nur möglich, wenn Sie einen virtuellen Computer mit einer VM-Größe erstellen, die diese Funktion unterstützt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Kommunikation zwischen virtuellen Computern und dem Internet

Sie können über das Internet eine Verbindung mit der öffentlichen IP-Adresse eines virtuellen Computers herstellen. Bei der Erstellung des virtuellen Computers *myVmMgmt* durch Azure wurde auch eine öffentliche IP-Adresse namens *myVmMgmt* erstellt und dem virtuellen Computer zugewiesen. Einem virtuellen Computer muss zwar keine öffentliche IP-Adresse zugewiesen werden, Azure weist aber dennoch standardmäßig jedem von Ihnen erstellten virtuellen Computer eine öffentliche IP-Adresse zu. Für die Kommunikation zwischen Internet und virtuellem Computer muss einem virtuellen Computer eine öffentliche IP-Adresse zugewiesen werden. Alle virtuellen Computer können in ausgehender Richtung mit dem Internet kommunizieren, unabhängig davon, ob dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu ausgehenden Internetverbindungen finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVmMgmt* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung mit dem virtuellen Computer *myVmMgmt*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt, auf Ihren Computer heruntergeladen und geöffnet. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. (Unter Umständen müssen Sie auf **More choices** (Weitere Optionen) und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren. 

In einem späteren Schritt wird auf dem virtuellen Computer *myVmWeb* per Ping mit dem virtuellen Computer *myVmMgmt* kommuniziert. Der Ping nutzt das ICMP, das von der Windows-Firewall standardmäßig verweigert wird. Aktivieren Sie das ICMP für die Windows-Firewall, indem Sie an einer Eingabeaufforderung den folgenden Befehl eingeben:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

In diesem Artikel wird zwar ein Ping verwendet, das Zulassen des ICMP durch die Windows-Firewall wird jedoch nicht für Produktionsbereitstellungen empfohlen.

Aus Sicherheitsgründen wird die Anzahl virtueller Computer, mit denen in einem virtuellen Netzwerk eine Verbindung hergestellt werden kann, häufig begrenzt. In diesem Tutorial wird der virtuelle Computer *myVmMgmt* zur Verwaltung des virtuellen Computers *myVmWeb* im virtuellen Netzwerk verwendet. Verwenden Sie den folgenden Befehl, um vom virtuellen Computer *myVmMgmt* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmWeb* herzustellen:

``` 
mstsc /v:myVmWeb
```

Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um über den virtuellen Computer *myVmWeb* mit dem virtuellen Computer *myVmMgmt* zu kommunizieren:

```
ping myvmmgmt
```

Eine Ausgabe ähnlich folgendem Beispiel wird angezeigt:
    
```
Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
Ping statistics for 10.0.1.4:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
      
Sie sehen, dass die Adresse des virtuellen Computers *myVmMgmt* 10.0.1.4 lautet. 10.0.1.4 war die erste verfügbare IP-Adresse im Adressbereich des Subnetzes *Privat*, in dem Sie in einem vorherigen Schritt den virtuellen Computer *myVmMgmt* bereitgestellt haben.  Der vollqualifizierte Domänenname des virtuellen Computers lautet *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Der Teil *dar5p44cif3ulfq00wxznl3i3f* des Domänennamens weicht für Ihren virtuellen Computer zwar ab, die übrigen Teil sind jedoch identisch. Standardmäßig verwenden alle virtuellen Azure-Computer den Azure DNS-Standarddienst. Alle virtuellen Computer in einem virtuellen Netzwerk können die Namen aller anderen virtuellen Computer im gleichen virtuellen Netzwerk mithilfe des DNS-Standarddiensts von Azure auflösen. Anstelle des DNS-Standarddiensts von Azure können Sie auch Ihren eigenen DNS-Server oder die Funktion für private Domänen des Azure DNS-Diensts verwenden. Ausführliche Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) oder [Verwenden von Azure DNS für private Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Wenn Sie Internetinformationsdienste (Internet Information Services, IIS) für Windows Server auf dem virtuellen Computer *myVmWeb* installieren möchten, geben Sie in einer PowerShell-Sitzung den folgenden Befehl ein:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Trennen Sie nach der Installation von IIS die Remotedesktopsitzung mit *myVmWeb*. Dadurch befinden Sie sich in der *myVmMgmt*-Remotedesktopsitzung. Öffnen Sie einen Webbrowser, und gehen Sie zu http://myvmweb. Die IIS-Willkommensseite wird angezeigt.

Trennen Sie die *myVmMgmt*-Remotedesktopsitzung.

Rufen Sie die öffentliche Adresse ab, die dem virtuellen Computer *myVmWeb* von Azure zugewiesen wurde:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Navigieren Sie auf Ihrem eigenen Computer zur öffentlichen IP-Adresse des virtuellen Computers *myVmWeb*. Beim Versuch, die IIS-Willkommensseite auf Ihrem eigenen Computer anzuzeigen, tritt ein Fehler auf. Der Fehler tritt auf, da beim Bereitstellen der virtuellen Computer von Azure standardmäßig eine Netzwerksicherheitsgruppe für jeden virtuellen Computer erstellt wurde. 

Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die eingehenden und ausgehenden Netzwerkdatenverkehr nach Port und IP-Adresse zulassen oder verweigern. Die von Azure erstellte Standard-Netzwerksicherheitsgruppe lässt über alle Ports die Kommunikation zwischen Ressourcen im gleichen virtuellen Netzwerk zu. Bei virtuellen Windows-Computern verweigert die Standard-Netzwerksicherheitsgruppe den gesamten eingehenden Datenverkehr aus dem Internet an allen Ports mit Ausnahme von TCP-Port 3389 (RDP). Daher können Sie standardmäßig auch eine RDP-Verbindung direkt mit dem virtuellen Computer *myVmWeb* über das Internet herstellen, auch wenn Port 3389 für einen Webserver nicht offen sein sollte. Da beim Surfen im Internet Port 80 verwendet wird, tritt bei der Kommunikation vom Internet ein Fehler auf, da die Standard-Netzwerksicherheitsgruppe keine Regel enthält, die Datenverkehr über Port 80 zulässt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie einen virtuellen Computer mit mehreren Subnetzen bereitstellen. Darüber hinaus haben Sie gelernt, dass beim Erstellen eines virtuellen Windows-Computers von Azure eine Netzwerkschnittstelle, die an den virtuellen Computer angefügt wird, sowie eine Netzwerksicherheitsgruppe erstellt wird, die Datenverkehr aus dem Internet nur über Port 3389 zulässt. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Netzwerkdatenverkehr nicht für einzelne virtuelle Computer, sondern für Subnetze filtern.

> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen mit der Azure CLI](./virtual-networks-create-nsg-arm-ps.md)

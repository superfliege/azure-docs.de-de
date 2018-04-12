---
title: Filtern von Netzwerkdatenverkehr – Azure PowerShell | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie an ein Subnetz gerichteten Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe unter Verwendung von PowerShell filtern.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 53406150cfc2ec4229ecd9cf3356ad03d60f8e7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe unter Verwendung von PowerShell

Sie können eingehenden und ausgehenden Netzwerkdatenverkehr im Subnetz eines virtuellen Netzwerks mithilfe einer Netzwerksicherheitsgruppe filtern. Netzwerksicherheitsgruppen enthalten Sicherheitsregeln, die Netzwerkdatenverkehr nach IP-Adresse, Port und Protokoll filtern. Sicherheitsregeln werden auf Ressourcen angewendet, die in einem Subnetz bereitgestellt sind. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen von Netzwerksicherheitsgruppe und Sicherheitsregeln
> * Erstellen eines virtuellen Netzwerks und Zuweisen einer Netzwerksicherheitsgruppe zu einem Subnetz
> * Bereitstellen von virtuellen Computern in einem Subnetz
> * Testen von Datenverkehrsfiltern

Sie können diesen Artikel auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-filter-network-traffic-cli.md) durcharbeiten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln. Sicherheitsregeln geben eine Quelle und ein Ziel an. Bei Quellen und Zielen kann es sich um Anwendungssicherheitsgruppen handeln.

### <a name="create-application-security-groups"></a>Erstellen von Anwendungssicherheitsgruppen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe für alle Ressourcen, die im Rahmen dieses Artikels erstellt werden. Das folgende Beispiel erstellt eine Ressourcengruppe am Standort *eastus*: 


```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Erstellen Sie mit [New-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup) eine Anwendungssicherheitsgruppe. Mithilfe einer Anwendungssicherheitsgruppe können Sie Server mit ähnlichen Portfilterungsanforderungen gruppieren. Das folgende Beispiel erstellt zwei Anwendungssicherheitsgruppen:

```azurepowershell-interactive
$webAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgWebServers `
  -Location eastus

$mgmtAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgMgmtServers `
  -Location eastus
```

### <a name="create-security-rules"></a>Erstellen von Sicherheitsregeln

Erstellen Sie mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) eine Sicherheitsregel. Das folgende Beispiel erstellt eine Regel, die eingehenden Datenverkehr aus dem Internet für die Anwendungssicherheitsgruppe *myWebServers* über die Ports 80 und 443 zulässt:

```azurepowershell-interactive
$webRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-Web-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $webAsg.id `
  -DestinationPortRange 80,443

The following example creates a rule that allows traffic inbound from the internet to the *myMgmtServers* application security group over port 3389:

$mgmtRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-RDP-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 110 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $mgmtAsg.id `
  -DestinationPortRange 3389
```

In diesem Artikel wird RDP (Port 3389) für den virtuellen Computer *myAsgMgmtServers* für das Internet verfügbar gemacht. In Produktionsumgebungen empfiehlt es sich, eine [VPN-basierte](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [private](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Netzwerkverbindung mit den Azure-Ressourcen herzustellen, die Sie verwalten möchten, anstatt den Port 3389 für das Internet verfügbar zu machen.

### <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Erstellen Sie eine Netzwerksicherheitsgruppe mit [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Das folgende Beispiel erstellt eine Netzwerksicherheitsgruppe namens *myNsg*: 

```powershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Das folgende Beispiel erstellt ein virtuelles Netzwerk namens *myVirtualNetwork*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration, und schreiben Sie sie anschließend mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in das virtuelle Netzwerk. Das folgende Beispiel fügt dem virtuellen Netzwerk ein Subnetz namens *mySubnet* hinzu und weist ihm die Netzwerksicherheitsgruppe *myNsg* zu:

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Rufen Sie vor dem Erstellen der virtuellen Computer mithilfe von [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) das virtuelle Netzwerkobjekt mit dem Subnetz ab:

```powershell-interactive
$virtualNetwork = Get-AzureRmVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```
Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse für die einzelnen virtuellen Computer:

$publicIpWeb = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` -ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmWeb

$publicIpMgmt = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` -ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmMgmt


Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) zwei Netzwerkschnittstellen, und weisen Sie ihnen eine öffentliche IP-Adresse zu. Das folgende Beispiel erstellt eine Netzwerkschnittstelle, ordnet ihr die öffentliche IP-Adresse *myVmWeb* zu und fügt sie der Anwendungssicherheitsgruppe *myAsgWebServers* zu:

```powershell-interactive
$webNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

Das folgende Beispiel erstellt eine Netzwerkschnittstelle, ordnet ihr die öffentliche IP-Adresse *myVmMgmt* zu und fügt sie der Anwendungssicherheitsgruppe *myAsgMgmtServers* zu:

```powershell-interactive
$mgmtNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, im in einem späteren Schritt die Datenverkehrsfilterung überprüfen zu können. 

Erstellen Sie mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) eine VM-Konfiguration und anschließend mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer. Das folgende Beispiel erstellt einen virtuellen Computer, der als Webserver fungiert. Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können: 

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$webVmConfig = New-AzureRmVMConfig `
  -VMName myVmWeb `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmWeb `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $webNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $webVmConfig `
  -AsJob
```

Erstellen Sie einen virtuellen Computer, der als Verwaltungsserver fungiert:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create the web server virtual machine configuration and virtual machine.
$mgmtVmConfig = New-AzureRmVMConfig `
  -VMName myVmMgmt `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmMgmt `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $mgmtNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $mgmtVmConfig
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Fahren Sie mit dem nächsten Schritt erst fort, wenn Azure die Erstellung des virtuellen Computers abgeschlossen hat.

## <a name="test-traffic-filters"></a>Testen von Datenverkehrsfiltern

Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVmMgmt* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung zwischen Ihrem lokalen Computer und dem virtuellen Computer *myVmMgmt*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

Geben Sie den Benutzernamen und das Kennwort ein, wie beim Erstellen des virtuellen Computers angegeben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie anschließend auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren. 
   
Die Verbindung wird erfolgreich hergestellt, da am Port 3389 eingehender Datenverkehr aus dem Internet für die Anwendungssicherheitsgruppe *myAsgMgmtServers* zugelassen wird, in der sich die an den virtuellen Computer *myVmMgmt* angefügte Netzwerkschnittstelle befindet.

Erstellen Sie mithilfe des folgenden PowerShell-Befehls eine Remotedesktopverbindung zwischen den virtuellen Computern *myVmMgmt* und *myVmWeb*:

``` 
mstsc /v:myvmWeb
```

Die Verbindung wird erfolgreich hergestellt, da eine Standardsicherheitsregel innerhalb jeder Netzwerksicherheitsgruppe Datenverkehr über alle Ports zwischen allen IP-Adressen in einem virtuellen Netzwerk zulässt. Über das Internet kann keine Remotedesktopverbindung mit dem virtuellen Computer *myVmWeb* hergestellt werden, da die Sicherheitsregel für *myAsgWebServers* am Port 3389 keinen eingehenden Datenverkehr aus dem Internet zulässt.

Installieren Sie mithilfe des folgenden PowerShell-Befehls Microsoft IIS auf dem virtuellen Computer *myVmWeb*:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Trennen Sie nach Abschluss der IIS-Installation die Verbindung mit dem virtuellen Computer *myVmWeb*. Dadurch befinden Sie sich in der Remotedesktopverbindung des virtuellen Computers *myVmMgmt*. Navigieren Sie in einem Internetbrowser zu http://myVmWeb, um den IIS-Begrüßungsbildschirm anzuzeigen.

Trennen Sie die Verbindung mit dem virtuellen Computer *myVmMgmt*.

Geben Sie auf Ihrem Computer in PowerShell den folgenden Befehl ein, um die öffentliche IP-Adresse des Servers *myVmWeb* abzurufen:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Navigieren Sie auf Ihrem Computer in einem Internetbrowser zu `http://<public-ip-address-from-previous-step>`, um sich zu vergewissern, dass Sie außerhalb von Azure auf den Webserver *myVmWeb* zugreifen können. Die Verbindung wird erfolgreich hergestellt, da am Port 80 eingehender Datenverkehr aus dem Internet für die Anwendungssicherheitsgruppe *myAsgWebServers* zugelassen wird, in der sich die an den virtuellen Computer *myVmWeb* angefügte Netzwerkschnittstelle befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Netzwerksicherheitsgruppe erstellt und dem Subnetz eines virtuellen Netzwerks zugeordnet. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen – Übersicht](security-overview.md) sowie unter [Verwalten einer Netzwerksicherheitsgruppe](virtual-network-manage-nsg-arm-ps.md).

Azure leitet standardmäßig Datenverkehr zwischen Subnetzen weiter. Sie können Datenverkehr zwischen Subnetzen aber beispielsweise auch über einen virtuellen Computer weiterleiten, der als Firewall fungiert. Informationen zum Erstellen einer Routingtabelle finden Sie im nächsten Artikel.

> [!div class="nextstepaction"]
> [Weiterleiten von Netzwerkdatenverkehr mithilfe des Azure-Portals](./tutorial-create-route-table-portal.md)
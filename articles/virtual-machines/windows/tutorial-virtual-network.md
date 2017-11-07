---
title: Virtuelle Azure-Netzwerke und virtuelle Windows-Computer | Microsoft-Dokumentation
description: 'Tutorial: Verwalten von virtuellen Azure-Netzwerken und virtuellen Windows-Computern mit Azure PowerShell'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/12/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 21f2d586a4c468071bec55c65005b35baf323fe7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Verwalten von virtuellen Azure-Netzwerken und virtuellen Windows-Computern mit Azure PowerShell

Virtuelle Azure-Computer nutzen Azure-Netzwerke für interne und externe Kommunikation. Dieses Tutorial enthält ausführliche Informationen zum Bereitstellen von zwei virtuellen Computern sowie zum Konfigurieren von Azure-Netzwerken für diese virtuellen Computer. In den Beispielen in diesem Tutorial wird davon ausgegangen, dass auf den virtuellen Computern eine Webanwendung mit einem Datenbank-Back-End gehostet wird. Eine Anwendung wird im Tutorial jedoch nicht bereitgestellt. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks und des Subnetzes
> * Erstellen einer öffentlichen IP-Adresse
> * Erstellen eines virtuellen Front-End-Computers
> * Netzwerkdatenverkehr absichern
> * Erstellen des virtuellen Back-End-Computers

Während der Durchführung dieses Tutorials werden die folgenden Ressourcen erstellt:

![Virtuelles Netzwerk mit zwei Subnetzen](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet*: Das virtuelle Netzwerk, das von den virtuellen Computer verwendet wird, um miteinander und mit dem Internet zu kommunizieren.
- *myFrontendSubnet*: Das Subnetz in *myVNet*, das von den Front-End-Ressourcen verwendet wird.
- *myPublicIPAddress*: Die öffentliche IP-Adresse, die für den Zugriff auf *myFrontendVM* über das Internet verwendet wird.
- *myFrontentNic*: Die von *myFrontendVM* für die Kommunikation mit *myBackendVM* verwendete Netzwerkschnittstelle.
- *myFrontendVM*: Der virtuelle Computer, der für die Kommunikation zwischen dem Internet und *myBackendVM* verwendet wird.
- *myBackendNSG*: Die Netzwerksicherheitsgruppe, die die Kommunikation zwischen *myFrontendVM* und *myBackendVM* steuert.
- *myBackendSubnet*: Das *myBackendNSG* zugeordnete Subnetz, das von den Back-End-Ressourcen verwendet wird.
- *myBackendNic*: Die von *myBackendVM* für die Kommunikation mit *myFrontendVM* verwendete Netzwerkschnittstelle.
- *myBackendVM*: Der virtuelle Computer, der Port 1433 für die Kommunikation mit *myFrontendVM* verwendet.

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="vm-networking-overview"></a>Übersicht über VM-Netzwerke

Virtuelle Azure-Netzwerke ermöglichen sichere Netzwerkverbindungen zwischen virtuellen Computern, dem Internet und anderen Azure-Diensten wie Azure SQL-Datenbank. Virtuelle Netzwerke werden in logische Segmente, sogenannte Subnetze, unterteilt. Subnetze dienen zur Steuerung des Netzwerkdatenflusses und als Sicherheitsgrenze. Bei der Bereitstellung eines virtuellen Computers ist in der Regel eine virtuelle Netzwerkschnittstelle enthalten, die an ein Subnetz angefügt ist.

## <a name="create-a-virtual-network-and-subnet"></a>Erstellen eines virtuellen Netzwerks und des Subnetzes

In diesem Tutorial wird ein einzelnes virtuelles Netzwerk mit zwei Subnetzen erstellt: Einem Front-End-Subnetz zum Hosten einer Webanwendung und einem Back-End-Subnetz zum Hosten eines Datenbankservers.

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *EastUS* eine Ressourcengruppe mit dem Namen *myRGNetwork*:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Erstellen von Subnetzkonfigurationen

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration namens *myFrontendSubnet*:

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Erstellen Sie dann eine Subnetzkonfiguration namens *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie ein VNET namens *myVNet* mithilfe von *myFrontendSubnet* und *myBackendSubnet* mithilfe von [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

Zu diesem Zeitpunkt wurde ein Netzwerk erstellt und in zwei Subnetze unterteilt: eines für Front-End-Dienste und eines für Back-End-Dienste. Im nächsten Abschnitt werden virtuelle Computer erstellt und mit diesen Subnetzen verbunden.

## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Dank einer öffentlichen IP-Adresse kann über das Internet auf Azure-Ressourcen zugegriffen werden. Die Zuordnungsmethode der öffentlichen IP-Adresse kann als „dynamisch“ oder „statisch“ konfiguriert werden. Standardmäßig werden öffentliche IP-Adressen dynamisch zugeordnet. Dynamische IP-Adressen werden freigegeben, wenn die Zuordnung eines virtuellen Computers aufgehoben wird. Dieses Verhalten bewirkt, dass sich die IP-Adresse bei jedem Vorgang ändert, der eine Aufhebung der VM-Zuordnung beinhaltet.

Als Zuordnungsmethode kann die statische Zuordnung festgelegt werden. Dadurch wird sichergestellt, dass die IP-Adresse einem virtuellen Computer zugewiesen bleibt, auch wenn seine Zuordnung aufgehoben ist. Wenn Sie eine statisch zugeordnete IP-Adresse verwenden, kann die IP-Adresse selbst nicht angegeben werden. Stattdessen wird sie aus einem Pool verfügbarer Adressen zugeordnet.

Erstellen Sie mithilfe von [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse namens *myPublicIPAddress*:

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Sie können den Parameter „-AllocationMethod“ in `Static` ändern, um eine statische öffentliche IP-Adresse zuzuweisen.

## <a name="create-a-front-end-vm"></a>Erstellen eines virtuellen Front-End-Computers

Ein virtueller Computer muss für die Kommunikation in einem virtuellen Netzwerk über eine virtuelle Netzwerkschnittstelle (NIC) verfügen. Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) eine Netzwerkschnittstelle:

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest. Diese Anmeldeinformationen benötigen Sie, um in weiteren Schritten eine Verbindung mit der VM herzustellen:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie die virtuellen Computer mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) und [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="secure-network-traffic"></a>Netzwerkdatenverkehr absichern

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken (VNet) verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen Netzwerkschnittstellen zugeordnet werden. Wenn eine Netzwerksicherheitsgruppe einer Netzwerkschnittstelle zugewiesen ist, gilt sie nur für den zugeordneten virtuellen Computer. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die Regeln für alle Ressourcen, die mit dem Subnetz verbunden sind.

### <a name="network-security-group-rules"></a>Netzwerksicherheitsgruppen-Regeln

NSG-Regeln definieren Netzwerkports, über die Datenverkehr zugelassen oder verweigert wird. Die Regeln können Quell- und Ziel-IP-Adressbereiche enthalten, um Datenverkehr zwischen bestimmten Systemen oder Subnetzen zu steuern. NSG-Regeln enthalten darüber hinaus eine Priorität (zwischen 1 und 4.096). Regeln werden gemäß ihrer Priorität ausgewertet. Eine Regel mit einer Priorität von 100 wird vor einer Regel der Priorität 200 ausgewertet.

Alle NSGs enthalten eine Gruppe von Standardregeln. Die Standardregeln können zwar nicht gelöscht werden, haben aber niedrigste Priorität und können somit durch selbst erstellte Regeln außer Kraft gesetzt werden.

- **Virtuelles Netzwerk:** Datenverkehr wird aus einem bzw. in ein virtuelles Netzwerk in ein- und ausgehender Richtung zugelassen.
- **Internet:** Ausgehender Datenverkehr wird zugelassen, aber eingehender Datenverkehr wird blockiert.
- **Lastenausgleich:** Lassen Sie für den Lastenausgleich (Load Balancer) von Azure die Überprüfung der Integrität der virtuellen Computer und der Rolleninstanzen zu. Sie können diese Regel außer Kraft setzen, wenn Sie keine Gruppe mit Lastenausgleich verwenden.

### <a name="create-network-security-groups"></a>Erstellen von Netzwerksicherheitsgruppen

Erstellen Sie eine eingehende Regel namens *myFrontendNSGRule*, um den eingehenden Webdatenverkehr für *myFrontendVM* mithilfe von [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) zu gestatten:

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Durch Erstellen einer Netzwerksicherheitsgruppe für das Back-End-Subnetz können Sie den Datenverkehr zu *myBackendVM* nur auf *myFrontendVM* beschränken. Im folgenden Beispiel wird eine NSG-Regel namens *myBackendNSGRule* erstellt:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Fügen Sie mit [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) die Netzwerksicherheitsgruppe *myFrontendNSG* hinzu:

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Fügen Sie jetzt mit „New-AzureRmNetworkSecurityGroup“ die Netzwerksicherheitsgruppe *myBackendNSG* hinzu:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Fügen Sie die Netzwerksicherheitsgruppen zu den Subnetzen hinzu:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Erstellen eines virtuellen Back-End-Computers

Der virtuelle Back-End-Computer kann für dieses Tutorial am einfachsten mithilfe eines SQL Server-Images erstellt werden. In diesem Tutorial wird nur der virtuelle Computer mit dem Datenbankserver erstellt, es werden jedoch keine Informationen für den Zugriff auf die Datenbank angegeben.

Erstellen Sie *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Legen Sie mit „Get-Credential“ den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie *myBackendVM*:

```azurepowershell-interactive
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

In dem verwendeten Image ist SQL Server installiert, wird aber in diesem Tutorial nicht verwendet. SQL Server ist enthalten, damit Sie sehen, wie Sie einen virtuellen Computer für die Verarbeitung des Webdatenverkehrs und einen virtuellen Computer für die Verarbeitung der Datenbankverwaltung konfigurieren können.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure-Netzwerke in Bezug auf virtuelle Computer erstellt und erhalten. 

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks und des Subnetzes
> * Erstellen einer öffentlichen IP-Adresse
> * Erstellen eines virtuellen Front-End-Computers
> * Netzwerkdatenverkehr absichern
> * Erstellen eines virtuellen Back-End-Computers

Im nächsten Tutorial erhalten Sie Informationen zum Überwachen der Sicherung von Daten auf virtuellen Computern mit Azure Backup.

> [!div class="nextstepaction"]
> [Back up Windows virtual machines in Azure (Sichern virtueller Windows-Computer in Azure)](./tutorial-backup-vms.md)

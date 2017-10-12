---
title: "Erstellen eines virtuellen Windows-Computers in einer Zone – Azure PowerShell | Microsoft-Dokumentation"
description: "Erstellen eines virtuellen Windows-Computers in einer Verfügbarkeitszone mit Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: e86fcb4dbf170e5bc07553165e09d6fc3d3cf283
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-powershell"></a>Erstellen eines virtuellen Windows-Computers in einer Verfügbarkeitszone mit PowerShell

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure PowerShell einen virtuellen Azure-Computer unter Windows Server 2016 in einer Azure-Verfügbarkeitszone erstellen. Eine [Verfügbarkeitszone](../../availability-zones/az-overview.md) ist eine physisch separate Zone in einer Azure-Region. Verwenden Sie Verfügbarkeitszonen, um Ihre Apps und Daten vor einem unwahrscheinlichen Fehler oder Ausfall eines gesamten Datencenters zu schützen.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Vergewissern Sie sich, dass das aktuelle Azure PowerShell-Modul installiert ist. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus2* erstellt. „USA, Osten 2“ ist eine der Azure-Regionen, die Verfügbarkeitszonen in der Vorschauversion unterstützen.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus2
```

## <a name="create-networking-resources"></a>Erstellen von Netzwerkressourcen

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Erstellen eines virtuelles Netzwerks, eines Subnetzes und einer öffentlichen IP-Adresse 
Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden. Erstellen Sie die IP-Adresse in einer Verfügbarkeitszone (*2* in diesem Beispiel). Um den virtuellen Computer in einer Verfügbarkeitszone zu erstellen (wird in einem späteren Schritt gezeigt), geben Sie die gleiche Zone an wie bei der Erstellung der IP-Adresse.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus2 `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address in an availability zone and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus2 -Zone 2 `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Erstellen Sie eine Netzwerksicherheitsgruppe und eine Netzwerksicherheitsgruppen-Regel. 
Die Netzwerksicherheitsgruppe sichert den virtuellen Computer mithilfe von Regeln für eingehenden und ausgehenden Datenverkehr. In diesem Fall wird eine Regel für eingehenden Datenverkehr für Port 3389 erstellt, die eingehende Remotedesktopverbindungen zulässt. Wir möchten auch eine Eingangsregel für Port 80 erstellen, um eingehenden Webdatenverkehr zuzulassen.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus2 `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Erstellen einer Netzwerkkarte für den virtuellen Computer 
Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) eine Netzwerkkarte für den virtuellen Computer. Die Netzwerkkarte verbindet die VM mit einem Subnetz, einer Netzwerksicherheitsgruppe und einer öffentlichen IP-Adresse.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus2 `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine VM-Konfiguration. Diese Konfiguration umfasst die beim Bereitstellen der VM verwendeten Einstellungen, z.B. ein Image des virtuellen Computers, Größe und Authentifizierungskonfiguration. Die Größe *Standard_DS1_v2* in diesem Beispiel wird in der Vorschauversion von Verfügbarkeitszonen unterstützt. Diese Konfiguration gibt auch die Verfügbarkeitszone an, die Sie beim Erstellen der IP-Adresse festgelegt haben. Wenn Sie diesen Schritt ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die Werte, die Sie eingeben, werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS1_v2 -Zone 2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus2 -VM $vmConfig
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zone für IP-Adresse und verwalteten Datenträger

Sie haben die IP-Adressressource des virtuellen Computers in der gleichen Verfügbarkeitszone erstellt wie den virtuellen Computer. Die verwaltete Datenträgerressource für den virtuellen Computer wird ebenfalls in der gleichen Verfügbarkeitszone erstellt. Dies kann mit [Get-AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk) überprüft werden:

```powershell
Get-AzureRmDisk -ResourceGroupName myResourceGroup
```

Die Ausgabe zeigt, dass sich der verwaltete Datenträger in der gleichen Verfügbarkeitszone befindet wie der virtuelle Computer:

```powershell
ResourceGroupName  : myResourceGroup
AccountType        : PremiumLRS
OwnerId            : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
ManagedBy          : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              : {2}
TimeCreated        : 9/7/2017 6:57:26 PM
OsType             : Windows
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 127
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/disks/myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Name               : myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Type               : Microsoft.Compute/disks
Location           : eastus2
Tags               : {}
```




## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen virtuellen Computer in einer Verfügbarkeitszone erstellen. Weitere Informationen zu Regionen und Verfügbarkeit für virtuelle Azure-Computer finden Sie [hier](regions-and-availability.md).
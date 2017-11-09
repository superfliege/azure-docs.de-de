---
title: "Erstellen virtueller Computer mit ausgeführtem SQL-/IIS-/.NET-Stapel in Azure | Microsoft-Dokumentation"
description: 'Tutorial: Installieren eines Azure SQL-, IIS-, .NET-Stapels auf einem virtuellen Windows-Computer'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6f7ef46d9c40138c211427845423783fefde5dc3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Installieren eines SQL-/IIS-/.NET-Stapels in Azure

In diesem Tutorial installieren wir einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell. Dieser Stapel besteht aus zwei virtuellen Computern unter Windows Server 2016: einer mit IIS und .NET und der andere mit SQL Server.

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers mit dem Cmdlet „New-AzVM“
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung



## <a name="create-a-iis-vm"></a>Erstellen eines virtuellen IIS-Computers 

In diesem Beispiel verwenden wir das Cmdlet [New-AzVM](https://www.powershellgallery.com/packages/AzureRM.Compute.Experiments) in der PowerShell-Cloud Shell, um einen virtuellen Computer unter Windows Server 2016 zu erstellen und dann IIS und .NET Framework zu installieren. Der virtuelle IIS- und der virtuelle SQL-Computer gehören derselben Ressourcengruppe und demselben virtuellen Netzwerk an, daher erstellen wir Variablen für diese Namen.

Klicken Sie auf die Schaltfläche **Try It** (Testen) rechts oben neben dem Codeblock, um Cloud Shell in diesem Fenster zu starten. Sie werden aufgefordert, an der Eingabeaufforderung Anmeldeinformationen für den virtuellen Computer anzugeben.

```azurepowershell-interactive
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzVm -Name myIISVM -ResourceGroupName $resourceGroup -VirtualNetworkName $vNetName 
```

Installieren Sie IIS und .NET Framework mithilfe der benutzerdefinierten Skripterweiterung.

```azurepowershell-interactive

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName myIISVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="azure-sql-vm"></a>Virtueller Azure SQL-Computer

Wir verwenden ein vorkonfiguriertes Azure Marketplace-Image einer SQL Server-Instanz zum Erstellen des virtuellen SQL-Computers. Zunächst erstellen wir den virtuellen Computer, und danach installieren wir die SQL Server-Erweiterung auf dem virtuellen Computer. 


```azurepowershell-interactive
# Create user object. You get a pop-up prompting you to enter the credentials for the VM.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroup
Add-AzureRmVirtualNetworkSubnetConfig -Name mySQLSubnet -VirtualNetwork $vNet -AddressPrefix "192.168.2.0/24"
Set-AzureRmVirtualNetwork -VirtualNetwork $vNet


# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location eastus `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow


# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location eastus `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name mySQLNic -ResourceGroupName $resourceGroup -Location eastus `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName mySQLVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName mySQLVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftSQLServer -Offer SQL2014SP2-WS2012R2 -Skus Enterprise -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM -ResourceGroupName $resourceGroup -Location eastus -VM $vmConfig
```

Verwenden Sie [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension), um dem virtuellen SQL-Computer die [SQL Server-Erweiterung](/sql/virtual-machines-windows-sql-server-agent-extension.md) hinzuzufügen.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension -ResourceGroupName $resourceGroup -VMName mySQLVM -name "SQLExtension"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell installiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers mit dem Cmdlet „New-AzVM“
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie einen IIS-Webserver mit SSL-Zertifikaten schützen.

> [!div class="nextstepaction"]
> [Schützen eines IIS-Webservers mit SSL-Zertifikaten](tutorial-secure-web-server.md)


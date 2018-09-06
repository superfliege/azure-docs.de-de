---
title: 'Tutorial: Erstellen virtueller Computer mit ausgeführtem SQL-/IIS-/.NET-Stapel in Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie den Azure SQL-/IIS-/.NET-Stapel auf einem virtuellen Windows-Computer in Azure installieren.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f8f91a4c039e07b636395ab953b8031d5ffe9958
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43343505"
---
# <a name="tutorial-install-the-sql47iis47net-stack-in-a-windows-vm-with-azure-powershell"></a>Tutorial: Installieren des SQL-/IIS-/.NET-Stapels auf einem virtuellen Windows-Computer mit Azure PowerShell

In diesem Tutorial installieren wir einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell. Dieser Stapel besteht aus zwei virtuellen Computern unter Windows Server 2016: einer mit IIS und .NET und der andere mit SQL Server.

> [!div class="checklist"]
> * Erstellen einer VM 
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des AzureRM.Compute-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM.Compute` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="create-a-iis-vm"></a>Erstellen eines virtuellen IIS-Computers 

In diesem Beispiel verwenden wir das [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)-Cmdlet in der PowerShell-Cloud Shell, um einen virtuellen Computer unter Windows Server 2016 zu erstellen und dann IIS und .NET Framework zu installieren. Der virtuelle IIS- und der virtuelle SQL-Computer gehören derselben Ressourcengruppe und demselben virtuellen Netzwerk an, daher erstellen wir Variablen für diese Namen.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

Installieren Sie IIS und .NET Framework mithilfe der benutzerdefinierten Skripterweiterung.

```azurepowershell-interactive
Set-AzureRmVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Erstellen eines anderen Subnetzes

Erstellen Sie ein zweites Subnetz für die SQL-VM. Rufen Sie das VNet über [Get-AzureRmVirtualNetwork]{/powershell/module/azurerm.network/get-azurermvirtualnetwork} ab.

```azurepowershell-interactive
$vNet = Get-AzureRmVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

Erstellen Sie mit [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) eine Konfiguration für das Subnetz.


```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

Aktualisieren Sie das VNet mit den neuen Subnetzinformationen, indem Sie [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) verwenden.
   
```azurepowershell-interactive   
$vNet | Set-AzureRmVirtualNetwork
```

## <a name="azure-sql-vm"></a>Virtueller Azure SQL-Computer

Verwenden Sie ein vorkonfiguriertes Azure Marketplace-Image einer SQL Server-Instanz zum Erstellen des virtuellen SQL-Computers. Zunächst erstellen wir den virtuellen Computer, und danach installieren wir die SQL Server-Erweiterung auf dem virtuellen Computer. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

Verwenden Sie [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension), um dem virtuellen SQL-Computer die [SQL Server-Erweiterung](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) hinzuzufügen.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell installiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer VM 
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie einen IIS-Webserver mit SSL-Zertifikaten schützen.

> [!div class="nextstepaction"]
> [Schützen eines IIS-Webservers mit SSL-Zertifikaten](tutorial-secure-web-server.md)


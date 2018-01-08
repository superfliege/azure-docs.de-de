---
title: Erstellen einer VM-Skalierungsgruppe mit Azure PowerShell | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie mit Azure PowerShell schnell eine VM-Skalierungsgruppe erstellen können."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 1a4c69427e0fa38e9206e2720c4746c97d922df1
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-with-azure-powershell"></a>Erstellen einer VM-Skalierungsgruppe mit Azure PowerShell
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Artikel zu den ersten Schritten erstellen Sie eine VM-Skalierungsgruppe mit Azure PowerShell. Sie können eine Skalierungsgruppe auch per [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) oder mit dem [Azure-Portal](virtual-machine-scale-sets-create-portal.md) erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 4.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bevor Sie eine Skalierungsgruppe erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *EastUS* erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Location "EastUS"
```


## <a name="create-load-balancer"></a>Erstellen eines Load Balancers
Die VM-Instanzen einer Skalierungsgruppe sind mit einem Load Balancer verbunden. Ein Azure Load Balancer ist ein Load Balancer der Schicht 4 (TCP, UDP), der Hochverfügbarkeit durch Verteilen des eingehenden Datenverkehrs auf fehlerfreie virtuelle Computer bietet. Der Integritätstest eines Load Balancers überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

Erstellen Sie ein virtuelles Netzwerk und einen Load Balancer, der eine öffentliche IP-Adresse aufweist und Webdatenverkehr über Port 80 verteilt. Sie können die folgenden PowerShell-Cmdlets kopieren und einfügen, um diese Ressourcen zu erstellen:

```azurepowershell-interactive
# Create a virtual network subnet
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet

# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -AllocationMethod Static `
  -Name "myPublicIP"

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"

# Create a Network Address Translation (NAT) pool
$inboundNATPool = New-AzureRmLoadBalancerInboundNatPoolConfig `
  -Name "myRDPRule" `
  -FrontendIpConfigurationId $frontendIP.Id `
  -Protocol TCP `
  -FrontendPortRangeStart 50001 `
  -FrontendPortRangeEnd 50010 `
  -BackendPort 3389

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroup" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -InboundNatPool $inboundNATPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol TCP `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol TCP `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb

# Create IP address configurations
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -LoadBalancerInboundNatPoolsId $inboundNATPool.Id `
  -SubnetId $vnet.Subnets[0].Id
```


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Erstellen Sie nun mit [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, für die das Plattformimage *Windows Server 2016 Datacenter* verwendet wird. Das Objekt *vmssConfig* erstellt zwei VM-Instanzen für „East US“ (USA, Osten) mit den Anmeldeinformationen, die in den Variablen *adminUsername* und *securePassword* angegeben sind. Geben Sie Ihre eigenen Anmeldeinformationen ein, und erstellen Sie wie folgt eine Skalierungsgruppe:

```azurepowershell-interactive
# Provide your own secure password for use with the VM instances
$securePassword = "P@ssword!"
$adminUsername = "azureuser"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location "EastUS" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher "MicrosoftWindowsServer" `
  -ImageReferenceOffer "WindowsServer" `
  -ImageReferenceSku "2016-Datacenter" `
  -ImageReferenceVersion "latest"

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername $adminUsername `
  -AdminPassword $securePassword `
  -ComputerNamePrefix "myVM"

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmssConfig
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.


## <a name="install-iis-webserver"></a>Installieren des IIS-Webservers
Verwenden Sie zum Testen Ihrer Skalierungsgruppe die benutzerdefinierte Skripterweiterung, um ein Skript herunterzuladen und auszuführen, mit dem IIS auf den VM-Instanzen installiert wird. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](../virtual-machines/windows/extensions-customscript.md).

Wenden Sie die benutzerdefinierte Skripterweiterung, mit der IIS installiert wird, wie folgt an:

```azurepowershell-interactive
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroup" `
            -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-web-server"></a>Testen des Webservers
Um Ihren Webserver in Aktion zu sehen, rufen Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse abgerufen, die in der Ressourcengruppe *myResourceGroup* erstellt wurde:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Geben Sie die öffentliche IP-Adresse des Lastenausgleichsmoduls in einen Webbrowser ein. Das Lastenausgleichsmodul verteilt den Datenverkehr auf eine Ihrer VM-Instanzen. Dies ist im folgenden Beispiel dargestellt:

![Ausführen der IIS-Website](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie können [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen wie folgt zu entfernen, sofern diese nicht mehr benötigt werden:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel zu den ersten Schritten haben Sie eine einfache Skalierungsgruppe erstellt und die benutzerdefinierte Skripterweiterung verwendet, um einen einfachen IIS-Webserver auf den VM-Instanzen zu installieren. Erweitern Sie Ihre Skalierungsgruppe, um die Möglichkeiten in Bezug auf die Skalierbarkeit und Automatisierung zu erhöhen, indem Sie die folgenden Anleitungen verwenden:

- [Bereitstellen der App in VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md)
- Automatisches Skalieren per [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) oder [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-automatic-upgrade.md)

---
title: "Tutorial zu Verfügbarkeitsgruppen für virtuelle Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verfügbarkeitsgruppen für virtuelle Windows-Computer in Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 503657d6e6e22560b94d4a1cbbc2a74651d1ee7a
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="how-to-use-availability-sets"></a>Verwenden von Verfügbarkeitsgruppen

In diesem Tutorial erfahren Sie, wie Sie die Verfügbarkeit und Zuverlässigkeit Ihrer Lösungen für virtuelle Computer in Azure mithilfe von sogenannten Verfügbarkeitsgruppen erhöhen. Verfügbarkeitsgruppen sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwarecluster verteilt werden. Dadurch wird sichergestellt, dass sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer VMs auswirken und die Lösung insgesamt verfügbar und betriebsbereit bleibt. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen
> * Überprüfen von Azure-Ratgeber

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Eine Verfügbarkeitsgruppe ist eine Funktion zur logischen Gruppierung, mit der Sie in Azure sicherstellen können, dass die darin enthaltenen VM-Ressourcen voneinander isoliert sind, wenn sie in einem Azure-Rechenzentrum bereitgestellt werden. Azure stellt sicher, dass die virtuellen Computer innerhalb einer Verfügbarkeitsgruppe auf mehrere physische Server, Compute-Racks, Speichereinheiten und Netzwerkswitches verteilt werden. Wenn ein Hardware- oder Softwarefehler in Azure auftritt, wird nur ein Teil Ihrer VMs beeinträchtigt, und die Anwendung insgesamt bleibt betriebsbereit und weiterhin für Ihre Kunden verfügbar. Verfügbarkeitsgruppen stellen eine wichtige Funktion für die Erstellung zuverlässiger Cloudlösungen dar.

In einer typischen VM-basierten Lösung gibt es möglicherweise vier Front-End-Webserver und zwei Back-End-VMs, die eine Datenbank hosten. Sie können in Azure zwei Verfügbarkeitsgruppen definieren, bevor Sie Ihre VMs bereitstellen: eine Verfügbarkeitsgruppe für die Webebene und eine Verfügbarkeitsgruppe für die Datenbankebene. Bei der Erstellung einer neuen VM können Sie dann die Verfügbarkeitsgruppe als Parameter für den Befehl „az vm create“ angeben, damit Azure automatisch sicherstellt, dass die in der Verfügbarkeitsgruppe erstellten VMs über mehrere physische Hardwareressourcen isoliert werden. Wenn bei der physischen Hardware, auf der Ihre Webserver- oder Datenbankserver-VMs ausgeführt werden, ein Problem auftritt, können Sie darauf vertrauen, dass die anderen Instanzen Ihrer Webserver- und Datenbank-VMs weiterhin einwandfrei ausgeführt werden, da sie sich auf anderer Hardware befinden.

Verwenden Sie Verfügbarkeitsgruppen, wenn Sie zuverlässige VM-basierte Lösungen in Azure bereitstellen möchten.

## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Sie können mithilfe von [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) eine Verfügbarkeitsgruppe erstellen. Im folgenden Beispiel wird die Anzahl der Update- sowie der Fehlerdomänen für die Verfügbarkeitsgruppe *myAvailabilitySet* in der Ressourcengruppe *myResourceGroupAvailability* auf *2* festgelegt.

Erstellen Sie eine Ressourcengruppe.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Erstellen Sie mit [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) mit dem Parameter **-sku aligned** eine verwaltete Verfügbarkeitsgruppe.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe

Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Nach der Erstellung kann einer Verfügbarkeitsgruppe kein vorhandener virtueller Computer mehr hinzugefügt werden. 

Die Hardware an einem Standort ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Eine **Updatedomäne** ist eine Gruppe von virtuellen Computern und der zugrunde liegenden physischen Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Virtuelle Computer in ein und derselben **Fehlerdomäne** nutzen denselben Speicher sowie eine Stromquelle und einen Netzwerkswitch gemeinsam. 

Beim Erstellen einer VM-Konfiguration mit [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) legen Sie den Parameter `-AvailabilitySetId` fest, um die ID der Verfügbarkeitsgruppe anzugeben.

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in der Verfügbarkeitsgruppe zwei VMs.

```azurepowershell-interactive
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig
    
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup `
    -Location eastus `
    -Name myNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAvailability `
    -SecurityRules $nsgRuleRDP
    
# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name mySubnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -ComputerName myVM$i `
        -Credential $cred `
        -VM $vm `
        -Windows `
        -EnableAutoUpdate `
        -ProvisionVMAgent
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Die Erstellung und Konfiguration der beiden virtuellen Computer dauert einige Minuten. Anschließend verfügen Sie über zwei virtuelle Computer, die auf die zugrunde liegenden Hardwarekomponenten verteilt sind. 

Im Portal sollten Sie bei der Verfügbarkeitsgruppe unter „Ressourcengruppen“ > „myResourceGroupAvailability“ > „myAvailabilitySet“ sehen können, dass die VMs auf zwei Fehler- und Updatedomänen verteilt sind.

![Verfügbarkeitsgruppe im Portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen 

Sie können der Verfügbarkeitsgruppe später weitere virtuelle Computer hinzufügen. Dazu müssen Sie jedoch wissen, welche VM-Größen in der Hardware verfügbar sind. Verwenden Sie [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="check-azure-advisor"></a>Überprüfen von Azure-Ratgeber 

Sie können mit Azure-Ratgeber auch weitere Informationen zu Methoden zur Verbesserung der Verfügbarkeit Ihrer virtuellen Computer abrufen. Azure-Ratgeber unterstützt Sie mit bewährten Methoden zum Optimieren von Azure-Bereitstellungen. Das Tool analysiert die Konfiguration Ihrer Ressourcen und Telemetriedaten zur Nutzung und macht anschließend Vorschläge, wie Sie die Wirtschaftlichkeit, Leistung, Verfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, wählen Sie die Option **Weitere Dienste** aus, und geben Sie **Advisor** ein. Das Advisor-Dashboard zeigt personalisierte Empfehlungen für das ausgewählte Abonnement. Weitere Informationen finden Sie unter [Erste Schritte mit Azure-Ratgeber](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen
> * Überprüfen von Azure-Ratgeber

Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

> [!div class="nextstepaction"]
> [Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)



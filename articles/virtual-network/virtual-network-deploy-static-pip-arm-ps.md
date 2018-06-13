---
title: Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse – Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von PowerShell einen virtuellen Computer mit einer statischen öffentlichen IP-Adresse erstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68656db0b76a29e7ab36fd6fa9ad4647712233ee
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
ms.locfileid: "31525131"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a>Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="start-your-script"></a>Starten des Skripts
Sie können das verwendete PowerShell-Skript ungekürzt [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1)herunterladen. Gehen Sie folgendermaßen vor, um das Skript an Ihre Arbeitsumgebung anzupassen.

Ändern Sie die Werte der unten angegebenen Variablen basierend auf den Werten, die Sie für die Bereitstellung verwenden möchten. Die folgenden Werte gelten für das in diesem Artikel verwendete Szenario:

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="create-the-necessary-resources-for-your-vm"></a>Erstellen der erforderlichen Ressourcen für Ihre VM
Vor dem Erstellen einer virtuellen Maschine benötigen Sie eine Ressourcengruppe, ein VNET, eine öffentliche IP-Adresse und eine Netzwerkschnittstelle für die Verwendung durch die VM.

1. Erstellen Sie eine neue Ressourcengruppe.

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. Erstellen Sie das VNET und das Subnetz.

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. Erstellen Sie die öffentliche IP-Ressource. 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. Erstellen Sie die Netzwerkschnittstelle (NIC) für die VM im oben erstellten Subnetz mit der öffentlichen IP-Adresse. Beachten Sie das erste Cmdlet zum Abrufen des VNET aus Azure. Dies ist erforderlich, da ein `Set-AzureRmVirtualNetwork` ausgeführt wurde, um das vorhandene VNET zu ändern.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. Erstellen Sie ein Speicherkonto zum Hosten des VM-Betriebssystemlaufwerks.

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers
Nachdem nun alle benötigten Ressourcen vorhanden sind, können Sie eine neue VM erstellen.

1. Erstellen Sie das Konfigurationsobjekt für die VM.

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. Beschaffen Sie die Anmeldeinformationen für das lokale Administratorkonto der VM.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

3. Erstellen Sie ein VM-Konfigurationsobjekt.

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. Legen Sie das Betriebssystemimage für die VM fest.

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. Konfigurieren Sie den Betriebssystemdatenträger.

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. Fügen Sie die Netzwerkschnittstelle der VM hinzu.

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. Erstellen Sie den virtuellen Computer.

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. Speichern Sie die Skript-Datei.

## <a name="run-the-script"></a>Ausführen des Skripts

Nachdem Sie die erforderlichen Änderungen vorgenommen haben, führen Sie das vorherige Skript aus. Der virtuelle Computer wird nach wenigen Minuten erstellt.

## <a name="set-ip-addresses-within-the-operating-system"></a>Festlegen von IP-Adressen innerhalb des Betriebssystems

Die öffentliche IP-Adresse sollte niemals manuell einem virtuellen Azure-Computer innerhalb des Betriebssystems des virtuellen Computers zugewiesen werden. Es wird davon abgeraten, die private IP-Adresse, die dem virtuellen Azure-Computer innerhalb des Betriebssystems einer VM zugewiesen ist, statisch zuzuweisen sofern nicht erforderlich, wie es etwa beim [Zuweisen mehrerer IP-Adressen zu einer Windows-VM](virtual-network-multiple-ip-addresses-powershell.md) der Fall ist. Wenn Sie die private IP-Adresse innerhalb des Betriebssystems manuell festlegen, stellen Sie sicher, dass es sich um dieselbe Adresse wie die private IP-Adresse, die der Azure-[Netzwerkschnittstelle](virtual-network-network-interface-addresses.md#change-ip-address-settings) zugewiesen ist, handelt. Anderenfalls könnte die Konnektivität mit dem virtuellen Computer verloren gehen. Erfahren Sie mehr über Einstellungen für [private IP-Adressen](virtual-network-network-interface-addresses.md#private).

## <a name="next-steps"></a>Nächste Schritte

Netzwerkdatenverkehr kann bei der in diesem Artikel erstellten VM ein- oder ausgehen. Sie können innerhalb einer Netzwerksicherheitsgruppe Sicherheitsregeln für den eingehenden und ausgehenden Datenverkehr definieren, der von und zur Netzwerkschnittstelle, dem Subnetz oder beiden Komponenten fließt. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen – Übersicht](security-overview.md).
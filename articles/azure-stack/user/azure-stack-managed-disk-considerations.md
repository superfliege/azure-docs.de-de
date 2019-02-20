---
title: Unterschiede und Aspekte für Managed Disks in Azure Stack | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Unterschiede und zu berücksichtigende Aspekte bei der Verwendung von Managed Disks in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 60f633049d6fdbb59744c8003e742ff649d48ea7
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243382"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack Managed Disks: Unterschiede und Überlegungen

Dieser Artikel beschreibt die bekannten Unterschiede zwischen [Azure Stack Managed Disks](azure-stack-manage-vm-disks.md) und [Managed Disks für Azure](../../virtual-machines/windows/managed-disks-overview.md). Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Artikel [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).

Managed Disks vereinfacht die Datenträgerverwaltung für IaaS-VMs durch die Verwaltung der [Speicherkonten](../azure-stack-manage-storage-accounts.md), die den VM-Datenträgern zugeordnet sind.

> [!Note]  
> Managed Disks steht in Azure Stack ab 1808 Update zur Verfügung. Es ist jetzt standardmäßig aktiviert, wenn virtuelle Computer über das Azure Stack-Portal ab 1811 Update erstellt werden.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Cheat Sheet: Unterschiede zwischen verwalteten Datenträgern

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
|Verschlüsselung für ruhende Daten |Azure Storage Service Encryption (SSE), Azure Disk Encryption (ADE)     |BitLocker-128-Bit-AES-Verschlüsselung      |
|Image          | Unterstützung von verwalteten benutzerdefinierten Images |Unterstützt|
|Sicherungsoptionen |Unterstützung für Azure Backup-Dienst |Noch nicht unterstützt |
|Optionen für die Notfallwiederherstellung |Unterstützung von Azure Site Recovery |Noch nicht unterstützt|
|Datenträgertypen     |SSD Premium, SSD Standard (Vorschau) und HDD Standard |SSD Premium, HDD Standard |
|Premium-Datenträger  |Vollständig unterstützt |Kann bereitgestellt werden, jedoch ohne Leistungsgrenzwerte oder Garantien  |
|Premium-Datenträger-IOPs  |Abhängig von der Größe des Datenträgers  |2.300 IOPS pro Datenträger |
|Durchsatz Premium-Datenträger |Abhängig von der Größe des Datenträgers |145 MB/Sekunde pro Datenträger |
|Datenträgergröße  |Azure Premium Disk: P4 (32 GiB) bis P80 (32 TiB)<br>Azure SSD Standard-Datenträger: E10 (128 GiB) bis E80 (32 TiB)<br>Azure HDD Standard-Datenträger: S4 (32 GiB) bis S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GB<br>M15: 256 GiB<br>M20: 512 GB<br>M30: 1024 GiB |
|Datenträger-Momentaufnahme Kopie|Momentaufnahme: Verwaltete Azure-Datenträger, die an einen ausgeführten virtuellen Computer angefügt sind, werden unterstützt|Noch nicht unterstützt |
|Analyse Datenträgerleistung |Unterstützung für aggregierte Metriken und Metriken pro Datenträger |Noch nicht unterstützt |
|Migration      |Bereitstellung eines Tools für die Migration von vorhandenen, nicht verwalteten Azure Resource Manager VMs, ohne dass der virtuelle Computer neu erstellt werden muss  |Noch nicht unterstützt |

> [!NOTE]  
> Managed Disks-IOPs und Durchsatz in Azure Stack ist eine Cap-Zahl statt einer bereitgestellten Zahl, die durch Hardware und in Azure Stack ausgeführte Workloads beeinflusst werden kann.

## <a name="metrics"></a>Metriken

Es gibt auch Unterschiede zu Speichermetriken:

- Mit Azure Stack unterscheiden die Transaktionsdaten in Speichermetriken nicht zwischen interner und externer Netzwerkbandbreite.
- In Azure Stack beinhalten die Transaktionsdaten in Speichermetriken keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.

## <a name="api-versions"></a>API-Versionen

Azure Stack Managed Disks unterstützt die folgenden API-Versionen:

- 2017-03-30
- 2017-12-01

## <a name="managed-images"></a>Verwaltete Images

Azure Stack unterstütz *verwaltete Images*, was es Ihnen ermöglicht, ein verwaltetes Image-Objekt auf einem generalisierten virtuellen Computer (sowohl nicht verwaltet als auch verwaltet) zu erstellen, der fortan nur noch virtuelle Computer auf verwalteten Datenträgern erstellen kann. Verwaltete Images ermöglichen die folgenden zwei Szenarien:

- Sie haben generalisierte, nicht verwaltete virtuelle Computer und möchten im weiteren Verlauf verwaltete Datenträger verwenden.
- Sie haben eine generalisierte, verwaltete VM und möchten mehrere, ähnliche verwaltete VMs erstellen.

### <a name="migrate-unmanaged-vms-to-managed-disks"></a>Migrieren nicht verwalteter VMs zu verwalteten Datenträgern

Befolgen Sie die [hier](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-vhd-in-a-storage-account) dargelegten Anleitungen, um ein verwaltetes Image aus einer generalisierten VHD in einem Speicherkonto zu erstellen. Dieses Image kann verwendet werden, um im weiteren Verlauf Zukunft verwaltete VMs zu erstellen.

### <a name="create-managed-image-from-vm"></a>Erstellen eines verwalteten Images aus einer VM

Nachdem Sie ein Image aus einer vorhandenen VM mit verwaltetem Datenträger mithilfe des [hier zu findenden](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-managed-disk-using-powershell) Skripts erstellt haben, erstellt das folgende Beispielskript eine ähnlich Linux-VM aus einem vorhandenen Image-Objekt:

```powershell
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "redmond"
$vmName = "myVM"
$imagerg = "managedlinuxrg"
$imagename = "simplelinuxvmm-image-2019122"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

$image = get-azurermimage -ResourceGroupName $imagerg -ImageName $imagename
# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Weitere Informationen finden Sie in den Azure-Artikeln zu verwalteten Images [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../../virtual-machines/windows/capture-image-resource.md) und [Erstellen eines virtuellen Computers aus einem verwalteten Image](../../virtual-machines/windows/create-vm-generalized-managed.md).

## <a name="configuration"></a>Konfiguration

Nach der Anwendung des Updates 1808 oder höher müssen Sie die folgende Konfiguration ausführen, bevor Sie Managed Disks verwenden:

- Wenn ein Abonnement vor dem Update 1808 erstellt wurde, führen Sie die folgenden Schritte aus, um das Abonnement zu aktualisieren. Andernfalls kann die Bereitstellung von VMs in diesem Abonnement mit einer Fehlermeldung „Interner Fehler im Datenträger-Manager.“ fehlschlagen.
   1. Navigieren Sie im Mandantenportal zu **Abonnements**, und suchen Sie nach dem Abonnement. Klicken Sie auf **Ressourcenanbieter**, klicken Sie dann auf **Microsoft.Compute**, und klicken Sie anschließend auf **Erneut registrieren**.
   2. Navigieren Sie unter dem gleichen Abonnement zu **Zugriffssteuerung (IAM)**, und überprüfen Sie, ob **Azure Stack – Verwalteter Datenträger** aufgeführt wird.
- Wenn Sie eine Umgebung mit mehreren Mandanten verwenden, bitten Sie Ihren Cloudoperator (der sich in Ihrem Unternehmen oder beim Dienstanbieter befinden kann), jedes Ihrer Gastverzeichnisse gemäß den folgenden Schritten in [diesem Artikel](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) neu zu konfigurieren. Andernfalls kann die Bereitstellung von VMs in einem Abonnement, das diesem Gastverzeichnis zugeordnet ist, mit einer Fehlermeldung „Interner Fehler im Datenträger-Manager.“ fehlschlagen.

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über virtuelle Azure Stack-Computer](azure-stack-compute-overview.md)

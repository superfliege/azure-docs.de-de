---
title: Datenträgerverschlüsselung für Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure PowerShell VM-Instanzen und angefügte Datenträger in VM-Skalierungsgruppen verschlüsseln können.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: iainfou
ms.openlocfilehash: 856d4bc7dd636b3a2f3d072a10989cafd7efd6a6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe
Um die ruhenden Daten mit einer Verschlüsselungstechnologie nach Industriestandard zu schützen, unterstützen VM-Skalierungsgruppen Azure Disk Encryption (ADE). Die Verschlüsselung kann für Windows- und Linux-VM-Skalierungsgruppen aktiviert werden. Weitere Informationen finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Azure Disk Encryption für VM-Skalierungsgruppen befindet sich derzeit in der Vorschauversion und ist in allen öffentlichen Azure-Regionen verfügbar. 
>
> Reimaging- und Upgradevorgänge von Skalierungsgruppen-VMs werden in der aktuellen Vorschauversion nicht unterstützt. In der Vorschauversion wird die Verschlüsselung von Skalierungsgruppen nur in Testumgebungen empfohlen. Aktivieren Sie in der Vorschauversion die Datenträgerverschlüsselung nicht in Produktionsumgebungen, in denen Sie möglicherweise ein Betriebssystemimage aktualisieren müssen.

Azure Disk Encryption wird unterstützt:
- für Skalierungsgruppen mit verwalteten Datenträgern (nicht für native oder nicht verwaltete Datenträger-Skalierungsgruppen).
- für Betriebssystem- und Datenvolumes in Windows-Skalierungsgruppen. Das Deaktivieren der Verschlüsselung wird bei Betriebssystem- und Datenvolumes für Windows-Skalierungsgruppen unterstützt.
- für Datenvolumes in Linux-Skalierungsgruppen. Die Betriebssystem-Datenträgerverschlüsselung wird in der aktuellen Vorschau für Linux-Skalierungsgruppen NICHT unterstützt.


## <a name="prerequisites"></a>Voraussetzungen
Für diesen Artikel ist Version 5.3.0 oder höher des Azure PowerShell-Moduls erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

Registrieren Sie mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) Ihr Azure-Abonnement für die Vorschauversion der Datenträgerverschlüsselung für VM-Skalierungsgruppen: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Warten Sie ca. 10 Minuten, bis der Status *Registriert* von [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) zurückgegeben wird, registrieren Sie dann den Anbieter `Microsoft.Compute` erneut: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```


## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Erstellen eines für die Datenträgerverschlüsselung aktivierten Azure-Schlüsseltresors
In Azure Key Vault können Schlüssel, geheime Schlüssel und Kennwörter gespeichert werden, um eine sichere Implementierung in Anwendungen und Diensten zu ermöglichen. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen.

Erstellen Sie mit [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) einen Schlüsseltresor. Damit der Schlüsseltresor für die Datenträgerverschlüsselung verwendet werden kann, legen Sie den Parameter *EnabledForDiskEncryption* fest. Das folgende Beispiel definiert auch Variablen für den Ressourcengruppennamen, den Schlüsseltresornamen und den Speicherort. Geben Sie Ihren eigenen eindeutigen Schlüsseltresornamen an:

```powershell
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```


### <a name="use-an-existing-key-vault"></a>Verwenden eines vorhandenen Schlüsseltresors
Dieser Schritt ist nur erforderlich, wenn Sie über einen vorhandenen Schlüsseltresor verfügen, den Sie mit der Datenträgerverschlüsselung verwenden möchten. Überspringen Sie diesen Schritt, wenn Sie im vorherigen Abschnitt einen Schlüsseltresor erstellt haben.

Sie können mit [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy) einen vorhandenen Schlüsseltresor im gleichen Abonnement und der gleichen Region wie die Skalierungsgruppe für die Datenträgerverschlüsselung aktivieren. Definieren Sie den Namen Ihres vorhandenen Schlüsseltresors in der Variablen *$vaultName* wie folgt:

```powershell
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) zuerst den Benutzernamen und das Kennwort des Administrators der VM-Instanzen fest:

```powershell
$cred = Get-Credential
```

Erstellen Sie nun mit [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) eine VM-Skalierungsgruppe. Um Datenverkehr an die einzelnen VM-Instanzen zu verteilen, wird auch ein Lastenausgleich erstellt. Der Lastenausgleich enthält Regeln zum Verteilen von Datenverkehr über TCP-Port 80 und zum Zulassen von Remotedesktop-Datenverkehr über TCP-Port 3389 und PowerShell-Remoting über TCP-Port 5985:

```powershell
$vmssName="myScaleSet"

New-AzureRmVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```


## <a name="enable-encryption"></a>Aktivieren der Verschlüsselung
Um VM-Instanzen in einer Skalierungsgruppe zu verschlüsseln, rufen Sie zunächst mit [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault) einige Informationen zum Schlüsseltresor-URI und zur Ressourcen-ID ab. Diese Variablen dienen dann zum Starten des Verschlüsselungsvorgangs mit [Satz AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```powershell
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Wenn Sie dazu aufgefordert werden, geben Sie *y* ein, um die Datenträgerverschlüsselung auf VM-Instanzen der Skalierungsgruppe fortzusetzen.


## <a name="check-encryption-progress"></a>Überprüfen des Verschlüsselungsfortschritts
Verwenden Sie [Get AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption) zum Überprüfen des Status der Datenträgerverschlüsselung:

```powershell
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Wenn VM-Instanzen verschlüsselt sind, gibt der Code *EncryptionSummary* den Wert *ProvisioningState/succeeded* an, wie in der folgenden Beispielausgabe gezeigt:

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```


## <a name="disable-encryption"></a>Deaktivieren der Verschlüsselung
Wenn Sie die verschlüsselten Datenträger von VM-Instanzen nicht mehr verwenden möchten, können Sie die Verschlüsselung mit [Disable-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) wie folgt deaktivieren:

```powershell
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie Azure PowerShell verwendet, um eine VM-Skalierungsgruppe zu verschlüsseln. Sie können auch [Azure CLI 2.0](virtual-machine-scale-sets-encrypt-disks-cli.md) oder Vorlagen für [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) oder [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) verwenden.

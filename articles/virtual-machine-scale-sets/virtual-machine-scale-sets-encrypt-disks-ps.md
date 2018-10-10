---
title: Verschlüsseln von Datenträgern für Azure-Skalierungsgruppen mit Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure PowerShell VM-Instanzen und angefügte Datenträger in Windows-VM-Skalierungsgruppen verschlüsseln können.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: cynthn
ms.openlocfilehash: c39df127e07601dbc5125d246f25da22ce4a0e40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981762"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell (Vorschauversion)

Um die ruhenden Daten mit einer Verschlüsselungstechnologie nach Industriestandard zu schützen, unterstützen VM-Skalierungsgruppen Azure Disk Encryption (ADE). Die Verschlüsselung kann für Windows- und Linux-VM-Skalierungsgruppen aktiviert werden. Weitere Informationen finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Azure Disk Encryption für VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschau und ist in allen öffentlichen Azure-Regionen verfügbar.

Azure Disk Encryption wird unterstützt:
- für Skalierungsgruppen mit verwalteten Datenträgern (nicht für native oder nicht verwaltete Datenträger-Skalierungsgruppen).
- für Betriebssystem- und Datenvolumes in Windows-Skalierungsgruppen. Das Deaktivieren der Verschlüsselung wird bei Betriebssystem- und Datenvolumes für Windows-Skalierungsgruppen unterstützt.
- für Datenvolumes in Linux-Skalierungsgruppen. Die Betriebssystem-Datenträgerverschlüsselung wird in der aktuellen Vorschau für Linux-Skalierungsgruppen NICHT unterstützt.

Reimaging- und Upgradevorgänge von Skalierungsgruppen-VMs werden in der aktuellen Vorschauversion nicht unterstützt. Die Vorschauversion von Azure Disk Encryption für VM-Skalierungsgruppen wird nur in Testumgebungen empfohlen. Aktivieren Sie in der Vorschauversion die Datenträgerverschlüsselung nicht in Produktionsumgebungen, in denen Sie möglicherweise ein Betriebssystemimage in einer verschlüsselten Skalierungsgruppe aktualisieren müssen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="register-for-disk-encryption-preview"></a>Registrieren der Vorschauversion der Datenträgerverschlüsselung

Azure Disk Encryption für die Vorschauversion von VM-Skalierungsgruppen erfordert, dass Sie Ihr Abonnement mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) selbst registrieren. Sie müssen die folgenden Schritte nur beim ersten Mal ausführen, wenn Sie das Vorschaufeature der Datenträgerverschlüsselung verwenden:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Es kann bis zu 10 Minuten dauern, bis die Registrierungsanforderung weitergegeben wurde. Sie können den Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen. Wenn `RegistrationState` *Registered* (Registriert) meldet, registrieren Sie den *Microsoft.Compute-Anbieter* erneut mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Erstellen eines für die Datenträgerverschlüsselung aktivierten Azure-Schlüsseltresors

In Azure Key Vault können Schlüssel, geheime Schlüssel und Kennwörter gespeichert werden, um eine sichere Implementierung in Anwendungen und Diensten zu ermöglichen. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen.

Erstellen Sie mit [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) einen Schlüsseltresor. Damit der Schlüsseltresor für die Datenträgerverschlüsselung verwendet werden kann, legen Sie den Parameter *EnabledForDiskEncryption* fest. Das folgende Beispiel definiert auch Variablen für den Ressourcengruppennamen, den Schlüsseltresornamen und den Speicherort. Geben Sie Ihren eigenen eindeutigen Schlüsseltresornamen an:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Verwenden eines vorhandenen Schlüsseltresors

Dieser Schritt ist nur erforderlich, wenn Sie über einen vorhandenen Schlüsseltresor verfügen, den Sie mit der Datenträgerverschlüsselung verwenden möchten. Überspringen Sie diesen Schritt, wenn Sie im vorherigen Abschnitt einen Schlüsseltresor erstellt haben.

Sie können mit [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy) einen vorhandenen Schlüsseltresor im gleichen Abonnement und der gleichen Region wie die Skalierungsgruppe für die Datenträgerverschlüsselung aktivieren. Definieren Sie den Namen Ihres vorhandenen Schlüsseltresors in der Variablen *$vaultName* wie folgt:

```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) zuerst den Benutzernamen und das Kennwort des Administrators der VM-Instanzen fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie nun mit [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) eine VM-Skalierungsgruppe. Um Datenverkehr an die einzelnen VM-Instanzen zu verteilen, wird auch ein Lastenausgleich erstellt. Der Lastenausgleich enthält Regeln zum Verteilen von Datenverkehr über TCP-Port 80 und zum Zulassen von Remotedesktop-Datenverkehr über TCP-Port 3389 und PowerShell-Remoting über TCP-Port 5985:

```azurepowershell-interactive
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

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Wenn Sie dazu aufgefordert werden, geben Sie *y* ein, um die Datenträgerverschlüsselung auf VM-Instanzen der Skalierungsgruppe fortzusetzen.

## <a name="check-encryption-progress"></a>Überprüfen des Verschlüsselungsfortschritts

Verwenden Sie [Get AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption) zum Überprüfen des Status der Datenträgerverschlüsselung:

```azurepowershell-interactive
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

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Azure PowerShell verwendet, um eine VM-Skalierungsgruppe zu verschlüsseln. Sie können auch [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md) oder Vorlagen für [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) oder [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) verwenden.

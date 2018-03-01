---
title: "Verschlüsseln von Datenträgern für Microsoft Azure Virtual Machine Scale Sets | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie angefügte Datenträger in VM-Skalierungsgruppen verschlüsseln."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: 3263ed4d6325207f38656d741fa4a1f091d4fdea
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe
Azure [Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/) unterstützt Azure Disk Encryption (ADE).  Azure Disk Encryption kann für Windows- und Linux-VM-Skalierungsgruppen aktiviert werden, um die ruhenden Daten in den Skalierungsgruppen mit Industriestandard-Verschlüsselungstechnologie zu schützen. Weitere Informationen finden Sie unter „Azure Disk Encryption für virtuelle Windows- und Linux-Computer“.

> [!NOTE]
>  Azure Disk Encryption für VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschau und ist in allen öffentlichen Azure-Regionen verfügbar.

Azure Disk Encryption wird unterstützt:
- für Skalierungsgruppen mit verwalteten Datenträgern (nicht für native oder nicht verwaltete Datenträger-Skalierungsgruppen).
- für Betriebssystem- und Datenvolumes in Windows-Skalierungsgruppen. Das Deaktivieren der Verschlüsselung wird bei Betriebssystem- und Datenvolumes für Windows-Skalierungsgruppen unterstützt.
- für Datenvolumes in Linux-Skalierungsgruppen. Die Betriebssystem-Datenträgerverschlüsselung wird in der aktuellen Vorschau für Linux-Skalierungsgruppen NICHT unterstützt.

Reimaging- und Upgradevorgänge von Skalierungsgruppen-VMs werden in der aktuellen Vorschauversion nicht unterstützt. Die Vorschauversion von Azure Disk Encryption für VM-Skalierungsgruppen wird nur in Testumgebungen empfohlen. Aktivieren Sie in der Vorschauversion die Datenträgerverschlüsselung nicht in Produktionsumgebungen, in denen Sie möglicherweise ein Betriebssystemimage in einer verschlüsselten Skalierungsgruppe aktualisieren müssen.

Ein End-to-End-Batchdateibeispiel für die Datenträgerverschlüsselung von Linux-Skalierungsgruppen finden Sie [hier](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat).  Dieses Beispiel erstellt eine Ressourcengruppe und eine Linux-Skalierungsgruppe, bindet einen 5-GB-Datenträger für Daten ein und verschlüsselt die VM-Skalierungsgruppe.

## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie die neuesten Versionen von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) oder [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), die die Verschlüsselungsbefehle enthalten.

Für die Vorschauversion von Azure Disk Encryption für VM-Skalierungsgruppen müssen Sie Ihr Abonnement mit den folgenden PowerShell-Befehlen selbst registrieren: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Warten Sie ca. 10 Minuten, bis beim folgenden Befehl der Status „Registriert“ zurückgegeben wird: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Erstellen eines für Datenträgerverschlüsselung aktivierten Azure Key Vault
Erstellen Sie einen neuen Schlüsseltresor im selben Abonnement und derselben Region wie die Skalierungsgruppe, und legen Sie als Zugriffsrichtlinie „EnabledForDiskEncryption“ fest.

```azurecli
rgname="linuxdatadiskencryptiontest"
VaultName="encryptionvault321"

az keyvault create --name $VaultName --resource-group $rgname --enabled-for-disk-encryption
```

Alternativ dazu können Sie auch einen vorhandenen Schlüsseltresor im selben Abonnement und derselben Region wie die Skalierungsgruppe für die Datenträgerverschlüsselung aktivieren.

```azurecli
VaultName="encryptionvault321"
az keyvault update --name $VaultName --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Aktivieren der Verschlüsselung

Mit den folgenden Befehlen wird ein Datenträger für Daten in einer ausgeführten Skalierungsgruppe mithilfe eines Schlüsseltresors in derselben Ressourcengruppe verschlüsselt. Sie können auch Vorlagen zum Verschlüsseln von Datenträgern in einer ausgeführten [Windows-](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) oder [Linux-Skalierungsgruppe](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) verwenden.

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"
EncryptionKeyVaultUrl="https://encryptionvaultlinuxsf.vault.azure.net"
VaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/linuxdatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvaultlinuxsf"

az vmss encryption enable -g $ResourceGroup -n $VmssName --disk-encryption-keyvault $VaultResourceId --volume-type DATA
az vmss update-instances -g $ResourceGroup -n $VmssName --instance-ids *
```

## <a name="check-encryption-progress"></a>Überprüfen des Verschlüsselungsfortschritts

Verwenden Sie die folgenden Befehle, um den Verschlüsselungsstatus der Skalierungsgruppe anzuzeigen.

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"

az vmss encryption show -g $ResourceGroup -n $VmssName
```

## <a name="disable-encryption"></a>Deaktivieren der Verschlüsselung
Sie deaktivieren die Verschlüsselung einer ausgeführten VM-Skalierungsgruppe mithilfe der folgenden Befehle. Sie können auch Vorlagen zum Deaktivieren der Verschlüsselung in einer ausgeführten [Windows-](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) oder [Linux-VM-Skalierungsgruppe](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux) verwenden.

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"

az vmss encryption disable -g $ResourceGroup -n $VmssName
```


---
title: Verschlüsseln von Datenträgern für Azure-Skalierungsgruppen mit Azure CLI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure CLI zum Verschlüsseln von VM-Instanzen und verknüpften Datenträgern in einer VM-Skalierungsgruppe von Linux verwenden.
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
ms.openlocfilehash: c8035a87816c56daa5ac9e1f95b40d15fa556cd9
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465157"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli-preview"></a>Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI (Vorschauversion)

Um die ruhenden Daten mit einer Verschlüsselungstechnologie nach Industriestandard zu schützen, unterstützen VM-Skalierungsgruppen Azure Disk Encryption (ADE). Die Verschlüsselung kann für Linux- und Windows-VM-Skalierungsgruppen aktiviert werden. Weitere Informationen finden Sie unter [Azure Disk Encryption for Linux and Windows (Azure Disk Encryption für Linux und Windows)](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Azure Disk Encryption für VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschau und ist in allen öffentlichen Azure-Regionen verfügbar.

Azure Disk Encryption wird unterstützt:
- für Skalierungsgruppen mit verwalteten Datenträgern (nicht für native oder nicht verwaltete Datenträger-Skalierungsgruppen).
- für Betriebssystem- und Datenvolumes in Windows-Skalierungsgruppen. Das Deaktivieren der Verschlüsselung wird bei Betriebssystem- und Datenvolumes für Windows-Skalierungsgruppen unterstützt.
- für Datenvolumes in Linux-Skalierungsgruppen. Die Betriebssystem-Datenträgerverschlüsselung wird in der aktuellen Vorschau für Linux-Skalierungsgruppen NICHT unterstützt.

Reimaging- und Upgradevorgänge von Skalierungsgruppen-VMs werden in der aktuellen Vorschauversion nicht unterstützt. Die Vorschauversion von Azure Disk Encryption für VM-Skalierungsgruppen wird nur in Testumgebungen empfohlen. Aktivieren Sie in der Vorschauversion die Datenträgerverschlüsselung nicht in Produktionsumgebungen, in denen Sie möglicherweise ein Betriebssystemimage in einer verschlüsselten Skalierungsgruppe aktualisieren müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.31 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="register-for-disk-encryption-preview"></a>Registrieren der Vorschauversion der Datenträgerverschlüsselung

Azure Disk Encryption für die Vorschauversion von VM-Skalierungsgruppen erfordert, dass Sie Ihr Abonnement mithilfe des Befehls [az feature register](/cli/azure/feature#az_feature_register) selbst registrieren. Sie müssen die folgenden Schritte nur beim ersten Mal ausführen, wenn Sie das Vorschaufeature der Datenträgerverschlüsselung verwenden:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Es kann bis zu 10 Minuten dauern, bis die Registrierungsanforderung weitergegeben wurde. Sie können den Registrierungsstatus mithilfe des Befehls [az feature show](/cli/azure/feature#az_feature_show) überprüfen. Wenn `State` *Registered* (Registriert) meldet, registrieren Sie den Anbieter *Microsoft.Compute* erneut mithilfe von [az provider register](/cli/azure/provider#az_provider_register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe

Vor der Erstellung einer Skalierungsgruppe müssen Sie zunächst mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#az_vmss_create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, die automatisch aktualisiert wird, wenn Änderungen angewendet werden. Außerdem werden SSH-Schlüssel generiert, falls sie unter *~/.ssh/id_rsa* nicht vorhanden sind. Jeder VM-Instanz wird ein Datenträger mit einer Kapazität von 32 GB angefügt, und die [benutzerdefinierte Skripterweiterung](../virtual-machines/linux/extensions-customscript.md) für Azure wird zum Vorbereiten der Datenträger mit [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) verwendet:

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Erstellen eines für Datenträgerverschlüsselung aktivierten Azure Key Vault

In Azure Key Vault können Schlüssel, geheime Schlüssel und Kennwörter gespeichert werden, um eine sichere Implementierung in Anwendungen und Diensten zu ermöglichen. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen.

Geben Sie Ihren eigenen eindeutigen *keyvault_name* an. Erstellen Sie dann mit [az keyvault create](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) einen Schlüsseltresor im gleichen Abonnement und derselben Region wie die Skalierungsgruppe, und legen Sie die Zugriffsrichtlinie *--enabled-for-disk-encryption* fest.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Verwenden eines vorhandenen Schlüsseltresors

Dieser Schritt ist nur erforderlich, wenn Sie über einen vorhandenen Schlüsseltresor verfügen, den Sie mit der Datenträgerverschlüsselung verwenden möchten. Überspringen Sie diesen Schritt, wenn Sie im vorherigen Abschnitt einen Schlüsseltresor erstellt haben.

Geben Sie Ihren eigenen eindeutigen *keyvault_name* an. Aktualisieren Sie dann Ihren Schlüsseltresor mit [az keyvault update](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update), und legen Sie die Zugriffsrichtlinie *--enabled-for-disk-encryption* fest.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Aktivieren der Verschlüsselung

Rufen Sie zum Verschlüsseln von VM-Instanzen in einer Skalierungsgruppe zunächst Informationen über die Key Vault-Ressourcen-ID mit [az keyvault show](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show) ab. Die folgenden Variablen werden zum Starten des Verschlüsselungsvorgangs mit [az vmss encryption enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable) verwendet:

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Es kann etwas Zeit in Anspruch nehmen, diesen Verschlüsselungsvorgang zu starten.

Da eine in einem vorherigen Schritt erstellte Upgraderichtlinie für die Skalierungsgruppe auf *automatisch* festgelegt ist, starten die VM-Instanzen den Verschlüsselungsvorgang automatisch. Bei Skalierungsgruppen, für die die Upgraderichtlinie auf „manuell“ festgelegt ist, starten Sie die Verschlüsselungsrichtlinie auf den VM-Instanzen mit [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances).

## <a name="check-encryption-progress"></a>Überprüfen des Verschlüsselungsfortschritts

Verwenden Sie [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show) zum Überprüfen des Status der Datenträgerverschlüsselung:

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Wenn VM-Instanzen verschlüsselt sind, gibt der Statuscode wie in der folgenden Beispielausgabe *EncryptionState/encrypted* an:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Deaktivieren der Verschlüsselung

Wenn Sie die verschlüsselten Datenträger von VM-Instanzen nicht mehr verwenden möchten, können Sie die Verschlüsselung mit [az vmss encryption disable](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) wie folgt deaktivieren:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Azure CLI verwendet, um eine VM-Skalierungsgruppe zu verschlüsseln. Sie können auch [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) oder Vorlagen für [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) oder [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) verwenden.

Ein End-to-End-Batchdateibeispiel für die Datenträgerverschlüsselung von Linux-Skalierungsgruppen finden Sie [hier](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). Dieses Beispiel erstellt eine Ressourcengruppe und eine Linux-Skalierungsgruppe, bindet einen 5-GB-Datenträger für Daten ein und verschlüsselt die VM-Skalierungsgruppe.

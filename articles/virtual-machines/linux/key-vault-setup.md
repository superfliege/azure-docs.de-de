---
title: "Einrichten von Azure Key Vault für virtuelle Linux-Computer | Microsoft Docs"
description: "Erfahren Sie, wie Sie Key Vault mithilfe von Azure CLI 2.0 für einen mit Azure Resource Manager erstellten virtuellen Computer einrichten."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 2cc9b4c978e9a4deb0c8443c4b0f9e301a7cf492
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a>Einrichten von Key Vault für virtuelle Computer mithilfe von Azure CLI 2.0

Im Azure Resource Manager-Stapel werden Geheimnisse/Zertifikate als Ressourcen modelliert, die durch Key Vault bereitgestellt werden. Weitere Informationen über Azure Schlüsseltresore finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-whatis.md) Damit Key Vault mit Azure Resource Manager-VMs verwendet werden kann, müssen Sie die *EnabledForDeployment*-Eigenschaft in Key Vault auf TRUE festlegen. Dieser Artikel zeigt das Einrichten von Key Vault mithilfe von Azure CLI 2.0 für die Verwendung mit virtuellen Azure-Computern (Azure-VMs). Sie können diese Schritte auch mit [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

Zum Ausführen dieser Schritte muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen eines Schlüsseltresors und Zuweisen der Bereitstellungsrichtlinie mit [az keyvault create](/cli/azure/keyvault#create). Das folgende Beispiel erstellt den Schlüsseltresor `myKeyVault` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Aktualisieren eines Schlüsseltresors für die Verwendung mit virtuellen Computern
Sie legen die Bereitstellungsrichtlinie für einen vorhandenen Schlüsseltresor mit [az keyvault update](/cli/azure/keyvault#update) fest. Das folgende Beispiel aktualisiert den Schlüsseltresor `myKeyVault` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Einrichten des Schlüsseltresors
Wenn Sie eine Vorlage verwenden, müssen Sie die `enabledForDeployment`-Eigenschaft für die Key Vault-Ressource wie folgt auf `true` festlegen:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Optionen, die Sie beim Erstellen eines Schlüsseltresors mithilfe von Vorlagen konfigurieren können, finden Sie unter [Create a Key Vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/) (Erstellen eines Schlüsseltresors).

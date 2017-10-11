---
title: "Einrichten von Azure Key Vault für virtuelle Linux-Computer | Microsoft Docs"
description: "Wie Sie Key Vault für die Verwendung mit einem virtuellen Computer Azure Resource Manager, Version 2.0 CLI einrichten."
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
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a>Zum Einrichten von Key Vault für virtuelle Computer mit dem Azure-CLI-2.0

In den Azure Resource Manager-Stapel werden geheime Schlüssel/Zertifikate als Ressourcen erstellt, die vom Schlüsseltresor bereitgestellt werden. Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Neuigkeiten von Azure Key Vault?](../../key-vault/key-vault-whatis.md) In der Reihenfolge für Key Vault mit Azure-Ressourcen-Manager für virtuelle Computer, die *EnabledForDeployment* Eigenschaft Key Vault muss festgelegt werden auf "true". In diesem Artikel wird gezeigt, wie Key Vault für die Verwendung mit virtuellen Azure-Computern (VMs) mit dem Azure-CLI-2.0 eingerichtet. Sie können auch folgenden Schritte mit der [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Um diese Schritte ausführen zu können, benötigen Sie die neueste [2.0 für Azure-CLI](/cli/azure/install-az-cli2) installiert und mit einer Azure-Konto angemeldet [az Anmeldung](/cli/azure/#login).

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen eines schlüsseltresors und weisen Sie die Bereitstellungsrichtlinie mit [az Keyvault erstellen](/cli/azure/keyvault#create). Das folgende Beispiel erstellt einen schlüsseltresor mit dem Namen `myKeyVault` in die `myResourceGroup` Ressourcengruppe:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Aktualisieren Sie einen Schlüsseltresor für die Verwendung mit virtuellen Computern
Die Bereitstellungsrichtlinie für einen vorhandenen Schlüssel Tresor mit Set [az Keyvault-Update](/cli/azure/keyvault#update). Die folgenden aktualisiert den schlüsseltresor mit dem Namen `myKeyVault` in die `myResourceGroup` Ressourcengruppe:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Einrichten des Key Vault
Wenn Sie eine Vorlage verwenden, müssen Sie festlegen der `enabledForDeployment` Eigenschaft `true` für den Schlüssel Tresor Ressource wie folgt:

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
Andere Optionen, die Sie bei der Erstellung einer Key Vault mithilfe von Vorlagen konfigurieren können, finden Sie unter [Erstellen eines schlüsseltresors](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
